---
title: 【概率论】5-9:多项式分布(The Multinomial Distributions)
categories:
    - Mathematic
    - Probability
keywords:
    - The Multinomial Distributions
toc: true
date: 2018-04-04 22:17:23
---

**Abstract:** 本文介绍多项式分布的相关知识
**Keywords:** The Multinomial Distributions

<!--more-->
# 多项式分布
生病的时候才会体会到人生的短暂和生命的含义，你可以选择自己的生活，也可以选择自己的快乐，一切都是正确的。
本文开始介绍多于一个变量的分布，其实分布我们已经学了不少了后面再讲一个双变量的正态分布本章就算结束了，主要学的就是如何使用前面学到的工具来对新的随机变量的性质进行分析。今天我们来分析多项式分布。
多项式是二项分布的一个扩展。
## 多项式分布的定义和导出 Definition and Derivation of Multinomial Distribution
把[二项分布](https://face2ai.com/Math-Probability-5-2-the-Bernoulli-and-Binomial-Distributions/)中的两个变量扩展成多个变量，就能得到我们我们今天要介绍的多项式分布，而且遵守和二项式分布一样的放回的采样方式（with replacement），在计数方法中我们也学过[多项式系数](https://face2ai.com/Math-Probability-1-3-Combinatorial-Methods/)这个知识，与我们今天要说的多项式分布是紧密相关的，比如我们举个例子：
人类的血型可以分为 A，B，o，AB 四种类型，每种类型都有相应的比例（这个比例是从所有人的类型中统计计算出来的）现在才去放回式的抽样，假设我们抽取了若干个样本，得到随机变量的向量为： $\vec{x}=(X_A,X_B,X_o,X_{AB})$  对应的概率为 $\vec{p}=(p_A,p_B,p_o,p_{AB})$ 那么我们可以根据多项式系数的相关知识得到其分布：
$$
f(\vec{x}|4,\vec{p})=Pr(X_A=x_1,X_B=x_2,X_o=x_3,X_{AB}=x_4)\\
=\begin{cases}
\begin{pmatrix}
&n&\\
x_1&x_2&x_3&x_4
\end{pmatrix}p_A^{x_1}p_B^{x_2}p_o^{x_3}p_{AB}^{x_4}&\text{if } x_1+x_2+x_3+x_4=n\\
0&\text{otherwise}
\end{cases}
$$

这就是多项式系数的扩展，称为多项式分布的的样子，对应于多个随机变量，随机变量的个数为固定值。可以写成一下形式：
$$
f(\vec{x}|n,\vec{p})=
\begin{cases}
\begin{pmatrix}
&n&\\
x_1&\dots&x_k
\end{pmatrix}p_1^{x_1}\dots p_{k}^{x_k}&\text{if } x_1+\dots+x_k=n\\
0&\text{otherwise}
\end{cases}\tag{5.9.1}
$$

>Definition Multinomial Distributions.A discrete random vector $\vec{X}=(X_1,\dots,X_k)$ whose p.f. is given Eq(5.9.1) has the multinomial distribution with parameters $n$ and $\vec{p}=(p_1,\dots,p_k)$ .

这个定义看起来没什么，而且上面的例子也给出了多项式分布的一般用法，接下来我们就说说多项式分布和二项分布的关系。
## 多项式分布和二项分布的关系 Relation between the Multinomial and Binomial Distributions
>Theorem Suppose that the random vector $\vec{X}=(X_1,X_2)$ has the multinomial distribution with parameters $n$ and $\vec{p}=(p_1,p_2)$ .Then $X_1$ has the binomial distribution with parameters $n$ and $p_1$ ,and $X_2=n-X_1$

这个定理应该不需要证明了，因为多项式分布无论从定义来看还是原理来看，二项式是多项式的退化，多项式更加宽泛。
上面的定理可以轻易的推导出下面两个推论：

>Corollary Suppose that the random vector $\vec{X}=(X_1,\dots,X_k)$ has the multinomial distribution with parameters $n$ and $\vec{p}=(p_1,\dots,p_k)$ .The marginal distribution of each variable $X_i(i=1,\dots,k)$ is the binomial distribution with parameters $n$ and $p$

这个推论比较好理解，一个多项式分布的边缘分布是其他变量所有可能值求和的结果，比如一个三个随机变量的多项式分布：
$$
f_3(x_3)=\sum_{\text{all }x_1}\sum_{\text{all }x_2}f(x_1,x_2,x_3)\text{ for }x_1+x_2+x_3=n
$$
那么可见 $x_3$ 的范围是从0到 $n$ 的其概率是 $p_3=1-p_1-p_2$ 明显的这是一个二项分布，$n=n$ 以及 $p=p_3$

>Corollary Suppose that the random vector $\vec{X}=(X_1,\dots,X_k)$ has the multinomial distribution with parameters $n$ and $\vec{p}=(p_1,\dots,p_k)$ with $k > 2$ .Let $\ell<k$ ,and let  $i_1,\dots,i_{\ell}$  be distinct elements of the set $\{1,\dots,k\}$ .The distribution of $Y=X_{i_1}+\dots+X_{i_{\ell}}$ is the binomial distribution with parameters $n$ and $p_{i_1}+\dots+p_{i_{\ell}}$

这个推论的证明办法还是从二项分布出发，把本来分成多类的现在分成两类，比如一个分类可以把十个样本分为5类 $\{1,2,3,4,5\}$ 现在我们重新分，$A=\{1,2,3\}$ 以及 $B=\{4,5\}$ 那么新的分布就是二项分布 $p_A=p_1+p_2+p_3$ 以及 $p_B=p_4+p_5$

多项分布也好，二项分布也好，其根本都是伯努利分布，所以我们在考虑这类分布之间的关系的时候可以退回到伯努利分布然后分析不同的试验过程，找出其不同点。
## 均值，方差，协方差 Means,Variances and Covariances
接着就是多项式的数字特征了，因为其是多随机变量的分布，比前面讲的分布会多一个协方差(Covariances)
>Theorem Means,Variances,and Covariances.Let the random vector $X$ have the multinomial distribution with parameters $n$ and $p$ .The means and variances of the coordinates of $X$ are
$$
E(X_i)=np_i\text{ and } Var(X_i)=np_i(1-p_i)\text{ for }i=1,\dots,k
$$
the covariances between the coordinates are
$$
Cov(X_i,X_j)=-np_ip_j
$$

证明过程我们只写最后一个协方差的计算，因为期望和方差我们可以通过前面的定理理解（其边缘分布就是二项分布），其期望和方差形式与二项分布相同。
协方差计算如下
$X_i+X_j$ 为一个随机变量，其他随机变量相加为另一个随机变量，那么新的分布是一个二项分布$p=p_i+p_j$ 以及$n=n$ 那么其分布是:
$$
Var(X_i+X_j)=n(p_i+p_j)(1-p_i-p_j)
$$
根据前面学的有关方差的知识，有：
$$
Var(X_i+X_j)=Var(X_i)+Var(X_j)-Cov(X_i,X_j)\\
=np_i(1-p_i)+np_j(1-p_j)-Cov(X_i,X_j)
$$
所以
$$
\begin{aligned}
n(p_i+p_j)(1-p_i-p_j)&=np_i(1-p_i)+np_j(1-p_j)-Cov(X_i,X_j)\\
Cov(X_i,X_j)&=-np_ip_j
\end{aligned}
$$

多项分布的协方差永远是负的，这是有道理的，因为多项分布的总数有限，所以当一个多了别人肯定就会少，这样的协方差肯定是负的。
## 总结
今天我们研究了多项分布，第一个多随机变量的分布，明天我们将会学习双变量的正态分布。
待续。。。





原文地址1：[https://www.face2ai.com/Math-Probability-5-9-Multinomial-Distribution](https://www.face2ai.com/Math-Probability-5-9-Multinomial-Distribution)转载请标明出处
