---
title: 【数字图像处理】5.10:灰度图像--图像增强 直方图均衡化（Histogram Equalization)
date: 2015-02-03 15:12
categories:
  - DIP
keywords:
  - 直方图均衡
toc: true
---
**Abstract:** 数字图像处理：第36天
**Keywords:** 直方图均衡
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
废话开始，图像处理这些代码已经有三千多行了，不多，但是感觉多加练习以后对算法理解和写代码的能力上都有很大提高，毕竟对于算法来说想明白了一定要用一下才会真正掌握，但不能靠记忆去记住一个算法，这就需要我们懒人的天性，不愿意记住完整的公式，更愿意记住一个简单的起始，通过自己的理解和数学推导算法，这是个很不错的 方法，而写代码属于一种技术工作，熟能生巧，要多加练习，并且也要思考其中的技术细节，总之，做一切事情思考下还是不错的。
废话完成，说说直方图均衡，在冈萨雷斯的书里面直方图均衡化在第三章提出，因为之前想按照书上目录上的顺序来写着一些列的博客，后来发现还是自己总结下的学习思路，按照自己理解的知识网络来走，所以刚要写直方图均衡的时候就是转向自己的节奏开始按照二值图像，灰度图像，彩色图像的知识结构介绍。
直方图均衡的目的和前面灰度变换一样，为了增强对比度，使图像的灰度分布在整个灰度范围内更加均衡，其中直方图需要来解释下，直方图是个统计概念，比如我们有十种颜色的球，每种颜色的球有不同的数量，假设颜色分布为a0~a9，数量为n（x）（x取值为a0~a9）那么直方图就是以a0~a9为横坐标，n为纵坐标的统计直方图：

![Center][]
如果将上图中的数据归一化（每个分量除以球数总和），也就是使得各分量总和为1，各个分量就表示这种颜色的球出现的频率，就得到频率直方图。
如果将各种颜色换成各灰度值，球的个数等效的换成具有该灰度的像素数量，或者换成该灰度出现的频率，就成了图像的直方图，对于彩色图像和灰度图像，直方图具有重要的统计意义，而对于二值图像来说，该意义不大，因为二值图像就两个灰度，所以其只能反映黑白面积比例。
直方图均衡的目的是为了使灰度分布的更广泛，从而来拉伸对比度：

![Center 1][]
事实表明当灰度的直方图范围从上面的左边变换成右边后，图像对比度得到提升，也就达到了我们增强图像的目的--更便于观察，更容易区分不同灰度间细节。
## 数学原理
直方图变换的最终操作和前面提到的灰度操作是一样的，即：
![Center 2][]
这是一种从灰度到灰度的映射，并且该映射与前面伽马变换对数变换的映射不同的是，它不具有确定的表达公式，而是根据原始图像的灰度分布不同而“自适应”的产生映射，并且必须具有以下两个性质：

1. 该函数必须单调递增（因为要从s反射回r所以该函数必须是严格的单调递增，即r和s为一对一的关系）
2. $0\leq r\leq L-1$ 时，必须满足$0\leq s \leq L-1$
这是两点约束，即我们找到的T必须使得上面成立，而且还要达到均衡直方图的目的。即完成下面的转化：
![Center 3][]
上面的是直方图的原始分布和目标分布，下面是响应的T。

推导过程：
先提出一个概率密度函数pdf的概念，就是每个灰度值对应出现的概率，用p表示，pr（r）表示原始灰度r出现的概率，其计算是用灰度值为r的像素总个数除以图像的像素总个数。同理变换后的ps（s）表示变换后灰度为s的像素的概率。
1. r到s是一对一的映射，所以若r0映射到s0那么pr（r0）=ps（s0）这个式子是因为像素个数不会改变，只是对应的灰度值改变了，这个就是我们接下来要用到的最基本的原理。
2. 我们的目标灰度分布是均匀分布，也就是上图右上的概率分布图，因为绿色部分面积必然为1，所以，ps的目标分布为：     ![Center 4][]
根据上述的基本原理和假设存在：
![Center 5][]
根据积分定理，w为积分假变量，那么：
![Center 6][]
所以：
![Center 7][]
将上面离散化：
![Center 8][]
上面为大概的公式推导，如有不严谨之处还请指出。
所以我们将按照上面得出的结论进行编程：
![Center 9][]
## 代码


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
 直方图均衡
 *******************************************************************************************/
//均衡直方图，将原图直方图，经过公式得到目标直方图
void EqualizationHist(int *src_hist,double *dst_map){
    int temphist[GRAY_LEVEL];
    InitHistogram(temphist);
    int max=findHistogramMax(src_hist);
    int min=findHistogramMin(src_hist);
    temphist[min]=src_hist[min];
    for(int i=min+1;i<=max;i++)
        temphist[i]=temphist[i-1]+src_hist[i];
    for(int i=max;i>=min;i--)//感谢pymess同学指正，之前的有问题
        temphist[i]-=temphist[min];
    int total=temphist[max];
    for(int i=min;i<=max;i++){
        dst_map[i]=((double)GRAY_LEVEL-1.0)*temphist[i]/total;
    }

}
//直方图均很，用输入图像得到输出图像
void HistogramEqualization(double *src,double *dst,int width,int height){
    int hist[GRAY_LEVEL];
    setHistogram(src, hist, width, height);
    double GrayMappingTable[GRAY_LEVEL];
    InitMappingTable(GrayMappingTable,GRAY_LEVEL,TABLE_DOUBLE);
    EqualizationHist(hist, GrayMappingTable);
    for(int i=0;i<width;i++)
        for(int j=0;j<height;j++)
            dst[j*width+i]=GrayMappingTable[(int)src[j*width+i]];

}
```
## 结果
原图：
![Center 10][]
原图直方图：
![Center 11][]
直方图均衡后图片：
![Center 12][]
直方图均衡后直方图：
![Center 13][]
## 总结
上面给出的结果为经典结果，很多文章都使用的这幅图片，值得解释的是，虽然我们从r到s的映射是一对一的，但r和s是离散的整数，如果s被映射到非整数，将就近取整，所以有些灰度值会被合并。
直方图运算速度快，效果好，应用范围很广，故总结如上。
待续。。。


[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203140744870.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203141746095.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203142337502.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203143359875.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203145321801.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203145404166.png
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203145538188.png
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203145705771.png
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203145754490.png
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203145939848.png
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203150424927.jpg
[Center 11]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203150504286.png
[Center 12]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203150532491.jpg
[Center 13]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization/20150203150553115.png





原文地址1：[https://www.face2ai.com/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization](https://www.face2ai.com/DIP-5-10-灰度图像-图像增强-直方图均衡化HistogramEqualization)转载请标明出处
