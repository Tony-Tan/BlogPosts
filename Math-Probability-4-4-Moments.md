---
title: 【概率论】4-4:距(Moments)
categories:
    - Mathematic
    - Probability
tags:
    - 距
    - 距生成函数
toc: true
date: 2018-03-25 10:58:47
---

**Abstract:** 本文介绍另一个基于期望的随机变量分布的数字特征——“矩” （不知道咋翻译的，英文moment表示时刻,瞬间等意思）
**Keywords:** Moments，Moments Generating Function

<!--more-->
# 距
能继续写关于基础数学知识的博客，靠的全是毅力吧，昨天一天博客的访问量是1，可能大家真的都对这个没什么兴趣。
有没有人看都要继续，因为我认为这个有用，而且值得写。
距这个名词第一次听说我记得非常清楚，2014年10月左右，在尚都蜗居的时候（挨饿的时候），准备去大疆面试，晚上看书在一本机器学习的书上看到概念，然后通过博客查了一下，得到了很多总结，还记得那个博主解释了3阶距4阶距什么的，但是不得其精妙，今天本篇的目的是掌握数学特性，后面再研究机器学习应用的时候就能领悟其中的奥秘了。看博客知乎学的知识，多半是碎片化的，系统的学习还是需要大量时间的
上一篇我们研究了第一个期望的衍生数字特征——方差，今天我们来第二个，Moments，中文翻译叫距，其本质是随机变量 $X$ 的 $k$ （正整数）次幂的期望，也就是 $E[X^k]$ 那么这个距可以看做期望的更一般形式，也就是说距其实包含了期望，期望是一种特殊的距，即 $k=1$ 当然当$k\geq 2$ 时，这个距开始有更大的用途了。
另外就是m.g.f.了，和p.d.f.以及c.d.f.一样重要的一个描述随机变量的函数，中文翻译叫“生成距函数”？不知道中文叫啥，但是他的作用很大，可以帮助得到多个独立随机变量的和的分布，也可以用来确定分布的某些限制属性。
## 距的存在 Existence of Moments
距就是一个随机变量指数函数的分布的期望，期望第一课就能求距，但是距的性质使他成为一个重要的数字特征。
对于任意随机变量 $X$ 以及正整数 $k$  ，期望$E[X^k]$ 被称为随机变量 $X$ 的 $k$ 阶距。那么如果按照距的说法，我们4.1中的期望就是1阶距。
就像不是所有随机变量都有期望一样，因为距也是期望，所以有些随机变量没有距，几阶都没有。

距存在的条件 $(E[X^k]<\infty)$  那么：
- 如果随机变量有界，那么距存在（也就是如果有有限的数字 $a,b$ 使得 $Pr(a\leq X\leq b)=1$ 成立）
- 即使随机变量没有界，那么也有可能存在所有的距。
- 下面定理表示如果存在 $k$ 阶距，那么存在所有小于 $k$ 阶的距。

> Theorem If $E(|X|^k)<\infty$ for some positive integer $k$ ,then $E(|X|^j)<\infty$ for every positive integer $j$ such that $j < k$

下面我们要做的就是证明这个定理了
证明：
证明之前，我们假定研究的对象是连续随机变量，并且其 p.d.f. 为 $f$ 那么：
$$
\begin {aligned}
E(|X|^j)&=\int^{\infty}_{-\infty}|x|^jf(x)dx\\
&=\int_{|x|\leq 1}|x|^jf(x)dx+\int_{|x|>1}|x|^jf(x)dx\\
&\leq \int_{|x|\leq 1}1\cdot f(x)dx+\int_{|x|>1}|x|^kf(x)dx\\
&\leq Pr(|X|\leq 1)+E(|X|^k)
\end{aligned}
$$

证明思路是利用了指数运算的计算性质，第一个小于等于号是首先小于一部分的随机变量被1替代，其次是后面大于1的随机变量指数变大，所以会产生一个小于等于，其实等号不会成立，因为定义中 $j\leq k$ 所以后面那一项一定是大于前一项的；第二个小于等于号就是把上面两个积分对应下来，一个是Pr的定义，一个是期望的定义的扩展，证毕。
### 中心距 Central Moments
进一步我们提出中心距的概念。

>Suppose that $X$ is a random variable for which $E(X)=\mu$ .For every positive integer $k$ ,the expectation $E [(X-\mu)^k]$ is called the $k$th central moment of $X$ or the $k$th moment of $X$  about the mean.

距的更广泛的一中定义，上面我们的原始定义可以看做中心距的均值为0的版本。
因为减去了期望，所以一阶中心距肯定是0了。如果不是说明你算错了。

更仔细观察，发现，方差是二街中心距。
方差也好，距也好都是期望的产品，所以其间必然有着各种关系。
中心距的另一个性质就是如果 $k$ 是奇数，同时 $X$ 的分布是一个对称的函数，那么其期望必然是0.
举个🌰 但是不给出答案了：
一个对称的 p.d.f 假设X有一个连续分布，其p.d.f.如下：
$$
f(x)=ce^{-(x-3)^2/2} \text{ for } -\infty<x<\infty
$$
我们需要求出他的期望，以及中心距。

这个例子可以好好研究一下，用了不少小trick，以及分部积分等微积分的知识。这里就不写详细的过程了，但是希望大家去看看书。
### 偏度 Skewness
偏度，上面我们说过，有些对称随机变量的奇数中心距是0，然后我们就想，有没有什么数字特征可以描述一个随机变量的对称程度。
>Definition Let $X$ be a random variable with mean $\mu$ ,standard deviation $\sigma$ ,and finite third moment. The skewness of $X$ is defined to be $E[(X-\mu)^3]/\sigma^3$

三阶中心距与标准差的三次方的比值，定义了偏度，除以一个标准差的三次方其实是为了让偏度与分布的离散程度无关，而只描述对称程度。对称的分布偏度是0，越偏越大，因为偏这个描述本身就没有定义，而且分散程度这个也没有数量定义，所以我们就用方差直接定义了分散程度，而这个偏度说是去除了分散程度对他的影响，就是除去了方差，这些其实想推理一下是有困难的，因为这个是定义，定义就是一个描述，然后起个名字，你完全可以用你的定义方法去定义偏度，但是只要别人的定义也不违反定理，那你就没有任何理由说别人的定义错。

然后关于偏度书上也有个例子，这里我就不写了，都是计算的例子，大家自己做两道题熟悉下就好了


## 距生成函数 Moment Generating Functions
这个可以说是本篇的大Boss，因为这是个新的工具，来研究分布，就是m.g.f，中文翻译过来是距生成函数？这个工具与p.d.f 和c.d.f一样重要，是一个描述随机变量重要的方法，但是这个方法跟随机变量的距关系比较大，而不像 p.d.f 或者 c.d.f 那样跟随机变量的概率关系较大。
>Definition Moment Generating Function. Let X be a random variable.For each real number t,define
$$
\psi(t)=E(e^{tX})
$$
The function $\psi(t)$ is called the moment generating function(abbreviated m.g.f.)of $X$

注意，m.g.f 只与随机变量的分布有关系，同一个分布只有一个m.g.f. 如果两个随机变量的m.g.f. 那么这两个随机变量分布也一定是一样的。
接着还是存在性问题，就是每个随机变量的m.g.f都存在么？显然也不是因为这里也用到了期望，因为期望不总存在所以m.g.f不总存在。
- 随机变量有界，所以 $e^{tX}$ 对所有 $t$ 都是有限的。
- 随机变量没有界，那么有些 $t$ 是有限的，有些 $t$ 则不是
- $t=0$ 这种特殊情况，m.g.f. 为1

那么我们有个问题了，我们上面说的那么热闹，我们还不知道这个函数到底跟距有啥关系，而且叫距生成函数，是距生出来的函数，还是这个函数能生距？

> Theorem Let $X$ be a random variables whose m.g.f $\psi(t)$ is finite for all values of $t$ in some open interval around the point $t=0$ .Then,for each integer $n>0$ ,the $n$th moment of $X$ ,$E(X)$,is finite and equals the $n$th derivative $\psi^{(n)}(t)$ at $t=0$ ,That is $E(X^n)=\psi^{(n)}(0)$ for $n=1,2\dots$


定理看明白没？没看明白我用大白话讲一下，就是m.g.f的n阶导数在 $t=0$ 处的值，刚好是随机变量的n阶距。很神奇吧，原因还是 $e^x$ 的导数的性质造成的这种现象，我们可以举个例子算算看：

随机变量 $X$ 的p.d.f. 是
$$
f(x)=\begin{cases}
e^{-x} & \text{for } x>0\\
0&\text{otherwise}
\end{cases}
$$
那么我们可以计算出 $X$ 的 m.g.f 和他的方差，先求m.g.f.
$$
\psi(x)=E(e^{tX})=\int^{\infty}_{0}e^{tx}e^{-x}dx\\
=\int^{\infty}_{0}e^{(t-1)x}dx
$$

这个积分只有当 $t-1<0$ 的情况下才可以求积分，这个是微积分的基础知识，不知道的去看微积分。那么只有当 $t<1$ 的时候，积分结果：
$$
\psi(t)=\frac{1}{1-t}
$$
那么接着就是对 $\psi(t)$ 求导了：
$$
\psi'(t)=\frac{1}{(1-t)^2}\\
\psi''(t)=\frac{2}{(1-t)^3}
$$

因为 $E(X)=\psi'(0)=1$ 以及 $E(X^2)=\psi''(0)=2$ 那么其方差：
$$
Var(X)=\psi''(0)-[\psi'(0)]^2=1
$$

如果不是使用m.g.f. 那么这个期望和方差求起来有些麻烦，首先就是求 $\int^{\infty}_{-\infty}xe^{-x}dx$ 这应该使用分部积分，然后再求方差，比较麻烦，m.g.f.则相对简单些。

## 距生成函数的性质 Properties of Moment Generating Functions
知道m.g.f是怎么生成距了以后我们就要系统的研究下，m.g.f的性质了。

>Theorem Let $X$ be a random varibale for which the m.g.f. is $\psi_1$ ;Let $Y=aX+b$ ,where $a$ and $b$ are given constants; and let $\psi_2$ denote the m.g.f. of $Y$ .Then for every value of $t$ such that  $\psi_1(at)$ is finite,
$$
\psi_2(t)=e^{bt}\psi_1(at)
$$

随机变量的m.g.f的线性变换，有点复杂，但是就是带到公式里去得到的。
证明：
$$
\psi_2(t)=E(e^{tY})=E[e^{(aX+b)t}]=e^{bt}E(e^{atX})=e^{bt}\psi_1(at)
$$
证毕


接着就是一个重要的性质了，一定数量不相关的随机变量的和的m.g.f.有一个非常简单的形式，这使得我们研究多个不相关的随机变量的和的时候有了一个好工具！

>Theorem Suppose that $X_1,\dots,X_n$ are $n$ independent random varibales;and for $i=1,\dots,n$ . let $\psi_i$ denote the m.g.f. of $X_i$ .Let $Y=X_1+\dots+X_n$ ,and let the m.g.f. of $Y$ be denoted by $\psi$ .Then for every value of t such that $\psi_i(t)$ is finite for $i=1,\dots,n$ ,
$$
\psi(t)=\Pi^{n}_{i=1}\psi_i(t)
$$

证明需要用到前面关于不相关的随机变量的积的期望等于期望的积的性质，不记得的童鞋查阅4.1和4.2，
$$
\psi(t)=E(e^{tY})=E(e^{tX_1+\dots+tX_n})=E(\Pi^{n}_{i=1}e^{tX_i})\\
E(\Pi^{n}_{i=1}e^{tX_i})=\Pi^{n}_{i=1}E(e^{tX_i})\\
\psi(t)=\Pi^{n}_{i=1}\psi_i(t)
$$

其中只有第二步有点技术含量，用到了独立随机变量的性质，接着我们看几个别的性质。
### 二项分布的距生成函数 The Moment Generating Function for the Binomial Distribution
二项分布 $(n,p)$ 的m.g.f 问题：
二项分布是多个独立的伯努利分布加起来的结果，根据上面的性质，独立随机变量求和，m.g.f性质，那么我们有：
$$
\begin{aligned}
\psi_i(t)&=E(e^{tX_i})\\
&=p\times e^{t}+(1-p)\times e^{0}\\
&=pe^t+1-p
\end{aligned}
$$

根据上面定理
$$
\psi(t)=(pe^t+1-p)^n
$$

证毕。

### 距生成函数的唯一性 Uniqueness of Moment Generating Functions
重要性质，但是受到我们知识的限制，我们没办法证明。
>Theorem If the m.g.f. of two random varibales $X_1$ and $X_2$ are finite and identical for all values of $t$ in an open interval around the point $t=0$ ,then the probability distributions of $X_1$ and $X_2$ must be identical.

在0附近这个条件要注意一下，其他的基本比较好动，目前我们还无法证明的话，那就留着呗，早晚有一天我们会证明他的。
而这条不会证明的定理也说明了m.g.f.可以用来完整的描述分布。

### 二项分布的可加性 The Additive Property of the Binomial Distribution
二项分布的可加性，m.g.f.对独立随机变量的加法贡献，使得我们又得到了两个独立的二项分布的随机变量加法结果：
>Theorem If $X_1$ and $X_2$ are independent random variables,and if $X_i$ has the binomial distribution with parameters $n_i$ and $p$ ($i=1,2,\dots$ ),then $X_1+X_2$ has the binomial distirbution with parameters $n_1+n_2$ and $p$

定理就是说两个独立的二项分布的随机变量的和的情况。
证明：
对于 $i=1$ 和 $i=2$
$$
\psi_i(t)=(pe^{t}+1-p)^{n_i}
$$
那么两个二项分布的随机变量的和的m.g.f. 就是：

$$
\psi(t)=(pe^{t}+1-p)^{n_1+n_2}
$$

根据前两条定理，m.g.f能唯一确定分布，又根据二项分布的m.g.f.，我们可以确定上面的这个m.g.f.就是 $n_1+n_2,p$ 的二项分布
证毕！
## 总结
总结就是这一篇知识点比较多，大家看的时候要多对应例子，不然定理没办法深入理解的。
明天继续。。。





原文地址1：[https://www.face2ai.com/Math-Probability-4-4-Moments](https://www.face2ai.com/Math-Probability-4-4-Moments)转载请标明出处
