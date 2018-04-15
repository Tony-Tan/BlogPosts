---
title: 【线性代数】5-1:行列式性质(The Properties of Determinants)
categories:
  - Mathematic
  - Linear Algebra
tags:
  - Determinants
  - Properties of the Determinants
toc: true
date: 2017-11-02 10:34:30
---

**Abstract:** 本文介绍矩阵的行列式相关性质
**Keywords:** Determinants，Properties of the Determinants

<!--more-->
## 开篇废话
各位老铁，我又回来了，这几天干嘛了？好基友在做天池的比赛，作为一个好事者，我也弄了两天，结果迷茫人生了。
个人认为很简单的问题发现做出来结果并不怎么样，分析了一下原因，首先自己能力并没有自己想的那么强，也没有别人吹捧你的那样，唯一能证明实力的就是自己做出来的结果，这个是不骗人的，怎么吹85%就是不如90%，所以实践是检验真理的唯一标准这句话放之四海而皆准；其次是做事的态度，不要把所有事都看的很简单，因为里面各种各样的坑到处都是，没做根本没办法完全看出来坑在哪，就像风平浪静的江面下面隐藏了各种暗流；
第三就是做事要认真，别想着投机取巧，自己随便拿了一段网上的代码（sklearn就用了一句，根本没有优化设计细节）然后就想得到好结果，典型的不劳而获的投机思想。
这就是这半个月干了啥，然后还把MIT的多变量微积分课程看完了，做题错的一塌糊涂（看来还要再看一遍书）。
## Determinants(行列式)
### 懵逼场景1
啊。。行列式。。大学学线性代数第一课就是这个，各位老铁啊，你们知道一脸懵逼是什么样子么？我当时就是，这是个啥玩意？怎么就跑出来了，然后我清楚的记得，老师告诉我们线性代数就是解方程，但是各位啊这特么行列式跟方程有毛关系啊。
### 懵逼场景2
前两天看MIT的多变量微积分，极坐标代换的时候,例如：
$$
\int \int_R xdA=\int \int_R xdxdy\\
x=cos(\theta)r\\
y=sin(\theta)r\\
\int \int _R cos(\theta)r \cdot rdrd\theta
$$
这是没错的过程，但是如果我们正常的带入一下好像不太对：
$$
dx=-sin(\theta)rd\theta+cos(\theta)dr\\
dy= cos(\theta)rd\theta+sin(\theta)dr
$$
带入到原式：
$$
\int \int_R cos(\theta)r d(cos(\theta)r)d(sin(\theta)r)\\
=\int \int_R cos(\theta)r \cdot (-sin(\theta)rd\theta+cos(\theta)dr) \cdot (cos(\theta)rd\theta+sin(\theta)dr)\\
=\int \int cos(\theta)r(-sin(\theta)cos(\theta)r^2d^2\theta+cos^2(\theta)rdrd\theta-sin^2(\theta)rd\theta dr+sin(\theta)cos(\theta)d^2r)
$$
什么情况，这个带入看起来没啥问题啊,但是这个结果肯定不对，二重积分可以简单看做是面积的求和，所以这个小面积有问题（就是被积分的部分有问题，具体的下面继续），微积分教材在这里引入了雅克比矩阵，因为啥他们没说就说要这么算，所以我们到线性代数上寻找一下结果

### Determinants is a Number
行列式最后得到了一个数字，行列式应该称为一种运算，像加减法一样，结果是个数字，对于矩阵A， $det(A)$ 这个数字包含了很多关于矩阵A的很多信息，比如矩阵是否可逆，并且有一些有趣的几何物理这是计算行列的是用途，行列式具有很多性质，就像加法有交换和结合的性质一样，行列式的性质就是今天我们主要要写的内容，至于解决懵逼场景，学完这章，大家都应该豁然了。
求行列式的方法主要有三种:
1. pivot formula :通过pivots这个是电脑的计算方法，通过消元(elimination)得到pivots，通过pivots的乘积得到determinant，如果矩阵rank小于行或列，矩阵是奇异的，这时候没有逆，determinant是0（因为pivots数量等于rank，小于行或者列，那么剩下的pivots位置是0所以乘起来就是0）
2. big formula :哈哈哈，就是我大学课本上写的，就是通过一个比较复杂的公式给出计算方法
3. cofactor formula:代数余子式的方法

到目前为止，任何关于行列式长什么样我们都不知道，也不知道他能干啥，但是好像有很多不错的性质，所以Pro. Stang说，孩子们，直接看定义没什么意思，我们来通过性质来学习下吧。

这种方式让我想到了陶哲轩大神的analysis（中文译：《实分析》）目前只看了第一章，通过皮阿诺公理来推到出自然数的加法乘法。。。可以充分的感受到这些大师们对我们小白的爱，他们洗的书有点“来吧，孩子，我来告诉你怎么来学会这些知识”，读起来比较友好。

## Properties of the Determinant
上面废话那么多，下面开始学术一点了，一下的性质中前三个是basic properties（rule 1 2 3），可以用前三个推导出后面的所有。
行列式的写法， $det(A)$ 或者 $|A|$
$$
det(\begin{bmatrix}a&b\\c&d\end{bmatrix})=\begin{vmatrix}a&b\\c&d\end{vmatrix}
$$
讲了大半天，终于知道行列式长什么样了，下面看性质了

1.单位矩阵的行列式 $det(I)=1$
$$\begin{vmatrix}
1&&\\
&\ddots&\\
&&1
\\\end{vmatrix}=1$$
2.交换行的时候，行列式结果变换正负号
$$\begin{vmatrix}
a&b\\
c&d
\\\end{vmatrix}=
-\begin{vmatrix}
c&d\\
a&b
\end{vmatrix}
$$
eg. 结合rule1 和rule2 能够得到一个关于permutation matrix P的性质，因为P是单位矩阵I经过行变换来的，所以 $det(P)=1$ 如果做了偶数次行变换， $det(P)=-1$ 如果做了奇数次行变换
3.行列式对于矩阵中的每个行是线性的，注意这里说的是对矩阵中每个行是线性的，并不是对整个矩阵是线性的，比如最明显的: $ det(I+I)\neq det(I)+det(I) $ , 这里的线性也表现在行乘法和行加法
$$
\begin{vmatrix}
ta&tb\\
c&d
\end{vmatrix}=
t\begin{vmatrix}
a&b\\
c&d
\end{vmatrix}\\
\begin{vmatrix}
a+a'&b+b'\\
c&d
\end{vmatrix}=
\begin{vmatrix}
a&b\\
c&d
\end{vmatrix}+
\begin{vmatrix}
a'&b'\\
c&d
\end{vmatrix}
$$
上面的性质跟面积和体积非常相似，比如你把一条边扩大t倍，体积也被扩大了t倍，并且单位矩阵的行列式等于1，这些都和体积（多于3维的我们也暂时叫体积吧）
上面三条已经能完全决定行列式的值了，到这我们就可以通过性质推到出 $n \times n$ 的矩阵的行列式的值了，但是那就没啥意思了，继续推到其他性质可能会更有帮助（原文说直接推到行列式的结果有点难，我认为大致思路就是通过乘法和加法分解成  $det(I)$ 的线性组合）
4.行列式的两行相等，行列式的值是0
$$
\begin{vmatrix}a&b\\a&b\end{vmatrix}=0
$$
证明方法很多，书上用到rule2，交换两行，变换符号后发现
$$
\begin{vmatrix}a&b\\a&b\end{vmatrix}=-\begin{vmatrix}a&b\\a&b\end{vmatrix}
$$
所以: $\begin{vmatrix}a&b\\a&b\end{vmatrix}=0$
这里有个延伸，在Boolean Algebra中上述论证不成立，因为-1=1，啥意思？c++中的bool变量除了0其他任何值都是1，所以-1=1，这时候基础规则变成了1，2，4，这三条规则可以构建boolean algebra中行列式。
5.某一行减去另一行的倍数，行列式不变
$$
\begin{vmatrix}a&b\\c-\ell a&d-\ell b\end{vmatrix}
=\begin{vmatrix}a&b\\c&d\end{vmatrix}
$$
证明过程:
$$
\begin{vmatrix}a&b\\c-\ell a&d-\ell b\end{vmatrix}
=\begin{vmatrix}a&b\\c &d \end{vmatrix}+
\begin{vmatrix}a&b\newline -\ell a&-\ell b\end{vmatrix}
=\begin{vmatrix}a&b\\c &d \end{vmatrix}
-\ell\begin{vmatrix}a&b\\a&b\end{vmatrix}
=\begin{vmatrix}a&b\\c&d\end{vmatrix}
$$
使用到了rule3 ，rule4。
6.当行列式中有一行都是0的时候，行列式为0
$$
\begin{vmatrix}0&0\\c&d\end{vmatrix}=0\\
\begin{vmatrix}a&b\\0&0\end{vmatrix}=0
$$
证明使用rule3 和rule4 可以轻松得到（把非0行加到0行，得到rule4的条件）
7.三角矩阵A， $det(A)=a_{11}a_{22}\dots a_{nn}$
$$
\begin{vmatrix}a&b\\0&d\end{vmatrix}=ad\\
\begin{vmatrix}a&0\\c&d\end{vmatrix}=ad
$$
证明也不难，用rule5 能够得到一个对角矩阵（消掉b），再用rule3和rule1,就得到了结果
8.奇异矩阵的行列式为0，可逆矩阵的行列式不等于0

证明前面应该提到了，从消元的角度来说，当一个矩阵的行列式是0证明其中有pivot不存在（在矩阵中用0填充）这样的话行列式存在全是0的行，所以行列式为0.
这里利用rule7加上消元就能得到行列式的一种算法：
$$
\begin{vmatrix}a&b\\c&d\end{vmatrix}=
\begin{vmatrix}a&b\\c-a \cdot \frac{c}{a}&d-b \cdot \frac{c}{a}\end{vmatrix}=
\begin{vmatrix}a&b\\0&d-b \cdot \frac{c}{a}\end{vmatrix}=a(d-b \cdot \frac{c}{a})=ad-bc
$$
第一步使用rule5 ,然后是rule7
值得注意的是消元过程可能会有permutation的过程那么 $PA=LU$ 可以得到：
$$
det(P)det(A)=det(L)det(U)\\
det(P)=\pm 1\\
det(L)=1\\
det(A)=\pm det(U)
$$
9.det(AB)=det(A)det(B)

这个看起来好像挺正常，但证明确有些困难，证明2x2的矩阵都要算上一会儿，但是这时候Pro. Stang给出了个snappy的方式，证明：当 $det(B)\neq 0$ 的情况下， $D(A)=\frac{det(AB)}{det(B)}$ 满足rule1 rule2 rule3，那么D(A)就是det(A）
(**这个地方的逻辑我有点混乱，为啥通过他满足1，2，3就知道他一定是det(A)呢**:
解答一下，想了一下想明白了，我们是怎么定义的determinant的？没有定义？没错，说到现在我们根本没说过行列式的定义，给出了表达形式，和8个性质，其中前3（4）个性质是basic，后面为衍生的，也就是说定义行列式从目前来讲是通过前三个性质来定义的，所以满足性质者就是（条件中矩阵的）行列式)

property 1： 如果 $A=I$  那么 $\frac{det(B)}{det(B)}=1$ 看起来rule1 没什么问题
property 2： 如果A中的任意两行进行交换，那么根据矩阵乘法，AB的中两行也进行了交换，那么 $\frac{det(AB)}{det(B)}$ 要变号，rule2 满足
property 3： 先看乘法，A其中一行乘以一个系数 $\ell$ ，根据乘法法则AB的结果也在同一行乘以了 $\ell$ ，那么乘法满足；继续看加法如果在A中的一行加上一个行向量那么：
$$
\begin{bmatrix}
a+a'&b+b'\\
c&d
\end{bmatrix}
\begin{bmatrix}
e&f\\
g&h
\end{bmatrix}=
(\begin{bmatrix}
a&b\\
c&d
\end{bmatrix}+
\begin{bmatrix}
a'&b'\\
0&0
\end{bmatrix})
\begin{bmatrix}
e&f\\
g&h
\end{bmatrix}=
\begin{bmatrix}
a&b\\
c&d
\end{bmatrix}
\begin{bmatrix}
e&f\\
g&h
\end{bmatrix}+
\begin{bmatrix}
a'&b'\\
0&0
\end{bmatrix}
\begin{bmatrix}
e&f\\
g&h
\end{bmatrix}
$$
这样来看 $\begin{bmatrix}a&b\\c&d\end{bmatrix}\begin{bmatrix}e&f\\g&h\end{bmatrix}$ 就是AB, $\begin{bmatrix}a'&b'\\0&0\end{bmatrix}\begin{bmatrix}e&f\\g&h\end{bmatrix}$ 只有第一行有数字，通过简单的计算得出，A中加上一行，AB中也加上了一行，满足property3（如果这个地方看着有点乱，那个笔算一下）
也就是上面给出证明对于矩阵A的一种计算D满足rule1，rule2，rule3，那么我们确定D是关于A的行列式操作，证毕
所以 $det(AB)=det(A)det(B)$ .并且衍生出了当 $B=A^{-1}$  是 $AA^{-1}=I$ 那么 $det(A)det(A^{-1})=1$
10.$det(A^T)=det(A)$ 矩阵的determinant对于矩阵的转置保持不变性，
证明过程如下：
对A进行分解：$PA=LU$ 两侧同时转置 $A^TP^T=U^TL^T$
根据rule9： 原始分解形式 $det(P)det(A)=det(L)det(U)$ 转置后 $det(P^T)det(A^T)=det(L^T)det(U^T)$

首先 $det(P)=det(P^T)=\pm 1$
其次 $det(L)=det(L^T)=1$ 对角线上全是1
第三 $det(U)=det(U^T)$ 对角矩阵，determinant就是对角线乘积，转置前后不变rule7； 所以 $det(A)=det(A^T)$ 证毕


rule10，相当于把前面的rules扩大了一倍，因为所有的对行性质都能扩展到对列的了，比如一列都是0 的行列式结果是0

## 总结
今天臭不要脸的把自己的博客发给了爱可可老师，结果爱老师还真就帮忙转发了，希望今天能看到留言，三个月了，一条留言都没有，太尴尬了。还有网友留言问有没有微积分和数学分析，概率论。哈哈。这都是我要写的，这样来说还是有人愿意学知识的，明天继续。。。。
