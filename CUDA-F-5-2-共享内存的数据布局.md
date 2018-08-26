---
title: 【CUDA 基础】5.2 共享内存的数据布局
categories:
    - CUDA
    - Freshman
keywords:
    - 行主序
    - 列主序
toc: true
date: 2018-06-02 21:01:03
---

**Abstract:** 本文主要研究几个关于共享内存的例子，以此来了解共享内存的性质，为我们的核函数加速
**Keywords:** 行主序，列主序，填充与无填充，从线程索引体映射数据元素

<!--more-->
## 开篇废话
同一个东西，A花大工夫做到极致，成本100，售价200；C模仿A的做法快速的通过仿制，节省了研发试验的所有开销，但是没有做到A那么完美，成本25，售价140。A虽然好但是不见得销量有C高，并且A的利润并没有C那么高，所以，作为商人，选择C是没错的，商人的目的就是盈利，但是问题来了，如果不是商人呢？而是一个科学家呢？
本文我们主要研究共享内存的数据布局，通过代码实现，来观察运行数据，换句话说，我们主要研究上一篇中的放西瓜，取西瓜，以及放冬瓜等的一些列操作对性能的影响，以及如何才能使效率最大化。
几个例子包括以下几个主题：
- 方阵与矩阵数组
- 行主序与列主序
- 静态与动态共享内存的声明
- 文件范围与内核范围的共享内存
- 内存填充与无内存填充

当使用共享内存设计核函数的时候下面两个概念是非常重要的：
1. 跨内存存储体映射数据元素
2. 从线程索引到共享内存偏移的映射

当上面这些主题和概念都得到很好地理解，设计一个高效的使用共享内存的核函数就没什么问题了，其可以避免存储体冲突并充分利用共享内存的优势。
注意，从几何上讲，方形属于矩形，这里我们说的矩形时指长方形。


## 方形共享内存
我们前面说过我们的线程块可以是一维二维和三维的，对应的线程编号是threadIdx.x, threadIdx.y以及threadIdx.z，为了对应一个二维的共享内存，我们假设我们使用二维的线程块，那么对于一个二维的共享内存
```c++
#define N 32
...
__shared__ int x[N][N];
...
```
当我们使用二维块的时候，很有可能会使用下面这种方式来索引x的数据：
```c++
#define N 32
...
__shared__ int x[N][N];
...
int a=x[threadIdx.y][threadIdx.x];
```
当然这个索引就是 $(y,x)$ 对应的，我们也可以用 $(x,y)$ 来索引。
在CPU中，如果用循环遍历二维数组，尤其是双层循环的方式，我们倾向于内层循环对应x，因为这样的访问方式在内存中是连续的，因为CPU的内存是线性存储的，但是GPU的共享内存并不是线性的，而是二维的，分成不同存储体的，并且，并行也不是循环，那么这时候，问题完全不同，没有任何可比性。
回顾放西瓜的例子以及存储体冲突的特性，容易想到，我们最应该避免的是存储体冲突，那么对应的问题就来了，我们每次执行一个线程束，对于二维线程块，一个线程束是按什么划分的呢？是按照threadIdx.x 维进行划分还是按照threadIdx.y维进行划分的呢？
这句话有点迷糊？那我再啰嗦一遍，因为这个很关键，我们每次执行的是一个线程束，线程束里面有很多线程，对于一个二维的块，切割线程束有两种方法，顺着y切，那么就是threadIdx.x固定（变化慢），而threadIdx.y是连续的变化，顺着x切相反；CUDA明确的告诉你，我们是顺着x切的，也就是一个线程束中的threadIdx.x 连续变化。
我们的数据是按照行放进存储体中的这是固定的，所以我们希望，这个线程束中取数据是按照行来进行的，所以
```c++
x[threadIdx.y][threadIdx.x];
```
这种访问方式是最优的，threadIdx.x在线程束中体现为连续变化的，而对应到共享内存中也是遍历共享内存的同一行的不同列

上面这个确实有点绕，我们可以画画图，多想象一下CUDA的运行原理，这个就好理解了，说白了就是不要一个线程束中访问一列共享内存，而是要访问一行。

![5-12](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/5-12.png)

对照上图，我们把一个int类型（四字节）的1024个元素的数组放到共享内存A中，每个int的索引对应到蓝框中，假设我们的块大小是 $(32,32)$ 那么我们第一个线程束就是 threadIdx.y=0,threadIdx.x=0......31,如果我们使用
```c++
A[threadIdx.x][threadIdx.y];
```
的索引方式，就会得到绿框的数据，可想而知，这冲突达到了最大，效率最低、
果我们使用
```c++
A[threadIdx.y][threadIdx.x];
```
我们就会得到红色框中的数据，无冲突，一个事务完成。

本文全部代码在GitHub上可下载使用:[https://github.com/Tony-Tan/CUDA_Freshman](https://github.com/Tony-Tan/CUDA_Freshman)
### 行主序访问和列主序访问
行主序访问和列主序访问我们上面已经把原理基本介绍清楚了，我们下面看实现后的试验，这里我们研究的访问，包括读和写，也就是加载和存储。

我们定义块的尺寸为
```c++
#define BDIMX 32
#define BDIMY 32
```
核函数只完成简单的两个操作：
- 将全局线程索引值存入二维共享内存
- 从共享内存中按照行主序读取这些值并存到全局内存中

项目完整的代码在24_shared_memory_read_data这个文件夹下，下文我们只贴部分代码。
核函数如下
```c++
__global__ void setRowReadRow(int * out)
{
    __shared__ int tile[BDIMY][BDIMX];
    unsigned int idx=threadIdx.y*blockDim.x+threadIdx.x;

    tile[threadIdx.y][threadIdx.x]=idx;
    __syncthreads();
    out[idx]=tile[threadIdx.y][threadIdx.x];
}
```
- 定义一个共享内存，大小为 $32\times 32$
- 计算当前线程的全局位置的值idx
- 将idx这个无符号整数值写入二维共享内存tile\[threadIdx.y\]\[threadIdx.x\]中
- 同步
- 将共享内存tile\[threadIdx.y\]\[threadIdx.x\]中的值写入全局内存对应的idx位置处

核函数的内存工作：
1. 共享内存的写入
2. 共享内存的读取
3. 全局内存的写入

这个核函数按照行主序读和写，所以对于共享内存没有读写冲突
另一种方法就是按照列主序访问了，核函数代码如下：
```c++
__global__ void setColReadCol(int * out)
{
    __shared__ int tile[BDIMY][BDIMX];
    unsigned int idx=threadIdx.y*blockDim.x+threadIdx.x;

    tile[threadIdx.x][threadIdx.y]=idx;
    __syncthreads();
    out[idx]=tile[threadIdx.x][threadIdx.y];
}
```
原理不再赘述，我们直接看运行结果：
对于使用nvprof如果出现 ======== Error: unified memory profiling failed.错误，是因为系统的保护机制，所以使用sudo权限来执行即可，如果sudo找不到你的nvprof，你可以用完整路径，或则添加到环境变量：

![re-1-1](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-1.png)

可见行主序的平均时间是 $1.552\mu s$ 而列主序是 $2.4640\mu s$ 注意如果直接使用来方法即cpu计时，那么会非常不准，比如我们红色方框内就是cpu计时的结果，原因是数据量太小，运行时间太短，误差相对就太大了，这显然是错误，很有可能我们前面也出现过理论和实际不符的情况也是因为计时有问题。

接下来我们看看检测存储体冲突的指标，会是什么数据：
```bash
shared_load_transactions_per_request
shared_store_transactions_per_request
```
- shared_load_transactions_per_request 结果：
```bash
nvprof  --metrics shared_load_transactions_per_request ./shared_memory_read_data
```

![sltpr](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/sltpr.png)
可以看到load过程行主序1个事务，而列主序32个

- shared_store_transactions_per_request 结果：

```bash
nvprof  --metrics shared_store_transactions_per_request ./shared_memory_read_data
```
![sstpr](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/sstpr.png)

同样行主序的事务是1，而列主序的事务是32

注意，我们这个设备是4-byte宽的，上面第二张图中有相关信息。

### 按行主序写和按列主序读
接下来我们来点混合的，我们按照行主序写入，按照列主序读取，声明下，这个核函数本身没什么意义，就像前面的核函数，其根本作用就是为了我们测试各种指标用到，所以大家不要过度关心输入输出是什么，而是关系不同的读写方法，在指标和性能上的差异。
按照列主序读：
```c++
out[idx]=tile[threadIdx.x][threadIdx.y];
```
按照行主序写：
```c++
tile[threadIdx.y][threadIdx.x]=idx;
```

核函数跟上面差不多，我就不贴了，直接看看结果：

![re-1-2](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-2.png)

shared_load_transactions_per_request:
![re-1-3](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-3.png)

shared_store_transactions_per_request
![re-1-4](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-4.png)

可以看出load的时候。也就是读取共享内存，然后写入全局内存的这步，是有冲突的，32路冲突是显然的。
同样的试验可以测试按照行主序读和列主序写，这里就不啰嗦一遍了，因为一毛一样。

### 动态共享内存
共享内存没有malloc但是也可以到运行时才分配，具体机制我没去了解，是不是共享内存也分堆和栈，但是我们有必要了解这个方法，因为写过C++程序的都知道，基本上我们的大部分变量是要靠动态分配手动管理的，CUDA好的一点就是动态的共享内存，不需要手动回收。
我们看核函数：
```c++
__global__ void setRowReadColDyn(int * out)
{
    extern __shared__ int tile[];
    unsigned int row_idx=threadIdx.y*blockDim.x+threadIdx.x;
    unsigned int col_idx=threadIdx.x*blockDim.y+threadIdx.y;
    tile[row_idx]=row_idx;
    __syncthreads();
    out[row_idx]=tile[col_idx];
}
```

其中extern用于表明这个共享内存是运行时才知道的
```c++
extern __shared__ int tile[];
```
一个int型的共享内存长度不知道，什么时候才能知道？当然是运行的时候：
```c++
setRowReadColDyn<<<grid,block,(BDIMX)*BDIMY*sizeof(int)>>>(out);
```
核函数配置参数，网格大小，块大小，第三个参数就是共享内存的大小了，那么问题来了，我们可不可以声明多个动态共享内存呢？是否可以继续添加参数呢？这个我还没试验，可以留作一个思考。
运行结果当然也没什么出奇的：

![re-1-5](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-5.png)
shared_load_transactions_per_request:

![re-1-6](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-6.png)

shared_store_transactions_per_request:

![re-1-7](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-7.png)

动态静态运行结果没什么差别，冲突不变。

### 填充静态声明的共享内存
填充我们在前面博客大概提到了，我们通过改变声明的共享内存大小来填充一些位置，比如最后一列，我们声明了这个尺寸的共享内存，其会自动对应到CUDA模型上的二维共享内存存储体，换句话说，所谓填充是在声明的时候产生的， 声明一个二维共享内存，或者一维共享内存，编译器会自动将其重新整理到一个二维的空间中，这个空间包含32个存储体，每个存储体宽度一定，换句话说，你声明一个二维存储体，编译器会把声明的二维转换成一维线性的，然后再重新整理成二维按照32个存储体，4-Byte/8-Byte宽的内存分布，然后再进行运算的。
这就是我们填充存储体的最根本原理。
原理明白了，下面的核函数就没啥了，就是加了个常量：
```c++
__global__ void setRowReadColIpad(int * out)
{
    __shared__ int tile[BDIMY][BDIMX+IPAD];
    unsigned int idx=threadIdx.y*blockDim.x+threadIdx.x;

    tile[threadIdx.y][threadIdx.x]=idx;
    __syncthreads();
    out[idx]=tile[threadIdx.x][threadIdx.y];
}
```
运行结果：

![re-1-75](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-75.png)

冲突不见了！同志们，填充起作用了！
### 填充动态声明的共享内存
动态填充和静态填充似乎没什么区别，一个在核函数内声明，一个在参数中体现，代码如下：
```c++
__global__ void setRowReadColDynIpad(int * out)
{
    extern __shared__ int tile[];
    unsigned int row_idx=threadIdx.y*(blockDim.x+1)+threadIdx.x;
    unsigned int col_idx=threadIdx.x*(blockDim.x+1)+threadIdx.y;
    tile[row_idx]=row_idx;
    __syncthreads();
    out[row_idx]=tile[col_idx];
}
```

调用代码：
```c++
setRowReadColDynIpad<<<grid,block,(BDIMX+IPAD)*BDIMY*sizeof(int)>>>(out);
```

填充示意图：
![re-1-8](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-8.png)

唯一要注意的就是索引，当填充了以后我们的行不变，列加宽了，所以索引的时候要：
```c++
unsigned int row_idx=threadIdx.y*(blockDim.x+1)+threadIdx.x;
unsigned int col_idx=threadIdx.x*(blockDim.x+1)+threadIdx.y;
```
有些童鞋可能能对这个索引，有点迷糊，这个时候你不要想硬件上也就是转换后的存储体内是什么样的，我们之关心我们逻辑上的布局就好，也就是上图的样子，因为到存储体会自动的转换过去转换回来，和图上的方式是一一对应的，如果这几个你一起考虑必然会混乱，而不明白为啥要threadIdx.x*(blockDim.x+1)+threadIdx.y

然后就没什么了，结果如下：

![re-1-9](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-9.png)
冲突不见了，同志们，填充起作用了！
### 方形共享内存内核性能的比较
接下来就是比较各个内核，我们直接运行：
```bash
sudo / $\mu$ sr/local/cuda/bin/nvprof ./shared_memory_read_data  11
```

![re-1-10](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-10.png)
可以观察到所有核函数的运行时间



|      Type       | Time(%) |   Time   | Calls |   Avg    |   Min    |   Max    |            Name            |
|:---------------:|:-------:|:--------:|:-----:|:--------:|:--------:|:--------:|:--------------------------:|
| GPU activities: | 16.12%  | 2.5600 $\mu$ s |  1    | 2.5600 $\mu$ s | 2.5600 $\mu$ s |          2.5600 $\mu$ s          | setColReadCol(int*) |
|                 | 13.30%  | 2.1120 $\mu$ s |   1   | 2.1120 $\mu$ s | 2.1120 $\mu$ s | 2.1120 $\mu$ s |        warmup(int*)        |                     |
|                 | 12.90%  | 2.0480 $\mu$ s |   1   | 2.0480 $\mu$ s | 2.0480 $\mu$ s | 2.0480 $\mu$ s |    setRowReadCol(int*)     |                     |
|                 | 12.70%  | 2.0170 $\mu$ s |   1   | 2.0170 $\mu$ s | 2.0170 $\mu$ s | 2.0170 $\mu$ s |   setRowReadColDyn(int*)   |                     |
|                 | 12.70%  | 2.0160 $\mu$ s |   1   | 2.0160 $\mu$ s | 2.0160 $\mu$ s | 2.0160 $\mu$ s |    setColReadRow(int*)     |                     |
|                 |  9.27%  | 1.4720 $\mu$ s |   1   | 1.4720 $\mu$ s | 1.4720 $\mu$ s | 1.4720 $\mu$ s |  setRowReadColIpad(int*)   |                     |
|                 |  8.88%  | 1.4090 $\mu$ s |   1   | 1.4090 $\mu$ s | 1.4090 $\mu$ s | 1.4090 $\mu$ s |    setRowReadRow(int*)    |                     |
|                 |  8.47%  | 1.3450 $\mu$ s |   1   | 1.3450 $\mu$ s | 1.3450 $\mu$ s | 1.3450 $\mu$ s | setRowReadColDynIpad(int*) |


具体内存里有什么我这里就不演示代码了，缩小声明的内存大小，然后打印出来，就能看到结果了。

所有上面这些，只需要明确并且熟练于心的就是我们声明的是我们编程的模型，实际存储的是另一种形状的二维存储体布局，这两个数据分布是一一对应的，我们在写核函数写功能的时候只要考虑我们的编程模型即可得到正确答案，但是优化就要考虑编程模型和存储体分布之间的关系，适当的填充会得到好的结果。
## 矩形共享内存
以上是正方形，长方形类似，只要掌握了编程模型和存储体之间的对应关系这个过程，其实来个三角形的内存都是无所谓的，但是为了配合我们还是写一下吧，从简。
长方形共享内存：
```c++
#define BDIMX_RECT 32
#define BDIMY_RECT 16
```
不一样长就是举矩形了，这时候我们就要考虑非正方形的索引问题了，这时候不能简单的交换坐标了，而是需要用我上面说到的，先转换成线性的，然后再重新计算行和列的坐标
### 行主序访问和列主序访问
这个和正方形的完全一样，我们就不再啰嗦了，只是调整了下共享内存的尺寸而已，其他没有变化。我们关注的是按行读按列写或者按列读按行写这种需要坐标转换的情况。
### 行主序写操作和列主序读操作
没错，这就是复杂的情况，我们先贴代码，然后好研究一下：
```c++
__global__ void setRowReadColRect(int * out)
{
    __shared__ int tile[BDIMY_RECT][BDIMX_RECT];
    unsigned int idx=threadIdx.y*blockDim.x+threadIdx.x;
    unsigned int icol=idx%blockDim.y;
    unsigned int irow=idx/blockDim.y;
    tile[threadIdx.y][threadIdx.x]=idx;
    __syncthreads();
    out[idx]=tile[icol][irow];
}
```
解释：
先贴图
![sltpr](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/rect.png)
注意红色为存储体空间，蓝色小标1，2，3，4表示四个不同的模型。
```c++
unsigned int idx=threadIdx.y*blockDim.x+threadIdx.x;
```
这句就是计算线性位置，因为我们把不同的尺寸的共享内存映射到线性空间图中 1->3 的过程；接着，
```c++
unsigned int icol=idx%blockDim.y;
unsigned int irow=idx/blockDim.y;
```
这一步是3->2 的过程，很多同学可能不明白，其实我也不明白，但是我仔细研究了一下，我们并没有改变原始tile的形状，我们只是改变了数组的索引顺序，原始的我们是按照1中的绿线从左到右逐行进行的，然后我们通过除法和取余，得到的新坐标是按照3中的绿色箭头从上到下，逐列进行的。所以当1中x方向坐标增加一个，对应于3中y方向坐标加一个，但是数组的形状不变，这就成了一个按照行写入，一个按照列读取。
只是这样就不是前面方形的32个冲突了，而是16个冲突，因为每进行一行32个元素，对应的是两列，那么就是16个冲突
运行结果：

![re-1-11](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-11.png)

可见如我们所推测的16路冲突在load的时候出现了。


### 动态声明的共享内存
接着就是动态版本的了，基本没区别，
代码：
```c++
__global__ void setRowReadColRectDyn(int * out)
{
    extern __shared__ int tile[];
    unsigned int idx=threadIdx.y*blockDim.x+threadIdx.x;
    unsigned int icol=idx%blockDim.y;
    unsigned int irow=idx/blockDim.y;
    unsigned int col_idx=icol*blockDim.x+irow;
    tile[idx]=idx;
    __syncthreads();
    out[idx]=tile[col_idx];
}
```
启动核函数：
```c++
setRowReadColRectDynPad<<<grid_rect,block_rect,(BDIMX+1)*BDIMY*sizeof(int)>>>(out);
```
和方形的类似，结果如下：

![re-1-12](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-12.png)

结果同静态内存一致
### 填充静态声明的共享内存
然后我们填充内存，填充内存的时候就需要注意索引计算了，但是并不需要做调整。
那么我们的代码是：
```c++
__global__ void setRowReadColRectPad(int * out)
{
    __shared__ int tile[BDIMY_RECT][BDIMX_RECT+IPAD];
    unsigned int idx=threadIdx.y*blockDim.x+threadIdx.x;
    unsigned int icol=idx%blockDim.y;
    unsigned int irow=idx/blockDim.y;
    tile[threadIdx.y][threadIdx.x]=idx;
    __syncthreads();
    out[idx]=tile[icol][irow];
}
```

![re-1-13](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-13.png)

结果是，在填充了一列的时候，我们只产生了两路冲突
填充两列的时候，代码为
```c++
__global__ void setRowReadColRectPad(int * out)
{
    __shared__ int tile[BDIMY_RECT][BDIMX_RECT+IPAD*2];
    unsigned int idx=threadIdx.y*blockDim.x+threadIdx.x;
    unsigned int icol=idx%blockDim.y;
    unsigned int irow=idx/blockDim.y;
    tile[threadIdx.y][threadIdx.x]=idx;
    __syncthreads();
    out[idx]=tile[icol][irow];
}
```
运行结果，没有冲突

![re-1-14](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-14.png)
如果不明白为什么，可以自己画个图，从长方形转换到存储体空间上，然后按照执行的过程比划比划
### 填充动态声明的共享内存
动态填充和方形的动态填充类似，tile没有二维索引了，所以需要计算出二维索引对应一维位置，其他情况类似，代码如下：
```c++
__global__ void setRowReadColRectDynPad(int * out)
{
    extern __shared__ int tile[];
    unsigned int idx=threadIdx.y*blockDim.x+threadIdx.x;
    unsigned int icol=idx%blockDim.y;
    unsigned int irow=idx/blockDim.y;
    unsigned int row_idx=threadIdx.y*(IPAD+blockDim.x)+threadIdx.x;
    unsigned int col_idx=icol*(IPAD+blockDim.x)+irow;
    tile[row_idx]=idx;
    __syncthreads();
    out[idx]=tile[col_idx];
}
```
索引转换，和二维坐标对应于一维坐标，这些都是前面用过的技术，观察结果，对于添加1列，得出两路冲突，和上面静态填充一致
![re-1-15](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-15.png)

### 矩形共享内存内核性能的比较
矩形的所有核函数运行结果比较

![re-1-16](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-5-2-共享内存的数据布局/re-1-16.png)


|      Type       | Time(%) |   Time   | Calls |   Avg    |   Min    |   Max    |             Name              |
|:---------------:|:-------:|:--------:|:-----:|:--------:|:--------:|:--------:|:-----------------------------:|
| GPU activities: | 66.27%  | 12.512us |   1   | 12.512us | 12.512us | 12.512us |  setRowReadColRectPad(int*)   |
|                 |  8.81%  | 1.6640us |   1   | 1.6640us | 1.6640us | 1.6640us |    setRowReadColRect(int*)    |
|                 |  8.81%  | 1.6640us |   1   | 1.6640us | 1.6640us | 1.6640us |  setRowReadColRectDyn(int*)   |
|                 |  8.47%  | 1.6000us |   1   | 1.6000us | 1.6000us | 1.6000us |         warmup(int*)          |
|                 |  7.63%  | 1.4400us |   1   | 1.4400us | 1.4400us | 1.4400us | setRowReadColRectDynPad(int*) |



## 总结

本文通过一组实验，得到了关于共享内存的存储体内分布情况，以及如何使用动态的共享内存，以及填充来避免冲突。
