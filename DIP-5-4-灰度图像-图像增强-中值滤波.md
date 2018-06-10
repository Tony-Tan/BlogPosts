---
title: 【数字图像处理】5.4:灰度图像-图像增强 中值滤波
date: 2015-01-30 15:26
categories:
  - DIP
tags:
  - 图像平滑
  - 中值滤波
toc: true
---
**Abstract:** 数字图像处理：第30天
**Keywords:** 图像平滑，中值滤波
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
开篇废话是中值滤波原理和基础代码都很好理解和编写，但是快速算法有点不好写，上周日看了下算法大概的思路然后就开始写，写了一天也不好，周一又调试了一上午，后来发现整个算法其实原理上没有任何难点，只是自己还没完全清楚每个细节就开始写代码，而且算法又是迭代的，一步有问题后面结果都不对，所以，下次再实现算法的时候，要先想一段时间，然后再实现，这样不仅不浪费时间反而可以节约时间，并且不会受到打击。

## 中值滤波介绍
中值滤波时典型的非线性方法，与前面介绍的方法不同，中值滤波更接近于灰度图像的腐蚀和膨胀，是在一定区域内比较大小，找出中值，也就是排序后中间那个数，也就是中学的中位数，平均数用于均值滤波，中位数用于中值滤波，要是专家就可以写本书：统计学在图像处理中的二三事（这句话属于扯淡）。
中值滤波会产生对原始图像的人为因素破坏，所以在医疗成像等对人为因素引起误差不能够接受的时候不能是用中值滤波。
中值滤波对椒盐噪声和斑点噪声效果显著，而且中值滤波具有较好的边缘保持特性，所以在图像处理中知名度很高。
## 数学原理
中值滤波的数学公式就是:
$$
g(x,y)=Median\{f(x-m/2,y-n/2)\dots f(x+m/2,y+n/2)\}
$$
翻译成自然语言就是在当前模板覆盖范围内需找中位数作为结果。
此计算中涉及到排序，所以，如果使用比较的方法排序，最快速度的复杂度是 $O(m\times n\times log(m\times n)\times W\times H)$ ，如果使用非比较型排序，算法最大时间复杂度是 $255\times W\times H$ 也就是 $O(W\times H)$ 但要使用更多的存储空间，最直观的方法是使用计数排序，建立一个255大小的空间，或者理解为一个直方图，来查找中值。
快速方法是使用计数排序，但存在一个类似于游标的指针，指向当前的中值，并记录当前模板覆盖范围内小于中值的数据的个数，当模板滑动的时候，观察移出数据和移入数据对小于中值个数的影响确定移动游标的方向，以此来减少直方图搜索范围，降低运算量。
## 快速算法
1. 初始化 $T=模板覆盖区域元素个数/2$
2. 首先，初始化直方图，将该行第一组模板覆盖的元素排序，找出中值 mid_value ，记录小于此中值的元素个数c。
3. 移出模板覆盖区域最左侧一列元素，对于每一个元素如果小于 mid_value ，$c=c-1$ ;
4. 移入模板覆盖外最右侧一列元素（相当于模板右移）， 对于每一个元素如果小于 mid_value ，$c=c+1$ ;
5. 比较c和T的大小：
 *  如果 $c<T$ :从 mid_value 开始，包括mid_value，向更大的方向检索，如果有搜索到元素，c加上对应的个数，直到 $c\geq T$ ，当前元素为新的 mid_value；
 *  如果 $c==T$ ：从mid_value开始，包括mid_value，向更大的方向检索，如果有搜索到元素，该元素为新的mid_value;
 *  如果 $c>T$ ：从mid_value开始，包括mid_value，向更小的方向检索，如果有搜索到元素，c减去对应的个数，直到c<=T，当前元素为新的mid_value，c的值加上新mid_value的个数；
6. 如果模板右边无数据，到下一行，回到第2步否则回到第3步，继续；
# 原型算法代码 #

```c++
//以下为低速普通中值滤波，排序使用计数排序
 void initHist(int *hist,int size){
    for(int i=0;i<size;i++)
        hist[i]=0;
}
int sort(int *src,int size){
    int hist[GRAY_LEVEL];
    int m=0;
    initHist(hist, GRAY_LEVEL);
    for(int i=0;i<size;i++)
        hist[src[i]]++;
    for(int i=0;i<GRAY_LEVEL;i++){
        m+=hist[i];
        if(m>size/2)
            return i;

    }
    return 0;
}
void MedianFilter(IplImage *src,IplImage *dst,int width,int height){
    IplImage* temp=cvCreateImage(cvSize(src->width+width, src->height+height), src->depth, src->nChannels);
    IplImage* dsttemp=cvCreateImage(cvSize(src->width+width, src->height+height), src->depth, src->nChannels);
    cvZero(temp);
    for(int j=0;j<src->height;j++){
        for(int i=0;i<src->width;i++){
            double value=cvGetReal2D(src, j, i);
            cvSetReal2D(temp, j+height/2, i+width/2, value);

        }
    }
    int *window=(int *)malloc(sizeof(int)*width*height);
    if(window==NULL){
        printf(" ");
        exit(0);

    }
    for(int j=height/2;j<temp->height-height/2-1;j++){
        for(int i=width/2;i<temp->width-width/2-1;i++){
            for(int n=-height/2;n<height/2+1;n++)
                for(int m=-width/2;m<width/2+1;m++){
                    window[(n+height/2)*width+m+width/2]=cvGetReal2D(temp, j+n, i+m);
                }
            double pix=sort(window,width*height);
            cvSetReal2D(dsttemp, j, i, pix);
        }
    }
    for(int j=height/2;j<temp->height-height/2-1;j++){
        for(int i=width/2;i<temp->width-width/2-1;i++){
            double value=cvGetReal2D(dsttemp, j, i);
            cvSetReal2D(dst, j-height/2, i-width/2, value);

        }
    }
    free(window);

}
```

## 快速算法代码
```c++
int findMedian(int *hist,int *movein,int *moveout,int movesize,int *cursor,int median,int t){
    for(int i=0;i<movesize;i++){
        hist[movein[i]]++;
        hist[moveout[i]]--;
        if(movein[i]<median)
            (*cursor)++;
        if(moveout[i]<median)
            (*cursor)--;
    }


    if((*cursor)<t){
        for(int i=median;i<GRAY_LEVEL;i++){
            (*cursor)+=hist[i];
            if(*cursor>=t+1){
                (*cursor)-=hist[i];
                return i;
            }
        }
    }else if((*cursor)>t){
            for(int i=median-1;i>=0;i--){
                (*cursor)-=hist[i];
                if(*cursor<=t){
                    return i;
                }
            }
        }
    else if ((*cursor)==t){
            for(int i=median;i<GRAY_LEVEL;i++){
                if(hist[i]>0)
                    return i;
        }

    }
    return -1;
}
//初始化一行
int InitRow(IplImage *src,int *hist,int row,int *cursor,int win_width,int win_height){
    int t=win_width*win_height/2+1;
    *cursor=0;
    for(int i=0;i<GRAY_LEVEL;i++)
        hist[i]=0;
    for(int j=-win_height/2;j<win_height/2+1;j++)
        for(int i=0;i<win_width;i++){
            int pixvalue=cvGetReal2D(src, j+row, i);
            hist[pixvalue]++;
        }
    for(int i=0;i<GRAY_LEVEL;i++){
        *cursor+=hist[i];
        if(*cursor>=t){
            *cursor-=hist[i];
            return i;

        }
    }
    return -1;

}

void MedianFilter(IplImage *src,IplImage *dst,int width,int height){
    int hist[GRAY_LEVEL];
    int median;
    int *movein=(int *)malloc(sizeof(int)*height);
    int *moveout=(int *)malloc(sizeof(int)*height);
    double *dsttemp=(double *)malloc(sizeof(double)*src->width*src->height);
    int t=width*height/2;
    for(int j=height/2;j<src->height-height/2-1;j++){
        int cursor=0;
        median=InitRow(src, hist, j, &cursor, width, height);
        dsttemp[j*src->width+width/2]=median;
        for(int i=width/2+1;i<src->width-width/2-1;i++){
            for(int k=-height/2;k<height/2+1;k++){
                movein[k+height/2]=cvGetReal2D(src, j+k, i+width/2);
                moveout[k+height/2]=cvGetReal2D(src, j+k, i-width/2-1);
            }
            median=findMedian(hist, movein, moveout, height, &cursor, median, t);
            dsttemp[j*src->width+i]=median;
        }
    }
    for(int j=0;j<src->height;j++){
        for(int i=0;i<src->width;i++){
            cvSetReal2D(dst, j, i, dsttemp[j*src->width+i]);

        }
    }
    free(dsttemp);
    free(movein);
    free(moveout);
}
```




## 效果
来观察下lena图矩阵原版的中值滤波结果：
原图数据：
![Center][]
我们的慢速结果：
![Center 1][]
我们的快速结果：
![Center 2][]
OpenCV的结果：
![Center 3][]
下面看加了椒盐噪声的lena图的中值滤波和高斯滤波的效果：
![Center 4][]
3x3中值：
![Center 5][]
3x3高斯：
![Center 6][]
5x5中值：
![Center 7][]
5x5高斯：
![Center 8][]
7x7中值：
![Center 9][]
7x7高斯：
![Center 10][]
观察结果：对于椒盐噪声影响严重的图片，中值滤波效果远远好于高斯滤波，中值滤波的模板越大图像被模糊的越严重
## 总结
图像增强基础的平滑算法就介绍到这里，下一篇开始介绍锐化相关



[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-4-灰度图像-图像增强-中值滤波/20150130151626067.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-4-灰度图像-图像增强-中值滤波/20150130151630136.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-4-灰度图像-图像增强-中值滤波/20150130151723491.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-4-灰度图像-图像增强-中值滤波/20150130151714393.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-4-灰度图像-图像增强-中值滤波/20150130151852907.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-4-灰度图像-图像增强-中值滤波/20150130152029256.jpg
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-4-灰度图像-图像增强-中值滤波/20150130152014667.jpg
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-4-灰度图像-图像增强-中值滤波/20150130152031655.jpg
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-4-灰度图像-图像增强-中值滤波/20150130152046740.jpg
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-4-灰度图像-图像增强-中值滤波/20150130152102372.jpg
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-5-4-灰度图像-图像增强-中值滤波/20150130152148504.jpg
