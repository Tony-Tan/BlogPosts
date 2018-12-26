---
title: 【CUDA 基础】1.1 异构计算与CUDA
categories:
  - CUDA
  - Freshman
tags:
  - 异构计算
  - CUDA
toc: true
date: 2018-02-15 09:48:57
---

**Abstract:** 介绍异构计算和CUDA概述，完成GPU输出Hello world！
**Keywords:** 异构计算，CUDA

<!--more-->
# 异构计算与CUDA
## 异构计算
异构计算，首先必须了解什么是异构，不同的计算机架构就是异构，上文书我们讲过计算机架构了，就是为了引出异构的概念，按照指令集划分或者按照内存结构划分，但是我觉得只要两片CPU型号不一样就应该叫异构（这个想法先保留，对错不确定）。
GPU本来的任务是做图形图像的，也就是把数据处理成图形图像，图像有个特点就是并行度很高，基本上一定距离意外的像素点之间的计算是独立的，所以属于并行任务。
GPU之前是不可编程的，或者说不对用户开放的，人家本来是做图形计算控制显示器的，虽然对用户不可编程，但是你只要把硬件卖给了我，就由不得你了，然后就有hacker开始想办法给GPU编程，来帮助他们完成规模较大的运算，于是他们研究着色语言或者图形处理原语来和GPU对话。后来黄老板发现了这个是个新的功能啊，然后就让人开发了一套平台，CUDA，然后深度学习火了，顺带着，CUDA也火到爆炸。
刚刚最新消息，英伟达新版本GPU架构会被命名为Turing，一丝欣慰，发自内心深处地敬那些为世界进步做出了杰出贡献的人们，他们是人类未来的希望。
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-1-1-异构计算-CUDA/0.jpeg)

x86 CPU+GPU的这种异构应该是最常见的，也有CPU+FPGA，CPU+DSP等各种各样的组合，CPU+GPU在每个笔记本或者台式机上都能找到。当然超级计算机大部分也采用异构计算的方式来提高吞吐量。
异构架构虽然比传统的同构架构运算量更大，但是其应用复杂度更高，因为要在两个设备上进行计算，控制，传输，这些都需要人为干预，而同构的架构下，硬件部分自己完成控制，不需要人为设计。
### 异构架构
举一个我用的工作站的构成，我使用的是一台 intel i7-4790 CPU加上两台Titan x GPU构成的工作站，GPU插在主板的PCIe卡口上，运行程序的时候，CPU像是一个控制者，指挥两台Titan完成工作后进行汇总，和下一步工作安排，所以CPU我们可以把它看做一个指挥者，主机端，host，而完成大量计算的GPU是我们的计算设备，device。
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-1-1-异构计算-CUDA/1.png)
上面这张图能大致反应CPU和GPU的架构不同。
- 左图：一个四核CPU一般有四个ALU，ALU是完成逻辑计算的核心，也是我们平时说四核八核的核，控制单元，缓存也在片上，DRAM是内存，一般不在片上，CPU通过总线访问内存。
- 右图：GPU，绿色小方块是ALU，我们注意红色框内的部分SM，这一组ALU公用一个Control单元和Cache，这个部分相当于一个完整的多核CPU，但是不同的是ALU多了，control部分变小，可见计算能力提升了，控制能力减弱了，所以对于控制（逻辑）复杂的程序，一个GPU的SM是没办法和CPU比较的，但是对了逻辑简单，数据量大的任务，GPU更搞笑，并且，注意，一个GPU有好多个SM，而且越来越多。

CPU和GPU之间通过PCIe总线连接，用于传递指令和数据，这部分也是后面要讨论的性能瓶颈之一。
一个异构应用包含两种以上架构，所以代码也包括不止一部分：
- 主机代码
- 设备代码

主机代码在主机端运行，被编译成主机架构的机器码，设备端的在设备上执行，被编译成设备架构的机器码，所以主机端的机器码和设备端的机器码是隔离的，自己执行自己的，没办法交换执行。
主机端代码主要是控制设备，完成数据传输等控制类工作，设备端主要的任务就是计算。
因为当没有GPU的时候CPU也能完成这些计算，只是速度会慢很多，所以可以把GPU看成CPU的一个加速设备。
NVIDIA目前的计算平台（不是架构）有：
- Tegra
- Geforce
- Quadro
- Tesla

每个平太针对不同的应用场景，比如Tegra用于嵌入式，Geforce是我们平时打游戏用到，Tesla是我们昨天租的那台腾讯云的，主要用于计算。

上面是根据应用场景分类的几种平台。

衡量GPU计算能力的主要靠下面两种***容量***特征：
- CUDA核心数量（越多越好）
- 内存大小（越大越好）

相应的也有计算能力的***性能***指标:
- 峰值计算能力
- 内存带宽

nvidia自己有一套描述GPU计算能力的代码，其名字就是“计算能力”，主要区分不同的架构，早其架构的计算能力不一定比新架构的计算能力强


| 计算能力 | 架构名  |
| -------- | ------- |
| 1.x      | Tesla   |
| 2.x      | Fermi   |
| 3.x      | Kepler  |
| 4.x      | Maxwell |
| 5.x      | Pascal  |
| 6.x      | Volta   |


这里的Tesla架构，与上面的Tesla平台不同，不要混淆，一个是平台名字，一个是架构名字
### 范例
CPU和GPU相互配合，各有所长，各有所短，不能说GPU就是比CPU强这种幼稚的话：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-1-1-异构计算-CUDA/2.png)

低并行逻辑复杂的程序适合用CPU
高并行逻辑简单的大数据计算适合GPU

一个程序可以进行如下分解，串行部分和并行不分：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-1-1-异构计算-CUDA/3.png)

CPU和GPU线程的区别：
1. CPU线程是重量级实体，操作系统交替执行线程，线程上下文切换花销很大
2. GPU线程是轻量级的，GPU应用一般包含成千上万的线程，多数在排队状态，线程之间切换基本没有开销。
3. CPU的核被设计用来尽可能减少一个或两个线程运行时间的延迟，而GPU核则是大量线程，最大幅度提高吞吐量

### CUDA：一种异构计算平台
CUDA平台不是单单指软件或者硬件，而是建立在Nvidia GPU上的一整套平台，并扩展出多语言支持
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-1-1-异构计算-CUDA/4.png)

CUDA C 是标准ANSI C语言的扩展，扩展出一些语法和关键字来编写设备端代码，而且CUDA库本身提供了大量API来操作设备完成计算。

对于API也有两种不同的层次，一种相对交高层，一种相对底层。
- CUDA驱动API
- CUDA运行时API

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-1-1-异构计算-CUDA/5.png)


驱动API是低级的API，使用相对困难，运行时API是高级API使用简单，其实现基于驱动API。
这两种API是互斥的，也就是你只能用一个，两者之间的函数不可以混合调用，只能用其中的一个库。

一个CUDA应用通常可以分解为两部分，
- CPU 主机端代码
- GPU 设备端代码


CUDA nvcc编译器会自动分离你代码里面的不同部分，如图中主机代码用C写成，使用本地的C语言编译器编译，设备端代码，也就是核函数，用CUDA C编写，通过nvcc编译，链接阶段，在内核程序调用或者明显的GPU设备操作时，添加运行时库。

**注意：核函数是我们后面主要接触的一段代码，就是设备上执行的程序段**

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-1-1-异构计算-CUDA/6.png)

nvcc 是从LLVM开源编译系统为基础开发的。

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-1-1-异构计算-CUDA/7.png)

CUDA工具箱提供编译器，数学库，调试优化等工具，当然CUDA的文档是相当完善的，可以去查阅，当然在我们基本了解基础结构的情况下，直接上来看文档会变得机械化。

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-1-1-异构计算-CUDA/8.png)
## "Hello World!"
Hello World是所有程序初学者都非常喜欢的，之前GPU是不能printf的，我当时就很懵，GPU是个做显示的设备，为啥不能输出，后来就可以直接在CUDA核里面打印信息了，我们写下面程序
``` C++
/*
*hello_world.cu
*/
#include<stdio.h>
__global__ void hello_world(void)
{
  printf("GPU: Hello world!\n");
}
int main(int argc,char **argv)
{
  printf("CPU: Hello world!\n");
  hello_world<<<1,10>>>();
  cudaDeviceReset();//if no this line ,it can not output hello world from gpu
  return 0;
}
```
完整代码可以从以下项目中clone：

[https://github.com/Tony-Tan/CUDA_Freshman](https://github.com/Tony-Tan/CUDA_Freshman)

简单介绍其中几个关键字
```
__global__
```
是告诉编译器这个是个可以在设备上执行的核函数
```
hello_world<<<1,10>>>();
```
这句话C语言中没有'<<<>>>'是对设备进行配置的参数，也是CUDA扩展出来的部分。
```
cudaDeviceReset();
```
这句话如果没有，则不能正常的运行，因为这句话包含了隐式同步，GPU和CPU执行程序是异步的，核函数调用后成立刻会到主机线程继续，而不管GPU端核函数是否执行完毕，所以上面的程序就是GPU刚开始执行，CPU已经退出程序了，所以我们要等GPU执行完了，再退出主机线程。
一般CUDA程序分成下面这些步骤：
1. 分配GPU内存
2. 拷贝内存到设备
3. 调用CUDA内核函数来执行计算
4. 把计算完成数据拷贝回主机端
5. 内存销毁

上面的hello world只到第三步，没有内存交换。

## CUDA C难么？
CPU与GPU的编程主要区别在于对GPU架构的熟悉程度，理解机器的结构是对编程效率影响非常大的一部分，了解你的机器，才能写出更优美的代码，而目前计算设备的架构决定了局部性将会严重影响效率。
数据局部性分两种
- 空间局部性
- 时间局部性

这个两个性质告诉我们，当一个数据被使用，其附近的数据将会很快被使用，当一个数据刚被使用，则随着时间继续其被再次使用的可能性降低，数据可能被重复使用。

CUDA中有两个模型是决定性能的：
- 内存层次结构
- 线程层次结构

CUDA C写核函数的时候我们只写一小段串行代码，但是这段代码被成千上万的线程执行，所有线程执行的代码都是相同的，CUDA编程模型提供了一个层次化的组织线程，直接影响GPU上的执行顺序。

CUDA抽象了硬件实现：
1. 线程组的层次结构
2. 内存的层次结构
3. 障碍同步

这些都是我们后面要研究的，线程，内存是主要研究的对象，我们能用到的工具相当丰富，NVIDIA为我们提供了：
- Nvidia Nsight集成开发环境
- CUDA-GDB 命令行调试器
- 性能分析可视化工具
- CUDA-MEMCHECK工具
- GPU设备管理工具

## 总结
本文从总体上粗略的介绍了CUDA这种高效的异构计算平台，并且概括了我们的将要遇到的苦难和使用到的工具，当我们学会的CUDA，那么编写高效异构计算就会像我们写串行程序一样流畅。
祝大家新年快乐。





原文地址1：[https://www.face2ai.com/CUDA-F-1-1-异构计算-CUDA](https://www.face2ai.com/CUDA-F-1-1-异构计算-CUDA)转载请标明出处
