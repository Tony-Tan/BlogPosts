---
title: 【数字图像处理】5.8:灰度图像-图像增强 Robert算子、Sobel算子
date: 2015-02-01 15:28
categories:
  - DIP
keywords:
  - Sobel算子
  - Robert算子
toc: true
---
**Abstract:** 数字图像处理：第34天
**Keywords:** Sobel算子,Robert算子,
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
继续废话，之前介绍了二阶微分，和非锐化掩蔽，按照顺序该说一阶微分了，一阶微分与二阶微分一样，是线性算子，线性算子的计算方法多半是生成模板，然后与图像卷积，一阶微分同样，几天简单的介绍两个一阶微分算子，Robert算子和Sobel算子，这两个算子应该算是大名鼎鼎了，因为这两个算子在后面的边缘检测中都是里程碑似的算法，在增强部分，他们也主要用在边缘增强，本文只简要介绍下两个算子的大概使用和增强效果，具体的数学原理推导和其他性质，将在图像分割部分完整介绍。

## 图像梯度介绍
首先介绍下梯度，梯度并非是一个数值，梯度严格意义上是一个向量，这个向量指向当前位置变化最快的方向，可以这么理解，当你站在一个山上，你有360°的方向可以选择，哪个方向下降速度最快（最陡峭），便是梯度方向，梯度的长度，表示为向量的长度，表示最大的变化速率。
梯度的数学表达：

![Center][]
其中![Center 1][]表示微分算子。梯度在三维坐标中表示为：
![Center 2][]
![Center 3][]
同样在二维中只取前两项，也就是由x方向的偏微分和y方向的偏微分组成。对于图像f中点(x,y)处的梯度，定义为：
![Center 4][]
与上面所述保持一致，图像梯度方向给出图像变化最快方向，当前点的梯度长度为：
![Center 5][]
次长度计算中有平方和开平方，所以将不再是现行操作。
为了简单计算，将上面求距离简化成：
![Center 6][]
然而上面式子最大的问题在于不具有旋转不变形，也就是不是各向同性的，具体原因是三角形三遍关系原理，因为梯度方向和长度对于旋转是不变的，所以，x轴和y轴发生旋转的时候，直角三角形两边发生变化，但保持斜边长度和方向不变，因此两个直角边的长度和必然发生改变，也就是上面的M值必然会改变，顾其不具有旋转不变形。
为了表达方便先重新来定义下模板位置，如下图
![Center 7][]
其中z5表示模板中心。
## Robert算子
奇葩算子Robert，说它奇葩确实奇葩，因为不知道Robert哪来的勇气或者推导过程，使用一个2x2的模板，而且是对角线做差，其差分为：
![Center 8][]
因为向量无法在图像中显示，我们要计算梯度向量的长度：
![Center 9][]
简化为绝对值方法：
![Center 10][]
这个就是Robert交叉算子。模板：
![Center 11][]
## Sobel算子
因为Robert算子是2x2的模板，不是对称的奇数模板，我们更喜欢3x3的模板，所以，要根据上面的Robert算子改造出来一个3x3模板，提出了下面这个计算方法：
![Center 12][]
怎么来的？说实话我一开始也不知道，只是说根据上面Robert算子，搞出来一个等价的，其数字模板为：
![Center 13][]
并且其下降速率（梯度的长度）计算公式：
![Center 14][]
所有上面的疑惑就是这个公式：
![Center 12][]
到底是怎么来的，为什么中间会有2，以及为什么是隔行相减，下面的过程是我自己发明的，没有数学依据，只是自己的猜测，根据Robert算子的两个式子，横向划过3x3的所有位置，然后相加，就得到了Sobel算子：
![Center 15][]
这个过程就用Robert产生了Sobel，同样的纵向移动就会产生x轴方向的算子。Sobel算子原理的论文不多，但都说这是个很好的边缘检测算子。
## 代码
Robert：
```c++

void Robert(double *src,double *dst,int width,int height){
    double RobertMask_x[9]={0,0,0,0,-1,0,0,0,1};
    double RobertMask_y[9]={0,0,0,0,0,-1,0,1,0};
    double *dst_x=(double *)malloc(sizeof(double)*width*height);
    double *dst_y=(double *)malloc(sizeof(double)*width*height);
    RealConvolution(src, dst_x, RobertMask_x, width, height, ROBERT_MASK_SIZE,ROBERT_MASK_SIZE);
    RealConvolution(src, dst_y, RobertMask_y, width, height, ROBERT_MASK_SIZE,ROBERT_MASK_SIZE);
    for(int j=0;j<height;j++)
        for(int i=0;i<width;i++){
            dst[j*width+i]=abs(dst_x[j*width+i])+abs(dst_y[j*width+i]);

        }
    free(dst_x);
    free(dst_y);

}

void RobertSharpen(double *src,double *dst,int width,int height,double c){
    Robert(src,dst,width,height);
    for(int j=0;j<height;j++)
        for(int i=0;i<width;i++){
            dst[j*width+i]=src[j*width+i]+c*dst[j*width+i];
        }


}
```

Sobel：
```c++
void Sobel(double *src,double *dst,int width,int height){
    double SobelMask_x[9]={-1,-2,-1,0,0,0,1,2,1};
    double SobelMask_y[9]={-1,0,1,-2,0,2,-1,0,1};
    double *dst_x=(double *)malloc(sizeof(double)*width*height);
    double *dst_y=(double *)malloc(sizeof(double)*width*height);
    RealRelevant(src, dst_x, SobelMask_x, width, height, SOBEL_MASK_SIZE,SOBEL_MASK_SIZE);
    RealRelevant(src, dst_y, SobelMask_y, width, height, SOBEL_MASK_SIZE,SOBEL_MASK_SIZE);
    for(int j=0;j<height;j++)
        for(int i=0;i<width;i++){
            dst[j*width+i]=abs(dst_x[j*width+i])+abs(dst_y[j*width+i]);

        }
    free(dst_x);
    free(dst_y);

}

void SobelSharpen(double *src,double *dst,int width,int height,double c){
    Sobel(src,dst,width,height);
    for(int j=0;j<height;j++)
        for(int i=0;i<width;i++){
            dst[j*width+i]=src[j*width+i]+c*dst[j*width+i];
        }


}
```







## 结果
原图：
![Center 16][]
Robert：
![Center 17][]
Robert Sharpen：
![Center 18][]
Sobel：
![Center 19][]
Sobel Sharpen：
![Center 20][]
可以观察出Sobel边缘较宽，来观察简单图形的Robert和Sobel局部放大图：
![Center 21][]
Robert：
![Center 22][]
Sobel：
![Center 23][]
Robert局部放大1，2，3：
![Center 24][]
![Center 25][]
![Center 26][]
Sobel局部放大图1，2，3：
![Center 27][]
![Center 28][] 
![Center 29][]
## 总结
Sobel和Robert都能对边缘有较强的响应，而且Sobel对边缘的响应较宽而且更加强烈，Robert算子对边缘响应较弱，而且对弯曲的边缘敏感度第（Robert1中圆形弧形部分亮度低）。
待续。。。。









[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201135930958.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201135941800.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201140106705.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201162712251.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201140430119.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201140559196.png
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201140729925.png
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201141335631.png
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201141611356.png
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201141719523.png
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201141740230.png
[Center 11]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201141912982.png
[Center 12]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201144450542.png
[Center 13]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201144622639.png
[Center 14]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201144724093.png
[Center 15]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201145019915.png
[Center 16]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201145426245.png
[Center 17]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201145637551.jpg
[Center 18]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201151230067.jpg
[Center 19]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201151321376.jpg
[Center 20]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201151359424.jpg
[Center 21]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201151834318.jpg
[Center 22]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201151905580.png
[Center 23]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201151918466.png
[Center 24]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201152055321.png
[Center 25]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201152111373.png
[Center 26]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201152117535.png
[Center 27]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201152218563.png
[Center 28]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201152225505.png
[Center 29]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-8-灰度图像-图像增强-Robert算子-Sobel算子/20150201152216992.png
