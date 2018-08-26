---
title: 【概率论】5-5:负二项分布(The Negative Binomial Distribution)
categories:
    - Mathematic
    - Probability
keywords:
    - The Negative Binomial Distribution
    - The Geometric Distribution
toc: true
date: 2018-03-29 08:57:12
---

**Abstract:** 本文介绍负二项分布，几何分布的基础知识
**Keywords:** The Negative Binomial Distribution，The Geometric Distribution

<!--more-->
## 开篇废话
到目前为止，所有的分部都是从Bernoulli 分布衍生出来的：
1. 二项分布，$n$ 次Bernoulli试验的结果中，每次试验的分布不变，结果为1的次数 $X$ 的分布
2. 超几何分布，$n$ 次Bernoulli试验，每次试验分布发生改变，结果为1的次数 $X$ 的分布，当试验分布变化不大的时候和二项分布结果相同
3. 泊松分布，用来在某种特殊情况下（$n$ 比较大， $p$ 比较小，而 $np$ 又不是很大的情况下）近似二项分布，当n趋近于无穷的时候等同于二项分布。

今天我们还是从二项分布出发，研究这样一个事实，对于Bernoulli过程，我们设定，当某个结果出现固定次数的时候，整个过程的数量，比如我们生产某个零件，假设每个零件的合格与否都是相互独立的，且分布相同，那么当我们生产出了五个不合格零件时，一共生产了多少合格的零件，这个数量就是一个负二项分布。
为什么叫负二项分布而不是正二项分布？
有两种说法，第一我们上面说到的例子，多半是失败到了固定次数时 $X$ 的分布，另一种是站在分布的系数上来观察的，在下面我们可以看得到。
## Definition and Interpretation
废话中给出的生产零件的例子就是引出定义的关键。我们来先看一个定理，描述上面过程的定理：
>Theorem Sampling until a Fixed Number of Success.Suppose that an infinite sequence of Bernoulli trails with probability of success $p$ are available.The number $X$ of failures that occur before the $r$th success has the following p.d.f.
$$
f(x|r,p)=
\begin{cases}
\begin{pmatrix}
r+x-1\\
x
\end{pmatrix}p^r(1-p)^x&\text{for }x=0,1,2,\dots\\
0&\text{otherwise}
\end{cases}
$$

证明如下
首先我们必须分析一下这个过程，当成功的次数达到目标后停止试验，也就是说最后一次必然是成功的，不然试验不会结束，所以我们需要的是在已经进行了的 $x+r-1$ 次实验中完成 $r-1$ 次成功，$x$ 次失败，那么从计数原理角度，概率为：
$$
\begin{aligned}
Pr(A_n)&=\begin{pmatrix}n-1\\r-1\end{pmatrix}p^{r-1}(1-p)^{(n-1)-(r-1)}p\\
&=\begin{pmatrix}n-1\\r-1\end{pmatrix}p^{r}(1-p)^{(n-r)}p
\end{aligned}
$$
然后这就是我们的目标了，n就是总的试验次数包括成功和失败。

>Definition Negative Binomial Distribution.A random variable $X$ has the negative binomial distribution with parameters $r$ and $p$ ( $r=1,2,\dots$ and $0 < p < 1$) if $X$ has a discrete distribution for which the p.f. $f(x|r,p)$ is as specified by theorem upside.

定义，告诉你，上面定理里面的p.f.叫负二项分布
因为存在关系：
$$
\begin{pmatrix}
r+x-1\\
x
\end{pmatrix}={\frac {(x+r-1)\dotsm (r)}{x!}}=(-1)^{x}{\frac {(-r)(-r-1)(-r-2)\dotsm (-r-k+1)}{x!}}=(-1)^{x}{\binom {-r}{x}}
$$
所以负二项分布可以写成：
$$
f(x|r,p)=
\begin{cases}
(-1)^{x}{\binom {-r}{x}}p^r(1-p)^x&\text{for }x=0,1,2,\dots\\
0&\text{otherwise}
\end{cases}
$$
这就是命名的由来（参考《统计推断第二版》）

## The Geometric Distribution
我们先学了超几何分布然后又跑回来学几何分布，难道我们跑错了？当然不是，因为几何分布跟负二项分布非常相关。
当负二项分布 $r=1$ 的时候产生的分布叫做几何分布，文字解释就是当我们第一个不合格的产品出现时，我们就停止生产，这是生产的合格产品的数量为随机变量 $X$

>Definition Geometric Distribution.A random varibale X has the geometric distribution with parameter p ($0 < p < 1$ )if X has a discrete distribution for which the p.f. f(x|1,p) is as follows:
$$
f(x|1,p)=
\begin{cases}
p(1-p)^x&\text{for }x=0,1,2,\dots\\
0&\text{otherwise}
\end{cases}
$$

这个跟负二项分布一模一样，所以就不再重复证明了
>Theorem if $X_1,\dots,X_r$ are i.i.d. random variable and if each $X_i$ has the geometric distribution with parameter $p$ ,then the sum $X_1+\dots+X_r$ has the negative binomial distribution with parameters $r$ and $p

又是加法规则，多个几何分布相加的结果是负二项分布。
证明过程也就是分析过程，首先我们假设我们有一个Bernoulli过程，那么如果只要有0发生就停止，这样产生的是几何分布，如果第一个几何分布 $X_i$ 产生以后，我们继续按照规则进行，那么接着会产生 $X_2,\dots,X_n$ 并且之间相互独立，而产生的到第 $n$ 个的时候，就产生了一个当0发生 $n$ 次的负二项分布。所以这 $n$ 个随机变量相加就是负二项分布。

这个证明不太严谨。但是从逻辑的角度上是成立的。
## Properties of Negative Binomial and Geometric Distributions
接下来我们看看性质
### m.g.f.
>Theorem Moment Generating Function.If X has the negative binomial distribution with parameters r and p ,then the m.g.f. of X is as follow:
$$
\psi(t)=(\frac{p}{1-(1-p)e^t})^r\text{ for } t< log(\frac{1}{1-p})
$$
$r=1$ 的时候上述表达式为几何分布的p.d.f. ，有点复杂了，我们来证明下。

证明：
要用到m.g.f.的定理就是多个独立的随机变量和的m.g.f.是其m.g.f.的积：
>Teorem 4.4.4 Suppose that $X_1,\dots,X_n$ are $n$ independent random varibales;and for $i=1,\dots,n$ . let $\psi_i$ denote the m.g.f. of $X_i$ .Let $Y=X_1+\dots+X_n$ ,and let the m.g.f. of $Y$ be denoted by $\psi$ .Then for every value of t such that $\psi_i(t)$ is finite for $i=1,\dots,n$ ,
$$
\psi(t)=\Pi^{n}_{i=1}\psi_i(t)
$$

以及上面的定理：多个同参数 $p$ 的独立几何分布的和是负二项分布，假设这些i.i.d的随机变量为 $X_1,\dots,X_n$
于是：
$$
\psi_i(t)=E(e^{tX_i})=p\sum^{\infty}_{x=0}[(1-p)e^t]^x
$$
为了使上面的表达式结果有限，或者说让 $[(1-p)e^t]^x$ 收敛，我们应该有 $0< (1-p)e^t < 1$  得到 $t<log(\frac{1}{1-p})$
根据微积分中级数的原理，当 $\alpha\in (0,1)$ 时：
$$
\sum^{\infty}_{x=0}\alpha^x=\frac{1}{1-\alpha}
$$

带入到 $\psi_i(t)$ 中就有
$$
\psi_i(t)=\frac{p}{1-(1-p)e^t}
$$

那么根据上面的定理4.4.4 我们就能得到结果
$$
\psi(t)=(\frac{p}{1-(1-p)e^t})^r\text{ for } t< log(\frac{1}{1-p})
$$

### Mean and Variance
>Theorem if $X$ has the negative binomial distribution with parameters $r$ and $p$ the mean and the varance of $X$ must be
$$
E(X)=\frac{r(1-p)}{p} \text{ and }Var(X)\frac{r(1-p)}{p^2}
$$
The mean and variance of the geometric distribution with parameter $p$ are the special case of equation upsite with $r=1$

如果有了m.g.f.求均值和方差都不是大问题，就是求两个导数，所以就直接写结果了，但是我们有更简单的方法，继续把负二项分布拆成几何分布，然后计算均值和方差后按照均值和方差的加法原则求负二项分布的均值和方差：
$$
E(X_i)=\psi'_i(0)=\frac{1-p}{p}\\
Var(X_i)=\psi''_i(0)-[\psi'_i(0)]^2=\frac{1-p}{p^2}
$$
然后就是把 $r$ 个 $X_i$ 求和就得到订立中的结果了。

### Memorless Property of Geometric Distributions
这条性质是第一次出现，所以值得注意
>Theorem Memoryless Property of Geometric Distributions.Let $X$ have the geometric distribution with parameter $p$ ,and let $k\geq 0$ .Then for every integer $t\geq 0$ ,
$$
Pr(X=k+t|X\geq k)=Pr(X=t)
$$

这个证明是个习题，我还没有做（嘻嘻嘻，后面补上），但是要指出的是，我们学的这些分布中只有几何分布有这种性质。

<font color='ff000'>此处有坑，记得补上</font>
## 总结
本文介绍了离散分布中最后两个，负二项分布，和几何分布，下一篇开始连续分布。
