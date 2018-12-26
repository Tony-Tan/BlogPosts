---
title: 【概率论】3-7:多变量分布(Multivariate Distributions Part II）
categories:
  - Mathematic
  - Probability
tags:
  - 条件分布
  - 贝叶斯理论
  - 直方图
  - 全概率公式
toc: true
date: 2018-03-15 09:20:38
---

**Abstract:** 本文继续上文的内容，讲解多变量分布的条件分布，全概率公式和贝叶斯公式。提出直方图的概念。
**Keywords:** Conditional Distributions，Law of Total Probability，Bayes' Theorem，Histograms

<!--more-->
# 多变量分布
向往自由的人更懂得约束自己，约束自己的行为，自由自己的思想，而我们现在周围的人更多的是反过来的，约束自己的思想，告诉自己不许瞎想，但是自由自己的行为，想干嘛干嘛，毕竟改革开放的红利使得大部分人从温饱过渡到有车有房还可以随意出门旅游看看世界了，而思维思想，包括知识，还停留在改革开放之前，这就导致了很诡异的一系列行为--有钱没文化。
本文继续上文，将前面的内容扩展至多变量的联合分布，最后引出关于直方图的一些知识。
## 多变量条件分布 Conditional Distributions
条件概率，是3.6中讲解的内容，我们反复的强调所有概率都是条件概率，所有分布也都是条件分布，所以与其说条件概率（分布）与一般的概率（分布）行为一致，不如说所有的所有的行为都是在条件概率的基础上进行的，只是我们省略了某些必然达成的条件。
假设 $n$ 个随机变量 $X_1,\dots,X_n$ 有一个连续的联合分布，其联合分布式p.d.f. 是  $f$ 并且 $f_0$  定义了其中 $k < n$ 个随机变量的边缘分布有 $f_0(x_1,\dots,x_n) > 0$ 那么条件分布，当条件 $X_1=x_1,\dots X_n=x_n$ 给定时 $(x_{k+1},\dots,x_n)$ 的p.d.f.是：

$$
g_{k+1,\dots,x_n}(x_{k+1},\dots,x_{n}|x_1,\dots,x_k)=\frac{f(x_1,\dots ,x_n)}{f_0(x_1,\dots,x_k)}
$$

>Definition 3.3.7 Conditional p.f. or p.d.f. Suppose that the random vector $\vec{X}=(X_1,\dots,X_n)$ is divided into two subvectors $\vec{Y}$ and $\vec{Z}$ ,where $\vec{Y}$ is a k-dimensional random vector comprising $k$ of the $n$ random variables in $\vec{X}$ and $\vec{Z}$ is an $(n-k)$-dimensional random vector comprising the other $n-k$ random variables in $\vec{X}$ .Suppose also that the $n$-dimensional joint p.f. or p.d.f. of $(\vec{Y},\vec{Z})$ is $f$ and that the marginal $(n-k)$-dimensional joint p.f. ,p.d.f. or p.f./p.d.f. of $\vec{Z}$ is $f_2$ .Then for every given point $z\in\mathbb{R}^{n-k}$ such that $f_2(z)>0$  ,the conditional $k$-dimensional p.f. p.d.f.or p.f./p.d.f. $g_1$ of $\vec{Y}$ given $\vec{Z}=\vec{z}$ is defined as follows:
$$
g_1(\vec{y}|\vec{z})=\frac{f(\vec{y},\vec{z})}{f_2(\vec{z})} \text{ for }\vec{y}\in \mathbb{R}^k
$$

这就是完整的定义，抄一遍下来还真写了不少字，但是整个思路很清晰，首先就是把多变量形成向量的形式，再把向量拆成两个小的空间当然上面这个定义的公式也可以写成：

$$
f(\vec{y},\vec{z})=g_1(\vec{y}|\vec{z}) f_2(\vec{z})
$$

写成乘法原理的形式，解决分母是0的尴尬局面。也可以看出，通过条件分布和边缘分布得到联合分布的方法是正确的（乘法原理的正确性）。
### 多变量全概率公式，贝叶斯定理 Law of Total Probability & Bayes' Theorem
条件概率和乘法原则得到证明后接着就可以扩展到全概率公式和贝叶斯公式：

>Theorem  Multivariate Law of Total Probability and Bayes' Theorem .Assume the conditions and notation given in Definition 3.3.7 .If $\vec{Z}$ has a continuous joint distribution ,the marginal p.d.f. of $\vec{Y}$ is
$$
f_1(\vec{y})=\underbrace{\int^{\infty}_{-\infty} \dots \int^{\infty}_{-\infty}}_{n-k}g_1(\vec{y}|\vec{z})f_2(\vec{z})d\vec{z}
$$

and the conditional p.d.f. of $\vec{Z}$ given $\vec{Y}=\vec{y}$ is
$$
g_2(\vec{z}|\vec{y})=\frac{g_1(\vec{y}|\vec{z})f_2(\vec{z})}{f_1(\vec{y})}
$$

上面的积分如果在离散概率情况要变成求和。
### 条件独立多随机变量 Conditionally Independent Random Variables
当我们定义某两个或某几个随机变量独立的时候，是看其概率分布的关系是否满足乘法关系，但是当我们说某两个或几个随机变量条件独立的时候，条件一定是同一个条件，比如 $\vec{x}$ 在条件 $\vec{z}$ 下和 $\vec{y}$ 相互独立。 意味着 $f(\vec{x},\vec{y}|\vec{z})=f_1(\vec{x}|\vec{z})f_2(\vec{y}|\vec{z})$ ,不可能出现这种情况：$\vec{x}$ 在条件 $\vec{z}$ 下和 $\vec{y}$ 在条件 $\vec{w}$ 下相互独立。

>Definition Conditionally Independent Random Variables.Let Z be a random vector with joint p.f.,p.d.f. or p.f./p.d.f. f_0(\vec{z}) .Several random variables X_1,\dots,X_n are conditionally independent given Z if,for all z such that f_0(z)>0 we have
$$
g(\vec{x}|\vec{z})=\prod_{i=1}^ng_i{x_i}|\vec{z})
$$

注意上面公式中 $g(\vec{x}|\vec{z})$ 是多变量的条件随机分布，而后面的乘数 $g_i{x_i}|\vec{z})$ 条件是多变量的而 $x_i$ 是单变量

进一步扩展，我们现在已经基本知道了前面所有分布定理的条件版本，但是只有一个条件，当条件是多个的时候，我们进行相同的操作，比如多条件的贝叶斯公式：
$$
g_2(\vec{z}|\vec{y},\vec{w})=\frac{g_1(\vec{y}|\vec{z},\vec{w})f_2(\vec{z}|\vec{w})}{f_1(\vec{y}|\vec{w})}
$$

还是前面说的，所有分布都是条件的，独立性也一样，独立是条件独立的特殊情况。
当我们证明某个条件随机变量或者某些条件独立同分布的随机变量满足某一结论的时候，那么其非条件形式也满足！

## 直方图 Histograms

直方图用处很多，Excel中，高考试卷上，图像处理里面，直方图是我们收集到的数据的直观显示，直方图对于显示条件独立同分布的随机变量是最有效的，换句话说，如果收集到的数据不是（条件）独立同分布的，那么显示出来没有意义，因为我们抽样的时候都是假设他们独立同分布的i.i.d

>Definition Histogram. Let $x_1,\dots,x_n$ be a collection of numbers that all lie between two values  $a < b$  That is  $a\leq x_i \leq b$ for all $i=1,\dots,n$ Choose some integer $k\geq 1$ and divide the interval $[a,b]$ into $k$ equal-legth subintervals of length $\frac{b-a}{k}$ .For each subinterval,count how many of the numbers $x_1,\dots,x_n$ are in the subinterval,Let $c_i$ be the count for subinterval $i$ for $i=1,\dots,k$ ,Choose a number $r>0$ (Typically, $r+1$ ,or $r=n$ or $r=\frac{n(b-a)}{k})$ Draw a two-dimensional graph with the horizonal axis running from $a$ to $b$ .For each subinterval $i=1,\dots,k$ draw a rectangular bar of width $\frac{b-a}{k} and height equal to $\frac{c_i}{r}$ over the midpoint of the $i$th interval.such a graph is called a histogram.

上面是直方图的基本定义，interval被分成多个等长的subinterval 当然我们也可以开发出不等长的subinterval 然后用每个直方图内的方块的面积表示这个subinterval中的数量。
## 总结
今天我们就把随机变量一般化了，这更加复合实际应用，而且操作也不是很复杂。
待续。。。





原文地址1：[https://www.face2ai.com/Math-Probability-3-7-Multivariate-Distributions-P2](https://www.face2ai.com/Math-Probability-3-7-Multivariate-Distributions-P2)转载请标明出处
