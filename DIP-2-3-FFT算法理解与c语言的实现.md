---
title: 【数字图像处理】2.3:FFT算法理解与c语言的实现
date: 2014-11-21 15:12
categories:
  - 数字图像处理
tags:
  - FFT
  - 快速离散傅里叶变换
  - 快速傅里叶算法
toc: true
---
**Abstract:** 数字图像处理：第5天
**Keywords:** FFT,快速傅里叶变换
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>

## 为什么需要FFT
简单的说，为了速度。我们承认DFT很有用，但是我们发现他的速度不是很快，1D的DFT原始算法的时间复杂度是 $O(n^2)$ ，这个可以通过公式观察出来，对于2D的DFT其时间复杂度是 $O（n^4)$ ，这个速度真的很难接受，也就是说，你计算一幅1024x768的图像时，你将等大概。。。大概。。。我也没试过，反正很久。不信的自己去试试。所以找到一种快速方法的方法计算FFT势在必行。  以下为DFT公式
![Center][]       
计算一个4点DFT。计算量如下：
![Center 1][]


## 如何得到FFT

通过观察DFT公式，我们发现DFT计算每一项 $X[u]$ 的时候都遍历了完整的 $x[n]$ 所以，我们的想法就是能不能通过其他的 $X[u+m]$（m为一个整数，可正可负）得到 $X[u]$ ，也就是充分利用之间的计算结构来构建现在的结果，这种方法就很容易表现成迭代的形式。本文介绍基2的FFT，及离散信号 $x[n]$ 的个数为2的k次方，即如果完整的离散信号中有N个信分量 $\{x_1，x_2，x_3....x_N\}$ 其中 $N=1<<k$。

## 数学基础

FFT的数学基础其实就是：

![Center 2][]

![Center 3][]

旋转因子具有以下性质：

![Center 4][]

这些性质使得我们可以利用前面的计算结果来完成出后续的计算。

![Center 5][]



### 2-FFT

观察：一个只有两个值的离散信号，假设N=2，利用性质2-对称性可以得到

![Image 1][]

### 4-FFT

与上面2点的一样，推广到4点，将会是这样，其中方框内的操作为上述2-FFT：

![Image 1][]

最终算法：

![Center 6][]


### 8-FFT

废话不多说，和上面一样：

计算过程为：

![Center 7][]

结果为：

![Center 8][]

## $2^n$-FFT

同理，推广到2^n，可以得到类似的结果，不过我们发现，为了使输出结果为顺序结果，输入的顺序经过了一系列的调整，而且每一步WN的幂次参数也是变化，我们必须得到其变化规律才能更好的编写程序：

观察WN的变化规律为：

![Center 9][]

节点距离（设为z）就是从WN的0次幂开始连续加到$W_N$的z次幂。然后间隔为z个式子，再次从0次幂加到z次。重复以上过程：

![Center 10][]

例如第二级，间隔z=2，节点为实心黑色点，节点0,1，不做操作，节点$2*W_8^0$,节点 $3*W_8^2$ ，节点4，5不做操作。。

其内在规律就是节点i是否乘以WN：
```c++
if（i%z==奇数）
```
节点 $i*W_N^{step}$

step每次增加的数量由当前的所在的计算级决定

```c++
step=1<<（k（总级数）-i（当前级数））
```
输入参数重新排序

![Center 11][]

i行表示数组下标，蓝色数字表示实际数据：其内在规律就是，下标为偶数的将被映射到自己本身下标的 $\frac{1}{2}$ 处，下表为奇数时被映射到数组后半段 $(size_{n/2})$ 的（下标-1）$\frac{1}{2}$处，将排列后的数据分为前后两个部分，递归次过程，直到只有两个元素。则停止。过程如下：

![Center 12][]
![Center 13][]
![Center 14][]
![Center 15][]
![Center 16][]

## 观察算法

观察至此，我们已经基本把FFT蝶形算法的所有特征都搞定了，接下来就是使用代码来实现了。

![Center 17][]

## 实现代码
```c++
//
//  main.c
//  Fourer1D
//
//  Created by Tony on 14/11/16.
//  Copyright (c) 2014年 Tony. All rights reserved.
//

#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <math.h>
//mac下M_PI在math.h中有宏定义，所以这里我们选择行的宏定义
#ifndef M_PI
#define M_PI 3.14159265358979323846
#endif
#define SIZE 1024*16
#define VALUE_MAX 1000

////////////////////////////////////////////////////////////////////
//定义一个复数结构体
///////////////////////////////////////////////////////////////////
struct Complex_{
    double real;
    double imagin;
};
typedef struct Complex_ Complex;
////////////////////////////////////////////////////////////////////
//定义一个复数计算，包括乘法，加法，减法
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
////////////////////////////////////////////////////////////////////
//在FFT中有一个WN的n次方项，在迭代中会不断用到，具体见算法说明
///////////////////////////////////////////////////////////////////
void getWN(double n,double size_n,Complex * dst){
    double x=2.0*M_PI*n/size_n;
    dst->imagin=-sin(x);
    dst->real=cos(x);
}
////////////////////////////////////////////////////////////////////
//随机生成一个输入，显示数据部分已经注释掉了
//注释掉的显示部分为数据显示，可以观察结果
///////////////////////////////////////////////////////////////////
void setInput(double * data,int  n){
    //printf("Setinput signal:\n");
    srand((int)time(0));
    for(int i=0;i<SIZE;i++){
        data[i]=rand()%VALUE_MAX;
        //printf("%lf\n",data[i]);
    }

}
////////////////////////////////////////////////////////////////////
//定义DFT函数，其原理为简单的DFT定义，时间复杂度O（n^2）,
//下面函数中有两层循环，每层循环的step为1，size为n，故为O（n*n）,
//注释掉的显示部分为数据显示，可以观察结果
///////////////////////////////////////////////////////////////////
void DFT(double * src,Complex * dst,int size){
    clock_t start,end;
    start=clock();

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
       /* if(imagin>=0.0)
            printf("%lf+%lfj\n",real,imagin);
        else
            printf("%lf%lfj\n",real,imagin);*/
    }
    end=clock();
    printf("DFT use time :%lf for Datasize of:%d\n",(double)(end-start)/CLOCKS_PER_SEC,size);

}
////////////////////////////////////////////////////////////////////
//定义IDFT函数，其原理为简单的IDFT定义，时间复杂度O（n^2）,
//下面函数中有两层循环，每层循环的step为1，size为n，故为O（n*n）,
///////////////////////////////////////////////////////////////////
void IDFT(Complex *src,Complex *dst,int size){
    clock_t start,end;
    start=clock();
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
        if(imagin>=0.0)
            printf("%lf+%lfj\n",real,imagin);
        else
            printf("%lf%lfj\n",real,imagin);
    }
    end=clock();
    printf("IDFT use time :%lfs for Datasize of:%d\n",(double)(end-start)/CLOCKS_PER_SEC,size);


}
////////////////////////////////////////////////////////////////////
//定义FFT的初始化数据，因为FFT的数据经过重新映射，递归结构
///////////////////////////////////////////////////////////////////
int FFT_remap(double * src,int size_n){

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
    FFT_remap(src, size_n/2);
    FFT_remap(src+size_n/2, size_n/2);
    return 1;


}
////////////////////////////////////////////////////////////////////
//定义FFT，具体见算法说明，注释掉的显示部分为数据显示，可以观察结果
///////////////////////////////////////////////////////////////////
void FFT(double * src,Complex * dst,int size_n){

    FFT_remap(src, size_n);
   // for(int i=0;i<size_n;i++)
    //    printf("%lf\n",src[i]);
	clock_t start,end;
    start=clock();
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

   /* for(int i=0;i<size_n;i++)
        if(src_com[i].imagin>=0.0){
            printf("%lf+%lfj\n",src_com[i].real,src_com[i].imagin);
        }
        else
            printf("%lf%lfj\n",src_com[i].real,src_com[i].imagin);*/
	for(int i=0;i<size_n;i++){
		dst[i].imagin=src_com[i].imagin;
		dst[i].real=src_com[i].real;
	}
	end=clock();
    printf("FFT use time :%lfs for Datasize of:%d\n",(double)(end-start)/CLOCKS_PER_SEC,size_n);

}
////////////////////////////////////////////////////////////////////

int main(int argc, const char * argv[]) {
    double input[SIZE];
    Complex dst[SIZE];
    setInput(input,SIZE);
    printf("\n\n");
    DFT(input, dst, SIZE);
    printf("\n\n");
    FFT(input, dst, SIZE);
    //IDFT(dst, NULL, SIZE);
    getchar();
}

```


## 测试结果

![Center 18][]
![Center 19][]


**其中时间都为s，FFT优势明显**


[Center]: DIP-2-3-FFT算法理解与c语言的实现/20141121123027574.png
[Center 1]: DIP-2-3-FFT算法理解与c语言的实现/20141121153524078.png
[Center 2]: DIP-2-3-FFT算法理解与c语言的实现/20141121140504892.jpg
[Center 3]: DIP-2-3-FFT算法理解与c语言的实现/20141121140852453.png
[Center 4]: DIP-2-3-FFT算法理解与c语言的实现/20141121141004459.png
[Center 5]: DIP-2-3-FFT算法理解与c语言的实现/20141121141610124.png
[Center 6]: DIP-2-3-FFT算法理解与c语言的实现/20141121143348078.png
[Center 7]: DIP-2-3-FFT算法理解与c语言的实现/20141121143551389.png
[Center 8]: DIP-2-3-FFT算法理解与c语言的实现/20141121143455546.png
[Center 9]: DIP-2-3-FFT算法理解与c语言的实现/20141121143717002.png
[Center 10]: DIP-2-3-FFT算法理解与c语言的实现/20141121144204323.png
[Center 11]: DIP-2-3-FFT算法理解与c语言的实现/20141121145130822.png
[Center 12]: DIP-2-3-FFT算法理解与c语言的实现/20141121145922031.png
[Center 13]: DIP-2-3-FFT算法理解与c语言的实现/20141121145921491.png
[Center 14]: DIP-2-3-FFT算法理解与c语言的实现/20141121150327971.png
[Center 15]: DIP-2-3-FFT算法理解与c语言的实现/20141121150355708.png
[Center 16]: DIP-2-3-FFT算法理解与c语言的实现/20141121150428078.png
[Center 17]: DIP-2-3-FFT算法理解与c语言的实现/20141121150519714.png
[Center 18]: DIP-2-3-FFT算法理解与c语言的实现/20141121153237670.png
[Center 19]: DIP-2-3-FFT算法理解与c语言的实现/20141121153252687.png
[4-FFT.gif]: DIP-2-3-FFT算法理解与c语言的实现/20141121153252687.png
