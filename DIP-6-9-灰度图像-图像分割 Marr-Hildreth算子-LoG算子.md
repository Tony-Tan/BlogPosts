---
title: 【数字图像处理】6.9:灰度图像-图像分割 Marr-Hildreth算子（LoG算子）
date: 2015-02-13 14:54
categories:
  - 数字图像处理
tags:
  - 边缘检测
  - LoG算子
toc: true
---

## 开篇废话
今天介绍二阶微分算子，二阶微分算子典型的是Laplace算子，LoG可以看成是一个高斯模板的拉普拉斯变换，但是也可以从根源上推导出LoG算子，而后面要介绍的DoG则是为了纯粹的减少计算，模拟LoG的一种方法。
#LoG原理
LoG最底层的原理是二阶微分算子，也就是对原始图像求二次微分的边缘定位算法，前面的边缘模型中可以得知，当使用二阶微分算子的时候，其对边缘的响应是一个零交叉，而且能够判断出高灰度方向，但二阶微分对噪声的敏感度过高，需要平滑预处理。
Marr和Hildreth【Marr和Hildreth，1980】证明了以下两个观点：
>（1）灰度变化与图像尺寸没有关系，因此检测需要不同尺度的算子
>（2）灰度的突然变化会在一阶导数中引起波峰和波谷，或者二阶导数中一起零交叉

所以可以提出一种能变换尺寸的（当时用的是标准Sobel等那些，固定尺寸的，当时Sobel还没有扩展），在各种大小的图像上都能起作用的，可以检测模糊的相对较大的边缘，也可以检测细小的锐度集中的精细细节，当然这种算子也必须对全图所有像素点其作用。Marr和Hildreth证明LoG算子是满足上述条件的最满意的算子。
## 数学原理
LoG算子： $\nabla^2G$
其中标准高斯函数，$G(x,y)$ 标准差 $\delta$ ：
$G(x,y)=e^{-\frac{x^2+y^2}{2\delta^2}}$

对一个标准高斯函数（未归一化）进行二次偏微分：
$\nabla^2G(x,y)=\frac{\partial^2G(x,y)}{\partial x^2}+\frac{\partial^2G(x,y)}{\partial y^2}=[\frac{x^2}{\delta^4}-\frac{1}{\delta^2}]e^{-\frac{x^2+y^2}{2\delta^2}}$

最终表达式：
$\nabla^2G(x,y)=[\frac{x^2+y^2-2\delta^2}{\delta^4}]e^{-\frac{x^2+y^2}{2\delta^2}}$

LoG零交叉出现在$x^2+y^2=2\delta^2$处LoG函数形状如下图，也被叫做墨西哥草帽算子。
下面全方位无死角观察下，说明，上面公式给出的是倒置的草帽，我们这里给他加了个负号，让它正过来。。。
![这里写图片描述](http://img.blog.csdn.net/20150213141425740)
![这里写图片描述](http://img.blog.csdn.net/20150213141437643)
![这里写图片描述](http://img.blog.csdn.net/20150213141457330)
![这里写图片描述](http://img.blog.csdn.net/20150213141500422)
![这里写图片描述](http://img.blog.csdn.net/20150213141836448)

剖面图，沿着直径切开：
![这里写图片描述](http://img.blog.csdn.net/20150213141953980)


#模板性质

 - 性质1，该模板对平坦区域应该无响应，所以算子内系数和应该为0，使用公式计算出来的结果不为零，需要整体上下平移模板
 - 性质2，LoG可以使用laplace算子和高斯模板进行卷积后求得，其等效于使用LoG大小和标准差的高斯平滑后得到laplace结果
 - 性质3，得到的结果要检测零交叉来定位边缘，因为噪声等原因，这里进行判断时可以使用阈值，也就是当出现零交叉的时候还要判断下正负值的差的绝对值是否满足阈值要求。
 - 性质4，LoG模板的大小和标准差的选择关系，遵循高斯分布的$3\delta$原则，也就是当位置超过均值正负$3\delta$以外的值很小，也就是说，LoG模板应该选择大于$6\delta$的最小奇数作为模板大小，过大效果不会有提高反而增加计算量，过小会造成截断，无法得到正确结果。

## 代码
```c++
//3x3的邻域内，如果中心像素大于阈值，切周围存在负像素值，则此点为0交叉点
void findCross(double *src,double *dst,int width,int height,double threshold){
    double *dsttemp=(double *)malloc(sizeof(double)*width*height);
    Zero(dst, width, height);
    double c_value=0.0;
    int flag=1;
    Zero(dsttemp, width, height);
    for(int j=1;j<height-1;j++){
        for(int i=1;i<width-1;i++){
            c_value=src[j*width+i];
            flag=1;
            for(int m=-1;m<=1&&flag;m++)
                for(int n=-1;n<=1;n++){
                    if(c_value>threshold&&src[(j+m)*width+i+n]<-0.1){
                        flag=0;
                        dsttemp[j*width+i]=255.0;
                        break;
                    }

                }
        }
    }
    matrixCopy(dsttemp, dst,width, height);
    free(dsttemp);


}
//得到LoG模板，根据公式得出double型模板，然后调整整体位置，使模板内系数和为0
void getLoGMask(double *mask,int width,int height,double delta){
    int center_x=width/2;
    int center_y=height/2;
    double x,y;
    for(int j=0;j<height;j++){
        for(int i=0;i<width;i++){
            x=i-center_x;
            y=j-center_y;
            mask[j*width+i]=-((x*x+y*y-2*delta*delta)/pow(delta,4))*(exp(-(x*x+y*y)/(2*delta*delta)));

        }

    }

    double sum=0.0;
    for(int i=0;i<width*height;i++){
        sum+=mask[i];
    }
    double di=sum/(double)(width*height);
    for(int i=0;i<width*height;i++){
        mask[i]-=di;
    }

}

//LoG边缘检测完整过程，
//生成模板
//与图像卷积
//寻找0交叉
double LoG(double *src,double *dst,int width,int height,int m_width,int m_height,double delta,double threshold){
    double *dsttemp=(double *)malloc(sizeof(double)*width*height);
    double * mask=(double *)malloc(sizeof(double)*m_width*m_height);
    getLoGMask(mask, m_width,m_height,delta);
    RealConvolution(src, dsttemp, mask, width, height, m_width, m_height);
    findCross(dsttemp,dst,width,height,threshold);
    free(dsttemp);
    free(mask);
    return findMatrixMax(dst,width,height);

}
```
## 操作结果
以下为算子对图像操作的结果
原图：
![这里写图片描述](http://img.blog.csdn.net/20150213143510422)
LoG结果：
![这里写图片描述](http://img.blog.csdn.net/20150213143608329)
![这里写图片描述](http://img.blog.csdn.net/20150213143634270)
![这里写图片描述](http://img.blog.csdn.net/20150213143636534)
零交叉检测结果：
![这里写图片描述](http://img.blog.csdn.net/20150213143746139)
![这里写图片描述](http://img.blog.csdn.net/20150213143755125)
![这里写图片描述](http://img.blog.csdn.net/20150213143757233)
![这里写图片描述](http://img.blog.csdn.net/20150213143845063)
![这里写图片描述](http://img.blog.csdn.net/20150213143855281)
![这里写图片描述](http://img.blog.csdn.net/20150213143913782)
![这里写图片描述](http://img.blog.csdn.net/20150213143937744)
![这里写图片描述](http://img.blog.csdn.net/20150213143946979)
![这里写图片描述](http://img.blog.csdn.net/20150213143957915)
原图：
![这里写图片描述](http://img.blog.csdn.net/20150213144038067)
零交叉检测结果：
![这里写图片描述](http://img.blog.csdn.net/20150213144208656)
![这里写图片描述](http://img.blog.csdn.net/20150213144224755)
![这里写图片描述](http://img.blog.csdn.net/20150213144240355)
![这里写图片描述](http://img.blog.csdn.net/20150213144127831)
![这里写图片描述](http://img.blog.csdn.net/20150213144129815)
![这里写图片描述](http://img.blog.csdn.net/20150213144142357)
![这里写图片描述](http://img.blog.csdn.net/20150213144320307)
![这里写图片描述](http://img.blog.csdn.net/20150213144347638)
![这里写图片描述](http://img.blog.csdn.net/20150213144400274)



## 总结
观察上述结果阈值为0的结果，或出现意大利通心粉效应--所有边缘会形成一个闭环，典型的是那些圈圈，这是一个严重的缺陷，所以不使用0阈值，而是使用超过0的阈值来解决这个问题，LoG的边缘检测效果还是不错的，当选择 $\delta$ 的时候可以使用多尺度，然后选择所有尺度都出现的公共边缘（逻辑与），不过这种计算量过大，一般作为考量 $\delta$ 选择的一种参考。
待续。。。
