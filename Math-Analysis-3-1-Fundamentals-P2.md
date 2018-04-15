---
title: 【陶哲轩实分析】3.1 集合基础(Part II)
categories:
    - Mathematic
    - Analysis
tags:
    - 集合基础
toc: true
date: 2018-04-13 17:44:44
---

**Abstract:** 本文介绍集合论的基础第二部分，主要介绍集合的另一部分公理和计算性质。
**Keywords:** 集合基础

<!--more-->
说明：<font face="黑体" color=#6F6FFF><B>以这种字体存在的博文，来自《陶哲轩实分析》的内容完整节选，版权归原作者和出版社所有</B></font>

## 集合基础

<font face="黑体" color=#6F6FFF><B>
很明显，某些集合似乎比其他的集合大.正式建立这个概念的一个途径是引入子集的概念.
</B></font>

---------------
前半部分中我们讲了几个公理，包括最简单的集合——空集，然后让空集扩大到单元素集合和双元素集合，就像定义了自然数1以后然后定义2，3，4，这时就该定义一个类似于自然数增长的运算——就是并集运算，当这些足够构成一个集合的时候，我们该研究研究构造出来的这些集合有没有什么性质了，和自然数一样有了加法就开始比大小，集合也来定义一下谁“大”谁“小”，当然这个没有序的关系，但是

---------------

<font face="黑体" color=#6F6FFF><B>
定义3.1.15(子集）设 $A,B$ 是集合.说 $A$ 是 $B$ 的子集，记作 $A \subseteq B$ ，当且仅当 $A$ 的每个元素都是 $B$ 的元素，即
对于任何对象 $x$ ， $x\in A \Longrightarrow x\in B$
说 $A$ 是 $B$ 的真子集，记作 $A\subsetneq B$ ,如果 $A\subseteq B$ 但 $A\neq B$.
</B></font>

---------------
这个定义里除了命名和描述以外，使用了数理逻辑中的蕴含的概念，当 $A \subseteq B$ 时 ，$x\in A$ 蕴含 $x\in B$

注意，这是一个定义，不是公理，其定义的基础是前面的公理3.1 ，3.2 以及3.3 ，所以公理是地基，更值得我们仔细研究。


---------------

<font face="黑体" color=#6F6FFF><B>
注3.1.16  由于这些定义仅用到相等的概念和“是……的一个元素”这样的关系，而这两者已然遵从代入公理，所以子集的概念也自动地遵从代入公理.譬如说，若 $A \subset B$ 且 $A= A'$ ，则 $A'\subset B$.
</B></font>

---------------
首先我们在前面已经说过，集合是满足代入公理的，而这个“属于”运算的定义只用到了“是xx的元素这类描述”，那么这就使得子集运算（关系）满足代入公理
这段有点不懂，没关系，因为我们后面有机会好好研究一个“东西”，和他们之间的关系组成的另一种有意思的“东西”

---------------

<font face="黑体" color=#6F6FFF><B>
例3.1.17 我们有 $\{1,2,3\}\subseteq \{1,2,3,4,5\}$，因为 $\{1,2,3\}$ 的每个元素也是 $\{1,2,3,4,5\}$ 的元素.事实上我们还有 $\{1,2,3\} \subsetneq \{1,2,3,4,5\}$ ，因为这两个集合不相等.给定任意一个集合，总有 $A\subseteq A$ (为什么?）以及 $\emptyset \subseteq A$ .(为什么?）
</B></font>

---------------
一个例子，用自然数的集合来演示子集和真子集。

---------------

<font face="黑体" color=#6F6FFF><B>
在集合论中的子集的概念类似于数的的“小于或等于”的概念.下面的命题演示了此事（更精确的表述见定义8.5.1).
</B></font>

---------------
当 $A\subseteq B$ 有没有感觉 $B$ 好像总比 $A$ 大，因为 $A$ 有的 $B$ 都有，而且还可能有些 $A$ 没有的。


---------------

<font face="黑体" color=#6F6FFF><B>
命题3.1.18(集合被包含关系部分地安排了次序）设 $A,B,C$ 是集合.如果 $A\subseteq B$ 且 $B\subseteq C$ ,则 $A\subseteq C$ .如果 $A\subseteq B$ 且 $B\subseteq A$ 则 $A=B$ 最后，若 $A\subsetneq B$ 且 $B\subsetneq C$ 则 $A\subsetneq C$
</B></font>

---------------
说过了，集合没有次序关系，然后这就来个类似次序的关系吧，或者说子集运算(关系)有传递性，如果一不小心搞成了一个圈 $A\subseteq B$ 且 $B\subseteq A$ 那么必然有 $A=B$ 然后真子集和自己关系有同样的操作，接着我们看其中一个证明，然后给出我们的证明。


---------------

<font face="黑体" color=#6F6FFF><B>
证明 我们只验证第一个结论.设 $A\subseteq B$ 且 $B\subseteq C$,为证 $A\subseteq C$ ,只须证明 $A$ 的每个元素都是 $C$ 的元素.于是，任取 $A$ 的一个元素 $x$ 那么从 $A\subseteq B$ 知 $x$ 必是 $B$ 的一个元素.但从 $B\subseteq C$ 也知 $x$ 是 $C$ 的一个元素.于是 $A$ 的每个元素确实都是 $C$ 的元素.这就是要证明的.
</B></font>

---------------
- 证明 $A\subseteq B$ 且 $B\subseteq A$ 那么必然有 $A=B$ :
    1. 根据子集的定义， $A\subseteq B$ 对于所有 $x\in A$ 必然有 $x\in B$
    2. 同样根据子集的定义， $B\subseteq A$ 对于所有 $x\in B$ 必然有 $x\in A$
    3. 结合[定义3.1.4](https://tony4ai.com/Math-Analysis-3-1-Fundamentals-P1/) 我们发现 1和2 就是描述集合相等的定义。
    4. 证毕
- 证明 若 $A\subsetneq B$ 且 $B\subsetneq C$ 则 $A\subsetneq C$
    1. 根据真子集的定义和传递性，我们能得到 $A\subseteq B\subseteq C$ ，且 $A\neq B,B \neq C$
    2. 我们现在要证明的是 $A\neq C$
    3. 所以下一部分我们要用反证法（也可以使用常规方法，但是反证法比较犀利）
    4. 假设 $A=C$ ，根据带入公理，那么就有 $A\subsetneq B\Rightarrow C\subsetneq B$ 根据定义3.1.15 $C\subseteq B \text{ and } C\neq B$
    5. $B\subsetneq C$ 根据定义3.1.15 $B\subseteq C \text{ and } B\neq C$ ,跟我们上面的证明 “ $A\subseteq B$ 且 $B\subseteq A$ 那么必然有 $A=B$ ” 冲突
    6. 所以 $A\neq C$
    7. 证毕


---------------

<font face="黑体" color=#6F6FFF><B>
注3.1.19	在子集和并集之间有一种联系，见习题3.1.7.
</B></font>

---------------
我们来证明 3.1.7 设是 $A,B,C$ 集合.证明 $A\cap B\subseteq A$  且 $A\cap B\subseteq B$，进而证明 $C\subseteq A$ 且 $C\subseteq B$  当且仅当 $C\subseteq A\cap B$ 用类似的精神证明 $A\subseteq A\cup B$ 且 $B\subseteq A\cup B$ 进而证明 $A\subseteq C$ 且 $B\subseteq C$ 当且仅当 $A\cup B\subseteq C$




---------------

<font face="黑体" color=#6F6FFF><B>
注3.1.20 子集关系 $\subsetneq$ 和小于关系 $<$ 之间有一个重要的区别.给定任意两个不同的自然数 $n,m$ ，我们知道它们中的一个必定小于另一个（命题2.2.13);但是，给定两个不同的集合，一般说来不必其中一个是另一个的子集.例如，取 $A:=\{2n : n\in\mathbb{N}\}$ 为偶自然数的集合，而 $B:=\{2n + 1:n\in\mathbb{N}\}$	为奇自然数的集合，则其中任何一个都不是另一个的子集.这就是为什么我们说，集合只被部分地安排了次序， 而自然数是完全地安排了次序的（见定义8.5.1和定义8.5.3).
</B></font>

---------------
上面类比子集运算和序，现在说明一下他们的不同，序是自然数字之间必然存在的关系，但是集合之间不是必须存在子集关系的。


---------------

<font face="黑体" color=#6F6FFF><B>
注3.1.21	我们还应该小心，子集关系 $\subseteq$ 与元素关系 $\in$ 可不是一回事.数 $2$ 是 $\{1,2,3\}$ 的一个元素，但却不是一个子集，于是 $2 \in \{1,2,3\}$ 但 $2 \nsubseteq \{1,2,3\}$ .的确 $2$ 根本就不是一个集合.反过来，$\{2\}$ 是 $\{1,2,3\}$ 的一个子集，而不是它的元素，于是 $\{2\}\subseteq \{1,2,3\}$ 但 $\{2\}\in\{1,2,3\}$ .问题在于数 $2$ 和集合 $\{2\}$ 是不同的对象.把集合与它们的元素区别开来是重要的，因为它们有不同的属性.例如，可以有一个由有限数组成的无限集合（自然数的集合就是这样的一个例子），同时也可以有一个有限集合，其每个元素都是“由无限个对象所组成的集合”（例如有限集 $\{\mathbb{N},\mathbb{Z},\mathbb{Q},\mathbb{R}\}$ ，它有四个元素，其中每个元素都是无限集合
</B></font>

---------------
区分集合和集合之间的关系，以及元素和集合之间的关系，也就是区分集合和元素，$\in$ 和 $\subseteq$ 之间的关系。
集合可以是元素，而集合可以是无限的（后面还有更详细的论述“无限”的含义），而四个无限的集合组成了一个有限的集合。


---------------

<font face="黑体" color=#6F6FFF><B>
我们现在给出一个公理，它使我们能够容易地构作大集合的子集合.
</B></font>

---------------
前面有了从小集合到大集合的公理，现在给一个从大集合到小集合的公理，可以类比减法，但是我们目前还没严格的学习减法，所以我们可以把它理解为一种减小的操作。

---------------

<font face="黑体" color=#6F6FFF><B>
公理3.5(分类公理）设 $A$ 是一个集合，并对于每个 $x \in A$ ，设 $P(x)$ 是一个关于 $x$ 的性质（即 $P(x)$ 或为一个真命题，或为一个假命题).那么存在一个集合叫 作 $\{x\in A:P(x)\text{成立}\}$ (或简写为 $\{x\in A:P(x)\}$ )，它的元素恰恰是 $A$ 中使 $P(x)$ 成立的 $x$ .换句话说，对于任何对象 $y$ ,
$$
y \in \{x \in A:P(x)\text{ 成立 }\} \Leftrightarrow (y\in A \text{ 且 } P(y)\text{ 成立})
$$
</B></font>

---------------
分类公理，分类——机器学习最最最最常见任务，一大堆东西分成若干类，所以原始的一大堆总是包含或等于分类后的一小堆。
而分类是要根据的，比如一个班级中我要找出所有男生，那么这个男生就是筛选条件，定理中的 $P(x)$ 注意，这个条件必须是一个明确的命题，无论真假，是一个确定的肯定句，不能出现任何模棱两可的描述。
然后注意一下冒号的用法。


---------------

<font face="黑体" color=#6F6FFF><B>
这个公理也叫作分离公理.注意，$\{x \in A : P(x)\text{成立}\}$ 永远是 $A$ 的一个子集合 (为什么?)，尽管它可以大得就是 $A$ 或小得成为空集.可以验证，代入公理适用于分类，于是，若 $A =B$  则 $\{x \in A :P(x)\} = \{x\in A': P(x)\}$ (为什么?).
</B></font>

---------------
解释下为什么，根据公理中的描述 $y\in A \text{ 且 } P(y)\text{ 成立}$ 数理逻辑中关于 “且” 的说明是前后两个命题必须同时成立，也就是 $y\in A$ 是必须的，所以根据子集的定义 $\{x \in A : P(x)\text{成立}\}\subseteq A$
分类是一种特殊的子集操作，如果分类操作不满足代入公理，那么就意味着子集不满足代入公理，这显然是不对的。
比如相等的集合 $A=B$ 根据同一个分类原则 $P$ 产生子集 $M$ 和 $N$ 那么：
证明：
1. $A\xrightarrow{P(x)} M$ 以及 $B\xrightarrow{P(x)} N$
2. $A=B$ 根据集合相等的定义，集合中所有元素都相同
3. P(x) 是一个明确的命题，所以其结果对于同一个 $x$ 必然是一致的，不可能出现两种或以上的结论
4. 所以产生的子集 $M$ 和 $N$ 拥有同样的元素。
5. 证毕

这里注意一下，$P(x)$ 的输出是符合或者不符合，而不是其他值，我们要的也是 $x$ 而不是 $P$ 中的结果，换句话说 $P$ 就是一个测试环节，而不是加工环节，我们把东西放进去测试一下，然后再原封不动的拿出来，但是其属性多了一个（“通过了 $P$ 测试”）


---------------

<font face="黑体" color=#6F6FFF><B>
例 3.1.22 设 $S:=\{1,2,3,4,5\}$ ，则集合 $\{n\in S : n < 4\}$ 是 $S$ 中的使 $n < 4$ 成立的元素 $n$ 的集合，即 $\{n\in S:n < 4\} = \{1,2,3\}$ .类似地，集合 $\{n\in S :n < 7\}$ 就是 $S$ 本身，而 $\{n\in S:n < 1\}$ 是空集.
</B></font>

---------------
一个选择公理的例子。


---------------

<font face="黑体" color=#6F6FFF><B>
我们有时写 $\{x \in A |P(x)\}$ 代替 $\{x \in A :p(x)\}$	当我们用冒号“:”来表示其他事物时，这个写法就有用了，例如当我们用冒号表示一个函数 $f : X \to Y$ 的定义域和值域时，就是这样.
</B></font>

---------------
分号有的时候有别的用途，所以换成竖线表示。
注意区分和概率的写法的区别

---------------

<font face="黑体" color=#6F6FFF><B>
我们可以使用此分类公理来定义集合的其他一些运算，即集合的交与集合的差.
</B></font>

---------------
有了分类公理又可以衍生出一些其他计算方法。


---------------

<font face="黑体" color=#6F6FFF><B>
定义3.1.23(交）两个集合 $S_1$ 和 $S_2$ 的交 $S_1\cap S_2$ 定义为集合
$$
S_1\cap S_2 :=\{x\in S_1:x\in S_2\}
$$
换句话说，$S_1\cap S_2$ 由全部同时属于 $S_1$ 和 $S_2$ 两个集合的元素构成，于是，对于一切
对象 $x$ ,
$$
x\in S_1\cap S_2\Leftrightarrow x\in S_1 \text{ 且 } x\in S_2
$$
</B></font>

---------------
关于交集的定义，注意 “且” 的用法


---------------

<font face="黑体" color=#6F6FFF><B>
注3.1.24 注意此定义是明确的（也就是说，它遵从代入公理，见§4.7)，因为它是借助更初始的运算来定义的，而这些初始的运算已经知道是遵从代入公理的. 类似的注释适用于本章后面的定义，并且通常将不再明确提及.
</B></font>

---------------
代入公理在本节反复提到，因为这是整个分析（各种分析，微积分，实分析等）中最基础的公理
初级的运算满足代入公理，那么又初级计算组合创造的计算也满足代入公理。

---------------

<font face="黑体" color=#6F6FFF><B>
例 3.1.25  $\{1,2,4\}\cap\{2,3,4\} = \{2,4\}$ ，$\{1,2\}\cap\{3,4\} = \emptyset$ ，$\{2,3\}\cup\emptyset = \{2,3\}$ 以及 $\{2,3\}\cap\emptyset =\emptyset$
</B></font>

---------------
交集的计算例子。

---------------

<font face="黑体" color=#6F6FFF><B>
注3.1.26 顺便指出，我们应该小心地使用英语单词 “and” ,不可使它含混地在上下文中既可指并，也可指交.例如，如果我们谈论“男孩和（and)女孩”我们指的是男孩的一个集合与女孩的一个集合的并，但是如果我们谈论“单身的和(and)男的”人的集合，则指的是单身者的集合与男性人的集合的交.（你能搞出一个语法规则来确定何时“and(和)”指的是并，而何时“and(和)”指的是交吗?）另一个问题是“and(和)”在英语中用来表示加法，例如人们可以说“2 and 3 is 5” 同时也说“ $\{2\}$ 的元素和 $\{3\}$ 的元素构成集合 $\{2,3\}$ ”以及“在 $\{2\}$ 和 $\{3\}$ 中的元素构成集合 $\emptyset$ ”.这肯定会引起混淆！我们求助于数学符号来代替诸如“and”之类的英语词汇的一个缘由，就是数学符号永远具有精确的清晰的含意，使用数学符号就避免了必须总得小心地根据上下文来确定一个单词到底是什么意思.
</B></font>

---------------
and的用法，要结合上下文，这个就是文字描述的注意点，而不是数学逻辑问题，如何明确的表明一个数学逻辑，有的时候需要明确的数学符号，而文字可能表达不出来，或者有歧义


---------------

<font face="黑体" color=#6F6FFF><B>
两个集合 $A,B$ 叫作是 **不交** 的，如果 $A\cap B=\emptyset$ .注意，这与相异（distinct)可不是同一个概念.例如集合 $\{1,2,3\}$ 与 $\{2,3,4\}$ 是相异的（有一个集合的元素不是另一个集合的元素)，但是它们不是不交的（因为它们的交是不空的集合).同时，集合 $\emptyset$ 与集合 $\emptyset$ 是不交的，但不是相异的.（为什么?）
</B></font>

---------------
相异，和不交是不同的概念，相异是不相等的意思，也就是两个集合中有些不同的对象。如果都相同，那么就是相等了。
证明 $\emptyset$ 与集合 $\emptyset$ 是不交的，但不是相异的：
1. 假设 $\emptyset$ 与集合 $\emptyset$ 是相异的。
2. 那么必然有 某元素 $x\in \emptyset$ 但 $x\notin \emptyset$
3. 首先2中自己就矛盾了，而且 $x\in \emptyset$ 与空集的公理矛盾，假设不成立，故 $\emptyset$ 与集合 $\emptyset$ 是相异的不成立。
4. 证毕

---------------

<font face="黑体" color=#6F6FFF><B>
定义3.1.27(差集）给定两个集合 $A$ 和我们定义集合 $A-B$ 或 $A\setminus B$ 为从集合 $A$ 中把 $B$ 中的任何元素都去掉所得的集合：
$$
A \setminus B := \{x \in A : x \notin B\}
$$
例如，$\{1,2,3,4\}\setminus\{2,4,5\} = \{1,3\}$ .在很多情况下，$B$ 是 $A$ 的子集，但并非必须如此.
</B></font>

---------------
给出了差集的定义


---------------

<font face="黑体" color=#6F6FFF><B>
我们现在给出并、交及差集的一些基本性质.
命题3.1.28(集合构成一个Boole代数）设 $A,B,C$ 是集合，并设 $X$ 是包含 $A,B,C$ 为其子集的集合.
(a) (最小元）我们有 $A\cup\emptyset = A$ 以及 $A\cap\emptyset = \emptyset$ .

(b) (最大元）我们有 $A\cup X= X$ 以及 $A\cap X =A$

(c)	(相等）我们有 $A\cup A=A$ 以及 $A\cap A=A$ .

(d) (交换性）我们有 $A\cup B=B\cup A$ 以及 $A\cap B=B\cap A$

(e)	(结合性）我们有 $(A\cup B)\cup C=A\cup (B\cup C)$ , $(A\cap B)\cap C=A\cap(B\cap C)$

(f)	(分配性）我们有 $A\cap(B\cup C)=(A\cap B)\cup(A\cap C)$ 以及 $A\cup(B\cap C)=(A\cup B)\cap(A\cup C)$

(g)	(分拆法）我们有 $A\cup(X\setminus A)=X$ 以及 $A\cap(X\setminus A)=\emptyset$

(h)	(De Morgan律）我们有 $X\setminus (A\cup B) = (X\setminus A)\cap (X\setminus B)$ 以及  $X\setminus (A\cap B) =(X\setminus A)\cup (X\setminus B)$

注3.1.29 De Morgan律以逻辑学家Augustus De Morgan 的名字命名，De Morgan把这些定律确立为集合律的基本定律.
证明见习题3.1.6.
</B></font>

---------------
- (a) 证明 $A\cup\emptyset = A$ 以及 $A\cap\emptyset = \emptyset$
    - $A\cup\emptyset = A$
        1. 根据并集的定义，我们有，如果 $x\in \{A\cup \emptyset\}$ 那么 $x\in A$ 或者 $x\in \emptyset$
        2. 根据公理3.1 $x\in \emptyset$ 这条永远不成立，那么 $\{A\cup \emptyset\}$ 的全部元素来自 $A$
        3. 故 $A\cup \emptyset=A$
    - $A\cap\emptyset = \emptyset$
        1. 根据交集的定义，我们知道，如果 $x\in \{A\cap\emptyset\}$ 那么 $x\in A$ 且 $x\in \emptyset$
        2. 根据公理3.1 $x\in \emptyset$ 这条永远不成立，所以不存在满足 $x\in A$ 且 $x\in \emptyset$ 条件的元素，所以是空集
        3. 故： $A\cap\emptyset = \emptyset$
- (b) 证明 $A\cup X= X$ 以及 $A\cap X =A$
    - $A\cup X= X$
        1. 条件中描述 $A\subseteq X$ 所以 $A$ 中的元素全部属于 $X$
        2. 根据并集的定义， $A\cup X$ 表示元素 $x\in A\text{或者}x\in X$ 的集合。
        3. 根据1，2中的描述，以及数理逻辑中“或”的意义，$x\in A\text{或者}x\in X$ 等价于 $x\in X$
        4. 得出结论，证毕。
    - $A\cap X =A$
        1. 条件中描述 $A\subseteq X$ 所以 $A$ 中的元素全部属于 $X$
        2. 根据交集的定义，$A\cap X$ 表示元素 $x\in A\text{ 且 }x\in X$ 的集合。
        3. 根据1，2中的描述，以及数理逻辑中“且”的意义，$x\in A\text{ 且 }x\in X$ 等价于 $x\in A$
- (c) 证明 $A\cup A=A$ 以及 $A\cap A=A$
    - $A\cup A=A$
        1. 根据并集的定义 $A\cup A$ 表明 $x\in A\text{ 或者 }x\in A$ 等价于 $x\in A$
        2. $x\in A$ 的集合就是 $A$
        3. 证毕
    - $A\cap A=A$
        1. 证明方法同上，将或改成且，并集改成交集
        2. 得出结论。
- (d) 证明 $A\cup B=B\cup A$ 以及 $A\cap B=B\cap A$
    - $A\cup B=B\cup A$
        1. 根据并集的定义， $x\in A \text{ 或者 } x\in B$
        2. 数理逻辑中关键词 “或” 的前后两个命题是可以互换而不影响结果的
        3. 所以 $A\cup B=B\cup A$ 就是 $x\in A \text{ 或者 } x\in B$ 的元素组成的集合，等价于 $x\in B \text{ 或者 } x\in A$ 的集合
        4. $x\in B \text{ 或者 } x\in A=x\in(B\cup A)$
        5. 证毕
    - $A\cap B=B\cap A$
        1. 证明如上，因为 “且” 也可以交换前后的命题的顺序，证明方法类似
- (e) 证明 $(A\cup B)\cup C=A\cup (B\cup C)$ , $(A\cap B)\cap C=A\cap(B\cap C)$
    - $(A\cup B)\cup C=A\cup (B\cup C)$
        1. 根据并集的定义，$A\cup B$ 的结果是 $x\in(A\cup B)$ 的集合就是 $x\in A$ 或者 $x\in B$
        2. 根据1中的解释 $(A\cup B)\cup C$ 的结果是 $x\in(A\cup B)$ 或者 $x\in C$ 的集合
        3. 结合1，2得出结论  $(A\cup B)\cup C$ 等价于 $x\in A$ 或者 $x\in B$ 或者 $x\in C$ 的集合。
        4. 同样的方法将右侧拆分， $A\cup(B\cup C)$ 等价于 $x\in A$ 或者 $x\in B$ 或者 $x\in C$ 的集合。
        5. 所以 $(A\cup B)\cup C=A\cup (B\cup C)$ 证毕
    - $(A\cap B)\cap C=A\cap (B\cap C)$
        1. 同上述证明一致，将括号打开用 “且” 连接
        2. 得到 $(A\cap B)\cap C=A\cap (B\cap C)$ 结论。
        3. 证毕。
- (f) 证明 $A\cap(B\cup C)=(A\cap B)\cup(A\cap C)$ 以及 $A\cup(B\cap C)=(A\cup B)\cap(A\cup C)$
    - $A\cap(B\cup C)=(A\cap B)\cup(A\cap C)$
        1. 分情况讨论 $A\cap(B\cup C)$ 有两种情况 $x\in A$ 且 $x\in (B\cup C)$ 根据括号中“并”的定义
        2. 第一种情况是  $x\in A$ 且 $x\in B$
        3. 第二种情况是  $x\in A$ 且 $x\in C$
        4. 2，3中有一个或者两个同时满足，就能得出 $A\cap(B\cup C)$
        5. 也就是 ($x\in A$ 且 $x\in B$) 或者 ($x\in A$ 且 $x\in C$)
        6. 5 写成数学语言 $x\in((A\cap B)\cup(A\cap C)$
        7. 证毕
    -  $A\cup(B\cap C)=(A\cup B)\cap(A\cup C)$
        1. 分情况讨论 $A\cap(B\cup C)$ 根据并集运算有两种情况
        2. 第一种情况 $x\in A$
        3. 第二种情况 $x\in (B\cap C)$
        4. 和上面的证明方法有点不同的是，当第一种情况满足的时候，也就是 $x\in A$ 的时候 $x\in((A\cup B)\cap(A\cup C))$ 成立
        5. 第一种情况满足的时候，也就是 $x\in (B\cap C)$ 的时候， $x\in((A\cup B)\cap(A\cup C))$ 也成立
        6. 证明思路类似于[引理3.1.13](https://tony4ai.com/Math-Analysis-3-1-Fundamentals-P1/)的证明过程。
- (g) 证明 $A\cup(X\setminus A)=X$ 以及 $A\cap(X\setminus A)=\emptyset$
    - $A\cup(X\setminus A)=X$
        1. 根据并集和差集的定义，$x\in A\cup(X\setminus A)$ 表示 $x\in A$ 或者 $x\in (X\setminus A)$
        2. 情况一 $x\in A$ 根据条件中 $A\subseteq X$ 可以得到 $x\in X$
        3. 情况二 $x\in (X\setminus A)$  根据差集合的定义，$x\in (X\setminus A)\Leftrightarrow x\in X \text{ 且 } x\notin A$ 可以得到 $x\in X$
        4. 综合2，3得到  $A\cup(X\setminus A)=X$
    - $A\cap(X\setminus A)=\emptyset$
        1. 根据交集和差集的定义，$x\in A\cap(X\setminus A)$ 表示 $x\in A$ 且 $x\in (X\setminus A)$
        2. $x\in (X\setminus A)\Leftrightarrow x\in X \text{ 且 } x\notin A$ 这与 $x\in A$ 不可能同时发生，所以其交集是 $\emptyset$
        3. 证毕。
- (h) $X\setminus (A\cup B) = (X\setminus A)\cap (X\setminus B)$ 以及  $X\setminus (A\cap B) =(X\setminus A)\cup (X\setminus B)$
    - $X\setminus (A\cup B) = (X\setminus A)\cap (X\setminus B)$
        1. 情况一 $x\in X$ 且 $x\in (A\cup B)$ 时，$x\notin (X\setminus A)\cap (X\setminus B)$
        2. 情况二 $x\in X$ 且 $x\notin (A\cup B)$ 时，$x\in (X\setminus A)\cap (X\setminus B)$
        3. 情况三 $x\notin X$ 且 $x\in (A\cup B)$ 时，$x\notin (X\setminus A)\cap (X\setminus B)$
        4. 情况四 $x\notin X$ 且 $x\notin (A\cup B)$ 时，$x\notin (X\setminus A)\cap (X\setminus B)$
        5. 得出结论 $X\setminus (A\cup B) = (X\setminus A)\cap (X\setminus B)$ 成立
    - $X\setminus (A\cap B) =(X\setminus A)\cup (X\setminus B)$
        1. 情况一 $x\in X$ 且 $x\in (A\cap B)$ 时，$x\notin (X\setminus A)\cup (X\setminus B)$
        2. 情况二 $x\in X$ 且 $x\notin (A\cap B)$ 时，$x\in (X\setminus A)\cup (X\setminus B)$
        3. 情况三 $x\notin X$ 且 $x\in (A\cap B)$ 时，$x\notin (X\setminus A)\cup (X\setminus B)$
        4. 情况四 $x\notin X$ 且 $x\notin (A\cap B)$ 时，$x\notin (X\setminus A)\cup (X\setminus B)$
        5. 得出结论 $X\setminus (A\cap B) =(X\setminus A)\cup (X\setminus B)$ 成立

---------------

<font face="黑体" color=#6F6FFF><B>
注3.1.30 读者可能已观察到上面在 $\cup$ 和 $\cap$ 之间的算律以及在 $X$ 和 $\emptyset$ 之间的算律有一定的对称性，这是对偶性的一个例子一两个不同的性质或者两个不同的对象彼此对偶.在现在的情况下，对偶性是以补关系 $A \mapsto X \setminus A$ 来实现的; Dc Morgan律断定这个关系把并转化成交，而把交转化成并.（此关系也交换了 $X$ 和空集.）上述的算律合起来叫作以英国数学家 George Boole(1815—1864)的名字命名的Boole代数的定律，Boole代数同时也适用于许多集合之外的其他对象，在逻辑学中起着举足轻重的作用.
</B></font>

---------------
讲历史

---------------

<font face="黑体" color=#6F6FFF><B>
我们现在已经积累了关于集合的一些公理和一些结果，但是依然还有许多事情 是我们还不能做的.对于一个集合我们想做的一件基本的事情是，从这个集合中取出每个对象来，以某种方式把它转化成另一个新的对象，例如我们可能想从一个数的集合，例如 $\{3,5,9\}$ 出发，增长它的每个元素而造出一个新的集合 $\{4,6,10\}$ .这并不是可以直接仅仅使用我们己有的公理所能做到的事，所以我们需要一个新的公理：
</B></font>

---------------
开始发生变化，从一个对象到另一种对象的需要，催生了另一个新的操作，并用一个公理来表示，这个公理(替换) 蕴含了分类公理。如下

---------------

<font face="黑体" color=#6F6FFF><B>
公理3.6(替换）设 $A$ 是一个集合.对于任意的对象 $x\in A$ 和任意的对象 $y$ , 假设有一个命题 $P(x,y)$ 依赖于 $x$ 和 $y$ ，使得对于每个 $x\in A $ 存在至多一个 $y$ ，使 $P(x,y)$ 成立.那么存在一个集合 $\{y:P(x,y) \text{对于某} x\in A\text{成立}\}$ ，使得对于任何对象 $z$ ,
$$
z\in\{y:P(x,y)\text{对某} x\in A\text{成立}\}\\
\Longleftrightarrow (\text{对于某}x\in A\text{成立})
$$
</B></font>

---------------
与分类公理不产生新对象不同，替换公理产生新对象。


---------------

<font face="黑体" color=#6F6FFF><B>
例3.1.31 设 $A:= \{3,5,9\}$ 并设 $P(x,y)$ 是命题 $y = x++$ ,即 $y$ 是 $x$ 的后继者.注意到对于每个 $x \in A$ ，恰有一个 $y$ 使得 $P(x，y)$ 成立——具体地说就是 $x$ 的后继者.于是上述公理断定集合 $\{y:\text{对于某}x\in \{3,5,9\}, y = x++\}$ 存在.在此，这明显就是集合 $\{4,6,10\}$ .(为什么?）
</B></font>

---------------
一个自然数的集合中每一个对象都变成其后继，根据集合相等的定义，就能得到 $\{y:\text{对于某}x\in \{3,5,9\}, y = x++\} = \{4,6,10\}$


---------------

<font face="黑体" color=#6F6FFF><B>
例3.1.32 设 $A:= \{3,5,9\}$ 并设 $P(x,y)$ 是命题 $y = 1$ ,那么对于每个 $x\in A$ 还是恰有一个 $y$ ，使 $P(x,y)$ 成立——具体地说，就是数 $1$ ，此时 $\{y :y= 1\text{对于某}x\in\{3,5,9\} \text{成立}\}$ 恰就是单元素集 $\{1\}$ .我们已把原来的集合的每个元素 3,5,9 都替换成了同一个对象，即1.于是这个相当没劲的例子表明，由上述公理得到的集合可以比原来的集合“小”
</B></font>

---------------
一个典型的替换公理的例子，把原始集合中的对象根据任意设计的，明确的规则，得到相应的结果，组成新的集合。


---------------

<font face="黑体" color=#6F6FFF><B>
我们常把形如
$$
\{y: y = f(x)\text{对于某} x\in A\text{成立}\}
$$
的集合简写成 $\{f(x):x\in A\}$ 或 $\{f(x)|x \in A\}$ .于是，例如说若 $A = \{3,5,9\}$ ，则  $\{x++:x\in A\}$ 就是集合 $\{4,6,10\}$ .当然，我们可以把替换公理与分类公理联合使用，于是，作为例子，我们可以构作出类似于 $\{f(x):x\in A; P(x)\text{成立}\}$ 这样的集合，从集合 $A$ 出发，使用分类公理造出集合 $\{x \in A : P(x)\text{成立}\}$，然后再应用替换公理来造出 $\{x\in A:P(x)\text{成立}\}$ 于是，作为例子 $\{n++: n\in\{3,5,9\}:n < 6\}=\{4,6\}\}$
</B></font>

---------------
分号用到这里了，替换公理与分类公理的不同也就是一个规则，产生一个新的对象（这个对象也不一定是新的，可以跟原始对象相同） 分类公理只测试。
替换公理，常写成 $\{f(x):\text{条件 }1:\dots:\text{条件 }n\}$ 就能根据条件和变换规则 $f(x)$ 产生一个新的集合。


---------------

<font face="黑体" color=#6F6FFF><B>
在我们的很多例子中都不明确地假定了自然数事实上都是对象（objects).我们正式把此事陈述如下：
</B></font>

---------------
然后我们把自然数组成的集合输入给替换公理使用，当然我们也可以研究其他对象的替换，比如一筐苹果的集合，替换规则是一个苹果咬一口，条件是红苹果，然后得到一筐被咬了一口的红苹果，大概就这个意思。


---------------

<font face="黑体" color=#6F6FFF><B>
公理3.7(无限）存在一个集合其元素叫作自然数，0是 $\mathbb{N}$ 中的一个对象，而且由每个自然数 $n\in\mathbb{N}$ 所指定的满足Peano公理（公理2.1〜2.5)的对象 $n++$ 也在 $N$ 中.
</B></font>

---------------
关于无限的定义，但是我们还不知道啥是无限，但是我们可以模糊的知道了一个关于自然数集合的定义。


---------------

<font face="黑体" color=#6F6FFF><B>
这是假设2.6的一个更正式的形式.称其为无限的道理是因为它引入了无限集合的一个最基本的例子，即自然数集 $\mathbb{N}$ .(我们将在§3.6中正式叙述何为有限何为无限.）从无限的公理中我们看到，如3, 5, 9等之类的数字的确是集合论中的对象，(由双元素集公理及双并公理）我们确实可以合法地构作如 $\{3,5,9\}$ 这样的集合，就像在我们的例子中己做过的那样.
</B></font>

---------------
无限的严格定义在后面。


---------------

<font face="黑体" color=#6F6FFF><B>
人们必须把集合的概念与集合的元素的概念区别开来；例如集合 $\{n + 3 : n\in \mathbb{N} ，0 \leq n\leq 5\}$ 与表达式或函数 $n+3$ 不是同一回事.我们用一个例子来强调此事：
</B></font>

---------------
集合和函数的不同，函数我们也没定义，但是我们可以他当做一个规则，而集合是一堆东西，一个规则和一堆东西完全是不同的。


---------------

<font face="黑体" color=#6F6FFF><B>
例3.1.33(非正式的）此例需要有减法概念，我们尚不曾正式引入.下述两集合是相等的
$$
\{n+3:n\in\mathbb{N},0\leq n\leq 5\} = \{8 - n：n\in \mathbb{N}, 0\leq n\leq 5\} \tag{3.1}
$$
(见下文)，尽管对于一切自然数 $n$ ,表达式 $n + 3$ 和 $8-n$ 绝对不是同一表达式. 于是，一个好主意是，当你谈论集合时，记住要使用那些花括号以免你偶然把一个集合混同于它的元素.这种反直观的情形的一个缘由是，字母 $n$ 在（3.1) 式两边是以不同的方式被使用的.为弄清此种情形，我们用字母替换字母来重写集合 $\{8 - n : n \in \mathbb{N}, 0 \leq n \leq 5\}$ ,那就得到 $\{8 - m : m \in\mathbb{N} ,0\leq m\leq 5\}$ .这与前一个集合完全是同一个集合(为什么?)，于是我们可把（3.1)重写为
$$
\{n+3:n\in\mathbb{N},0 \leq n \leq 5\} = \{8 — m : m\in N, 0 \leq m \leq 5\}
$$
现在容易看到（使用（3.1.4))为什么这个等式是真的：每个形如 $n + 3$ 的数，其中 $n$ 是介于0和5之间的自然数，也是形如 $8 - m$ 那样的数，其中 $m:=5 - n$ (注意 $m$ 因此也是介于0和5之间的自然数).看看要是我们不先把一个 $n$ 换成 $m$ ，（3.1) 的上述解释将会是多么更让人糊涂！
</B></font>

---------------
一个例子，然后证明一下为什么
为什么中问为啥是相等，我们根据替换原则和条件，能够得出一系列的对象，这些对象组成的集合是结果，于是我们要考察集合相等，集合相等要考察的是其中的对象，只要对象都相等，那么两个集合相等，换句简单的话说，我们不关心每个对象怎么来的，我们只关心他们是否相等。


---------------

<font face="黑体" color=#6F6FFF><B>

## 习题3.1
3.1.1 证明（3.1.4)中的相等的定义是自反的、对称的和传递的.

3.1.2 仅使用定义3.1.4、公理3.2和公理3.3,证明集合 $\emptyset$ ，$\{\emptyset\}$，$\{\{\emptyset\}\}$ 以及 $\{\emptyset,\{\emptyset\}\}$ 全是不同的（即其中没有两个是彼此相等的).

3.1.3 证明引理3.1.13中剩下未证的那些结论.

3.1.4 证明命题3.1.18.

3.1.5 设 $A,B$ 是集合证明三个命题 $A\subseteq B,A\cup B=B,A\cap B=A$ ，在逻辑上是等价的 (其中任何一个都蕴含其他两个).

3.1.6 证明命题3.1.28.(提示：可以使用其中的一些断言去证明另一些断言.有些断言还曾在 前面的引理3.1.13中出现过.）

3.1.7 设是 $A,B,C$ 集合.证明 $A\cap B\subseteq A$  且 $A\cap B\subseteq B$，进而证明 $C\subseteq A$ 且 $C\subseteq B$  当且仅当 $C\subseteq A\cap B$ 用类似的精神证明 $A\subseteq A\cup B$ 且 $B\subseteq A\cup B$ 进而证明 $A\subseteq C$ 且 $B\subseteq C$ 当且仅当 $A\cup B\subseteq C$

--------------
- $A\cap B\subseteq A$  且 $A\cap B\subseteq B$，进而证明 $C\subseteq A$ 且 $C\subseteq B$  当且仅当 $C\subseteq A\cap B$
    1. $A\cap B\subseteq A$  且 $A\cap B\subseteq B$ 这个比较好证明，使用定义，可以明确的得出上面两个表达式正确。
    2. 证明 $C\subseteq A$ 且 $C\subseteq B$  当且仅当 $C\subseteq A\cap B$
        - $C\subseteq A$ 且 $C\subseteq B$  时 $C\subseteq A\cap B$
            1. $C\subseteq A$ 且 $C\subseteq B$ 所以对于所有 $x\in C$ 都有$x\in A$ 且 $x\in B$ 根据交集的定义， $x\in A\text{ 且 } x\in B=x\in (A\cap B)$
            2. 根据子集的定义， $C\subseteq (A\cap B)$
        - 证明当 $C\subseteq A\cap B$ 时 $C\subseteq A$ 且 $C\subseteq B$
            1. $C\subseteq A\cap B$ 时，对于所有 $x\in C$ 都有 $x\in A\cap B$
            2. 根据交集的定义 $x\in A\cap B$ 有 $x\in A\text{ 且 } x\in B$
            3. 所以 $C\subseteq A$ 且 $C\subseteq B$
- 同样的方式可以证明 $A\subseteq A\cup B$ 且 $B\subseteq A\cup B$ 进而证明 $A\subseteq C$ 且 $B\subseteq C$ 当且仅当 $A\cup B\subseteq C$ ，使用相似的方法，由于字数太多，我们就省略相关证明。


--------------
3.1.8 设是集合.证明下面两个吸收律
$$
A\cap (A\cup B)=A,A\cup(A\cap B)=A
$$

------------------
根据命题3.1.28
- $A\cap (A\cup B)=(A\cap A)\cup(A\cap B)$
- 根据 并集和交集的定义， 利用引理3.1.13 的办法可以证明 $(A\cap A)\cup(A\cap B)=A$
- 同理可以证明 $A\cup(A\cap B)=A$


------------------

3.1.9 设 $A,B,X$ 是集合且 $A\cup B=X,A\cap B=\emptyset$  证明 $A =X\setminus B$ 且 $B=X\setminus A$

-----------
证明
1. $X\setminus B$ 根据定义 $x\in X$ 且 $x\notin B$
2. 对于所有 $x\in A$ 根据 $A\cup B=X,A\cap B=\emptyset$ 有 $x\in X$ 且 $x\notin B$ 也就是 $x\in(X\setminus B)$
3. 证得 $A=(X\setminus B)$
4. 同理证明 $B=X\setminus A$

-----------

3.1.10 设 $A,B$ 是集合.证明三个集合 $A\setminus B$ , $A\cap B$ , $B\setminus A$ 是不交的，且它们的并是 $A\cup B$.

-------------------
证明
- $(A\setminus B)\cap (A\cap B)=\emptyset$
    1. $x\in A$ 且 $x\notin B$
    2. $x\in A$ 且 $x\in B$
    3. 同时满足1，2的对象不存在
    4. 证毕
- $(A\setminus B)\cap (B\setminus A)=\emptyset$
    1. $x\in A$ 且 $x\notin B$
    2. $x\in B$ 且 $x\notin A$
    3. 同时满足1，2的对象不存在
    4. 证毕
- $(A\cap B)\cap (B\setminus A)=\emptyset$
    1. $x\in A$ 且 $x\in B$
    2. $x\in B$ 且 $x\notin A$
    3. 同时满足1，2的对象也不存在
    4. 证毕
- 证明 $(A\setminus B)\cup(A\cap B$)\cup($B\setminus A)=A\cup B$
    1. $x\in A$ 且 $x\notin B$
    2. $x\in A$ 且 $x\in B$
    3. $x\in B$ 且 $x\notin A$
    4. 1,2,3有一个发生就行，那么2，包含了1，3中的所有对象，故
    5. $(A\setminus B)\cup(A\cap B$)\cup($B\setminus A)=A\cup B$



-------------------

3.1.11 证明替换公理蕴含分类公理.

-------------------
粗略的证明：
替换公理和分类公理的区别我们前面讲到过，分类公理中的性质是个测试，而替换公理可以产生新的对象，那么我们把替换公理中的替换原则改成分类，就得到替换公理可以生成分类公理。
分类是个过程：“一个对象满足分类规则则得到这个对象”
把这个过程当做替换规则就能得到替换公理对应的分类公理。

-------------------

## 总结
本文证明题非常多，所以如果有些纰漏请您指出，这一课是全书最长的一课，后面我们继续。。
