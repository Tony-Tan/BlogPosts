---
title: 【概率论】4-1:随机变量的期望(The Expectation of a Random Variable Part I)
categories:
    - Mathematic
    - Probability
keywords:
  - Expectation
toc: true
date: 2018-03-20 09:48:55
---

**Abstract:** 本文主要介绍期望的基础之知识，第一部分介绍连续和离散随机变量的期望。
**Keywords:** Expectation

<!--more-->
## 开篇废话
好像大家比较喜欢关于学习方面的废话，那么以后就不说社会现象了，哈哈哈。
期望是整个这一章的基础，概率论学习例子最重要，前面几节例子都写的不多，所以让大家多看书，博客只能算个总结性的东西，而期望这个概念更是需要用练习去理解，我做数学的目的是为了研究机器学习，不是为了做习题，但是做习题是最快速的学数学的方法。
为了使得基础扎实，所以把本来可以一篇完成的博客拆分成了两篇，第一篇写离散和连续随机变量的期望，下一篇写随机变量函数的期望。

## 本章引言
一个随机变量的全部信息被保存在他的分布中，当事件到随机变量的确定后，随机变量的分布唯一描述这个随机变量的全部性质。
但是整个分布包含太多信息了，比如一个复杂的分布，参数可能有几百上千个，有些性质就变得不那么明显了。
举个通俗的例子，我们描述一个人的身材（把身材当做随机变量），最完整的方法就像做CT，把整个人的三维模型数据采集出来，这就相当于其分布函数，但是这个数据量也好，耗时也好，都是非常大的，而且有些数据也没啥大作用，我们可能只关心这个人的射高体重，就能大概猜测出来这个人的大概样子，而不关心他的脑袋有多大，眼睛有多大。
这个例子是个很通俗的解释，但是类比的很恰当（为自己鼓掌）。
我们的目的就像找到身材中的身高和体重一样，找到分布中的某几个关键数值，这些数值可以反映出分布的某些重要性质——期望！
## Expectation for a Discrete Distribution
先举个不切实际的例子，买股票，通过某种计算，我们知道了某只股票的赚钱的分布，只有两种请款个，一种是赚10块钱，概率是90%，一种是赔100块钱，概率是10%。那么我们要不要买这只股票。
分析，首先事件是两个，一个是赚10元，一个是赔100，那么我们把这两个事件映射成随机变量 10，-100，那么离散分布：$Pr(10)=0.9,Pr(-100)=0.1$  我们可能赚多少钱，相当于随机变量的加权平均，也就是 $E=10\times 0.9+(-100)\times 0.1 =-1$ 我们买这只股票的赚钱期望值是-1 ，这个-1其实是没有意义的，因为我们从事件到随机变量的映射其实只做了两个事件的一对一映射，我们得到的 -1 这个随机变量根本不知道对应什么事件，但是我们可以把第一步的从事件到随机变量的映射改成一个线性的函数，也就是收益 $a$ (可正可负)对应是随机变量是 $X=a$ 那么这样就存在逆映射，随机变量-1对应赔了一块钱。

> Definition Mean of Bounded Discrete Random Variable. Let $X$ be a bounded discrete random variable whose p.f. is $f$ .the expectation of $X$ denoted by $E(X)$ ,is a number define as follow:
$$
E(X)=\sum_{\text{All }x}xf(x)
$$
The expectation of $X$ is also referred to as the mean of $X$ or the expected value of $X$

上面定义了一个有限的离散分布的期望，每个分布对应唯一的期望，有限的离散分布都有期望，但是后面要说的连续的分布可能没有期望。

一个例子，但是很重要，重要到可以当做一个定理：
一个随机变量X有一个参数为p的伯努利分布，那么他的期望是什么？
$$
E(X)=p\times 1+(1-p)\times 0=p
$$
简单的例子，但是是后面很多求解的基础组成，这个值得我们关注一下。

上面我们讲的都是有限个离散分布的情况，当X是无限的时候其实也可以求期望，也就是求所有可能的值的加权平均数

>Definition Mean of General Discrete Random Variable. Let X be a discrete random variable whose p.f. is f.Suppose that at least one of the following sums is finite:
$$
\sum_{\text{Positive }x}xf(x) , \sum_{\text{Negative }x}xf(x)
$$
Then the mean,expectation,or expected value of $X$ is said to exist and is defined to be
$$
E(x)=\sum_{\text{All } x}xf(x)
$$


这个定义跟我在其他书上看到的还是有点区别，首先是分了两类，正的随机变量求了一个加权和，负的随机变量也求了一个加权和，判断了一下这两个和是不是有限的，如果其中至少一个是有限的的，那么就能得出其期望是 $E(x)=\sum_{\text{All } x}xf(x)$  为啥两个都是无限的不行，因为没办法确定符号，当两个和有一个是无限的，我们可以认定其符号是正还是负，其值肯定是无穷，所以我们能得到一个明确的结论，是正无穷还是负无穷。
如果两个和都是无穷，一个正无穷，一个负无穷，那么他们的和将会没有意义，所以，我们的期望定义就变成了上面这个样子。

那我们就来举一个例子，无边界离散随机变量期望不存在的例子：离散随机变量X有分布如下
$$
f(x)=
\begin{cases}
\frac{1}{2|x|(|x|+1)}&\text{ if }x=\pm 1,\pm 2,\pm 3,\dots\\
0& \text{ otherwise }
\end{cases}
$$
那么他的期望是：

$$
\sum^{-\infty}_{x=-1} x\frac{1}{2|x|(|x|+1)}=-\infty \\
\sum^{\infty}_{x=1} x\frac{1}{2|x|(|x|+1)}=\infty
$$

所以期望不存在。
期望可以是任意一个实数，当然也包括 $\pm \infty$ 前提是必须明确知道这个实数是什么。

注意：期望之和分布唯一相关，和其他任何东西都无关，如果两个随机变量有同样的分布，那么他俩就有一样的期望，即使他俩是风马牛不相及的事物。
所以我们常说一个分布的期望是多少，甚至不知道这个分布的随机变量是啥都无所谓。
**期望只和分布有关系！**

## Expectation for a Continuous Distribution
到了连续情况下，我们就要用积分取代上面的所有求和，还有一个问题就是 p.f. 过度到p.d.f 的过程，p.f. 每个点对应的值就是其概率，但是p.d.f.对应的点并不是概率，那么这个区别我们要怎么处理呢？
首先我们应该不考虑p.f.对应的值是概率这个想法，而是把它仅仅当做一个权值，每个随机变量对应不同的权值，这些权值的特点是相加的和是1，同样，对于连续随机变量，有无数个随机变量，也有无数个权值，虽然这些权值不是其对应的概率，但是这些权值的和也就是积分结果也是1，满足加权平均的要求，期望的公式中，$f(x)$ 只是一个权重，虽然他有时可以是随机变量对应的概率，有时也可以不是。

>Definition Mean of Bounded Continuous Random Variable. Let X be a bounded continuous random variable whose p.d.f. is f.The expectation of X ,denote E(X),is defined as follows:
$$
E(X)=\int^{\infty}_{-\infty}xf(x)dx
$$

从求和变成了积分，求得的结果也叫做均值或者期望值。
这是有限情况下的结果，有限的随机变量必然有期望。而一般情况下的定义如下：

>Definition Mean of General Continuous Random Variable.Let X be a continuous random variable whose p.d.f. is f.Suppose that at least one of the following integrals is finite:
$$
\int^{\infty}_{0}xf(x)dx,\int^{0}_{-\infty}xf(x)dx
$$
Then the mean,expectation,or expected value of X is said to exist and is defined to be
$$
E(X)=\int^{\infty}_{-\infty}xf(x)dx
$$

和离散情况套路一致，如果两个积分结果都是无限的，那么这个分布期望不存在，如果其中一个有限，那么期望结果就是确定的，原因也一致。

下面这个例子必须给出，一个伟大的人给出的特例，柯西分布：
计算以下分布函数的期望：
$$
f(x)=\frac{1}{\pi(1+x^2)} \text{ for } -\infty < x < \infty
$$
求积分可以知道其结果是1，因为
$$
\frac{d}{dx}tan^{-1}(x)=\frac{1}{1+x^2}
$$
具体求积分这段就不写了，容易证明，柯西分布是一个合法的分布，但是其期望：
$$
\int^{\infty}_{0}\frac{x}{\pi(1+x^2)}dx=\infty
$$
同理
$$
\int^{0}_{-\infty}\frac{x}{\pi(1+x^2)}dx=-\infty
$$

所以其期望不存在，那么是什么原因导致本来收敛的积分变得不确定了呢？原因是乘以了x，使得p.d.f. 的收敛性发生了极大的变化。

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-4-1-The-Expectation-of-a-Random-Variable-P1/cauchy.png)

## Interpretation of the Expectation

其中有些关于期望的性质要说一下：
均值和重心的关系，一个分布的均值一般来说是在整个分布的重心。
对称分布的期望一般在对称轴所在的随机变量处。
当我们计算一个分布的期望之前要确定期望是否存在。
分布的变化对期望影响很大，期望小小的变化都会引起期望的剧烈变化。所以期望可以作为分布的一个重要特征。
## 总结
本文介绍了期望的定义和确定期望的方法，包含有限离散，有界连续，无限无界离散连续分布的期望求法，下文我们将介绍期望的性质。
