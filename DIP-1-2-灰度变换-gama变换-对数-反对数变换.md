---
title: \[数字图像处理\]1.2:灰度变换，gama变换，对数，反对数变换
categories:
  - DIP
keywords:
  - gama变换
  - 对数
  - 反对数变换
toc: true
date: 2014-11-11 13:55
---
**Abstract:** 数字图像处理：第2天
**Keywords:** 灰度变换，gama变换，对数，反对数变换
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>

灰度变换，及按照一定规则对像素点的灰度值进行变换，变换的结果可以增强对比度，或者达到其他的效果（例如二值化，或者伽马变换），由于灰度变换为针对单个像素点的灰度值进行变换，素以算法复杂度一般为O（W\*H）（图像宽和高）
```c++
#include <cv.h>
#include <highgui.h>
#include <stdio.h>
#include <math.h>
#define CONTRASTFUNC0 -1	//翻转
#define CONTRASTFUNC1 0		//分段
#define CONTRASTFUNC2 1		//对数
#define CONTRASTFUNC3 2		//反对数
#define CONTRASTFUNC4 3		//n次幂
#define CONTRASTFUNC5 4		//n次根
#define CONTRASTGAMA  5     //gama
#define GRAYLEVEL 8
#define MAX_PVALUE (1<<GRAYLEVEL)

#define GETPIX(image,x,y) ((unsigned char) (image->imageData)[(x)*image->width+(y)])
#define SETPIX(image,x,y,value) (((image->imageData)[(x)*image->width+y])=((unsigned char)value))

unsigned char ContrastTable[MAX_PVALUE];//映射表

void ContrastStretch(IplImage *src,IplImage *dst,int method,
  double p0,double p1,int p2,int p3){
  if(method==CONTRASTFUNC0){//图像翻转
    for(int i=0;i<MAX_PVALUE;i++)
      ContrastTable[i]=MAX_PVALUE-1-i;
  }
  else if(method==CONTRASTFUNC1){//分段拉伸
    for(int i=0;i<MAX_PVALUE;i++)
      ContrastTable[i]=i<=p0?i*p1/p0                 :
		     i<=p2?(i-p0)*(p3-p1)/(p2-p0)+p1:
		     (i-p2)*(MAX_PVALUE-1-p3)/(MAX_PVALUE-1-p2)+p3;
  }
  else if(method==CONTRASTFUNC2){//对数
    for(int i=0;i<MAX_PVALUE;i++)
      ContrastTable[i]=46*log(double(1+i));//46*log(256)近似于256

  }
  else if(method==CONTRASTFUNC3){//反对数
    for(int i=0;i<MAX_PVALUE;i++)
      ContrastTable[(int)(46*log(double(1+i)))]=i;
    for(int i=0;i<MAX_PVALUE;i++)
      if(ContrastTable[i]==0)
        ContrastTable[i]=ContrastTable[i-1];

  }
  else if(method==CONTRASTFUNC4){//N次方
    double coef=255/pow(255.,(double) p0);//coef为系数，即255要映射到255
    for(int i=0;i<MAX_PVALUE;i++)
      ContrastTable[i]=coef*pow((double)i,(double)p0);

  }
  else if(method==CONTRASTFUNC5){//N次根
    double coef=255/pow(255.,(double) p0);//coef为系数，即255要映射到255
    for(int i=0;i<MAX_PVALUE;i++)
      ContrastTable[(int)(coef*pow((double)i,(double)p0))]=i;
    for(int i=0;i<MAX_PVALUE;i++)
      if(ContrastTable[i]==0)
        ContrastTable[i]=ContrastTable[i-1];
  }
  else if(method==CONTRASTGAMA){//gama
    double gama=p0;
    double coef=255/pow(255.,gama);//coef为系数，即255的gama次幂要映射到255
    coef=(p1<=coef&&p1>0.0)?p1:coef;
    for(int i=0;i<MAX_PVALUE;i++)
      ContrastTable[i]=coef*pow((double)i,gama);
  }

///////////////////////////////重新映射/////////////////////////////////////////////
  for(int i=0;i<256;i++)
    printf("%d->%d\n",i,ContrastTable[i]);
    for(int i=0;i<src->width;i++)
      for(int j=0;j<src->height;j++)
        SETPIX(dst,i,j,ContrastTable[GETPIX(src,i,j)]);
}

int main(){
  IplImage * image = cvLoadImage("e:\\OpenCV_Image\\lena.jpg",0);
  IplImage * test =cvCreateImage(cvSize(512,512),image->depth,1);
  //meanfilter(image,test,3);
  ContrastStretch(image,test,0,100,0,100,255);
  cvNamedWindow("原图");
  cvNamedWindow("变换");
  cvShowImage("原图",image);
  cvShowImage("变换",test);
  cvSaveImage("e:\\OpenCV_Image\\lena_thr100.jpg",test);
  cvWaitKey();
  cvReleaseImage(&image);
}

```
灰度翻转：
![Center][]
分段线性变换：（50,40）（150,180）
![Center 1][]
分段线性变换（50,70）（150,130）
![Center 2][]
对数变换
![Center 3][]
反对数：
![Center 4][]
二次方：
![Center 5][]
二次根：
![Center 6][]
分段线性变换，当参数设置为（100,0）（100,255）时，函数为二值化函数：
![Center 7][]

[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-1-2-灰度变换-gama变换-对数-反对数变换/20141111133834016.jpg
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-1-2-灰度变换-gama变换-对数-反对数变换/20141111133913905.jpg
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-1-2-灰度变换-gama变换-对数-反对数变换/20141111134111109.jpg
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-1-2-灰度变换-gama变换-对数-反对数变换/20141111134125335.jpg
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-1-2-灰度变换-gama变换-对数-反对数变换/20141111134148719.jpg
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-1-2-灰度变换-gama变换-对数-反对数变换/20141111134210731.jpg
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-1-2-灰度变换-gama变换-对数-反对数变换/20141111134239732.jpg
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-1-2-灰度变换-gama变换-对数-反对数变换/20141111135816015.jpg





原文地址1：[https://www.face2ai.com/DIP-1-2-灰度变换-gama变换-对数-反对数变换](https://www.face2ai.com/DIP-1-2-灰度变换-gama变换-对数-反对数变换)转载请标明出处
