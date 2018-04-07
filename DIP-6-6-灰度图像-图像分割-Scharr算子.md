---
title: 【数字图像处理】6.6:灰度图像-图像分割 Scharr算子
date: 2015-02-11 19:00
categories:
  - DIP
tags:
  - 边缘检测
  - scharr算子
toc: true
---
**Abstract:** 数字图像处理：第44天
**Keywords:** 边缘检测,scharr算子
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
感受下markdown的写博客的感觉，好像在写程序一样，果然是程序员的好工具，不过开头怎么没有空格。。。一空格就自动变成代码了，这让我情何以堪，好吧，以后的文章开头不空格了。本来打算上一篇直接介绍Scharr算子，但是发现Prewitt也能占很大篇幅，为了保证每一篇的内容不过长，所以拆了一篇出来，下一篇写Sobel，Prewitt，Scharr的对比。
## Scharr算子介绍
果然没有空格，好吧，不空格就不空格吧，OpenCV的Canny算法介绍中提到了Scharr算子，并且说$3\times3$的Scharr算子比Sobel算子准确性要强，后面一篇会给出一些具体的数据，以及具体的实验步骤以及数据，先看看Scharr长什么样子吧：
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211151527747.jpg)
与Sobel的不同点也是在平滑部分，这里所用的平滑算子是 $\frac{1}{16}*[3,10,3]$（LaTeX数学公式确实不错，妈妈再也不用担心我的手写体了。。），相比于 $\frac{1}{4}*[1,2,1]$ ，中心元素占的权重更重，这可能是相对于图像这种随机性较强的信号，邻域相关性不大，所以邻域平滑应该使用相对较小的标准差的高斯函数，也就是更瘦高的模板
#代码
```c++
double Scharr(double *src,double *dst,double *edgedriction,int width,int height){
    double ScharrMask1[3]={0.1875,0.625,0.1875};
    double ScharrMask2[3]={1,0,-1};
    double *dst_x=(double *)malloc(sizeof(double)*width*height);
    double *dst_y=(double *)malloc(sizeof(double)*width*height);
    RealConvolution(src, dst_x, ScharrMask1, width, height, 1, 3);
    RealConvolution(dst_x, dst_x, ScharrMask2, width, height, 3, 1);

    RealConvolution(src, dst_y, ScharrMask2, width, height, 1, 3);
    RealConvolution(dst_y, dst_y, ScharrMask1, width, height, 3, 1);
    for(int i=0;i<width*height;i++)
        dst_y[i]=-dst_y[i];
    if(edgedriction!=NULL)
        getEdgeAngle(dst_x, dst_y, edgedriction, width, height);
    for(int j=0;j<height;j++)
        for(int i=0;i<width;i++){
            dst[j*width+i]=abs(dst_x[j*width+i])+abs(dst_y[j*width+i]);
        }
    free(dst_x);
    free(dst_y);
    //matrixMultreal(dst, dst, 1.0/16.0, width, height);
    return findMatrixMax(dst,width,height);
}

```


## 效果
原图：
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211151559306.png)
scharr算子结果：
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211151921155.jpg)
按顺序局部放大：
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211151937659.jpg)
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211152002014.jpg)
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211152018719.jpg)
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211152030637.jpg)
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211152037956.jpg)
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211152049453.jpg)
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211152101949.jpg)
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211152111980.jpg)
注意7中具有细小噪声点，放大后观察：
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211152135970.jpg)
阈值处理后：
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211152157142.jpg)
![这里写图片描述](DIP-6-6-灰度图像-图像分割-Scharr算子/20150211152221912.jpg)

## 总结
Scharr作为一阶微分算子，与其他微分算子具有相同的基本特点，即对突变有较强的响应，但缺点也是使用Scharr后处理时，阈值无法很好的分离边缘候选点中边缘点与非边缘点，其优点是速度极快，而且Scharr大小固定，也就是只有$3 \times 3$，第一篇markdown的博客，待续。。。
