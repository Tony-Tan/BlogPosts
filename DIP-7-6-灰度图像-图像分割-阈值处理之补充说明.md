---
title: 【数字图像处理】7.6:灰度图像--图像分割 阈值处理之补充说明
date: 2015-03-08 19:00
categories:
  - DIP
keywords:
  - 阈值处理
toc: true
---
**Abstract:** 数字图像处理：第55天
**Keywords:** 阈值处理
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
在前面的介绍中，说到过，影响阈值处理的两个主要问题是目标和背景的大小关系，和噪声对目标的影响，补充说明就是来解决下这两个问题。
#算法原理
首先来解决噪声影响，在图像增强的时候提到过，低通滤波和平滑能够减少图像噪声，通过减少噪声，可以一定程度上提高阈值处理的结果。例如未去噪的时候直方图如下：
加入11%的高斯噪声的图像直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308182457479.jpeg)

使用高斯滤波器进行平滑后的直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308182533827.jpeg)

但是对于相对较小的目标，直方图上基本看不出目标和背景的差别:
原图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308183257726.jpeg)

直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308183149507.jpeg)

我们的方法是使用边缘处理结果边缘作为Mask，得到Mask为1的原图处的灰度值，有这些灰度值做直方图，可以得到下面：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308183446505.jpeg)

可以看到相对明显的边界，值得注意的是，这里选取边界的算法一定选用检测结果是外边界和内边界结合的边缘图像。所以我们选用Sobel算子进行边缘检测，边缘检测后的阈值处理（对边缘结果的阈值处理）。最后对Mask出来的灰度集合进行OTSU阈值计算，得出最终结果。

## 代码
上代码
```c++
/*对于小目标物体
 *使用边缘检测结果作为MASK
 *得到MASK为1处的原图灰度集合
 *对这个集合做阈值分割
 *的到最终的结果
 */
void SobelThreshold(double *src,double *dst,int width,int height,double sobel_threshold,int type){
    double *mask=(double *)malloc(sizeof(double)*width*height);
    double *temp=(double *)malloc(sizeof(double)*width*height);
    //use 0.05*width and 0.05*height gaussian mask smooth src
    GaussianFilter(src, temp, width, height, width/25,height/25, (double)width/150.);
    double max=Sobel(temp, mask, NULL, width, height, 5);
    Threshold(mask, mask, width, height, max*sobel_threshold, THRESHOLD_TYPE3);
    ///////////////////////////////////////////////////////////////////////////
    int hist[GRAY_LEVEL];
    double hist_d[GRAY_LEVEL];
    InitHistogram(hist);
    for(int i=0;i<width*height;i++)
        if(mask[i]!=0.0)
            hist[(int)src[i]]++;
    Hist_int2double(hist, hist_d);
    setHist2One(hist_d, hist_d);
    double threshold=findMaxDeta(hist_d);//
    printf("Threshold:%g \n",threshold);
    Threshold(src, dst, width, height, threshold, type);
    free(mask);
    free(temp);

}

```
## 结果分析
原图，加入3%的高斯噪声
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308184139999.jpeg)

边缘检测后的直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308184108940.jpeg)

处理结果：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308185119275.jpeg)

原图，加入7%的高斯噪声
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308185052225.jpeg)

边缘检测的直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308185222892.jpeg)

处理结果：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308185237103.jpeg)

平滑后的阈值处理：
加入11%的高斯噪声的图像，平滑后进行阈值处理：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308185619045.jpeg)

原图直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308185447349.jpeg)

平滑后直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-6-灰度图像-图像分割-阈值处理之补充说明/20150308185637843.jpeg)

## 总结
为了解决前面所说的两个影响阈值处理的两个重要因素，提出的两种解决方法，也可以使用局部阈值或者可变阈值进行处理。
待续。。。
