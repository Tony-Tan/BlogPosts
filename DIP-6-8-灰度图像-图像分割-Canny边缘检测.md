---
title: 【数字图像处理】6.8:灰度图像-图像分割 Canny边缘检测
date: 2015-02-13 11:55
categories:
  - DIP
keywords:
  - canny
  - 边缘检测
toc: true
---
**Abstract:** 数字图像处理：第46天
**Keywords:** canny,边缘检测
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
废话开始，Canny大名鼎鼎，大家都称之为Canny算子，包括wiki上也是写的Canny detector，但是按照我的理解，我觉得叫做Canny算法比较合适，但如果叫做算子，那也应该叫做复合算子，因为Canny本身并不是一个线性模板（像Sobel那样）或者一个局部比较类的算法（像中值滤波那样的），Canny是一套完整的理论，并实现出了完整的算法。
Canny是目前已知的最好的边缘检测算法，是不是之一我不确定，但可以肯定的是，它的应用非常广泛，基本用到边缘检测的，大家永远第一个想到它。Canny算法的复杂度比前面的**检测加阈值**的算法计算复杂度更高，空间复杂度也要高一些，但现在的计算设备，对于Canny基本可以实现实时，并且有人用GPU来实现，所以从86年Canny提出了这个算法到现在，在边缘检测方面，其地位还是比较稳固的。
Canny算法的另一个显著特征是它有完整的数学推导过程，能够证明这个算法能给出最好的边缘。后面我们将会简单的看一下数学过程。


----------


## 算法原理
算法原理，Canny首先提出了三种基本条件，来定义一个边缘。来看原文【Canny1986】：
>1) Good detection. There should be a low probability of failing to mark real edge points, and low probability of falsely marking nonedge points. Since both these probabilities are monotonically decreasing functions of the output signal-to-noise ratio, this criterion corresponds to maximizing signal-to-noise ratio.
>2) Good localization. The points marked as edge points by the operator should be as close as possible to the center of the true edge.
>3) Only one response to a single edge. This is implicitly captured in the first criterion since when there are two responses to the same edge, one of them must be considered false. However, the mathematical form of the first criterion did not capture the multiple response requirement and it had to be made explicit.

翻译一下：

1. 好的检测：一定要尽可能少的遗漏边缘点，尽可能少的添加非边缘点。正确的边缘点为信号，被错误检测出来的非边缘点为噪声，所以第一点归结为提高信噪比。
2. 准确的位置：检测出的边缘点一定要与真正的边缘中心，尽可能的近。
3. 边缘单一响应：对于一个边缘，只能产生一个响应，如果对于一个边缘产生两个响应，第一点的数学求解过程不能保证这一点，所以要单独明确，如果对于一边有两个响应，必须去掉一个。

这就是Canny的指导思想，并且根据这一思想进行建模。


----------


## 数学原理
发表在VOL. PAMI-8, NO. 6, NOVEMBER 1986上的文章给出了明确的求解过程，包括建模上面的理论，并求解最优解，因为本人数学功底一般，后面的求解过程有兴趣的同学可以自行查看论文，这里只给出建模上面三个基本原理的过程，以一维下的情况给出。
首先我们设滤波器的单位冲击响应为： $f(x)$ ，定义边缘本身为 $G(x)$ ，边缘中心位置为$x=0$处，滤波器响应范围 $[-w,+w]$
边缘对滤波器的响应为：
$H_G(x)=\int_{-w}^{+w} G(-x)f(x)\,dx$ (1)

噪声 $n(x)$ 相应的均方根为：
$H_n=n_0[\int_{-w}^{+w}f^2(x)\,dx]^{1/2}$ (2)

其中 $n^2_0$ 是单位长度的均方噪声幅度
由上面两个式子，我们定义信噪比SNR：
$SNR=\frac{\int_{-w}^{+w} G(-x)f(x)\,dx}{n_0[\int_{-w}^{+w}f^2(x)\,dx]^{1/2}}$ (3)

这便是原理1的建模，使信噪比尽可能大，来满足原理1。
为了度量检测出的边缘与实际边缘的位置关系，我们使用均方根误差距离。对于检测滤波器 $f(x)$ 我们一般认为滤波结果给出的局部最大值为检测到的边缘，所以如果没有噪声加入时响应结果的一阶导数在$x=0$处应该为0，即 $x=0$ 为对滤波器响应的局部最大值，也就是计算边界点。
设 $H_n(x)$ 只是噪声对滤波器的响应， $H_G(x)$ 只是边缘对滤波器的响应，根据上面的设想，一定有 $x=x_0$ 处满足：

$H^{'}_n(x_0)+H^{'}_G(x_0)=0$  (4)

泰勒展开 $H^{'}_G(x_0)$ 可以得出：

$H^{'}_G(x_0)=H^{'}_G(0)+H^{''}_G(0)x_0+o(x_0^2)$ (5)

因为可以确定 $H^{'}_G(0)=0$ 结合(4)(5)可以得出：

$H^{''}_G(0)x_0 \approx {-H^{'}_n(x_0)}$(6)

 $H^{'}_n(x_0)$ 为高斯随机量，并且其方差是 $H^{'}_n(x_0)$ 的均方值，并且给出其期望为：

$E[H^{'}_n(x_0)^2]=n_0^2\int^{+w}_{-w}f^{'2}(x)\,dx$(7)

将(7)和(6)结合，可以得到：

$E[x^2_0]\approx {\frac{n_0^2\int^{+w}_{-w}f^{'2}(x)\,dx}{[{\int_{-w}^{+w} G^{'}(-x)f^{'}(x)\,dx}]^2}}=\delta x^2_0$ (8)

$\delta x^2_0$ 是 $x_0$ 标准差的近似，位置由其倒数给出：

$Localization =\frac{|\int^{+w}_{-w}G^{'}(-x)f^{'}(x)\,dx|}{n_0[\int^{+w}_{-w}f^{'2}(x)\,dx]^{-1/2}}$(9)

这便是原理2的建模，使信噪比尽可能大，来满足原理1。
结合原理1，原理2，我们得出为了满足前面两个基本原理，需要最大化：

$\frac{\int_{-w}^{+w} G(-x)f(x)\,dx}{n_0[\int_{-w}^{+w}f^2(x)\,dx]^{1/2}}\frac{|\int^{+w}_{-w}G^{'}(-x)f^{'}(x)\,dx|}{n_0[\int^{+w}_{-w}f^{'2}(x)\,dx]^{-1/2}}$(10)

下面就是第三个原理的过程了，其与前两个原理使用的只是不太相同，根据Schwarz不等式，SNR（3）中给出的式子上边界为：

$n^{-1}_0[\int^{+w}_{-w}G^{2}(x)dx]^{1/2}$

Localization(9)上边界于：

$n^{-1}_0[\int^{+w}_{-w}G'^{2}(x)dx]^{1/2}$

最大值出现在 $f(x)=G(-x)$ 其中 $x$ 属于 $[-w,+w]$
检测点到真实点的平均距离为：

$x_{ave}=\pi(\frac{-R(0)}{R^{''}(0)})^{1/2}$(11)

其中R是函数g的自相关：

$R(0) = \int ^{+\infty}_{-\infty}g^{2}(x)\,dx$

$R^{''}(0) = -\int ^{+\infty}_{-\infty}{g'}^{2}(x)\,dx$


所以 $f'$ 的零交叉平均距离为：

$x_{zc}(f)=\pi(\frac{\int ^{+\infty}_{-\infty}f'^{2}(x)\,dx}{\int ^{+\infty}_{-\infty}{f''}^{2}(x)\,dx})^{1/2}$(12)


对于 $f$ 的最大噪声响应 $x_{max}$ 定义为两倍 $x_{zc}$ 我们定义其为滤波器宽度的k倍

$x_{max}(f)=2x_{zc}(f)=kW$(13)

所以噪声最大的数量在 $N_n$ 区域内：

$N_n=\frac{2W}{x_{max}}=\frac{2}{k}$(14)

数学建模大概如上，可能看不懂，但是没关系，因为这并不影响我们对算法过程的理解。
----------


## 算法过程
算法过程比较容易：

1. 高斯平滑，采用$5 \times5$的高斯滤波器对图像进行平滑。
2. 使用Sobel算子检测边缘候选点，计算梯度方向，得到简化的梯度方向。
3. 非极大值抑制，减少多重响应。
4. 边缘跟踪，采用双阈值处理候选点。
5. 形态学细化，对于有些较粗的边界采用形态学方法处理。

具体解析：
第一步：高斯平滑，Canny的核心检测算子是Sobel算子，所以。Canny如果按照算子划分的话属于一阶微分算子，所以它具有一阶微分算子的特性，对噪声敏感，所以，我们的第一步就是降低噪声，使用高斯平滑降低噪声，冈萨雷斯书上介绍说Canny证明高斯平滑是最好的平滑算子，这一点我并未查证，但根据高斯函数的频域特性可知，它没有振铃，而且当频率值超过 $3\times \delta$ 时基本趋近于零，所以可以当做一种完美的滤波器（以上这句话是我猜测的，没有理论根据），此步骤的主要目的--降噪
第二步：Sobel边缘检测，此处为检测边缘候选点，并计算出候选点梯度的过程，其中Sobel可选大小为3x3，5x5，7x7或者-1，其中-1表示为Scharr算子，这个在前文已经比较过了Sobel和Scharr的性能，在这里不再解释
第三步：局部非极大值抑制，俗话就是找局部最大值，寻找方法是得出每个候选点的梯度方向，沿着梯度方向和梯度反方向比较相邻的元素，如果候选点是三个中最大的，则保留，否则，置零。
第四步：双阈值滞后滤波，这里的滞后滤波应该是比较难以理解的，我们分别设置两个阈值，大阈值和小阈值之间比例大于为3：1或2：1，这里的方法是:

```
 1. 输入候选点灰度值
 2. 判断是否大于大阈值，如果大于则到第4步
 2. 判断是否大于小阈值，如果小于则到第5步
 3. 判断该点是否连通于边缘点，不是则到第5步
 4. 此点为边缘点，到第6步
 5. 此点非边缘点
 6. 如果图像还有未处理候选边缘像素，返回第1步，否则结束
```

双阈值滞后处理是Canny的核心部分，下面给出双阈值的阈值传递函数：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212181022310.jpeg)

上图说明只有的时滞后阈值的传递函数，解释下就是如果一个候选点大于高阈值，那么它肯定是边缘点，如果大于小阈值，则需要它连通到边缘点。
我们在算法实现时使用方法是，先找到所有大于大阈值的点的集合H，H全部为边缘点，然后找出所有大于小阈值的点的集合L，其中L包含所有的H，那么以H中的每个点为种子点，以八邻域遍历L，被遍历到的为边缘点，未被遍历的为非边缘点。这里的遍历与图的遍历相同，可以使用深度优先或广度优先。

第五步：细化结果，有时一个边缘会产生两个等价的边缘点，使用细化可以得到单像素边缘，但需注意，这两个点都是正确的点，选其中任意一个都是正确的。
## 代码实现

```c++
/*
 *   四个角度对应编号
 *   0 1 2
 *   3 * 5
 *   6 7 8
 *   edgedirction
 */
void getEdgeDirection(double *edgedirection,double *sample_direction,int width,int height){
    double angle=0.0;
    for(int i=0;i<width*height;i++){
        angle=edgedirection[i];
        if(angle<22.5||angle>=337.5)
            sample_direction[i]=5.0;
        else if(angle<67.5&&angle>=22.5)
            sample_direction[i]=2.0;
        else if(angle<112.5&&angle>=67.5)
            sample_direction[i]=1.0;
        else if(angle<157.5&&angle>=112.5)
            sample_direction[i]=0.0;
        else if(angle<202.5&&angle>=157.5)
            sample_direction[i]=3.0;
        else if(angle<247.5&&angle>=202.5)
            sample_direction[i]=6.0;
        else if(angle<292.5&&angle>=247.5)
            sample_direction[i]=7.0;
        else if(angle<337.5&&angle>=292.5)
            sample_direction[i]=8.0;
        else if(angle==-1.0)
            sample_direction[i]=-1.0;
        }
}
/*
 *   四个角度对应编号
 *   0 1 2
 *   3 * 5
 *   6 7 8
 *
 */
void Non_MaxSuppression(double *src,double *dst,double *dirction,int width,int height){
    double *temp=(double*)malloc(sizeof(double)*width*height);
    int dir;
    int y;
    int x;
    double value_c;
    Zero(temp, width, height);
    for(int j=1;j<height-1;j++)
        for(int i=1;i<width-1;i++){
            if(dirction[j*width+i]!=-1.0){
                dir=(int)dirction[j*width+i];
                y=dir/3-1;
                x=dir%3-1;
                value_c=src[j*width+i];
                if(value_c<=src[(j+y)*width+i+x]||value_c<src[(j-y)*width+i-x])
                    temp[j*width+i]=0.0;
                else
                    temp[j*width+i]=value_c;
            }
        }
    matrixCopy(temp, dst, width, height);
    free(temp);
}
void EdgeTrack(double *src,int width,int height,Position *seed){
    int x=seed->x;
    int y=seed->y;
    if(x>=0&&x<width&&y>=0&&y<height&&src[y*width+x]==1.0){
        src[y*width+x]=2;
        for(int j=-1;j<2;j++)
            for(int i=-1;i<2;i++){
                if(!(j==0&&i==0)){
                    Position seed_next;
                    seed_next.x=x+i;
                    seed_next.y=y+j;
                    EdgeTrack(src,width,height,&seed_next);
                }
            }
    }
}
void NonZeroSetOne(double *src,double *dst,int width,int height){
    for(int i=0;i<width*height;i++)
        dst[i]=src[i]!=0.0?1.0:0.0;
}
void Canny(double *src,double *dst,int width,int height,int sobel_size,double threshold1,double threshold2){
    double *temp=(double *)malloc(sizeof(double)*width*height);
    double *edge_a=(double *)malloc(sizeof(double)*width*height);//边缘幅度
    double *edge_d=(double *)malloc(sizeof(double)*width*height);//边缘方向
    double *threshold_max=(double *)malloc(sizeof(double)*width*height);
    double *threshold_min=(double *)malloc(sizeof(double)*width*height);
/*
 *step1:gaussian smooth
*/
    double gaussianmask[25]={ 2, 4, 5, 4, 2,
                              4, 9,12, 9, 4,
                              5,12,15,12, 5,
                              4, 9,12, 9, 4,
                              2, 4, 5, 4, 2};
    RealConvolution(src, temp, gaussianmask, width, height, 5, 5);
    matrixMultreal(temp, temp, 1.0/159.0, width, height);
/*
 *step2:sobel
 */
    if(sobel_size==3)
        Scharr(temp, edge_a, edge_d, width, height);
    else if(sobel_size==5||sobel_size==7)
        Sobel(temp, edge_a, edge_d, width, height,sobel_size);
/*
 *step3:Non_MaxSuppression
 */
    getEdgeDirection(edge_d, edge_d, width, height);
    Non_MaxSuppression(edge_a, temp, edge_d, width, height);
 /*
 *step4:double threshold
 */
    Threshold(temp, threshold_max, width, height, threshold1, MORETHAN);
    Threshold(temp, threshold_min, width, height, threshold2, MORETHAN);
    NonZeroSetOne(threshold_max,threshold_max,width,height);
    NonZeroSetOne(threshold_min,threshold_min,width,height);

    for(int j=0;j<height;j++){
        for(int i=0;i<width;i++){
            if(threshold_max[j*width+i]==1.0&&threshold_min[j*width+i]!=2.0){
                Position p;
                p.x=i;
                p.y=j;
                EdgeTrack(threshold_min, width, height, &p);
            }

        }
    }
/*
 *step5:result
*/
    Zero(dst, width, height);
    for(int i=0;i<width*height;i++)
        if(threshold_min[i]==2.0)
            dst[i]=255.0;
    free(temp);
    free(threshold_max);
    free(threshold_min);
    free(edge_d);
    free(edge_a);

}
```

## 实现结果
实验每步结果：
原图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212181924413.jpeg)

STEP1：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212181937158.jpeg)

STEP2：
Sobel梯度幅度结果：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212181953180.jpeg)

Sobel梯度方向结果：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212182006783.jpeg)

STEP3：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212182025625.jpeg)

STEP4：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212182315899.jpeg)


----------


原图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212182333621.jpeg)

STEP1：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212182345025.jpeg)

STEP2：
Sobel梯度幅度结果：

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212182356134.jpeg)

Sobel梯度方向结果：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212182416318.jpeg)

STEP3：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212182426040.jpeg)

STEP4：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-6-8-灰度图像-图像分割-Canny边缘检测/20150212182449655.jpeg)
## 总结
总结，Canny实现起来算法过程并不难，可能进一步优化加速就需要一些难度了，冈萨雷斯书中提到，第四步滞后阈值可以和第三步非极大值抑制放在一起。
待续。。。。
