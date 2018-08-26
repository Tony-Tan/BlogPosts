---
title: 【CUDA 基础】4.3 内存访问模式
categories:
    - CUDA
    - Freshman
keywords:
    - 内存访问模式
    - 对齐
    - 合并
    - 缓存
    - 结构体数组
    - 数组结构体
toc: true
date: 2018-05-03 22:08:07
---

**Abstract:** 本文介绍内存的访问过程，也就是从应用发起请求到硬件实现的完整操作过程，这里是优化内存瓶颈的关键之处，也是CUDA程序优化的基础。
**Keywords:** 内存访问模式，对齐，合并，缓存，结构体数组，数组结构体

<!--more-->
## 开篇废话
>"物有本末，事有终始，知所先后，则近道矣"
>——《大学·大学之道章》

这句话出自大学，大学非我们现在上的大学，而我不知道我们现在的大学为什么叫大学，是否和《大学》有关系，但是大学第一章，就是讲道，让读书人懂得道："格物，致知，诚意，正心，修身，齐家，治国，平天下"，但是我们现在似乎所有的课程都不讲这些了——可能是因为时间过去太久了，所以成了糟粕，但是我觉得对我还是有些启发的。
第一句引用的话没有官方解释，我觉得小学时候学的语文，概括中心思想，我觉得都是扯淡，一百个人有一百种想法，但是答案却是统一的，所以，我觉得这种就是培养有流水线上的机器，我只说这句话对我的启发，而且我们就从机器学习这个角度说。
举个例子，忽略时间，单从技术的角度，神经网络算是始还是终？是本还是末？我觉得不是始，也不是终。更不是本。
对于人工智能领域，我们的始是对于智慧的理解和再造，终点就是制造出人类智慧的机器，而神经网络只是我们要探索的深林里面一个大数，当有人认为这是最高峰的时候，那么他会不断地向上爬，直到最顶端，也许发现到达了我们的最终目的，但更大的可能性是周围还有更高的树。所以神经网络不是始也不是终，只是中间的一个过程，必须承认，我们走了三四十年才看到这棵这么大的树，但是如果还没爬到最顶端，就认定这个树就是终点了，似乎缺少一些谨慎。
而整个学科的本，我认为是数学，当如果哪一天证明，数学的整个体系在人工智能面前崩溃了，各种反公理，反定理的事件都在人工智能中发生了，那就证明我错了。但目前，本还是本。
废话有点多，今天我们要学习的也是CUDA中最最最重要的课程之一，当然我不会一口气写完，可能要写两天，但是以一篇的篇幅发表，力求写清楚写明白。用一些简单通俗，但是足够恰当的比喻和一些实例，让大家更容易了解。

多数GPU程序容易受到内存带宽的限制，所以最大程度的利用全局内存带宽，提高全局加载效率（后面会详细说明），是调控内核函数性能的基本条件。如果不能正确调控全局内存使用，那么优化方案可能收效甚微。
CUDA执行模型告诉我们，CUDA执行的基本单位是线程束，所以，内存访问也是以线程束为基本单位发布和执行的，存储也一致。我们本文研究的就是这一个线程束的内存访问，不同线程的内存请求，其目标位置的不同，可以产生非常多种情况。所以本篇就是研究这些不同情况的，以及如何实现最佳的全局内存访问。
注意：访问可以是加载，也可以是存储。
注意，我们本文使用命令进行编译，省去反复修改CMakelist的麻烦.
## 对齐与合并访问
全局内存通过缓存实现加载和存储的过程如下图
![1-1](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/1-1.png)
全局内存是一个逻辑层面的模型，我们编程的时候有两种模型考虑：一种是逻辑层面的，也就是我们在写程序的时候（包括串行程序和并行程序），写的一维（多维）数组，结构体，定义的变量，这些都是在逻辑层面的；一种是硬件角度，就是一块DRAM上的电信号，以及最底层内存驱动代码所完成数字信号的处理。
L1表示一级缓存，每个SM都有自己L1，但是L2是所有SM公用的，除了L1缓存外，还有只读缓存和常量缓存，这个我们后面会详细介绍。
核函数运行时需要从全局内存（DRAM）中读取数据，只有两种粒度，这个是关键的：
- 128字节
- 32字节

解释下“粒度”，可以理解为最小单位，也就是核函数运行时每次读内存，哪怕是读一个字节的变量，也要读128字节，或者32字节，而具体是到底是32还是128还是要看访问方式：
- 使用一级缓存
- 不使用一级缓存

对于CPU来说，一级缓存或者二级缓存是不能被编程的，但是CUDA是支持通过编译指令停用一级缓存的。如果启用一级缓存，那么每次从DRAM上加载数据的粒度是128字节，如果不适用一级缓存，只是用二级缓存，那么粒度是32字节。
还要强调一下CUDA内存模型的内存读写，我们现在讨论的都是单个SM上的情况，多个SM只是下面我们描述的情形的复制：SM执行的基础是线程束，也就是说，当一个SM中正在被执行的某个线程需要访问内存，那么，和它同线程束的其他31个线程也要访问内存，这个基础就表示，即使每个线程只访问一个字节，那么在执行的时候，只要有内存请求，至少是32个字节，所以不使用一级缓存的内存加载，一次粒度是32字节而不是更小。
在优化内存的时候，我们要最关注的是以下两个特性
- 对齐内存访问
- 合并内存访问

我们把一次内存请求——也就是从内核函数发起请求，到硬件响应返回数据这个过程称为一个内存事务（加载和存储都行）。
当一个内存事务的首个访问地址是缓存粒度（32或128字节）的偶数倍的时候：比如二级缓存32字节的偶数倍64，128字节的偶数倍256的时候，这个时候被称为对齐内存访问，非对齐访问就是除上述的其他情况，非对齐的内存访问会造成带宽浪费。
当一个线程束内的线程访问的内存都在一个内存块里的时候，就会出现合并访问。
对齐合并访问的状态是理想化的，也是最高速的访问方式，当线程束内的所有线程访问的数据在一个内存块，并且数据是从内存块的首地址开始被需要的，那么对齐合并访问出现了。为了最大化全局内存访问的理想状态，尽量将线程束访问内存组织成对齐合并的方式，这样的效率是最高的。下面看一个例子。
- 一个线程束加载数据，使用一级缓存，并且这个事务所请求的所有数据在一个128字节的对齐的地址段上（对齐的地址段是我自己发明的名字，就是首地址是粒度的偶数倍，那么上面这句话的意思是，所有请求的数据在某个首地址是粒度偶数倍的后128个字节里），具体形式如下图，这里请求的数据是连续的，其实可以不连续，但是不要越界就好。
![4-6](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-6.png)
上面蓝色表示全局内存，下面橙色是线程束要的数据，绿色就是我称为对齐的地址段。
- 如果一个事务加载的数据分布在不一个对齐的地址段上，就会有以下两种情况：
    1. 连续的，但是不在一个对齐的段上，比如，请求访问的数据分布在内存地址1~128，那么0~127和128~255这两段数据要传递两次到SM
    2. 不连续的，也不在一个对齐的段上，比如，请求访问的数据分布在内存地址0~63和128~191上，明显这也需要两次加载。
![4-8](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-8.png)
上图就是典型的一个线程束，数据分散开了，thread0的请求在128之前，后面还有请求在256之后，所以需要三个内存事务，而利用率，也就是从主存取回来的数据被使用到的比例，只有 $\frac{128}{128\times 3}$ 的比例。这个比例低会造成带宽的浪费，最极端的表现，就是如果每个线程的请求都在不同的段，也就是一个128字节的事务只有1个字节是有用的，那么利用率只有 $\frac{1}{128}$

这里总结一下内存事务的优化关键：用最少的事务次数满足最多的内存请求。事务数量和吞吐量的需求随设备的计算能力变化。

## 全局内存读取
注意我们说的都是读取，也就是加载过程，写或者叫做存储是另外一回事！
SM加载数据，根据不同的设备和类型分为三种路径：
1. 一级和二级缓存
2. 常量缓存
3. 只读缓存

常规的路径是一级和二级缓存，需要使用常量和只读缓存的需要在代码中显式声明。但是提高性能，主要还是要取决于访问模式。
控制全局加载操作是否通过一级缓存可以通过编译选项来控制，当然比较老的设备可能就没有一级缓存。
编译器禁用一级缓存的选项是：
```bash
-Xptxas -dlcm=cg
```
编译器启用一级缓存的选项是：
```bash
-Xptxas -dlcm=ca
```
当一级缓存被禁用的时候，对全局内存的加载请求直接进入二级缓存，如果二级缓存缺失，则由DRAM完成请求。
每次内存事务可由一个两个或者四个部分执行，每个部分有32个字节，也就是32，64或者128字节一次（注意前面我们讲到是否使用一级缓存决定了读取粒度是128还是32字节，这里增加的64并不在此情况，所以需要注意）。
启用一级缓存后，当SM有全局加载请求会首先通过尝试一级缓存，如果一级缓存缺失，则尝试二级缓存，如果二级缓存也没有，那么直接DRAM。
在有些设备上一级缓存不用来缓存全局内存访问，而是只用来存储寄存器溢出的本地数据，比如Kepler 的K10,K20。
内存加载可以分为两类：
- 缓存加载
- 没有缓存的加载

内存访问有以下特点：
- 是否使用缓存：一级缓存是否介入加载过程
- 对齐与非对齐的：如果访问的第一个地址是32的倍数（<font color="ff0000">前面说是32或者128的偶数倍，这里似乎产生了矛盾，为什么我现在也很迷惑</font>）
- 合并与非合并，访问连续数据块则是合并的

### 缓存加载
下面是使用一级缓存的加载过程，图片表达很清楚，我们只用少量文字进行说明：
1. 对齐合并的访问，利用率100%
![4-9](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-9.png)

2. 对齐的，但是不是连续的，每个线程访问的数据都在一个块内，但是位置是交叉的，利用率100%
![4-10](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-10.png)

3. 连续非对齐的，线程束请求一个连续的非对齐的，32个4字节数据，那么会出现，数据横跨两个块，但是没有对齐，当启用一级缓存的时候，就要两个128字节的事务来完成
![4-11](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-11.png)

4. 线程束所有线程请求同一个地址，那么肯定落在一个缓存行范围（缓存行的概念没提到过，就是主存上一个可以被一次读到缓存中的一段数据。），那么如果按照请求的是4字节数据来说，使用一级缓存的利用率是 $\frac{4}{128}=3.125\%$
![4-12](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-12.png)

5. 比较坏的情况，前面提到过最坏的，就是每个线程束内的线程请求的都是不同的缓存行内，这里比较坏的情况就是，所有数据分布在 $N$ 个缓存行上，其中 $1\leq N\leq 32$，那么请求32个4字节的数据，就需要 $N$ 个事务来完成，利用率也是 $\frac{1}{N}$
![4-13](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-13.png)


CPU和GPU的一级缓存有显著的差异，GPU的一级缓存可以通过编译选项等控制，CPU不可以，而且CPU的一级缓存是的替换算法是有使用频率和时间局部性的，GPU则没有。
### 没有缓存的加载
没有缓存的加载是指的没有通过一级缓存，二级缓存则是不得不经过的。
当不使用一级缓存的时候，内存事务的粒度变为32字节，更细粒度的好处是提高利用律，这个很好理解，比如你每次喝水只能选择一瓶大瓶500ml的或则一个小瓶的250ml，当你非常渴的时候需要400ml水分，喝大瓶的，比较方便，因为如果喝小瓶的一瓶不够，还需要再喝一瓶，此时大瓶的方便.但如果你需要200ml的水分的时候，小瓶的利用率就高很多。细粒度的访问就是用小瓶喝水，虽然体积小，但是每次的利用率都高了不少，针对上面使用缓存的情况5，可能效果会更好。
继续我们的图解：
1. 对齐合并访问128字节，不用说，还是最理想的情况，使用4个段，利用率 $100\%$
![4-14](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-14.png)
2. 对齐不连续访问128字节，都在四个段内，且互不相同，这样的利用率也是  $100\%$
![4-15](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-15.png)

3. 连续不对齐，一个段32字节，所以，一个连续的128字节的请求，即使不对齐，最多也不会超过五个段，所以利用率是 $\frac{4}{5}=80\%$ ,如果不明白为啥不能超过5个段，请注意前提是连续的，这个时候不可能超过五段
![4-16](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-16.png)

4. 所有线程访问一个4字节的数据，那么此时的利用率是 $\frac{4}{32}=12.5\%$
![4-17](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-17.png)

5. 最欢的情况，所有目标数据分散在内存的各个角落，那么需要 N个内存段， 此时与使用一级缓存的作比较也是有优势的因为 $N\times 128$ 还是要比 $N\times 32$ 大不少，这里假设 $N$ 不会因为 $128$ 还是 $32$ 而变的，而实际情况，当使用大粒度的缓存行的时候， $N$ 有可能会减小
![4-18](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-18.png)


### 非对齐读取示例

下面就非对齐读取进行演示，
代码如下：
```c++
#include <cuda_runtime.h>
#include <stdio.h>
#include "freshman.h"


void sumArrays(float * a,float * b,float * res,int offset,const int size)
{

    for(int i=0,k=offset;k<size;i++,k++)
    {
        res[i]=a[k]+b[k];
    }

}
__global__ void sumArraysGPU(float*a,float*b,float*res,int offset,int n)
{
  //int i=threadIdx.x;
  int i=blockIdx.x*blockDim.x+threadIdx.x;
  int k=i+offset;
  if(k<n)
    res[i]=a[k]+b[k];
}
int main(int argc,char **argv)
{
  int dev = 0;
  cudaSetDevice(dev);

  int nElem=1<<18;
  int offset=0;
  if(argc>=2)
    offset=atoi(argv[1]);
  printf("Vector size:%d\n",nElem);
  int nByte=sizeof(float)*nElem;
  float *a_h=(float*)malloc(nByte);
  float *b_h=(float*)malloc(nByte);
  float *res_h=(float*)malloc(nByte);
  float *res_from_gpu_h=(float*)malloc(nByte);
  memset(res_h,0,nByte);
  memset(res_from_gpu_h,0,nByte);

  float *a_d,*b_d,*res_d;
  CHECK(cudaMalloc((float**)&a_d,nByte));
  CHECK(cudaMalloc((float**)&b_d,nByte));
  CHECK(cudaMalloc((float**)&res_d,nByte));
  CHECK(cudaMemset(res_d,0,nByte));
  initialData(a_h,nElem);
  initialData(b_h,nElem);

  CHECK(cudaMemcpy(a_d,a_h,nByte,cudaMemcpyHostToDevice));
  CHECK(cudaMemcpy(b_d,b_h,nByte,cudaMemcpyHostToDevice));

  dim3 block(1024);
  dim3 grid(nElem/block.x);
  double iStart,iElaps;
  iStart=cpuSecond();
  sumArraysGPU<<<grid,block>>>(a_d,b_d,res_d,offset,nElem);
  cudaDeviceSynchronize();
  iElaps=cpuSecond()-iStart;
  CHECK(cudaMemcpy(res_from_gpu_h,res_d,nByte,cudaMemcpyDeviceToHost));
  printf("Execution configuration<<<%d,%d>>> Time elapsed %f sec --offset:%d \n",grid.x,block.x,iElaps,offset);


  sumArrays(a_h,b_h,res_h,offset,nElem);

  checkResult(res_h,res_from_gpu_h,nElem);
  cudaFree(a_d);
  cudaFree(b_d);
  cudaFree(res_d);

  free(a_h);
  free(b_h);
  free(res_h);
  free(res_from_gpu_h);

  return 0;
}

```
编译指令：
```bash
tony@tony-Lenovo:~/Project/CUDA_Freshman/18_sum_array_offset$ nvcc -O3 -arch=sm_35 -Xptxas -dlcm=cg -I ../include/ sum_array_offset.cu -o sum_array_offset
```
运行结果

![res-1](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/res-1.png)


![res-cg_nvprof](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/res-cg_nvprof.png)

编译指令，启用一级缓存：

```bash
tony@tony-Lenovo:~/Project/CUDA_Freshman/18_sum_array_offset$ nvcc -O3 -arch=sm_35 -Xptxas -dlcm=ca -I ../include/ sum_array_offset.cu -o sum_array_offset
```
![res-2](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/res-2.png)

![res-ca_nvprof](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/res-ca_nvprof.png)

这里我们使用的指标是：
$$
全局加载效率=\frac{请求的全局内存加载吞吐量}{所需的全局内存加载吞吐量}
$$

### 只读缓存
只读缓存最初是留给纹理内存加载用的，在3.5以上的设备，只读缓存也支持使用全局内存加载代替一级缓存。也就是说3.5以后的设备，可以通过只读缓存从全局内存中读数据了。
只读缓存粒度32字节，对于分散读取，细粒度优于一级缓存
有两种方法指导内存从只读缓存读取：
1. 使用函数 _ldg
2. 在间接引用的指针上使用修饰符

代码：
```c++
__global__ void copyKernel(float * in,float* out)
{
    int idx=blockDim*blockIdx.x+threadIdx.x;
    out[idx]=__ldg(&in[idx]);

}
```
注意函数参数，然后就能强制使用只读缓存了。

## 全局内存写入
内存的写入和读取（或者叫做加载）是完全不同的，并且写入相对简单很多。一级缓存不能用在 Fermi 和 Kepler GPU上进行存储操作，发送到设备前，只经过二级缓存，存储操作在32个字节的粒度上执行，内存事物也被分为一段两端或者四段，如果两个地址在一个128字节的段内但不在64字节范围内，则会产生一个四段的事务，其他情况以此类推。
我们将内存写入也参考前面的加载分为下面这些情况：
1. 对齐的，访问一个连续的128字节范围。存储操作使用一个4段事务完成：
![4-19](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-19.png)

2. 分散在一个192字节的范围内，不连续，使用3个一段事务来搞定
![4-20](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-20.png)

3. 对齐的，在一个64字节的范围内，使用一个两段事务完成。
![4-21](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-21.png)
### 非对齐写入示例
与读取情况类似，且更简单，因为始终不经过一级缓存，所以略过此实验。

## 结构体数组与数组结构体
写过C语言的人对结构体都应该非常了解，结构体说白了就是基础数据类型组合出来的新的数据类型，这个新的数据类型在内存中表现是：结构中的成员在内存里对齐的依次排开，然后我们我们就有了接下来的话题，数组的结构体，和结构体的数组。
数组结构体（AoS）就是一个数组，每个元素都是一个结构体，而结构体数组（SoA）就是结构体中的成员是数组用代码表示：
AoS
```c++
struct A a[N];
```
SoA
```c++
struct A{
    int a[N];
    int b[N]
}a;
```
如果你分不清这两个名字，没关系，我也分不清，记住AoS是数组就行了，CUDA对细粒度数组是非常友好的，但是对粗粒度如结构体组成的数组就不太友好了，具体表现在，内存访问利用率低。比如当一个线程要访问结构体中的某个成员的时候，当三十二个线程同时访问的时候，SoA的访问就是连续的，而AoS则是不连续：
![4-22](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/4-22.png)
这样看来AoS访问效率只有 $50\%$
对比AoS和SoA的内存布局，我们能得到下面结论。
- 并行编程范式，尤其是SIMD（单指令多数据）对SoA更友好。CUDA中普遍倾向于SoA因为这种内存访问可以有效地合并。



### AoS数据布局的简单数学运算
我们看一下AoS的例子

```c++
#include <cuda_runtime.h>
#include <stdio.h>
#include "freshman.h"

struct naiveStruct{
    float a;
    float b;
};
void sumArrays(float * a,float * b,float * res,const int size)
{

    for(int i=0;i<size;i++)
    {
        res[i]=a[i]+b[i];
    }

}
__global__ void sumArraysGPU(float*a,float*b,struct naiveStruct* res,int n)
{
  //int i=threadIdx.x;
  int i=blockIdx.x*blockDim.x+threadIdx.x;
  if(i<n)
    res[i].a=a[i]+b[i];
}
void checkResult_struct(float* res_h,struct naiveStruct*res_from_gpu_h,int nElem)
{
    for(int i=0;i<nElem;i++)
        if (res_h[i]!=res_from_gpu_h[i].a)
        {
            printf("check fail!\n");
            exit(0);
        }
    printf("result check success!\n");
}
int main(int argc,char **argv)
{
  int dev = 0;
  cudaSetDevice(dev);

  int nElem=1<<18;
  int offset=0;
  if(argc>=2)
    offset=atoi(argv[1]);
  printf("Vector size:%d\n",nElem);
  int nByte=sizeof(float)*nElem;
  int nByte_struct=sizeof(struct naiveStruct)*nElem;
  float *a_h=(float*)malloc(nByte);
  float *b_h=(float*)malloc(nByte);
  float *res_h=(float*)malloc(nByte_struct);
  struct naiveStruct *res_from_gpu_h=(struct naiveStruct*)malloc(nByte_struct);
  memset(res_h,0,nByte);
  memset(res_from_gpu_h,0,nByte);

  float *a_d,*b_d;
  struct naiveStruct* res_d;
  CHECK(cudaMalloc((float**)&a_d,nByte));
  CHECK(cudaMalloc((float**)&b_d,nByte));
  CHECK(cudaMalloc((struct naiveStruct**)&res_d,nByte_struct));
  CHECK(cudaMemset(res_d,0,nByte_struct));
  initialData(a_h,nElem);
  initialData(b_h,nElem);

  CHECK(cudaMemcpy(a_d,a_h,nByte,cudaMemcpyHostToDevice));
  CHECK(cudaMemcpy(b_d,b_h,nByte,cudaMemcpyHostToDevice));

  dim3 block(1024);
  dim3 grid(nElem/block.x);
  double iStart,iElaps;
  iStart=cpuSecond();
  sumArraysGPU<<<grid,block>>>(a_d,b_d,res_d,nElem);
  cudaDeviceSynchronize();
  iElaps=cpuSecond()-iStart;
  CHECK(cudaMemcpy(res_from_gpu_h,res_d,nByte_struct,cudaMemcpyDeviceToHost));
  printf("Execution configuration<<<%d,%d>>> Time elapsed %f sec\n",grid.x,block.x,iElaps);


  sumArrays(a_h,b_h,res_h,nElem);

  checkResult_struct(res_h,res_from_gpu_h,nElem);
  cudaFree(a_d);
  cudaFree(b_d);
  cudaFree(res_d);

  free(a_h);
  free(b_h);
  free(res_h);
  free(res_from_gpu_h);

  return 0;
}

```
```bash
nvcc -O3 -arch=sm_35 -Xptxas -dlcm=ca -I ../include/ AoS.cu -o  AoS
```

![AoS](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/aos.png)
```bash
nvcc -O3 -arch=sm_35 -Xptxas -dlcm=cg -I ../include/ AoS.cu -o  AoS
```
![AoS2](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/aos2.png)


### SoA数据布局的简单数学运算
然后看SoA的例子
```c++
#include <cuda_runtime.h>
#include <stdio.h>
#include "freshman.h"


void sumArrays(float * a,float * b,float * res,int offset,const int size)
{

    for(int i=0,k=offset;k<size;i++,k++)
    {
        res[i]=a[k]+b[k];
    }

}
__global__ void sumArraysGPU(float*a,float*b,float*res,int offset,int n)
{
  //int i=threadIdx.x;
  int i=blockIdx.x*blockDim.x*4+threadIdx.x;
  int k=i+offset;
  if(k+3*blockDim.x<n)
  {
      res[i]=a[k]+b[k];
      res[i+blockDim.x]=a[k+blockDim.x]+b[k+blockDim.x];
      res[i+blockDim.x*2]=a[k+blockDim.x*2]+b[k+blockDim.x*2];
      res[i+blockDim.x*3]=a[k+blockDim.x*3]+b[k+blockDim.x*3];
  }

}

int main(int argc,char **argv)
{
  int dev = 0;
  cudaSetDevice(dev);
  int block_x=512;
  int nElem=1<<18;
  int offset=0;
  if(argc==2)
    offset=atoi(argv[1]);
  else if(argc==3)
    {
        offset=atoi(argv[1]);
        block_x=atoi(argv[2]);
    }
  printf("Vector size:%d\n",nElem);
  int nByte=sizeof(float)*nElem;
  float *a_h=(float*)malloc(nByte);
  float *b_h=(float*)malloc(nByte);
  float *res_h=(float*)malloc(nByte);
  float *res_from_gpu_h=(float*)malloc(nByte);
  memset(res_h,0,nByte);
  memset(res_from_gpu_h,0,nByte);

  float *a_d,*b_d,*res_d;
  CHECK(cudaMalloc((float**)&a_d,nByte));
  CHECK(cudaMalloc((float**)&b_d,nByte));
  CHECK(cudaMalloc((float**)&res_d,nByte));
  CHECK(cudaMemset(res_d,0,nByte));
  initialData(a_h,nElem);
  initialData(b_h,nElem);

  CHECK(cudaMemcpy(a_d,a_h,nByte,cudaMemcpyHostToDevice));
  CHECK(cudaMemcpy(b_d,b_h,nByte,cudaMemcpyHostToDevice));

  dim3 block(block_x);
  dim3 grid(nElem/block.x);
  double iStart,iElaps;
  iStart=cpuSecond();
  sumArraysGPU<<<grid,block>>>(a_d,b_d,res_d,offset,nElem);
  cudaDeviceSynchronize();
  iElaps=cpuSecond()-iStart;

  printf("warmup Time elapsed %f sec\n",iElaps);
  iStart=cpuSecond();
  sumArraysGPU<<<grid,block>>>(a_d,b_d,res_d,offset,nElem);
  cudaDeviceSynchronize();
  iElaps=cpuSecond()-iStart;
  CHECK(cudaMemcpy(res_from_gpu_h,res_d,nByte,cudaMemcpyDeviceToHost));
  printf("Execution configuration<<<%d,%d>>> Time elapsed %f sec --offset:%d \n",grid.x,block.x,iElaps,offset);


  sumArrays(a_h,b_h,res_h,offset,nElem);

  checkResult(res_h,res_from_gpu_h,nElem-4*block_x);
  cudaFree(a_d);
  cudaFree(b_d);
  cudaFree(res_d);

  free(a_h);
  free(b_h);
  free(res_h);
  free(res_from_gpu_h);

  return 0;
}

```
```bash
nvcc -O3 -arch=sm_35 -Xptxas -dlcm=ca -I ../include/ SoA.cu -o SoA
```

![SoA-ca](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/soa-ca.png)

```bash
nvcc -O3 -arch=sm_35 -Xptxas -dlcm=cg -I ../include/ SoA.cu -o SoA
```

![SoA-cg](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/soa-cg.png)


## 性能调整
优化设备内存带宽利用率有两个目标：
1. 对齐合并内存访问，以减少带宽的浪费
2. 足够的并发内存操作，以隐藏内存延迟

第三章我们讲过优化指令吞吐量的核函数，实现并发内存访问量最大化是通过以下方式得到的：
1. 增加每个线程中执行独立内存操作的数量
2. 对核函数启动的执行配置进行试验，已充分体现每个SM的并行性

接下来我们就按照这个思路对程序进行优化试验：展开技术和增大并行性。
### 展开技术
把前面讲到的展开技术用到向量加法上，我们来看看其对内存效率的影响：
代码

```c++
#include <cuda_runtime.h>
#include <stdio.h>
#include "freshman.h"


void sumArrays(float * a,float * b,float * res,int offset,const int size)
{

    for(int i=0,k=offset;k<size;i++,k++)
    {
        res[i]=a[k]+b[k];
    }

}
__global__ void sumArraysGPU(float*a,float*b,float*res,int offset,int n)
{
  //int i=threadIdx.x;
  int i=blockIdx.x*blockDim.x*4+threadIdx.x;
  int k=i+offset;
  if(k+3*blockDim.x<n)
  {
      res[i]=a[k]+b[k];
      res[i+blockDim.x]=a[k+blockDim.x]+b[k+blockDim.x];
      res[i+blockDim.x*2]=a[k+blockDim.x*2]+b[k+blockDim.x*2];
      res[i+blockDim.x*3]=a[k+blockDim.x*3]+b[k+blockDim.x*3];
  }

}

int main(int argc,char **argv)
{
  int dev = 0;
  cudaSetDevice(dev);
  int block_x=512;
  int nElem=1<<18;
  int offset=0;
  if(argc==2)
    offset=atoi(argv[1]);
  else if(argc==3)
    {
        offset=atoi(argv[1]);
        block_x=atoi(argv[2]);
    }
  printf("Vector size:%d\n",nElem);
  int nByte=sizeof(float)*nElem;
  float *a_h=(float*)malloc(nByte);
  float *b_h=(float*)malloc(nByte);
  float *res_h=(float*)malloc(nByte);
  float *res_from_gpu_h=(float*)malloc(nByte);
  memset(res_h,0,nByte);
  memset(res_from_gpu_h,0,nByte);

  float *a_d,*b_d,*res_d;
  CHECK(cudaMalloc((float**)&a_d,nByte));
  CHECK(cudaMalloc((float**)&b_d,nByte));
  CHECK(cudaMalloc((float**)&res_d,nByte));
  CHECK(cudaMemset(res_d,0,nByte));
  initialData(a_h,nElem);
  initialData(b_h,nElem);

  CHECK(cudaMemcpy(a_d,a_h,nByte,cudaMemcpyHostToDevice));
  CHECK(cudaMemcpy(b_d,b_h,nByte,cudaMemcpyHostToDevice));

  dim3 block(block_x);
  dim3 grid(nElem/block.x);
  double iStart,iElaps;
  iStart=cpuSecond();
  sumArraysGPU<<<grid,block>>>(a_d,b_d,res_d,offset,nElem);
  cudaDeviceSynchronize();
  iElaps=cpuSecond()-iStart;

  printf("warmup Time elapsed %f sec\n",iElaps);
  iStart=cpuSecond();
  sumArraysGPU<<<grid,block>>>(a_d,b_d,res_d,offset,nElem);
  cudaDeviceSynchronize();
  iElaps=cpuSecond()-iStart;
  CHECK(cudaMemcpy(res_from_gpu_h,res_d,nByte,cudaMemcpyDeviceToHost));
  printf("Execution configuration<<<%d,%d>>> Time elapsed %f sec --offset:%d \n",grid.x,block.x,iElaps,offset);


  sumArrays(a_h,b_h,res_h,offset,nElem);

  checkResult(res_h,res_from_gpu_h,nElem-4*block_x);
  cudaFree(a_d);
  cudaFree(b_d);
  cudaFree(res_d);

  free(a_h);
  free(b_h);
  free(res_h);
  free(res_from_gpu_h);

  return 0;
}

```
编译指令。

```bash
nvcc -O3 sum_array_offset_unrolling.cu -o sum_array_offset_unrolling -arch=sm_35 -Xptxas -dlcm=cg -I ../include/
```
![unrolling-1](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/unrolling-1.png)

nvprof 内存效率

![unrolling-nv](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/unrolling-nv.png)

### 增大并行性
通过调整块的大小来实现并行性调整，也是前面讲过的套路，我们关注的还是内存利用效率
代码同上面的展开技术。
![res-block](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/res-block.png)

offset=11的时候

![res-off-set-11](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-4-3-内存访问模式/res-off-set-11.png)

由于数据量少，所以时间差距不大，512有最佳速度，不仅因为内存，还有并行性等多方面因素，这个前面我们也曾提到过。要看综合能力。

本文全部代码都在Github上有完整版，请访问：[https://github.com/Tony-Tan/CUDA_Freshman](https://github.com/Tony-Tan/CUDA_Freshman)
## 总结
这是我今年写作时间最长的一篇博客，写了三天，主要是代码比较多，结果也比较多
这里我们没用Cmake，而是用的指令，原因是方便修改编译选项，试验时间结果不明显的原因是数据量小，部分结果和书上不一致，主要是书的时间比较久了，GPU换代太快。
全局内存本篇算是比较完整了，后面还有其他内存知识，我们继续。
