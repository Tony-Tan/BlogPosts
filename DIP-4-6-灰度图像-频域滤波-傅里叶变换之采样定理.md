---
title: 【数字图像处理】4.6:灰度图像-频域滤波 傅里叶变换之采样定理
date: 2015-01-14 19:17
categories:
  - DIP
keywords:
  - 采样定理
  - 奈奎斯特采样定理
toc: true
---
**Abstract:** 数字图像处理：第19天
**Keywords:** 采样定理，奈奎斯特采样定理
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>


## 开篇废话
采样定理，大名鼎鼎，早有耳闻，就是不知道为啥，但很好记，采样频率一定要大于原始信号最大频率的两倍，为啥是两倍，不知道，怎么从离散恢复出连续信号，也不知道，但前几天学过傅里叶了以后在看采样定理，就会发现其实采样定理是傅里叶的一种应用，或者叫做推广。
采样定理是连接连续信号与离散信号的桥梁，就行傅里叶是连接时域（空域）与频域的桥梁一样，他们的共同点是带我们从另外的角度看问题，也许正是这种原因，这样的定理都格外引人注意，因为他们让我们以不同的角度看世界，也会从另外的角度看到一些之前没看到的角落。
上图：
![Center][]
## 采样定理
设x(t)是某一个带线信号，在 $|w|>w_m$ 时，$X(jw)=0$ ；如果 $w_s>2w_m$ ，其中$w_s=\frac{2\pi}{T}$，那么 $x(t)$ 就唯一由其样本 $x(nT)$ , $n=\dots -2,-1,0,1,2,\dots$ 所确定。

    上一句的话的注释：wm是信号的最高频率，大于 $w_m$ 的频率幅值为0；采样频率为 $w_s$ ，采样周期为 $T$ ，也就是冲击串的周期是T。采样频率一定要大于某个值，也就是采样周期要小于某个值，定性的说法就是采样越密集越好，这与我们平时的感觉也是一样的，相当于单位面积像素越多的显示器显示效果越好一样的道理。大于2倍最高频率称为奈奎斯特率。

## 混叠现象
先来看一下，不满足采样定理会出现什么情况，如果采样间隔很大（采样周期很大）将会产生混叠现象。
先看一个实际例子，一个圆盘以恒定速度旋转，旋转方向逆时针，转一圈的时间为T。
为了识别圆盘的位置，我们画出任意一条半径，来记录我们在不同时间间隔下拍下来的圆盘位置。拍摄到半径的旋转位置为信号，旋转方向为与上一次拍到图片夹角最小的方向。

*  情况一：我们拍照的间隔是T/8，也就是采样频率是旋转频率的8倍（由于旋转速度恒定，即信号频率恒定，所以最高频率就是恒定频率）。我们来看t0到t11的旋转情况，将每一时刻拆分出来：
![Center 1][]
在一幅图上表示：
![SouthEast][]
*  情况二：拍照间隔为(1/2)T的，采样频率是原始频率的2倍，我们来看t0到t11的旋转情况，将每一时刻拆分出来：
![SouthEast 1][]
合并到一张图像：
![SouthEast 2][]
现在已经分不清旋转的方向了，混叠现象出现，这也是为啥要大于原信号最高频率的两倍而不是大于等于原信号最高频率的两倍。

*  情况三：拍照间隔为(7/8)T的，采样频率是原始频率的（8/7）倍，我们来看t0到t11的旋转情况，将每一时刻拆分出来：
![Center 2][]
同样，放到一张图上的：
![SouthEast 3][]
这幅图我们已经完全把旋转方向感觉反了，这就是混叠现象。
上面的例子从常识上给出了采样定律的正确性，下面从数学的角度证明采样定理。
## 采样定理数学原理
利用冲击函数的采样特性，我们可以将一个连续时间信号与冲击串相乘得到一个有幅度变化的离散序列，这个过程就是一个采样的过程，冲击串的周期称为采样周期，就是采样定理里面提到的ws，得到下图：
![Center 3][]
在时域中：
![Center 4][]
![Center 5][]
因为冲击函数的取样特性![Center 6][]![Center 7][]带入上面式子，得到：
![Center 8][]
根据时域内乘法后的傅里叶变换等于频域卷积的性质，我们有：
![Center 9][]
式子中：
![Center 10][]
因为一个信号与冲击函数的卷积等于该信号的移位，![Center 11][]![Center 12][]
所以：
![Center 13][]
因为原信号是带限的，所以其频谱与冲击串卷积后将变成一个周期信号：
![Center 14][]
可以看到原信号与冲击串卷积后，其原始结构并未改变，而只是复制成了周期形式，这是采样定理最关键的地方，首先必须确定原信号的频谱的频率有限，第二是频谱和冲击串卷积后复制的频谱之间不能相互影响，为了满足这一点，就要求采样频率大于原信号最大频率的两倍，如果相互影响就会产生混叠现象：
![Center 15][]
为了恢复原始信号，我们只需要对离散信号的频谱进行低通滤波，理想情况下将得到完整的原始信号，条件是滤波截止频率在下图的红色位置，即wm和ws-wm之间：
![Center 16][]
即可恢复原始信号。
一个完整的采样到恢复的过程总结为以下：
![Center 17][]
## 混叠的数学原理
当采样频率小于信号最大频率的两倍时发生的现象，称之为混叠，混叠只对那些没有被采样到的点，从离散恢复回来后会有差异，但被采样的点不会变化， ![Center 18][] xr(t)为从离散序列中恢复出来的连续信号。
从频域的角度看混叠，我们以信号![Center 19][]为例：
![Center 20][] ![Center 21][]
![Center 22][] ![Center 23][]
![Center 24][] ![Center 25][]
![Center 26][]
![Center 27][]
![Center 28][]
可以看出当采样频率小于信号最大频率时出现混叠，当虚线和实线标志位置对换时这个结果导致恢复后信号的相位发生了倒置，称为相位倒置。
再看一个时域的例子：
![Center 29][]
![Center 30][]
采样间隔相同，频率高的信号采样后失真。
离散信号的采样定理与上述相同，因此不再赘述。
## 总结
至此对采样定理进行了简要介绍，其实前面介绍离散周期信号的傅里叶级数的时候，为什么离散周期信号傅里叶级数项是有限的，就是因为采样的原因，上图已经给出了原因。

[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114164442151.jpg
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114173513704.png
[SouthEast]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114173754213.png
[SouthEast 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114175446500.png
[SouthEast 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114175856875.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114174242671.png
[SouthEast 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114174927265.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114180748718.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114181225937.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114182517656.png
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114182704218.png
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114182807375.png
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114183020687.png
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114183128875.png
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114183208156.png
[Center 11]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114183401682.png
[Center 12]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114183422258.png
[Center 13]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114183513239.png
[Center 14]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114183821095.png
[Center 15]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114184150821.png
[Center 16]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114184450312.png
[Center 17]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114184735765.png
[Center 18]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114185233293.png
[Center 19]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114185501166.png
[Center 20]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114185601553.png
[Center 21]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114185530010.png
[Center 22]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114185700209.png
[Center 23]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114185623908.png
[Center 24]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114185754607.png
[Center 25]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114185732439.png
[Center 26]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114185846093.png
[Center 27]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114185957675.png
[Center 28]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114185940265.png
[Center 29]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114190313440.png
[Center 30]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理/20150114190323128.png





原文地址1：[https://www.face2ai.com/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理](https://www.face2ai.com/DIP-4-6-灰度图像-频域滤波-傅里叶变换之采样定理)转载请标明出处
