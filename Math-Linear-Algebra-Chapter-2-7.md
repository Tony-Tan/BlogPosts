---
title: \[线性代数\]2-7:转置与变换(Transposes and Permutation)
toc: true
categories:
  - Mathematic
  - Linear Algebra
date: 2017-09-12 16:47:01
keywords:
  - Transposes
  - Permutation
  - Symmetric
  - Inner Products
  - R'R
---
**Abstract:** 矩阵的转置和行变换（permutation），包含一些运算的转置，以及对称概念的提出和相关性质
**Keywords:** Transposes,Permutation,Symmetric,Inner Products,R'R
<!--more-->
## 开篇废话
这些基本运算的篇，好难写，公式和基本逻辑太多，说少了说不明白，说多了又啰嗦。本来计划的是写短小精悍的，基本每篇就写一个知识点，现在看看是不行了，这些东西都太连贯了，没办法拆开，争取后面到了高级算法的时候就可以每篇写很短，写精髓了，这一些就是一两千字，对我有点挑战啊，哈哈哈。如果各位有看不懂的，请回顾以前的文章，因为我是按照基本逻辑来的，就是一个知识点衍生另一，不会凭空就搞出来什么知识点，那样又变成大学上课了，big Picture一定要有，就是我们第一篇线性代数的，big Picture！

## 转置(Transposes)
### 转置(Transposes)
转置是矩阵特有的计算，他的根本就是矩阵是一块数字，其中有顺序和位置关系，今天说的转置和置换，都是针对位置的，也就是元素的数值并不改变，要改变的是元素的位置关系，permutation我们后面再说，transpose的计算规则的就是，对于某元素，其位置行和列相互交换
$$
(A^T)_{ij}=A_{ji}
$$
一个下三角矩阵的transpose是上三角矩阵。
但是下三角矩阵的逆还是下三角矩阵。
### 映射(Properties)
sum:
$$(A+B)^T=A^T+B^T$$
Products:
$$(AB)^{T}=B^{T}A^{T}$$
Inverse:
$$
(A^{-1})^T=(A^T)^{-1}
$$
这里唯一有点疑惑的可能就是Product需要调换位置了，想要理解为啥要换位置，可以回归最简单的
$Ax=b$ 等号有个很好的作用就是，如果对两侧同时做某一操作的时候，等号不会改变，
$$
(Ax)^T=b^T
$$
我们来分析下，如果按照我们之前的解释，Ax是x作为系数关于A的线性组合，从矩阵乘法的角度就Column Model，其结果是b，一个列向量，用A各列线性组合出来的结果。如果b转置了，变成了一个行向量，那么我们对应的也应该改成Row Model来调整等号前面的结构了，所以
$$
b^T=x^TA^T
$$
x转置变成了行，A的列也变成了行，那么行模型可以很好的让他们的结果与b的行形式相等
没明白的同学，拿笔算算，就会发现确实是这样的，而且如果不调换位置，矩阵不满足乘法要求，尺寸对不上。
那么根据上面的特征，组合多个x得到矩阵B
$$
B=\begin{bmatrix}x_1&x_2\end{bmatrix}
AB=\begin{bmatrix}Ax_1&Ax_2\end{bmatrix}
B^TA^T=\begin{bmatrix}x_1^TAT\\x_2^TA^T\end{bmatrix}
$$
与上面情况吻合，证明在矩阵与矩阵相乘过程也符合情况。
在更多的矩阵相乘的时候，与逆的情况类似
比如上一节的男猪脚：
$$
A=LDU\\
A^T=U^TD^TL^T\\
D^T=D\\
$$

***对角矩阵的转置还是他自己***

逆的转置就是转置的逆
$$
A^{-1}A=I\\
A^T(A^{-1})^T=I^T=I\\
$$
so
$$
(A^T)^{-1}=(A^{-1})^T
$$
## 再说 Inner Product
之前不完整的介绍过内积，就是点乘，但是一般情况两个向量内积，写成转置形式

$$
\langle a,b\rangle=a^Tb
$$

后面的转置才比较正规，内积普遍用于实际生产，比如机械，金融等各类公式，这样我们就非常接近应用数学的核心了。

$$
(Ax)^Ty=x^T(A^Ty)
$$

$(Ax)$ 与 $y$ 的内积等于 $x$ 与 $A^Ty$ 的内积.
## 对称(symmetric)
对称的定义是：
$$
A^T=A
$$
Means
$$
a_{ji}=a_{ij}
$$
对称矩阵的逆也是对称矩阵，本篇讨论的主要是矩阵内部位置关系相关的计算。
对于对称矩阵A:
$$
(A^{-1})^T=(A^T)^{-1}=A^{-1}
$$

## $R^TR$
对于一个转置和其本身的乘积，也是一个对称矩阵，证明很简单：
$$
(R^TR)^T=R^TR
$$
so
$R^TR$是一个对称矩阵。
对称矩阵的消元过程也比较简单，对于对称矩阵 $A=LDU$ 其中$L^T=U$也就说上三角矩阵和下三角矩阵是转置关系，$(LDL^T)^T=LDL^T$ 也证明A是对称的，对称矩阵消元需要的计算量应该是非对称矩阵的一半大概是 $n^3/6$

## Permutation
Permutation就是之前消元里面的行变换的矩阵化表示，其定义是：

>A Permutation matrix P has the rows of the identity I in any order
置换矩阵有很多特殊的性质，比如说
> $p^{-1}=p^T$
>对于nxn的Permutation矩阵组,一共有 $n!$ 个矩阵

## $PA=LU$
上节的LU分解有一个前提假设就是，不存在主元是0的情况，也就是说不需要行变换就可以达到消元的效果，但是现在我们去掉这个假设，使问题更为一般化，PA就是对A的某些行进行变换，后进行分解
对于行变换，有两种方式:
1:先对A进行行变换，然后分解，那么就是 $PA=LU$
2:一边分解一边进行行变换，也就是在中间过程 $A=L_1P_1U_1$
更多情况下，我们选择第一种方式，因为第一种方式更为简单方便。
## Conclusion
总结一下就是转置和对称的相互关系，以及其一些特性，矩阵中元素的位置变换成为了本文重点。





原文地址1：[https://www.face2ai.com/Math-Linear-Algebra-Chapter-2-7](https://www.face2ai.com/Math-Linear-Algebra-Chapter-2-7)转载请标明出处
