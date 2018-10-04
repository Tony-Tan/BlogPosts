---
title: 【数字图像处理】4.7:灰度图像-频域滤波 傅里叶变换之离散时间傅里叶变换(DTFT)
date: 2015-01-15 20:38
categories:
  - DIP
keywords:
  - 离散时间傅里叶变换
toc: true
---
**Abstract:** 数字图像处理：第20天
**Keywords:** 离散时间傅里叶变换
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>



## 开篇废话
本来是不想写DTFT的，原因1，与前面傅里叶变换（FT）推导过程相似，原因2，在图像处理中DTFT应用不是很广泛，但后来想想还是写出来，原因1，不写出来我觉得心里不踏实，原因2，DTFT是DFT的近亲，不写的话家族不完整，下一篇写DFT，其实写到这个阶段，要写的东西就少了许多，因为很多都是引用前面的结论和一些性质。但还是写出来吧，为了心里踏实。
忘了哪位中国上一辈的科学家说过，“搞科研不能糊弄，你糊弄它，它就糊弄你。”我这算不上科研，但感觉还是学踏实点心里有底，不至于以后哪里出了问题总是会怀疑自己知识基础有问题。还有，希望我们当代的科研工作者能好好搞研究，都不好好教学了还搞不好研究，那就真是一群废物了。

## 从离散周期信号的傅里叶级数推导离散时间傅里叶变换

    一个某一个有限序列 $x[n]$ ,其在某一个阶段 $N(N_1<=n<=N_2)$ 内不为0，其外部，全部为0，用这个信号构造一个周期信号 $x'[n],x[n]$ 是$x'[n]$ 的一个周期。
![SouthEast][]
$x'[n]$ 是周期信号，所以其有傅里叶级数，并且其傅里叶级数是周期的：

![SouthEast 1][]
在 $N$ 内 $x'[n]=x[n]$ ，替换求和内容为：

![SouthEast 2][]
在N外 $x[n]=0$ ；所以定义函数：
![SouthEast 3][]
所以系数$a_k$ 正比于 $X(e^{jw})$ ：
![SouthEast 4][]
其中 $w0=\frac{2\pi}{N}$ ，上面式子和第一个式子结合起来就有：
![SouthEast 5][]
因为 $w_0*N=2*\pi$ ，所以有：
![SouthEast 6][]
随着 $N$ 的增加，$w_0$不断减少，当 $N$ 趋近于无穷大的时候， $w_0$ 趋近于无穷小，$x'[n]=x[n]$  ，此时，上式变成一个积分式，积分变量为w0，因为 $w_0=\frac{2*pi}{N}$ ，所以积分区间为 $2\pi$ ，就有：
![SouthEast 7][]
因为 $X(e^jw)e^{jw}$ 的周期是 $2\pi$ ，所以积分区间可以去任何长度的 $2\pi$ 区间，得出以下变换对：
![SouthEast 8][]
这就是离散时间傅里叶变换对，同样，转换到频域的叫分析公式，转换到时域的叫综合公式。

## 性质
![SouthEast 9][]
![SouthEast 10][]
## 总结
至此，傅里叶家的四种主要变换已经全部推导了以下，下一篇写下DFT是什么，然后介绍几个常见问题，并给出傅里叶家谱和之间的相互关系。


[SouthEast]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT/20150115200005247.png
[SouthEast 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT/20150115200344100.png
[SouthEast 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT/20150115200543112.png
[SouthEast 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT/20150115200802093.png
[SouthEast 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT/20150115201238984.png
[SouthEast 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT/20150115201805315.png
[SouthEast 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT/20150115201729781.png
[SouthEast 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT/20150115202637437.png
[SouthEast 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT/20150115203147156.png
[SouthEast 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT/20150115203533699.png
[SouthEast 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT/20150115203526093.png





原文地址1：[https://www.face2ai.com/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT](https://www.face2ai.com/DIP-4-7-灰度图像-频域滤波-傅里叶变换之离散时间傅里叶变换DTFT)转载请标明出处
