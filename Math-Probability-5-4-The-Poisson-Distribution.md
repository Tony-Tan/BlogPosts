---
title: 【概率论】5-4:泊松分布(The Poisson Distribution)
categories:
    - Mathematic
    - Probability
keywords:
    - Poisson Distribution
    - Poisson Processes
toc: true
date: 2018-03-28 15:40:55
---

**Abstract:** 本文介绍Poisson分布相关知识
**Keywords:** Poisson Distribution

<!--more-->
# 泊松分布
前面这几个分布包括今天说的泊松分布都是和二项分布，伯努利分布相互联系的，之间有各种各样的关系，我们的学习目的不是背诵所有这些分布的性质，而是在这些性质的推到过程。

很多实验比较关注次数，比如一段时间内到达商店的顾客的人数，电话交换机每分钟受到的通话请求，洪水或者其他自然人为灾害发生的次数。泊松分布被用来建模，一段事件这些事情发生的次数，并且泊松分布也是用来近似当 $p$ 很小的时候的二项分布的一种方法。
## 泊松分布的定义和性质 Definition and Properties of the Poisson Distributions
先来看一个商店一段时间有多少顾客到来的例子，这个例子会贯穿正片博客，大家应该好好读一下。

---------------
商店老板相信，顾客们以每个小时4.5 人次的数量来到商店，他想找到一个X的分布，这个X表示在未来某个一个小时，到店的客人数，并且他认为这些到来的客人之间相互独立，于是他的做法是按照一个小时3600秒计算，平均每秒来 0.00125 个人，并且假设一秒钟不会同时出现两个人同时到店的可能，那么某时间点，到达的人数为0或者1，为1的可能性是0.00125，整个过程是一个二项分布，n=3600，p=0.00125。
这看起来很正确也很流畅
于是他要计算p.f.了：
$$
f(x|n=3600,p=0.00125)=
\begin{cases}
\begin{pmatrix}
3600\\x
\end{pmatrix}p^x(1-p)^{3600-x}&\text{for }0\leq x\leq 3600\\
0&\text{otherwise}
\end{cases}
$$

这个式子非常有意思，当参数 $\begin{pmatrix} 360 0\\x \end{pmatrix}$ 变大的时候， 参数 $p^x(1-p)^{3600-x}$ 似乎以同等的速度变小，而整体却变化不大，于是我们对相邻的两个随机变量值做个比较（以下把 $X$ 扩展到在0到 $n$ 之间变化）
$$
\begin{aligned}
\frac{f(x+1)}{f(x)}&=
\frac
{\begin{pmatrix}n\\x+1\end{pmatrix}p^{x+1}(1-p)^{n-x-1}}
{\begin{pmatrix}n\\x\end{pmatrix}p^{x+1}(1-p)^{n-x-1}}\\
&=\frac{(n-x)p}{(x+1)(1-p)}\\
&\approx\frac{np}{x+1}
\end{aligned}
$$

那么根据这个比值，如果我们设 $\lambda=np$ 那么我们会有一个递归关系：
$$
f(1)=f(0)\lambda\\
f(2)=f(1)\frac{\lambda}{2}=f(0)\frac{\lambda^2}{2}\\
f(3)=f(2)\frac{\lambda}{3}=f(0)\frac{\lambda^3}{6}\\
\vdots\\
f(n)=f(n-1)\frac{\lambda}{n}==f(0)\frac{\lambda^n}{n!}\\
$$
因为$f$ 是一个近似来的 p.f. 那么我们需要让他满足我们的条件，比如，所有随机变量对应的概率求和是1.
$$
\sum^{\infty}_{x=0}f(x)=1
$$
因为整个关系式能调整的部分就只有 $f(0)$ 了，那么我们只好调整初始化条件来使得p.f.成立了，
$$
\sum^{\infty}_{x=0}f(0)\frac{\lambda^n}{n!}=1\\
f(0)\sum^{\infty}_{x=0}\frac{\lambda^n}{n!}=1\\
\text{for :}\sum^{\infty}_{x=0}\frac{\lambda^n}{n!}=e^{\lambda}\\
\text{so :}f(0)=e^{-\lambda}
$$
所以我们只需要让 $f(0)=e^{-\lambda}$ 即可，关于求和等于 $e^{\lambda}$ 的计算可以参开微积分书籍。

那么我们就有了一个新的能够近似上面二项分布的新分布——Poisson Distribution：
$$
f(x|\lambda)=
\begin{cases}
\frac{e^{-\lambda}\lambda^x}{x!}&\text{for }x=1,2,3,\dots\\
0&\text{otherwise}
\end{cases}
$$

这个分布就是我们今天的主角，也是概率论中非常重要的一个分布，可以用来描述一段时间内某事发生的次数的模型。

>Definition Poisson Distribution.Let $\lambda > 0$ .A random variable X has the Poisson Distribution with mean $\lambda$ if the p.f. of $X$ is as follow:
$$
f(x|\lambda)=
\begin{cases}
\frac{e^{-\lambda}\lambda^x}{x!}&\text{for }x=1,2,3,\dots\\
0&\text{otherwise}
\end{cases}
$$

还是传统的定义方法，告诉你，这个式子是泊松分布~

### 泊松分布的均值 Mean
>Theorem Mean. The mean of Poisson Distribution with p.f. equal to upside is $\lambda$ .
怎么样！神奇不神奇~均值是 $\lambda$
我们接下来就来证明这一点。
直接使用期望的定义
$$
E(X)=\sum^{\infty}_{x=0}xf(x|\lambda)
$$
当x=0 时，值为0，我们直接从1开始
$$
\begin{aligned}
E(X)&=\sum^{\infty}_{x=0}x\frac{e^{-\lambda}\lambda^x}{x!}\\
&=\sum^{\infty}_{x=1}\frac{e^{-\lambda}\lambda^x}{(x-1)!}\\
&=\lambda\sum^{\infty}_{x=1}\frac{e^{-\lambda}\lambda^{x-1}}{(x-1)!}\\
\text{if we set } y=x-1\\
&=\lambda\sum^{\infty}_{y=0}\frac{e^{-\lambda}\lambda^{y}}{y!}
\end{aligned}
$$
这样 $\sum^{\infty}_{y=0}\frac{e^{-\lambda}\lambda^{y}}{y!}$ 变成了一个对p.f.为$f(y|\lambda)$ 的概率函数求和的计算，结果必然为1，那么我们就证明了泊松分布的期望是 —— $\lambda$

### 泊松分布的方差 Varaince
>Theorem Variance.The variance of Poisson distribution with mean $\lambda$ is also $\lambda$
意外不意外！惊喜不惊喜！依旧是 $\lambda$
证明：
我们将用到和上面证明期望一样的方法就是通过凑，来使得求和里面变成 p.f.的样子
$$
\begin{aligned}
E[X(X-1)]&=\sum^{\infty}_{x=0}x(x-1)f(x|\lambda)\\
&=\sum^{\infty}_{x=2}x(x-1)f(x|\lambda)\\
&=\sum^{\infty}_{x=2}x(x-1)\frac{e^{-\lambda}\lambda^x}{x!}\\
&=\lambda^2\sum^{\infty}_{x=2}\frac{e^{-\lambda}\lambda^{x-2}}{x-2!}\\
\text{We set }y=x-2\\
E[X(X-1)]&=\lambda^2\sum^{\infty}_{y=0}\frac{e^{-\lambda}\lambda^y}{y!}\\
&=\lambda^2
\end{aligned}
$$

然后我们祭出我们的大招 $E[X(X-1)]=E[X^2]-E[X]=E[X^2]-\lambda=\lambda^2$ 所以 $E[X^2]=\lambda^2+\lambda$ 那么
$$
Var(X)=E[X^2]-E^2[x]=\lambda^2+\lambda-\lambda^2=\lambda
$$

至此证毕，构造了 $E[X^2]$ 然后求出了 $Var(X)$

### 泊松分布的距生成函数 m.g.f.
接着我们研究第三大工具，m.g.f.
>Theorem Moment Generating Function.The m.g.f. of the Poisson distribution with mean $\lambda$ is
$$
\psi(t)=e^{\lambda(e^t-1)}
$$
for all real $t$
证明如下：
$$
\psi(t)=E(e^{tX})=\sum^{\infty}_{x=0}\frac{e^{tx}e^{-\lambda}\lambda^x}{x!}=e^{-\lambda}\sum^{\infty}_{x=0}\frac{(\lambda e^t)^x}{x!}
$$
根据 $e$ 级数性质
$$
\sum^{\infty}_{x=0}\frac{(\lambda e^t)^x}{x!}=e^{\lambda e^t}
$$
那么我们对于 $-\infty < t< \infty$ 有：
$$
\psi(t)=e^{-\lambda}e^{\lambda e^t}=e^{\lambda(e^t-1)}
$$

有了m.g.f就能得到期望，方差或者其他阶距。

### 泊松分布随机变量相加

>Theorem If the random variable $X_1,\dots,X_k$ are independent and if $X_i$ has Poisson distribution with mean $\lambda_i(i=1,\dots,k)$ ,then the sum $X_1+\dots+X_k$ has the Poisson distribution with mean $\lambda_1+\dots+\lambda_k$

拥有相同参数的二项分布可以进行加法运算，这一点我们前面就已经证明过了，今天要证明的是Poisson分布也能进行加法，而且不需要参数一致，用到的方法是用m.g.f进行分析：

证明：
首先令 $\psi_i(t)$ 来定义 $X_i$ 的m.g.f. 并且$X_i$ 是均值为 $\lambda_i$ 的Poisson分布。并且 $X_1,\dots,X_n$ 之间相互独立，那么对于 $-\infty<t<\infty$ 我们有：
$$
\psi(t)=\Pi^k_{i=1}\psi_i(t)=\Pi^k_{i=1}e^{\lambda_i(e^t-1)}=e^{(\lambda_1+\dots+\lambda_k)(e^t-1)}
$$

结合前面泊松分布的m.g.f.可见定理成立。
## 二项分布的泊松近似 The Poisson Approximation to Binomial Distributions
接下来我们研究一下泊松分布近似二项分布的详细内容。
>Theorem Closeness of Binomial and Pisson Distribution.For each integer n and each $0 < p < 1$ ,let $f(x|n,p)$ denote the p.f. of the binomial distribtuion with parameters $n$ and $p$ .Let $f(x|\lambda)$ denote the p.f. of the Poisson distribution with mean $\lambda$ .Let ${\{P_n\}}^{\infty}_{n=1}$ be a sequence of numbers between 0 and 1 such that $lim_{n\to \infty}np_n=\lambda$ . Then
$$
lim_{n\to \infty}f(x|n,p_n)=f(x|\lambda)
$$
for all $x=0,1\dots$

定理表明了二项分布和Poisson分布的近似关系，虽然我们在开篇的例子里面已经提到了用Poisson分布来近似$n$ 比较大，$p$ 比较小，$np$ 又不大的问题，但是我们还是需要从理论上分析下二项分布和Poisson分布到底有什么关系。
证明：
首先我们写出二项分布
$$
f(x|n,p_n)=\frac{n(n-1)\dots(n-x+1)}{x!}p_n^x(1-p_n)^{n-x}
$$
提示，把组合运算展开写的。
然后我们令 $\lambda_n=np_n$ 那么 $lim_{n\to \infty}\lambda_n=\lambda$ 这样我们就有
$$
f(x|n,p_n)=\frac{\lambda_n^x}{x!}\frac{n}{n}\cdot\frac{n-1}{n}\dots \frac{n-x+1}{n}(1-\frac{\lambda_n}{n})^n(1-\frac{\lambda_n}{n})^{-x}
$$

对于每个 $x\geq 0$ 来说，我们有：
$$
lim_{n\to \infty}\frac{n}{n}\cdot\frac{n-1}{n}\dots \frac{n-x+1}{n}(1-\frac{\lambda_n}{n})^{-x}=1
$$
这个是微积分要解决的问题，不知道的同学需要去参考下微积分的知识。上文中倒数第二个定理，我们没有证明，但是那个结论在这里还需要再次使用
$$
lim_{n\to \infty}(1-\frac{\lambda_n}{n})^{n}=e^{-\lambda}
$$

所以
$$
lim_{n\to \infty}f(x|n,p_n)=\frac{e^{-\lambda}\lambda^x}{x!}=f(x|\lambda)
$$

证毕。
接下来这个定理是说超几何分布和Poisson分布之间的关系的，没有证明，但是可以参考下结论。

>Theorem Closeness of Hypergeometric and Poisson Distribution.Let $\lambda>0$ .Let $Y$ have the Poisson distribution with mean $\lambda$ .For each postive integer  $T$ ,let $A_T,B_T$ ,and $n_T$ be integers such that $lim_{T\to \infty}n_TA_T/(A_T+B_T)=\lambda$ .Let $X_T$ have the hypergeometric distribution with parameters $A_T,B_T$ and $n_T$ .Tor each fixed $x=0,1,\dots$ ，
$$
lim_{T\to \infty}\frac{Pr(Y=x)}{Pr(X_t=x)}=1
$$

## 泊松过程 Poisson Processes
前面我们第一个例子说的如何估算在一个小时内到店的客户，那么如果是我想知道半个小时或者15分钟的顾客数量呢？难道是要用2.25个或者1.125个作为平均数的Poisson Distribution建模么？于是我们使用Poisson过程来对这种情况建模。

>Definition Poisson Process.A Poisson process with rate $\lambda$ per unit time is a process that satisfies the following two properties:
i: The number of arrivals in every fixed interval of time of length $t$ has the Poisson distribution with mean $\lambda t$
ii: The numbers of arrivals in every collection of  disjoint time intervals are independent

泊松过程满足两点要求，首先固定时间段内平均时长 $\lambda t$，其次每个不同时段之间人数彼此独立。
所以上面我们说是否能用改了 $\lambda$ 的泊松分布建模这个答案是肯定的就是通过改变 $\lambda$ 值来重新建模的。
后面有一个关于泊松过程的选读内容，有兴趣的同学可以在书上找到
## 总结
本文介绍泊松分布，性质及用途，以及泊松过程
明天继续。。





原文地址1：[https://www.face2ai.com/Math-Probability-5-4-The-Poisson-Distribution](https://www.face2ai.com/Math-Probability-5-4-The-Poisson-Distribution)转载请标明出处
