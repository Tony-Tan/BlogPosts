---
title: 【数字图像处理】9.2:彩色图像-彩色变换 补色处理
date: 2015-03-16 14:20
categories:
  - DIP
tags:
  - 色彩变换
  - 补色
  - 色环
toc: true
---
**Abstract:** 数字图像处理：第69天
**Keywords:** 色彩变换,补色,色环
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
啊啊啊啊啊。。。办公室好乱。像菜市场那个一样，说好的做一个安静的美男子呢。
彩色图像处理中彩色变换是指
$g(x,y)=T[f(x,y)]$

其中$f$ , $g$为返回结果是向量的函数，T是$x,y$空间上对f的一个算子。
要顺便说下HSI在0°和360°处是个不连续的点，而且饱和度为0是色相未定义。
今天开始介绍真彩色图像，也包括多通道图像，补色处理就是根据色环，将现有颜色的色相反转180°，得到新的色相，这个变化的用途不知道，一个很一般的也很常见的应用是RGB带CMY的变化，不过，别的好像目前没听说过什么处理要把图像变成补色后才能处理。
#算法原理
算法原理很简单，找到补色，等位替换就好。
色环牛顿老爷子发明的，没有错，就是那个被苹果砸了的牛顿，不得不说，牛顿是位大师级的人物，色环如下：
![这里写图片描述](DIP-9-2-彩色图像-彩色变换-补色处理/20150316141306249)
这个色环可能不是牛顿老爷子的色环，但通过这个来展示下补色就是一条直径上两端的颜色。
对于RGB图像，补色是对应的CMY图像
对于HSI和HSV图像，H分量需要进行相应的旋转，而亮度分量也需要相应的反转，而饱和度不变，能够得到类似的补色效果。
## 代码实现
```c++
void Complementary_Color(C3 *src,C3 *dst,int width,int height,int color_space_type){
    switch (color_space_type) {
        case COLOR_SPACE_RGB:
            RGB2CMY(src, dst, width, height);
            break;
        case COLOR_SPACE_CMY:
            CMY2RGB(src, dst, width, height);
            break;
        case COLOR_SPACE_HSI:{
            for(int i=0;i<width*height;i++){
                double h=src[i].c1;
                if(h>=M_PI)
                    dst[i].c1=h-M_PI;
                else
                    dst[i].c1=M_PI+h;
                dst[i].c2=src[i].c2;
                dst[i].c3=255.-src[i].c3;
            }
            break;
        }
        case COLOR_SPACE_HSV:{
            for(int i=0;i<width*height;i++){
                double h=src[i].c1;
                if(h>=180.0)
                    dst[i].c1=h-180.0;
                else
                    dst[i].c1=180.0+h;
                dst[i].c2=src[i].c2;
                dst[i].c3=1.0-src[i].c3;
            }
            break;
        }
        default:
            break;
    }
}
```
## 结果观察
原图：
![这里写图片描述](DIP-9-2-彩色图像-彩色变换-补色处理/20150316141655788.png)

RGB：
![这里写图片描述](DIP-9-2-彩色图像-彩色变换-补色处理/20150316141713478.jpeg)

HSI：
![这里写图片描述](DIP-9-2-彩色图像-彩色变换-补色处理/20150316141731293.jpeg)

HSV：
![这里写图片描述](DIP-9-2-彩色图像-彩色变换-补色处理/20150316141750513.jpeg)

Paintbrush：
![这里写图片描述](DIP-9-2-彩色图像-彩色变换-补色处理/20150316141818795.jpeg)
## 总结
简单的介绍了最简单的色彩变换，补色变换，可以看出HSI，HSV和RGB处理的结果有些不同，而PaintBrush处理和我处理都不同，不知道它使用的什么算法。
待续。。。
