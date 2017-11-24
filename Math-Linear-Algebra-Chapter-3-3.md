---
title: 【机器学习基础之线性代数】3-3:Rank
categories:
  - Mathematic
  - Linear Algebra
tags:
  - Rank
  - Row Reduced form
  - Pivot Columns
  - Free Columns
  - Special Solutions
toc: true
date: 2017-09-25 15:20:38
---

**Abstract:** 本文将介绍线性代数中最最最重要的概念之一，秩（Rank）
**Keywords:** Rank,Row Reduced form,Pivot Columns,Free Columns,Special Solutions

<!--more-->

## 开篇废话
来段废话吧，之前写过一个小短文关于学习的，我把能力分成知识和经验两种方面来说，知识要考学习，而经验要依靠不断的练习和思考，以及通过知识作为基础来升级创造经验。不过看看周围那些掌握了更多社会资源的人，他们经验相当丰富，各种各样的经验，但是知识却有所欠缺，这样就使得某些产品和服务发生了根本上的变化，为了掩盖其中有一些不良变化，这些人又会利用自己丰富的经验来掩盖或者转移问题，这样下去不知道会产生些什么。
## Rank
Rank，the ordinary members of an organization (such as the enlisted soldiers of an army)，谋组织原有的成员，中文翻译“秩”，把八个字翻译成一个字，意思字形上还没什么关系，所以我们以后不会用这个字，博客中仅使用Rank。
前面我们所有的知识都依靠方程组来推进的，Rank也一样，毕竟线性代数就是研究方程组的，方程组$Ax=b$在消元过程中，有些行最后的结果是$0x=0$这个现象从字面上理解就是上面的方程和这行的方程存在倍数关系，比如本行的方程是$2x+2y=2$那么上面可能有一行$x+y=1$或者其他等价方程，还有可能是上面几个方程的线性组合刚好和本行方程一致，这样的意思就表明，这行方程对于全局求解没有提供任何有用的信息，比如描述一个人，A说你是个男的，B说你不是个女的，B这个信息就没用了（也可以说A的信息没用），那这样虽然有两条信息，但是原有的信息（原有的成员）就是1，B的信息可以通过A的演化出来，所以Rank就是矩阵所表示的方程组中那些原有的成员的数量。

>Defination: The rank of A is the number of pivots.This number is r

解释，如果矩阵中存在pivot那么证明，第i行的pivot下面的行没办法表示出第i行，上面的也没办法表示第i行



### Rank one
只有一个pivot的矩阵，是一种特殊的矩阵
$$
A=\begin{bmatrix}
1&3&10\\
2&6&20\\
3&9&30
\end{bmatrix}
\to
R=\begin{bmatrix}
1&3&10\\
0&0&0\\
0&0&0
\end{bmatrix}
=
\begin{bmatrix}
1\\2\\3
\end{bmatrix}
\begin{bmatrix}
1&3&10
\end{bmatrix}
=uv^T
$$
对于上面的例子可以看出，Rank one矩阵的分解形式可以理解为两个向量的outer product，那么矩阵A的Nullspace等于R的Nullspace：
$$
Ax=0
\to
uv^Tx
=0 \to
u(v^Tx)=0 \to
v^Tx=0
$$
可以观察出，$Ax=0$表明x在A的Nullspace中, $v^Tx=0$表明 $x$和 $av^T$ 相互垂直
也就是行空间内的任意向量($av^T$)和Nullspace内的任意向量垂直。

Rank r还是行空间和列空间的维数，同时跟Nullspace的维度有关！
### Free Columns & Pivot Columns
[3-2:Nullspace](http://tony4ai.com/2017/09/19/Math-Linear-Algebra-Chapter-3-2/)中对Free Columns和Pivot Columns有相关介绍，下面再介绍一些相关的性质，Free Columns是不包含Pivot的列，Pivot Columns则是包含Pivot的列，
对于任何一个Free Column都是其前面pivot列的线性组合。
举个例子，我们不看具体数字就看形状
$$
\begin{bmatrix}
1&x&x&x&x&x&x\\
0&1&x&x&x&*&x\\
0&0&0&1&x&x&x\\
0&0&0&0&0&1&x\\
0&0&0&0&0&0&1\\
0&0&0&0&0&0&0\\
\end{bmatrix}
$$
我们直接把矩阵写成R的形式（A是原始矩阵，U是上三角矩阵，R是reduced row echelon matrix，pivot上下都是0，通过方程线性组合而得到）
这个R中第三列明显是个free column，星号表示任意数字，都能够用第一列和第二列的线性组合得到，第五列也可以通过一二四列组合出来。
> Defination: The pivot columns are not combinations of earlier columns.The free columns are combinations of earlier columns.The combinations are the **Special Solutions**


## Special Solutions
Special Solutions上一篇也提到过了，我不知道怎么翻译（particular solution和special solution分不清谁是谁）。通俗得说就是给free variables赋予特殊值，然后求出pivot对应的未知数的，一般我们都是分别设置0和1，一个为1其他是0，然后迭代。

> $Ax=0$ has r pivots and n-r free variables: n columns minus r pivot columns.The nullspace matrix M contains the n-r special solutions .Then $AN=0$

可以这么理解，n-r个free variables每个都被设置成1一次，共n-r个special solutions.

原方程组中独立的方程数就是rank的大小，独立的概念在后面介绍，到时候回来看会非常惊艳。

最后一个知识利用到了前面讲到的分块乘法，回忆我们的方程组，如果未知数按照$x_1,x_2,x_3,\dots,x_n$的方法排列，我们可以得到系数矩阵A，但是我们稍微调换一下未知数顺序比如$x_2,x_1,x_3,\dots,x_n$他的系数矩阵B就是A中第一列和第二列交换的结果。所以我们可以根据pivot所在的列进行冲洗排列，比如上面的那个星号矩阵就可以得到：
$$
\begin{bmatrix}
1&x&x&x&x&x&x\\
0&1&x&x&x&x&x\\
0&0&1&x&x&0&x\\
0&0&0&1&x&0&0\\
0&0&0&0&1&0&0\\
0&0&0&0&0&0&0\\
\end{bmatrix}
\to
\begin{bmatrix}
1&0&0&0&0&x&x\\
0&1&0&0&0&x&x\\
0&0&1&0&0&0&x\\
0&0&0&1&0&0&0\\
0&0&0&0&1&0&0\\
0&0&0&0&0&0&0\\
\end{bmatrix}
=\begin{bmatrix}
I&F\\
0&0
\end{bmatrix}
$$
那么这个新的R我们称为$R_{new}$，那么他的Nullspace：
$$
R_{new}=
\begin{bmatrix}
I&F\\
0&0
\end{bmatrix}
N=\begin{bmatrix}
-F\\I
\end{bmatrix}
R_{new}N=0
$$
$-F$ 包含r个pivot variables，并且$R_{new}$中的I和N中I规模是不一样的。
## Conclusion
总结一下，这篇从原文来看有点模糊，就是前一篇的知识也有后一篇的知识也有而且融合在一些，想捋顺出来一点点说有点费劲，写了一篇用了一下午，下面看看还能不能再写两篇。
