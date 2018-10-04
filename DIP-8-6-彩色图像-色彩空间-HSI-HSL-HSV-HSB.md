---
title: 【数字图像处理】8.6:彩色图像-色彩空间 HSI(HSL)、HSV(HSB)
date: 2015-03-15 14:57
categories:
  - DIP
keywords:
  - 色彩空间
  - HSI
  - HSV
toc: true
---
**Abstract:** 数字图像处理：第66天
**Keywords:** HSI,HSV
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
# 彩色图像-色彩空间 HSI(HSL)、HSV(HSB)
色彩空间介绍最后两种与人类感知相对较接近的两种空间，彩色图像处理后面的内容大部分用在图像的基础处理，而如果想学习图像分析，应该开始学习一些模式识别和机器学习的算法，打算最近开始学习一些，并且开一个新的博客主题，欢迎讨论。。。
今天介绍下两种相对较接近色彩定义的，人的视觉只能分辨颜色的三种变化：亮度、色调、饱和度，HSI和HSV即表示其相对的变量，其中H（Hue）表示色调，S（Saturation）表示饱和度，I/V（Intensity/Value）表示亮度/明度。HSI和HSV作为相对颜色，但在彩色图像处理中使用广泛，下面来逐一介绍这两种空间，和其与RGB空间的转换。
## HSI色彩空间
色调H(Hue)： 与光波的波长有关，它表示人的感官对不同颜色的感受，如红色、绿色、蓝色等，它也可表示一定范围的颜色，如暖色、冷色等。
饱和度S(Saturation)： 表示颜色的纯度，纯光谱色是完全饱和的，加入白光会稀释饱和度。饱和度越大，颜色看起来就会越鲜艳，反之亦然。
亮度I(Intensity)： 对应成像亮度和图像灰度，是颜色的明亮程度。
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-6-彩色图像-色彩空间-HSI-HSL-HSV-HSB/20150315140859738.png)

RGB和HSI的转换几种转换公式：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-6-彩色图像-色彩空间-HSI-HSL-HSV-HSB/20150315143324202.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-6-彩色图像-色彩空间-HSI-HSL-HSV-HSB/20150315143409729.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-6-彩色图像-色彩空间-HSI-HSL-HSV-HSB/20150315143421850.png)


----------


## HSV色彩空间
HSV模型通常用于计算机图形应用中。在用户必须选择一个颜色应用于特定图形元素各种应用环境中，经常使用HSV 色轮。在其中，色相表示为圆环；可以使用一个独立的三角形来表示饱和度和明度。典型的，这个三角形的垂直轴指示饱和度，而水平轴表示明度。在这种方式下，选择颜色可以首先在圆环中选择色相，在从三角形中选择想要的饱和度和明度。
HSV模型的另一种可视方法是圆锥体。在这种表示中，色相被表示为绕圆锥中心轴的角度，饱和度被表示为从圆锥的横截面的圆心到这个点的距离，明度被表示为从圆锥的横截面的圆心到顶点的距离。某些表示使用了六棱锥体。这种方法更适合在一个单一物体中展示这个HSV色彩空间；但是由于它的三维本质，它不适合在二维计算机界面中选择颜色。
HSV色彩空间还可以表示为类似于上述圆锥体的圆柱体，色相沿着圆柱体的外圆周变化，饱和度沿着从横截面的圆心的距离变化，明度沿着横截面到底面和顶面的距离而变化。这种表示可能被认为是HSV色彩空间的更精确的数学模型；但是在实际中可区分出的饱和度和色相的级别数目随着明度接近黑色而减少。此外计算机典型的用有限精度范围来存储RGB值；这约束了精度，再加上人类颜色感知的限制，使圆锥体表示在多数情况下更实用。
转换公式：
RGB->HSV
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-6-彩色图像-色彩空间-HSI-HSL-HSV-HSB/20150315145318868.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-6-彩色图像-色彩空间-HSI-HSL-HSV-HSB/20150315145332783.png)

HSV->RGB
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-6-彩色图像-色彩空间-HSI-HSL-HSV-HSB/20150315145343757.png)

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-6-彩色图像-色彩空间-HSI-HSL-HSV-HSB/20150315145428311.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-6-彩色图像-色彩空间-HSI-HSL-HSV-HSB/20150315145440229.png)
## 总结
今天主要介绍HSx模型，这类色彩空间与色彩定义对应，在后面的彩色图像处理中将大量用到。
待续。。。





原文地址1：[https://www.face2ai.com/DIP-8-6-彩色图像-色彩空间-HSI-HSL-HSV-HSB](https://www.face2ai.com/DIP-8-6-彩色图像-色彩空间-HSI-HSL-HSV-HSB)转载请标明出处
