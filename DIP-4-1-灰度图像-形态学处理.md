---
title: 【数字图像处理】4.1：灰度图像-形态学处理

date: 2015-01-05 19:45
categories:
  - DIP
keywords:
  - 形态学
  - 腐蚀
  - 膨胀
  - 开闭操作
toc: true
---
**Abstract:** 数字图像处理：第14天
**Keywords:** 形态学，腐蚀，膨胀，开闭操作
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>


## 开篇废话
新年第一篇博客，希望大家新年里能都收获更多的知识和技术，今天说的是灰度图像的形态学处理，灰度图像与二值图像的区别在于其记录了灰度信息，所以，形态学处理的定义与二值图像有些不同，因为二值图像可以用一系列的二维坐标来表示图像信息，而灰度图需要一个三维坐标表示，而且二值图像中结构元SE是平坦的，没有灰度信息的，但灰度图中，结构元是可以带有第三维信息的，即结构元也是灰度的，这就带来了一些问题，因为二值图像中，形态学的输出结果完全由输入图像产生，但是结构元一旦引入灰度信息，那么输出结果将不再由输入图像唯一确定。所以，一般情况下，结构元都使用平坦的结构元。

## 腐蚀与膨胀
腐蚀与膨胀是形态学的基本操作，在灰度图像中也是如此，在二值图像中腐蚀和膨胀定义为对图像进行translation以后的“与”和“或”的逻辑操作结果，在灰度图像中，为了保存灰度信息，“与”和“或”操作被对应的替换成了“最大值”和“最小值”操作这样就给出了灰度图像中腐蚀和膨胀的操作定义。
另一种定义《图像处理、分析与机器视觉》中给出的是这样的，首先给出了个本影的概念，如果假设给出函数y=f(x)，那么f产生的曲线到x轴的阴影叫做本影，曲线叫做顶面（三维的叫面，二维的叫线，三维以上叫超平面），结构元表示为函数v=k(x)，一维函数f，k的腐蚀和膨胀等效于其本影（二值图像）的腐蚀与膨胀，并以此为基础推广到二维灰度图。

![Center][]
我们以512x512的lena图中的第256行为例，展示下其操作结果，结构元为横向的1x5的结构元SE（1行，5列），中心为SE中心。下图为灰度图剖面示意图，其中蓝色为原图数据，绿色为膨胀后数据，红色为腐蚀后数据：
![Center 1][]
我们将局部放大，可以看到其具体情况：
![Center 2][]
## 开操作与闭操作
与二值图像中的定义相同，开、闭操作也又腐蚀膨胀操作衍生出来的，依旧先腐蚀后膨胀是开操作，先膨胀后腐蚀是闭操作。
具体如下图，蓝色为原图，绿色为闭操作，红色为开操作：

![Center 3][]
局部放大，可以看出，开操作是将跨度小于结构元的峰顶消去，闭操作是将跨度小于结构元的谷底填平，冈萨雷斯的书中层有个滚动圆形结构元的例子也很生动，结论一致：

![Center 4][]


## 顶帽和底帽
顶帽操作和底帽操作是灰度图像所特有的，其原理是开操作将使峰顶消去，具体消去了多少呢，可以用原图减去开操作结果，这样就能得到其消去的部分，而这个过程成为顶帽操作，顶帽就是开操作消去的峰顶，这一部分对应于图像中较亮的部分，也叫白色顶帽。
同理，底帽操作是用闭操作的结果减去原图就得到被闭操作填充的谷底部分，这一部分对应于图像中较暗的部分，也叫黑色底帽。

## 测地腐蚀与测地膨胀
测地腐蚀与测地膨胀在二值图像中的操作是将腐蚀与膨胀结果与ground做“与”和“或”操作，与灰度膨胀中膨胀的推广一样，“与”和“或”被取代为最大值和最小值。
## 重建操作
重建操作分为很多种，包括重建开操作、重建顶帽操作等。其根本原理是通过腐蚀找到SE的模式，然后迭代膨胀或者迭代顶帽操作直到图像收敛。
## 数学表达

腐蚀：

膨胀：

开操作：

闭操作：

顶帽操作：

底帽操作：

测地腐蚀：

测地膨胀：

嘿嘿。。公式还在生产中。。公式太多。。哈哈哈哈。。。。。

## 代码
```c++
#include <cv.h>
#include <highgui.h>
#include <stdio.h>
#define TOFINDMAX 0
#define TOFINDMIN 1
#define isSIZEEQU(x,y) (((x)->width)==((y)->width)&&((x)->height)==((y)->height))
struct position{
    int x;
    int y;
};
typedef struct position Position;
//判断结构元是否平滑
int isSmooth(IplImage *src){
    int width=src->width;
    int height=src->height;
    for(int i=0;i<width;i++)
        for(int j=0;j<height;j++){
            int v=cvGetReal2D(src,j,i);
            if(v!=255.0&&v!=0.0)
                return 0;
        }
    return 1;
}
//判断两幅图像是否相等
int isEqu(IplImage *src1,IplImage *src2){
    if(!isSIZEEQU(src1, src2))
        return 0;
    for(int i=0;i<src1->width;i++)
        for(int j=0;j<src1->height;j++){
            double v1=cvGetReal2D(src1, j, i);
            double v2=cvGetReal2D(src2, j, i);
            if(v1!=v2)
                return 0;
        }
    return 1;

}
//将图像全部设置为1
void One(IplImage *src){
    int width=src->width;
    int height=src->height;
    for(int i=0;i<width;i++)
        for(int j=0;j<height;j++)
            cvSetReal2D(src, j, i, 255.0);


}
//位移，如果非平滑SE将加上sevalue，即对应的灰度值
void Translation(IplImage *src ,IplImage *dst,double SEvalue,Position *d,int istoFindMin){
    IplImage *temp=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    int srcwidth=src->width;
    int srcheight=src->height;
    int dstwidth=dst->width;
    int dstheight=dst->height;
    if(istoFindMin)
        One(temp);
    else
        cvZero(temp);
    for(int i=0;i<srcwidth;i++){
        for(int j=0;j<srcheight;j++){
            int target_x=i+d->x;
            int target_y=j+d->y;
            if(target_x>=0&&target_y>=0&&
               target_x<dstwidth&&target_y<dstheight){
                double value=cvGetReal2D(src, j, i)+SEvalue;
                value=(value>=255.0?255.0:value);
                cvSetReal2D(temp, target_y, target_x, value);
            }
        }
    }
    cvCopy(temp, dst, NULL);

    cvReleaseImage(&temp);
}
//找出两幅等大图像中同一位置中相对较大的像素值
void MaxPix(IplImage *src1 ,IplImage *src2,IplImage *dst){
    if(!isSIZEEQU(src1, src2)||!isSIZEEQU(src1, dst)){
        printf("MaxPix wrong: src size not equ!\n");
        exit(1);
    }
    int width=src1->width;
    int height=src1->height;
    for(int i=0;i<width;i++)
        for(int j=0;j<height;j++){
            double value1=cvGetReal2D(src1, j,i);
            double value2=cvGetReal2D(src2, j,i);
            value1>value2?cvSetReal2D(dst, j,i,value1):cvSetReal2D(dst, j, i, value2);
        }

}
//找出两幅等大图像中同一位置中相对较小的像素值
void MinPix(IplImage *src1 ,IplImage *src2,IplImage *dst){
    if(!isSIZEEQU(src1, src2)||!isSIZEEQU(src1, dst)){
        printf("MaxPix wrong: src size not equ!\n");
        exit(1);
    }
    int width=src1->width;
    int height=src1->height;
    for(int i=0;i<width;i++)
        for(int j=0;j<height;j++){
            double value1=cvGetReal2D(src1, j,i);
            double value2=cvGetReal2D(src2, j,i);
            value1<value2?cvSetReal2D(dst, j, i, value1):cvSetReal2D(dst, j, i, value2);
        }

}
//灰度图像膨胀
void Dilate_Gray(IplImage *src,IplImage *dst,IplImage *se,Position *center){
    int SEissmooth=isSmooth(se);
    IplImage *temp=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    IplImage *temp_last=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    Position centerde;
    centerde.x=se->width/2;
    centerde.y=se->height/2;
    if(center==NULL){
        center=¢erde;
    }
    int sewidth=se->width;
    int seheight=se->height;
    cvCopy(src,temp_last,NULL);
    for(int i=0;i<sewidth;i++)
        for(int j=0;j<seheight;j++){
            cvCopy(src,temp,NULL);
            double value=cvGetReal2D(se, j, i);
            if(value!=0.0){
                Position d;
                d.x=center->x-i;
                d.y=center->y-j;
                if(SEissmooth)
                    Translation(temp, temp, 0.0, &d,TOFINDMAX);
                else
                    Translation(temp, temp, value, &d,TOFINDMAX);
                MaxPix(temp, temp_last, temp_last);
            }
        }
    cvCopy(temp_last, dst, NULL);
    cvReleaseImage(&temp);
    cvReleaseImage(&temp_last);

}
//灰度图像腐蚀
void Erode_Gray(IplImage *src,IplImage *dst,IplImage *se,Position *center){
    int SEissmooth=isSmooth(se);
    IplImage *temp=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    IplImage *temp_last=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    Position centerde;
    centerde.x=se->width/2;
    centerde.y=se->height/2;
    if(center==NULL){
        center=¢erde;
    }
    int sewidth=se->width;
    int seheight=se->height;
    cvCopy(src,temp_last,NULL);
    for(int i=0;i<sewidth;i++)
        for(int j=0;j<seheight;j++){
            cvCopy(src,temp,NULL);
            double value=cvGetReal2D(se, j, i);
            if(value!=0.0){
                Position d;

                d.x=i-center->x;
                d.y=j-center->y;
                if(SEissmooth)
                    Translation(temp, temp, 0.0, &d,TOFINDMIN);
                else
                    Translation(temp, temp, -1.0*value, &d,TOFINDMIN);
                MinPix(temp, temp_last, temp_last);

            }
        }
    cvCopy(temp_last, dst, NULL);
    cvReleaseImage(&temp);
    cvReleaseImage(&temp_last);

}
//开操作
void Open_Gray(IplImage *src,IplImage *dst,IplImage *se,Position *center){
    IplImage *temp=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    Erode_Gray(src, temp, se, center);
    Dilate_Gray(temp, temp, se, center);
    cvCopy(temp, dst, NULL);
    cvReleaseImage(&temp);

}
//闭操作
void Close_Gray(IplImage *src,IplImage *dst,IplImage *se,Position *center){
    IplImage *temp=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    Dilate_Gray(src, temp, se, center);
    Erode_Gray(temp, temp, se, center);
    cvCopy(temp, dst, NULL);
    cvReleaseImage(&temp);

}
//灰度梯度形态学提取
void Gard_Gray(IplImage *src,IplImage *dst,IplImage *se,Position *center){
    IplImage *temp_dilate=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    IplImage *temp_erode=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    Dilate_Gray(src, temp_dilate, se, center);
    Erode_Gray(src, temp_erode, se, center);
    cvSub(temp_dilate, temp_erode, dst, NULL);
    cvReleaseImage(&temp_erode);
    cvReleaseImage(&temp_dilate);


}
//顶帽操作
void TopHat(IplImage *src,IplImage *dst,IplImage *se,Position *center){
    IplImage *temp=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    Open_Gray(src, temp, se, center);
    cvSub( src,temp, dst, NULL);
    cvReleaseImage(&temp);

}
//底帽操作
void BottomHat(IplImage *src,IplImage *dst,IplImage *se,Position *center){
    IplImage *temp=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    Close_Gray(src, temp, se, center);
    cvSub(temp,src, dst, NULL);
    cvReleaseImage(&temp);

}
//测地腐蚀
void Erode_Gray_g(IplImage *src,IplImage *ground,IplImage *dst,IplImage *se,Position *center){
    int SEissmooth=isSmooth(se);
    IplImage *temp=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    IplImage *temp_last=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    Position centerde;
    centerde.x=se->width/2;
    centerde.y=se->height/2;
    if(center==NULL){
        center=¢erde;
    }
    int sewidth=se->width;
    int seheight=se->height;
    cvCopy(src,temp_last,NULL);
    for(int i=0;i<sewidth;i++)
        for(int j=0;j<seheight;j++){
            cvCopy(src,temp,NULL);
            double value=cvGetReal2D(se, j, i);
            if(value!=0.0){
                Position d;

                d.x=i-center->x;
                d.y=j-center->y;
                if(SEissmooth)
                    Translation(temp, temp, 0.0, &d,TOFINDMIN);
                else
                    Translation(temp, temp, -1.0*value, &d,TOFINDMIN);
                MinPix(temp, temp_last, temp_last);

            }
        }
    MaxPix(temp_last,ground,temp_last);
    cvCopy(temp_last, dst, NULL);
    cvReleaseImage(&temp);
    cvReleaseImage(&temp_last);


}
//测地膨胀
void Dilate_Gray_g(IplImage *src,IplImage *ground,IplImage *dst,IplImage *se,Position *center){
    int SEissmooth=isSmooth(se);
    IplImage *temp=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    IplImage *temp_last=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    Position centerde;
    centerde.x=se->width/2;
    centerde.y=se->height/2;
    if(center==NULL){
        center=¢erde;
    }
    int sewidth=se->width;
    int seheight=se->height;
    cvCopy(src,temp_last,NULL);
    for(int i=0;i<sewidth;i++)
        for(int j=0;j<seheight;j++){
            cvCopy(src,temp,NULL);
            double value=cvGetReal2D(se, j, i);
            if(value!=0.0){
                Position d;
                d.x=center->x-i;
                d.y=center->y-j;
                if(SEissmooth)
                    Translation(temp, temp, 0.0, &d,TOFINDMAX);
                else
                    Translation(temp, temp, value, &d,TOFINDMAX);
                MaxPix(temp, temp_last, temp_last);
            }
        }
    MinPix(temp_last, ground, temp_last);
    cvCopy(temp_last, dst, NULL);
    cvReleaseImage(&temp);
    cvReleaseImage(&temp_last);
}
//重建开操作
void Rebuild_Open(IplImage *src,IplImage *dst,IplImage *ground,IplImage *erodeSE,IplImage *dilateSE,int eroden){
    IplImage *temp=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    IplImage *temp_last=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    cvCopy(src, temp, NULL);
    for(int i=0;i<eroden;i++){
        Erode_Gray(temp, temp, erodeSE, NULL);
    }

    while(!isEqu(temp, temp_last)){
        cvCopy(temp, temp_last, NULL);
        Dilate_Gray_g(temp, ground, temp, dilateSE, NULL);

    }
    cvCopy(temp, dst, NULL);
    cvReleaseImage(&temp);
    cvReleaseImage(&temp_last);

}
//重建闭操作，这段没测试
void Rebuild_Close(IplImage *src,IplImage *dst,IplImage *ground,IplImage *dilateSE,IplImage *erodeSE,int dilaten){
    IplImage *temp=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    IplImage *temp_last=cvCreateImage(cvGetSize(src), src->depth, src->nChannels);
    cvCopy(src, temp, NULL);
    for(int i=0;i<dilaten;i++){
        Dilate_Gray(temp, temp, dilateSE, NULL);
    }

    while(!isEqu(temp, temp_last)){
        cvCopy(temp, temp_last, NULL);
        Erode_Gray(temp, temp, erodeSE, NULL);
        MinPix(temp, ground, temp);

    }
    cvCopy(temp, dst, NULL);
    cvReleaseImage(&temp);
    cvReleaseImage(&temp_last);

}
//重建顶帽操作
void Rebuild_Tophat(IplImage *src,IplImage *dst,IplImage *ground,IplImage *dilateSE,IplImage *erodeSE,int eroden){
    Rebuild_Open(src,dst,ground,erodeSE,dilateSE,eroden);
    cvSub(src, dst, dst, NULL);

}
//重建底帽操作
void Rebuild_Bottomhat(IplImage *src,IplImage *dst,IplImage *ground,IplImage *dilateSE,IplImage *erodeSE,int dilaten){
    Rebuild_Close(src,dst,ground,dilateSE,erodeSE,dilaten);
    cvSub(src, dst, dst, NULL);

}
int main(){

    return 1;

}
```


## 结果图片
以下结果原图为lena 512x512的图像产生：
腐蚀：
![Center 5][]
膨胀：
![Center 6][]
开操作：
![Center 7][]
闭操作：
![Center 8][]
顶帽操作：
![Center 9][]
底帽操作：
![Center 10][]
重建操作示意（冈萨雷斯 中文第三版 P437）：
去除横向亮条：重建顶帽操作
![Center 11][]
重建开操作，去除纵向亮纹：
![Center 12][]
上图横向膨胀：
![Center 13][]
膨胀结果与重建顶帽操作的最小操作：
![Center 14][]
原图对比：
![Center 15][]



[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105183312480.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105184314531.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105184358453.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105184704226.png
[Center 4]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105184658703.png
[Center 5]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105193504319.png
[Center 6]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105193542726.png
[Center 7]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105193629823.png
[Center 8]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105193721989.png
[Center 9]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105193805654.png
[Center 10]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105193810937.png
[Center 11]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105194004058.jpg
[Center 12]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105194047187.jpg
[Center 13]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105194206362.jpg
[Center 14]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105194248326.jpg
[Center 15]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-4-1-灰度图像-形态学处理/20150105194246968.png
