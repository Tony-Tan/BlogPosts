---
title: 【概率论】5-3:超几何分布(The Hypergeomtric Distribution)
categories:
    - Mathematic
    - Probability
keywords:
    - Hypergeomtirc Distribution
    - Finite Population Correction
toc: true
date: 2018-03-28 09:27:39
---

**Abstract:** 本文主要介绍超几何分布
**Keywords:** Hypergeomtirc Distribution,Finite Population Correction

<!--more-->
## 开篇废话
实力这个东西是不能被完全表现出来的，中华民族传统文化告诉我们，有十分的能力，只显示一分，但是我们现在是有一分能力要显示出十分，这叫推销自己，而且我们自己根本不知道自己只有一分能力，人心浮躁，我们还是憋着看书学习吧，外面的是是非非自然有人去解决，我们要做的是推动人类文明的发展😆
上文书我们说到（改成单口相声了）二项分布就是若干个独立同分布的伯努利分布的随机变量的和的结果，而伯努利分布如果对应最原始的抽样的话应该是这样的场景：
如果我们有一个不透明的箱子，里面有 $A$ 个红球， $B$ 个蓝球，其被拿出来的可能性相等，在我们拿出之前我们不知道我们会拿到什么（也就是保证随机性）那么我们拿出一个球是红球(称为事件R)的概率是 $Pr(R)=\frac{A}{A+B}$ ，如果我们连续进行本实验，那么就有两种取样方式，而这就导致了从伯努利到二项分布，和从伯努利到超几何分布的变化
## Definition and Examples
首先我们用一个具体的例子来看。

----------
继续上面说的拿球的例子，假设我们要连续拿出n个球，$n\geq 0$  （这里我们只考虑 $n\geq 2$ 的情况，$n=0$ 的时候说明试验不进行， $n=1$ 的时候是伯努利分布，上一课学习的东西，我们这里也不再说了） 我们假设每次取出时，拿到红球的随机变量为 $X_i=1$ 拿到蓝球的随机变量是 $X_i=0$ 并且每次试验是独立的，如果我们采用不放回的抽取方式，那么我们可以得出结论 $Pr(X_2=1|X_1=0)\neq Pr(X_2=1|X_1=1)$ ，因为我们第一次拿球，里面一共有 $A+B$ 个球包含 $A$ 个红球，如果第一次取出了红球，那么第二次我们再取相当于从 $A+B-1$ 个球包含 $A-1$ 红球中取，或者如果第一次取到的是蓝球，那么第二次相当于从 $A+B-1$ 个球中含 $A$ 个红球中取。
$$
Pr(X_2=1|X_1=0)=\frac{A}{A+B-1}>Pr(X_2=1|X_1=1)=\frac{A-1}{A+B-1}
$$


----------

这就是简单的不放回抽样，前面我们研究过，如果从抽取范围非常大的样本中抽取少量的时候，可以不考虑其概率变化，但是如果样本集本来就不是很大，那么就要考虑这个概率变化了。
从有限的样本集中不放回的抽样，这就是我们今天研究的对象超几何分布的背景。
如果考虑从包含两种情况的样本集合中抽取n个样本，其中是某一情况的样本个数X，其就是一个超几何分布。

>Theorem Probability Function .The distribuiton of $X$ is Example has the p.f.
$$
f(x|n,A,B)=
\begin{cases}
\frac{
\begin{pmatrix}A\\x\end{pmatrix}
\begin{pmatrix}B\\n-x\end{pmatrix}
}{
\begin{pmatrix}A+B\\n\end{pmatrix}
}&\text{for }max(0,n-B)\leq x\leq min(n,A)\\
0&\text{otherwise}
\end{cases}
$$

这个定理的证明只是用到了计数法则中的乘法法则，简化成分阶段的试验，但是 $x$ 范围要要注意一下，就是确保从A中来的和从B中来的结果总数是在范围内的，或者说，以取出红球的数量为x看来说，$x$ 不能超过 $A$，$n-x$ 是蓝球的数量不能超过 $B$ 写成数学形式就是 $max(0,n-B)\leq x\leq min(n,A)$

>Definition Hypergeomtric Distribution.Let $A,B$ and $n$ be nonnegative integers with $n\leq A+B$ .If a random variable $X$ has a discrete distribution with p.f. as in upside,then it is said that $X$ has the hypergeometric distribution with parameters $A,B$ and $n$

这个定义依旧简单明了，和前面二项分布的定义基本一个套路，给一个p.f.然后告诉你这个就是xx分布。

我们分析下超几何分布有什么特点，与二项分布有什么不同，首先他们的基础实验都是伯努利分布，但是不同的在于二项分布每一步的试验都是恒定分布的伯努利试验，但是超几何分布不是，其每一步的伯努利分布都有不同的分布，且当前分布和前面试验结果有关，如果按照这种思路，其实超几何分布的每一次取出都是一次和前面所有取出相关的伯努利分布（而二项分布是相互独立的）。

为什么叫超几何分布？
超几何分布的名字和二项分布的名字来源相似都是和某个展开式的系数有关系，超几何函数的级数展开系数和我们今天的超几何分布有关系，所以就命名为超几何分布。

几何分布和超几何分布的关系？
几何分布是二项分布中的一种极端情况，就是前 $k-1$ 次必须失败，第 $k$ 次成功，然后试验结束，换句话说就是在伯努利过程中当结果是 $X_k=1$ 时完成整个过程，这时的概率是 $p(1-p)^{k-1}$ 。那么如果 $X$ 是几何分布，那么其p.f.就是 $f(x|1,p)=p(1-p)^x$ for $x=0,1,2,\dots$
## The Mean and Variance for a Hypergeomtirc Distribution
基础了解的差不多了我们就开始研究下数字特征了。

>Theorem Mean and Variance.Let X have a hypergeometric distribution with strictly positive parameters A,B and n.Then:
$$
E(X)=\frac{nA}{A+B}\\
Var(X)=\frac{nAB}{(A+B)^2}\times \frac{A+B-n}{A+B-1}
$$

证明有点复杂了，要用到我们前面用到的一种思想，因为是有限的离散分布，我们总能把它转换成穷举的模式
首先证明期望：
思路，以拿球为例子说明，$A$ 个红球， $B$ 个蓝球 如果第i次试验拿到了红球 随机变量 $X_i=1$ 一共抽取 $n$ 个球。
我们假设所有球都不同，把他们所有可能的排列都列出来，然后把我们抽取过程等效成从所有可能的排列中选一个，观察前n个球中红球的数量。那么我们会有 $(A+B)!$ 行，每一个行对应一个随机结果，且等可能性，那么我们关心的是前 $n$ 个球是怎么排布样的（因为我们就抽取 $n$ 个球），这时候我们看列，第一列到第 $n$ 列，其中红球的平均数就是超几何分布的期望，每一列红球出现的比例都是 $\frac{A}{A+B}$ 原因是 红球和蓝球等可能出现，
有点复杂，我们来举个例子，三个球，一个红球，两个蓝球，我们按照上面的思路把所有可能的出场情况都写出来，共 $3!=6$ 种，我们只观察前两个球（假设从三个球中拿两个）的分布
$$
R_1,B_1,B_2\\
R_1,B_2,B_1\\
B_1,R_1,B_2\\
B_1,B_2,R_1\\
B_2,R_1,B_1\\
B_2,B_1,R_1
$$

我们现在观察第一列，也就是我们取出第一个球的时候，红球的期望是 $1\times\frac{2}{6}=1\times\frac{1}{1+2}$ ，同样第二列和第一列相同，那么前n列都是这样的，所以期望是 $\frac{nA}{A+B}$
证毕
这里用到了两个思想，
1. 把全部的取球过程列举出来，避免加权，我们把所有球都当做不同球。
2. 当把所有的结果列举出来，每行之间是互斥的。把前面每步取球相互影响的试验变成了互不影响的试验

这个证明使用了等价问题转化的思想。
同理我们可以证明每一列的方差是：
$$
Var(X_i)=\frac{AB}{(A+B)^2}
$$
因为每列之间并不独立，所以要用到前面方差求和的公式：
$$
Var(X)=\sum^{n}_{i=1}Var(X_i)+2{\sum\sum}_{i<j}Cov(X_i,X_j)
$$
因为考虑到所有 $Cov(X_i,X_j)=Coc(X_1,X_2)\text{for }i\neq j$
最后的结果就是：
$$
\begin{aligned}
Cov(X_1,X_2)&=-\frac{AB}{(A+B)^2(A+B-1)}\\
Var(X_i,X_j)&=\frac{nAB}{(A+B)^2}-\frac{n(n-1)AB}{(A+B)^2(A+B-1)}\\
&=\frac{nAB(A+B-1)-n(n-1)AB}{(A+B)^2(A+B-1)}\\
&=\frac{nAB}{(A+B)^2}\frac{A+B-n}{(A+B-1)}
\end{aligned}
$$
证毕

纯计算过程，但可以看出我们的证明也越来越复杂了。

## Comparison of Sampling Methods
上面我们就大致完成了超几何分布的知识，但是要说的是我们又涉及到了采样，我在上面说过，当我们不放回采样的对象，数量不多的时候，我们要用超几何分布来建模，当数量非常大的时候，不放回采样每次采集出现目标的概率变化非常小的时候，我们可以把超几何分布当做二项分布处理，而且我们仔细观察超几何分布的期望和方差，期望和二项分布一致，方差只差一个系数 $\alpha=\frac{T-n}{T-1}$ 这个系数是有名字 "finite population correction" 有限整体校正参数。有限就证明了超几何分布的采样是从有限个样本中进行的，如果样本数量无限大，这个参数接近于1，就变成了二项分布。而当T较小的时候，这个参数对整体结果影响较大，这时超几何分布和二项分布差异较大。
分析系数 $\alpha$ 也有很多有趣的，比如$n=T$ 的时候，结果是0，只有一种方法，方差是0.并且 $\alpha\in [0,1]$  其越接近1，证明放回抽样和不放回抽样差距越小，越接近零表示其差距越大。

>Theorem $a_n$ and $c_n$ be sequences of real numbers such that $a_n$ converges to $0$ ,and $c_na_n^2$ converges to $0$ .Then
$$
lim_{n\to \infty}(1-a_n)^{c_n}e^{-a_nc_n}=1
$$
In particular,if $a_nc_n$ converges to $b$ ,then $(1+a_n)^{c_n}$ converges to $e^b$

这个定理看起来跟我们前面写的没什么关系，而且证明也没有给出，但是用微积分的知识可以进行证明，我们下面还是看看二项分布和超几何分布之间的区别吧。

>Theorem Closeness of Binomial and Hypergeometric Distribution .Let $0< p < 1$ ,and let $n$ be a positive integer.Let $Y$ have the binomial distribution with parameters $n$ and $p$ .For each positive integer $T$ ,let $A_T$ and $B_T$ be integers such that  $lim_{T\to \infty}A_{T}=\infty$ , $lim_{T\to \infty}B_{T}=\infty$ ,and $lim_{T\to \infty} A_T/(A_T+B_T)=p$ .Let $X_T$ have the hypergeometric distribution with parameters $A_T,B_T$ ,and $n$ .For each fixed $n$ and each $x=0,\dots,n$
$$
lim_{T\to \infty}\frac{Pr(Y=x)}{Pr(X_T=x)}=1
$$


这个证明用到了上面未证明的定理，而整个证明也比较复杂，书上有详细的过程，我就不写了，想了解详细过程的同学去看看书吧，今天大概就这样了。

## 总结
本文介绍了超几何分布的相关知识，下一篇我们继续研究Poison分布。
待续。。。
