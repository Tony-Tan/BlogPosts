---
title: 【线性代数】3-2:零空间(Nullspace)
categories:
  - Mathematic
  - Linear Algebra
tags:
  - Ux=0
  - Rx=0
  - 0空间
  - 主列
  - 自由列
  - 特解
toc: true
date: 2017-09-19 17:40:36
---

**Abstract:** 零空间的相关知识点，使用到前面的消元过程
**Keywords:** Nullspace，Pivot Columns，Free Columns，Special Solutions，Ux=0，Rx=0

<!--more-->

# 零空间
## $Ax=0$
之前讲$Ax=b$的时候提到过，正着看反着看的例子，其实这个办法是MIT18.01Caculus里面讲的一种技巧，不同的方向含义不同，今天更直接了当，把b改成o，好啦，来吧，怎么能让A的列组合出来0？不用说0肯定可以，那么只有0么？并不是。
>The nullspace of A consists of all solutions to Ax=0.These vectors x are in $\Re^n$ the nullspace containing all solutions of Ax=0 is donate by $N(A)$

其实这个nullspace还是挺别致的，起码他包含0，而之前Ax=b就不一定包含0。所以可以看出，nullspace是个subspace，原因是如果x，y向量Nullspace里面的两个向量，那么$A(x+y)=0$，并且$A(cx)=0$成立，所以nullspace是个子空间 $Ax=b$并不一定是。
### Special Solutions
一般情况下，我们理解都是A0=0，这是最常规的，Ax=0的其他解一看就不是什么正经解，的确是这样的，看看下面这个例子：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-3-2/例子1.png)

没错看吧：
$A=[1,2,3]$,$N(A)$是个啥？$1x+2y+3z=0$是个通过原点的平面，一个方程，三个未知数，那么有两个变量是随意的（这两个变量的值是随意的，但这两个变量并不是随意的），只要另一个委屈自己，使整体满足就可以，那么另外两个变量随意选择，这个随意选择就有技术含量了，你选两个0，那么x只能是0，这两个随意的变量叫做“free variables”,其个数等于m或n中大的那个减去主元的个数，而且上面说的随便两个变量也不是那么随便的，需要对应一定的列，下面的例子会仔细说明。

>The nullspace consists of all combinations of the special solutions

也就是如果有两个free variable 那么nullspace就是个平面，根据上面我们的研究发现确实这样的，x+y+z=0是个过原点的平面。这两个Special solutions的linear combination就是N(A).

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-3-2/又一个例子.png)

这个例子是找C矩阵的nullspace，上面忘了说了，如果你想研究nullspace首先要进行一些列的elimination操作，也就是把原始矩阵转化成U(上三角矩阵)，矩阵U中有主元的叫做主列，也就是说这部分系数是完整的，比如有两个主列，那么这两个主列对应的x中的元素就是不自由要委屈自己的，因为他有足够多的限制，而那些没有主元的列就是free列了，这些列在x中对应的元素就可以放飞自己了，但是一般情况下我们会分别让他们是0和1，这样free部分可以以标准基的形式出现，张成整个free空间，然后让主列的x元素去委屈自己，这个地方这么理解可能有点困难，仔细看下面这段话：
***首先我们必须承认Nullspace是矩阵的子空间，子空间就是不完整的A的列所张成的列空间，维度必然低于（等于）A的列空间的维度，一个五维空间中的向量（向量属于 $\Re^5$ ）三维子空间必然有两维不是完整的（不自由的），但是另外三维是完整的（自由的）三维空间***那么这三维的基就是我们选的标准基，如果你执意要选不标准的也可以，只要能张成三维空间就行。其实这就是特解的完整理解，上面的话都是我说的，不严谨可以提出。
举个例子:
$$
x+y=0
$$
那么他的A
$$
A=\begin{bmatrix}1&1\end{bmatrix}
$$
第一列是主列，第二列是free的，我们可以选择1，那么x=-1。（-1，1）的任何倍数都是0空间内的向量，我们就得到了一条直线。注意A的列空间是个平面，而有一个free variable的nullspace是个直线，少一维，free variable控制这vector在这条直线上的位置。
回来继续看上面C的例子
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-3-2/例子的解.png)

***Special solutions span the Nullspace！！！！！***
## $Ux=0$ & $Rx=0$
求解nullspace的过程
>1：Forward elimination to U or its reduced form R
>2：Back substitution in Ux=0 or Rx=0 product x


解释下，reduced form 就是通过向上消元，把pivot上方的元素消掉变成0，同时缩放，把pivot全部变成0.$N(A)=N(U)=N(A)$
U或者R中没有pivot的列是free列，对应x中的一个free variable，如果当m<n的时候，至少包含1个free variable。

## Conclusion
这篇主要介绍Nullspace，线性代数最重要的四个spaces已经搞定两个，还剩下两个就比较容易通过前两个推导了（什么？我居然没说是哪四个？好吧，列空间，Nullspace，行空间，左Nullspace）。这篇文字叙述有点多，因为没办法形象的





原文地址1：[https://www.face2ai.com/Math-Linear-Algebra-Chapter-3-2](https://www.face2ai.com/Math-Linear-Algebra-Chapter-3-2)转载请标明出处
