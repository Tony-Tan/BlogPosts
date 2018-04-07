---
title: 【数字图像处理】8.4:彩色图像-色彩空间 YIQ 、YUV 、YCbCr 、YC1C2 和I1I2I3
date: 2015-03-13 15:29
categories:
  - DIP
tags:
  - 色彩空间
  - YIQ
  - YUV
  - YCbCr
toc: true
---
**Abstract:** 数字图像处理：第64天
**Keywords:** 色彩空间,YIQ,YUV,YCbCr
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
背后有人，今天不说废话。。。。
## YIQ
NTSC电视系统指定的色彩空间，为了压缩RGB的传输带宽提高传输速度。
YIQ中Y代表亮度信号（Luminance），IQ作为附加信息，I表示相位（In-phase）色彩从橙色到青色，Q表示正交（Quadrature-phase）色彩从紫色到黄绿色，故YIQ也叫做色差信号（Chrominance signal）。
YIQ中Y信号可以直接用于黑白电视作为灰度信号，进行显示，彩色电视则加入IQ分量与Y一起产生颜色。
转换公式：
![这里写图片描述](DIP-8-4-彩色图像-色彩空间-YIQ-YUVYCbCr-YC1C2和I1I2I3/20150313143318884.png)
![这里写图片描述](DIP-8-4-彩色图像-色彩空间-YIQ-YUVYCbCr-YC1C2和I1I2I3/20150313143634049.png)

## YUV
YUV在德国和法国发展的彩色电视系统PAL和SECAM中使用。
Y分量与YIQ中Y相同。
以下来自wiki：
>首先YUV是一类信号的总称，$Y'UV, YUV, YCbCr，YPbPr$等专有名词都可以称为$YUV$，“Y”表示明亮度（Luminance、Luma），“U”和“V”则是色度、浓度（Chrominance、Chroma），$Y'UV, YUV, YCbCr, YPbPr$常常有些混用的情况，其中YUV和Y'UV通常用来描述模拟信号，而相反的YCbCr与YPbPr则是用来描述数位的影像信号，例如在一些压缩格式内MPEG、JPEG中，但在现今，YUV通常已经在电脑系统上广泛使用。YUV Formats分成两个格式：
1. 紧缩格式（packed formats）：将Y、U、V值储存成Macro Pixels阵列，和RGB的存放方式类似。
2. 平面格式（planar formats）：将Y、U、V的三个分量分别存放在不同的矩阵中。

![这里写图片描述](DIP-8-4-彩色图像-色彩空间-YIQ-YUVYCbCr-YC1C2和I1I2I3/20150313144329727.png)
![这里写图片描述](DIP-8-4-彩色图像-色彩空间-YIQ-YUVYCbCr-YC1C2和I1I2I3/20150313144316280.png)

YIQ和YUV非常适合压缩，因为亮度和色度可以使用不同的比特来编码。
文献中U表示红-蓝色差，V对应绿-品红色差。
YUV可用于彩色图像的高光分析。
## YCbCr
YCbCr不是一种绝对色彩空间，是YUV压缩和偏移的版本。
>YCbCr或Y'CbCr有的时候会被写作：YCBCR或是Y'CBCR，是色彩空间的一种，通常会用于影片中的影像连续处理，或是数字摄影系统中。Y'为颜色的亮度（luma）成分、而CB和CR则为蓝色和红色的浓度偏移量成份。Y'和Y是不同的，而Y就是所谓的流明（luminance），表示光的浓度且为非线性，使用伽马修正（gamma correction）编码处理。
YCbCr的Y与YUV中的Y含义一致，Cb和Cr与UV同样都指色彩，Cb指蓝色色度，Cr指红色色度，在应用上很广泛，JPEG、MPEG、DVD、摄影机、数字电视等皆采此一格式。因此一般俗称的YUV大多是指YCbCr。
YCbCr格式有：
4∶4∶4 ,
4∶2∶2 ,
4∶1∶1 ,
4∶2∶0 .
四种压缩比.

YCbCr 4:4:4表示Y，Cb，Cr占同样的比特位
下面的四个像素为:
$ [Y0 U0 V0] [Y1 U1 V1] [Y2 U2 V2] [Y3 U3 V3]$

存放的码流为:
$Y0 U0 V0 Y1 U1 V1 Y2 U2 V2 Y3 U3 V3$


----------


YCbCr 4:2:2表示Cb，Cr占Y比特位的一半
下面的四个像素为:
$[Y0 U0 V0] [Y1 U1 V1] [Y2 U2 V2] [Y3 U3 V3]$

存放的码流为:
$Y0V0Y1U1Y2V2Y3U3 $

解析后为：
$[Y0 U0 V1] [Y1 U0 V1] [Y2 U2 V3] [Y3 U2 V3] $

----------

YCbCr 4:1:1表示Cb，Cr占Y比特位的$\frac{1}{4}$每个由4个水平方向相邻的像素组成的宏像素需要占用6字节内存。
下面的四个像素为:
$ [Y0 U0 V0] [Y1 U1 V1] [Y2 U2 V2] [Y3 U3 V3]$

存放的码流为:
$Y0U0Y1Y2V2Y3 $

解析后为：
$[Y0 U0 V2] [Y1 U0 V2] [Y2 U0 V2][Y3 U0 V2] $

----------

YCbCr 4:2:0 并不是没有Cr分量，而是采取上下左右分别抽样的方式：

 - 如果一行是4:2:0的话，下一行就是4:0:2，再下一行是4:2:0...以此类推。对每个色度分量来说，水平方向和竖直方向的抽样率都是2:1，所以可以说色度的抽样率是4:1。

对非压缩的8比特量化的视频来说，每个由2x2个2行2列相邻的像素组成的宏像素需要占用6字节内存。
下面八个像素为：
   $$[Y0 U0 V0] [Y1 U1 V1] [Y2 U2 V2] [Y3 U3 V3]\\
  [Y5 U5 V5] [Y6 U6 V6] [Y7U7 V7] [Y8 U8 V8]$$
存放的码流为：
   $$Y0 U0 Y1 Y2 U2 Y3\\
 Y5 V5 Y6 Y7 V7 Y8$$

解析后：
 $$[Y0 U0 V5] [Y1 U0 V5] [Y2 U2 V7] [Y3 U2 V7]\\
   [Y5 U0 V5] [Y6 U0 V5] [Y7U2 V7] [Y8 U2 V7]$$

![这里写图片描述](DIP-8-4-彩色图像-色彩空间-YIQ-YUVYCbCr-YC1C2和I1I2I3/20150313151447503.png)

## YC1C2
YC1C2与YCbCr相近，知识YC1C2更接近胶片色阶，而YCbCr更接近荧光粉的色阶。
## I1I2I3
此空间描述了一个特征模型而不是色彩空间I1I2I3可以对图像获得较好的分割结果。
其中I1I2I3如下：
$I_1=\frac{R+G+B}{3}$
$I_2=\frac{R-B}{2}$
$I_3=\frac{2G-R-B}{4}$
## 总结
简单介绍几个色彩空间，作为彩色图像的基础，色彩空间相对概念性较强，在后面处理中，如需更深入的知识会在后续介绍。
待续。。。
