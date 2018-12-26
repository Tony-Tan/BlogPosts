---
title: 【概率论】5-8:Beta分布(The Beta Distributions)
categories:
    - Mathematic
    - Probability
tags:
    - Beta分布
toc: true
date: 2018-04-02 15:14:12
---

**Abstract:** 本文介绍Beta分布的相关知识内容
**Keywords:** The Beta Distribution

<!--more-->
# Beta分布
我们预测未来某件事情是否发生的主要依据是先验知识，于是我相信，自古流传至今的那些道理应该是值得信任的，人无信不立，立壁千仞无欲则刚，生于忧患死于安乐，这些所谓的被我曾经鄙视的大道理，现在看看，真的是值得我自己坚持的，我大中华文化几千年，流传出来的一定是被很多人验证过的先验知识，而现在这些有钱的爸爸总结出来的可能只适用于这个时代，想要追求真理，安全起见还是要多读古人的智慧。
本文继续在连续随机变量上进行探索，Gamma分布的随机变量是满足某些条件下的所有正实数，而我们今天要研究的beta族分布是分布在 $[0,1]$ 区间上的一种类型的连续分布。一个最常见的例子，是Bernoulli过程中对每次试验的成功概率的建模。
Bernoulli过程就是多次的独立的试验形成的一个结果序列，这个系列中每个随机变量的成功概率就可以用Beta分布来建模。
## 贝塔函数 The Beta Function
和Gamma分布一样，Beta分布也是先有的Beta函数，先来看个例子，这个例子可以引出我们的Beta函数。
🌰 ：
一个机器制造零件，只有两种情况就是合格和不合格，不会出现第三种情况，我们让 $P$ 表示不合格的零件占总零件的比例，假设我们得到了n个零件，其中X个不合格，我们假设在给定条件P下每个零件的合格与否条件独立，那么我们就能得出在[3.6中的例子](https://face2ai.com/Math-Probability-3-6-Conditional-Distributions-P2/)，对应这个例子，当给定 $X=x$ 的条件下 $P$ 的分布：
$$
g_2(p|x)=\frac{p^x(1-p)^{n-x}}{\int^{1}_{0}q^x(1-q)^{n-x}dx} \text{ for }0<p<1
$$

这个p.d.f.就是我们今天要研究的主角，在研究完整分布之前，我们先来研究这个分母

>Definition The Beta Function .For each positive $\alpha$ and $\beta$ ,define:
$$
B(\alpha,\beta)=\int^{1}_{0}x^{\alpha-1}(1-x)^{\beta-1}dx
$$
the function B is called the beta function

所以上述就是beta函数的定义，也是上面例子中的分母的形式，可以看出beta函数中的 $\alpha,\beta > 0$
本文后面用到了3.9的一部分知识未在博客中体现，预计作为补充内容在下一部分给出，所以这个地方有些可以跳过。或者通过书本学习相关内容。

>Theorem For all $\alpha,\beta >0$ ,
$$
B(\alpha,\beta)=\frac{\Gamma(\alpha)\Gamma(\beta)}{\Gamma(\alpha+\beta)}
$$

这个命题的证明就用到了上面说的3.9的一部分选学内容，我们后面会给出相关证明，但是目前我们就当做此定理已经证明。

## 贝塔分布的定义 Definition of the Beta Distributions
那么我们接下来就要定义Beta分布了。
>Definition Beta Distributions.Let $\alpha ,\beta >0$ and let X be a random variable with p.d.f.
$$
f(x|\alpha,\beta)=
\begin{cases}
\frac{\Gamma(\alpha)\Gamma(\beta)}{\Gamma(\alpha+\beta)}x^{\alpha-1}(1-x)^{\beta-1}&\text{ for }0<x<1\\
0&\text{otherwise}
\end{cases}\tag{5.8.3}
$$

观察可以发现，如果 $\alpha=1,\beta=1$ 那么5.8.3就是 $[0,1]$ 的均匀分布。

---------------
举个🌰 ：
这个例子在西方社会可能比较常见，在我们这不流行这么落后的方法，资本主义国家迷路都是看指南针，看地图，我们是直接扔鞋，高效有特色！一天天选个举还要用模型预测，我口算都能算出来我们的选举结果。
从一个有79.1%墨西哥裔美国人的地区中选择220个陪审员，但是只有一百个陪审员是墨西哥裔，根据二项随机变量X的期望值是 $E(X)=220\times 0.791=174.02$ 。显然这比100多了不少。当然出现174个墨西哥裔的陪审员并不是必须的，也是概率的，因为 X可以为 [0,220] 中的任意数字。我们令 P 为墨西哥裔陪审员的比例。法庭假设X 在条件 $P=p$ 上一个二项分布，参数 n=220 和 p ，我们比较感兴趣是否P小于0.791，我们现在假设存在种族歧视（墨西哥裔陪审员比例小于0.791）比如我们认为选择系统存在一个0.8的偏移，也就是 $P<0.8\times0.791=0.6328$ 那么我们要计算的就是当给定 $X=100$ 时 $P\leq 0.6328$ 的条件概率

解：
假设P的分布在得到X前已经被确定（比如选举系统被人做了手脚），那么我们把它假设成一个beta分布，参数为 $\alpha,\beta$ ,那么 $P$ 的p.d.f.是：
$$
f_2(p)=\frac{\Gamma(\alpha)\Gamma(\beta)}{\Gamma(\alpha+\beta)}x^{\alpha-1}(1-x)^{\beta-1} \text{ ,  for }0<x<1
$$
X在给定P=p条件下的概率函数：
$$
g_1(x|p)=\begin{pmatrix}200\\x\end{pmatrix}p^x(1-p)^{220-x}\text{, for }x=0,\dots,220
$$

然后我们用伟大的贝叶斯公式来X=100 条件下的P的概率：
$$
\begin{aligned}
g_2(p|100)&=\frac{\begin{pmatrix}220\\100\end{pmatrix}p^{100}(1-p)^{120} \cdot \frac{\Gamma(\alpha)\Gamma(\beta)}{\Gamma(\alpha+\beta)}x^{\alpha-1}(1-x)^{\beta-1}}{f_1(100)}\\
&=\frac{\begin{pmatrix}220\\100\end{pmatrix}\Gamma(\alpha+\beta)}{\Gamma(\alpha)\Gamma(\beta)f_1(100)}p^{\alpha+100-1}(1-p)^{\beta+120-1}
\end{aligned}
$$

上面结果可以看出来左半部分就是个数，右半部分才含有变量，并且这个形状，很明显，还是一个beta分布，然后我们选择参数值就可以知道这个 $Pr(P\leq 0.6328|X=100)$ 的分布了，而这个参数选择要在我们徐汇了beta分布的期望求法以后才能知道怎么选择参数。

---------------

>Theorem Suppose that $P$ has the beta distribution with parameters $\alpha$ and $\beta$ ,and the conditional distribution of $X$ given $P=p$ is the binomial distribution with parameters $n$ and $p$ .Then the conditional distribution of $P$ given $X=x$ is the beta distribution with parameters
$\alpha+x$ and $\beta+n-x$

这个定理上面我们的例子中已经用事实证明了可行，但是并没有严谨的证明，所以这个定理是未严格证明的定理。
## 贝塔分布的距 Moments of Beta Distributions
>Theorem Moments.Suppose that X has the beta distribution with parameters $\alpha$ and $\beta$ .Then for each positive integer k,
$$
E(X^k)=\frac{\alpha(\alpha+1)\dots(\alpha+k-1)}{(\alpha+\beta)(\alpha+\beta+1)\dots(\alpha+\beta+k-1)}
$$
In particular,
$$
E(X)=\frac{\alpha}{\alpha+\beta},\\
Var(X)=\frac{\alpha\beta}{(\alpha+\beta)^2(\alpha+\beta+1)}
$$

证明：
For $k=1,2,\dots$
$$
\begin{aligned}
E(X^k)&=\int^{1}_{0}x^kf(x|\alpha,\beta)dx\\
&=\frac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)\Gamma(\beta)}\int^{1}_{0}x^{\alpha+k-1}(1-x)^{\beta-1}dx
\end{aligned}
$$
根据公式 5.8.2
$$
E(X^k)=\frac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)\Gamma(\beta)}\cdot\frac{\Gamma(\alpha+k)\Gamma(\beta)}{\Gamma(\alpha+k+\beta)}
$$
简化之后就是定理中形状了，证毕。

beta分布有很多不同参数组合形式，计算器均值和c.d.f.是非常有用技能。


在选择参数之前，我们必须明确Beta分布一般来建模概率的分布，0到1之间的分布，如果其中某个概率出现的比较大，那么分布在图像上会给出一个峰值，并且Beta分布的图像大致如下:
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-5-8-The-Beta-Distribution/figure_1.png)
均值就是峰值的位置。
接着我们把参数改一下，看看有什么变化
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-5-8-The-Beta-Distribution/figure_2.png)

可见，在均值不变的情况，增大 $\alpha$ 和 $\beta$ 的值，分布变高变瘦了。

----------
还要继续上面的例子，简单的概括一下上面的例子，首先，我们感兴趣的是一个概率的概率，而研究概率的办法是研究分布，也就是概率的分布，我们用beta 分布来建模这个概率，然后我们做试验来验证我们之前猜测概率也好，希望的概率也好，验证他们是否合理，根据上面选陪审员的例子，我们的目的就是为了验证有没有种族歧视，因为墨西哥裔占总人口数为 $79.1%$  ，而只选择出了100人，理论上应该选择出174.02 人，我们想知道当我们选择出100人的条件下，是否还是公平的，用概率为$79.1%$ 的参数去抽取了，还是用 $79.1%\times 0.8$ 或者更夸张的参数选取的。根据上面例子中我们已经求出了条件概率，接下来我们研究选择什么样的 $\alpha$ 和 $\beta$ 来准确的计算这个概率。
首先我们先来看原始分布（不是 $g_2(p|100)$ 条件分布）原始分布我们希望的是对所有人平等的，所以根据期望来计算，这个分布的期望必然是 0.791 ，这样才是公平的，根据beta分布的数字特征，我们能计算出：
$$
E(X)=\frac{\alpha}{\alpha+\beta}=0.791\Rightarrow \alpha=3.785\beta
$$
这个关系是我们最基本的性质，所以在条件情况 $g_2(p|100)$ 下的参数也应该满足这个关系，$g_2$ 参数为 $\alpha+100$ 和 $\beta+120$ 那么我们就能得出一个系列的不同参数的 $g_2$ 但是这不好研究，因为 $p$ 是自变量，还有 $\beta$ (或者 $\alpha$ ) 两个变量，所以我们来看当 $p< 0.791\times 0.8 = 0.6328$ 的时候各 $\beta$ 对这个条件分布的相互关系：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-5-8-The-Beta-Distribution/0_6328.png)
因为当 $p<0.6328$ 就相当于非常歧视了，所以我们必须让这个概率低，怎么也要低于0.5 那么对应的 $\beta$ 就要选至少 51.5 ，此时 $\alpha$ 为 194.9
这个时候如果我们把 $\beta=51.5,\alpha=194.9$ 作为参数带回到原始我们假设的 $p$ 的分布，得到 $P(X=100)=3.28\times 10^{-8}$ 这也就意味着，我们原始的关于均值是0.791的beta分布，发生220个陪审员中有100个墨西哥裔的概率是 $3.28\times 10^{-8}$ 基本为0，所以这里面肯定有不公平！
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-5-8-The-Beta-Distribution/figure_3.png)

----------

## 总结

这篇文章写了三天，原因是昨天胃肠炎发烧了，所以如果有点不连贯，请大家谅解，重点是例子，注意，重点是例子。





原文地址1：[https://www.face2ai.com/Math-Probability-5-8-The-Beta-Distribution](https://www.face2ai.com/Math-Probability-5-8-The-Beta-Distribution)转载请标明出处
