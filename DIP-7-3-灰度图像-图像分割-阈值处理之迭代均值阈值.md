---
title: 【数字图像处理】7.3:灰度图像--图像分割 阈值处理之迭代均值阈值
date: 2015-03-07 11:49
categories:
  - DIP
keywords:
  - 迭代阈值
  - 迭代均值
toc: true
---
**Abstract:** 数字图像处理：第52天
**Keywords:** 迭代阈值,迭代均值
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
废话开始，本来打算昨天写这篇，半路被几个孙子（大学室友）拉去打Dota，结果输了一晚上，暴雪出的魔兽争霸和魔兽世界可谓游戏中的经典，一个是完美的游戏逻辑设计，其次是游戏画面，然后就有了各路模仿者，有感而发--做面向用户的应用程序，在满足软件基本要求的基础上，完美的逻辑设计和优秀的人机交互将能使软件经久不衰。
## 迭代均值算法
下面开始介绍迭代均值，迭代均值的基本算法如下

1. 初始化阈值为$T_0$
2. 用$T_i$将全部像素值分为两部分$G_1$和$G_2$，计算两部分的均值分别为$m_1$和$m_2$
3. 用$m_1$和$m_2$产生新的阈值 $T_i=\frac{m_1+m_2}{2}$
4. 迭代上面步骤2和步骤3，直到
$|T_i-T_{i-1}|<\Delta T$

收敛条件是迭代后阈值变化小于一个收敛控制条件，这个条件决定阈值收敛的精确度，当$\Delta T$设置过大，迭代次数减小，但精确度降低，如果$\Delta T$设置过小，迭代次数增加，准确度提高。
其次是初始化阈值$T_0$的选择，选择的阈值必须左右都有像素，尽量选择靠近中间的像素，这样可以有效的减少迭代次数。在代码中我使用的初始化阈值是，找出像素最大值和最小值，然后计算出他们的平均值。
#代码
此算法比较简单，上代码：
```c++
//迭代法求阈值，初始化一个阈值
//将直方图分为两部分
//求出两部分的均值
//这两个均值的均值为新的阈值，迭代这些步骤
//deta_t 精确度，当迭代n次以后阈值tn与第n-1次迭代结果tn-1相差小于deta_t时，迭代停止。
void IterativeThreshold(double *src,double *dst,double deta_t,int width,int height,int type){

    int hist[GRAY_LEVEL];
    InitHistogram(hist);
    setHistogram(src, hist, width,height);
    int hist_min=findHistogramMax(hist);
    int hist_max=findHistogramMin(hist);
    double threshold_value=(hist_max+hist_min)/2.0;
    double threshold_last=threshold_value;
    while (threshold_last-threshold_value>=deta_t||
           threshold_last-threshold_value<=-deta_t) {
        threshold_last=threshold_value;
        double mean1=getMeaninHist(0, (int)threshold_value, hist);
        double mean2=getMeaninHist((int)threshold_value,hist_max+1, hist);
        threshold_value=(mean1+mean2)/2.0;
    }
    Threshold(src,dst, width, height, threshold_value,type);
}
```
## 结果与分析
观察运行结果：
未加噪声的图像，仅有两个灰度值：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113224804.jpeg)

直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113240903.jpeg)


----------


加入1%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113445298.jpeg)

直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113458231.jpeg)

----------

加入3%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113508324.jpeg)

直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113518027.jpeg)

----------

加入5%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113531490.jpeg)

直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113544734.jpeg)

----------

加入7%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113559071.jpeg)

直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113615747.jpeg)

----------

加入9%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113738067.jpeg)

直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113750532.jpeg)

----------

加入11%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113656011.jpeg)

直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307113709988.jpeg)

----------
lena图测试结果：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307114052601.jpeg)

直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307114114332.jpeg)

----------

baboon图测试结果：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307114249163.jpeg)

直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307114301940.jpeg)

----------
## 结论
迭代均值能够以较小的计算代价得出相对准确的阈值，只需要输入一个控制精度的参数，所以属于相对自动的算法（与p-tile相比），但与前面提到的一样，算法受到目标大小的影响，当目标和背景的面积相对大小相近的时候算法计算效果较好，当目标比背景大很多的时候，算法基本没有效果，背景比目标大很多的时候同样失效（观察直方图面积可以大概观察出目标与背景的比例）。
另一个问题就是噪声影响，观察上面11%的结果，其受到噪声和目标大小的双重影响：

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-3-灰度图像-图像分割-阈值处理之迭代均值阈值/20150307114931847.png)

所以效果不理想。
待续。。。
