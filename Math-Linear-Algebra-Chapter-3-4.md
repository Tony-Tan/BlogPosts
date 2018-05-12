---
title: 【线性代数】3-4:方程组的完整解( $Ax=b$ )
categories:
  - Mathematic
  - Linear Algebra
keywords:
  - Ax=b
  - Special Solution
  - Full Column Rank
  - Full Row Rank
  - Complete Solution
toc: true
date: 2017-09-25 15:20:42
---

**Abstract:** Ax=b的完整解，以及一个解，infinity个解，没有解的所有条件和说明
**Keywords:** Ax=b,Special Solution,Full Column Rank,Full Row Rank,Complete Solution

<!--more-->

## 开篇废话
废话没啥好说的，成长总要经历痛苦，只有不断的让自己痛苦才能不断的提升能力，逐渐掌握自己的命运，一直生活在安逸快乐中是个好事，但有一天命运一旦降临将束手无措，也许一辈子的辛苦努力就是为了逃过某次致命一击，用一生的辛苦来扼住命运的喉咙，不也是精彩的一生么？别抱怨社会，别抱怨政府，也别抱怨不公平，如果认定不公平，那为什么不是获利一方？如果说自己的父亲不给力，那你有一天也是别人的父亲，原始财富一定要有人积累，这也对家族的责任。
## $Ax=b$
之前我们已经研究了 $Ax=0$的相关内容，值得说一下的是，列空间和nullspace是有些区别的，列空间指的是b所在的空间，而nullspace是x所在的空间，这个要区别一下，这些所有空间都是针对矩阵的。

## One Particular Solution
搞不懂Particular Solution和Sceptical Solution有啥区别的可以仔细看看了，之前我也没发现其有什么根本不同，Particular Solution是把所有的free variables设置为0，来一个完整的例子
$$
\begin{bmatrix}
1&3&0&2\\
0&0&1&4\\
1&3&1&6
\end{bmatrix}
\begin{bmatrix}
x_1\\x_2\\x_3\\x_4
\end{bmatrix}
=
\begin{bmatrix}
1\\6\\7
\end{bmatrix}
$$
Augmented Matrix:
$$
\begin{bmatrix}
1&3&0&2&1\\
0&0&1&4&6\\
1&3&1&6&7
\end{bmatrix}
=
\begin{bmatrix}
A&b
\end{bmatrix}
$$
经过消元，对原始矩阵方程消元和对Augment Matrix消元得到结果相同，这里就只写augment matrix：
$$
\begin{bmatrix}
1&3&0&2&1\\
0&0&1&4&6\\
0&0&0&0&0
\end{bmatrix}
=
\begin{bmatrix}
R&d
\end{bmatrix}
$$
通过定位pivot可以确定free columns是第二列和第四列，如果我们把$x_2,x_4$设置为零，那么就得到了x为
$$
\begin{bmatrix}
x_1\\x_2\\x_3\\x_4
\end{bmatrix}
=
\begin{bmatrix}
1\\0\\6\\0
\end{bmatrix}
=x_{particular}
$$
可以通过回代验证$Rx=d$或者$Ax=b$这里的free variables都是0，那么就说这个解是particular的，而且可以发现pivot对应的x位置与d有关系，按顺序就是d对应的元素，这不是巧合，观察矩阵可以得到相应的结论。

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-3-4/particular.png)
这是我们到目前位置讨论的方程组的两种解，一个当b=0的时候，一个当b不等于0的时候，如果我们把这两个方程组左右相加，那么就得到
$$
Ax_p=b\\
Ax_n=0\\
Ax_p+Ax_n=b+0\\
A(x_p+x_n)=b
$$
那么完整的解(式子中的数字来自上面的例子，nullspace我没有写出来，大家可以自行验证)：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-3-4/complete.png)

为什么完整解是上看的式子呢，可以看下一节的详细介绍。
## The Complete Solution
前面我们确定了完整的解就是$x_p+x_n$，那么我们到底有多少个解呢？

|序号| m&r | n&r |     Matrix Shape      | Ax=b |   Solution    |
|:---:|:---:|:---:|:---------------------:|:----:|:-------------:|
|1| r=m | r=n | Square and Invertible | Ax=b |       1       |
|2| r=m | r<n |    Short and Wide     | Ax=b |   $\infty$    |
|3| r<m | r=n |     Tall and Thin     | Ax=b |    0 or 1     |
|4| r<m | r<n |     Not full rank     | Ax=b | 0 or $\infty$ |

这四种情况，简单讲解下：

-------
第一种：是方阵，我们最标准的方程组，rank与m,n相等，那么就一个解。这种情况下，消元后的R是单位矩阵：
$$
R=\begin{bmatrix}I\end{bmatrix}
$$

------
第二种：如果方程组的个数小于未知数的个数，而rank与行相同，rank=m<n,消元后的R是如下矩阵：
$$
R=\begin{bmatrix}I&F\end{bmatrix}
$$

------
第三种：如果方程组的个数大于未知数的个数，而rank与列数相同，rank=n<m,消元后的R是如下矩阵：
$$
R=\begin{bmatrix}I\\0\end{bmatrix}
$$
如果b与R下面全是0的行对应的行不是0，那么就没有解

------
第四种：如果有效的方程组的个数小于未知数的个数，与情况二相似，但是有效方程数还小于远方程数，rank<m,rank<n,消元后的R是如下矩阵：
$$
R=\begin{bmatrix}I&F\\0&0\end{bmatrix}
$$
如果b与R下面全是0的行对应的行不是0，那么就没有解

自此，Ax=b 的所有解以及对应的情况都已经搞定了，***但是为啥complete solution是$x_p+x_n$呢？***这就是个悬案了，后面一定给出答案，现在就暂时记住就好了（回归课堂教育，先记住，就没有然后了）。

## Conclusion
这篇也比较凌乱，不像写算法那种博客很流畅，解释，代码，总结这种套路在这不太实用，因为知识错综复杂，本文上面的四种解的情况才是重点，包括什么时候没有解，什么时候无数个解，如果按照表和矩阵R的形式来看，就会豁然开朗，后面继续。。。
