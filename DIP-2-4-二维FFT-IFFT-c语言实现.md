---
title: 【数字图像处理】2.4:二维FFT,IFFT,c语言实现

date: 2014-11-25 19:38
categories:
  - DIP
tags:
  - FFT
  - 二维FFT
  - 二维IFFT
toc: true
---
**Abstract:** 数字图像处理：第6天
**Keywords:** 二维FFT，二维IFFT，图像快速傅里叶
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>

网上关于FFT的实例有很多，具体也可以参照上一篇，其实Matlab，OpenCV都可以很轻松的实现相关操作，但是对于学习其原理，还是自己操作下比较好。
二维FFT的是实现方法是先对行做FFT将结果放回该行，然后再对列做FFT结果放在该列，计算完所有的列以后，结果就是响应的二维FFT。
本次所有操作都是对基2的数据进行的操作。
二维IFFT网上很少见到，操作过程是：上述的傅里叶变换结果，先对每列做一维IFFT，结果放在该列，取复数矩阵其共轭，然后再按照每行做一维IFFT，其结果放在该行，即为最终结果。上代码：
一维情况下：
```c++
//  1D.c
//  Fourer
//
//  Created by 谭升 on 14/11/25.
//  Copyright (c) 2014年 谭升. All rights reserved.
//

#include "Fourer.h"



int isBase2(int size_n){
    int k=size_n;
    int z=0;
    while (k/=2) {
        z++;
    }
    k=z;
    if(size_n!=(1<<k))
        return -1;
    else
        return k;
}
////////////////////////////////////////////////////////////////////
//复数基本运算
///////////////////////////////////////////////////////////////////
void Add_Complex(Complex * src1,Complex *src2,Complex *dst){
    dst->imagin=src1->imagin+src2->imagin;
    dst->real=src1->real+src2->real;
}
void Sub_Complex(Complex * src1,Complex *src2,Complex *dst){
    dst->imagin=src1->imagin-src2->imagin;
    dst->real=src1->real-src2->real;
}
void Multy_Complex(Complex * src1,Complex *src2,Complex *dst){
    double r1=0.0,r2=0.0;
    double i1=0.0,i2=0.0;
    r1=src1->real;
    r2=src2->real;
    i1=src1->imagin;
    i2=src2->imagin;
    dst->imagin=r1*i2+r2*i1;
    dst->real=r1*r2-i1*i2;
}
void Copy_Complex(Complex * src,Complex *dst){
    dst->real=src->real;
    dst->imagin=src->imagin;
}
void Show_Complex(Complex * src,int size_n){
    if(size_n==1){
        if(src->imagin>=0.0)
        printf("%lf+%lfj  ",src->real,src->imagin);
        else
        printf("%lf%lfj  ",src->real,src->imagin);

    }
    else if(size_n>1){
        for(int i=0;i<size_n;i++)
            if(src[i].imagin>=0.0){
                printf("%lf+%lfj  ",src[i].real,src[i].imagin);
            }
            else
                printf("%lf%lfj  ",src[i].real,src[i].imagin);



    }


}
////////////////////////////////////////////////////////////////////
//计算WN
///////////////////////////////////////////////////////////////////
void getWN(double n,double size_n,Complex * dst){
    double x=2.0*M_PI*n/size_n;
    dst->imagin=-sin(x);
    dst->real=cos(x);
}
////////////////////////////////////////////////////////////////////
//随机初始化输入
///////////////////////////////////////////////////////////////////
void setInput(double * data,int  n){
    printf("Setinput signal:\n");
    srand((int)time(0));
    for(int i=0;i<n;i++){
        data[i]=rand()%VALUE_MAX;

    }

}
////////////////////////////////////////////////////////////////////
//标准DFT
///////////////////////////////////////////////////////////////////
void DFT(double * src,Complex * dst,int size){

    for(int m=0;m<size;m++){
        double real=0.0;
        double imagin=0.0;
        for(int n=0;n<size;n++){
            double x=M_PI*2*m*n;
            real+=src[n]*cos(x/size);
            imagin+=src[n]*(-sin(x/size));

        }
        dst[m].imagin=imagin;
        dst[m].real=real;

    }
}
////////////////////////////////////////////////////////////////////
//IDT，复原傅里叶
///////////////////////////////////////////////////////////////////
void IDFT(Complex *src,Complex *dst,int size){
    for(int m=0;m<size;m++){
        double real=0.0;
        double imagin=0.0;
        for(int n=0;n<size;n++){
            double x=M_PI*2*m*n/size;
            real+=src[n].real*cos(x)-src[n].imagin*sin(x);
            imagin+=src[n].real*sin(x)+src[n].imagin*cos(x);

        }
        real/=SIZE;
        imagin/=SIZE;
        if(dst!=NULL){
            dst[m].real=real;
            dst[m].imagin=imagin;
        }
    }


}
////////////////////////////////////////////////////////////////////
//FFT前，对输入数据重新排序
///////////////////////////////////////////////////////////////////
int FFTReal_remap(double * src,int size_n){

    if(size_n==1)
        return 0;
    double * temp=(double *)malloc(sizeof(double)*size_n);
    for(int i=0;i<size_n;i++)
        if(i%2==0)
            temp[i/2]=src[i];
        else
            temp[(size_n+i)/2]=src[i];
    for(int i=0;i<size_n;i++)
        src[i]=temp[i];
    free(temp);
    FFTReal_remap(src, size_n/2);
    FFTReal_remap(src+size_n/2, size_n/2);
    return 1;


}
int FFTComplex_remap(Complex * src,int size_n){

    if(size_n==1)
        return 0;
    Complex * temp=(Complex *)malloc(sizeof(Complex)*size_n);
    for(int i=0;i<size_n;i++)
        if(i%2==0)
            Copy_Complex(&src[i],&(temp[i/2]));
        else
            Copy_Complex(&(src[i]),&(temp[(size_n+i)/2]));
    for(int i=0;i<size_n;i++)
        Copy_Complex(&(temp[i]),&(src[i]));
    free(temp);
    FFTComplex_remap(src, size_n/2);
    FFTComplex_remap(src+size_n/2, size_n/2);
    return 1;


}
////////////////////////////////////////////////////////////////////
//FFT公式
///////////////////////////////////////////////////////////////////
void FFT(Complex * src,Complex * dst,int size_n){

    int k=size_n;
    int z=0;
    while (k/=2) {
        z++;
    }
    k=z;
    if(size_n!=(1<<k))
        exit(0);
    Complex * src_com=(Complex*)malloc(sizeof(Complex)*size_n);
    if(src_com==NULL)
        exit(0);
    for(int i=0;i<size_n;i++){
        Copy_Complex(&src[i], &src_com[i]);
    }
    FFTComplex_remap(src_com, size_n);
    for(int i=0;i<k;i++){
        z=0;
        for(int j=0;j<size_n;j++){
            if((j/(1<<i))%2==1){
                Complex wn;
                getWN(z, size_n, &wn);
                Multy_Complex(&src_com[j], &wn,&src_com[j]);
                z+=1<<(k-i-1);
                Complex temp;
                int neighbour=j-(1<<(i));
                temp.real=src_com[neighbour].real;
                temp.imagin=src_com[neighbour].imagin;
                Add_Complex(&temp, &src_com[j], &src_com[neighbour]);
                Sub_Complex(&temp, &src_com[j], &src_com[j]);
            }
            else
                z=0;
        }

    }


    for(int i=0;i<size_n;i++){
        Copy_Complex(&src_com[i], &dst[i]);
    }
    free(src_com);


}
void RealFFT(double * src,Complex * dst,int size_n){


    int k=size_n;
    int z=0;
    while (k/=2) {
        z++;
    }
    k=z;
    if(size_n!=(1<<k))
        exit(0);
    Complex * src_com=(Complex*)malloc(sizeof(Complex)*size_n);
    if(src_com==NULL)
        exit(0);
    for(int i=0;i<size_n;i++){
        src_com[i].real=src[i];
        src_com[i].imagin=0;
    }
    FFTComplex_remap(src_com, size_n);
    for(int i=0;i<k;i++){
        z=0;
        for(int j=0;j<size_n;j++){
            if((j/(1<<i))%2==1){
                Complex wn;
                getWN(z, size_n, &wn);
                Multy_Complex(&src_com[j], &wn,&src_com[j]);
                z+=1<<(k-i-1);
                Complex temp;
                int neighbour=j-(1<<(i));
                temp.real=src_com[neighbour].real;
                temp.imagin=src_com[neighbour].imagin;
                Add_Complex(&temp, &src_com[j], &src_com[neighbour]);
                Sub_Complex(&temp, &src_com[j], &src_com[j]);
            }
            else
                z=0;
        }

    }


    for(int i=0;i<size_n;i++){
        Copy_Complex(&src_com[i], &dst[i]);
    }
    free(src_com);

}

////////////////////////////////////////////////////////////////////
//IFFT实现
////////////////////////////////////////////////////////////////////
void IFFT(Complex * src,Complex * dst,int size_n){
    for(int i=0;i<size_n;i++)
        src[i].imagin=-src[i].imagin;
    FFTComplex_remap(src, size_n);
    int z,k;
    if((z=isBase2(size_n))!=-1)
        k=isBase2(size_n);
    else
        exit(0);
    for(int i=0;i<k;i++){
        z=0;
        for(int j=0;j<size_n;j++){
            if((j/(1<<i))%2==1){
                Complex wn;
                getWN(z, size_n, &wn);
                Multy_Complex(&src[j], &wn,&src[j]);
                z+=1<<(k-i-1);
                Complex temp;
                int neighbour=j-(1<<(i));
                Copy_Complex(&src[neighbour], &temp);
                Add_Complex(&temp, &src[j], &src[neighbour]);
                Sub_Complex(&temp, &src[j], &src[j]);
            }
            else
                z=0;
        }

    }
    for(int i=0;i<size_n;i++){
            dst[i].imagin=(1./size_n)*src[i].imagin;
            dst[i].real=(1./size_n)*src[i].real;
        }
}
```
二维情况下：
```c++
//
//  2D.c
//  Fourer
//
//  Created by 谭升 on 14/11/25.
//  Copyright (c) 2014年 谭升. All rights reserved.
//

#include "Fourer.h"
/*


 */
int DFT2D(double *src,Complex *dst,int size_w,int size_h){
    for(int u=0;u<size_w;u++){
        for(int v=0;v<size_h;v++){
            double real=0.0;
            double imagin=0.0;
            for(int i=0;i<size_w;i++){
                for(int j=0;j<size_h;j++){
                    double I=src[i*size_w+j];
                    double x=M_PI*2*((double)i*u/(double)size_w+(double)j*v/(double)size_h);
                    real+=cos(x)*I;
                    imagin+=-sin(x)*I;

                }
            }
            dst[u*size_w+v].real=real;
            dst[u*size_w+v].imagin=imagin;

        }

    }
    return 0;
}
/*

 */
int IDFT2D(Complex *src,Complex *dst,int size_w,int size_h){
    for(int i=0;i<size_w;i++){
        for(int j=0;j<size_h;j++){
            double real=0.0;
            double imagin=0.0;
            for(int u=0;u<size_w;u++){
                for(int v=0;v<size_h;v++){
                    double R=src[u*size_w+v].real;
                    double I=src[u*size_w+v].imagin;
                    double x=M_PI*2*((double)i*u/(double)size_w+(double)j*v/(double)size_h);
                    real+=R*cos(x)-I*sin(x);
                    imagin+=I*cos(x)+R*sin(x);

                }
            }
            dst[i*size_w+j].real=(1./(size_w*size_h))*real;
            dst[i*size_w+j].imagin=(1./(size_w*size_h))*imagin;

        }
    }
    return 0;
}
/*



 */
void ColumnVector(Complex * src,Complex * dst,int size_w,int size_h){
    for(int i=0;i<size_h;i++)
        Copy_Complex(&src[size_w*i], &dst[i]);

}
/*

 */
void IColumnVector(Complex * src,Complex * dst,int size_w,int size_h){
    for(int i=0;i<size_h;i++)
        Copy_Complex(&src[i],&dst[size_w*i]);

}
/*
 */
int FFT2D(double *src,Complex *dst,int size_w,int size_h){
    if(isBase2(size_w)==-1||isBase2(size_h)==-1)
        exit(0);
    Complex *temp=(Complex *)malloc(sizeof(Complex)*size_h*size_w);
    if(temp==NULL)
        return -1;
    for(int i=0;i<size_h;i++){
        RealFFT(&src[size_w*i], &temp[size_w*i], size_w);
    }

    Complex *Column=(Complex *)malloc(sizeof(Complex)*size_h);
    if(Column==NULL)
        return -1;
    for(int i=0;i<size_w;i++){
        ColumnVector(&temp[i], Column, size_w, size_h);
        FFT(Column, Column, size_h);
        IColumnVector(Column, &temp[i], size_w, size_h);

    }



    for(int i=0;i<size_h*size_w;i++)
        Copy_Complex(&temp[i], &dst[i]);
    free(temp);
    free(Column);
    return 0;
}
/*
 */
int IFFT2D(Complex *src,Complex *dst,int size_w,int size_h){

    if(isBase2(size_w)==-1||isBase2(size_h)==-1)
        exit(0);

    Complex *temp=(Complex *)malloc(sizeof(Complex)*size_h*size_w);
    if(temp==NULL)
        return -1;
    Complex *Column=(Complex *)malloc(sizeof(Complex)*size_h);
    if(Column==NULL)
        return -1;

    for(int i=0;i<size_w;i++){
        ColumnVector(&src[i], Column, size_w, size_h);
        IFFT(Column, Column, size_h);
        IColumnVector(Column, &src[i], size_w, size_h);

    }
    for(int i=0;i<size_w*size_h;i++)
        src[i].imagin=-src[i].imagin;
    for(int i=0;i<size_h;i++){
        IFFT(&src[size_w*i], &temp[size_w*i], size_w);

    }


    for(int i=0;i<size_h*size_w;i++)
        Copy_Complex(&temp[i], &dst[i]);
    free(temp);
    free(Column);
    return 0;

}

```


结果:

![Center][]


![Center 1][]



[Center]: DIP-2-4-二维FFT-IFFT-c语言实现/20141125193401542.png
[Center 1]: DIP-2-4-二维FFT-IFFT-c语言实现/20141125193557185.png
