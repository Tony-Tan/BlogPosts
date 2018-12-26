---
title: 【概率论】1-3:组合(Combinatorial Methods)
categories:
  - Mathematic
  - Probability
tags:
  - 组合
  - 二项式系数
  - 多项式系数
toc: true
date: 2018-01-26 9:20:58
---

**Abstract:** 本文主要介绍组合的相关知识，以及引出的二项式系数，多项式系数
**Keywords:** Combination，Binormial Coeffcient，Multinormial Coeffcient

<!--more-->
# 组合
苏东坡有诗云：“可使食无肉，不可居无竹。无肉令人瘦，无竹令人俗。人瘦尚可肥，俗士不可医。” 穷可以通过任意一种方法变富，但是没文化真的没办法短时间变富。从概率论的角度来说，如果我们有文化，我们可以通过买彩票变富，比如买32个数选6个，我们的中奖概率是$P_{32,6}$ ,我们有这么大的概率一夜变富，但是目前科技还没有能一夜变得才高八斗的方式概率能超过$P_{32,6}$ 的，所以，我一直跟周围的人说，钱很好赚，但是有文化这个事真的特别难，钱对于人来说个附属品，可以随时增减，但是文化思想是属性，不好改。
## 组合方法 Combinatorial Methods
我们的废话中还复习了下上一节的知识，彩票分两种，一种是有序排列，一种是无序排列，比如开奖过程中**Without Replacement**的方式抽取，先后出来的号码是 $\{4,1,5,3,8\}$ ，那么第一种中奖方式是：“你的五个号码必须与上述号码一致，并且，顺序必须一致！如果你的号码是 $\{1,4,3,8,5\}$ ，那么恭喜你，一分钱都没中！”；第二种方式是：“号码部分顺序，只要你的数字是 $\{1,3,4,5,8\}$ 这个集合就可以”，第一种方式就是我们昨天说的排列问题，第二种就是我们今天要说的组合，两种计数方法的共同前提就是**Without Replacement**。如果**With Replacement**，那么情况将会完全不同。
### 组合 Combination
重新描述下问题，回到最初的设计，我们把试验的所有结果看做一个有限的集合，也就是所说的有限的样本空间，多步without replacement的过程中，每一步之间没有影响，如果每次抽取的结果严格要求与步骤保持对应，就是所说的排列问题，如果我们只关心结果的元素而不关心顺序，那么我们就要用到组合（如果不太明白这段话，可以参考上面的例子，或者下面的例子）

>Definition Combination:Consider a set with $n$ elements.Each subset of size $k$ chosen from this set is called a combination of $n$ elements taken $k$ at a time.We denote the number of distinct such combinations by the symbol $C_{n,k}$

为什么说一次取 $k$ 个呢？因为一次拿出来就没有先后顺序了。所以当我们从一个有 $\{1,2,3\}$ 的集合里面拿出两个元素，
Permutation的结果是：

$$
\{
(1,2),(1,3),(2,1),(2,3),(3,1),(3,2)
\}
$$

而Combination的结果是：

$$
\{
\{1,2\},\{1,3\},\{2,3\}
\}
$$
注意看我用的括号，一个是小括号，而一个大括号，小括号的(1,2)和(2,1)是不同的，但是大括号的{1,2}和{2,1}却不做区分，这个过程明确了，那么我们就要思考接下来的问题了，如何计算不考虑顺序的数量。

数学有个非常有意思的地方就是，当你在不断地学习时，相当于你在不断的扩展你的工具包，而且这些工具包没有重要与次要，只是看你需要哪个，你可以用各种已经证明过得定理来证明你当前的问题，我们上一篇学到的Permutation就可以马上拿来计算Combination，而且，我们也可以用其他的定理来计算Combination。

继续解决问题，如果我们倒着看，先看Combination我们假设 $A=\{1,2,3\}$ 是一个集合S（包含 $n$ 个元素）的子集，这是一个combination的结果，那么它对应的S的Permutation结果就是：A集合中三个元素的Permutation，那么也就是说：

$$
C_{n,3}*P_{3,3}=P_{n,3}
$$

很明显我们可以把3一般化成 $k(k\leq n)$ 那么:

$$
C_{n,k}*P_{k,k}=P_{n,k}\\
C_{n,k}=\frac{P_{n,k}}{P_{k,k}}\\
C_{n,k}=\frac{n!}{k!(n-k)!}
$$
再次说明一下，我们这两篇所讲的都是计数方法，不涉及到随机，不管是Permutation和Combination都是数数的过程，其结果是数字（一般来说都是自然数），对试验，以及试验结果没有任何影响，一旦试验条件确定，这个数字唯一确定，不会有任何更改。
重复强调其不影响随机的主要原因是，我上高中的时候老师给我们讲过二项式，而那时我理解的是在二项式中随意挑数字，所以有随机过程在里面，其实完全没有，这两节跟随机一毛钱关系都没有，所以下面我们开始研究二项式
## 二项式系数 Binormial Coeffcient
二项式：
$$
(x+y)^n
$$
其中 $n>0$ 为常整数，其展开形式就是二项式展开，每一项的系数就是二项式系数，举个🌰：
$$
(x+y)^2=x^2+2xy+y^2
$$
这个是比较简单的二项式（最简单的是：x+y），初中就学过，对应的n=2，二项式对应的系数：
$x^2:1$
$xy:2$
$y^2:1$

那么这个和我们的Combination有什么关系呢？
我们展开二项式看一下：
$$
(x+y)^n=\underbrace{(x+y)(x+y)\dots (x+y)}_{n}
$$

一共n个元素组成了多项式（，那么当我们想要得到项 $x^ky^{n-k}$ 的时候，我们要做的是在n个式子中选k个来使用x，而不需要关心y因为k个x一旦选中，y自然是剩下的n-k个式子的y，来看个图，就豁然开朗了：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-1-3-Combinatorial-Methods/1-binomial.png)

我们来看 $x^1y^{n-1}$ ，最简单的是我们选中第一项的x 做为 $x^1$，他会和剩下的所有项的y相乘，但是不会和自己的y相乘，这样，我们还可以选剩下所有项中的x作为 $x^1$ ,一共有$C_{n,1}$ 种选择方法。

接着我们看 $x^2y^{n-2}$ :
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-1-3-Combinatorial-Methods/2-binomial.png)

这个就复杂一点，首先黄色问好是想说当我们选中第一项，第二项的x来作为 $x^2$ 中的x的时候，我们要不要把他们中y放进 $y^{n-2}$ ,答案当然是不能，这两项的结合是 $xy$ 而不是 $x^2$ 这样，我们就可以继续上面的思路了，一共n个项，选出两项的方法就是$C_{n,2}$

于是我们得出了我们的主题，二项式对应的系数就是Combination的结果：

对于正整数 $n$ ，小于等于n的正整数 $k$ :
$$
(x+y)^n=\sum^n_{k=0}\begin{pmatrix}n\\k\end{pmatrix} x^ky^{n-k}
$$

这就是二项式定理，如果上面的解释真的看不懂，那个笔，自己分析下3次的二项式，就基本明白了。
接着这个定理也比较有用，而且很直观：
$$
\begin{pmatrix}n\\k\end{pmatrix}=\begin{pmatrix}n\\n-k\end{pmatrix}
$$
想要证明？很容易：
$$
(x+y)^n=(y+x)^n=\underbrace{(y+x)(y+x)\dots (y+x)}_{n}
$$

1. 计算第 k项（ $y^kx^{n-k}$ ）的系数，可以直接套用二项式定理，可以得到 $\begin{pmatrix}n\\k\end{pmatrix}$ ,如果按照x+y的顺序呢？就是选择n-k项（$x^{n-k}y^k$）于是得到 $\begin{pmatrix}n\\n-k\end{pmatrix}$ ，很明显，相等。
Q.E.D
2. 从计算的角度：
$$
C_{n,k}=\frac{n!}{k!(n-k)!}\\
C_{n,n-k}=\frac{n!}{(n-k))!(n-(n-k))!}=\frac{n!}{k!(n-k)!}
$$
Q.E.D

举个特别的例子，上面说的都是without replacement的方式进行的，如果是with replacement的呢？
> 可以有两组，每组n种颜色的球，每组取一个一共有多少Combination？
假设第二个球与第一个球颜色不同，那么我们有$C_{n,2}$ 种取法，那么如果颜色可以相同呢？当然是再加上n被，因为之前被排除掉的n再补回来：
$$
n+\begin{pmatrix}n\\2\end{pmatrix}
$$
原始例子是关于基因的，讲出来可能大家不太懂，需要等位基因，表现型等比较专业的知识背景（哈哈，我是学生物医学工程的）。
二项式解决了，能不能应用到多项式呢？试试吧。



## 多项式系数 Multinormial Coeffcient
首先假设另一种试验，上面我们注意到定义说一次取出$k$个，那么如果我们分步进行抽取呢？两步理论上互不影响，分两步分别取$k_1$,$k_2$个，根据乘法原理和上面的Combination，我们可以确定分两步，取$k_1$,$k_2$ 并且 $(k_1+k_2\leq n)$ 会有：
$$
\begin{pmatrix}n\\k_1\end{pmatrix}\begin{pmatrix}n-k_1\\k_2\end{pmatrix}=\frac{n!}{k_1!(n-k_1)!}\frac{(n-k_1)!}{k_2!(n-k_1-k_2)!}\\
=\frac{n!}{k_1!k_2!(n-k_1-k_2)!}=\frac{n!}{k_1!k_2!k_3!} \quad where \quad (k_3=n-k_1-k_2)
$$
上面的$k_3$ 是为了整洁写上去的，同时如果我们分三步，$k_1$,$k_2$,$k_3$ 并且三步之后把所有的取完，那么$k_3$ 就有意义了。
如果我们把n分成k份（随意分）并且分成k步执行，那么定义：
$$
\begin{pmatrix}n\\n_1，n_2，\dots ，n_k\end{pmatrix}=\frac{n!}{n_1!n_2!\dots n_k!}
$$
称之为多项式系数，文字上的意义就是分多次取，每次同时取$n_i$ 个，without replacement，直到最后把所有取完，一共的结果的个数。

多项式定理：
$$
(x_1+x_2+\dots+x_k)^n=\sum\begin{pmatrix}n\\n_1，n_2，\dots ，n_k\end{pmatrix}x_1^{n_1}x_2^{n_2}\dots x_k^{n_k}\\
n_1+n_2+\dots+n_k=n
$$
上述$n_i$ 都是非负整数，多项式系数也可以用二项式的那种图来解释，但是过于麻烦，这里就不再举例了，二项式可以看做多项式系数的退化版本，因为从算式上可以清楚的看出：
$$
\begin{pmatrix}n\\k，n-k\end{pmatrix}=\begin{pmatrix}n\\k\end{pmatrix}
$$
## 总结
完成另一种计数方法Combination的总结，这两篇给的例子不多，但是每个都很经典，有时候看懂例题更重要，因为概率以例子为主，待续。。





原文地址1：[https://www.face2ai.com/Math-Probability-1-3-Combinatorial-Methods](https://www.face2ai.com/Math-Probability-1-3-Combinatorial-Methods)转载请标明出处
