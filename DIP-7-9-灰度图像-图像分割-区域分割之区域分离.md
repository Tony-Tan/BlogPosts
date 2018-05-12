---
title: 【数字图像处理】7.9:灰度图像-图像分割 区域分割之区域分离
date: 2015-03-10 20:37
categories:
  - DIP
keywords:
  - 区域分割
toc: true
---
**Abstract:** 数字图像处理：第58天
**Keywords:** 区域分割
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
废话开始，今天本来只想写一篇，但晚上觉得还是快把区域分割简单介绍下，后面开始彩色图像类的知识学习和代码实现，下一篇介绍分水岭算法，这才是个头疼的算法，今天的区域分离（合并）相对比较好理解。
## 算法原理
首先本算法依然是基于区域的，用到的区域的性质是区域的均值和标准差，简单描述算法，如果一个区域满足设定的均值范围和标准差范围，设置整个区域为亮，否则将次区域分为四份，每一份继续递归进行，直至预先设定的最小区域。
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-9-灰度图像-图像分割-区域分割之区域分离/20150310201759768.jpeg)

从结构来讲可以抽象成一颗四叉树：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-9-灰度图像-图像分割-区域分割之区域分离/20150310201721315.jpeg)

算法最核心的是设计一个判别式，上面说的判别式是均值和均方的联合，也可以使用其他判别式，根据实际情况可以具体设计。

算法：

1. 初始化，输入参数，包括均值上下界m1,m2，标准差上下界d1,d2
2. 计算区域均值和标准差，如果满足条件，输出对应设置为亮 否者将区域分为四份
3. 将其中一份带入步骤2递归进行计算
4. 如果区域分割小于设定的最小值结束递归。

## 代码
```c++
/*
 *区域分割算法，递归进行判断
 *如果区域不符合条件，将区域
 *分为四份，递归判断每个区域
 *知道区域分为最小设定值
 */

void findSplitRegion(double *src,double *dst,int width,int height,int x,int y,int w_width,int w_height,double mean_param1,double mean_param2,double variance_param1,double variance_param2){
    double mean=RegionMean(src, width, height, x, y, w_width, w_height);
    double variance=RegionStdDeviation(src, width, height, x, y, w_width, w_height);
    if(mean>mean_param1&&
       mean<=mean_param2&&
       variance>variance_param1&&
       variance<=variance_param2){
        RegionSetOne(dst, width, height, x, y, w_width, w_height);
    }else{
#define MINIMAL_CELL 3
        if(w_width>=MINIMAL_CELL&&w_height>=MINIMAL_CELL){
            findSplitRegion(src, dst, width,height, x, y, w_width/2+1, w_height/2+1, mean_param1, mean_param2, variance_param1, variance_param2);
            findSplitRegion(src, dst, width,height, x+w_width/2, y, w_width/2+1, w_height/2+1, mean_param1, mean_param2, variance_param1, variance_param2);
            findSplitRegion(src, dst, width,height, x+w_width/2, y+w_height/2+1, w_width/2+1, w_height/2, mean_param1, mean_param2, variance_param1, variance_param2);
            findSplitRegion(src, dst, width,height, x, y+w_height/2, w_width/2+1, w_height/2+1, mean_param1, mean_param2, variance_param1, variance_param2);
        }
    }
}

void RegionSplit(double *src,double *dst,int width,int height,double mean_param1,double mean_param2,double variance_param1,double variance_param2){
    double *dsttemp=(double *)malloc(sizeof(double)*width*height);
    findSplitRegion(src, dsttemp, width, height, 0,0, width, height, mean_param1, mean_param2, variance_param1,variance_param2);
    matrixCopy(dsttemp, dst, width, height);
    free(dsttemp);

}
```
## 实验结果
原图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-9-灰度图像-图像分割-区域分割之区域分离/20150310202929976.jpeg)
想要分离周围的星云,参数见图中标注：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-9-灰度图像-图像分割-区域分割之区域分离/20150310202837514.jpeg)
原图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-9-灰度图像-图像分割-区域分割之区域分离/20150310203055667.jpeg)
同样分离周围的星云，参数见图中标注：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-9-灰度图像-图像分割-区域分割之区域分离/20150310203021441.jpeg)
## 总结
此算法运行速度很快，但精确度不够高，因为设置的最小区域值决定了区域分割的准确性，所以会有锯齿状的边缘，这是一个缺点。
待续。。。
