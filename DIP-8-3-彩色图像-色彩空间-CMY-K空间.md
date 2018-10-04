---
title: 【数字图像处理】8.3:彩色图像-色彩空间 CMY(K)空间
date: 2015-03-12 18:55
categories:
  - DIP
keywords:
  - CMK
---
**Abstract:** 数字图像处理：第63天
**Keywords:** CMY
<!--more-->
<font color="00FF00">本文最初发表于csdn，于2018年2月17日迁移至此</font>
# 彩色图像-色彩空间 CMY(K)空间
继续废话，因为色彩空间比较多，所以先大概每个空间都进行一下简介，然后根据后面的应用来回顾这些色彩空间的一些知识，根据不同色彩空间开发出的不同算法会在后面继续介绍，最近生存环境恶劣，但是学习不能停。
看到同志在博客里留言，说博客写的很牛。首先被别人，尤其是陌生表扬心里都会开心，其次写这些博客就是为了记录下自己学习的历程，如果能给别人一些启发那就更好了，希望能继续坚持下去，做自己喜欢的事，无论周围环境如何。
## CMY模型
首先必须要说一下光的彩色和染料的颜色有什么不同，首先看到的光的颜色就是光的颜色，比如看到光是红色的，那么这就是红色的光，但染料颜色和光的颜色不同，我们看到的染料颜色是它反射的颜色，也就是说你看到红色的染料，其实它是非红色的（也就是青色），也就是染料本身的颜色是所看到的相反的颜色。
CMY就是染料的颜色，RGB的补色，RGB是典型的加性色彩空间，而CMY则是典型的减性色彩空间。下面观察CMY的色度图
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-3-彩色图像-色彩空间-CMY-K空间/20150312184220426.png)

- C：青（Cyan）(G+B)
- M：洋红或品红（Magenta）(R+B)
- Y：黄（Yellow）(R+G)

CMY的增强版是CMYK，K表示key，是黑色，加入黑色的原因有很多，比如CMY染料不纯，所以三种颜色混合的时候得不到黑色而是一种暗红，其次如果使用CMY混合得到黑色，那么此处将会被喷了足够多的墨，不容易干，最后，黑色的墨水很便宜，所以在CMY的基础上加入了K。
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/DIP-8-3-彩色图像-色彩空间-CMY-K空间/20150312185021061.png)

进一步说明CMY在打印时组合颜色并不是线性的，各种颜色吸收和反射曲线不同，所以具体的比例依据彩色墨水的具体性质而定。
CMY和RGB的转换：
$$
\left[\begin{array}{c}
C\\
M\\
Y\\
\end{array}\right] =
\left[\begin{array}{c}
G_{max}\\
G_{max}\\
G_{max}\\
\end{array}\right]-\left[\begin{array}{c}
R\\
G\\
B\\
\end{array}\right]
$$

逆变换：
$$
\left[\begin{array}{c}
R\\
G\\
B\\
\end{array}\right]=
\left[\begin{array}{c}
G_{max}\\
G_{max}\\
G_{max}\\
\end{array}\right]-
\left[\begin{array}{c}
C\\
M\\
Y\\
\end{array}\right]
$$

其中 $G_{max}$ 表示单个通道的最大值。
## 总结
RGB和CMY的理解较简单，在此不再赘述。
待续。。。





原文地址1：[https://www.face2ai.com/DIP-8-3-彩色图像-色彩空间-CMY-K空间](https://www.face2ai.com/DIP-8-3-彩色图像-色彩空间-CMY-K空间)转载请标明出处
