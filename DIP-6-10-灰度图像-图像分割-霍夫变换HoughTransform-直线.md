---
title: 【数字图像处理】6.10:灰度图像-图像分割 霍夫变换(Hough Transform)--直线"
date: 2015-02-16 09:12
categories:
  - DIP
tags:
  - 霍夫变换
  - 直线检测
toc: true
---
**Abstract:** 数字图像处理：第48天
**Keywords:** 霍夫变换,直线检测
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
废话开始，要过年了，到处人心惶惶，沉下心写篇博客，下一篇就等农历新年以后了。马上新年了，希望自己在新年能提高技术，找到一份图像处理的好工作，也希望大家都能学习到更多的知识，做自己喜欢做的事情。
以前基本每天都写博客，坚持了三个月感觉确实有提高，也能把知识总结分享出来，看着每天博客的访问量不断增长，心里很有成就感，共同学习，共同进步，喜欢分享的人，才能获得别人的分享，好多优秀的同学并不喜欢分享知识，或者用很高深的话显示出自己的知识，现在想想，能用俗话说清相对论的人才是高手，让那些故弄玄虚，作假，抄袭欺骗国家欺骗人民的院士，专家都去shi吧。
废话稍微多了一点点，说说霍夫变换，Hough Transform，由Hough提出，问题原型是如何找到图像中的直线，后来延伸到可以检测出任何可以表示成方程的图形，霍夫变换的检测可以检测不完整的图形，也就是中间有间断的，并且霍夫变换对旋转具有不变性，对噪声不敏感，但是霍夫变换的缺点是需要的存储量较大，标准的霍夫变换运算量大，相对较慢。
本篇只介绍标准霍夫变换对直线的检测，放在图像分割这部分是因为冈萨雷斯书中将霍夫变换用于连接边缘。
#数学原理
我们本篇只介绍检测直线，对于图像中直线，一般用方程 $y=kx+b$ ，式子中 $k$ 表示直线的斜率， $b$ 表示直线相对于y轴的截距，如图中所示：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215185437867.jpeg)

如果按照正常思维，搜索图中的直线，使用穷举的方法，假设图像一共有N个像素任意两点可以构成一条直线，所以过一点应该有(N-1)/2条，所以全图像存在N(N-1)/2条直线，如果要确定一点是否是直线上的点，一共需要至少 $N^2(N-1)/2$ 次计算，这个代价的算法在实际中基本没有价值，于是，Hough提出了一种巧妙的方法，将直线表示成 $-b=xk-y$ 这种变换的意义在于自变量不是x而是k，y也不再是因变量，b变成了因变量，所以图像上任意两点，可以从（x，y）坐标系映射到（k，b）坐标系，系数选取点 $(x_1=1.8,y_1=7.6)$ 和点 $(x_2=3.4,y_2=10.8)$ ，绘制出两条曲线：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215190807615.jpeg)

可以根据上图得出k=2，b=4，那么在$y=kx+b$的点的在k，b坐标系上的交点都在(2,-4)处，
根据上面我们可以利用这个特点，将一幅图像从(x,y)坐标系，转换到(k,b)坐标系，假设原图中有k个亮点，可以在(k,b)坐标系画出k条直线，那么越多的直线交于一点，说明该点的坐标为斜率和截距的直线点在原图中出现频率越大，那么我们就能根据这个特点找出这些点。
问题来了，当原图直线和x轴垂直时，斜率k趋近于无穷大，所以在(k,b)坐标系内无法表示，所以我们换一种方法，使用直线 $y=kx+b$ 的法线，截距为-b，那么这条直线为 $y=-\frac{1}{k}x-b$ ，如果使用参数方程，原直线为 $cos(\theta)y-sin(\theta)*x=cos(\theta)b$ ，令 $\varrho=cos(\theta)b$ 原坐标的直线为 $cos(\theta)y-sin(\theta)*x=\varrho$ ，那么法线的参数方程为 $sin(\theta)y+cos(\theta)*x=-\varrho$ ，方程坐标系为 $(\theta，\varrho)$  。
将上面的思想应用到 $(\theta，\varrho)$ 坐标系，我们将得到，如下的信息，原图：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193045036.jpeg)

转换到参数坐标系：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193110277.jpeg)

交点出就是两条对角线的参数。
我们来观察一条直线的参数坐标系：
水平直线：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193217279.jpeg)

对应参数坐标系：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193230180.jpeg)

垂直直线：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193232819.jpeg)

对应参数坐标系：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193243879.jpeg)

45°直线：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193256515.jpeg)

对应参数坐标系：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193308059.jpeg)

-45°直线：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193410475.jpeg)

对应参数坐标系：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193422643.jpeg)

五条直线交于一点：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193328105.jpeg)

对应参数坐标系：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193343971.jpeg)

上图中越明亮的点说明重叠参数方程越多，我们来观察两条直线对应参数坐标系的立体情况：
原图：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193815926.jpeg)

平面的参数方程坐标系：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193833164.jpeg)

参数方程坐标系的立体显示：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193931521.jpeg)

![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215193935002.jpeg)

![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215194005467.jpeg)

![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215194022798.jpeg)


#代码
```c++
void SHT(int x,int y,int zero,double * polar){
    double angle_step=POLARSTEP;
    double angle=-M_PI_2;
    for(int i=0;i<POLARWIDTH;i++){
        int p_y=(int)(((sin(angle)*y+cos(angle)*x)+0.5)*POLARHEIGHT_ZOOM)+zero;
        polar[p_y*POLARWIDTH+i]++;
        angle+=angle_step;
    }
}
/////////////////////////////////////////////////////////////////////////////
void HoughLine(double *src,double *dst,int width,int height,int lineLength){
    int polar_height=2*POLARHEIGHT_ZOOM*(int)(sqrt(width*width+height*height)+1);
    int polar_width=POLARWIDTH;
    double *polar=(double *)malloc(sizeof(double)*polar_height*polar_width);
    Zero(polar,polar_width,polar_height);
    for(int j=0;j<height;j++){
        for(int i=0;i<width;i++){
            if(src[j*width+i]==255.0)
                SHT(i, j,polar_height/2,polar);
        }
    }
    for(int j=0; j<polar_height;j++)
        for(int i=0;i<polar_width;i++){
            if(polar[j*polar_width+i]>lineLength){
                double theta=i*POLARSTEP;
                if(theta==M_PI_2)
                    DrawLine(dst, width, height, theta, abs(j-polar_height/2)/POLARHEIGHT_ZOOM);
                else if (theta==0)
                    DrawLine(dst, width, height, theta, abs(j-polar_height/2)/POLARHEIGHT_ZOOM);
                else{
                    DrawLine(dst, width, height, theta, -(int)((j-polar_height/2)/cos(i*POLARSTEP))/POLARHEIGHT_ZOOM);
                }
            }
        }
    free(polar);
}

```
## 效果
Hough变换输入图像因为边缘图像，或是二值图像效果如下图：
原图：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215194417925.png)

边缘检测：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215194403510.jpeg)

霍夫直线检测：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215194444364.jpeg)

原图：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215194448813.png)

边缘检测：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215194550446.jpeg)

霍夫直线检测：
![这里写图片描述](DIP-6-10-灰度图像-图像分割-霍夫变换HoughTransform-直线/20150215194609603.jpeg)

## 结论
霍夫变换可以有效的检测出图像中的直线，但需要设定一定的参数，比如定位参数坐标中极大值的方法，以避免错误的检出结果，广义的霍夫变换可以检测任何$g(\vec x,\vec c)=0$表示的形状，只是计算难度根据方程的复杂度决定，$\vec c$表示方程参数，参数越多需要的存储空间越大，需要的计算量也越大。
待续。。。
