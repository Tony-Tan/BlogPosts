---
title: 【数字图像处理】9.1:彩色图像-伪彩处理 灰度图转伪彩色图像
date: 2015-03-16 12:05
categories:
  - DIP
tags:
  - 伪彩色图像
  - 伪彩色生成算法
toc: true
---
**Abstract:** 数字图像处理：第68天
**Keywords:** 伪彩色图像
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
观察了一下冈萨雷斯的书，发现彩色图像处理只用了一章进行介绍，原因分析了一下，后来发现，好像别的介绍的也不多，得出一个结论，冈萨雷斯这本书只能作为一部纲领性的介绍，它基本涵盖了图像处理的基础知识，但是如果想使用某种方向作为工作的话，需要继续找更多专业的书和开源项目来学习。还是像我之前抱怨的那样，每次看一本书之前都觉得自己看完了会变得超级厉害，但每次看完一本书以后反而会觉得自己像个傻瓜一样，需要更多的书来学习，如此循环，这一生都不会幸福了。盗图一张，与各位共勉
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316111545767)
## 原理
说到伪彩色图像，与其对应的是真彩色，下面介绍下其区别和性质。
我们知道能够观察出颜色的光的波长范围是有限的，只有那么一小段，换句话说也就是说有一大段光，只有一小段有颜色，其他都是灰度的，但人类视觉有一个特点就是，只能分辨出二十几种灰度，也就是说采集到的灰度图像分辨率超级高，有一千个灰度级，但很遗憾，人们只能看出二十几个，也就是说信息损失了五十倍，但人类视觉对彩色的分辨能力相当强，能够分辨出几千种色度。
在从采集的角度说下伪彩和真彩色，伪彩色原始图像是灰度图像
灰度图像的来源：

1. 单通道相机或其他传感器（比如CT用的平板）采集到的都是灰度图，这里包括使用单通道采集的频率高于可见光的频率的电磁波，可见光，低于可见光频率的电磁波。
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316114201245)
图中红色框内为不可见光，没有颜色，所以他们一定是灰度图，需要时要进行伪彩色处理。
2. 使用多通道采集设备采集的不可见光，这种图像有时候是单通道的，就是1中所说的，也有可能是多通道，不如卫星就有可能，红外，可见光，还有其他不可见光采集设备，这样的多通道灰度图有时候需要进行伪彩处理。

真彩色图的来源：
用多通道采集设备，多为相机来采集可见光，这样得到的是多通道真彩色图像。
#算法分析
对于单通道灰度图转换成伪彩图像的方法是将一种灰度映射为一种颜色，而映射方式不唯一，可以根据需要自行设定，下面的代码使用的算法是我自己想出来的，使用到了HSV色彩空间，并将其中的饱和度和亮度设为1.0，色相使用灰度0到255映射到0°到270°：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316115513133.jpeg)

## 代码
```c++
void Gray2Color(double *src,RGB* dst,int width,int height,int type){
    HSV* temp=(HSV*)malloc(sizeof(HSV)*width*height);
    for(int i=0;i<width*height;i++){
        double gray_value=src[i];
        if(type==HIGHVALUE_EQU_RED)
            temp[i].c1=HSVMAX-GRAY2HSV*gray_value;
        else if(type==LOWVALUE_EQU_RED)
            temp[i].c1=GRAY2HSV*gray_value;
        temp[i].c2=1.0;
        temp[i].c3=1.0;
    }
    HSV2RGB(temp, dst, width, height);
    free(temp);
}
```
#效果分析
灰度渐进图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316115620073.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316115504302.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316115642381.jpeg)

MacBook Pro x光扫描图
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316115728292.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316115619543.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316115703785.jpeg)

卫星地形图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316115741023.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316115913079.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316115930598.jpeg)

星云：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316120127409.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316120139343.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-9-1-彩色图像-伪彩处理-灰度图转伪彩色图像/20150316120025589.jpeg)

## 总结
伪彩色图乡相对灰度图像能够识别更多的细节，可分辨性较强。而且转换方式灵活，可以根据需要自行设计转换函数，或者自制映射表。
待续。。。
