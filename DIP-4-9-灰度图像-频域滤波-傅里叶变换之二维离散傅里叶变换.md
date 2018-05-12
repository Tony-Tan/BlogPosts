---
title: 【数字图像处理】4.9:灰度图像-频域滤波 傅里叶变换之二维离散傅里叶变换
date: 2015-01-18 15:15
categories:
  - DIP
keywords:
  - 卷积
  - 图像傅里叶变换
toc: true
---
**Abstract:** 数字图像处理：第22天
**Keywords:** 卷积，图像傅里叶变换
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>


## 开篇废话
今天要记录的是二维离散傅里叶变换的一些性质，也是傅里叶在图像处理中要用到的一些性质，所以要重点学习下，并总结下。重点在性质。

## 二维DFT数学公式
根据一维离散傅里叶变换，直接给出二维DFT的公式，这个公式的具体代码实现在前面文章中有提到，但是实际中由于DFT计算速度过慢，普遍使用FFT作为快速算法，DFT和IDFT的公式如下：
![Center][]
![Center 1][]
二维DFT公式的推导过程与一维相同，对连续傅里叶变换进行采样或者是将原始数据周期复制后计算傅里叶级数，然后取主值，在此不再赘述。

## 性质

### 空间与频率关系
对于MxN的图像，![Center 2][]和![Center 3][]是空间的取样间隔，所以原函数的总长度是Mx![Center 2][]和 $Nx$ ![Center 3][]，频域间隔u，v为以下关系：
![Center 4][]
![Center 5][]
所以，频率域样本间隔与空域样本间隔成反比。
### 平移，旋转与中心化
将原图像进行平移或者对频域信号进行移动时，与其对应的频率和空域也会有相应的变换，变换关系如下：
![Center 6][]
可以看出，频率域原点移动到（u0，v0），对应的空域为原信号与后面复指数信号的乘积，这也是所有书上都要说的频谱中心化，也就是（u0，v0）=（M/2,N/2），这样就能得到：
![Center 7][]
上面的 $(-1)^{x+y}$ ，是 $\frac{M}{2},\frac{N}{2}$ 带入![Center 8][]得到的。
对于旋转特性，我们选择用极坐标表示二维笛卡尔坐标系，于是有以下变换性质：
![Center 9][]
得出结论，频谱与空域具有旋转同步性，即空域旋转多少频域就旋转多少，频域旋转空域就旋转多少。
证明实验如下：
![Center 10][]
原图
![Center 11][]
傅里叶变换后的频谱
![Center 12][]
旋转后的图片
![Center 13][]
旋转后的频谱
### 微分性质
微分性质，即一个信号的微分的傅里叶变换具有对高频成分的放大作用
$$
f'(x,y)<=>j\times u\times v\times F(u,v)
$$
所以得出微分可以计算边缘-这种高频成分的功能

### 周期性与对称性
DFT的周期性与离散周期信号的傅里叶级数一致，即原信号周期，频谱也周期，图像大小为MxN：
![Center 14][]
$k_1，k_2$ 为整数。
下面给出原始频谱和中心化后的频谱的图示，来自冈萨雷斯：
![Center 15][]
![Center 16][]
可以看出，频谱是周期排布的。
下图总结了其他一些对称的变换性质：
![Center 17][]
对应几个特殊的性质的解释：
![Center 18][]
![Center 19][]
## 傅里叶谱与相角
图像傅里叶转换后的结果是复数矩阵，因为复数没法用在图像中显示，所以在图像中我们看到的都是幅度，也就是结果实部和虚部的平方和的平方根，也就是复数的长度，但是复数还有另一个性质，也就是其相位，根据实验显示，相位存储了图像的轮廓信息，或者叫做机构信息，而幅度存储了灰度信息。下面来看一下实验：
首先，我们通过FFT得到lena的傅里叶变换结果，也就是等大的复数矩阵，我们将所有元素的长度归一化，也就是用每个像素的实部和虚部除以其距离，这样将得到幅度为1，但相角不同的复数矩阵，然后用IFFT进行逆运算，得到的计算结果在0.001左右，为了显示，将其扩大1000倍进行显示：
![Center 20][]
原图
![Center 21][]
相角重建
接下来考察幅度的影响，我们将所有元素的相角设为45，135，225，315度，也就是对所有的实部和虚部设为其距离除以根号2，这样得到的IFFT结果如下：
![Center 22][]
可见，幅度对结构信息没有影响。
代码：
```c++
    //将幅度归一，相角保持不变，图中Complex为复数数据
    void OneRange(Complex *src,Complex *dst,int width,int height){

        double realv=0.0,imaginv=0.0;
        for(int i=0;i<width*height;i++){
            realv=src[i].real;
            imaginv=src[i].imagin;
            double distance=sqrt(realv*realv+imaginv*imaginv);
            dst[i].real=realv/distance;
            dst[i].imagin=imaginv/distance;

        }

    }
    //将相角归一，相角保持不变，图中Complex为复数数据
    void OneAngel(Complex *src,Complex *dst,int width,int height){

        double realv=0.0,imaginv=0.0;
        for(int i=0;i<width*height;i++){
            realv=src[i].real;
            imaginv=src[i].imagin;
            double distance=sqrt(realv*realv+imaginv*imaginv);
            dst[i].real=distance/sqrt(2.0);
            dst[i].imagin=distance/sqrt(2.0);

        }

    }
```
## 二维卷积定理
卷积定义：
![Center 23][]
频域和空域的卷积关系：
![Center 24][]
卷积的一个问题在于卷积以后会对原始信号的周期进行一定长度的扩展，比如长度为A和B的信号的卷积，卷积结果的信号长度为A+B-1，所以对于周期信号，卷积后的结果有可能造成信号纠缠：
![Center 25][]
![Center 26][]
解决这个问题的方法很简单，就是对周期进行用零扩展，长度为A和B的信号卷积，将A和B都扩展成A+B-1，用0填充。f和h的图像进行卷积，首先进行扩展：
![Center 27][]

## 总结
至此对图像傅里叶变换的一些性质进行了简要介绍，并总结出下表，方便以后出现问题进行查阅：
![Center 28][]
![Center 29][]
![Center 30][]
下一篇介绍图像混淆

[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118141144636.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118141153013.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118141702018.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118141708336.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118141527591.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118141538168.png
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118142253019.png
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118142859713.png
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118143112656.png
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118143247234.png
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118153044782.jpeg
[Center 11]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118153040156.jpeg
[Center 12]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118153136512.jpeg
[Center 13]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118153135609.jpeg
[Center 14]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118144104755.png
[Center 15]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118144300820.png
[Center 16]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118144312598.png
[Center 17]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118145616983.png
[Center 18]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118145713158.png
[Center 19]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118145705437.png
[Center 20]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118145038415.jpg
[Center 21]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118145052736.jpg
[Center 22]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118145311857.jpg
[Center 23]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118145925203.png
[Center 24]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118150010812.png
[Center 25]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118150334345.png
[Center 26]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118150347948.png
[Center 27]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118150523748.png
[Center 28]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118150823819.png
[Center 29]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118150813406.png
[Center 30]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-9-灰度图像-频域滤波-傅里叶变换之二维离散傅里叶变换/20150118150847921.png
