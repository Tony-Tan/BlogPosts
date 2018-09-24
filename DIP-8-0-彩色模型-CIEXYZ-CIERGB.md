---
title: \[数字图像处理\]8.0:彩色模型，CIE XYZ，CIE RGB
date: 2015-03-11 14:03
categories:
  - DIP
keywords:
  - 颜色空间
  - CIE XYZ
  - CIE 1931
toc: true
---
**Abstract:** 数字图像处理：第60天
**Keywords:** CIE XYZ,CIE 1931
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
# 彩色模型，CIE XYZ，CIE RGB
今天学习彩色模型，常用的图像包括彩色图，灰度图，二值图，并且彩色图像->灰度图像->二值图像，为一个退化过程，即图像包含的信息逐步减少，根据冈萨雷斯第三版介绍的篇幅来看，对灰度图相关的算法描述较多，因此，可以说，就目前的算法来说，针对灰度图像的图像处理还是比较成熟的，但不能确定是否将来产生的新的理论能够统一现有所有算法，而且创造出更高级的算法。想要深入学习图像处理，个人认为应该对所有算法，所有模型有清晰的认识，了解其来源和完整过程，有助于理解后面的算法。

## 正文

颜色的根本来源人类对光的一种心理学感知，而光具有波粒二象性，我们只关心其波性质，原因是人类感官对不同波长的光的反应不同，继而从认知的层次产生了颜色，即光本身没有颜色这个属性，而是人们根据个人需要，把感受到的不同波长定义了不同的颜色。
如图，给出了物理学的对颜色的描述：

![Center][]

一句话总结，就是人类根据自身对光的感官，主观的定义了颜色。
色彩空间：指的是用一种客观的方式叙述颜色在人眼上的感觉，通常需要三色刺激值。更精确地说，首先先定义三种主要颜色（primary color），再利用颜色叠加模型，即可叙述各种颜色。需要注意的是，三种主要颜色未必需要是真正的颜色（也就是该种颜色无法真的被创造出来）。

## 生理因素
首先作为“视觉传感器”，人眼解剖结构表明，对颜色的感知，主要由视锥细胞完成，视锥细胞主要对三种波长敏感，即短（S, 420-440nm）、中（M, 530-540nm）和长（L, 560-580nm）三种波长敏感，视杆细胞支队光强敏感。

![Center 1][]

S在大概420纳米处出现峰值，M大概在534纳米处出现峰值，L大概在564处出现峰值，等分布波长谱产生白光，不等分布波长产生颜色。

## 条件等色（color metamer）现象
对于条件等色，可以这样解释：颜色由其光的波长（或频率）唯一定义，也就是，一种波不可能由其他波组合出来，因为不可能用多种波长合成一种波长，而人的感官细胞会产生一种错觉，即几种波的混合刺激等效于另一种波的单独刺激，也就是说你平时看到的颜色并不一定是真实的颜色。

## CIE-RGB色彩空间
因为上述现象的存在，前辈们设计了一个实验，首先选定三种颜色作为基础颜色，将其混合，通过调整其混合比例和强度（三种波长），来和另外的纯色（单一波长），当观察者感觉两种结构的效果相同时，记录下混合的比例，这样我们就可以用三种颜色来达成另外一种颜色，测试结构绘制如下：

![Center 2][]

这个函数图像有一个有趣的特点，r函数出现了负值，这个在我们的理论描述种是不可能出现的，因为不存在负光强，这里对其进行解释：对于光谱色较暗的段，通过三色叠加不能达到相应的效果，只能提高原色的亮度，才能得到匹配，所以还原为原色时，三色也等量下调，所以出现负波形。
由于其负波形的存在，所以上述模型不能再现实中完全再现所有颜色，也就是说，如果使用该色彩空间，有一段颜色是无法合成的，因为负波段无法产生。
CIE-RGB常被用rg色度来表示：
![Center 3][]
![Center 4][]
## 色彩空间
色彩空间指的是用一种客观的方式叙述颜色在人眼上的感觉，通常需要三色刺激值。更精确地说，首先先定义三种主要颜色（primary color），再利用颜色叠加模型，即可叙述各种颜色。需要注意的是，三种主要颜色未必需要是真正的颜色（也就是该种颜色无法真的被创造出来）。
## Grsassmann定律
由格拉斯曼（Grsassmann）总结的在颜色相加混合时的规律，概括为以下四点：

1. 人的视觉只能分辨颜色的三种变化：亮度、色调、饱和度；
2. 两种颜色混合时的补色律和中间色定律；
3. 感觉上相似的颜色，可以互相代替—代替律；
4. 亮度相加定律：由几个颜色组成的混合色的亮度，是各颜色光亮度的总和。

## 明度与色度
明度：眼睛对光源和物体表面的明暗程度的感觉，主要是由光线强弱决定的一种视觉经验。
色度：色度是不包括亮度在内的颜色的性质，它反映的是颜色的色调和饱和度。

## CIE-XYZ
由于上述RGB空间部分颜色无法再现的原因，前辈们改变方法。
假定Grassmann定律成立，这个新空间通过线性变换而有关于CIE-RGB空间。新空间将以三个新颜色匹配函数来定义：![\\overline\{x\}(\\lambda)][overline_x_lambda]、![\\overline\{y\}(\\lambda)][overline_y_lambda]和![\\overline\{z\}(\\lambda)][overline_z_lambda]。带有频谱功率分布I(λ)的颜色的对应的XYZ三色刺激值为给出为：
![Center 5][]
![Center 6][]
![Center 7][]
值得注意的是，XYZ并不是红绿蓝三种颜色的对应，而是通过这三种颜色导出的一种参数，其颜色并无太大意义，而是具有较高的数学意义。
CIE XYZ色彩空间故意设计得Y参数是颜色的明度或亮度的测量。颜色的色度接着通过两个导出参数x和y来指定，它们是所有三个三色刺激值X、Y和Z的函数所规范化的三个值中的两个：
XYZ匹配函数如下：
![Center 8][]    
XYZ色度图效果如下：
![Center 9][]
与CIE-RGB色度图相叠加，如下图：
![Center 10][]
上图的解释：在CIE rg色度图中展示规定CIE XYZ色彩空间的三角形构造。三角形Cb-Cg-Cr就是在CIE xy色度空间中的xy=(0,0),(0,1),(1,0)三角形。连接Cb和Cr的直线是alychne。注意光谱轨迹通过rg=(0,0)于435.8 nm，通过rg=(0,1)于546.1 nm，通过rg=(1,0)于700 nm。还有，均等能量点（E）位于rg=xy=(1/3,1/3)。
CIE—XYZ色度的性质：
*  色度图展示了对一般人可见的所有色度。这个用颜色展示的区域叫做人类视觉的色域。在CIE绘图上所有可见色度的色域是用颜色展示的马蹄铁形状。色域的曲线边界叫做“光谱轨迹”并对应于单色光，波长用纳米标记。色域底下的直线边界叫做“紫线”，这些颜色尽管在色域的边界上，但没有匹配的单色光。更少饱和的颜色位于图形内部而白色位于中央。
*  所有可见色度对应于x、y和z的非负值（因此对应于X、Y和Z的非负值）。
*  如果你在色度图上选择了任何两点，则位于这两点之间直线上任何颜色都可以用这两个颜色混合出来。这得出了色域的形状必定是凸形的。混合三个光源形成的所有颜色都可以在色度图内的源点形成的三角形内找到（对于多个光源也如是）。
*  两个同等明亮颜色的等量混合一般不位于这个线段的中点。用更一般术语说，在xy色度图上距离不对应于两种颜色之间的差别程度。设计了其他色彩空间（特别是CIELuv和CIELab）来满足这个问题。
*  给定三个真实光源，这些光源不能覆盖人类视觉的色域。几何上说，在色域中没有三个点可以形成包括整个色域的三角形，更简单的说，人类视觉的色域不是三角形。
*  平直能量频谱的光对应于点 (x,y) = (1/3,1/3)。

至此我们简单的介绍了下CIE—XYZ空间的来源和相关知识，后续将介绍其他色彩空间知识，但出于知识的路线原因决定，下篇开始介绍二值图像的相关操作，然后介绍灰度图像相关的工作，最后介绍彩色图像时，再介绍其他色彩空间极其应用。欢迎收看。


[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/20141220123439812.jpg
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/20141222112304172.jpg
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/20141222114140775.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/20141222140240997.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/20141222140249625.png
[overline_x_lambda]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/557e8d4cdd91b4461411088939ab1a41.png
[overline_y_lambda]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/bee0b0d1dea7e1937ec3710c7c435f3e.png
[overline_z_lambda]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/5aa5f099f72042122adaa7b313c37045.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/20141222140916895.png
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/20141222140922562.png
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/20141222140935911.png
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/20141222141529859.png
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/20141222141337649.png
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-0-彩色模型-CIEXYZ-CIERGB/20141222141141897.png





原文地址1：[https://www.face2ai.com/DIP-8-0-彩色模型-CIEXYZ-CIERGB](https://www.face2ai.com/DIP-8-0-彩色模型-CIEXYZ-CIERGB)转载请标明出处
