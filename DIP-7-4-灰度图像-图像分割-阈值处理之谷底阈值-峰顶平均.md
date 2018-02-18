---
title: 【数字图像处理】7.4:灰度图像-图像分割 阈值处理之谷底阈值、峰顶平均
date: 2015-03-07 16:19
categories:
  - 数字图像处理
tags:
  - 阈值处理
  - 直方图阈值法
toc: true
---
## 开篇废话
废话开始，这篇介绍两种基于直方图的方法，前面介绍的几种阈值处理方法，可以使用直方图作为处理工具，也可以不使用直方图，直接操作图像也可以，不过建议使用直方图，因为直方图只进行一次计算，免去后续多次的访问全图像素。
今天介绍的算法有意个前提条件，就是直方图必须是一个双峰图，我们通过找到双峰之间的谷底，或者双峰值的平均值作为阈值。
其次是对直方图进行加工，怎么加工？平滑，通过对直方图的平滑使不标准的直方图变得平滑，最终达到双峰的效果。
## 算法描述
算法过程详解
1. 计算图像直方图
2. 使用$\frac{1}{4}[1,2,1]$平滑直方图，直到直方图平滑成双峰图。
3. 计算直方图的一阶导数，按照符号（正负）将一阶导数“格式化”--一阶导数大于0的设置为1，等于0的设置为0，小于0的设置为-1
4. 寻找峰顶和谷底，从1 -> 0 -> -1的为峰顶从-1 -> 0 -> 1的为谷底
5. 谷底为阈值，或者使用双峰的峰值的平均值作为阈值，进行阈值处理。
解释下第2步，如何判断是否是双峰，同样使用第3步的方法，得出“格式化”的直方图，如果整个“格式化”直方图的模式是：
1，1，1，1，1，1，1，0,（0，0，0，0）-1，-1，-1，-1，-1，-1，-1，**0（0，0，0，0）**,1，1，1，1，1，1，1，1，1，0（0，0，0，0）-1，-1，-1，-1，-1，-1，-1
满足这种模式的直方图就是一个双峰图，否则不是。粗体处为谷底。
本算法难度不大，所以直接上代码。
## 代码实现
```c++
/*********************************************************************************/
/*********************************************************************************/
/*双峰型直方图，经过直方图平滑后呈现出双峰后找出谷底，以此值为阈值划分灰度值
 *平滑直方图采用1/4[1 2 1]的模板
 *判断是否是双峰采用1阶微分，判断正负性
 *直方图非双峰不能使用该方法。
 */

//直方图从int转换为double
void Hist_int2double(int *hist,double *hist_d){
    for(int i=0;i<GRAY_LEVEL;i++)
        hist_d[i]=(double)hist[i];
}
//平滑直方图，是指呈现双峰形状
void SmoothHist(double *hist,double *dsthist){
    double *histtemp=(double *)malloc(sizeof(double)*GRAY_LEVEL);
    histtemp[0]=0.0;
    histtemp[GRAY_LEVEL]=0.0;
    for(int i=0;i<GRAY_LEVEL;i++)
        histtemp[i]=hist[i];
    for(int i=1;i<GRAY_LEVEL-1;i++){
        histtemp[i]=0.25*histtemp[i-1]+0.5*histtemp[i]+0.25*histtemp[i+1];
    }
    for(int i=0;i<GRAY_LEVEL;i++)
        dsthist[i]=histtemp[i];
    free(histtemp);
}
//判断是否是双峰直方图，如果是返回谷底，否则返回0
//#define DOUBLEHUMP_BOTTOM 1
//#define DOUBLEHUMP_MEANHUMP 2
int isDoubleHump(double *hist,int returnvalue){
    double * diffHist=(double *)malloc(sizeof(double)*GRAY_LEVEL);
    int * statusHist=(int *)malloc(sizeof(int)*GRAY_LEVEL);
    diffHist[0]=0.0;
    diffHist[GRAY_LEVEL-1]=0.0;
    for(int i=1;i<GRAY_LEVEL-1;i++){
        diffHist[i]=hist[i+1]-hist[i];
    }
    for(int i=1;i<GRAY_LEVEL;i++){
        if(diffHist[i]>0)
            statusHist[i]=1;
        else if(diffHist[i]<0)
            statusHist[i]=-1;
        else if(diffHist[i]==0&&statusHist[i-1]>=0)
            statusHist[i]=1;
        else if(diffHist[i]==0&&statusHist[i-1]<0)
            statusHist[i]=-1;
    }
/*1st order:
 *______________                ________________
 *              |              |
 *              |              |
 *              |______________|
 *status:       1             -1
 *hist:
 *0 0 0 0 0 0 0 1 0 0 0 0 0 0 -1 0 0 0 0 0 0 0 0
 */
    for(int i=1;i<GRAY_LEVEL-1;i++)
        if(statusHist[i]*statusHist[i+1]<0){
            if(statusHist[i]>0)
                statusHist[i]=1;
            else if(statusHist[i]<0)
                statusHist[i]=-1;
        }else{
            statusHist[i]=0;
        }
    statusHist[GRAY_LEVEL-1]=0;
 /*double hump diff:
  *______________                _______________
  *              |              |               |
  *              |              |               |
  *              |______________|               |______________
  *status:       1             -1               1
  *             top           bottom           top
  *0 0 0 0 0 0 0 1 0 0 0 0 0 0 -1 0 0 0 0 0 0 0 1 0 0 0 0
  *the arry test store nonzero
  */
    int test[4]={0,0,0,0};
    int test_num=0;
    for(int i=1;i<GRAY_LEVEL-1;i++){
        if(statusHist[i]!=0){
            test[test_num]=statusHist[i];
            if(test_num>=3){
                free(diffHist);
                free(statusHist);
                return 0;
            }
            test_num++;
        }
    }

    if(test_num==3&&test[0]==1&&test[1]==-1&&test[2]==1){
        if(returnvalue==DOUBLEHUMP_BOTTOM){
            for(int i=1;i<GRAY_LEVEL;i++)
                if(statusHist[i]==-1){
                    free(diffHist);
                    free(statusHist);
                    return i;
                }
        }else if(returnvalue==DOUBLEHUMP_MEANHUMP){
            int hump[2];
            for(int i=0,k=0;i<GRAY_LEVEL;i++)
                if(statusHist[i]==1){
                    hump[k]=i;
                    k++;
                }
            free(diffHist);
            free(statusHist);
            return (hump[0]+hump[1])/2;
        }
    }
    free(diffHist);
    free(statusHist);
    return 0;
}
//谷底法阈值分割，适用于直方图是双峰的。
void ValleyBottomThreshold(double *src,double *dst,int width,int height,int type){
    int *hist=(int *)malloc(sizeof(int)*GRAY_LEVEL);
    double *hist_d=(double *)malloc(sizeof(double)*GRAY_LEVEL);
    setHistogram(src, hist, width, height);
    Hist_int2double(hist, hist_d);
    double threshold=0.0;
#define MAXLOOP 1000
    for(int i=0;i<MAXLOOP;i++){
        SmoothHist(hist_d, hist_d);
        if(0.0!=(threshold = (double)isDoubleHump(hist_d,DOUBLEHUMP_BOTTOM))){
            Threshold(src, dst, width, height, threshold, type);
            printf("smooth times:%d   threshold:%g\n",i,threshold);
            break;
        }
    }
    free(hist);
    free(hist_d);

}
//与谷底法类似，不是使用最小谷底值，而是使用峰值位置平均值
void MeanDoubleHumpThreshold(double *src,double *dst,int width,int height,int type){
    int *hist=(int *)malloc(sizeof(int)*GRAY_LEVEL);
    double *hist_d=(double *)malloc(sizeof(double)*GRAY_LEVEL);
    setHistogram(src, hist, width, height);
    Hist_int2double(hist, hist_d);
    double threshold=0.0;
    for(int i=0;i<MAXLOOP;i++){
        SmoothHist(hist_d, hist_d);
        if(0.0!=(threshold = (double)isDoubleHump(hist_d,DOUBLEHUMP_MEANHUMP))){
            Threshold(src, dst, width, height, threshold, type);
            break;
        }
    }
    free(hist);
    free(hist_d);

}


```
上面包括谷底阈值和双峰平均值的阈值处理
## 结果观察
原图：
![这里写图片描述](http://img.blog.csdn.net/20150307161314721)

直方图和平滑后的直方图，以及格式化的直方图：
![这里写图片描述](http://img.blog.csdn.net/20150307161421614)

阈值处理结果
![这里写图片描述](http://img.blog.csdn.net/20150307161439851)

原图
![这里写图片描述](http://img.blog.csdn.net/20150307161603918)

直方图和平滑后的直方图，以及格式化的直方图：

![这里写图片描述](http://img.blog.csdn.net/20150307161353659)

阈值处理结果
![这里写图片描述](http://img.blog.csdn.net/20150307161630438)


## 结论
针对双峰图像的谷底阈值和峰顶平均阈值算法是纯粹的基于直方图的阈值处理方法，本算法对图像的唯一要求是原直方图是双峰的，或者经过平滑可以形成双峰的，这样就可以利用一阶导数得到谷底和峰顶位置，找到合适的阈值进行阈值处理，本算法对噪声不敏感，但鲁棒性不好，容易无法给出阈值。所以需要谨慎使用。
待续。。。
