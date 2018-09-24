---
title: \[线性代数\]6-4:对称矩阵(Symmetric Matrices)
categories:
  - Mathematic
  - Linear Algebra
keywords:
  - Eigenvalues
  - Eigenvectors
  - Symmetric Matrices
  - Projection Matrices
  - Spectral Theorem
  - Principal Axis Theorem
toc: true
date: 2017-11-22 15:18:03
---

**Abstract:** 本篇继续线性代数的高潮部分，对称矩阵，以及对称矩阵的一些性质
**Keywords:** Eigenvalues，Eigenvectors，Symmetric Matrices，Projection Matrices，Spectral Theorem，Principal Axis Theorem

<!--more-->
# 对称矩阵
这几篇在难度上确实要比前面的内容大很多，所以看书理解和总结都变得不那么流畅了，但是慢慢看下来收获还是有很大的，而且我发现不管学的多认真，还是会有遗漏，所以我觉得之前的想法就是一次性把什么什么学会是不可能的，只能学到自己觉得达到自己能发现的最大限度，等到应用之时还是要回来查阅，这样往往会有进一步的更大发现，

## 对称矩阵 (Symmetric Matrices)
对称矩阵我们在最早的知识里面就学过 $A^T=A$ 的矩阵叫做[对称矩阵](http://face2ai.com/Math-Linear-Algebra-Chapter-4-2/)，我们也学过[投影矩阵](http://face2ai.com/Math-Linear-Algebra-Chapter-4-2/),但是当时我们并没有强调过一点就是投影矩阵都是对称的，这个性质今天在这里会有很大的用途。
我们继续说投影矩阵，所谓投影矩阵，就是在和向量 $\vec{c}$ 相乘的时候，投影到矩阵A的列空间内，那么其中，投影 $p$ 和 原向量 $\vec{c}$ 的差 $\vec{e} =\vec{c}-\vec{p}$ 与子空间正交。

举个例子，在三维空间内，A的列空间是一个二维平面那么，A对应的投影矩阵P能够把任何方向的向量投影到平面上，那么如果向量本身属于平面那么 $Px=x$ 显然是不用质疑的（我们之前在投影那篇文章中也讲过） 但是，同志们，看看这个有木有很面熟啊，这个明显就是投影矩阵 $P$ 的特征值和特征向量么？没错，$P$ 有一个平面的特征向量，可以随便选！能选多少个呢、当然是无数个，但是问题又来了，这无数多个并不是独立的，因为一共就二维，选出来三个线性独立的向量都是不可能的，所以这个平面能选出两个线性独立的特征向量，并且对应的特征值都是1，这里有人可能疑惑为啥要选两个，因为我们[6-2](http://face2ai.com/Math-Linear-Algebra-Chapter-6-2/)的时候说过只有特诊向量足够的情况下才能对角化，投影矩阵明显是个3x3的矩阵，那么特征向量也应该有三个呀！我们的子空间是二维的，所以理论上应该有两个特征向量在上面，剩下一维存在一个，那么这一个也能很好找，$\vec{e}$ 就是 也就是和子空间正交的向量都行 $Px=0x$ 表明 $\vec{x}$ 和子空间正交，那么这是个特征值为0的特征向量，这样我们又进一步规范一下，选择三个特征向量相互正交，这个也是可以做到的，也就是对于矩阵P我们找到了三个相互正交的特征向量，并且长度缩放到单位长度。

以上三维投影到二维平面可以通过几何来解释，但为了能让大家从线性空间来理解，就没用几何方法，大家可以自己脑补。

得出结论，对称矩阵 $P^T=P$ 的特征向量相互正交并且为单位向量。
对称矩阵"It is no exaggeration to say that these are the most important matrices the world will ever see -- in the theory of linear algebra and alos in the applications" 翻译成中文：“对称矩阵是史上最牛B的矩阵，无论在理论还是应用”
这个我们目前还无法考证，还没做过应用呢？不是么，但是我知道PCA中确实用了对称矩阵，SVD等一些列相关技术。
一个矩阵能被如此称赞，不外乎几点原因，首先是其本身拥有较好的性质，其次这个矩阵在自然生活中经常出现，就像正态分布，那么难的公式，却能准确的描述自然届的现象。最后就是如果表现形式简单，那么这个就是非常有用的东西啦。

下面我们开始探索对称矩阵的性质。
如果一个对称矩阵满足：
$$
suppose:\\
A^T=A\\
A=S\Lambda S^{-1}\\
then:\\
A^T=(S^{-1})^T\Lambda^T S^T
$$
这种情况下就有下面这种***可能***了，也就是对应的 $S=(S^{-1})^T$ 注意我们这里说的是可能，并不排除不可能的情况，原文书上用的也是possibly，也就是说我们目前假设:
$$
S^T=S^{-1}\\
S^TS=I
$$
这里我们可以预报一下：
>1. 对称矩阵只有实数特征值
>2. 对称矩阵特征向量可以选择正交单位向量 *orthonormal*


对于 $S^TS=I$ 面熟么？还有印象么？我们认识啊，[正交矩阵 ](http://face2ai.com/Math-Linear-Algebra-Chapter-4-4/)
$Q^TQ=I$ 矩阵Q中每列之间相互正交，也就是我们对于对称矩阵可以写成：
$$
A=Q\Lambda Q^{-1}=Q\Lambda Q^T\\
with:\\
Q^{-1}=Q^T
$$

这个就是著名的普定理 "Spectral Theorem":
> Every symmetric matrix has the factorization $A=Q\Lambda Q^T$ with real eigenvalues in $\Lambda$ and orthonormal eigenvectors in $S=Q$

对于所有对称矩阵都能分解成 $A=Q\Lambda Q^T$ 的形式并且在 $\Lambda$ 中的所有特征值都是实数，其对应的特征向量是正交单位矩阵，即 $S=Q$

普定理在数学中很重要，对应的主轴定理 “Principle Axis Theorem”在集合物理中有重要地位。

这个定理对于从后到前 $A=Q\Lambda Q^T$ 很好证明，但是对于对称矩阵的特征值都是实数和特征向量相互正交比较难证明，也就是说，对角化时，特征向量矩阵是正交矩阵的时候很容易证明原始矩阵是对称的，但是对称矩阵不太容易证明，可以对角化，并且对角化的特征矩阵 $S$ 是正交矩阵 $Q$

***插播一句，orthonormal，翻译成中文是正交，而且normal，因为特征向量是可以随意缩放的，所以主要强调正交性***
我们接下来要做的伟大的证明，分为下面三步来证明：
1. 首先举个例子，来展示下特征值都是实数，特征向量orthonormal
2. 当特征值不重复的时候的证明
3. 当特征值重复的时候的证明

-------
我们来先看个🌰：
$$
A=\begin{bmatrix}1&2\\2&4\end{bmatrix}\\
\begin{vmatrix}1-\lambda &2\\2&4-\lambda\end{vmatrix}=0\\
\lambda^2-5\lambda=0\\
\lambda_1=0\\
\lambda_2=5\\
x_1=\begin{bmatrix}2\newline -1\end{bmatrix}\\
x_2=\begin{bmatrix}1\newline 2\end{bmatrix}
$$
可以看出$x_1$ 属于矩阵的nullspace，而 $x_2$ 属于矩阵的column space，但是我们学四个子空间的时候有nullspace和rowspace是正交的，但是这里的 $x_2$ 属于矩阵的column space，为什么呢？因为矩阵实对称的，所以对称矩阵的rowspace和columnspace一致。
$$
Q^{-1}AQ=
\frac{1}{\sqrt{5}}
\begin{bmatrix}2&1\newline -1&2\end{bmatrix}
\begin{bmatrix}1&2\\2&4\end{bmatrix}
\frac{1}{\sqrt{5}}
\begin{bmatrix}2&1\newline -1&2\end{bmatrix}=
\begin{bmatrix}0&0\newline 0&5\end{bmatrix}=
\Lambda
$$
这就是个简单的例子，但是证明全体元素成立不能靠举一个例子来证明，但证明不成立可以靠举个反例来证明不成立。

-------

下面证明 *所有实数对称矩阵特征值都是实数*

怎么证明一个数是实数呢，只能用点实数的性质，实数的性质不少但是能证明实数是实数的不多，可以想到一个就是实数的共轭是其本身，这也可以说是复数的性质，证明是实数也就是说证明不是复数。
复数的共轭
$$
\lambda=a+bi\\
\bar{\lambda}=a-bi
$$

根据复数的性质，以及向量的基本计算，对于实数矩阵A，满足：

$$
Ax=\lambda x\\
\bar{Ax}=\bar{\lambda x}\\
A\bar{x}=\bar{\lambda}\bar{x}\\
Transpose:\\
\bar{x}^TA=\bar{x}^T\bar{\lambda}\\
for:\\
\bar{x}^TAx=\bar{x}^T\lambda x\\
\bar{x}^TAx=\bar{x}^T\bar{\lambda}x\\
and:\\
\bar{x}^Tx=|x|^2>0\\
so:\\
\bar{\lambda}=\lambda
$$

QED

整个过程证明了特征值的共轭等于原特征值，故特征值是实数被证明了。
证明思路就是通过构造出 $\bar{\lambda}=\lambda$  的结构来证明，主要用到了A是实数矩阵的性质，通过转置和乘法等来完成这个过程。
$(A-\lambda I)x=0$ 可以得出既然特征值是实数，A也是实数，那么x肯定是实数（实数没办法仅通过乘法加法得出复数）

-------

下面证明 *所有实数对称矩阵特征向量相互正交,当特征值不相等的时候*

$$
suppose:\\
Ax=\lambda_1x\\
Ay=\lambda_2y\\
A^T=A\\
(Ax)^T=\lambda_1x^T\\
(Ax)^Ty=x^TAy=\lambda_1x^Ty\\
x^TAy=x^T\lambda_2 y\\
then:\\
\lambda_1x^Ty=x^T\lambda_2 y\\
for:\\
\lambda_1\neq \lambda_2\\
so:\\
x^Ty=0
$$
QED

-------

插播一个小栗子：
$$
A=Q\Lambda Q^T=
\begin{bmatrix}x_1 &x_2\end{bmatrix}
\begin{bmatrix}\lambda_1 &\\&\lambda_2\end{bmatrix}
\begin{bmatrix}x_1^T \\x_2^T\end{bmatrix}=\lambda_1x_1x_1^T+\lambda_2x_2x_2^T
$$
$A=\lambda_1x_1x_1^T+\lambda_2x_2x_2^T$ 把A写成了两个rank=1的矩阵的线性组合，并且这个rank=1的矩阵还是投影矩阵，当然也是对称矩阵，是不是很神奇，这个矩阵分解是比较有意思的，基是矩阵，而且基实对称的rank=1的投影矩阵，是不是很多头衔啊，头衔越多越厉害，不信你去看看大大有多少头衔。这个投影矩阵可以理解为投影到特征向量组成的空间(Eigenspace)

---------

其实写到这我有点疑惑了，本来写博客一个是自己总结，另一个是给大家一个参考，但是当我写了上面那个头衔了以后，我发现，如果没有基础或者不从头看起，直接看本文可能会感到疑惑，这也是知识的一个性质，就是连续性和扩展性，那么没有基础，基本都是空中楼阁（这种大牛太多了，基础没用，直接上算法的比比皆是，我以前也是，我现在改邪归正了）

## 实数矩阵的复特征值(Complex Eigenvalues of Real Matrix)
本文主要说对称矩阵的特征值，特征向量，对称矩阵的特征值和特征向量一定是实数，我们上面基本都为证明这个结论，那么什么样的矩阵会产生复数特征值特征向量呢？
如果一个矩阵包含一个复数特征值，那么一定是成对出现的，所谓成对就是如果 $\lambda=a+bi$ 是一个特征值，那么 $\bar{\lambda}=a-bi$ 也一定是A的一个特征值，证明：
$$
Ax=\lambda x\\
A\bar{x}=\bar{\lambda x}=\bar{\lambda} \bar{x}
$$
上面的取共轭操作hexo渲染有点问题，在两个字母中间的$\bar{\lambda x}$ 其实是个长的,他画的有点短
我们后面有一章专门介绍复数矩阵，所以这里有点迷糊的不要紧，放过自己，继续看下面。

## Eigenvalues 和 Pivots
Pivots是我们这章之前主要研究的对象，因为我们主要研究的是矩阵用于方程组的求解，而本章开始Eigenvalues的研究，那么我们的惯性思维就是，既然是一个体系下的知识重点，那么他们有联系么？
> product of pivots = determinant = product of Eigenvalues

这个是个特殊的性质，结合前面trace的性质我们知道了特征值和矩阵相关的两个算数性质，但是这个具体的证明我还没学会，包括下面的这个结论，证明我也没看懂。

> The number of positive eigenvalues of $A=A^T$ equals the number of positive Pivots

这个结论书上给出了证明，但是说实话，我没明白，所以这个地方必须先留个空白，把书上的东西抄过来没意义

***上述两个结论不会证明，后续补上，此处留坑***

这就是pivots和eigenvalues可以通过上述两个结论产生联系，不过联系应该也就这么多了。

## 所有对称矩阵可以对角化(All Symmetric Matrices are Diagonalizable)

接下来我们要证明最后一个结论，就是在有重复特征值的情况下，对称矩阵依然可以被对角化，其实本文第一个例子中就包含相同的特征值，两个 $\lambda=1$ 虽然如此我们还是在平面中找到了两个相互正交的特征向量，一个特殊的例子没办法证明全部情况，下面我们系统证明一下：
正式的证明之前有个小trick,就是给矩阵对角线上的每个元素加一个扰动 $nc$ 这样所有的特征值不同（***具体为啥我也没想明白,有明白人请指教一下***）所有有不同的特征向量，当 $c\to 0$ 得到原始特征值，和一组不同特征向量（这个思路是Prof. Strang写在书上的，他说这个有点不严谨，但是I am sure this is true）
接下来将正规的证明方法
首先用到一个理论：
> **Schur's Theorem**: Every square matrix factors into $A=QTQ^{-1}$ where T is upper trangular and $\bar{Q}^T=Q^{-1}$ If A has real eigenvalues the Q and T can be chosen real:$Q^TQ=I$

这个定理给出了详细的证明，但比较复杂，我试着简单的证明了一下（如果有不严谨的地方，请各位指出）:
pf:
对于一个矩阵A，它代表的是A的列空间的基的矩阵，我们可以把它进行 $QR$ 分解，得到以Q为基（正交基）的同样的子空间，那么 $AQ$ 将还是这个子空间的基，所以我们可以得到：
$$
AQ=QT\\
so:
A=QTQ^{-1}
$$
上述过程中T是和R类似的上三角矩阵,对于所有方阵成立。
也就是说如果我们把Schur 定理稍微进行变形:
$$
A^T=Q^TT^TQ\\
when:\\
A^T=A\\
T^T=T
$$
因为T是三角矩阵，所以当它也是对称矩阵的时候，必然是对角的。QED
以上是我刚发明的简单的证明方法，对于Schur's Theorem 我们需要找的就是是否存在T使得 $AQ=QT$ 成立，其中Q是正交矩阵（研究证明题必须先把目标搞明白，别看了一路都是对的，就是不知道要干嘛，我刚才就犯了这个毛病，看哪句都是真命题，然后迷迷糊糊不知道要证明啥），下面描述下书上的方法：
我们要寻找Q，满足$AQ=QT$
观察T是上三角矩阵，所以第一列只有一个元素$t_{11}$ 其中 $q_1$ 是Q的第一列,也就是说 $Aq_1=t_{11}q_1$ (这个要是不明白就自己拿笔画个矩阵比划一下子，就知道了)


$$
\bar{Q}^T_1AQ_1=
\begin{bmatrix}
\bar{q}^T_1\\
\vdots \\
\bar{q}_n^T
\end{bmatrix}
\begin{bmatrix}
&&\\
Aq_1&\dots&Aq_n\\
&&
\end{bmatrix}=
\begin{bmatrix}
t_{11}&\dots&\dots&\dots \\
0&&& \\
0&&A_2&\\
0&&&
\end{bmatrix}
$$
观察上面的整个过程$Aq_1=t_{11}q_1$ 其实是A的特征值和特征向量，那么$q_2\dots  q_n$这些值怎么确定？答案是无所谓，只要找一组和$q_1$ 都正交的基就可以填充出其他部分（因为$t_{12}\dots t_{1n}$ $q_1\dots q_n$ 相乘得到A的其他部分，使得等式成立），我们的目的是为了找T，所以第一步我们算是迈出去了，下一步就是找$A_2$ 中的T了，根据假设，我们可以认为 $A_2Q_2=Q_2T_2$ 这里面的矩阵都比上一步少小一号(size=n-1)递归调用上面的证明方法，可以得出$Q_2$ , $t_{22}$ 以及$A_3$
如此递归下去可以找到所有A的第一个特征向量，然后组合成Q，T
$$
Q=Q_1\begin{bmatrix}1&0\\0&Q_2\end{bmatrix}\\
T=\begin{bmatrix}t_{11}&0\\0&T_2\end{bmatrix}\\
AQ=QT
$$
以上可证存在T使得 $AQ=QT$ 成立，那么如果A是symmetric的，那么:
$$
A=QTQ^T\\
A^T=Q^TT^TQ\\
A=A^T\\
so:\\
T=T^T
$$
T是上三角矩阵，得出结论，T是对角矩阵
QED
上面这一小段其实在证明Schur定理，因为Schur定理一旦得到证明，那么自然可以得到我们想要的结论，所有对称矩阵可以被对角化，Schur矩阵以复数形式给出，因为我们前面已经证明了对称矩阵的特征值都是实数，所以这里可以用实数表达，当然复数是对于非对称矩阵的，因为非对称实数矩阵可能得到复数特征值和特征向量。
## Conclusion
这篇文章扎扎实实写了24小时，而且中间确实有不太清楚的地方，至今没动，所以都高亮标注了，提醒读者也提醒自己要来填坑，schur定理的证明方法还有别的，这个是Prof. Strang 书上的方法，后面如果有新发现继续补充。





原文地址1：[https://www.face2ai.com/Math-Linear-Algebra-Chapter-6-4](https://www.face2ai.com/Math-Linear-Algebra-Chapter-6-4)转载请标明出处
