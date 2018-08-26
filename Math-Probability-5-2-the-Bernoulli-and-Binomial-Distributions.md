---
title: 【概率论】5-2:伯努利和二项分布(The Bernoulli and Binomial Distributions)
categories:
    - Mathematic
    - Probability
keywords:
    - Bernoulli Distributions
    - Binomial Distributions
toc: true
date: 2018-03-27 21:15:22
---

**Abstract:** 本文介绍Bernoulli Distribution （伯努利分布）和Binomial Distribution（二项分布）
**Keywords:** Bernoulli Distributions，Binomial Distributions

<!--more-->
## 开篇废话
吐血更，一天三篇，虽然上一篇只能算一段，但是确实应该加快总结的步伐了，给后面的新内容腾出足够的时间
>一杯敬自由，一杯敬死亡

在本章的开始，我们从离散分布下手，看看每个分布有这什么样的特点，然后用我们的工具分析研究其内在的性质，当然要从最简单的开始，逐步构建出我们要研究的有代表性的这些分布，第一个被处理的就是伯努利分布（bernoulli Distribution）
随机变量 $X$ 只有两个取值，0或者1，并且取1的概率固定是$p$ 那么我们就说 $X$ 有一个参数为 $p$ 的伯努利分布。如果我们只知道试验输出对应的随机变量只有两个结果，非此即彼，那么这个随机变量的分布就是伯努利族中的一个随机变量。
如果随机变量 $X_1,X_2,\dots,X_n$ 有相同的伯努利分布，他们的和就是其中为1的随机变量的个数，这个个数也是随机的，其对应的分布为二项分布。
## The Bernoulli Distributions
上来先来个例子：

------------------

临床试验，对于某种治疗，我们简单的把结果划分成两种，一种有效，一种无效，我们用随机变量来表示这两个结果，$X=1$ 表示治疗有效 $X=0$ 表示治疗无效，那么我们要做的是得到这个概率就是 $Pr(X=1)=p$ 的值就是我们关心的结果。$p$ 的取值范围在 $[0,1]$ 对应于不同的 $p$ 我们就有了伯努利分布族。

------------------

>Definition Bernoulli Distribution.A random variable X has the Bernoulli distribution with parameter $p$ ( $0\leq p\leq 1$ )if X can take only the values 0 and 1 and the probabilities are
$$
Pr(X=1)=p
$$
and
$$
Pr(X=0)=1-p
$$

其概率函数可以被写成：
$$
f(x|p)=
\begin{cases}
p^x(1-p)^{1-x}&\text{ for }x=0,1\\
0&\text{otherwise}
\end{cases}
$$
p.f.的表示方法可以看出伯努利分布是依赖于参数 $p$ 的，所以 $p$ 可以看成一个条件，那么我们后面所有类似的分布都可以将其p.f.或者p.d.f.写成这种形式。
c.d.f.（似乎我们学c.d.f的时候已经讲过了）可以被写成：
$$
F(x|p)=
\begin{cases}
0&\text{ for }x<0 \\
1-p&\text{ for }0 < x < 1 \\
1&\text{ for }x\geq 1
\end{cases}
$$
### Expectation
当我们研究完其p.f.和c.d.f.以后就研究研究他的期望吧，也没啥可研究的了，随机变量 $X$ 有参数为 $p$ 的伯努利分布，那么其期望：
$$
E(X)=p\times1 + 0\times(1-p)=p
$$
然后我们研究一下随机变量 $X^2$ 的概率分布
$$
E(X^2)=p\times1^2 + (1-p)\times0^2=p
$$

### Variance
期望完了当然是方差了，同样是随机变量 $X$ 有参数为 $p$ 的伯努利分布，那么其方差：
$$
Var(X)=E[(X-E(X))^2]=(1-p)^2p+(-p)^2(1-p)=p(1-p)(1-p+p)=p(1-p)
$$
或者通过更简单的公式：
$$
Var(X)=E[X^2]-E^2[X]=p-p^2=p(1-p)
$$
结果一致。

### m.g.f.
我们说过除了p.d.f./p.f.和c.d.f.，m.g.f.也是非常重要的分布标书工具，所以伯努利分布自然也有m.g.f.
$$
\begin {aligned}
\psi(t)=E[e^{tX}]=p(e^{t\times 1})+(1-p)(e^{t\times 0}) &\text{  for } -\infty<t<\infty
\end {aligned}
$$
这个写起来应该没啥难度，注意好 $X$ 就行，然后就是期望对应的概率值。

### Bernoulli Trials/Process
说到序列我就想起了数学分析，Tao的分析我们已经开始更新了，但是我想把概率基础部分先写完，然后一边研究数理统计一边写分析的博客，想到分析的原因是我看到了序列
如果一个序列不论是否有限，每一个元素都是独立同分布的（i.i.d.）的伯努利随机变量，那么我们就叫他们伯努利序列或者伯努利过程。

>Definition Bernoulli Trails/Process.If the random variables in a finite or infinite sequence $X_1,X_2,\dots$ and i.i.d.,and if each random variable $X_i$ has the Bernoulli distribution with parameter p,then it is said that $X_1,X_2,\dots$ are Bernoulli trials with parameter $p$ .An infinite sequence of Bernoulli trials is also called a Bernoulli Process.

伯努利过程的例子最简单的就是连续丢同一枚硬币，组成的结果正反，就组成了伯努利过程。
## The Binomial Distributions
举个例子，这个例子和上面伯努利过程有关，连续生产一批零件，每个零件有一定的合格率，，所有零件组成的序列是一个伯努利过程，那么么我们想知道这些随机变量的和满足怎么样的分布。

>Definition Binomial Distribution.A random variable $X$ has the binomial distribution with parameters $n$ and $p$ if $X$ has a discrete distribution for which the p.f. is as follow:
$$
f(x|n,p)=
\begin{cases}
\begin{pmatrix}n\\x\end{pmatrix} p^x(1-p)^{n-x }&\text{ for }x=0,1,\dots\\
0&\text{otherwise}
\end{cases}
$$
in this distribution ,$n$ must be a positive integer, and $p$ must lie in the interval $0\leq p\leq 1$

这个定义确实是以定义的语言风格来写的，直接明了的告诉你，什么东西，叫什么名字，来源出处并不是定义要阐述的，但是我们要从理论上分析为啥这就是二项分布了呢？二项分布首先是因为这个分布产生系数和二项式系数一致，而且中有两个项，而其来源是多个独立同分布的伯努利分布随机变量求和结果。

***注意：二项分布是概率论和数理统计的重要基础！***

>Theorem If the random varibales $X_1,\dots,X_n$ from $n$ Bernoulli trials with parameter $p$ ,and if $X=X_1+\dots+X_n$ ,then $X$ has the binomial distribution with parameters $n$ and $p$

这个定理的证明用到的是前面计数方法以及乘法法则，加法法则，也就是 $n$ 个样本中每一个都有 $p$ 的概率是1，其余是0，总和是 $x$ 的组合方法共有 $\begin{pmatrix}n\\x\end{pmatrix}$ 种，所以把这些种概率 $p^x(1-p)^{n-x }$ 相加就得到了结果，被定义为二项分布。

根据上面这条定理，我们可以很轻松的计算二项分布的数字特征了。终于知道学习那些数字特征的计算法则的用途了，下面将会非常简单。
### Expectation
随机变量 $X$ 是一个参数为 $n$ 和 $p$ 的二项分布，那么其期望是：
$$
E(X)=\sum^{n}_{i=0}E(X_i)=np
$$
用到的法则：
1. 独立的随机变量的和的期望，等于期望的和

### Variance
随机变量 $X$ 是一个参数为 $n$ 和 $p$ 的二项分布，那么其方差是：
$$
Var(X)=\sum^{n}_{i=1}=np(1-p)
$$
用到的法则：
1. 独立的随机变量的和的方差，等于方差的和

如果使用别的方法求方差会非常麻烦，比如定义或者 $Var(X)=E[X^2]-E^2[X]$ 别问我怎么知道的。
### m.g.f.
随机变量 $X$ 是一个参数为 $n$ 和 $p$ 的二项分布，那么其距生成函数是：
$$
\psi(t)=E(e^{tX})=\Pi^{n}_{i=1}E(e^{tX_i})=(pe^t+1-p)^n
$$
用到的法则：
1. 独立的随机变量的和的m.g.f.，等于m.g.f.的累积

### 二项分布随机变量相加
>Theorem If $X_1,\dots,X_n$ are independent random varibales,and if $X_i$ has the binomial distribution with parameters $n_i$ and $p$ ( $i=1,\dots,k$ ) ,then the sum $X_1+\dots+X_k$ has the binomial distribution with parameters $n=n_1+\dots+n_k$ and $p$ .

当多个二项分布有不同的 $n$ 但是有相同的 $p$ 那么他们可以相加，$n$ 是所有 $n$ 的和， $p$ 不变，这个可以根据将二项分布打散成伯努利分布然后再加起来可以看出来定理是正确的

那么什么时候可以使用上述定理呢？
1. 所有随机变量相互独立
2. 参数 $p$ 必须相同

这两点有任何一点不成立，上面的定理都不成立。
书上接着给了个大长例子，讲的是血液检验，还有到了二分查找法，可以看看
## 总结
本文介绍伯努利分布和二项分布，分析了其对应数字特征，和m.g.f下一篇我们继续研究分布——超几何分布。
待续。。。
