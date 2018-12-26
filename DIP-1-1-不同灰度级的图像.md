---
title: 【数字图像处理】1.1:灰度级
date: 2014-11-06 10:58
categories: DIP
tags:
  - 灰度级数
---
**Abstract:** 数字图像处理：第1天
**Keywords:** 灰度级数
<!--more-->

<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>

结论一：对于细节较多的图像，当图像大小（N）不变的情况下，灰度级别对于感官质量相对独立；
解释：如果图像细节较多，降低灰度级别，视觉上感觉差别不大
代码编写比较随意，未进行性能优化。只为观察效果：
```c++
#define K 1 //灰度级别
#define STEP (256/(1<<K))
int main(){
  //char name[50];
  IplImage * image = cvLoadImage("e:\\OpenCV_Image\\airplane.jpg",0);
  //IplImage * result = cvCreateImage(cvGetSize(image),8,1);
  cvNamedWindow("原图");
  cvNamedWindow("变换");
  cvShowImage("原图",image);
  for(int i=0;i<image->width;i++)
    for(int j=0;j<image->height;j++){
      int value=cvGetReal2D(image,i,j);
      int step=STEP;
      while(value>step){
      	step+=STEP;
    }
  value=step;
  cvSetReal2D(image,i,j,value);
  }
  cvShowImage("变换",image);
  //sprintf(name,"%s%d%s","e:\\OpenCV_Image\\airplane_",K,".jpg");
  //cvSaveImage(name,image);
  cvWaitKey();
  cvReleaseImage(&image);
  return 0;

}
```

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-1-1-不同灰度级的图像/20141106101032359.jpg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-1-1-不同灰度级的图像/20141106101045531.jpg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-1-1-不同灰度级的图像/20141106105356188.jpg)

以上三张图从左到右从上到下，分别是：原始图像，K=1,2,3,4,5,6,7,8（1阶为二值化图像，8阶为原始图像灰度图）

问题：对于同一分辨率下的图片如何评价其细节的多少。。。





原文地址1：[https://www.face2ai.com/DIP-1-1-不同灰度级的图像](https://www.face2ai.com/DIP-1-1-不同灰度级的图像)转载请标明出处
