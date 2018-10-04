---
title: 【数字图像处理】7.5:灰度图像-图像分割 阈值处理之OTSU阈值
date: 2015-03-08 12:35
categories:
  - DIP
keywords:
  - OTSU算法
toc: true
---
**Abstract:** 数字图像处理：第54天
**Keywords:** OTSU算法
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
# 灰度图像-图像分割 阈值处理之OTSU阈值
废话开始，今天介绍OTSU算法，本算法比前面给出的算法更能够给出数学上的最佳阈值，不需要任何输入附加参数、与同样不需要输入附加参数的迭代均值和均值阈值来比较，OTSU给出的阈值能使分类更加均匀。
阈值处理将灰度值分为两类，而对于分类问题，已有的一种最优闭合解--贝叶斯决策规则。
## 贝叶斯决策规则
首先介绍下贝叶斯公式的形象化理解，考虑下图
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308140256646.jpeg)

上面的12幅图中有手枪和弹夹，只有弹夹和手枪出现在同一个盒子的时候才有杀伤力，也就是你拿到一个盒子，你不知道里面是什么，有可能是枪，有可能是弹夹，有可能同时有枪和弹夹。下面来从概率学角度分析
设盒子里有枪为事件A，那么A出现的概率设为 $p(A)$ 。
设盒子里有弹夹为事件B，那么B出现的概率设为 $p(B)$ 。
那么同时出现事件A和事件B的概率为 $p(AB)$
看图可以知道
$p(A)=\frac{8}{12}=\frac{2}{3}$ ..........(1)
$p(B)=\frac{7}{12}$ ..........(2)
$p(AB)=\frac{3}{12}=\frac{1}{4}$ ..........(3)

考虑我们随机抽出一个盒子，先拿出一个东西，比如先拿出一把枪，那么也就是事件A发生了，那么我们继续从盒子里拿东西，有可能拿到弹夹，也有可能啥也没有，那么拿到弹夹的概率就如下：
$p(B|A)=\frac{3}{8}$ ..........(4)

同理，如果先拿出来的是个弹夹，那么接下来拿出枪的概率是：
$p(A|B)=\frac{3}{7}$ ..........(5)

结合(1)(2)(3)(4)(5)，可以得到：
$p(AB)=p(A|B)\times p(B)=p(B|A)\times p(A)$ ..........(6)


----------

假设下面情形：
已知拿出枪的概率是：
$p(A)=\frac{2}{3}$

拿出枪以后拿出弹夹的概率
$p(B|A)=\frac{3}{8}$

拿出弹夹的概率：
$p(B)=\frac{7}{12}$

求拿出弹夹以后拿出枪的概率
$p(A|B)=\frac{p(B|A)\times p(A)}{p(B)}$

以上就是贝叶斯公式的一般形式，更复杂的形式会在后面的文章中详细介绍。（更复杂的形式是指盒子里有枪，子弹，弹夹，手榴弹。。。。。。）
## 数学原理
OTSU算法可以基于直方图计算，考虑灰度级为{0，1，2........L-1}大小为 $M \times N$ 的图像，设 $n_i$ 为灰度级为i的像素的总数量，那么:
$M \times N=\sum^{L-1}_{i=0}n_i$

$p(n_i)=\frac{n_i}{M \times N}$

$\sum^{L-1}_{i=0}p_i=1$

假设阈值为k将直方图分成两部分。
部分1$(C_1)$的概率为：
$p_1(k)=\sum^{k}_{i=0}p_i$

部分2$(C_2)$的概率为：
$p_2(k)=\sum^{L-1}_{i=k+1}p_i$

部分1$(C_1)$的平均数：
$m_1(k)=\sum^{k}_{i=0}i\times P(i|C_1)=\sum^{k}_{i=0}i\times \frac{P(C_1|i)\times P(i)}{P(C_1)}$

$P(C_1|i)$ 的值为1，因为 $i$ 是属于 $C_1$ 的，所以发生$i$ 以后发生 $C_1$ 的概率是100%，所以
$m_1(k)=\frac{1}{P_1(k)}\sum^{k}_{i=0}i\times p_i$

部分2$(C_2)$ 的平均数：
$m_2(k)=\frac{1}{P_2(k)} \sum^{L-1}_{k+1}i\times p_i$


全图的均值
$m_G=\sum^{L-1}_{i=0}iP_i$

上面的式子可以由下面验证：
$P_1m_1+P_2m_2=m_G$

$P_1+P_2=1$

下面就是关键部分了，如何评价一个阈值的好坏，提出一个阈值，将像素灰度分为两类，通过以下的公式来评价阈值质量：

$\eta=\frac{\delta_B^2}{\delta_G^2}$

$\delta_G^2=\sum^{L-1}_{i=0}(i-m_G)^2\times p_i$

$\delta_B^2$是类间方差，其定义为：
$\delta_B^2=P_1(m_1-m_G)^2+P_2(m_2-m_G)^2$

公式还可以写成：
$\delta^2_B=P_1P_2(m_1-m_2)^2=\frac{P_1(m_1-m_G)^2}{1-P_1}$

于是最佳阈值$k^{*}$ 由下面得出：
$\delta^2_B(k^{*})=max_{0\leq k \leq L-1}\delta^2_B(k)$

通过上式可以通过迭代计算出最佳的k值。使用k作为阈值，对图像进行处理。
## 代码实现
```c++
/*
 *OTSU 算法
 *otsu 算法使用贝叶斯分类原理得到最好聚类
 *
 *
 */
//归一化直方图

void setHist2One(double *hist_d,double *dst_hist_d){
    double sum=0.0;
    for(int i=0;i<GRAY_LEVEL;i++)
        sum+=hist_d[i];
    if(sum!=0)
        for(int i=0;i<GRAY_LEVEL;i++)
            dst_hist_d[i]=hist_d[i]/sum;

}
//计算公式中最大的deta，并返回直方图灰度
double findMaxDeta(double *hist_d){
    double max_deta=-1.0;
    double max_deta_location=0.0;
    double m_g=0.0;

    for(int i=0;i<GRAY_LEVEL;i++)
        m_g+=i*hist_d[i];


    for(int i=0;i<GRAY_LEVEL;i++){
        double p1=0.0;
        double m1=0.0;
        double deta=0.0;
        for(int j=0;j<=i;j++){
            p1+=hist_d[j];
            m1+=j*hist_d[j];
        }
        deta=p1*(m1-m_g)*(m1-m_g)/(1-p1);
        if(deta>max_deta){
            max_deta_location=i;
            max_deta=deta;
        }
    }
    return max_deta_location;
}
void OTSUThreshold(double *src,double *dst,int width,int height,int type){
    int hist[GRAY_LEVEL];
    double hist_d[GRAY_LEVEL];
    setHistogram(src, hist, width, height);
    Hist_int2double(hist, hist_d);
    setHist2One(hist_d, hist_d);
    double threshold=findMaxDeta(hist_d);
    Threshold(src, dst, width, height, threshold, type);
}

```
## 观察结果
原图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308152743862.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308152907539.jpeg)

加入1%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308152918272.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308152814765.jpeg)

加入3%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308152827323.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308152836777.jpeg)

加入5%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308153000860.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308153012264.jpeg)

加入7%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308152915106.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308152932001.jpeg)

加入9%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308153053245.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308153102277.jpeg)

加入11%的高斯噪声：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308153112137.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308153008599.jpeg)

lena:
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308153300433.jpeg)

baboon:
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值/20150308153428432.jpeg)

## 总结
OTSU算法产生的阈值是数学角度上的最佳分类，数学基础的贝叶斯公式，但应用也有一定的局限性，比如，前面说过最多的，对全局阈值，目标与背景的大小关系，当目标和背景大小相差很多时，或者噪声很大的时候，对OTSU产生影响较大。
待续。。。





原文地址1：[https://www.face2ai.com/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值](https://www.face2ai.com/DIP-7-5-灰度图像-图像分割-阈值处理之OTSU阈值)转载请标明出处
