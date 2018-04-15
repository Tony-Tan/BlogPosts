---
title: 【线性代数】2-4:矩阵操作(Matrix Operations)
toc: true
categories:
  - Mathematic
  - Linear Algebra
date: 2017-09-05 17:15:19
tags:
  - addition
  - subtraction
  - multiplication
  - inner product
  - outer product
---
**Abstract:** 矩阵基本计算，包括加减乘法，主要是乘法的几种不同的理解

**Keywords:** Addition，Subtraction，Multiplication，Inner Product，Outer Product
<!--more-->



## 开篇废话
今天的废话是，还没想好，想好了再补上
## 矩阵加法、减法
矩阵加减法，规则很简单，矩阵要求尺寸一样，row一样，column也得一样，这样按照对位相加减就行了。
$$
\begin{bmatrix}a_{11}&&a_{12}&&a_{13}\\
a_{21}&&a_{22}&&a_{23}\\
a_{31}&&a_{32}&&a_{33}\end{bmatrix}
\pm
\begin{bmatrix}b_{11}&&b_{12}&&b_{13}\\
b_{21}&&b_{22}&&b_{23}\\
b_{31}&&b_{32}&&b_{33}\end{bmatrix}\\=
\begin{bmatrix}a_{11}\pm a_{11}&&a_{12}\pm b_{12}&&a_{13}\pm b_{13}\\
a_{21}\pm b_{21}&&a_{22}\pm b_{22}&&a_{23}\pm b_{23}\\
a_{31}\pm b_{31}&&a_{32}\pm b_{32}&&a_{33}\pm b_{33}\end{bmatrix}
$$
这个没啥好说的，别减错地方就行。一对一进行

## 乘法
乘法才是矩阵计算的关键，计算意义，计算量，等很多是些非常有意义的研究课题，我记得本科学习线性代数的时候，老师先来将行列式，接着就是矩阵的计算法则，然后接着就是rank类的东西了，确实书本是这么讲的，但是现在想想，好像没啥逻辑，所以就没去上课了（给自己随便找个借口逃课）。
### 矩阵规模
相乘的两个矩阵尺寸上有些要求，例如对于： $AB$
***If A has n column,B must have n rows***
如果A为$m\times n$，B为 $n\times p$那么他们相乘的结果：
$$
(m\times n)(n \times p)=(m\times p)
$$

### Row Dot Product Column
Dot product之前已经讲过了，就是如何通过两个向量，pia的一下变成一个数字，矩阵可以看做是向量组成的，所以给出第一个规则，假设乘法为$AB=C$
$$
C_{ij}=(row\;i\;of\;A) \cdot (column\;j\;of\;B)
$$
这个规则是我之前上课学的，也是最基本的矩阵乘法公式，当然也可以写成求和的形式，但是我觉得通过dot product来看这个反而更直观一些，就不再把求和那个写出来了，补充句，在矩阵相乘的编程处理中，经过调整内外层循环，可以最大化利用高速缓冲，能够提高乘法速度。这个让我想起来之前项目，同事非要自己写乘法，然后就按照公式计算顺序，写了一个一毛一样的东西出来，虽然只做3x3的一个小矩阵乘法，速度什么的基本没什么影响，但是我觉得这当你不能保证你写的功能的稳定性和速度的时候，使用稳定的第三方库是个不错的选择。
#### Inner or Outer
dot product也叫inner product，当时我就在想有没有outer product，Pro. Strang在课堂上没有介绍outer但是在书上写了下，inner的矩阵形式是这样的：
$$
\begin{bmatrix}a_1&a_2&\dots&a_n\end{bmatrix}
\begin{bmatrix}b_1\\b_2\newline \vdots \\b_n\end{bmatrix}=\sum_{i=0}^{n}a_i*b_i
$$
outer的矩阵形式，就是。。
$$
\begin{bmatrix}a_1\\a_2\newline \vdots \\a_n\end{bmatrix}
\begin{bmatrix}b_1&b_2&\dots&b_m\end{bmatrix}
=\begin{bmatrix}
a_1b_1&&a_1b_2&&\dots &&a_1b_m\\
a_2b_1&&a_2b_2&&\dots &&a_2b_m\\
\vdots&&\vdots&&\dots &&\vdots\\
a_nb_1&&a_nb_2&&\dots &&a_nb_m\\
\end{bmatrix}
$$
其实outer的过程通过一行一列产生一个矩阵，所以当多列和多行（行和列必须相等数量）的矩阵相乘的时候就会产生多个矩阵，再对矩阵进行相加，这个过程的编程实现时，缓存利用率很高，就是本段开头说道的矩阵相乘的速度问题，当然这是不是最快的解决办法我也不知道，知识从哪本书上看到了这种说法，印象比较深刻
#### Column Model
列模型，如果把矩阵看做是很多列的组合，那么可以回归到最早的$Ax=b$的过程
$A$是被乘矩阵, $\textbf{x}$ 扩展成多列的矩阵$X$
$$
  X=\begin{bmatrix}
  \vdots&&\vdots&&\dots &&\vdots\\
  x_1&&x_2&&\dots&&x_n\\
  \vdots&&\vdots&&\dots &&\vdots\\
  \end{bmatrix}
$$
把x代入
$$
  AX=
  A
  \begin{bmatrix}
  \vdots&&\vdots&&\dots &&\vdots\\
  x_1&&x_2&&\dots&&x_n\\
  \vdots&&\vdots&&\dots &&\vdots\\
  \end{bmatrix}\\
  =
  \begin{bmatrix}
  \vdots&&\vdots&&\dots &&\vdots\\
  A x_1&&A x_2&&\dots&&A x_n\\
  \vdots&&\vdots&&\dots &&\vdots\\
  \end{bmatrix}
$$
写数学类的博客就是累，还是贴代码那种技术博客好写。
把X分解成多个列，每列与A的乘积作为结果对应的列，这就是列视角，或者叫做列模型
#### Row Model
有行就有列，有列就有行：
$$
A=\begin{bmatrix}
\dots&& a_1 &&\dots\\
\dots&& a_2 &&\dots\\
\vdots&&\vdots&&\vdots\\
\dots&& a_m &&\dots\\
\end{bmatrix}\\
AX=
\begin{bmatrix}
\dots&& a_1 &&\dots\\
\dots&& a_2 &&\dots\\
\vdots&&\vdots&&\vdots\\
\dots&& a_m &&\dots\\
\end{bmatrix}X
=
\begin{bmatrix}
\dots&& a_1X &&\dots\\
\dots&& a_2X &&\dots\\
\vdots&&\vdots&&\vdots\\
\dots&& a_mX &&\dots\\
\end{bmatrix}
$$
行过程和列过程基本呈现一种对称关系，这也是线性代数有趣的一点，经常是左右开工，得到相同的结果。
#### Block
没错，矩阵是可以切块的，最极端的方式就是每个矩阵按照一个块一个元素的切法，那就和原始矩阵一样了，这种切块粒度太小，如果把一个矩阵当做一块，那粒度又太大，举个一般的例子🌰
继续使用上一节消元的矩阵
$$
E=\begin{bmatrix}
1&&0&&0\\
-3&&1&&0\\
0&&0&&1\\
\end{bmatrix}\\
A=\begin{bmatrix}
1&&x&&x\\
3&&x&&x\\
4&&x&&x\\
\end{bmatrix}\\
EA=
\left[\begin{array}{c|cc}
1&0&0\\\hline
-3&1&0\\
0&0&1\\
\end{array}\right]
\left[\begin{array}{c|cc}
1&&x&&x\\\hline
3&&x&&x\\
4&&x&&x\\
\end{array}\right]\\
=
\left[\begin{array}{c|cc}
1&&x&&x\\\hline
0&&x&&x\\
0&&x&&x\\
\end{array}\right]
$$
分块进行
$$
EA=
\left[\begin{array}{c|c}
I&0\\\hline
-CA^{-1}&I\\
\end{array}\right]
\left[\begin{array}{c|c}
A&B\\\hline
C&D\\
\end{array}\right]\\
=
\left[\begin{array}{c|c}
IA+0C&IB+0D\\\hline
-CA^{-1}A+IC&-CA^{-1}B+D\\
\end{array}\right]
$$

吃完饭码了这些公式，然后我又饿了，这是个消元的过程，E是消元矩阵，我们把3x3矩阵分块成了2x2的块矩阵，分块的规则是，对应要相乘的矩阵规模必须匹配正确（原文match）。然后把块当做元素继续按照乘法法则进行相乘.

***注意：分成的块矩阵相乘与矩阵相乘要求一致，顺序不能互换。$AB\neq BA$***
#### Schur Complement
矩阵分块消元，右下角的那个矩阵，也就是
$$ -CA^{-1}B+D $$
被叫做Schur Complement。
### The Laws
法律说明，你必须遵守法律，矩阵的法律和现实的法律有点不一样，现实的法律是“法不禁止即可为”，但是矩阵的法律如果没说明可以，也没规定不行，那这个law你就要小心使用，最好在论证后进行使用！
$$
A+B=B+A\\
c(A+B)=cA+cB\\
A+(B+C)=(A+B)+C\\
AB \neq BA\\
C(A+B)=CA+CB\\
(A+B)C=AC+BC\\
A(BC)=(AB)C\\
A^p=AA\dots A(p\;factors)\\
A^p\cdot A^q=A^{p+q}\\
(A^p)^q=A^{pq}\\
$$

以上是不完全的矩阵法则

$$
A(B+C)=AB+AC
$$
证明：
逐列进行，比如b，c是B和C的对应两个列
$$A(b+c)=Ab+Ac$$
***这是所有的关键---线性（linearity）***

## 总结
这是矩阵基本操作的一个总结，依法办事，做法律允许的计算才能得到正确的结果，当然也取决于机器的问题，比如后面要写的数值分析，里面就有不少算法精度收到机器的限制，导致合法操作得不到准确结果，各位加油，我们后面继续！
