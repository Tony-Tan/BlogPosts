---
title: 【概率论】1-2:计数方法(Counting Methods)
categories:
  - Mathematic
  - Probability
keywords:
  - Counting Methods
  - 技术方法
  - Combinatorial Methods
  - 组合方法
  - Multiplication
  - 乘法法则
  - Permutations
  - 排列
  - Stirling's Formula
  - 斯特林公式
toc: true
date: 2018-01-25 10:35:46
---

**Abstract:** 本文主要介绍有限样本空间下的古典概率问题，以及其中包含的计数方法，排列的基本思想
**Keywords:** Counting Methods，Combinatorial Methods，Multiplication，Permutations，Stirling's Formula
<!--more-->
## 开篇废话
其实有时候天天写博客没有什么废话可以说了，因为前一天说的已经差不多了，但是废话这个传统不能断，因为一旦断了，就没办法接上了。
最近这几篇博客感觉每一篇知识点都有点多，概率这个东西更讲究应用，所以决定本文分成两篇写，这两篇只讲原理下后面讲案例，这样对比这看可能更好，一般的教材的通用做法是讲一个知识点，然后给出丰富的例子让我们来理解知识点，但是这样知识点之间衔接就被例题打断了，而先讲理论再来例题，理论又容易记不住，所以这是个两难的选择，但是我还是更倾向于先把理论用通俗的话讲明白，然后再举例子，这样我会感觉到踏实一点，也能检验自己理论是否真的搞明白了。
另外我对碎片化的学习表示非常反对，所谓任务导向的学习，什么不会学什么，这样做的好处是多快好省，这是我们dang经济建设的口号，“这个口号本身很矛盾，多就不能快，好就不能省，一分钟洗一万个土豆，你敢吃么？给我一千块钱盖房子，我只能用纸壳报纸给你盖”（语出自袁腾飞老师）,多快好省的问题就在于许多问题不会很快暴露，但是会陆陆续续的一直干扰你。
本文中的部分内容高中就有涉及，所以这篇博客写的关键字有点多，这里只强调与后续概率相关的知识，如果有遗漏大家可以自己补习。
## Finite Sample Space(有限样本空间)
我们书接上回，上回书我们说到试验的所有输出组成的集合，我们称之为样本空间，当样本空间中的样本点的个数是有限的时候，我们就称之为有限样本空间，那么我们的问题来了，如果我们已知试验的条件，怎么确定结果是否有限个，如果是有限个，那么怎么确定个数呢？也就是怎么数一下结果呢？于是就有了下面的一些列计数方法。
### Classical Interpretation
我们在第一篇introduction里面提到过Classical Interpretation，古典观点的概率，本篇中我们主要的出发点与古典概率一致，古典概率假设：“试验中，每一个样本点的概率相等”，那么从这个出发点看，结合上文中概率的[性质T3](http://tony4ai.com/Math-Probability-1-1-Definition-of-Probability/),假设样本空间有n个样本点，把每个样本点看做一个事件，这样n个事件都互不相关，n个事件的概率和是1，那么每个事件的概率是 $\frac{1}{n}$ 这个概念说起来可能比较拗口，各种条件之间看起来可能比较复杂，但是举个例子就非常简单了，扔骰子：
扔一个均匀的六面体骰子，那么样本空间是 $\{1,2,3,4,5,6\}$ 所有点数出现的概率相等，那么$Pr(x=1)=\frac{1}{6}$ 、 $Pr(x=2)=\frac{1}{6}$ 、$Pr(x=3)=\frac{1}{6}$ 、$Pr(x=4)=\frac{1}{6}$ 、 $Pr(x=5)=\frac{1}{6}$ 、$Pr(x=6)=\frac{1}{6}$
这个结论比较能够满足我们的直观感觉，条件中“均匀的骰子”就表明样本空间中每个样本点等可能出现。
现在我们已经基本理解了古典概率的基本思路了，但是思考下，我们扔的一个骰子，能够很轻松的确定结果集的所有元素，那么如果我们扔两个骰子，或者扔更多的骰子呢？数数可能有点困难了，所以我们引出了下面的课题，计数方法。
## Counting Methods(计数方法)
计数方法的根本目的就是为了计算试验结果的个数，与试验出现的结果无关，也不影响任何试验的随机性，计数只是通过条件给出的前提，在试验前或者试验后，计数结果不变。
总结下就是一句话：计数不影响试验的结果，计数方法在概率中的最主要的应用就是为了计算试验结果的个数。
### Multiplication Rule(乘法原理)
乘法原理是最基础的一种计算结果的方法，首先我们感受下实际的用途，先后扔两个骰子，将结果排列成一个有序的序列，比如第一个骰子是6，第二个骰子是4，那么结果我们记做 $(6,4)$ 那么所有可能出现的结果：
$$
\{(1,1),(1,2),(1,3),(1,4),(1,5),(1,6),\\
(2,1),(2,2),(2,3),(2,4),(2,5),(2,6),\\
(3,1),(3,2),(3,3),(3,4),(3,5),(3,6),\\
(4,1),(4,2),(4,3),(4,4),(4,5),(4,6),\\
(5,1),(5,2),(5,3),(5,4),(5,5),(5,6),\\
(6,1),(6,2),(6,3),(6,4),(6,5),(6,6)\}
$$
一共36种结果，36，很神奇的数字，因为每个骰子有6种结果，两个骰子，难道是$6^2$ 的关系？我们先不给出结论，我们来观察试验过程，首先我们扔第一个骰子，可能出现以下结果：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-1-2-Counting-Methods\first_dic.png)

下面我们扔第二个骰子：

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-1-2-Counting-Methods\two_dic.png)

看出来了吧，第一个骰子能扔出来6种结果，每一种结果后第二个骰子还能扔出6种结果，那么两个骰子的结果总数就是 $6\times 6$ 种结果。
同样扔两个硬币就会得到$2\times 2$ 种结果，分别是：
$$
{(HT),(HH),(TH),(TT)}
$$
>Definition: Multiplication Rule for Two-Part Experiments,
(i)The Experiments is performed two parts
(ii)The first part of the experiment has $m$ possible outcomes $x_1,x_2,\dots,x_m$  ,and regardless of which one of these outcomes $x_i$ occurs,the second part of the experiment has $n$ possible outcomes $y_1,y_2\dots,y_n$
the sample space $S$ contains excatly $mn$ outcomes

对于可以分成两步的试验，如果第一步试验有$m$ 种结果，第二步试验有 $n$ 种结果，那么整个试验共有$mn$ 种结果。

接下来的扩展把上述两步试验扩展成更一般的试验，也就是多步试验，那么整体试验的结果是每一步结果数量的连续乘积。
>Definition:Multiplication Rule.Suppose that an experiment has k parts($k\geq2$),that the $i$th part of the experiment can have $n_i$ possible outcomes($i=1,\dots,k$),and that all of the outcomes in each part can occur regardless of which specific outcomes have occurred in the other parts.Then the sample space $S$ of the experiment will contain all vectors of the form $(u_1,\dots,u_k)$ where $u_i$ is one of the $n_i$ possible outcomes of part $i(i=1,\dots,k)$ .The total number of these vectors in $S$ will be equal to the product $n_1n_2\dots n_k$

上面这段英文定义就是乘法原理的多阶段版本，也是通用版本，这里值得指出的就是重点的一句，**各个阶段的试验互不影响**，这个是非常关键的前提，如果不满足这个条件，乘法原理不成立。
### Permutations(排列)
接下来就是排列了，排列怎么来的？从乘法原理来的呀，我们在看下面三种情况，陈希孺老师称下面这个叫“坛子模型”，这个模型虽然简单，但是是构成概率论的基础：

1. 一个不透明的箱子，里面三个球，除了每个球颜色不同其他都相同（假设红R绿G蓝B三种颜色），也就是说如果靠触觉没办法区分，那么我们不看从箱子里拿球出来，每个球被拿出的可能性相同，并且，我们每次拿出球以后记录了颜色以后，再放回去，然后重复上述过程，这样我们重复三次，可能的结果：
$$\{
RRR,RRG,RRB,RGR,RGG,RGB,RBR,RBG,RBB,\\
GRR,GRG,GRB,GGR,GGG,GGB,GBR,GBG,GBB,\\
BRR,BRG,BRB,BGR,BGG,BGB,BBR,BBG,BBB
\}$$
根据实验的各步骤之间互不影响，所以乘法原理成立，结果有27种($3\times3\times3$) 这个重复的过程有个关键的步骤就是，每次取出球记录颜色后再放回去，这一步很重要。

2. 如果我们取出球以后不放回去呢？那么我们可能得到的结果就是：
$$
\{
RGB,RBG,GRB,GBR,BRG,BGR
\}
$$
解释下，第一步我们有三种可能的结果，同时第一个球不会放回，第二步受到第一步的影响，只可能有两种结果，同时第二个球不会被放回，那么第三步只有最后一个球，没有选择，只能是他，那么，这个不放回的过程，共有$3\times2\times1$ 种结果，比上面的模型1，要少很多种情况，

3. 我们简化上面2中步骤，把三步减到两步，那么根据2，我们共有$3\times2$种，虽然结果都是6，但是，效果是不同的，例如如果我们有四个球，不放回的取两个 $4\times3$ 共12种情况，如果是全取出 $4\times3\times2\times1$ 种情况。

如此便可引出组合的概念
> Definition: Permutations. Suppose that a set has $n$ elements.Suppose that an experiment consisits of selecting $k$ of the elements one at a time without replacement.Let each outcome consist of the $k$ elements in the order selected.Each such outcome is called a permutation of n elements taken $k$ at a time.We denote the number of distinct such permutations by the symbol $P_{n,k}$

定义的翻译，有n个球，取k个出来，一次取一个，不放回，并且取出的顺序被严格记录，不允许顺序被打乱。这个就是排列。
>Theorem Number of Permutations. The number of permutations of $n$ elements taken $k$ at a time is $P_{n,k}=n(n-1)\dots(n-k+1)$

上述为排列的数字结果，总结出下面的表示方法，提出一个阶乘的概念,阶乘的具体概念可以自己查一查，注意，我们约定$0!=1$,注意，这是个约定。

> Theorem Permutations:The number of distinct orderings of $k$ items selected without replacement from a collectioin of $n$ different items $(0\leq k\leq n)$ is:
$$
P_{n,k}=\frac{n!}{(n-k)!}
$$

小结一下上面关于排列的知识，最主要的关键字有两个，一个是“with replacement放回”与“without replacement不放回”，另一个就是顺序，取出结果的顺序被严格记录不允许颠倒顺序。
### Birthday Problem(生日问题)
问题描述：“假设一年365天（不考虑闰年）k个人，至少有两个出生在同一天（只考虑月和日，不考虑年）的概率是多少？”
秉着越困难的题越短的思路，这个题可能要考虑不少，所以我们要仔细考虑下，
首先k一定不能大于365，不然肯定有同一天出生的人，那么概率是1没有讨论价值，那么我们可以利用前面集合的概念，假设样本空间S包含 $(365^k)$ 种出生组合（这是个with replacement）的结果，事件A是至少两个人出生在同一天，那么在全集S上的补集$A^c$ 的意义就是所有k个人都不同一天出生，那么根据排列的公式就有$P_{365,k}$ 种情况，那么根据每天等概率的前提下，$Pr(A^c)=\frac{P_{365,k}}{365^k}$ ，根据[1-1：Definition of Probability中的T3](http://tony4ai.com/Math-Probability-1-1-Definition-of-Probability/)
$$
Pr(A)=1-Pr(A^c)=1-\frac{P_{365,k}}{365^k}=1-\frac{365!}{(365-k)!365^k}
$$

我们可以带入几个k值来计算下概率：


| $k$ | $Pr(A)$ | $k$ | $Pr(A)$ |
| --- | ------- | --- | ------- |
| 5   | 0.027   | 25  | 0.569   |
| 10  | 0.117   | 30  | 0.706   |
| 15  | 0.253   | 40  | 0.891   |
| 20  | 0.411   | 50  | 0.970   |
| 22  | 0.476   | 60  | 0.994   |
| 23  | 0.507   |     |         |


可见当有60个人的时候，这些人中有至少两个同一天过生日的概率是0.994，这就是很著名的生日问题，虽然很简单，但是很有趣，当然我们也可以不用补集的方法，直接正面解决，但是可能有点困难，大家可以自己试试。

### Stirling's Formula(斯特林公式)
Stirling's Formula,斯特林公式，主要为了解决的问题是，在$P_{n,k}=\frac{n!}{(n-k)!}$ 的计算过程中，当n比较大的而k不太大的时候，这个计算变得很麻烦，因为数字太大了！什么？没有概念？那我下面写几个不太大的数字直观的给你看下：
$$
P_{10,2}=\frac{10!}{(10-2)!}=10\times9=90\\
P_{100,2}=\frac{10!}{(10-2)!}=10\times9=9,900\\
P_{1000,2}=\frac{10!}{(10-2)!}=10\times9=999,000\\
P_{10000,2}=\frac{10!}{(10-2)!}=10\times9=99,990,000
$$
这里我是口算的，所以让$k=2$ 如果让$k=5$ ，那么计算将会非常麻烦，但是我们发现一个有用的关系：
$$
\frac{n!}{a_n}=e^{ln(n!)-ln(a_n)}
$$

但是$ln(n!)$ 也太大了，没办法直接计算，于是Stirling's Formula 被提出：

> Theorem Stirling's Formula .Let:
$$
set:\;s_n=\frac{1}{2}ln(2\pi)+(n+\frac{1}{2})ln(n)-n\\
then:\;lim_{n\to \infty}|s_n-ln(n!)|=0\\
or:\;lim_{n\to \infty}\frac{(2\pi)^{1/2}n^{n+1/2}e^{-n}}{n!}=1
$$

证明过程可以被简单的写成 $ln(n!)\sim\frac{1}{2}ln(2\pi)+(n+\frac{1}{2})ln(n)-n$ 即
$e^{ln(n!)}\sim e^{\frac{1}{2}ln(2\pi)+(n+\frac{1}{2})ln(n)-n}$ 也就是证明$n!\sim (2\pi)^{1/2}n^{n+\frac{1}{2}}e^{-n}$ 成立：
证明：
略（后面补上）
## 总结
本来想多写点知识点，结果发现，多就不能快，那我们明天继续，另外stirling 公式后面再给出证明，此处先略过，待续。。
