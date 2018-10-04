---
title: 【线性代数】5-2:置换和余因子(Permutations and Cofactors)
categories:
  - Mathematic
  - Linear Algebra
keywords:
  - Determinants
  - 'Pivot Formula'
  - 'Big Formula'
  - 'Cofactors Formula'
  - Cofactors
  - Permutations
toc: true
date: 2017-11-03 09:50:36
---

**Abstract:** 行列式的几种求法，以及相关的衍生问题
**Keywords:** Determinants，'Pivot Formula'，'Big Formula'，'Cofactors Formula'，Cofactors，Permutations

<!--more-->
# 置换和余因子
今天写的是行列式的三种计算方法，瞬间想到了孔乙己的茴香豆的四种写法，一个多少有点文化的人（被老师们解读为迂腐）却被一些没什么文化的人嘲笑挖苦；如果孔乙己是个那个时代的悲剧，那我们自己会不会成为这个时代的悲剧呢？读书无用论，某首富的“北大，清华大不如胆大”论，如果思维继续，结果最后肯定是喜闻乐见
## 主值方程
Pivot的方式求行列式的值，Pro. Stang说这是matlab的做法，也就是计算机求行列式一般通过消元后得到Pivot，然后将所有Pivots相乘，得到行列式的值，这里有个主意的地方，我们反复强调，如果不是满rank的话，Pivot必然在某些行或者列里面不存在，那么这个矩阵是奇异矩阵，行列式值为0。
能够支持Pivot的乘积等于行列式的原因是上文关于[properties](http://face2ai.com/Math-Linear-Algebra-Chapter-5-1/) 中Rule5 是消元的主要过程，rule5 告诉我们消元前后行列式的值不变，但是有的时候我们不光要消元还要进行行交换，这个是随机次数的，所以行列式的值等于Pivot乘积的前面正负号不明确，故:
$$
det(A)=\pm p_{11}p_{22}\dots p_{nn}
$$
从另一个角度讲，如果把消元过程用矩阵方式表达 $PA=LU$ LU分解的矩阵形式，通过rule8 ，就能知道
$$
det(P)det(A)=det(L)det(U)\\
det(P)=\pm 1\\
det(L)=1\\
det(A)=\pm det(U)
$$
这样的话，U的对角线是由Pivot组成的，这个就是Pivot Formula的另一个切入点，都能证明行列式的pivot formula的正确性。
Pivot过程就是消元的过程，通过消元，得到行列式的值。
通过相乘的过程我们还能得到一个子矩阵的行列式，比如矩阵$A$的左上角的一块小的矩阵 $A'$ 他的行列式等于这个子矩阵覆盖的pivot的值（没有行变换）
$$
det(A')=p_{11}p_{22}\dots p_{kk} \\
if \, det(A'')=p_{11}p_{22}\dots p_{k-1k-1}\\
p_{kk}=\frac{det(A')}{det(A'')}
$$

## 大方程
Pivot 形式的行列式求法计算量小，但是有个问题就是不直观，不是直接得到行列式的值，而是通过一个中间过程-Pivot-的消元过程，那么有没有直接的求法，没错，数学家们就是这么不满足于已有的形式，又发明了个直观，但是超级复杂的式子，这个式子有多少项呢，对于一个$n \times n$ 的矩阵，big formula 有 $n!$ 项，这下怕了吧，11x11的矩阵有百万项，所以这个方法我觉得就学会3x3一下的矩阵就行。
那么这个big formula是怎么来的呢？我们可以回忆下，行列式是可以被打碎的：
$$
det(A)=
\begin{vmatrix}
a,b,c\\d,e,f\\g,h,i
\end{vmatrix}=
\begin{vmatrix}
a,0,0\\d,e,f\\g,h,i
\end{vmatrix}+
\begin{vmatrix}
0,b,0\\d,e,f\\g,h,i
\end{vmatrix}+
\begin{vmatrix}
0,0,c\\d,e,f\\g,h,i
\end{vmatrix}
$$
我有点后悔了，因为选的有点大，所以下面只做分解后的第一项，  $\begin{vmatrix}a,0,0\\d,e,f\\g,h,i\end{vmatrix}$ 继续分解:
$$
\begin{vmatrix}
a,0,0\\
d,e,f\\
g,h,i
\end{vmatrix}=
\begin{vmatrix}
a,0,0\\
d,0,0\\
g,h,i
\end{vmatrix}+
\begin{vmatrix}
a,0,0\\
0,e,0\\
g,h,i
\end{vmatrix}+
\begin{vmatrix}
a,0,0\\
0,0,f\\
g,h,i
\end{vmatrix}
$$
没错，每次分解都会产生n个分解结果，下面还是只分解第一项 $\begin{vmatrix}a,0,0\\d,0,0\\g,h,i\end{vmatrix}$ ，第二三项省略：
$$
\begin{vmatrix}
a,0,0\\
d,0,0\\
g,h,i
\end{vmatrix}=
\begin{vmatrix}
a,0,0\\
d,0,0\\
g,0,0
\end{vmatrix}+
\begin{vmatrix}
a,0,0\\
d,0,0\\
0,h,0
\end{vmatrix}+
\begin{vmatrix}
a,0,0\\
d,0,0\\
0,0,i
\end{vmatrix}
$$
分解出来是这个形状的，我们只做了下图中绿色的部分

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-5-2/行列式分解.png)

我们只做了27个行列式中的3个，故只做了1/9。但是这个分解的结果显示最后的行列式都为0，因为总有一列全是0，但是这种情况并不是所有人都是0，这里我不再继续分解别的行列式了，但是我给出最后不是0的行列式（下图中红色为非零行列式）

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-5-2/非零.png)

所以我们可以得到，一定是每个行选出一个元素，每个列也只能选出一个元素组成的行列式才是非零的，那么这就是一个组合的问题，也就是说，第一行我们可以选择任意一列，故有n种方式，第二行只能有n-1列的选择余地，以此类推到最后一行没有自由度，只能选最后的一个，所以，有  $n!$ 个项，那么怎么求呢？

$$
det(A)=\\
\begin{vmatrix}
a_{11}&&\\
&a_{22}&\\
&&a_{33}
\end{vmatrix}+
\begin{vmatrix}
a_{11}&&\\
&&a_{23}\\
&a_{32}&
\end{vmatrix}+
\begin{vmatrix}
&a_{12}&\\
a_{21}&&\\
&&a_{33}
\end{vmatrix}\\
+\begin{vmatrix}
&a_{12}&\\
&&a_{23}\\
a_{31}&&
\end{vmatrix}+
\begin{vmatrix}
&&a_{13}\\
&a_{22}&\\
a_{31}&&
\end{vmatrix}+
\begin{vmatrix}
&&a_{13}\\
a_{21}&&\\
&a_{32}&
\end{vmatrix}\\
=a_{11}a_{22}a_{33}\begin{vmatrix}
1&&\\
&1&\\
&&1
\end{vmatrix}+
a_{11}a_{23}a_{32}\begin{vmatrix}
1&&\\
&&1\\
&1&
\end{vmatrix}+
a_{12}a_{21}a_{33}\begin{vmatrix}
&1&\\
1&&\\
&&1
\end{vmatrix}\\
+a_{12}a_{23}a_{31}\begin{vmatrix}
&1&\\
&&1\\
1&&
\end{vmatrix}+
a_{13}a_{22}a_{31}\begin{vmatrix}
&&1\\
&1&\\
1&&
\end{vmatrix}+
a_{13}a_{21}a_{32}\begin{vmatrix}
&&1\\
1&&\\
&1&
\end{vmatrix}
$$

累死我了。。。这么多公式，最后总结就是选出来不是一行一列的元素相乘，再乘上对应的permutation的行列式值 $\pm1$ ，最后大家加在一起，对于二维矩阵就是主对角线，减去另一条对角线。
$$
det(A)=\sum det(P)a_{1\alpha}a_{2\alpha}\dots a_{n\alpha}
$$
够big的了，这个方法以后尽量不提了，出了三乘三一下的矩阵，谁用这个方法估计是脑子进水了。。再见

## 代数余子式方程 
说了不提上面的公式，但是还是要提一下，我们观察上面的第二幅图，其实每个非零分支都能看成是一个小的矩阵，这个小的矩阵去掉了上一步分解过程中被选中的元素所在的那一行，和那一列（因为后面所选出来的元素不能与之前的元素在同一行或者同一列，这样被首先选中的那个元素的所在行和列的所有元素没有存在的意义）剩下的行列组成了一个小一号的矩阵，也就是下一个分支（不会去包含选中元素的那行或者那列的所在分支），这个分支的求解也是按照原始规则，选择一个元素，并去掉这一行这一列所有元素得到一个更小一号的矩阵。
描述有点模糊，下面是算法过程
1. 选择任意一个元素
2. 去掉该元素所在行或者列的其他元素后形成一个小一号的矩阵
3. 重复到1直到2中的小一号矩阵只有一个元素

这个迭代的过程就是big formula的一种算法实现，但是我们重新定义一下，小一号的那个矩阵的行列式我们称为 cofactor
所以公式就变成了
$$
det(A)=a_{i1}C_{i1}+a_{i2}C_{i2}+\dots +a_{in}C_{i1n}
$$
这是按照行元素来划分的行列式，也可以按照列，或者按照任意方式，但是前提是这些a不可以在同一行或者同一列。
这些cofactor的符号怎么求？看big formula里面的说明吧，就是permutation行列式决定的。
## 总结
这节计算课可以总结为pivot formula利用rule5 和 rule 7 就能推导出determinant的值和pivot乘积相等，从而可以通过消元elimination得到determinant，然后就是big formula的计算方法了，通过优化big formula 的过程就得到了cofactor的计算方法，同时得到了个cofactor的定义，明天继续。。





原文地址1：[https://www.face2ai.com/Math-Linear-Algebra-Chapter-5-2](https://www.face2ai.com/Math-Linear-Algebra-Chapter-5-2)转载请标明出处
