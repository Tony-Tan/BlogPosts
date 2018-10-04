---
title: 【数字图像处理】5.6:灰度图像--图像增强 拉普拉斯算子
date: 2015-01-31 14:49
categories:
  - DIP
keywords:
  - 拉普拉斯算子
toc: true
---
**Abstract:** 数字图像处理：第32天
**Keywords:** 拉普拉斯算子
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
今天的废话是，锐化和后面的分割有很大的关系，所以决定把图像增强总结完以后开始说分割，分割中有很多又去的课题，比如边缘提取，形状识别等，具有挑战性，图像增强除了平滑和锐化还有灰度变换，但在很早以前已经写过灰度变换的一些例子了，所以，后面只简要的写下原理，周一就可以开始研究分割了，每当结束一个大分支的时候总有一种踏上新的征途的感觉，但不得不承认，之前所学习的一切只是基础知识，如果想深入的话可能要花费很长时间，尤其是图像处理这么复杂的多学科领域，新技术日新月异，但只要打好基础，一定会在以后从事的领域中大展拳脚，希望与因为爱好而从事图像处理，计算机视觉的攻城狮们共同进步。
## 数学基础
拉普拉斯算子，二阶微分线性算子，为什么上来就学二阶微分算子，前文说过，与一阶微分相比，二阶微分的边缘定位能力更强，锐化效果更好，所以我们来先学习二阶微分算子，使用二阶微分算子的基本方法是定义一种二阶微分的离散形式，然后根据这个形式生成一个滤波模板，与图像卷积。
各向同性滤波器，图像旋转后响应不变，这就要求滤波模板自身是对称的，如果不对称，结果就是，当原图旋转90°时，原图某一点能检测出细节（突变）的，现在却检测不出来，这就是各向异性的原因。我们更关心的是各向同性滤波模板，对图像的旋转不敏感。
对于二维图像 $f(x,y)$ ，二阶微分最简单的定义--拉普拉斯算子定义为：

![Center][]
对于任意阶微分算子都是线性算子，所以二阶微分算子和后面的一阶微分算子都可以用生成模板然后卷积的方式得出结果。
根据前面对二阶微分的定义有：
![Center 1][]
![Center 2][]
根据上面的定义，与拉普拉斯算子的定义相结合，得到：
![Center 3][]
也就是一个点的拉普拉斯的算子计算结果是上下左右的灰度的和减去本身灰度的四倍。同样，可以根据二阶微分的不同定义，所有符号相反，也就是上式所有灰度值全加上负号，就是-1，-1，-1，-1，4。但要注意，符号改变，锐化的时候与原图的加或减应当相对变化。上面是四邻接的拉普拉斯算子，将这个算子旋转45°后与原算子相加，就变成八邻域的算子了，也就是一个像素周围一圈8个像素的和与中间像素8倍的差，作为拉普拉斯计算结果。
因为要强调图像中突变（细节），所以平滑灰度的区域，无响应，即模板系数的和为0，也是二阶微分必备条件。
最后的锐化公式：
![Center 4][]
g是输出，f为原始图像，c是系数，也就是要加上多少细节的多少，与上一篇的锐化过程是一致的，先提取细节，然后再加（或者减去负细节）到原图中。
得到滤波模板，此模板尺寸恒定，就是3x3，不像平滑模板，尺寸可变：
![Center 5][]

## 代码
```c++
void Laplace(double *src,double *dst,int width,int height,int mask_type){
    double LaplaceMask0[9]={0,1,0,1,-4,1,0,1,0};
    double LaplaceMask1[9]={1,1,1,1,-8,1,1,1,1};
    double LaplaceMask2[9]={0,-1,0,-1,4,-1,0,-1,0};
    double LaplaceMask3[9]={-1,-1,-1,-1,8,-1,-1,-1,-1};
    switch(mask_type){
        case SHARPEN_LAP_0:
            RealRelevant(src, dst, LaplaceMask0, width, height, LAPLACE_MASK_SIZE,LAPLACE_MASK_SIZE);
            break;
        case SHARPEN_LAP_1:
            RealRelevant(src, dst, LaplaceMask1, width, height, LAPLACE_MASK_SIZE,LAPLACE_MASK_SIZE);
            break;
        case SHARPEN_LAP_2:
            RealRelevant(src, dst, LaplaceMask2, width, height, LAPLACE_MASK_SIZE,LAPLACE_MASK_SIZE);
            break;
        case SHARPEN_LAP_3:
            RealRelevant(src, dst, LaplaceMask3, width, height, LAPLACE_MASK_SIZE,LAPLACE_MASK_SIZE);
            break;
        default:
            printf("wrong mask type\n");
            matrixCopy(src, dst, width, height);
            break;
    }



}
void LaplaceSharpen(double *src,double *dst,int width,int height,int mask_type,double c){


    Laplace(src,dst,width,height,mask_type);
    matrixMultreal(dst,dst,c,width,height);

    switch(mask_type){
        case SHARPEN_LAP_0:
        case SHARPEN_LAP_1:
            matrixSub(src,dst,dst,width,height);
            break;
        case SHARPEN_LAP_2:
        case SHARPEN_LAP_3:
            matrixAdd(src,dst,dst,width,height);
            break;
        default:
            printf("wrong mask type\n");
            matrixCopy(src, dst, width, height);
            break;
    }
}
```
## 结果
来看对一副月球图片的锐化效果，分别使用四种模板，其结果是四邻接的两个模板结果相同，八邻接的结果相同，观察两个不同的系数，分别c=0.5和c=0.8，具体系数已经标注在图片上了：
原图：
![Center 6][]
原图灰度图像：
![Center 7][]
细节提取：
![Center 8][]
![Center 9][]
![Center 10][]
![Center 11][]
锐化图像：
![Center 12][]
![Center 13][]
![Center 14][]
![Center 15][]
![Center 16][]
![Center 17][]
![Center 18][]
![Center 19][]
## 总结
总结就是，二阶微分拉普拉斯算子对图像的锐化效果很不错，但对噪声敏感，后面介绍非锐化掩蔽，和一阶微分算子


[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131135408892.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131135715051.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131135725518.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131135933220.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131140714026.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131142008177.png
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131143951412.png
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144013096.jpg
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144029815.png
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144115262.png
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144127055.png
[Center 11]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144555794.png
[Center 12]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144006837.png
[Center 13]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144208536.png
[Center 14]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144226304.jpg
[Center 15]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144206021.jpg
[Center 16]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144301092.png
[Center 17]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144243711.png
[Center 18]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144403099.jpg
[Center 19]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-6-灰度图像-图像增强-拉普拉斯算子/20150131144304927.jpg





原文地址1：[https://www.face2ai.com/DIP-5-6-灰度图像-图像增强-拉普拉斯算子](https://www.face2ai.com/DIP-5-6-灰度图像-图像增强-拉普拉斯算子)转载请标明出处
