---
title: 【线性代数】2-1:Ax=b
toc: true
categories:
  - Mathematic
  - Linear Algebra
date: 2017-08-31 15:08:37
tags:
  - row picture
  - column Picture
  - system of equations
---
**Abstract:** 通过不同的角度解方程组$Ax=b$
**Keywords:** row picture，column Picture，system of equations
<!--more-->



## 开篇废话
今天不想说啥废话了，不论外接环境是什么样子的，别人强迫你做什么你认为很没有价值的事，千万稳住，做自己认为对的，有价值的就可以，追逐梦想的道路上无论是否有人陪伴都是充实的，失去方向的剧中狂欢没有任何价值。
## 解方程组
$$
  x-2y=1 \\
  3x+2y=11
$$
同志们，来解方程组，这是小学四五年级的数学题，也是线性代数的核心问题，解方程组，没错2x2的方程组没啥好说的，咔咔咔，就算粗来了，但是200x200的规模就有点大了，所以线性代数知识就有用了。
## Row Picture
不知道Picture这个词本身就是这种含义，还是Pro Strang喜欢这么说，Open Course和书上都是各种各样的Picture。
什么是Row Picture？
看到方程组中的两个等式么，每一行就是一个Picture，或者叫做Graph，在二维坐标系下，表现出来的是一条直线，同样第二个方程也是一条直线，直线上所有的点都满足方程，所以两条直线相交处就是方程组的解。
![row_picture](Math-Linear-Algebra-Chapter-2-1/row_picture.png)

## Column Picture
这个是重点了，因为这个能引出后面一些列的知识，如果我们竖着看，把方程的系数排列整齐，把每个未知数的所有系数按照列向量排列：
$$
x\begin{bmatrix} 1\\1 \end{bmatrix}+y\begin{bmatrix}-2\\2 \end{bmatrix}=\begin{bmatrix}1\\11 \end{bmatrix}=\textbf{b}
$$
怎么样，意外不意外，惊喜不惊喜，和前面讲到的Linear Combination是不是一毛一样，经过scale的两个向量相加得到另一个向量，接着我们就开始寻求scalars了。

***也就是我们通过寻找特定的scalars，来组合出我们规定的 $\textbf{b}$ ***

在图像上，column picture 就变成了两个（或若干个）向量组合得到目标向量了，如图：

![column_picture](Math-Linear-Algebra-Chapter-2-1/column_picture.png)
## Coefficient Matrix
下面矩阵正式出场，我们把上面那两个系数向量挨着拼接起来，就能得到一个系数矩阵(Coefficient Matrix)
$$
A=\begin{bmatrix}1&-2\\3&2\end{bmatrix}
$$
然后写成方程形式就是：
$$
A\textbf{x}=\textbf{b}
$$
其中 $\textbf{x}=\begin{bmatrix}x\\y\end{bmatrix}$,x 和 y就是上面的未知数。
## Matrix · Vector
没错，上面的表示就是一个系数矩阵x向量，更通用一些，我们不在局限于上面的2x2的A，而是放飞自我的A，x也是放飞自我的x，那么
### Row
$$
A\textbf{x}=\begin{bmatrix}
row(1)\cdot \textbf{x}\\
row(2)\cdot \textbf{x}\\
\dots\\
row(n)\cdot \textbf{x}\\
\end{bmatrix}
$$
系数矩阵每一行和未知数向量点乘，得到的就是方程组的原始形式。
### Column
$$
A\textbf{x}=col(1)x_1+rcol(2)x_2+\dots+col(n)x_n\\
$$
系数矩阵的每一列的线性组合
## Identity Matrix
神奇矩阵$I$，$IA=A$看到没，这就是他牛的地方，和谁乘在一起都是谁
$$
\begin{equation}
I=\begin{bmatrix}
1\\
&1 & & \\
&&\ddots\\
&&& 1\\
&&&& 1
\end{bmatrix}
\end{equation}
$$
空白处全是0
左乘右乘都不变！
## 更多未知数
当n超过3的是后row picture就不能picture了，三维以上的就画不粗来了，当然column Picture也画不出来，但是多维向量更容易想象，并不是说column picture比row好，但是从线性代数角度，col的意义更丰富.

## 总结
这是线性最基础，最核心的思想之一，虽然简单，但却是所有知识的源头。
