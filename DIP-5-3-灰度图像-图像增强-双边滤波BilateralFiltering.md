---
title: 【数字图像处理】5.3:灰度图像-图像增强 双边滤波 Bilateral Filtering
date: 2015-01-29 15:46
categories:
  - 数字图像处理
tags:
  - 图像增强
  - 双边滤波
toc: true
---
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
废话开始，话说昨天写博客写完了，发表以后居然刷出来的是空白，顿时很生气，因为写了一上午的东西瞬间就没了，于是在微博上吐槽了csdn，于是csdn的官方微博和客服微博都跟我进行了沟通和道歉，感觉态度还是不错的，作为用户没有付给他们钱，但还是受到了不小的重视，感觉还是不错。学习是一个被分享之后经过自己加工后再分享的过程，一个分享的平台是很重要的选择，好的平台能够学到知识，并能分享知识，和别人讨论知识，收集资源分享资源。希望大家共同进步。
图像增强，平滑第二天，虽然说是第二天，但要学习和研究包括写程序，都不是一天完成的。上一篇写的是线性滤波模板，此类模板我们可以叫他们静态模板，因为其只依赖于我们的选择，我们一旦选择完成，模板就唯一确定，不会在卷积的过程中产生变换，所以这类模板具有线性性质，但缺点是不灵活，不能根据不同灰度变化情况来实时的调整权重，双边滤波就是一种非线性模板，能够根据像素位置和灰度差值的不同产生不同的模板，得到不同的滤波结果。
## 基本思路
双边滤波器是针对高斯平滑的提升版本，高斯平滑根据像素邻域的距离决定权重，生成权重的函数为高斯函数，所以叫高斯平滑或者高斯滤波，效果是使图像模糊，并一定程度上的保存边缘，双边滤波的改进是增加了灰度值的影响，也就是邻域的像素灰度值如果和中心像素的灰度值越接近，那么权值在高斯权值的基础上在加上一个相对较大的权值，相反，如果灰度差很大，将会给已生成的高斯模板对应的位置加上一个小的权值，以此类推，并将模板系数归一化（和为1，其目的是完全平滑的图像结果不变），因此模板的系数不再单纯的依赖位置关系，更依赖于灰度关系，因此边缘将能够被有效的保存。
## 数学基础
数学开始，数学公式可能看的比较难懂，但是懂了以后就会彻底理解整个算法，上面的描述只需要几个公式就能准确的表示出来。
来看基础版本的公式，这个公式通用均值滤波和高斯滤波：
![Center][]
上式中：将上式中的积分脑补成求和，求和范围是模板覆盖的范围，![Center 1][]就代表模板的坐标位置（x，y），X就可以表示参数，比如高斯里的标准差参数。1/kd(x)为归一化参数，保证绝对平滑的位置灰度值不变，c的选择比较灵活，如果选择高斯函数，那么就产生了高斯滤波模板。
![Center 2][]
上面是只利用位置（距离）产生模板系数的公式。下面我看一下只利用灰度信息产生一个模板（非线性）：
![Center 3][]
f表示灰度分布，也就是模板内被覆盖的灰度值，x（向量）代表原点位置，![Center 1][]向量代表当前求和位置，同样积分改成求和，求和区间为模板覆盖的区间，同样1/kr（x）是归一化参数，保证平滑图片的不变性。
![Center 4][]
根据上面的描述，函数c只根据位置来平滑，平滑效果好，但边缘保存弱，函数s只根据灰度差值产生模板，边缘保存效果好，平滑效果差。
为了得到一个边缘保持性好，同时平滑能力强的方法，我们决定将他们合体：
![Center 5][]
这个公式总和了上面两种处理方法，同时根据距离和灰度差值产生模板系数，产生了一种新的非线性模板，归一化的k计算如下：
![Center 6][]
对于图像，脑补成求和而不是积分，这个就是Bilateral Filter的形式，也就是说，c和s并没有规定为高斯函数，如果你有更好的，可以自己开发，当然最一般的情况下，c和s我们选择高斯函数：
![Center 7][]
其中d表示距离，这里用欧氏距离来计算（欧氏距离就是初中学的最简单的那个）： 
![Center 8][]
$\sigma_d$为距离的标准差，由我们手动决定，但要注意的是，标准差对于一个高斯函数来说，决定的是它的“胖瘦”，也就是图形是宽还是窄，如果过窄，其中心权重接近1，其他权重会很小，极限情况下退化成冲击，则只有中心位置元素，如果标准差选择过大，高斯函数会过胖，也就是趋于一条直线，这时高斯平滑接近于均值，每个位置权重过于接近，并且，距离超过3倍的deta，那么权重也会很小，所以这个性质在我们选择参数和后面观察结果也是很有用的参考。
同理我们选择高斯函数作为s函数：
![Center 9][]
式子中灰度差为：
![Center 10][]
也就是模板内不同位置的灰度与中心灰度的差。
观察下处理结果，英文不难，不再翻译：
![Center 11][]
来看原始论文的效果：
![Center 12][]
主要观察点在猫咪的胡须，看到即使右下角特别模糊的情况下，猫咪的胡须还是可以识别出来的。
## 代码
与前面文章同样，代码未经过优化，知识原始公式的翻译，如果应用于工程，需要使用快速算法或将算法进行优化：
```c++
//高斯函数
double gaussian(double x,double deta){
   return exp(-0.5*(x*x)/(deta*deta));
}
//计算当前模板系数
double BilateralWindow(double *window,int width,int height,double deta_d,double deta_r){
    double *mask=(double *)malloc(sizeof(double)*width*height);
    if(mask==NULL){
        printf("bilateral window malloc wrong\n");
        exit(0);
    }
    GaussianMask(mask,width,height,deta_d);
    double detavalue=0.0;
    double center_value=window[height/2*width+width/2];
    double k=0.0;
    double result=0.0;
    for(int j=0;j<height;j++){
        for(int i=0;i<width;i++){
            detavalue=center_value-window[j*width+i];
            mask[j*width+i]*=gaussian(detavalue,deta_r);
            k+=mask[j*width+i];
        }
    }
    for(int i=0;i<width*height;i++){
        result+=mask[i]*window[i];
    }
    free(mask);
    return result/k;
}
//双边滤波
void BilateralFilter(IplImage *src,IplImage *dst,int width,int height,double deta_d,double deta_r){
    double *window=(double *)malloc(sizeof(double)*width*height);
    for(int j=height/2;j<src->height-height/2;j++){
        for(int i=width/2;i<src->width-width/2;i++){
            for(int m=-height/2;m<height/2+1;m++){
                for(int n=-width/2;n<width/2+1;n++)
                    window[(m+height/2)*width+n+width/2]=cvGetReal2D(src, j+m, i+n);
            }
            double value=BilateralWindow(window,width,height,deta_d,deta_r);
            cvSetReal2D(dst, j, i, value);
        }
    }
    free(window);
}
```
## 观察效果
下面来观察我们的效果，具体参数已经标在了图像上：

![Center 13][]
![Center 14][]
![Center 15][]
![Center 16][]
![Center 17][]
![Center 18][]
![Center 19][]
![Center 20][]
![Center 21][]
![Center 22][]
![Center 23][]
![Center 24][]
![Center 25][]
![Center 26][]
![Center 27][]
![Center 28][]
![Center 29][]
![Center 30][]
观察结论：$\sigma_d$（距离标准差）越大会导致图像更加模糊，因为使用高斯函数，$\sigma_r$（灰度标准差）越大会导致细节变得更模糊，所以可以根据3倍 $\sigma$ 原则来选取合适的模板大小和deta大小，灰度差范围-255到255，距离差范围根据模板大小确定。

## 总结
双边滤波，可以很好的保存边缘并产生平滑效果，比高斯滤波和均值滤波效果更好，但计算量也更大
参考论文：

![Center 31][]







[Center]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129144428942.png
[Center 1]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129144755475.png
[Center 2]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129145111704.png
[Center 3]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129145404287.png
[Center 4]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129145813340.png
[Center 5]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129150801315.png
[Center 6]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129150653946.png
[Center 7]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129151118288.png
[cute.gif]: http://static.blog.csdn.net/xheditor/xheditor_emot/default/cute.gif
[Center 8]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129151125480.png
[Center 9]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129152126994.png
[Center 10]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129152138686.png
[Center 11]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129152643090.png
[Center 12]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129152737940.png
[Center 13]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153208389.png
[Center 14]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153154185.png
[Center 15]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153203607.png
[Center 16]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153215369.png
[Center 17]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153226539.png
[Center 18]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153311538.png
[Center 19]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153344782.png
[Center 20]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153355406.png
[Center 21]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153336552.png
[Center 22]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153421801.png
[Center 23]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153510910.png
[Center 24]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153532141.png
[Center 25]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153550487.png
[Center 26]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153536064.png
[Center 27]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153554690.png
[Center 28]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153659564.png
[Center 29]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153727573.png
[Center 30]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129153835535.png
[Center 31]: DIP-5-3-灰度图像-图像增强-双边滤波BilateralFiltering/20150129154416084.png
