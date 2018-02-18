---
title: 【数字图像处理】7.7:灰度图像-图像分割 阈值处理之局部阈值
date: 2015-03-09 11:14
categories:
  - 数字图像处理
tags:
  - 阈值处理
  - 局部阈值处理
toc: true
---
学习DIP第57天
转载请标明本文出处：***[http://blog.csdn.net/tonyshengtan][http_blog.csdn.net_tonyshengtan]***，出于尊重文章作者的劳动，转载请标明出处！文章代码已托管，欢迎共同开发：***[https://github.com/Tony-Tan/DIPpro][https_github.com_Tony-Tan_DIPpro]***

# 开篇废话 #

废话开始，今天说下区域阈值（局部阈值）,前面介绍的阈值都是全局阈值，也就是阈值根据全局信息产生，而作用对象也是整幅图像的全部像素，而局部阈值的产生是一个中心像素c(x,y)的邻域的一些属性来计算出一个或多个阈值以及阈值的判别式。这句话比较难懂，举个例子，假设c的邻域R，根据邻域R计算出阈值T1,T2,T3.....Tn我们可以表示成向量T=(T1,T2,T3....Tn)，设计阈值判别式Q(T,pixValue)其中pix\_value的值就是像素c(x,y)的灰度值，判别式返回真假，真的话像素设置为亮，否则设置成暗。

# 算法内容 #

该算法的关键点在于设计判别式Q和计算阈值向量T，因为此算法的通用性不是很强，但优点是灵活性强，可以根据不同的图片性质来设计不同的执行方案，比如下面例子中使用最简单的两种统计学参数，均值和标准差，当中心像素大于均值的n倍并且大于标准差的m倍。设置窗口大小，也就是邻域大小，参数n，参数m，最后得到较好的阈值结果。

# 代码实现 #

    /*局部阈值 *使用均值和标准差作为判定依据 *输入参数包括邻域大小，均值系数，以及标准差系数 * * */
    void LocalThreshold(double *src,double *dst,int width,int height,int w_size,double mean_param,double std_dev_param){
        double *temp=(double *)malloc(sizeof(double)*width*height);

        Zero(temp, width,height);
        double mean_g=matrixMean(src, width, height);
        for(int j=w_size/2;j<height-w_size/2;j++){
            for(int i=w_size/2;i<width-w_size/2;i++){
                double deta=0.0;
                double mean=0.0;
                double pix_value=src[j*width+i];
                //local mean
                for(int m=-w_size/2;m<w_size/2+1;m++){
                    for(int n=-w_size/2;n<w_size/2+1;n++){
                        mean+=src[(j+m)*width+i+n];
                    }
                }
                mean/=(double)(w_size*w_size);
                //local deta
                for(int m=-w_size/2;m<w_size/2+1;m++){
                    for(int n=-w_size/2;n<w_size/2+1;n++){
                        deta+=(src[(j+m)*width+i+n]-mean)*(src[(j+m)*width+i+n]-mean);
                    }
                }

                deta/=(double)(w_size*w_size);
                deta=sqrt(deta);
                if(pix_value>mean_param*mean_g&&pix_value>std_dev_param*deta){
                    temp[j*width+i]=255.0;
                }
            }
        }
        matrixCopy(temp, dst, width, height);
        free(temp);
    }

# 算法效果 #

![这里写图片描述][20150309111042924]
![这里写图片描述][20150309111024174]

# 总结 #

相比于全局阈值，局部阈值对目标大小，以及噪声敏感度强，但其缺点是设计针对性强，没有什么通用的算法，而且输入的参数多半需要分析实验产生，不能实现自动阈值处理，其优点是功能强大。
待续。。。


[http_blog.csdn.net_tonyshengtan]: http://blog.csdn.net/tonyshengtan
[https_github.com_Tony-Tan_DIPpro]: https://github.com/Tony-Tan/DIPpro
[20150309111042924]: http://img.blog.csdn.net/20150309111042924
[20150309111024174]: http://img.blog.csdn.net/20150309111024174


本文章迁移自[http://blog.csdn.net/tonyshengtan/article/details/44151055](http://blog.csdn.net/tonyshengtan/article/details/44151055)
