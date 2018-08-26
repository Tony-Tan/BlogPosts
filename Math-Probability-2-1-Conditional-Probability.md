---
title: 【概率论】2-1:条件概率(Conditional Probability)
categories:
  - Mathematic
  - Probability
keywords:
  - Conditional Probability
  - 条件概率
  - Multiplication Rule
  - 乘法原理
  - Partitions
  - Law of total Probability
  - 全概率公式
toc: true
date: 2018-01-31 10:34:36
---

**Abstract:** 本文介绍条件概率的定义及相关知识，提出全概率公式
**Keywords:** Conditional Probability，Multiplication Rule，Partitions
，Law of total Probability
<!--more-->
## 开篇废话

关于学习看过不同人的很多说法，大部分来自“知乎（分享你新编的故事）”，虽然有些不太可信，但是有一些确实有点道理，比如有人问是否应该着重训练微积分的计算能力，其实这个问题我个人也有过，原因就是经过高考的人有点不太能把握“什么程度才能叫做一项知识学会了”，因为就算你对一个知识点非常透彻，但是还是有人能通过这个知识点创造出你完全不会解的题，于是我们就开始怀疑自己到底到底学会没学会，那微积分有必要反复做题么？有一个答案的大概意思是，反复训练微积分，就像反复训练加减法一样，过了小学，这个训练就没有必要进行了，这个似乎很有道理。
那么我们准备做机器学习的同学们有必要反复训练自己的基础算法么？比如线性回归啊，感知机啊什么的么？开始的时候当然很有必要，但是当你进入一定阶段了，就要研究其背后的分析方法的原理了，所以，会用算法，理解算法，分析算法是完全不同的层次，就像小学，大学还有博士阶段一样。所以，打好基础，学好数学，才能读博士.当然现在如火如荼的环境下，小学生就业也非常乐观。
今天继续概率论的讨论，本来想把事件的独立也添加到本文，但是后来分析了一下，内容太多容易乱，所以本文只介绍条件概率，还是那句话，好就不能快，多就不能省，虽然感觉时间紧迫，但是也要一步一步来。

## The Definition of Conditional Probability(条件概率的定义)
概率本身用途有限，为什么？首先从我们的知识图上可以看到微积分，线性代数，概率论本身处于数学基础，基础很少被直接应用于平时的问题中，而概率更是如此，除了算算彩票，骰子，基本没有什么场景能完全满足概率的“条件”，但是作为基础，统计和随机过程则是非常实际的应用方法，概率对于统计的一个作用就是：当知道某个特定的事件被观察到的时候，通过概率论的知识，可以更新某些事件的概率。
比如我们观察一个试验，事件A是我们关心的，试验结束后，我们得到的关于A是否发生的信息是事件B发生了，那么此时事件A的概率被称为条件B发生时A的条件概率。
>Definition Conditional Probability: Sippose that we learn that an event B has occurred and that we wish to compute the probability of another event A taking into account that probability of the event A given that the event B has occurred and is denoted $Pr(A|B)$ . If $Pr(B)>0$ ,we compute this probability as:
$$
Pr(A|B)=\frac{Pr(A\cap B)}{Pr(B)}
$$
ps:The conditional probability $Pr(A|B)$ is not defined if $Pr(B)=0$

翻译一下，当我们知道事件B已经发生并且我们希望计算另外一个事件A的时候考虑当B发生时A的概率，那就是条件概率 $Pr(A|B)$ 。当$Pr(B)=0$ 时，条件概率无定义

但是我们应该注意到，所有事件的概率都是条件的，比如试验：扔一个均匀的硬币，事件A表示正面，那么概率$Pr(A)$ 的条件就是"扔一个均匀硬币"，如果把扔一个均匀的硬币这个事件看做事件B，那么出现正面的概率是$Pr(A|B)$ ，这里事件B是肯定发生的，所以作为条件给出，如果事件B不是肯定发生，那么就要用我们下面给出的计算法则计算正面出现的概率了。
以上是从[Subjective Interpretation](http://face2ai.com/Math-Probability-1-0-Introduction/)的角度定义的，但是从[Frequency Interpretation](http://face2ai.com/Math-Probability-1-0-Introduction/)的角度，我们可以这么理解，一个重复$N$ 次的试验事件B发生的$N_B$次数与试验次数的比例近似于$Pr(B)=\frac{N_B}{N}$ 事件A和事件B同时发生的次数$N_{A\cap B}$ 与试验次数$N$ 的比例近似于$Pr(A\cap B)=\frac{N_{A\cap B}}{N}$，所以当B事件发生时，A事件也发生的概率接近于比例：$\frac{N_{A\cap B}}{N_B}=\frac{\frac{N_{A\cap B}}{N}}{\frac{N_B}{N}}=\frac{Pr(A\cap B)}{Pr(B)}$

通过上面的几个表达式可以了解关于频率观点下的条件概率的计算方法。

1. 举个例子：
条件描述：扔两个六面体骰子，每个面出现概率相等，两个骰子互不影响。
事件的概率：那么当我们知道其结果的和是奇数的条件下，其和小于8的事件T的概率是多少？
分析：首先我们通过条件知道这两个骰子出现每个数字概率相等为 $\frac{1}{6}$ 那么就可以分析出所有结果了：


| event |  Probability   |
|:-----:|:--------------:|
|   2   | $\frac{1}{36}$ |
|   3   | $\frac{2}{36}$ |
|   4   | $\frac{3}{36}$ |
|   5   | $\frac{4}{36}$ |
|   6   | $\frac{5}{36}$ |
|   7   | $\frac{6}{36}$ |
|   8   | $\frac{5}{36}$ |
|   9   | $\frac{4}{36}$ |
|  10   | $\frac{3}{36}$ |
|  11   | $\frac{2}{36}$ |
|  12   | $\frac{1}{36}$ |


那么：
$$
Pr(A\cap B)=\frac{2}{36}+\frac{4}{36}+\frac{6}{36}=\frac{12}{36}=\frac{1}{3}\\
Pr(B)=\frac{2}{36}+\frac{4}{36}+\frac{6}{36}+\frac{4}{36}+\frac{2}{36}=\frac{1}{2}
$$
Hence:
$$
Pr(A|B)=\frac{Pr(A\cap B)}{Pr(B)}=\frac{2}{3}
$$
2. 再举个例子，两个箱子，装着不同的螺丝，箱子A装着长螺丝7个和短螺丝3个，B装长螺丝6个短螺丝4个，这两个箱子被随机分给我们，如果我们有 $\frac{1}{3}$ 的概率被分到箱子A，$\frac{2}{3}$ 的概率被分到箱子B，那么当我们已知被分到A箱子的时候，我们拿出一个长螺丝的概率是多少？

这道题看起来很复杂，当然求问题的解很简单，$\frac{Pr(N_L\cap A)}{Pr(A)}=\frac{7}{7+3}$ ，但是这道明显是个分步的试验，首先得到什么样的箱子是个未知的随机事件，然后从箱子里拿螺丝还是随机事件，但是当我们已知是哪个箱子了以后，拿螺丝的过程将会被很容易的描述


在进入下一部分之前我想简单分析下什么时候可能条件概率，第一种就是一个试验，结果被划分成不同的事件，试验结束后，可以观察到某个事件B发生了，这是我们会更新我们在试验前计算的事件A的概率，称之为条件概率$Pr(A|B)$ ；第二种是分步的试验，第二步的试验产生的事件A可能根据第一步的不同而不同，所以当第一步产生了事件B，那么事件A的概率将会被重新计算为$Pr(A|B)$ 其值等于 $\frac{Pr(A\cap B)}{Pr(B)}$ 的结果。
同时我们观察这个关系式可以得出$Pr(B)\leq 1$所以$Pr(A|B)\leq Pr(A\cap B)$，这个关系式是确定的，但是$Pr(A|B)\leq \geq Pr(A)$ 之间的大小不确定。
## The Multiplication Rule(乘法原则)
乘法原则在之前的计数方法里有提到过，就是分步试验每步有不同的结果，那么组合起来其满足乘法关系，这里的乘法法则是通过上面条件概率的定义得出的：
>Definition Multiplication Rule for Conditional Probability:
$$
if \quad Pr(B)>0:\quad Pr(A\cap B)=Pr(B)Pr(A|B)\\
if \quad Pr(A)>0:\quad Pr(A\cap B)=Pr(A)Pr(B|A)
$$


这两个式子是上面条件概率公式的变形，但是却有了不同含义，我们把它定义为条件概率的乘法原则

>Definition Multiplication Rule for Conditional Probability:Suppose that $A_1,A_2,A_3\dots A_n$ are  events such that $Pr(A_1\cap A_2\cap A_3\dots \cap A_{n-1})>0$ then
$$
Pr(A_1\cap A_2\cap A_3\dots \cap A_n)=\\
Pr(A_1)Pr(A_2|A_1)Pr(A_3|A_1\cap A_2)\dots Pr(A_n|A_1\cap A_2 \cap A_3 \cap \dots \cap A_{n-1})
$$

这个公式的证明很容易，把等号右边的式子前两个结合，得到一个事件并的事件，把它设为一个新事件$C_i$并进行替换和迭代，就能根据上面的$Pr(A\cap C_i)=Pr(C_i)Pr(A|C_i)$ 把全部整合，最后得到等号左边的结果。

举个例子来使用上面的公式，如果一个盒子里有r个红色球，b个蓝色球，其中r和b均大于2，那么我们每次随机取出一个球，without replacement，那么我们取出4个球，其排列是"红，蓝，红，蓝"的概率是多少呢？
分析，首先取出球的概率是会相互影响的，因为是without replacement，除了第一个，后面的球的概率都会因为上一次的取出而改变，所以我们假设取出序列为"红，蓝，红，蓝"的事件为$R_1\cap B_2\cap R_3\cap B_4$
那么
$$
Pr(R_1\cap B_2\cap R_3\cap B_4)=Pr(R_1)Pr(B_2|R_1)Pr(R_3|R_1\cap B_2)Pr(B_4|R_1\cap B_2\cap R_3)\\
=\frac{r}{r+b}\frac{b}{r+b-1}\frac{r-1}{r+b-2}\frac{b-1}{r+b-3}
$$
这里主要的一个关键点就是分步试验的后面步骤受到前面步骤的影响，所以最后的结果是用条件概率给出的乘法关系。
条件概率的性质和普通概率的性质一样，因为我们开篇的时候说过所有的概率都是条件概率，只不过有些条件是规定的必然出现的，我们就把这个条件省略掉当成已知试验条件，不用考虑进行计算。
为了验证上面这句话，我们给出下面这个定理:

> Suppose that $A_1,A_2,A_3\dots A_n,B$ are events such that $Pr(B)>0$ and $Pr(A_1\cap A_2\cap A_3 \dots A_{n-1}|B)>0$ then:
$$
Pr(A_1\cap A_2\cap \dots A_n|B)=\\
Pr(A_1|B)Pr(A_2|A_1\cap B)Pr(A_3|A_2\cap A_1\cap B)\dots Pr(A_n|A_{n-1} \cap \dots \cap A_2\cap A_1\cap B)
$$

上面这个过程的证明和上面乘法原理的证明一样，就是通过等号右边每两个结合运用乘法原理，能够得到和等号左边一样的结果。
我们只要掌握一个规律就可以，那就是，条件概率和普通的概率一样，加上某个条件时，其计算方法和不加这个条件时候计算方法一致。
## Conditional Probability and Partition - Law of total Probability (条件概率与分割，全概率公式)
在[1-1的T3](http://face2ai.com/Math-Probability-1-1-Definition-of-Probability/)中，我们介绍了当一个样本空间被划分成两部分的时候，概率的计算方法，那么如果我们把切分继续下去，也就是一个样本空间我们把它切成k块不相交的子空间时，那么响应的计算会有什么变换呢？

>Definition partition Let S denote the sample space of some experiment,and consider k events $B_1 \dots B_k$ in S such that $B_1 \dots B_k$ are disjoint and $\bigcup^k_{i=1}B_i=S$ It is said that these events from a partition of S

翻译下，意思是说把样本空间打碎成k个disjointed的事件，这些事件可以组合成S，那么打碎的过程叫做partition

一般来说，当一个碎片发生时，整个试验的不确定性将会降低，因为其结果空间变得小了，但并不意味着这个碎片上含有的事件的概率会升高。

根据上面打碎原理，我们可以得出下面的全概率公式，
>Theorem Law of total probability:Suppose that the events $B_1 \dots B_k$  from a partition of the space S and $Pr(B_j)>0$ for $j=1,\dots ,k$ Then ,for every event A in S:
$$
Pr(A)=\sum^k_{j=1}Pr(B_j)Pr(A|B_j)
$$

上述公式为全概率公式，将一个样本空间分割成k个不相交的小空间，然后每个空间上有事件A的一部分在整个空间上的概率为$Pr(A\cap B_j)=Pr(A|B_j)Pr(B_j)$ 把他们都加起来就是完整的事件A的概率了。
全概率公式可以通过乘法原理和partition的定义获得，当然也可以画图证明，通过集合论的知识也可以。

①画图：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-1-5-Conditional-Probability/law_of_total_probability.png)
圆圈是内是A，各分块内都有A的一部分，$B_i\cap A$，那么所有的部分加起来就是完整的A，通过下面集合论也可以完整的解释

②集合论：
$$
A=(B_1\cap A)\cup(B_1\cap A)\cup\dots \cup(B_k\cap A)
$$
并且 $(B_j\cap A)$ 之间是disjointed ，所以
$$
Pr(A)=\sum^k_{j=1}Pr(B_j\cap A)\\
if \quad Pr(B_j)>0 (j=1\dots k)\quad then \quad  Pr(B_j\cap A)=Pr(B_j)Pr(A|B_j)
$$
就可以得到上述的全概率公式了。

同样全概率公式也有条件版本：
$$
Pr(A|C)=\sum^k_{j=1}Pr(B_j|C)Pr(A|B_j\cap C)
$$
通过画图可以简单的了解一下最简单的情况：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-1-5-Conditional-Probability/law_of_total_probability2.png)

怎么样很机制吧，给完整的样本空间S再加个套，这样$Pr(S)\neq 1$ 而是一个小于1的概率了，这种情况下产生了一个条件概率版本的全概率公式。
如果从分析的方法看：
$$
A\cap C=(B_1\cap A \cap C)\cup(B_1\cap A \cap C)\cup\dots \cup(B_k\cap A \cap C)
$$
并且 $(B_j\cap A|C)$ 之间是disjointed ，所以
$$
Pr(A| C)=\sum^k_{j=1}Pr(B_j\cap A | C)=\sum^k_{j=1}\frac{Pr(B_j\cap A \cap C)}{Pr(C)}\\
if \quad Pr(B_j)>0 (j=1\dots k)\quad then \quad  Pr(B_j\cap A|C)=Pr(B_j)Pr(A|B_j\cap C)
$$
证明过程和上述完全一致，这里就不再描述了。

## Augmented Experiment
>Definition Augmented Experiment: If desired,any experiment can be augmented to include the potential or hypothetical observation of as much additional information as we would find useful to help us calculate any probabilities that we desire

上面这个定义是告诉我们所有的试验如果需要都能通过潜在或者假想的条件将其变成条件概率的形式，如果有利于计算，我们可以这样进行扩展。

## Conclusion(总结)
果然一个条件概率就写了一天，如果加上独立事件肯定没办法写好，明天继续。。
