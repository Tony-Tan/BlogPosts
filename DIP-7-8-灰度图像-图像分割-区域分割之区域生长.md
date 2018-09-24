---
title: 【数字图像处理】7.8:灰度图像-图像分割 区域分割之区域生长
date: 2015-03-10 10:37
categories:
  - DIP
keywords:
  - 区域生长
  - 图像分割
  - 区域分割
toc: true
---
**Abstract:** 数字图像处理：第57天
**Keywords:** 区域分割,区域生长
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
# 开篇废话
继续说废话，昨天写博客被同事看到了，问我，为什么你每一篇开始都是废话，我说凑字数，在一个可以写点轻松的话，天天在算法的海洋里飘荡，偶尔说几句荒山野岭的废话也算活跃气氛了。
# 区域分割介绍
今天介绍基于区域的分割方法，前面基于阈值的分割方法暂时告一段落，基于区域的分割运用同样广泛，但和阈值比较，区域分割难度也稍微大了一些，比如后面要讲到的分水岭算法，分水岭算法是个算法族，并不是单一的一个固定算法，比如有基于形态学的，也有基于其他的，但思想都一样，分水岭是那种典型的，看起来很简单，一说原理，小学生都能听懂，但实现起来难度不小，也可能是我代码能力不行，反正写了将近一整天，修改了一天才算看到点结果。
由于基于区域的分割算法已经成为一个专门的研究领域，这几篇博文只介绍一点点最基础的，通用的算法，至于高深的高科技的算法，留到未来的某个时刻。这里只讲最简单的。
今天介绍的区域生长，是其中比较简单的一种。
# 区域生长算法
区域生长的算法过程总结如下：

1. 选取种子点$c(x,y)$
2. 以种子点为中心，对其邻域像素进行递归遍历
3. 对于每一个邻域像素$N(x',y')$，设计一个判别式$Q(c(x,y),N(x',y'))$。
4. 如果判别式为真，邻域像素N被设置为新的种子点，进入第2步，并将该点加入结果集合（与种子点为同一区域）。否则退出此次递归回到3，检测下一个邻域像素。


整体思路是以种子点为中心，遍历图，深度优先或广度优先没有没有关系，判断中心点和其邻域是否满足判别式，注意，此处最终要的点是判别式，设计判别式可以针对不同的应用，下面代码中设计的判别式是个简单的范围判别式，也就是说如果被判别的像素灰度值，在一定范围内，则为真，否则为假，范围是由种子点和附加参数param一起产生的。
#代码
```c++
/*区域生长，设置一个种子点x（灰度值为x_v），然后以种子点为中心
 *向四周进行图搜索，如果点y（灰度值为y_v）,邻域满足param+x_v>y_v>x_v-param（条件1）
 *则此点与种子点归为一个区域，以此递归，条件1可以根据不同的需要自行设置其他.
 */
//递归遍历邻域，并判断条件是否成立
void findGrowRegion(double *src,double *dst,int seed_x,int seed_y,int width,int height,int regionNum,double value,double param){
    dst[seed_y*width+seed_x]=(double)regionNum;
    for(int j=-1;j<2;j++)
        for(int i=-1;i<2;i++){
            if(seed_x>=0&&seed_y>=0&&seed_x<width&&seed_y<height&&(j!=0||i!=0)){
                if(src[(j+seed_y)*width+i+seed_x]>=value-param
                   &&src[(j+seed_y)*width+i+seed_x]<=value+param
                   &&dst[(j+seed_y)*width+i+seed_x]==0.0)
                    findGrowRegion(src, dst, i+seed_x, j+seed_y, width, height, regionNum, value,param);
            }
        }
}
void RegionGrow(double *src,double *dst,Position * position,int p_size,int width,int height,double param){
    double * dsttemp=(double *)malloc(sizeof(double)*width*height);
    Zero(dsttemp, width, height);
    int regionNum=100;

    for(int i=0;i<p_size;i++){
        int x=position[i].x;
        int y=position[i].y;
        findGrowRegion(src, dsttemp, x,y, width, height, regionNum,src[y*width+x],param);
        regionNum+=10;
    }

    matrixCopy(dsttemp, dst, width, height);
    free(dsttemp);
}
```
# 结果分析
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-8-灰度图像-图像分割-区域分割之区域生长/20150310102947344.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-8-灰度图像-图像分割-区域分割之区域生长/20150310102957219.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-8-灰度图像-图像分割-区域分割之区域生长/20150310103121036.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-8-灰度图像-图像分割-区域分割之区域生长/20150310103016999.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-8-灰度图像-图像分割-区域分割之区域生长/20150310103140802.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-8-灰度图像-图像分割-区域分割之区域生长/20150310103152174.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-8-灰度图像-图像分割-区域分割之区域生长/20150310103208850.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-8-灰度图像-图像分割-区域分割之区域生长/20150310103222111.jpeg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-7-8-灰度图像-图像分割-区域分割之区域生长/20150310103231315.jpeg)

# 总结
区域生长算法实现较简单，但如果递归区域面积过大，可能造成程序卡死，可能是栈空间不够或者别的，这个需要处理下，这个算法的缺点是需要设置种子点，本算法的优点也是可以设置种子点，这样灵活但不够智能，算法执行速度较快。
待续。。。





原文地址1：[https://www.face2ai.com/DIP-7-8-灰度图像-图像分割-区域分割之区域生长](https://www.face2ai.com/DIP-7-8-灰度图像-图像分割-区域分割之区域生长)转载请标明出处
