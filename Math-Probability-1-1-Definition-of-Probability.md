---
title: 【概率论】1-1:概率定义(Definition of Probability)
categories:
  - Mathematic
  - Probability
keywords:
  - Sample Space
  - 样本空间
  - Finite Sample Space
  - 有限样本空间
  - Kolmogorov axioms(Probability Axioms)
  - 柯氏公理
  - Disjointed Events
  - 不想交事件
  - Definition of Probability
  - 概率定义
  - Properties of Probability
  - 概率性质
  - Bonferroni Inequality
toc: true
date: 2018-01-24 10:12:00
---
**Abstract:** 本文介绍样本空间，公理化的概率的定义，以及概率的性质
**Keywords:** Sample Space，Finite Sample Space，Kolmogorov axioms(Probability Axioms)，Definition of Probability，Properties of Probability，Bonferroni Inequality

<!--more-->
# 概率定义
**开篇提示:基本定理的证明都要用到集合论的知识，所以前面的集合论博客一定要先看哦！**


做基础难的另一个原因是看不到结果，研究算法也好做应用也好，起码能写个程序观察下结果，虽然不知道为啥有结果但是能看着点东西总觉得自己在进步，但是天天做数学题真是看不出啥进展，没有感官上的刺激容易让人失去动力。

## 样本空间  Sample Space
继续上文中的讨论，我们在上一篇文章中说到了试验的outcome，并且对其进行约定必须是完全已知的，并把它当做集合来看，于是我们引入一个新的名词来命名这个包含所有结果的集合--样本空间(Sample Space)

> Definition: The collection of all possible outcomes of an experiment is called the sample space of experiment

有一个神奇的事情就是陈希孺老师的《概率论与数理统计》中并没有在概率论部分提出样本空间这个概念，而是在数理统计部分提出的样本的概念，不知道老师为何如此安排，但别的入门书籍都是在前面就给出样本空间的定义，所以我们可以先接受这个概念。
举个🌰 :
掷一个六面的骰子，可以预期的结果无非就是123456点。
$$
\{1,2,3,4,5,6\}
$$
那么这个描述中，试验就是“掷一个六面的骰子”，试验可能的outcome的集合就是上面的集合，所以我们说这个试验的样本空间就是上面描述的集合。

### 样本点 Point
试验的每一个outcome可以成为一个样本点(Point或者Element),所以事件event还可以看成是样本点的集合（之前一篇说的是样本空间的子集），这里概念是一致的，全部的样本点组成了样本空间，部分样本点组成了事件，和容易判定的一个关系。

### 有限样本空间 Finite Sample Space
有限的样本空间，首先我们应该去看一下集合论的博客，在那里面我们介绍了集合部分的内容，而且在后面我会把《陶哲轩实分析》中集合的提出和证明加入进去，里面有有关有限集合和无限集合的讨论。

试验可能出现无限结果的可能，比如某位置x测温度的试验，其可以描述成一个函数，
$$
T=f(x)
$$
这明显是一个把位置映射到实数的函数，那么结果T就是个连续（实际上不可能连续，因为测试工具不可能精确到趋近于0，这句话如果不太理解没关系，这涉及到分析学中的实数连续性的东西），说白话，理想情况下（温度计没误差，可以精确到无限位）就是试验结果有无穷多，那么这个结果集合是无限的。

当我们把样本空间看做集合，首先我们肯定不研究上面这个连续的例子，这个太复杂了，我们还是来点简单的，自然从无限多个结果的试验转换到有限个实验结果的实验上，比如扔骰子。
只要是扔有限个正常骰子，其结果都是有限的，比如扔一个，其结果是：
$$
\{1,2,3,4,5,6\}
$$
扔两个：
$$
X_1=\{1,2,3,4,5,6\}\\
X_2=\{1,2,3,4,5,6\}\\
then:\\
Y_2=\{(x_1,x_2)|x_1\in X_1 ,x_2 \in X_2\}
or\\
Y_2=X_1\times X_2
$$
最后这个表达式 $X_1\times X_2$ 表示为笛卡尔积，有限集合的笛卡尔积是有限的，所以，扔有限个骰子的结果是完全确定的，也就是这类试验是Finite Sample Space的。

## 概率是什么 What is the Probability ?
概率在上一篇中我们更多的用可能性来替代，事件(event)有可能性，那么我们进一步,每个事件具有概率。
下面我们通过Kolmogorov Axiom 柯氏公理来定义概率，需要解释的是，公理不同于定理，公理是不证自明的，也就是说公理不需要证明，他可以明确的告诉你，我就是对的，公理也是近代数学的基础，数学分析主要研究这套理论，（广告：后面会写数学分析相关的博客）。

### 柯氏公理 1 Kolmogorov Axiom 1
对于任意事件A：
$$
Pr(A) \geq 0
$$
公理1：任何事件的概率都是非负的

### 柯氏公理 2 Kolmogorov Axiom 2
公理2，如果对于某实验X的事件S，必然发生，我们说S的概率：
$$
Pr(S)=1
$$
必然要发生的事件的概率是1
比如试验：我们有3个红色的球，我们随意选一个出来，选出是红球的概率，那么事件“选出红球”必然发生，那么他的概率就是1.

### 不相交事件 Disjointed Events
已经两条公理了，再有一条就大功告成了，但是在这之前必须插播一条关于不相交事件的说明，我们前面反复说事件就是样本点的集合，那么就涉及到集合相交的问题，如果两个事件包含多于一个相同的样本点，那么他们相交，否则不相交。

如果两个不相交的事件A，B对应的概率是$Pr(A)$ 、$Pr(B)$，那么 $\{A发生 or B发生\}$ 这个事件C的概率，我们可以很自然的认为是A的概率加上B的概率，即 $Pr(C)=Pr(A)+Pr(B)$

上面这条假设可谓是基石一样存在，进一步扩展就是变成多个不向交的事件，无限多个不相交的事件，同样假设成立。
于是根据这个假设，可以提出第三个公理

### 柯氏公理 3 Kolmogorov Axiom 3
对于无限不向交事件序列$A_1,A_2,A_3,\dots$ 那么：
$$
Pr(\bigcup^\infty_{i=1}A_i)=\sum^\infty_{i=1}Pr(A_i)
$$

公理3相对复杂一点点，我们来举个🌰说明下，概率论的逻辑性没有数分和线性代数那么强，但是例子性非常强，多举例子才能更容易理解：
扔骰子的例子，我们扔一个均匀的标准的，六面体骰子，我们定义事件A是得到点{1，2}，定义事件B是得到点{3，4}，定义事件C是得到点数{5}，定义事件D是得到{6}，可见这四个事件是完全不向交的，于是那么我们可以定义一个并集，事件$S=A\cup B \cup C$ 那么我们可以计算出$Pr(S)=Pr(A)+Pr(B)+Pr(C)$

### 概率的定义 Definition of Probability
> Definition: A probability measure ,or simply a probability,on a sample space S is a specification numbers Pr(A) for event A that satisfy of Axioms 1,2 and 3
定义，概率描述，或者概率，在一个样本空间S上，对于事件A，是一个特别的数字Pr(A)，其满足三条公理。
这句话有点拗口，但是我们可以利用中文来分析下句子成分，主语"概率描述或者概率"，谓语“是”，宾语“数字”，宾语的定语“对于事件A”，“满足三条公理”，状语“在样本空间上”，那么这个套路就很清晰了：
```
时间：不详
地点：样本空间上
人物：概率
事件：对于某个事件进行可能性评估
经过：如果满足三条公理
结果：可以得到完备的概率定义
```
## 概率的性质 Properties of Probability

根据三条公理可以引申出不少性质，有点像线性代数中行列式的提出，概率的提出也是通过先定义性质，再引出实体（公理也是性质）,然后再得出其他性质，下面我们介绍一些列Theorem：
### T1: $Pr(\emptyset) = 0$
直观的来看这条定理，空集对应的事件N中包含0个样本点，所以不会发生这样的事件，所以可能性是不可能，是0，但是我们要用公理来证明定理，这样才能体现数学的公理化，这个定理的证明比较简单，可以用两种方法证明，第二种是DeGroot给出的证明，第一种是Tony的证法：

方法1（<font color=#ff0000 >此方法有错误！你能找到哪里出了问题么？</font>）：

①设事件A对应空集合，根据公理1，设其概率是
$$
A=\emptyset\\
Pr(A)\geq0
$$

②我们再设一个事件S，其包含全部样本点，那么这个事件就变成了必然事件，根据公理2，其概率
$$Pr(S)=1$$

③且根据集合论
$$S \cap A=\emptyset$$
所以S和A是不相关的

④根据集合论
$$S \cup A=S$$

<font color=#ff0000 >⑤根据公理下面即将要证明的T2可以得到</font>：
$$
Pr(S)=Pr(S\cup A)=Pr(S)+Pr(A)=1
$$
又因为②，可得到
$$
Pr(A)=0
$$
Q.E.D

<font color=#ff0000 >没错⑤存在严重的逻辑问题，因为T2中用到了T1的结论，所以产生了相互证明的逻辑问题！注意，这种问题经常发生！</font>：


-------
方法2：
设一个无限序列$A_0,A_1,A_2,\dots$ 并且对于任意$A_i=\emptyset$,那么根据概率论可知他们都是不相交的 $\emptyset \cap \emptyset=\emptyset$ ，根据公理3:
$$
Pr(\emptyset)=Pr(\bigcup^\infty_{i=0}A_i)=\sum^\infty_{i=0}Pr(A_i)=\sum^\infty_{i=0}Pr(\emptyset)
$$
所以可以得到
$$
Pr(A)=0
$$
Q.E.D

### T2: $Pr(\bigcup^n_{i=1}A_i)=\sum^n_{i=1}Pr(A_i)$
T2是对公理3的一个退化版本，也可以叫加法原理，无限个不相交事件退化成有限个
证明:
我们可以假设当$m>n$ 时，$A_m=\emptyset$ 所以：
$$
Pr(\bigcup^n_{i=1}A_i)=Pr(\bigcup^\infty_{i=1}A_i)\\
=Pr(\bigcup^n_{i=1}A_i\cup\bigcup^\infty_{i=n+1}A_i)\\
=Pr(\bigcup^n_{i=1}A_i)+Pr(\bigcup^\infty_{i=n+1}A_i)\\
=Pr(\bigcup^n_{i=1}A_i)+0\\
=\sum^n_{i=1}(Pr(A_i))
$$
Q.E.D

### T3: $Pr(A^c)=1-Pr(A)$
接下来就是更进一步的推到了，一般理论的发展就是，现有少量的公理，然后推出比较基础的使用广泛的性质，然后进一步推出更特殊的更专业的性质。

证明：
假设样本空间全集为S，根据公理2， $Pr(S)=1$ 那么
$$
A^c\cap A=\emptyset\\
A^c\cup A=S\\
Pr(A\cup A^c)=Pr(A)+Pr(A^c)=Pr(S)=1\\
Pr(A)=1-Pr(A^c)\\
$$
Q.E.D

### T4: If $A\subset B$ then $Pr(A)\leq Pr(B)$
基本定理的证明都要用到集合论的知识，所以前面的集合论博客一定要先看哦！

①$A^c\cap B\neq \emptyset ,so,Pr(A^c\cap B)>0$

②$B=A\cup(A^c\cap B)$

③$A\cap(A^c\cap B)=\emptyset$

④$Pr(B)=Pr(A)+Pr(A^c\cap B)$

⑤$Pr(B)>Pr(A)$
Q.E.D

### T5: $0\leq Pr(A)\leq 1$

对于全集S，$A\subset S$ 且 $Pr(S)=1$ 所以根据T5，$Pr(A)\leq Pr(S)$
再结合公理1，就可得到T5的结论
Q.E.D

### T6: $Pr(A\cap B^c)=Pr(A)-Pr(A\cap B)$
根据T2

① $(A\cap B)\cap(A\cap B^c)=\emptyset$

② $(A\cap B)\cup(A\cap B^c)=A$

③ $Pr(A)=Pr(A\cap B)+Pr(A\cap B^c)$

得出：
$Pr(A\cap B^c)=Pr(A)-Pr(A\cap B)$

Q.E.D

### T7: $Pr(A\cup B)=Pr(A)+Pr(B)-Pr(A\cap B)$
根据集合论中的结论：

① $A\cup B=B\cup(A\cap B^c)$

② $B\cap(A\cap B^c)=\emptyset$

所以
③ $Pr(A\cup B)=Pr(B)+Pr(A\cap B^c)$

根据T6:
④ $Pr(A\cup B)=Pr(A)+Pr(B)-Pr(A\cap B)$
Q.E.D
### T8: Bonferroni Inequality
对于所有的事件 $A_1,A_2,\dots,A_n$
$$
Pr(\bigcup^n_{i=1}A_i)\leq \sum^n_{i=1}Pr(A_i) \\
Pr(\bigcap^n_{i=1}A_i)\geq 1-\sum^n_{i=1}Pr(A^c_i)
$$
书中并没有给出Bonferroni不等式的证明，但是感觉证明也并不难，

第一个不等式是说，当存在事件相关的时候，其和的概率会比其相加的小，T7给出了证明

？第二个不等式
$$
1=\sum^n_{i=1}Pr(A^c_i)\\
Pr(\bigcap^n_{i=1}A_i)+\sum^n_{i=1}Pr(A^c_i)\geq 1
$$
第二个不等式可能有点问题，因为题设并没说明白A是否能够构成样本空间的全集，或者我可能理解有问题，这个先画个问好吧。
### $Pr(a)=0$
某个事件为概率为0并不意味着这件事永远不会发生，比如在连续的情况下，每个点的概率都是0，只有面积才有意义（这个后面会更详细的叙述）
## 总结
这篇入门知识总结相当全面，而且是从分析的角度进行入门，数学的美感完全让我忘记了饥饿，我的湿炒牛河都变成干炒了，明天继续。。





原文地址1：[https://www.face2ai.com/Math-Probability-1-1-Definition-of-Probability](https://www.face2ai.com/Math-Probability-1-1-Definition-of-Probability)转载请标明出处
