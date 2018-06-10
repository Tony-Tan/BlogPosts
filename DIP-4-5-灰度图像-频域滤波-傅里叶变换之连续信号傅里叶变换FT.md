---
title: 【数字图像处理】4.5:灰度图像-频域滤波 傅里叶变换之连续信号傅里叶变换（FT）
date: 2015-01-13 15:41
categories:
  - DIP
tags:
  - 连续信号傅里叶变换
toc: true
---
**Abstract:** 数字图像处理：第18天
**Keywords:** 连续信号傅里叶变换
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>

## 开篇废话
这两天博客写的有点多，感觉博客应该是知识的总结和理解，而是单纯的为了写博客而写博客，不过这两天的内容连续性很强，所以一口气下来也未尝不是好事，这两三篇文章一直在研究理论，已经好久没写代码了，哈哈，后面准备写下DFT和采样相关的，然后就回归冈萨雷斯，看图像比看数学有意思多了。
看奥本海姆的《信号与系统》发现看到最后竟然没有DFT，因为在图像处理里面用到的主要是DFT，有点郁闷，不过感觉傅里叶家族的东西基本相通，一个能推出另一个，另一个的特化就是前一个，等等，希望把FS（傅里叶级数），FT（傅里叶变换），DTFT（离散时间傅里叶变换）看完以后能触类旁通，学明白图像频域滤波的基础知识。
本篇文章介绍FT-傅里叶变换，DTFT-离散时间傅里叶变换将在后面介绍，下篇将介绍采样定理，原因，采样定理是FT的直接产物，DTFT在图像中应用不大，但作为DFT的近亲，还是要简单介绍下。

## 傅里叶变换
我们平时所说的傅里叶变换实际上说的就是FT，其针对的原始信号是连续的信号，包括周期与非周期的（因为其涉及到收敛问题，所以连续信号的傅里叶变换是否存在要使用判断条件的），当然周期性的连续信号变换结果与傅里叶级数的变换结果是一致的。

## 从连续周期级数推导FT
下面我们来推导下傅里叶变换，看下面一个周期方波信号，宽2T1，周期为T：
![Center][]
图像如下
![Center 1][]
根据前面的知识，其存在傅里叶级数，且该信号的级数系数为：
![Center 2][]
由于并未给出T1和T的具体值，这里我们设T=4\*T1，其图像是：
![Center 3][]
黑色包络线并非级数结果，因为结果是离散的，是红色点标出的值，红色点的纵坐标就是级数的系数值，横坐标为对应的频率。
可以看出，决定形状的变量有k，T1，和T，我们对ak的表达式做简单的变形，将w0与k合体，因为w0为基波频率，其根据周期T唯一确定，如下： 
![Center 4][]
这样一来，如果式子右边以w为变量，且w连续，那么就是一种很常见的波形了，我们暂且称之为SA函数波形，也就是上图外面的包络波形，如果w是离散的，也就是求级数的情况，相当于对包络函数的等间隔取样。
接下来我们设定T为T1的整数倍，而w0由T唯一确定，而且，最重要的是，我们发现上面式子中，右侧公式的值不受T的影响（虽然w0受到T影响，但可以用k来抵消掉，而使w保持不变 ）所以我们来调整T的大小，随之T的增大，w0不断变小，而包络线不变，这样当T逐渐增大，w0=2\*pi/T逐渐变小当如下图：
当T逐渐增大的时候
![Center 5][]
T进一步增加
![Center 6][]
我们来看级数系数在对应的w0的变化在SA函数取样
当T=8\*T1的时候：
![Center 7][]
当T=16\*T1的时候：
![Center 8][]
当T趋近于无穷大，级数系数取样间隔变得无穷小，周期方波变成只有一个方波的非周期绝对可积信号。
以上通过扩大一个周期信号的周期，给出一个周期无穷大的绝对可积的信号的“傅里叶级数”形式，当我们原始信号为一个周期无穷大，或者是非周期的绝对可积信号，我们将使用相同的思想，先将原始信号按照一定的周期复制成周期信号，然后求解周期信号的傅里叶级数，然后将周期扩展到无穷大：
![Center 9][]
上面图中![Center 10][]是x(t)周期复制的结果，![Center 10][]的傅里叶级数为：
![Center 11][]
分析公式为：
![Center 12][]
由于x(t)在-T/2到T/2内与![Center 10][]相同，在区间-T/2到T/2外，x(t)为0，所以将![Center 10][]换成x(t)：
![Center 13][]
定义Tak的包络函数
![Center 14][]
所以，系数ak可以写为：
![Center 15][]
把ak带回来式子![Center 11][]中得到
![Center 16][]
因为![Center 17][]，改写为：
![Center 18][]
当T趋近于无穷大的时候，w0趋近于无穷小上式子变成积分形式，上式求和内容为一个矩形面积，当w0趋近于无穷小的时候，![Center 10][]收敛于x（t）。
![Center 19][]
至此，我们得出傅里叶完整公式对：
![Center 20][]
因为傅里叶变换是无限的“级数”，所以存在收敛问题：
 *  条件1：信号绝对可积
 *  ![Center 21][]
 *  条件2：任何区间内，x（t）具有有限个最大值与最小值
 *  条件3：任何区间内，x（t）具有有限个不连续点，并且每个不连续点的值是有限值。
对于周期函数的傅里叶变换，其结果与傅里叶级数相同，我们可以理解为其傅里叶变换的一串冲击函数。
傅里叶变换的性质：
![Center 22][]
至此，对连续函数的傅里叶变换进行了简要的推导，由于其在图像处理里面很少用到，但有可能在模式识别中有用到，所进行简要的介绍。

[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113112315031.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113111957137.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113112445300.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113113122384.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113113855705.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113135106375.png
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113135547887.png
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113140705864.png
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113140722946.png
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113143512945.png
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113143818476.png
[Center 11]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113145642833.png
[Center 12]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113145657450.png
[Center 13]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113150606914.png
[Center 14]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113150654125.png
[Center 15]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113150811090.png
[Center 16]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113151036186.png
[Center 17]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113151038437.png
[Center 18]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113151131504.png
[Center 19]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113151757293.png
[Center 20]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113151634437.png
[Center 21]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113152616587.png
[Center 22]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-5-灰度图像-频域滤波-傅里叶变换之连续信号傅里叶变换FT/20150113153252421.png
