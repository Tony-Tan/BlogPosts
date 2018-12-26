---
title: 【数字图像处理】5.2:灰度图像-图像增强 平滑之均值滤波、高斯滤波
date: 2015-01-28 12:09
categories:
  - DIP
tags:
  - 平滑
  - 均值滤波
  - 高斯平滑
toc: true
---
**Abstract:** 数字图像处理：第28天
**Keywords:** 平滑，均值滤波，高斯平滑
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>

## 开篇废话
今天的废话是，我早上来了开始写博客，写了大概和下文差不多的内容，写了很多，就在发表以后，我震惊了，博客是空的，没有内容，我就表示呵呵了，不知道是网速的问题还是CSDN的问题，总之，我就不说啥了。
今天的内容是是平滑，先介绍均值和高斯平滑，高斯平滑是均值平滑的一个扩展，或者是一个进化版本。均值的原理是，一个规定的邻域内，所有像素的平局值作为最终计算的结果，每个像素的权值相同，为总像素的倒数，而高斯平滑是上述的升级版本，邻域内每个像素的权值不同，而权值是由高斯函数确定的。
均值平滑和高斯平滑都是线性的，也就是，一旦参数给定，模板就确定下来，不会因为位置和像素分布不同而改变，而线性模板的基本运算是卷积。
线性模板的另一个性质就是可以进行频域分析，比如高斯平滑的频域仍然是高斯的，而且是低通的，这与前面讲到的平滑是消除尖锐的噪声（高频）的操作相互证明了其正确性，均值平滑是一个盒状滤波器，其频域为sinc函数，也是一个近似的低通，但sinc函数有旁瓣，所以，模板宽度选择不好可能会有一些不好的效果，比如有些高频会被保留，这一点也比较好理解。
比较下两种均值（加权和不加权）。比如一维的一个序列 $\{0，0，0，0，0，1000，0，0，0，0\}$ ，明显1000是个边缘，如果使用3个宽度的均值平滑，结果是 $\{0，0，0，0，333，333，333，0，0，0\}$ ，边缘被完全模糊掉了。但如果使用 $\{1，2，1\}$ 的近似高斯平滑模板，结果是 $\{0，0，0，0，250，500，250，0，0，0\}$ ，边缘被保留。所以，加权平均（高斯）可以保留一定的细节。
对于设计的线型滤波器，其效果可以先是由傅里叶变换，到频域进行观察，便可大致推测出其效果，测试图片（灰度输入）：
![Center][]
## 均值滤波
### 数学
基本数学原理公式，以3x3为例：
![Center 1][]
得到滤波模板：
![Center 2][]
应用于图像的计算公式：
![Center 3][]
上式中的w(s,t)恒等于1。
### 效果
观察下用3x3，5x5和7x7的均值模板与图像卷积的结果：
3x3模板：
![Center 4][]
5x5模板：
![Center 5][]
7x7模板：
![Center 6][]
可以得出结论，均值滤波的模板越大，图像越模糊。
### 代码
```c++
    void MeanMask(double *mask,int width,int height){
        double w=width;
        double h=height;
        double meanvalue=1.0/(w*h);
        for(int i=0;i<width*height;i++)
            mask[i]=meanvalue;


    }
    void MeanFilter(IplImage *src,IplImage *dst,int width,int height){
        double * pixarry=(double *)malloc(sizeof(double)*src->width*src->height);
        double * dstarry=(double *)malloc(sizeof(double)*src->width*src->height);
        double * mask=(double *)malloc(sizeof(double)*width*height);
        for(int j=0;j<src->height;j++){
            for(int i=0;i<src->width;i++){
                pixarry[j*src->width+i]=cvGetReal2D(src, j, i);
            }
        }
        MeanMask(mask, width, height);
        RealRelevant(pixarry,dstarry,mask,src->width,src->height,width,height);
        for(int j=0;j<src->height;j++){
            for(int i=0;i<src->width;i++){
                cvSetReal2D( dst,j,i,dstarry[j*src->width+i]);
            }
        }
        free(pixarry);
        free(dstarry);
        free(mask);
    }
```
这个代码并不是最快速的，只是为了和高斯平滑相互保持一致。所以，如果要使用，需要先优化。

## 高斯滤波
高斯平滑，就是一种加权的均值，权值由高斯函数确定。

### 数学
高斯函数：
![Center 7][]
在三维中的形状如下：
![Center 8][]
生成上图的matlab代码：
```matlab
    % 公式： p(z) = exp(-(z-u)^2/(2*d^2)/(sqrt(2*pi)*d)
    X = 0 : 1 : 100;
    Y = 0 : 1: 100;

    % 方差
    d= 49;
    Z = zeros(101, 101);
    for row = 1 : 1 : 101
        for col = 1 : 1 : 101
            Z(row, col) = (X(row) - 50) .* (X(row)-50) + (Y(col) - 50) .* (Y(col) - 50);
        end
    end
    Z = -Z/(2*d);
    Z = exp(Z) / (sqrt(2*pi) * sqrt(d));
    surf(X, Y, Z);
```
当坐标（u，v）远离原点时，函数值越小。这与概率的知识相符，越远离中心（原点）的像素灰度值，对中心像素灰度值的相关性越小，所以被赋予的权值越小。
### 效果
观察效果，使用不同标准差，和模板大小的结果：
![Center 9][]
![Center 10][]
![Center 11][]
![Center 12][]
![Center 13][]
![Center 14][]
![Center 15][]
![Center 16][]
![Center 17][]
结论：
 *  同等模板大小，标准差越大越模糊
 *  标准差相同，模板越大图像越模糊。

### 代码
```c++
  void GaussianMask(double *mask,int width,int height,double deta){
      double deta_2=deta*deta;
      int center_x=width/2;
      int center_y=height/2;
      double param=1.0/(2*M_PI*deta_2);
      for(int i=0;i<height;i++)
          for(int j=0;j<width;j++){
              double distance=Distance(j, i, center_x, center_y);
              mask[i*width+j]=param*exp(-(distance*distance)/(2*deta_2));

      }
      double sum=0.0;
      for(int i=0;i<width*height;i++)
          sum+=mask[i];
      for(int i=0;i<width*height;i++)
          mask[i]/=sum;
  }


  void GaussianFilter(IplImage *src,IplImage *dst,int width,int height,double deta){
      double * pixarry=(double *)malloc(sizeof(double)*src->width*src->height);
      double * dstarry=(double *)malloc(sizeof(double)*src->width*src->height);
      double * mask=(double *)malloc(sizeof(double)*width*height);
      for(int j=0;j<src->height;j++){
          for(int i=0;i<src->width;i++){
              pixarry[j*src->width+i]=cvGetReal2D(src, j, i);
          }
      }
      GaussianMask(mask, width, height, deta);
      RealRelevant(pixarry,dstarry,mask,src->width,src->height,width,height);
      for(int j=0;j<src->height;j++){
          for(int i=0;i<src->width;i++){
              cvSetReal2D( dst,j,i,dstarry[j*src->width+i]);
          }
      }
      free(pixarry);
      free(dstarry);
      free(mask);
  }
```
## 总结
之前写了很多结论，现在都忘完了，因为理论很简单，主要是观察结果，线性平滑基本就介绍这些，下篇介绍非线性的。

[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128120639281.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128114711343.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128114720017.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128114728020.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128115026655.jpg
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128115035438.jpg
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128115043332.jpg
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128115602134.png
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128115553593.png
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128120049175.png
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128120107411.png
[Center 11]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128120158220.png
[Center 12]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128120215755.png
[Center 13]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128120150968.png
[Center 14]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128120235426.png
[Center 15]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128120254880.png
[Center 16]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128120309809.png
[Center 17]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波/20150128120322554.png





原文地址1：[https://www.face2ai.com/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波](https://www.face2ai.com/DIP-5-2-灰度图像-图像增强-平滑之均值滤波-高斯滤波)转载请标明出处
