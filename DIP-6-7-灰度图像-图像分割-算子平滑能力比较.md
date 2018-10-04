---
title: 【数字图像处理】6.7:灰度图像-图像分割 Sobel算子，Prewitt算子和Scharr算子平滑能力比较
date: 2015-02-13 11:49
categories:
  - DIP
keywords:
  - Sobel算子
  - Scharr算子
  - Prewitt算子
toc: true
---
**Abstract:** 数字图像处理：第45天
**Keywords:** Sobel算子,Scharr算子,Prewitt算子
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
# 灰度图像-图像分割 Sobel算子，Prewitt算子和Scharr算子平滑能力比较
依然是废话，这篇主要想对比下Sobel，Prewitt和Scharr算子的平滑能力，由于一阶微分对噪声响应强，进行微分之前进行降噪是非常必要的，这里我们进行的实验是，以lena图作为实验原图，取其中一行数据作为无噪声的原始信号，分别加上不同的强度的高斯白噪声，对噪声的分类和噪声具体性质的研究将在图像恢复中描述。但这里我们使用不同强度的高斯白噪声。
## 数学原理
数学原理主要介绍下衡量噪声强度的方法-均方根误差*root-mean-square error*，第 $i$ 个测量值与真实值差的平方 $d^2$ ，对 $d^2_i$ 进行求和后平均
$$
Re=\frac{1}{n}\sum_{i=1}^nd^2_i
$$
## 代码
```matlab
%matlab代码
clear all;clc;
noise_ratio=3;%噪声的标准差3%
x=imread('/Users/Tony/DIPImage/lena','jpg');
signal=double(x(250,:));
noiseImage=uint8(randn(512,512)*2.55*noise_ratio);
dst=x+noiseImage;
figure(1);
imshow(dst);
for m=1:100
    noise=randn(1,512)*2.55*noise_ratio;
    signal_noise=signal+noise;
    for n=2:511
        scharr(n)=signal_noise(n-1)*3./16.+signal_noise(n)*10./16.+signal_noise(n+1)*3./16.;
        sobel(n)=signal_noise(n-1)*0.25+signal_noise(n)*0.5+signal_noise(n+1)*0.25;
        prewitt(n)=signal_noise(n-1)*1./3.+signal_noise(n)*1./3.+signal_noise(n+1)*1./3.;
    end
    d_noise(m)=0;
    d_scharr(m)=0;
    d_sobel(m)=0;
    d_prewitt(m)=0;
    for n=2:511
        d_scharr(m)=(d_scharr(m)+(scharr(n)-signal(n))^2);
        d_sobel(m)=(d_sobel(m)+(sobel(n)-signal(n))^2);
        d_prewitt(m)=(d_prewitt(m)+(prewitt(n)-signal(n))^2);
        d_noise(m)=d_noise(m)+noise(m)^2;
    end
end
x=1:100;
figure(2);
plot(x,d_scharr,'-r',x,d_sobel,'.-b',x,d_prewitt,'-g',x,d_noise,'-k');
```
Matlab写程序写的不多，所以将就看。
#实验结果
下面我们分别使用不同强度的高斯加性白噪声叠加到图像上，并计算
$\frac{1}{4}\times[1,2,1]$，$\frac{1}{3}\times[1,1,1]$，$\frac{1}{16}\times[3,10,3] $
对叠加了噪声的lena图的第250行数据进行平滑，叠加的噪声的标准差分别是当前信号的$0.5\%,1\%,2\%,3\%,4\%,5\%$，均值为$0$下面我们来观察效果。
下面折线图中，为了观察清楚，均方误差未乘以$\frac{1}{n}$，因为所有信号使用的n都相同，图中的黑色线为未处理信号的噪声强度，红色为Scharr算子的结果，绿色为Prewitt算子的结果，蓝色为Sobel算子结果，下面我们来观察结果：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184248537.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184348657.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184418750.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184423479.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184443179.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184447862.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184527936.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184547436.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184610602.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184614130.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184624239.png)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-7-灰度图像-图像分割-算子平滑能力比较/20150211184640975.png)

## 总结
当噪声强度超过标准差为信号的 $4\%$ 时，Sobel和Scharr的性能开始接近，超过 $5\%$ 的时候Prewitt，Sobel，Scharr性能基本相同，但小于 $3\%$ 时候Scharr的性能明显强于Sobel，并且其性能排名为$Scharr > Sobel > Prewitt$当噪声标准差为 $0.5\%$ 时，误差大概为1个像素左右，此时不进行平滑的结果更好，此时均值平滑的效果最差。
自此简单的评估了下各算子的噪声平滑效果，在小噪声情况下，$3\times3$的算子Scharr算子性能强于Sobel。
待续。。。。





原文地址1：[https://www.face2ai.com/DIP-6-7-灰度图像-图像分割-算子平滑能力比较](https://www.face2ai.com/DIP-6-7-灰度图像-图像分割-算子平滑能力比较)转载请标明出处
