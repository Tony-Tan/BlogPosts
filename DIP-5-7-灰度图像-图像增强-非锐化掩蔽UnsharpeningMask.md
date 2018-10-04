---
title: 【数字图像处理】5.7:灰度图像-图像增强 非锐化掩蔽 （Unsharpening Mask）
date: 2015-01-31 19:53
categories:
  - DIP
keywords:
  - 非锐化掩蔽(USM)
toc: true
---
**Abstract:** 数字图像处理：第33天
**Keywords:** 非锐化掩蔽(USM)
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
废话开始，今天写了两篇博客，为了加快学习进度，而且是周末，相当于给自己加个班，而且理论之前已经研究明白了，所以写起来也比较自如。有人在群里问，学习图像处理，要看书还是要写代码还是要作项目，我觉得，看书像是内功心法，写代码相当于招式练习，项目就像实战一样，所以如果只写代码或者一上来就去跟别人做项目而完全不看书研究基础算法就像空中楼阁，美丽但不牢固，更重要的是容易迷失自己，也就是走火入魔，个人观点，本人也在基础练习阶段，所以说这些没什么经验依据，只是自己的理解。
非锐化掩蔽，一开始感觉这个词好难接受，不知道要干嘛，google之发现线索不多，经过一番研究发现这个词这么理解：非锐化--锐化的相反的操作是平滑，所以非锐化就是平滑操作；掩蔽--字面意思是隐藏，其实我们可以把它理解成为减去除去，所以这个过程就是减去平滑后的图像得到的结果。而实际算法的思路是，原图减去平滑后的图像，得到被削弱的边缘部分，然后按照一定比例和原图相加，如果比例为1，那么就是非锐化掩蔽，如果大于1就是高提升滤波，和前面频率域的高提升，高频强调思路一致，只是那部分用的是频率域方法。

## 数学原理
数学原理与前面的锐化原理基本保持一致，只是在确定细节的方法上有些不同：
1. 生成模板，$\bar{f}$ 表示 $f$ 的平滑结果：
![Center][]
2. 钝化模板按照一定比例与原图相加：
![Center 1][]
其中 $k=1$ 时为非锐化掩蔽。
$k>1$时为高提升滤波。
## 示意图
观察示意图：
![Center 2][]
算法基本步骤：

1. 平滑图像，边缘和突变被消减，而平滑部分不变（此处不适合采用边缘保持性好的平滑算法，代码中使用了均值滤波和高斯滤波）。
2. 原图像与平滑后图像相减，得到钝化模板。
3. 钝化模板的k倍与原图相加得到锐化结果。
值得注意的是，因为钝化模板有负值，所以，如果k选择过大的时候有可能图像产生负值
![Center 3][]


## 代码

```c++
void UnsharpMasking(double *src,double *dst,int width,int height,int smooth_type,int smooth_mask_width,int smooth_mask_height,double gaussian_deta,double k){
    switch (smooth_type) {
        case SMOOTH_GAUSSIAN:
            GaussianFilter(src, dst,width,height, smooth_mask_width, smooth_mask_height,gaussian_deta);
            break;
        case SMOOTH_MEAN:
            MeanFilter(src, dst,width,height, smooth_mask_width, smooth_mask_height);
            break;
        default:
            break;
    }
    matrixSub(src, dst, dst, width, height);
    matrixMultreal(dst, dst, k, width, height);
    matrixAdd(src, dst, dst, width, height);
}
```

## 结果
实验结果，分别采用高斯滤波和均值滤波，作为平滑算法。
原图：
![Center 4][]
高斯滤波：
$5\times 5$ ，$\sigma=1$ ，$k=1$ ：
钝化模板：
![Center 5][]
锐化结果：
![Center 6][]
$5\times 5$ ，$\sigma=1$ ，$k=2$：
锐化结果：
![Center 7][]
$5\times 5$ ，$\sigma=2$ ，$k=1$：
钝化模板：
![Center 8][]
锐化结果：
![Center 9][]
$5\times 5$ ，$\sigma=2$ ，$k=2$
锐化结果：
![Center 10][]
$7\times 7$ ，$\sigma=1$ ，$k=1$
钝化模板：
![Center 11][]
锐化结果：
![Center 12][]
$7\times 7$ ，$\sigma=1$ ，$k=2$
锐化结果：
![Center 13][]
$7\times 7$ ，$\sigma=2$ ，$k=1$
钝化模板：
![Center 14][]
锐化结果：
![Center 15][]
$7\times 7$ ，$\sigma=2$ ，$k=2$：
锐化结果：
![Center 16][]
均值
5x5，k=1：
钝化模板：
![Center 17][]
锐化结果：
![Center 18][]
5x5，k=2：
锐化结果：
![Center 19][]
7x7，k=1：
钝化模板：
![Center 20][]
锐化结果：
![Center 21][]
7x7，k=2：
锐化结果：
![Center 22][]

## 总结
观察上面结果，发现：
1. k越大对细节增强越明显。
2. 模板越大会导致增细节被增强的越宽，所以模板大小要适度。
3. 被平滑消除的边缘越多，锐化后增强的越多。
下一篇介绍下一阶微分算子
待续。。。



[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131191926698.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131191948722.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131192302681.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131193027406.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131195559575.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131193654958.jpg
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131193714208.jpg
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131193705422.jpg
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131193752818.jpg
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131193742612.jpg
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131193814717.jpg
[Center 11]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194310450.jpg
[Center 12]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194329311.jpg
[Center 13]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194315486.jpg
[Center 14]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194402866.jpg
[Center 15]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194421883.jpg
[Center 16]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194427199.jpg
[Center 17]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194548229.jpg
[Center 18]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194538600.jpg
[Center 19]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194555994.jpg
[Center 20]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194611438.jpg
[Center 21]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194703562.jpg
[Center 22]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask/20150131194721673.jpg





原文地址1：[https://www.face2ai.com/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask](https://www.face2ai.com/DIP-5-7-灰度图像-图像增强-非锐化掩蔽UnsharpeningMask)转载请标明出处
