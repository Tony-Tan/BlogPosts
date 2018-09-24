---
title: 【线性代数】6-1:特征值介绍(Introduction to Eigenvalues)
categories:
  - Mathematic
  - Linear Algebra
keywords:
  - Eigenvalues
  - Eigenvectors
  - Sigular
  - Markov matrix
  - Trace
  - Imaginary Eigenvalues
toc: true
date: 2017-11-14 18:13:04
---

**Abstract:** 线性代数重点，关于矩阵特征值特征向量的相关知识第一篇文章，简单介绍特征值
**Keywords:** Eigenvalues，Eigenvectors，Sigular，Markov matrix，Trace，Imaginary Eigenvalues

<!--more-->
# 开篇废话
线性代数看了也有一段时间了，做题考试怎么样不知道，但是整个框架现在已经渐渐明朗了起来，但是，我发现Prof. Strang的书写到这里才写了一半，也就是说他用半本书详细的讲述了线性代数整个基础知识体系，那么后半本书呢？分别是：application，numerical linear algebra，和complex vectors and matrix。应用和数值计算部分可以说是非常有用的，我记得上学的时候数值计算是一门单独的选修课，这么说我们的课程设计还是完整的，至于没学会，责任我一个人承担😆。
接着说特征值，作为一个靠着图像处理入门的人，对特征值简直就是不可抗拒，说实话，真的对各种特征都有不必崇敬的心理，当然图像的各种特征值和eigenvalue还是不太一样，但是PCA里面就是Eigenvalue，当时简直是，觉得这个太厉害了，因为不懂所以觉的厉害，就像我90岁的姥姥觉得微信不可思议一样，不可思议源于无知，90岁的老人即使不识字不会用电话也不会被嘲笑，因为环境，历史，他们经历了我们无法了解的苦难，但是在象牙塔里天天王者荣耀，以至于什么都不懂，我觉得就应该感觉到羞愧了，没错，我很羞愧，所以在努力补救。
# Introduction to Eigenvalues
我们之前研究了很多关于 $Ax=b$ 的问题，研究针对一个稳定的问题(系统)，当系统处于动态情况下，我们之前的所有知识就不能继续帮助我们了，而eigenvalue在dynamic problem([WIKIPEDIA](https://en.wikipedia.org/wiki/Dynamic_problem))上可以帮助我们解决很多问题（内心os：写到这个我才发现，Stang是从动态问题切入，引出eigenvalue，之前我还没发现，我说怎么一直在研究矩阵的n次方，如果有人问，你有没有好好备课就来写博客，我只能说，每次从新看书都有新发现）
提出问题：$\frac{d\vec{u}}{dt}=A\vec{u}$ 这个微分方程怎么解？解随着时间不断变化，所以消元是不行了，这种问题常见于震荡，衰变等，在这些问题中，我们就要使用eigenvalue和eigenvector的一些厉害的特性了
##  $Ax= \lambda x$
**注意，我们第六章所有矩阵都是方的方的方的。**
$Ax=b$ 我们之前已经详细的讲过了，第一种境界，就是方程组，第二种境界是向量的线性组合，第三境界就是子空间，但是问题是，b是个固定值，也就是说，b一旦确定，我们在子空间内就没办法动了，但是我们现在提出一个新的 $Ax=b'$ 这个 $b'$ 将不再是固定不变的，而且他和变量 $x$ 高度相关，没错
$$
Ax=\lambda x
$$
这个将是我们现行代数后半段的研究核心，从子空间的角度来观察一下这个式子，首先 $x$ 在 $A$ 的column space 中（因为等号右边都是在A的列空间中的），并且x通过矩阵 $A$ 投影到子空间后，方向不变，只是长度变了，这就是一个很好的性质了，我们想一下之前我们在研究projection的时候研究过，如果想把所有向量都投影到一个固定的方向上 $\vec{a}$，需要根据这个方向上的向量来确定投影矩阵[【机器学习基础之线性代数】4-2:Porjections](http://face2ai.com/Math-Linear-Algebra-Chapter-4-2/),这个求出来的投影矩阵对于所有 $\vec{a}$ 方向上的向量都能投影到其本身；而我们的 $Ax=\lambda x$与之相反是已知矩阵A，来求一个向量，能被映射到自己所在的方向上，但是不保证其长度不变，长度伸缩 $\lambda$
上面和projection类比只是为了说明一个是求ptojection矩阵，一个是求方向向量；一个是自身投影到自身，一个是投影到自身并有缩放。
写个表格吧(这部分不是书里的，是我刚想到的)


|  比较项目  |     Projection     |   $Ax=\lambda x$    |
|:----------:|:------------------:|:-------------------:|
|  已知条件  | 方向向量 $\vec{a}$ |      矩阵 $A$       |
|  求解目标  |    投影矩阵 $P$    | 方向向量 $\vec{x} $ |
| 投影后结果 |       $Pa=a$       |   $Ax=\lambda x$    |
| 多次投影后 |      $P^na=a$      |  $A^nx=\lambda^nx$  |




经过上图的对比，可以看出一些问题，尤其是右下角那个式子将会是我们接下来要研究的一个重要表现形式
## $A^n$
哦，sorry忘了给各位介绍了上面说的 $\lambda$ 叫特征值，英文名 Eigenvalue（好像不是英文），$x$ 叫特征向量，英文名 Eigenvector（好像也不是英文）。
我们来观察个矩阵的n次方的例子：
$$
A=\begin{bmatrix}.8&.3 \newline .2&.7\end{bmatrix}\\
A^2=\begin{bmatrix}.70&.45 \newline .30&.55\end{bmatrix}\\
A^3=\begin{bmatrix}.650&.525 \newline .350&.475\end{bmatrix}\\
\vdots\\
A^n=\begin{bmatrix}.6000&.6000 \newline .4000&.4000\end{bmatrix}\\
$$
这个不是我求出来的，我从书上抄过来的，如果有问题别找我，可以看出来，把一个矩阵连续的投影到列空间，经过一段时间后，就会趋于稳定，矩阵就会不在变化，这个结论表面看上去是这样的，但是没有理论证明啊，那么我们就来证明一下，但是首先我们要求一下特征值：
$$
Ax=\lambda x\\
(A-\lambda I)x=0
$$
说明一下，按照上式的形式，并且根据规定其中 $x$ 不是0，我们求解特征值的问题就变成求解矩阵$A-\lambda I$ 了，如果x不是0，那么矩阵就必须是奇异的，也就是说矩阵中列一定是线性相关的，所以null space才会有非0向量，那么奇异矩阵的一个性质就是determinant为0，那么我们的特征值就是：
$$
det(A-\lambda I)=0\\
$$
行列式求法我们已知，其实这就是个n次方程了，n是矩阵的规模(长或者宽)，那么一般情况下我们会得到n个解，注意这里的解可以是0，也可以重复，也可以是复数，这些情况我们后面讨论，但是每个特征值都对应一个特征向量，带回到 $Ax=\lambda x$ 就可以得到两个特征向量了。
那么上面的证明就可以这样写了，这里我们假设矩阵A可以得到两个不同方向的特征向量 $x_1,x_2$，**具体证明后面给出**：
$$
Ax_1=\lambda_1x_1 \dots \dots \dots (1) \\
Ax_2=\lambda_2x_2 \dots \dots \dots (2) \\
AA=\begin{bmatrix}A\cdot col(A)_1 & A\cdot col(A)_2\end{bmatrix}\\
a_1=col(A)_1\\
a_2=col(A)_2\\
a_1=p_1x_1+p_2x_2\\
a_2=q_1x_1+q_2x_2\\
$$

$$
Aa_1=A(p_1x_1+p_2x_2)=p_1Ax_1+p_2Ax_2\\
$$
plug (1) into $Aa_1$:
$$
Aa_1=\lambda_1p_1x_1+\lambda_2 p_2x_2\\
A^2a_1=\lambda_1^2p_1x_1+\lambda_2^2 p_2x_2\\
\vdots \\
A^na_1=\lambda_1^np_1x_1+\lambda_2^n p_2x_2\\
$$
同理可以得到 $a_2$那么最后 $A^n$ 的结果就是:
$$
A^n=\begin{bmatrix}
\lambda_1^{(n-1)}p_1x_1+\lambda_2^{(n-1)} p_2x_2  &
\lambda_1^{(n-1)}q_1x_1+\lambda_2^{(n-1)} q_2x_2
\end{bmatrix}
$$

数学过程基本就是这个样子了，推到应该还算严谨，***思路是把矩阵的列分解到以特征向量为基的表示形式，通过特征向量和矩阵乘积还等于特征向量的这个特征，能够继续迭代进行下去***，如下图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-6-1/特征向量基.png)

图中的数字是$A=\begin{bmatrix}.8&.3\newline .2&.7\end{bmatrix}$ 作为例子来分解的。
最后还要用到极限的一些知识，首先来看两个特征值，$\lambda$ 他们要被n次方，所以如果：
1. 如果 $\lambda>1$ 那么n次方后会非常非常大；
2. 如果 $\lambda=1$ 那么n次方后稳定，还是1  --steady state；
3. 如果 $\lambda<1$ 那么n次方后趋近于0     --decaying state；

## Markov matrix
$A=\begin{bmatrix}.8&.3\newline .2&.7\end{bmatrix}$ 这种矩阵有个统一的称号，Markov matrix，为什么呢，后面可能挺有说明，因为我曾经在跟踪算法见过Markov链之类的算法，应该是一个Markov，特点:***所有列中元素的和是1***，这种矩阵就是Markov 矩阵。Markov矩阵中有一个特征值必然为1，问我为啥？**后面教你算**！

## 两个性质
两个性质🤣，特么我竟然想到了三个代表：
1. 奇异矩阵的特征值必然包含0
2. 2x2 奇异对称矩阵的特征向量们相互正交

证明一下：
奇异矩阵的特征值，我们回忆下求特征值的过程，其实是求 $(A-\lambda I)x=0$ 中 $\lambda$ 的过程，用到的就是矩阵 $(A-\lambda I)$ 是奇异矩阵，那么如果A已经是奇异矩阵了，那么 $\lambda$ 是0 必然是解中的一种情况，这就证明了1；
如果是奇异对称的矩阵，那么当同样根据上面所说的，当 $\lambda=0$ 时，$Ax_1=0$ 表示 $x_1$ 在A的null space中，另外的特征向量 $x_2$ 必然在A的列空间，又因为A对称所以列空间与行空间相同，故而 $x_1$ 与 $x_2$ 正交 ，QED

越特殊的矩阵，其特征值和特征向量越特殊，这是一个非常有用的性质。后面有一张表，可以总览一下。
## Reflection & Projection Matrix
$R=\begin{bmatrix}0&1\\1&0\end{bmatrix}$ 特征值也比较特殊：$\pm1$
$$
R=\begin{bmatrix}0&1\\1&0\end{bmatrix}=2\begin{bmatrix}0.5&0.5\\0.5&0.5\end{bmatrix}-\begin{bmatrix}1&0\\0&1\end{bmatrix}
$$


0.5的Markov矩阵P特征值0和1，所以P是个投影矩阵？为什么，一个特征值是1，说明 $Px_2=x$
这明显就是个投影矩阵，并且Nullspace部分被干掉了（ $Px_1=0$ ）
P 的特征向量(1,1),(1,-1) ,R的特征向量也是(1,1),(1,-1)，所以下面的式子成立了
$$R=2P-I\\
Rx=\lambda_R x\\
Px=\lambda_P x\\
Ix=x\\
Rx=(2P-I)x\\
\lambda_R =2 \lambda_P-1\\
\lambda_{R1} =2 \times 1-1=1\\
\lambda_{R2} =2 \times 0-1=-1$$
Reflection和Projection如下图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-6-1/r&p.png)
上图的理解步骤和求解 $A^n$ 的过程类似，首先分解左侧乘数向量到以特征向量为基的一组基，所以经过A的变换后，一个特征向量上的长度($\lambda=0$)为0，另一个则是1，就是一个分量没了，另一个保持不变，这就是projection的过程，类似的一个长度为-1，另一个不变的就是reflection，如果有不明白的同学可以那笔画一下，也就是上面的图形，也就差不多了。

通过上面一些列推到可以得出，矩阵shift I ，特征值减1，特征向量不变。或者可以理解为当矩阵们拥有相同的特征向量，特征值满足加法原则。

下面说另外两个"代表"
1. 特征值和特征向量之间有关系，当矩阵之间也有关系的时候，比如R和P
2. $R^2$ 的时候 $\lambda^2$

上面这些性质也好，计算也好，看起来都是没什么体系的，也就说按照我们的传统思想，首先下定义，然后学公式，然后做计算，最后老师如果还有时间可能给你来两道应用题，但是美国感觉更感性一些，先从外围把一些有意思的东西先写一下，这样看起来的问题就是没有我们上面那个套路那么规范，但是一旦我们把后面的东西学完了，就会发现，这些东西可以连起来的。

# The Equation for the Eigenvalues
接下来就是“正规操作”的计算了，首先我们需要说一下，我们第六章基本每个计算的第一步都是算$Ax=\lambda x$ ,我们要研究的就是 $(A-\lambda)x=0$ 也就是 $(A-\lambda I)$ 的Nullspace，当我们求出 $\lambda$ 就可以很轻松的求出对应的eigenvector。前面我们说过为了得到非零eigenvector，我们的目标矩阵Nullspace必须存在非零向量，也就是矩阵必须不可逆，也就是 $(A-\lambda I)$ 必须是奇异矩阵。那么就是 $det(A-\lambda I)=0$ 接下来就是解多次方成了，请注意，这里解的方程和我们之前研究的方程组有本质区别，之前研究的 $Ax=b$ 是多元一次方程组，线性，但是我们现在研究的是一元多次方程，所以这个解和前面的解不同，多次方程可能会有重复的解，甚至复数解，这些前面提到一些，但是后面几篇会详细讲解。
>定义 特征值方程:$det(A-\lambda)=0$

对于$n\times n$的矩阵determinant形成的多项式最多n次，所以应该共有n个特征值，这些特征值可以重复，每个特征值指向一个特征向量。

举个计算的例子，我其实在博客里很少举例子，但是这个例子可以帮助我们了解下求eigenvalue的过程：
$$
A=\begin{bmatrix}1&2\\2&4\end{bmatrix}
$$
矩阵奇异，所以0必然是一个特征值，另一个特征值是5，怎么算出来的，当然后面有些其他方法，我们现在就按照equation of eigenvalue来算就行
$$
A-\lambda I=\begin{bmatrix}1-\lambda&2\\2&4-\lambda\end{bmatrix}\\
det(A-\lambda I)=(1-\lambda)(4-\lambda)-4=0\\
\lambda^2-5\lambda=0\\
\lambda_1=0\\
\lambda_2=5
$$
接下来求特征向量:
$$
(A-0 I)x=0\\
\begin{bmatrix}1&2\\2&4\end{bmatrix}x=0\\
x=\begin{bmatrix}2\newline -1\end{bmatrix}\\
$$
然后是另一个：
$$
(A-5 I)x=0\\
\begin{bmatrix}-4&2\\2&-1\end{bmatrix}x=0\\
x=\begin{bmatrix}1\\2\end{bmatrix}\\
$$
从求解可以看出，特征向量是可以伸缩的，也就对应于一个特征值，特征向量可以求出来无数个，但是都在一个方向上，根据子空间也能得出，2x2的矩阵有一个pivot那么他的行空间列空间等于rank=1，那么他的列空间，行空间维度是1，所以nullspace也是1，也就是一条直线，也就是特征向量所在空间只有1维，即一条直线QED

来个大家喜欢的，下面是解题的过程
1. Compute $det(A-\lambda I)$
2. Find the roots of this polynomial
3. Solve $det(A-\lambda I)x=0$ to find an eigenvector x

上面说特征向量是在一个方向上所有向量，那么为了统一，我们可以把这些向量归一化，得到单位的特征向量，Matlab里面是这么做的。

***Warning：这里我们说过特征值可能重复，与之对应，特征向量也可能重复，那么就会产生缺维的现象，也就是特征向量没办法span出矩阵的全部维度，所以有些向量没办法用特征向量组成的基来表示，这类矩阵也没办法对角化***

一个悬崖边写了个"warning，前面是悬崖"，程序员都摔死了。

# Good News,Bad News
“坏消息是我们迷路了，以后只能靠吃牛粪为生，好消息是牛粪有的是”

坏消息是特征值和我们之前讲的消元啊，什么的基本没什么关系，当我们进行elimination的时候特征值改变了，所以pivot一般和特征值也没啥关系（三角矩阵有关系）
好消息是，特征值跟矩阵的其他一些值有关系：
1. n个特征值的乘积等于矩阵行列式的值
2. n个特征值的和等于矩阵的Trace
$$
trace=\sum_{i=1}^{n}a_{ii}
$$
就是矩阵没变换的时候，对角线上的值的和，叫做trace。
上面这两条好消息可以帮助我们在2x2的时候看出来特征值，但是当矩阵维度扩大的时候计算无法避免，但是这两条规则可以帮助我们验算结果是否正确。

上面这些得出一个结论，这一章开始的线性代数已经跟前面没啥关系了，pivot和eigenvalue毫无关系了，而且我们本章的核心 $Ax=\lambda x$ 都不是线性的

对角矩阵的特征值就是，对角线上的元素，为什么，因为行列式的其他项都是零呀，自己找个纸写一下，不会留言，我教你。

# Imaginary Eigenvalues
复数特征值，多次方程出现复数解非常正常，这里还要举个计算的例子：
$$
Q=\begin{bmatrix}0&-1\newline 1&0\end{bmatrix}
$$
根据方程求出$\lambda_1=-i$ 另一个特征值 $\lambda_2=i$
检验$trace=i-i=0$ , $determinant=\lambda_1\lambda_2=-1$  结果正确
计算特征向量:
$$
x_1=\begin{bmatrix}1\\i\end{bmatrix}\\
x_2=\begin{bmatrix}i\\1\end{bmatrix}
$$
纯实数矩阵得到复数特征值特征向量，这事以后会经常出现，但是这个特征值和特征向量也很特殊，并且有些特殊的性质（特殊的特征值对应特殊的性质，这个也是特征值重要的一个原因哦！前面说到过）
1. Q是正交矩阵，所以他的所有特征值的绝对值是1,即 $|\lambda|=1$
2. Q是反对称的矩阵，所以特征值是纯虚数

解释下对称矩阵 $A=A^T$ 是对称矩阵， $-A=A^T$ 是反对称矩阵
上面这些对应的性质都不是巧合，后面有专门的定理来证明这些巧合

# Eigshow in Matlab
这段是讲Matlab中的一个demo ，描述2x2的矩阵的特征值的特征向量一些变化，等我安装个matlab然后录个视频看看效果，如果有补充再来写，所以这段：
略！

# Conclusion
这篇写了两天，也算是良心之做了，可能有点啰嗦有点乱，因为水平不到位，不能很好的判断哪些重要哪些次要，所以每一个知识点都反复说，宁缺毋滥，所以各位海涵吧，明天继续。。





原文地址1：[https://www.face2ai.com/Math-Linear-Algebra-Chapter-6-1](https://www.face2ai.com/Math-Linear-Algebra-Chapter-6-1)转载请标明出处
