---
title: 【概率论】3-7:多变量分布(Multivariate Distributions Part I）
categories:
  - Mathematic
  - Probability
keywords:
  - Joint Distributions
  - 联合分布
  - Mixed Distributions
  - 混合分布
  - Marginal Distributions
  - 边缘分布
  - Independent Random Variable
  - 独立随机变量
toc: true
date: 2018-03-14 09:55:14
---

**Abstract:** 本文将3.4，3.5，3.6的内容扩展到多个随机变量中去，并得到相对应的结论，由于内容较多，故分为两部分完成
**Keywords:** Joint Distributions,Mixed Distributions,Marginal Distributions,Independent Random Variable

<!--more-->
## 开篇废话
今天讲的废话可能真是废话，因为这个道理真是自古以来经过无数次的验证，关于合作，合作就是多个人在一起做一件事，有钱出钱有力出力，所有的想法，工作和信息都要共享。这是一个团队存在的必要条件，但是有两种东西千万别想着与团队内部人员分享，首先是别人的收益，你不能指望别人把收益分给你，这个不现实也不讲道理的，说好是谁的就是谁的，不能拿别人的任何收益，这是保证团队不崩溃的底线；第二不要让别人跟你分担风险，你的风险就是你的风险，你出资赔钱你就要忍着，不可能让别人补偿你，这是不符合规矩的，别人也没这个义务，甚至别人主动补偿都不能要；最后一点就在于沟通，有些话说了必须负责任，任何事给出预期，同时必须提示风险，别总拍着胸脯保证什么什么，尤其是没有发生的事，这样的结果就是，一旦负面情况发生，你的责任就会非常大了，而且大家会对你这个产生怀疑！
接着就是正经的废话了，关于概率论，我这两天尝试着看数理统计方面的书，发现，难度有点提升的过快，尤其是概率论掌握的不是很熟练的时候，我的概率论现在什么水平？看了一遍书，写了下书后的习题，目前也就这样，但是写完博客会是一个很大的提升，整个思路和认知都会有所提升，所以我决定先把概率论的博客写完再继续数理统计，到时候应该能通常一点了
本文是对前三节内容的扩展，我们学习概率论从试验，到事件再到随机变量，从概率，到概率分布，都是从简单的可见的，到复杂的抽象的，这篇就把前面的限制进一步减小，从单个随机变量到两个随机变量，再到今天的多个随机变量的过程
## Joint Distributions
当一个分布中随机变量的个数超过两个的时候，我们称之为多变量概率分布；在实际应用中多变量随机分布应用更广。

### Joint Discrete Distribution
>Definition Joint Distribution Function/c.d.f.:The joint c.d.f. of $n$ random variables $X_1,\dots ,X_n$ is the function $F$ whose value at every point $(x_1,\dots ,x_n)$ in n-dimensional space $\mathbb{R}^n$ is specified by the relation
$$
F(x_1,\dots , x_n)=Pr(X_1\leq x_1,X_2\leq x_2,\dots X_n\leq x_n)
$$

多变量c.d.f.和前面单变量双变量c.d.f有相似的性质
举个🌰 ：
假设某设备有三个部件，其中部件 $i$ 有可能在 $X_i$ 时刻失效，$i$ 可能是1，2，3，那么 $X_1,X_2,X_3$ 的联合 c.d.f. 是
$$
F(x_1,x_2,x_3)=\begin{cases}(1-e^{x_1})(1-e^{-2x_2})(1-e^{-3x_3})&\text{ for }x_1,x_2,x_3\geq 0\\
0&\text{otherwise}
\end{cases}
$$

使用向量记录多个随机变量，像这样： $\vec{X}=(x_1,\dots ,x_n)$ 称为随机向量，对于一个多变量的c.d.f. $F(x_1,x_2,x_3)$ 我们可以简写成：$F(\vec{X})$
这里需要注意的是当我们用向量来管理多个变量的时候，注意其维度，这时的c.d.f被定义在一个 $\mathbb{R}^n$ 的空间上。

>Definition Joint Discrete Distribution/p.f. It is said that $n$ random variables $X_1,\dots ,X_n$ have a discrete joint distribution if the random vector $(X_1,\dots ,X_n)$ can have only a finite number or an infinite sequence of different possible values $(x_1,\dots,x_n)$ in  $\mathbb{R}^n$ .The joint p.f. of $X_1,\dots,X_n$ is then defined as the function $f$ such that for every point $(x_1,\dots,x_n)\in \mathbb{R}^n$
$$
f(x_1,\dots,x_n)=Pr(X_1=x_1,\dots,X_n=x_n)
$$

这个定义说明，随机向量 $\vec{X}$ 有一个离散分布，其每个点 $\vec{x} \in \mathbb{R}^n$ 的概率是
$$
f(\vec{x})=Pr(\vec{X}=\vec{x})
$$

下面这个定理和[3.4](https://face2ai.com/Math-Probability-3-4-Bivariate-Distribution/)中双变量分布相似
> Theorem If $X$ has a joint discrete distribution with joint p.f. $f$ then for every subset $C\subset \mathbb{R}^n$ ,
$$
Pr(\vec{X}\in C)=\sum_{x\in C}f(x)
$$
如果每个随机变量$X_1,\dots,X_n$ 每个随机变量都有离散的分布，那么 $\vec{X}=(X_1,\dots,X_n)$ 就有一个离散的联合分布

### Joint Continuous Distribution
>Definition Continuous Distribution/p.d.f. It is said that $n$ random variables $X_1,\dots,X_n$  have a continuous joint distribution if there is a nonnegative function $f$ defined on $\mathbb{R}^n$ such that for every subset $C\subset \mathbb{R}^n$,
$$
Pr[(X_1,\dots,X_n)\in C ]=\underbrace{\int\dots\int}_{C}f(x_1,\dots,x_n)dx_1,\dots,dx_n
$$

上面的定义也可以用向量来重新表示，向量表示更加简单，但是使用时要注意区分：

$$
Pr[\vec{X}\in C ]=\underbrace{\int\dots\int}_{C}f(\vec{x})d\vec{x}
$$

>Theorem If the joint distribution of $X_1,\dots,X_n$ is continuous,then the joint p.d.f. $f$ can be derived from the joint c.d.f. $F$ by using the relation
$$
f(x_1,\dots,x_n)=\frac{\partial^nF(x_1,\dots,x_n)}{\partial x_1\dots \partial x_n}
$$
at all points $(x_1,\dots,x_n)$ at which the derivative in this relation exists.

上面的定理和3.4中的定理非常相似，只是进行了响应的扩展，但是理论基础一致。
举个🌰 ：
找出上面例子的joint p.d.f
$$
F(x_1,x_2,x_3)=\begin{cases}(1-e^{x_1})(1-e^{-2x_2})(1-e^{-3x_3})&\text{ for }x_1,x_2,x_3\geq 0\\
0&\text{otherwise}
\end{cases}
$$
求偏导得到
$$
f(x_1,x_2,x_3)=\begin{cases}6e^{-x_1-2x_2-3x_3}&\text{ for }x_1,x_2,x_3> 0\\
0&\text{otherwise}
\end{cases}
$$
注意，这里 $x_1,x_2,x_3$ 的范围是大于0的，而前面的例子是C.D.F. 是$x_1,x_2,x_3$ 的范围是大于等于0的
## 插播新闻
突发事件，就在我写本文的时候，Stephen Hawking 教授去世，享年76岁；非亲非故，也不是相关专业的学生，Howking先生的著作也没拜读过，但是感觉莫名的失落，希望晚辈们能继承先贤们的遗志，为了人类的进步事业做出贡献。
## Mixed Distributions
混合分布就是一个联合分布里有连续的随机变量也有离散的随机变量，而处理起来也和双变量联合分布一样，连续部分就用积分和微分处理，离散就用求和做差处理。
>Definition Joint p.f./p.d.f. Let $X_1,\dots ,X_n$ be random variables,some of which have a continuous joint distribution and some of which have discrete distributions ,their joint distribution would then be represented by a function $f$ that we call the joint p.f./p.d.f .The function has the property that the probability that $X$ lies in a subset $C \subset \mathbb{R}^n$ is calculated by summing $f(x)$ over the values of the coordinates of $x$ that correspond to the discrete random variables and integrating over those coordinates that correspond to the continuous random  variables for all piints $\vec{x}\in C$

对于连续和离散混合的分布，还是那句话对不同的敌人用不同的战术，但总体思路都是一样的
## Marginal Distributions
### Deriving a Marginal p.d.f.
对于一个多元连续的联合分布，求其边缘概率密度函数的方法是：
$$
f_1(x_1)=\underbrace{\int^\infty_{-\infty}\dots \int^\infty_{-\infty}}_{n-1}f(x_1,\dots,x_n)dx_2\dots dx_n
$$
求谁的边缘分布就把其他的所有变量求积分就可以了，当然也可以求某两个，三个，多个变量的边缘分布。
如果里面对应的变量是离散型的，就把对应的积分换成求和就可以了
### Deriving a Marginal c.d.f.
对于一个联合分布，其c.d.f. 为$F$ ,其中x_1的边缘c.d.f.为：
$$
F_1(x_1)=Pr(X_1\leq x_1,X_2<\infty,\dots ,X_n<\infty)\\
=lim_{x_2,\dots,x_n \to \infty}F(x_1,x_2,\dots,x_n)
$$

由上可见，跟双变量的操作也是如出一辙，只是变量多了一些，计算起来更加复杂，需要小心一点
## Independent Random Variable
多维的随机变量的独立性相对要复杂一些，但是原理还是相似的。
> Definition Independent Random Variables,It is said that n random varibales $X_1,\dots,X_n$ are independent if for every $n$ sets $A_1,A_2,\dots A_n$ of real numbers
$$
Pr(X_1\in A_1,X_2\in A_2,\dots,X_n\in A_n)=Pr(X_1\in A_1)Pr(X_2 \in A_2)\dots Pr(X_n\in A_n)
$$

上面定义的一个推广就是如果 $(X_1,X_2,\dots,X_n)$ 相互独立那么其非空子空间内的变量也相互独立。

>Theorem  Let $F$ denote the joint c.d.f. of $X_1,\dots X_n$ and let $F_i$ denote the marginal univariate c.d.f. of $X_i$ for $i=1,\dots,n$ The variables $X_1,\dots,X_n$ are independent if and only if,for all points $(x_1,x_2,\dots,x_n)\in \mathbb{R}^n$
$$
F(x_1,x_2,\dots,x_n)=F_1(x_1)F_2(x_2)\dots F_n(x_n)
$$

上述定理说明了独立随机变量的联合c.d.f.的求法，下面看看我们后面要经常看到了一种随机变量，他们之间的关系叫做独立同分布的随机变量，简称，i.i.d.

>Defintion Random Samples/i.i.d./Sample Size:Consider a given probability on the real line that can be represented by either a p.f. or a p.d.f. $f$ It is said that $n$ random variables $X_1,\dots,X_n$ form a random sample from this distribution if these random varibales are independent and the marginal p.f. or p.d.f. of each of them  is $f$ Such random variables are also said to be independent and identically distributed ,abbrevuated i.i.d.We refer to the number $n$ of random varibales as the sample size

上面定义了独立同分布的随机变量，样本大小，以及涉及到了一个简单的采样过程。
对于i.i.d.的随机变量，其联合p.f或者p.d.f.为
$$
g(x_1,\dots,x_n)=f(x_1)f(x_2)\dots f(x_n)
$$

因为i.i.d讲究的的是独立同分布，同分布就保证了不可能出现混合分布的形式，否则会自相矛盾。
## 总结
至此我们讲完了上半部分关于多变量的扩展，理论上难度并不是很大，但是实际操作起来可能会出现各种小问题，需要大家谨慎。
致敬Stephen Hawking 先生！
待续。。
