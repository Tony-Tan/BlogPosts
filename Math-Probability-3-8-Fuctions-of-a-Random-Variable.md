---
title: 【概率论】3-8:随机变量函数(Functions of a Random Variable)
categories:
  - Mathematic
  - Probability
tags:
  - The Probability Integral Transformation
  - Simulation
  - Pseudo-Random Numbers
  - General Function
toc: true
date: 2018-03-16 09:49:24
---

**Abstract:** 本文介绍通过函数这个工具，来研究随机变量
**Keywords:** The Probability Integral Transformation，Simulation，Pseudo-Random Numbers，General Function

<!--more-->
## 开篇废话
我们到目前为止对概率的研究经过了试验结果，事件，随机变量大概这三个过程，其实每个过程都是更高层的抽象，比如，对于直观的事实，实验结果，我们通过一种函数，或者称为一种收集，将结果抽象成了事件，而对事件研究了一段时间后又将事件通过函数（随机变量）映射到了实数域，整个过程，更加抽象，更加复杂，但是计算和模拟现实中的试验结果变得更加容易更加准确。
对于实数的研究，函数是绕不开的话题，而函数的微分积分等又是现代科学的基础，所以本文简要的介绍下随机变量的函数。
问题的描述变成了当我们已知一个随机变量 $X$ 具有某个p.f. 或者 p.d.f 那么随机变量 $Y=f(x)$ 分布是什么。
## Random Variable with a Discrete Distribution
先看一个例子：
离散随机变量 $X$ 在 $[1,\dots ,9]$ 有一个均匀分布，我们关系的是随机变量距离区间中心5的距离Y的分布情况。
这时候 $Y$ 的定义的数学化表示是： $Y=|X-5|$ 其分布函数不太好写，但是可以列举出来：
$$
Y\in \{0,1,2,3,4\}\\
Pr(Y=1)=Pr(X\in {4,6})=\frac{2}{9}\\
Pr(Y=2)=Pr(X\in {3,7})=\frac{2}{9}\\
Pr(Y=3)=Pr(X\in {2,8})=\frac{2}{9}\\
Pr(Y=4)=Pr(X\in {1,9})=\frac{2}{9}\\
Pr(Y=0)=Pr(X\in {5})=\frac{1}{9}\\
$$

这就是一个最简单的例子，关于离散随机变量的函数的分布问题。

>Theorem Function of a Discrete Random Variable. Let $X$ have a discrete distribution with p.f. $f$ and let $Y=r(X)$ for some function of $r$ defined on the set of possible values of $X$ For each possible value y of $Y$ the p.f. $g$ of $Y$ is
$$
g(y)=Pr(Y=y)=Pr[r(X)=y]=\sum_{x;r(x)=y}f(x)
$$

解读下上面的公式，其实公式写的很清楚，当我们知道函数r了以后，满足 $r(X)=y$ 的所有X对应的概率最后组合成了Y，所以要进行求和，其实这一步跟从试验结果得到事件的过程是一样的。但是下面对于连续分布来说，就非常不一样了。
## Random Variable with a Continuous Distribution
继续用例子开头
已知连续随机变量 $X$ 有在 $[-1,1]$ 一个均匀分布，那么我们求连续随机变量 $Y=X^2$ 的分布：
1. 根据条件我们有：
$$
f(x)=
\begin{cases}
\frac{1}{2}&\text{for } -1\leq x\leq 1\\
0&\text{otherwise}
\end{cases}
$$
2. 分析我们的目标函数 $Y=X^2$ 其定义域 $[-1,1]$ 其值域 $[0,1]$
3. 那么随机变量 $Y$ 的c.d.f. 应该是：
$$
G(y)=Pr(Y\leq y)=Pr(X^2\leq y)\\
=Pr(-y^{1/2}\leq X\leq y^{1/2})\\
=\int^{y^{1/2}}_{-y^{1/2}}f(x)dx=y^{1/2}
$$
4. 因为$0<y<1$ 所以 c.d.f. g(y)是
$$
g(y)=\frac{dG(y)}{dy}=\frac{1}{2y^{1/2}}
$$
或者其他情况是0。

分析下上面的例子，并没有直接从随机变量X来计算随机变量Y的分布，原因很简单，没办法算，因为这个是个连续随机变量，没办法像离散随机变量一样得到满足 $r(X)=y$ 的所有X，因为连续情况下有无限多组这样的对应，而且上面的例子并不是一对一的双射函数（双射函数的定义可以参考数学分析教材）这就是最重要的麻烦，只要得到一对一的双射关系（本例中是X的p.d.f.的一个区间 对应 Y的一个p.d.f的区间）所以，上面用的就是这个思路，最后得到了相应的解。
现在我们只给出最简单的函数变换，线性变换对随机变量的影响，其推理过程和上面例子相似，只是这里我们直接给出结果:

> Theorem Linear Function: Suppose that X is a random variable for which the p.d.f. is f and that $Y=aX+b(a\neq 0)$ .Then the p.d.f. of Y is
$$
g(y)=\frac{1}{|a|}f(\frac{y-b}{a}) \text{ for } -\infty <y<\infty
$$

因为连续随机变量的函数变换比较复杂，这里面只给出了一个线性变换的变换定理，其他变换方式变换需要用到上面例子那种分析方法来获取。同样线性的结论也可以用例子中的方法来证明。

## The Probability Integral Transformation
接下来我们进入今天的难点，就是概率的积分问题，并且会引出一个非常重要非常有用的结论。
🌰 ：
连续随机变量X 的p.d.f.
$$
f(x)=
\begin {cases}
e^{-x}& \text{ for } x>0\\
0&\text{ otherwise}
\end {cases}
$$
那么X 的c.d.f.：
$$
F(x)=
\begin {cases}
1-e^{-x}& \text{ for } x>0\\
0&\text{ otherwise}
\end {cases}
$$

如果按照我们前面定义的随机变量Y跟X的关系满足 $Y=r(x)$ ,那么我们另 $r=F$ 会得到什么结论了，换句话说，随机变量 $Y=F(x)$ 那么Y的c.d.f.怎么求（X的c.d.f.的c.d.f.）

$$
G(y)=Pr(Y\leq y)=Pr(1-e^{-x}\leq y)=Pr(X\leq -log(1-y))\\
=F(-log(1-y))=1-e^{-[-log(1-y)]}=y
$$
上面的结果是 $G(y)=y$ 是 $[0,1]$ 的上的均匀分布！这是个意外还是个巧合？一个随机变量的c.d.f函数(或者叫做积分)变化来的随机变量的c.d.f.居然是 $[0,1]$ 均匀分布！

> Theorem Probability Integral Transformation .Let $X$ have a continuous c.d.f. $F$ and let $Y=F(x)$. (This transformation from $X$ to $Y$ is called the probability integral transformation)This distribution of $Y$ is uniform distribution on the interval of $[0,1]$

证明过程如下：
1. $F$ 是单随机变量 $X$ 的c.d.f. 那么其定义域可以是 $\mathbb{R}$ 但是其值域为 $[0,1]$
2. 因为 $Y=F(X)$ 所以 $Y\in [0,1]$ 并且 $Pr(Y>1)=Pr(Y<0)=0$
3. 那么如果我们想找到 $X$ 中的某个点 $x_0$ 和 $Y$ 中的 $y_0$ 对应相当于[3.3中关于分位数的计算](https://tony4ai.com/Math-Probability-3-3-Cumulative-Distribution-Function/)中，$X$ 的 $y_0$ 分位数，并且每个分位数和对应的分位是一一对应的
4. 这样的话，根据意义对应的关系，如果要保证 $Y\leq y_0$ 的充分必要条件是 $X\leq F^{-1}(y_0)=x_0$
5. 那么对应的概率关系就是 $G(y)=Pr(Y\leq y)=Pr(X\leq x_0)=F(x_0)=y$
Q.E.D

看起来很神奇的一个结论，就被我们这么证明了，最重要的一点就是对于连续随机变量，其c.d.f.后的变量可以和原始变量产生一个一对一满射的关系，继续进行c.d.f的计算就能得到一个均匀分布的c.d.f

上面的结论进一步精简就是任意连续分布的c.d.f. 函数 $F$ 的c.d.f.函数 $F$ 就是均匀分布，那么一个均匀分布的 $F^{-1}$ 就是一个任意连续分布的c.d.f（ $F$ ）

>Corollary Let $Y$ have the uniform distribution on the interval $[0,1]$ ,and let $F$ be a continuous  c.d.f. with quanntile function $F^{-1}$.Then $X=F^{-1}(Y)$ has c.d.f. $F$

上面的定理给我们了一个重要的引用结论，也就是如何通过均匀生成任意的分布的随机变量
## Simulation
上面的结论帮助我们仿真生成随机数，我们知道我们的计算机上用于生成随机数的算法是伪随机的，用的是周期非常大的一个函数来假装生成均匀分布的随机数，有时候我们需要特定分布的随机数，那么就需要用上面的原理将均匀分布的随机变量转换成任意分布的随机变量了。
如果非常详细的描述这个过程是不可能的，因为这个课题在CS和数学界已经发展成一个专门的课题了，我没办法用我目前的知识去讲解，所以我们大概知道下任意分布的随机变量的生成：
1. 使用系统api生成若干个均匀分布的随机数 $\vec{x}$
2. 求目标分布的c.d.f的逆函数 $F^{-1}$
3. 求目标c.d.f.分布的随机变量 $F^{-1}(\vec{x})$
上面这种算法被叫做分位数法，当然我们现在大部分软件中用的都是更高级的算法，有兴趣的同学可以自己去查资料。
## 总结
本文主要介绍单个随机变量的函数，最重要的一点是连续随机变量的c.d.f的c.d.f是均匀分布，这个在后面的应用中会经常用到，推理过程都很粗糙，因为本篇的内容实在太深奥，这里只能是盲人摸象一样给出自己的理解，我们下一篇继续。。
