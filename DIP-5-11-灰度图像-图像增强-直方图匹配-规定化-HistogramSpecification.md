---
title: 【数字图像处理】5.11:灰度图像-图像增强 直方图匹配（规定化）Histogram Specification"
date: 2015-02-04 10:16
categories:
  - DIP
keywords:
  - 直方图匹配
  - 直方图规定化
  - 增强对比度
toc: true
---
**Abstract:** 数字图像处理：第37天
**Keywords:** 直方图匹配,直方图规定化,增强对比度
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
# 开篇废话
开篇废话，本文应该是图像增强部分的最后一篇，直方图匹配（规定化）通俗一点说，就是人为规定输出图像的直方图，根据上文的说的均衡化的推倒过程，其中我们设定输出直方图为 $\frac{1}{L-1}$ 其实这就是规定化的，只是规定为一个常数，如果想要实现输出图像要根据输入的直方图产生，我们就需要使用直方图规定化，或直方图均衡，但如果直方图使用恒定，比如我们不想用常数，而是想用高斯，可以直接根据上文改一个高斯出来，这就省去了每次调用时都要人工产生直方图，更官方一点的话就是直方图匹配是直方图均衡的一般化，直方图均衡是直方图规定化的特例（当规定直方图为常数，例如都是1）。

# 数学原理
看一下原理，直方图匹配使用了直方图均衡做中间环节，将原图直方图和目标直方图进行均衡，然后互射，从原始图像直接映射到目标直方图均衡的结果，然后根据目标直方图均衡的逆映射，得到目标灰度值，示意图如下：
![Center][]
上面的示意图完整的表示了整个算法过程：

1. 计算原图的直方图Hr，输入目标直方图Hs
2. 均衡Hr，Hs得到映射 $G(r)$ 和 $Z(s)$
3. 得到最终映射为 $Z^{-1}(G(r))$
对于原图灰度r计算T为最终要得到的映射关系：

![Center 1][]
我们输入（规定）一个随机变量，具有如下性质：
![Center 2][]
根据上面两个式子，我们有：
![Center 3][]
那么就必须有：
![Center 4][]
这就是上面的算法过程的数学过程。
需要说明的是，实际操作因为离散的原因G(z)有可能不是满射的，也就是说G^-1可能会出现对应空值的情况，比如原始灰度a->均衡后灰度b->逆映射到目标是为空。为了防止这种情况产生大量0灰度结果，我们可以使用填充技术，如果逆映射为空，就用附近的灰度结果来填充。

# 代码
```c++
/********************************************************************************************
 直方图基本操作
 *******************************************************************************************/
void InitMappingTable(void * arry,int size,int Data_type){
    if(Data_type==TABLE_INT)
        for(int i=0;i<size;i++)
            ((int*)arry)[i]=0;
    else if(Data_type==TABLE_CHAR)
        for(int i=0;i<size;i++)
            ((char*)arry)[i]=0;
    else if(Data_type==TABLE_DOUBLE)
        for(int i=0;i<size;i++)
            ((double*)arry)[i]=0;

}
void InitHistogram(int *hist){
    for(int i=0;i<GRAY_LEVEL;i++)
        hist[i]=0;
}

void setHistogram(double *src,int *hist,int width,int height){
    InitHistogram(hist);
    for(int j=0;j<height;j++)
        for(int i=0;i<width;i++){
            int tempv=src[j*width+i];
            hist[tempv]++;
        }
}
int findHistogramMax(int *hist){
    for(int i=GRAY_LEVEL-1;i>=0;i--){
        if(hist[i]!=0)
            return i;
    }
    return -1;

}
int findHistogramMin(int *hist){
    for(int i=0;i<GRAY_LEVEL;i++){
        if(hist[i]!=0)
            return i;
    }
    return -1;
}
void fillMaptable(double * map){

    for(int i=1;i<GRAY_LEVEL;i++){
        if(map[i]==0)
            map[i]=map[i-1];

    }

}
/********************************************************************************************
 直方图归一化
 *******************************************************************************************/
void HistogramSpecification(double *src,double *dst,int* hist,int width,int height){
    int src_hist[GRAY_LEVEL];
    setHistogram(src, src_hist, width, height);
    double srcMap[GRAY_LEVEL];
    double histMap[GRAY_LEVEL];
    InitMappingTable(srcMap,GRAY_LEVEL,TABLE_DOUBLE);
    EqualizationHist(src_hist, srcMap);
    EqualizationHist(hist, histMap);
    int histMap_[GRAY_LEVEL];
    InitHistogram(histMap_);
    for(int i=0;i<GRAY_LEVEL;i++)
        histMap_[(int)histMap[i]]=i;
    double dstMap[GRAY_LEVEL];
    for(int i=0;i<GRAY_LEVEL;i++){
        dstMap[i]=histMap_[(int)srcMap[i]];
    }

    fillMaptable(dstMap);
    for(int i=0;i<width;i++)
        for(int j=0;j<height;j++)
            dst[j*width+i]=dstMap[(int)src[j*width+i]];
}
```

 

# 结果对比
原图：
![Center 5][]
原图直方图：
![Center 6][]
直方图匹配1：
![Center 7][]
目标直方图：
![Center 8][]
实际操作结果直方图：
![Center 9][]
直方图匹配2：
![Center 10][]
目标直方图：
![Center 11][]
实际操作结果直方图：
![Center 12][]
直方图匹配3：
![Center 13][]
目标直方图：
![Center 14][]
实际操作结果直方图：
![Center 15][]
直方图匹配4：
![Center 16][]
目标直方图：
![Center 17][]
实际操作结果直方图：
![Center 18][]
# 总结
直翻图匹配交直方图均衡使用更灵活，更能控制输出的灰度特性，主要优点就是更加自由可以自己设计目标，所以应用范围交直方图均衡更加广泛，当目标直方图设计为常数是，直方图匹配就是直方图均衡。
待续。。。



[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204092942843.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204093958904.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204094118730.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204095222744.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204095241652.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100939523.jpg
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100835749.png
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100026705.jpg
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100046361.png
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100748575.png
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100127107.jpg
[Center 11]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100144985.png
[Center 12]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100716049.png
[Center 13]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100201708.jpg
[Center 14]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100219306.png
[Center 15]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100551153.png
[Center 16]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100236605.jpg
[Center 17]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100437553.png
[Center 18]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification/20150204100628359.png





原文地址1：[https://www.face2ai.com/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification](https://www.face2ai.com/DIP-5-11-灰度图像-图像增强-直方图匹配-规定化-HistogramSpecification)转载请标明出处
