---
title: 【数字图像处理】2.2:二维DFT
date: 2014-11-17 15:43
categories:
  - 数字图像处理
tags:
  - DFT
toc: true
---
**Abstract:** 数字图像处理：第4天
**Keywords:** DFT, 离散傅里叶变换
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>

傅里叶变换数学原理会在后续完整介绍，目前只实现代码，观察下结果，公式在上一篇博客中已经描述

上代码：
```c++
//
//  main.c
//  Fourer2D
//
//  Created by 谭升 on 14/11/17.
//  Copyright (c) 2014年 谭升. All rights reserved.
//

#include <stdio.h>
#include <math.h>
#include <time.h>
#include <stdlib.h>
#define VALUE_MAX 255
#define WIDTH 5
#define HEIGHT 5

struct Complex_{
    double real;
    double imagin;
};
typedef struct Complex_ Complex;


int Initdata(double (*src)[WIDTH],int size_w,int size_h){
    srand((int)time(0));
    for(int i=0;i<size_w;i++){
        for(int j=0;j<size_h;j++){
            src[i][j]=rand()%VALUE_MAX;
            printf("%lf ",src[i][j]);
        }
        printf(";\n");
    }
    return 0;
}
int DFT2D(double (*src)[WIDTH],Complex (*dst)[WIDTH],int size_w,int size_h){
    for(int u=0;u<size_w;u++){
        for(int v=0;v<size_h;v++){
            double real=0.0;
            double imagin=0.0;
            for(int i=0;i<size_w;i++){
                for(int j=0;j<size_h;j++){
                    double I=src[i][j];
                    double x=M_PI*2*((double)i*u/(double)size_w+(double)j*v/(double)size_h);
                    real+=cos(x)*I;
                    imagin+=-sin(x)*I;

                }
            }
            dst[u][v].real=real;
            dst[u][v].imagin=imagin;
            if(imagin>=0)
                printf("%lf+%lfj ",real,imagin);
            else
                printf("%lf%lfj ",real,imagin);
        }
        printf(";\n");
    }
    return 0;
}
int IDFT2D(Complex (*src)[WIDTH],Complex (*dst)[WIDTH],int size_w,int size_h){
    for(int i=0;i<size_w;i++){
        for(int j=0;j<size_h;j++){
            double real=0.0;
            double imagin=0.0;
            for(int u=0;u<size_w;u++){
                for(int v=0;v<size_h;v++){
                    double R=src[u][v].real;
                    double I=src[u][v].imagin;
                    double x=M_PI*2*((double)i*u/(double)size_w+(double)j*v/(double)size_h);
                    real+=R*cos(x)-I*sin(x);
                    imagin+=I*cos(x)+R*sin(x);

                }
            }
            dst[i][j].real=(1./(size_w*size_h))*real;
            dst[i][j].imagin=(1./(size_w*size_h))*imagin;
            if(imagin>=0)
                printf("%lf+%lfj ",dst[i][j].real,dst[i][j].imagin);
            else
                printf("%lf%lfj ",dst[i][j].real,dst[i][j].imagin);
        }
        printf(";\n");
    }
    return 0;
}


int main() {
    double src[WIDTH][HEIGHT];
    Complex dst[WIDTH][HEIGHT];
    Complex dst_[WIDTH][HEIGHT];
    Initdata(src, WIDTH, HEIGHT);
    printf("\n\n");
    DFT2D(src,dst,WIDTH,HEIGHT);
    printf("\n\n");
    IDFT2D(dst,dst_,WIDTH,HEIGHT);
}
```

由于只是为了观察结果，所以使用了固定大小的二维数组，若实际工作中应该根据需要动态分配内存。如下结果：上面为原始数据，中间为DFT后的数据，最下面为IDFT后的结果。

![](DIP-2-2-二维DFT/20141117154113661.png)
