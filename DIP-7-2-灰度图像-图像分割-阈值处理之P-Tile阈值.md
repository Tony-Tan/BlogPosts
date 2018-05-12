---
title: 【数字图像处理】7.2:灰度图像-图像分割 阈值处理之P-Tile阈值
date: 2015-03-06 17:00
categories:
  - DIP
keywords:
  - 阈值处理
  - p-tile
toc: true
---
**Abstract:** 数字图像处理：第51天
**Keywords:** 阈值处理,p-tile
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
## 开篇废话
废话不多说，因为刚才（上一篇）已经说过了，p-tile可能听起来挺可怕，没关系，说个它的对象--中位数，这个都知道吧，数值排排站，然后选出中间那个，或者说，假如数据一共有N个，那么中位数就是排在第 $N\times 0.5%$ 的那个数；p位数，也叫p分位，可以理解为数值排排站以后第 $N*p$ 的那个数。
## p-tile均值
根据上面对p分位的理解，可以看出这个阈值处理方法是半自动的方法，也就是阈值的生成需要人工控制，就是要手动输入p分位的p，下面代码中p取值$(0 ,1]$
## 代码
```c++
//阈值法，p分位法
//p分位为统计学方法
//当p为0.5时为中位数
void PtileThreshold(double *src,double *dst,double p_value,int width,int height,int type){/*0<p_value<1*/
    int total_pix_count=width*height;
    int pix_count=0;
    int hist[GRAY_LEVEL];
    double threshold_value=0.0;
    InitHistogram(hist);
    setHistogram(src, hist, width,height);
    for(int i=0;i<GRAY_LEVEL;i++){
        pix_count+=hist[i];
        if(pix_count>=(int)((double)total_pix_count*p_value)){
            threshold_value=(double)i;
            break;
        }
    }
    Threshold(src,dst, width, height, threshold_value,type);
}
```
## 效果
原图一个只有两个灰度值的图像，这里使用对其加入5%的高斯噪声，
未处理图像：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-2-灰度图像-图像分割-阈值处理之P-Tile阈值/20150306164631631.jpeg)
未处理时的直方图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-2-灰度图像-图像分割-阈值处理之P-Tile阈值/20150306164804279.jpeg)
观察直方图，估计出最佳阈值位置：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-2-灰度图像-图像分割-阈值处理之P-Tile阈值/20150306164829925.jpeg)
下面使用不同的p值来测试结果：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-2-灰度图像-图像分割-阈值处理之P-Tile阈值/20150306165259105.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-2-灰度图像-图像分割-阈值处理之P-Tile阈值/20150306165319010.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-2-灰度图像-图像分割-阈值处理之P-Tile阈值/20150306165244612.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-2-灰度图像-图像分割-阈值处理之P-Tile阈值/20150306165335998.jpeg)

可以看出，第二次（我试了好久。。。。。）测试结果能够得出最好结果。

----------
lena图处理测试：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-2-灰度图像-图像分割-阈值处理之P-Tile阈值/20150306165555993.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-2-灰度图像-图像分割-阈值处理之P-Tile阈值/20150306165609004.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-2-灰度图像-图像分割-阈值处理之P-Tile阈值/20150306165622857.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-2-灰度图像-图像分割-阈值处理之P-Tile阈值/20150306165632934.jpeg)

## 总结
首先确定p值需要经验或实验，所以P-Tile方法应用于自适应有些困难，其次，影响处理结果的因素是目标与背景大小的比例，目标过大背景过小或者背景过大目标过小，都会对测试结果产生很大影响，其次是噪声，噪声也会对实验结果产生影响。
待续。。。
