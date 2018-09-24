---
title: \[线性代数\]2-5:矩阵的逆(Inverse)
toc: true
categories:
  - Mathematic
  - Linear Algebra
date: 2017-09-11 20:00:16
keywords:
  - Inverse
  - Singular
  - Pivot
  - Inverse of AB
  - Gauss-Jordan
  - Left-Inverse
  - Right-Inverse
---
**Abstract:** 矩阵的“逆”，以及相关计算
**Keywords:** Inverse，Singular，Gauss-Jordan

<!--more-->

# 矩阵的逆
## 逆
### $A^{-1}$
逆，就是乘法的逆，也就是你和你的逆乘起来等于单位的你，如果你是矩阵，那就是单位矩阵，如果你是实数，那逆就是倒数,当然如果是是0，你就没有逆了，如果有了，那就逆天了😆
逆的表示很简单
$$
I=AA^{-1}
$$
上面就是我那段解释的数学语言，$A^{-1}$ 是 $A$ 的逆，由于矩阵乘法有顺序问题，当A是方阵的时候：
$$
I=A^{-1}A
$$

一个矩阵可逆，那么他的左逆和右逆一致，就是他的逆。
### Notes
可以理解为矩阵逆的性质或者特点，原文标记为Note
>Note1:
>The Inverse exist if and only if elimination produces n pivots(row exchanges are allowed)

并不是所有矩阵都有逆！所以，如何判断矩阵是否可逆（invertible）就是求矩阵逆的第一步，要是不可逆，那就不能求逆了。矩阵逆的存在当且仅当消元后产生n个主元（允许行交换）。

>Note2:
>The matrix A cannot have two different inverse.

解释下，利用了乘法结合律(parentheses)，利用左乘和右乘来证明矩阵的左逆和右逆相等
简单的证明：
Suppose $BA=I$ , $AC=I$ Then $B=C$ :
$$
B(AC)=(BA)C
$$
Gives
$$
BI=IC
$$
or
$$
B=C
$$


>Note3:
>if A is invertible, the one and only solution to $Ax=b$ is $x=A^{-1}b$

这个Note主要是说明$Ax=b$有解的情况，也就是系数矩阵可逆，或者说主元数量为N
证明：
$$
Ax=b
$$
Then:
$$
x=A^{-1}Ax=A^{-1}b\\
x=A^{-1}b
$$

>***Note4***:
>This is important,Suppose there is nonzero vector $\textbf{x}$ such that $Ax=0$ then Acannot have an inverse . No matrix can bring $\textbf{0}$ back to $\textbf{x}$

解释一下，这一条是重要的一个note，这是后面向量空间的一个重要结论（后面再说，这个不着急，这是线性代数的最核心内容），如果 $Ax=0$ ,其中x不是0，那么A没有逆，证明如下：
$$
Ax=0\\
A^{-1}Ax=A^{-1}0\\
x=A^{-1}0
$$
上面式子中$x\neq0$所以等号不可能成立，那么$A^{-1}$不存在

>Note5:
>A 2x2 matrix is invertible if and only if $ad-bc$ is not zero:
$$
\begin{bmatrix}a&b\\c&d\end{bmatrix}^{-1} =  \frac{1}{ad-bc}\begin{bmatrix}d&-b\\{-c}&a\end{bmatrix}
$$
本note的解释就是，后面讲到行列式的时候就会有详细的证明了

>Note6:
>A diagonal matrix has an inverse provided no diagonal entries are zero:

$$
A=\begin{bmatrix}d_1&\,&\, \\\,& \ddots &\,\\\,&\,& d_n\end{bmatrix}
$$
then
$$
A^{-1}=\begin{bmatrix}1/d_1&\,&\,\\\,& \ddots &\,\\\,&\,& 1/d_n\end{bmatrix}
$$
这是一个关于对角矩阵的故事，对角矩阵的对角元素全部非零，其他元素为0，其逆是其所有元素的倒数

## $(AB)^{-1}$ and $(AB\dots Z)^{-1}$
两个矩阵相乘的逆，当然你可以把结果一步一步算出来，得到算数结果，然后求逆，但这里的逆视为一种操作，不关系结果是多少，只关注本操作的一系列特性，因为这些特性能是计算变得更简单，按照最基本计算规律可能无法计算，这也是“计算方法”或者“数值分析”课程所最核心的内容，按照某些算法和计算性质，能够优化计算速度，提高计算结果精确度，当然这些都是通过计算机计算的，所数值分析是cs的课程
两个矩阵相乘的逆
$$
(AB)^{-1}=B^{-1}A^{-1}
$$
两个矩阵要交换位置
$$
(ABC)^{-1}=C^{-1}B^{-1}A^{-1}
$$
$$
(AB\dots Z)^{-1}=Z^{-1} \dots B^{-1}A^{-1}
$$
三个或者更多个的时候，要完全倒过来
证明方法灰常简单：
$$
(AB)^{-1}(AB)=B^{-1}(A^{-1}A)B=B^{-1}(I)B=B^{-1}B=I
$$
逆运算与乘法在一起结合就是这样的。

## 高斯乔丹消元(GAUSS-JORDAN Elimination)
高斯乔丹消元求矩阵的逆，适合小型矩阵的笔算。
其主要基础是矩阵乘法的分块性质
$$
A^{-1}\begin{bmatrix}A&&I\end{bmatrix}\\
=\begin{bmatrix}A^{-1}A&&A^{-1}I\end{bmatrix}\\
=\begin{bmatrix}I&&A^{-1}\end{bmatrix}
$$
其中最关键的一点是如何让 $A$ 变成 $I$,这里就是高斯消元的主要问题点，首先生成一个曾广矩阵，然后消元小区下三角矩阵，以及上三角矩阵，最后只有一个部分对角矩阵，然后用对角矩阵乘以其倒数，右侧的I就变成了$A^{-1}$


其过程大概如下
$$
\begin{bmatrix}A&&I\end{bmatrix}\\
A=LR\\
$$

$$
R=L^{-1}A\dots (1)\\
L^{-1}\begin{bmatrix}A&&I\end{bmatrix}=\begin{bmatrix}R&&L^{-1}I\end{bmatrix}
$$
(1)的主要过程就是通过消元，使得增广矩阵中的A矩阵变成一个上三角矩阵R，对角线一下都是零
$$
R=UD \dots(2)\\
D=U^{-1}R\\
U^{-1}\begin{bmatrix}R&&L^{-1}I\end{bmatrix}=\begin{bmatrix}D&&U^{-1}L^{-1}I\end{bmatrix}
$$
(2)回代过程，D只有对角线上有元素，其他全部是零
$$
I=D^{-1}D\dots(3)\\
\begin{bmatrix}D&&L^{-1}IU^{-1}\end{bmatrix}D^{-1}=\begin{bmatrix}I&&D^{-1}U^{-1}L^{-1}I\end{bmatrix}
$$
(3)对角线归一化，使得A对角线上的元素变成1
同样的一些列操作作用在右半部分$I$上，就能得到一个$D^{-1}U^{-1}L^{-1}$的矩阵
因为
$$A=LUD$$
所以
$$A^{-1}=D^{-1}U^{-1}L^{-1}$$
可知，其结果是正确的。

### 逆矩阵的性质(Properties)

>1：一个矩阵如果是对称的，并且有逆，那么逆也是对称的。
>2：三角矩阵的逆如果存在可能是一个稠密矩阵

性质2很重要，有些稀疏矩阵的逆可能是稠密矩阵，这在数值分析中导致某些算法失效，或者效率急剧下降。

## 奇异和可逆(Singular vs Invertible0
奇异矩阵，和可逆矩阵是一对反义词，奇异这个翻译不知道是谁根据啥想出来的，但是很有迷惑性，不知道啥是奇异矩阵，什么又是非奇异的，奇异矩阵的具体问题我们会在后面学到，但是目前奇异矩阵可以当做没有逆的矩阵。
***三角矩阵可逆的唯一条件是对角元素全部非0***

## Conclusion
本文主要介绍下矩阵逆的一些性质，证明Gauss-Jordan的过程是我自己写的，可能有问题，如果有不严谨的地方，希望大家给予指点，谢谢





原文地址1：[https://www.face2ai.com/Math-Linear-Algebra-Chapter-2-5](https://www.face2ai.com/Math-Linear-Algebra-Chapter-2-5)转载请标明出处
