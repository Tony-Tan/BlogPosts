---
title: 【概率论】3-3:累积分布函数(Cumulative Distribution Function)
categories:
  - Mathematic
  - Probability
keywords:
  - Cumulative Distribution Function
  - 概率累计函数
  - Quantial
  - 分位数
toc: true
date: 2018-02-06 10:09:15
---

**Abstract:** 本文介绍描述随机变量分布的另一种工具，累积分布函数，CDF
**Keywords:** Cumulative Distribution Function，Quantial

<!--more-->
## 开篇废话
有一位时事评论人士曾经说过里根总统是一位非常耿直的总统，这位伟大的总统有一个很简单的评判人的观点，他认为一个人以前的做法和以后的做法将会非常相似，比如你是曾经是一个罪犯，那么你以后很有可能还会犯罪。佛说放下屠刀立地成佛，文人说浪子回头金不换，但是能达到这种境界的“坏人”不多，更多情况的是民间谚语：“狗改不了吃屎”。

## Definition and Basic Properties
当我们从试验和事件通过随机变量数学化以后，所有数学性质都是围绕随机变量展开的，其中比较关键的就是随机变量到概率的映射，离散分布（离散随机变量）和连续分布（连续随机变量）我们前两篇已经讨论过了，而且描述这两种形式的随机变量的方法也不同，离散分布通过概率函数从随机变量得到概率，连续分布通过概率密度函数结合积分来得到概率，并且概率函数和概率密度函数都有一些自己的性质，可以帮助我们分析问题。我们这节的目的是找出一个可以同时用于离散分布和连续分布的工具，来指示随机变量和概率间的关系。
> Definition (Cumulative) Distribution Function :The distribution function or cumulative distribution function(abbreviated c.d.f) $F$ of a random variable $X$ is the function:
$$
F(x)=Pr(X\leq x)\quad \text{for} \quad -\infty<x<+\infty
$$

分布函数或者叫做累积分布函数，定义就是上式，还记得概率分布的定义么？没错$Pr(X\in C)=Pr({s:X(s)\in C})$ 这个描述中当把$X(s)\in C$ 中C这个关系定义成 $X\leq x$ 那么我们就有了分布函数，或者累积概率分布函数。
我们说过分布对于随机变量的研究非常重要，而一个函数被叫做分布函数，那么这个函数对随机变量甚至整个概率体系的数学化都是最基本的基础。
来个🌰 :
伯努利的c.d.f.,最简单的伯努利的分布是 $Pr(1)=p$ ,$Pr(0)=1-p$ ,那么其概率分布根据上面的定义是：
$$
F(x)=
\begin{cases}
0 &\text{for} &x<0\\
1-p &\text{for}& 0\leq x<1\\
1 &\text{for}&x\geq 1
\end{cases}
$$

这个过程没啥解释的当变量小于0的时候概率是0，大于等于0，小于等于1的时候只有$Pr(0)=1-p$ 在这个范围内，最后当$x\geq 1$ 的时候其对应的是 $Pr(0)+Pr(1)$ 。

接下来介绍c.d.f的三条重要性质，我们把事件千辛万苦的搞成数字，为的就是用各种工具来研究他们之间的关系，这些性质就是帮助我们的工具。
### P1 Nondecreasing
> The function $F(x)$ is nondecreasing as $x$ increases;that is,if $x_1<x_2$ then $F(x_1)\leq F(x_2)$

我们学过微积分应该知道非减函数的定义，当在定义域中$x_1\leq x_2$ 时 $f(x_1)\leq f(x_2)$

如果从集合的角度我们也可以证明，那么我们将回归到概率的公理和基本性质
证明：
$$
\text{for }x_1\leq x_2\\
\{x:x<x_1\}\subset \{x:x<x_2\}
$$
根据[1-1中的T4](http://face2ai.com/Math-Probability-1-1-Definition-of-Probability/)可以得到：
$$
Pr(\{x:x<x_1\})\leq Pr( \{x:x<x_2\})
$$
Q.E.D.

### P2 Limits
> Limits at $\pm\infty$ $lim_{x\to -\infty}F(x)=0$ and $lim_{x\to \infty}F(x)=1$

两种极限下c.d.f的表现，证明可以借用上面P1的过程，
$$
\text{for }x_1\leq x_2\\
\{x:x<x_1\}\subset \{x:x<x_2\}
$$

只要能求出
$Pr(\{x:x<x_1\})=1 \quad \text{for } x_1=+\infty$

以及

$Pr(\{x:x<x_2\})=0 \quad \text{for } x_2=-\infty$

只要能证明上述两个结果那么就能得到结论
c.d.f不需要有连续性，如图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-3-3-Cumulative-Distribution-Function/cdf.png)

根据函数的连续性定义，当左右极限和函数值相等时连续，
$$
F(x^{-})=lim_{y\to x,y<x}F(y)\\
F(x^{+})=lim_{y\to x,y>x}F(y)
$$

在cdf的间断点上，只可以有右极限。
### P3 Continuity from the Right
> Continuity from the Right.A c.d.f. is always continuous from the right: that is,$F(x)=F(x^{+})$ at every point $x$

<font color=#00ff00 face="微软雅黑">这个证明后续补上，需要数学分析中极限的证明（遗留问题）</font>
## Determining Probabilities from the Distribution Function
那么如何从分布函数中找到某个点（离散下）或者区间（连续下）的概率呢？
### T1： $Pr(X>x) = 1 - F(x)$
>Theorem: For every value $x$,
$$
Pr(X>x) = 1 - F(x)
$$
QED
证明显而易见，依据定义 $F(x)=Pr(X\leq x)$ 又因为 $\{x:X\leq x\}$ 和$\{x:X > x\}$ 是互补事件，所以依据[1-1](http://face2ai.com/Math-Probability-1-1-Definition-of-Probability/)必然有 $Pr(X>x) = 1 - F(x)$
### T2:$Pr(x_1<X\leq x_2)=F(x_2)-F(x_1)$
> Theorem :For all value $x_1$ and $x_2$ such that $x_1<x_2$ :
$$
Pr(x_1<X\leq x_2)=F(x_2)-F(x_1)
$$

这个证明也是相对简单，用到了上面T1的证明过程：
证明：
三个集合 $A=\{X:X\leq x_1 \}$、$B=\{X: x_1<X\leq x_2 \}$ 、 $C=\{X:X\leq x_2 \}$
$$
B=A^{c}\cap C
$$
根据[1-1中的T6](http://face2ai.com/Math-Probability-1-1-Definition-of-Probability/)：
$$
Pr(B)=Pr(C)-Pr(A\cap C)\\
\text{for: }A=A\cap C\\
Pr(B)=Pr(C)-Pr(A)\\
Pr(x_1<X\leq x_2)=F(x_2)-F(x_1)
$$
QED

证明过程用到了前面1-1的一条定理，证明相对简单。

### T3:$Pr(X<x)=F(x^{-})$
>Theorem For each value $x$
$$
Pr(X<x)=F(x^{-})
$$
这个证明和上面的P3的证明一样用到了数学分析中极限的证明，后面写数学分析的时候回来补上

<font color=#00ff00 face="微软雅黑">这个证明后续补上（遗留问题）</font>
### T4:$Pr(X=x)=F(x)-F(x^{-})$
>Theorem For every value $x$
$$
Pr(X=x)=F(x)-F(x^{-})
$$
证明:
根据概率定义明显有以下
$$
Pr(X=x)=Pr(X\leq x)-Pr(X<x)
$$
根据cdf的定义
$$
F(x)=Pr(X\leq x)
$$
那么根据这个定理，图中的 $Pr(x_1)=z_1-z_0$

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-3-3-Cumulative-Distribution-Function/cdf2.png)
## The c.d.f. of a Discrete Distribution
对于离散分布，cdf可以通过定义得到，而其函数形状应该是阶梯状的，而且离散分布的cdf有以下几点性质（设 $f(x)$ 是离散随机变量的概率函数）：
1. $F(x)$ will have a jump of magnitude $f(x_i)$ at each possible valuee $x_i$ of $X$
2. $F(x)$ will be constant between every pair of successive jump
3. The distribution of a discrete random variable $X$ can be represented equally well by either the p.f. or the c.d.f. of $X$

上面这几点性质主要是描述离散cdf在函数图像的表现。

## The c.d.f. of a Continuous Distribution
连续分布的c.d.f.
> Theorem Let X have a continuous distribution, and let f(x) and F(x) denote its p.d.f and the c.d.f. respectively.Then $F$ is continuous at every $x$:
$$
F(x)=\int^{x}_{-\infty}f(t)dt
$$
and
$$
\frac{dF(x)}{dx}=f(x)
$$
at  all x such that f is continuous.


进入连续模式下，我们更多开始关注p.d.f和c.d.f这类函数的函数性质，极限，导数，积分这些是了解函数的最基本的手段，根据cdf的定义和微积分基本定理，上述表达成立一点都不意外，很和谐流畅的数学表达。
证明过程主要使用cdf的定义，和pdf的定义以及微积分的意义，这里就不写了，太简单了。

## The Quantile Function
研究了cdf的函数性质，那么对于这种单调的函数，其必有反函数，那么cdf的反函数是啥？有啥用途么？有很大用处。
> Definition Quantiles/Percentiles: Let $X$ be a random variable with c.d.f. $F$ .For each $p$ strictly between $0$ and $1$ ,define $F^{-1}(p)$  to be the smallest value $x$ such that $F(x)\geq p$ .Then $F^{-1}(p)$ is called the $p$ quantile of $X$ or the $100p$ percentile of $X$ .The function $F^{-1}$ defined here on the open interval $(0,1)$ is called the quantile function of $X$

定义的简单解释，对于一般情况，我们假定cdf严格递增（可以通过水平检测）我们观察cdf的图像可以知道其横轴是随机变量的所在轴，纵轴是$F(x)$ 对应的值，$[0,1]$ 那么如果我们去其反函数，那么自变量变成了 $[0,1]$ ，而对应的值域就是随机变量，当我们选定任意  $1\geq p\geq 0$ 时通过反函数$F^{-1}（p）$ 得到的是随机变量的值，这个随机变量含义是，获得概率和为 $p$ 的随机变量上限。
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-3-3-Cumulative-Distribution-Function/quantile.png)
对于特殊情况，不能通过水平检验的，我们规定，其反函数的下:
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-3-3-Cumulative-Distribution-Function/quantile2.png)

所以我们规定当出现这种情况是，我们选择较小的随机变量值$x_1$
虽然我们前面说数学应该更多使用分析的方法，但是这里使用了大量的图片是为了让对函数图像没有概念的同学可以直观的感受下，如果从分析的方法直接分析值域和定义域的关系能得到相同的答案。
我们的quantiles 是cdf的衍生物，所以他和cdf一样，只依赖与分布，分布一旦确定cdf、quantiles 就唯一确定了。

### Median quartiles
>Definition Median/Quantiles.The $\frac{1}{2}$ quantile or the 50th percentile of a distribution is called its median.The $\frac{1}{4}$ quantile or 25th percentile is the lower quartile.The $\frac{3}{4}$ quantile or 75th percentile is called upper quartile.

三个比较特殊的位置，分别是25% 50% 75%，其中50%是最特殊的。

## Conclusion
本文主要介绍另一个非常重要的描述分布的工具，分布函数（累计分布函数）c.d.f，用好这些工具可以帮我们更顺利的完成后面的工作
