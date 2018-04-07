---
title: 【概率论】4-2:Properties of Expectation
categories:
    - Mathematic
    - Probability
tags:
    - Properties of Expectation
toc: true
date: 2018-03-23 10:24:47
---

**Abstract:** 本文介绍关于期望的性质，主要是计算性质，所以本文会有非常多公式定理，例子可能较少
**Keywords:** Properties of Expectation

<!--more-->
## 开篇废话
更新了下博客主题，添加了RSS订阅功能，这个功能看起来不错，以前听说过，但是一直也没用过，今天下了个软件，注册了个账号，帮忙收集信息也是不错，效率高很多，欢迎大家订阅。
本文介绍期望的一些性质，计算性质，而且很多是比较常见的随机变量函数的期望，从这篇看起来我们的套路有点越来越接近国内教材了，定义完了是计算性质，但是这个计算性质确实是必须的，不掌握好后面很多内容学起来就会吃力，就像我前两天看了一会儿统计，发现很多关于计算的的性质，在统计书籍里是直接使用的，如果不掌握好那就是好几脸懵逼。
## Basic Theorems
>Linear Function:.If Y=aX+b,where a and b are finite constants,then
$$
E(Y)=aE(X)+b
$$

线性关系，最简单的变化， $a,b$ 是有限的常数，那么新的随机变量的期望和原始变量的关系满足上式，其实用上一篇的关于随机变量函数的方法就能证明这个问题，我们来计算一下：
$$
E(Y)=E(aX+b)=\int^{\infty}_{\infty}(ax+b)f(x)dx\\
=a\int^{\infty}_{-\infty}xf(x)dx+b\int^{\infty}_{-\infty}f(x)dx\\
=aE(x)+b
$$

上面用到上一篇的公式，然后用到了积分的线性性质，完成了证明。

>Corollary If $X=c$ with probability 1 ,then $E(X)=c$

证明：
$$
E(X)=\int^{\infty}_{-\infty}cf(x)dx\\
=c\int^{\infty}_{-\infty}f(x)dx=c
$$
Q.E.D

>Theorem If there exists a constant such that $Pr(X\geq a)=1$, then $E(X)\geq a$. If there exists a constant $b$ such that $Pr(X\leq b)=1$,then $E(X)\leq b$

这个定理说明当存在一个常数 $a$ 满足 $Pr(X\geq a)=1$ 那么 $E(X)\geq a$ 另一部分是反过来的，所以我们只要证明了一半，另一半可以用同样的方法得到结论。
证明：
$$
E(X)=\int^{\infty}_{-\infty}xf(x)dx=\int^{\infty}_{a}xf(x)dx\\
\geq \int^{\infty}_{a}af(x)dx=aPr(X\geq a)=a
$$
Q.E.D
其中这一步 $\int^{\infty}_{a}xf(x)dx\geq \int^{\infty}_{a}af(x)dx$ 用到的条件是 $x\geq a$ 而 $\int^{\infty}_{a}af(x)dx=aPr(X\geq a)$ 用到的是积分的线性性质，和概率的相关定义。

> Theorem Suppose that  $E(x)=a$ and that either $Pr(X\geq a)=1$ or $Pr(X\leq a)=1$ .Then $Pr(X=a)=1$

定理解释当知道一个随机变量的期望值是 $a$ 时，那么如果知道 $Pr(X\geq a)=1$ 或者 $Pr(X\leq a)=1$ 必然有 $Pr(X=a)=1$ 。
证明当X时离散情况下 $Pr(X\geq a)=1$ 其他情况类似，假设 $x_1,x_2,\dots$ 包含所有 $x>a$ 那么 $Pr(X=x)>0$ 令 $p_0=Pr(X=a)$ 那么
$$
E(X)=p_0a+\sum^{\infty}_{j=1}x_jPr(X=x_j)
$$

每个 $x_j$ 都大于 $a$ 其和不能变大 因为
$$
E(X)\geq p_0a + \sum^{\infty}_{j=1}aPr(X=x_j)=a
$$
证毕。
其实离散情况想一下就正大概知道定理的正确性，但是连续情况下用微积分证明难度就有点大了。

>Theorem If $X_1,\dots,X_n$ are $n$ random variables such that each expectation $E(X_i)$ is finite $(i=0,\dots,n)$ ,then
$$
E(X_1+\dots+X_n)=E(X_1)+\dots+E(X_n)
$$

证明期望的加法性质，连续双随机变量证明过程如下，其他情况类似：
$$
E(X_1+X_2)=\int^{\infty}_{-\infty}\int^{\infty}_{-\infty}(x_1+x_2)f(x_1,x_2)dx_1dx_2\\
=\int^{\infty}_{-\infty}\int^{\infty}_{-\infty}x_1f(x_1,x_2)dx_1dx_2+\int^{\infty}_{-\infty}\int^{\infty}_{-\infty}x_2f(x_1,x_2)dx_1dx_2\\
=\int^{\infty}_{-\infty}x_1f_1(x_1)dx_1+\int^{\infty}_{-\infty}x_2f_2(x_2)dx_2\\
=E(X_1)+E(X_2)
$$

证明过程最关键一步是 $\int^{\infty}_{-\infty}\int^{\infty}_{-\infty}x_1f(x_1,x_2)dx_1dx_2=\int^{\infty}_{-\infty}x_1f_1(x_1)dx_1$ 的过程，首先调换积分变量的次序 $\int^{\infty}_{-\infty}\int^{\infty}_{-\infty}x_1f(x_1,x_2)dx_2dx_1$ 这样，内层积分中$x_1$ 是常量，那么就可以提出来 $\int^{\infty}_{-\infty}x_1[\int^{\infty}_{-\infty}f(x_1,x_2)dx_2]dx_1$ 这样中括号里面的部分就是 $x_1$ 的边缘变量了，同理可得 $x_2$ 的情况，故得到最后结论。


**上述证明过程证明了随机变量的和的期望等于期望的和，而不需要考虑其联合分布，同理可以推广到多变量情况**

下面我们就要考虑多变量线性关系了

>Corollary Assume that $E(x_i)$ is finite for $i=1,\dots,n$ For all constants $a_1,\dots,a_n$ and $b$
$$
E(a_1X_1+\dots + a_nX_n+b)=a_1E(X_1)+\dots a_nE(X_n)+b
$$

这个引理的证明是上面加法性质的证明以及前面第一个线性关系的扩展，这里就不再证明了。

**注意：上面线性的函数g可以有 $E[g(x)]\neq g(E[x])$ 的关系，但其他函数没有这种关系！Jensen's inequality 会给出其他函数之间两者的关系**

>Definition Convex Functions A function g of a vector argument is convex if ,for every $\alpha\in (0,1)$ and every x and y,
$$
g[\alpha x+(1-\alpha)y] \geq \alpha g(x)+(1-\alpha)g(y)
$$

这个定义就是凸函数的一般定义

> Theorem Jensen's Inequality. Let g be a convex function,and let $X$ be a random vector with finite mean.Then $E[g(X)]\geq g(E[X])$

詹森不等式，说明了函数的期望和期望的函数之间的一般大小关系，等号当且仅当$g$ 是线性函数时成立，证明过程书上没写，等我思考出完整的证明后再来补充一下。


## Expectation of a Product of Independent Random Variables
独立随机变量之积的期望关系

>If $X_1,\dots,X_n$ are $n$ independent random variables such that each expectation $E(X_i)$ is finite $(i=1,\dots,n)$ then
$$
E(\Pi^{n}_{i=1}X_i)=\Pi^{n}_{i=1}E(X_i)
$$

证明，为了方便我们假设这组连续随机变量的联合分布p.d.f. 是 $f$  并且 $f_i$ 是其中 变量 $i$ 的边缘分布，因为他们之间彼此独立，所以
$$
f(x_1,\dots,x_i)=\Pi^{n}_{i=1}f_i(x_i)
$$
因此：
$$
E(\Pi^{n}_{i=1}X_i)\\
=\int^{\infty}_{-\infty}\dots \int^{\infty}_{-\infty}(\Pi^{n}_{i=1}x_i)f(x_1,\dots,x_n)dx_1,\dots,x_n\\
=\int^{\infty}_{-\infty}\dots \int^{\infty}_{-\infty}\Pi^{n}_{i=1}[x_if_i(x_i)]dx_1,\dots,x_n\\
=\Pi^{n}_{i=1} \int^{\infty}_{-\infty}x_if_i(x_i)dx_i
$$

最后一步的拆分大家可能会感到疑惑不解，把积分内层拆开，那么每个除了当前要积分的 $x_i$ 以外，其他变量都是已知的就可以挪到本层积分的外面，然后本层积分的结果也必然是是个数字，所以一层一层的拆开就是最后的式子，如果实在看不懂，试试两个独立现需随机变量的计算请款个就知道了。

## 总结
总结就是今天我们研究了上一篇的扩展部分，我们得到了很多期望的计算性质，而这些兴致将在后面的计算中非常有用处！
待续。。。
