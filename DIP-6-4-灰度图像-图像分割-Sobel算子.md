---
title: 【数字图像处理】6.4:灰度图像-图像分割 Sobel算子
date: 2015-02-10 11:54
categories:
  - 数字图像处理
tags:
  - Sobel算子
  - 边缘检测
toc: true
---
**Abstract:** 数字图像处理：第42天
**Keywords:** 边缘检测,Sobel算子
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
废话开始，Sobel我们并不陌生，之前在图像增强的时候也已经介绍了它的作用，并且还杜撰了一下它的来历，也就是用Robert平移相加（类似于相关或卷积），下面可以给出Sobel的另一个来源，因为Sobel数学推导的过程和资料很少，而且当时提出Sobel的时候应该也是没有数学论证的，而只是简单的实验后，发现效果非常好。
我们还要介绍下扩展Sobel算子，Sobel原始模型为标准3x3模板，但可以扩展成5x5到任意奇数x奇数的大小，而模板系数的确定可以根据帕斯卡三角来计算，真的很神奇。Sobel之后延伸出了Scharr算子，这个算子也为3x3算子，但是效果据说比3x3的Sobel好，后面文章将会给出具体对比。

## 算子形式
数学形式的标准Sobel为：
![Center][] 
![Center 1][]    
此模板为最早提出的Sobel模板，由于模板的对称性，我们可以将它分解一下，并根据卷积的运算性质，可以得到：
![Center 2][]
也就是说，图像对Sobel 的响应等于，对模板分解后的小模板分别卷积，而观察小模板我们可以发现，其中 $[1,0,-1]$ 或其转置为差分，也就是用于寻找边缘候选点的，而另一个 $[1,2,1]$ 是一个标准平滑算子，这也就是很多书上说，Sobel具有平滑和微分的功效，原因就是这里了，也就是说，算子先将图像横向或纵向平滑，然后再纵向或横向差分，得到的结果是平滑后的差分结果。
或者，也可根据以下方式得到分解到的两个模板，其中星号表示卷积：
![Center 3][]
另一种得到模板的方法是通过帕斯卡三角，得到，并且帕斯卡三角的奇数行是最有高斯模板的整数系数的逼近，也就是说，高斯模板可以通过帕斯卡三角查询到其整数系数的近似，来观察帕斯卡三角：
![Center 4][]
其中标注的就可以用来生成扩展的Sobel算子，其中较常用的有5x5和7x7的模板。
用两个小模板分别卷积的另一个好处是减少计算量，对于使用大小为nxn的模板，卷积计算量为 $O(n*n*width*height$ 而分开成小模板卷积计算量是 $O(2*n*width*height$ 也就是 $O(n*width*height$ 减少了一项，当n相对较大的时候，计算量明显减少。
帕斯卡三角的计算是通过组合公式给出，具体不在这里描述，所以Sobel算子的模板计算方法我们就有了大概的了解。
opencv文档中给出了关于sobel算子的下面信息：
![Center 5][]
和上面描述的方法类似，更直观，可以用来理解sobel的模板结构，不同的差分方向带来的问题就是边缘方向的确定，由于算子属于一阶微分，也就是梯度算子之一，所以梯度方向信息也显得很重要，比如后面要说的canny就是用到了梯度方向的信息，所以，在确定方向时要注意算子的差分方向。
对于阶梯型边缘，计算过程及结果如下，红色为模板中心：
![Center 6][]
可以看到，相比于Robert算子，Sobel得到的边界候选位置相对较宽，而且包括全部的内边界和外边界。并且差分被放大了，也就是说，用Sobel算子处理后的图片有可能超过原图像灰度级别，对于这个问题，处理方法是将平滑分算子（分解后的平滑部分，例如【1，2，1】）归一化，得到的差值仍在原始灰度级范围内。

## 代码效果
代码：
```c++
double Sobel(double *src,double *dst,double *edgedriction,int width,int height,int sobel_size){
    //double SobelMask_x[3]={-1,-2,-1,0,0,0,1,2,1};
    double *dst_x=(double *)malloc(sizeof(double)*width*height);
    double *dst_y=(double *)malloc(sizeof(double)*width*height);
    if(sobel_size==3){
        double SobelMask1[3]={0.25,0.5,0.25};
        double SobelMask2[3]={1,0,-1};
        RealConvolution(src, dst_x, SobelMask1, width, height, 1, 3);
        RealConvolution(dst_x, dst_x, SobelMask2, width, height, 3, 1);

        RealConvolution(src, dst_y, SobelMask2, width, height, 1, 3);
        RealConvolution(dst_y, dst_y, SobelMask1, width, height, 3, 1);
    }else if(sobel_size==5){
        double SobelMask1[5]={0.0625,0.25,0.375,0.25,0.0625};
        double SobelMask2[5]={1/3.0,2/3.0,0,-2/3.0,-1/3.0};
        RealConvolution(src, dst_x, SobelMask1, width, height, 1, 5);
        RealConvolution(dst_x, dst_x, SobelMask2, width, height, 5, 1);

        RealConvolution(src, dst_y, SobelMask2, width, height, 1, 5);
        RealConvolution(dst_y, dst_y, SobelMask1, width, height, 5, 1);

    }else if(sobel_size==7){
        double SobelMask1[7]={0.015625,0.09375,0.234375,0.3125,0.234375,0.09375,0.015625};
        double SobelMask2[7]={0.1,0.4,0.5,0,-0.5,-0.4,-0.1};
        RealConvolution(src, dst_x, SobelMask1, width, height, 1, 7);
        RealConvolution(dst_x, dst_x, SobelMask2, width, height, 7, 1);

        RealConvolution(src, dst_y, SobelMask2, width, height, 1, 7);
        RealConvolution(dst_y, dst_y, SobelMask1, width, height, 7, 1);

    }
    if(edgedriction!=NULL)
        //getEdgeDirection(dst_x, dst_y, edgedriction, width, height);
        getEdgeAngle(dst_x, dst_y, edgedriction, width, height);
    for(int j=0;j<height;j++)
        for(int i=0;i<width;i++){
            dst[j*width+i]=abs(dst_x[j*width+i])+abs(dst_y[j*width+i]);
        }
    free(dst_x);
    free(dst_y);
    return findMatrixMax(dst,width,height);
}
```
下面对比3x3，5x5，7x7算子的效果：
原图：
![Center 7][]
![Center 8][]
![Center 9][]
![Center 10][]
局部放大：
![Center 11][] 
![Center 12][] 
![Center 13][]
阈值后
![Center 14][]
![Center 15][]
![Center 16][]
![Center 17][]
## 结论
Sobel算子的效果相比于其他算子，效果较好，而且计算量不大，可以用于实时系统，其结合简单的阈值可以得到较好的效果，但得到的边缘较宽，可以使用形态学细化。
待续。。。。



[Center]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210091720752.png
[Center 1]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210091735242.png
[Center 2]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210092132940.png
[Center 3]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210100357601.png
[Center 4]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210103705512.png
[Center 5]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210104639175.png
[Center 6]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210112905939.png
[Center 7]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210113951970.png
[Center 8]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210114237156.png
[Center 9]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210114247280.png
[Center 10]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210114257374.png
[Center 11]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210114304973.png
[Center 12]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210114311728.png
[Center 13]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210114323519.png
[Center 14]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210115127916.png
[Center 15]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210115128917.png
[Center 16]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210115134814.png
[Center 17]: DIP-6-4-灰度图像-图像分割-Sobel算子/20150210115140742.png
