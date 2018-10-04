---
title: 【数字图像处理】6.5:灰度图像-图像分割 Prewitt算子
date: 2015-02-11 10:17
categories:
  - DIP
keywords:
  - Prewitt算子
  - 边缘检测
toc: true
---
**Abstract:** 数字图像处理：第43天
**Keywords:** Prewitt算子,边缘检测
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
废话开始，发现CSDN有了新的博客写作方式-MarkDown看起来很腻害的样子，有空试一下，希望以后能有更好的知识分享总结出来，当然要用更好的方式，更鲜明的表达出自己对知识的理解和观点，翻了下以前写的博客，感觉自己写博客的调理更清楚了，而且发现博客最好别写太长，当然，大牛除外，因为太长了可能有点驾驭不住。
今天来学习Prewitt算子，这个算子也是一阶微分算子，所以和前面说的Sobel有些类似，但不同的是平滑模板和不同情况下的效果。
## Prewitt算子
来看prewitt算子，这个算子形式简单，基本形式如下：
![Center][]
一排1减去另一排1，差分被它体现的淋漓尽致，当然我们观察它的性质还是要看分解形式，也就是前两个小模板，$[1，0，-1]$ 不用解释，差分的形式，为什么不用 $[1，-1]$ 进行差分？首先对于2x3的模板和3x3的模板，我们更倾向于3x3，因为3x3的模板中心落在实数上 ，其次$[1，0，-1]$的差分结果能够在一定程度上减少噪声影响。这个差分的性质，Sobel，Prewitt以及后面的Scharr都是一样的，所以这里并不是他们的差异，他们的差异主要集中在平滑算子上。Sobel算子的平滑算子是一个接近高斯的小模板，而Prewitt的平滑算子则是一个均值模板，也就是 $\frac{1}{3}[1，1，1]$ ，其原理与Sobel也保持一致，横向平滑，纵向差分产生Y方向的一阶微分，或者纵向平滑横向差分，产生X方向一阶微分，当然要注意按照这个模板做出的梯度方向是左手坐标系，也就是和图像坐标系一致的，即 $(0,0)$ 在左上角，x轴向右为正，y轴向下为正，为了使用习惯，可以把y轴取负，就能得到传统的右手坐标系了。。。
关于扩展，没有见过有人扩展prewitt，但是按照理论是绝对可行的，我猜想，可以扩展成
![Center 1][]

## 代码和结果

代码：
```c++
/////////////////////////////////////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////////////////////
double Prewitt(double *src,double *dst,int width,int height){
    double PrewittMask1[3]={1.0/3.0,1.0/3.0,1.0/3.0};
    double PrewittMask2[3]={-1.0,0.0,1.0};
    double *dst_x=(double *)malloc(sizeof(double)*width*height);
    double *dst_y=(double *)malloc(sizeof(double)*width*height);
    RealRelevant(src, dst_x, PrewittMask1, width, height, 1, 3);
    RealRelevant(dst_x, dst_x, PrewittMask2, width, height, 3, 1);

    RealRelevant(src, dst_y, PrewittMask2, width, height, 1, 3);
    RealRelevant(dst_y, dst_y, PrewittMask1, width, height, 3, 1);
    for(int j=0;j<height;j++)
        for(int i=0;i<width;i++){
            dst[j*width+i]=abs(dst_x[j*width+i])+abs(dst_y[j*width+i]);
        }
    free(dst_x);
    free(dst_y);
    return findMatrixMax(dst,width,height);

}
```

结果如下：
原图：
![Center 2][]
prewitt算子的处理结果：
![Center 3][]
局部放大：
![Center 4][] 
![Center 5][] 
![Center 6][] 
![Center 7][] 
![Center 8][] 
![Center 9][]
![Center 10][] 
![Center 11][] 
![Center 12][] 
![Center 13][]
在观察下阈值
![Center 14][]
![Center 15][]
![Center 16][]
![Center 17][]
## 结论
结论是prewitt会使灰度值相对集中，相比于Sobel并不会凸显出边界响应，整体边缘候选点区域接近，不适合做阈值后处理，但优点是速度快，计算简单。




[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211092423079.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211094303908.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095529339.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095610932.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095628965.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095634347.png
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095645764.png
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095646874.png
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095658696.png
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095658090.png
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095710053.png
[Center 11]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095716106.png
[Center 12]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095714611.png
[Center 13]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211095726792.png
[Center 14]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211101039011.png
[Center 15]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211101048823.png
[Center 16]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211101059119.png
[Center 17]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-5-灰度图像-图像分割-Prewitt算子/20150211101107980.png





原文地址1：[https://www.face2ai.com/DIP-6-5-灰度图像-图像分割-Prewitt算子](https://www.face2ai.com/DIP-6-5-灰度图像-图像分割-Prewitt算子)转载请标明出处
