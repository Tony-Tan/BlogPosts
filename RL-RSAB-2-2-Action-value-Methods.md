---
title: 【强化学习】2.2 行为评价方法(Action-value Methods)
categories:
    - Reinforcement Learning
    - RL-An Introduction
tags:
    - 强化学习
    - k臂赌博机
    - 多臂赌博机
    - 利用
    - 探索
    - 行为评价方法
    - 样本均值方法
toc: true
date: 2018-10-10 22:03:20
---

**Abstract:** 本文介绍第一种强化学习方法——行为评价方法(Action-value Methods)，非常简单但是可以通过这个简单的算法来感受下强化学习的难点和问题解决的思路
**Keywords:** 强化学习, k臂赌博机, 多臂赌博机, 利用, 探索, 行为评价方法，样本均值方法, $\varepsilon$-greedy方法
<!--more-->
# 行为评价方法(Action-value Methods)
本文介绍解决k-臂赌博机的第一种简单的naive的方法，注意区分两个重要的概念，评价方法(value function)产生的值(value)和奖励信号(reward signal)之间的区别。
## 评价方法和奖励信号的回顾
注意我们前面介绍的两个概念，相似但是完全不同，就是评价方法(value function)产生的对行为的评价，以及行为执行后的奖励信号(reward signal)。


{% note info %}
这里再举个小栗子🌰吧，加入你是个超级富豪，你去街边游戏厅玩一块钱一次的赌博机，当然游戏厅有一排赌博机，假设有 $k$ 个，你随身都带着助理，你的助理是哈佛耶鲁MIT三大名校的统计学博士，你俩去玩赌博机，玩之前，你的助理会告诉你你下一局应该把你的一块钱下注到哪台机器，你肯定要问他他是怎么算出来的，于是，他拿出笔记本电脑，连接到了超级服务器上，用一个超级复杂的公式，评估出了所有赌博机的输赢概率，最后得出，第二台赌博机，赢的概率最大，为90(这个数是博士设计出来的公式的结果)比别的赌博机的对应值都高，所以你心悦诚服的去把你的一块钱压到了二号赌博机。
这个例子，你的博士助理就是一个value function或者说他的那套算法是value function也可以，那个90就是某个行为(选择的某台机器的value值)。
于是你就去赌博了，堵你的一块钱，经过赌博机的一些列运行，果不其然，你赢了两块钱，你很高兴的买了一个冰棍，你和你的博士朋友一起吃了起来 —— 这里面的两块钱就是reward signal。
可见value是和reward signal 完全不同但又息息相关的概念
{% endnote %}

上面我们应该大概能区分action的value和reward signal了。
我们继续回顾我们上文（[点击查看详情](https://face2ai.com/RL-RSAB-2-1-A-k-armed-Bandit-Problem)）讲到的这个公式：
$$
q_{\ast}(a)\doteq\mathbb{E}[R_t|A_t=a]
$$

公式中 $q_{\ast}(a)$ 是一个函数，表示reward signal的期望，公式中包含了 $R_t$ 为对应的第 $t$ 个过程中的 Reward Signal ，而接着就用
$$
Q_t(a)\approx q_\ast(a)
$$
来定义了个value function：换句话说，我们刚弄了个每一步的reward signal的期望，就被人顺水推舟做了value function，因为value function 总是要和reward signal相关的value变大的时候reward signal一定也要对应的变大或者变小也可以，只要他们之间的变化时按照固定规律进行的，我们就能通过value function来最大化reward signal。
还有一个区别value 和reward signal的办法是value时选择action之前用的，reward是action之后(甚至是多个actions之后)得到的，来自environment的反馈。

**一定要注意的就是value function和reward signal之间的关联和相互利用，不要糊涂了，我们的最终目的是最大化reward signal**

## 样本均值(sample-average method)方法
上面已经用reward signal的期望来做value function了，那么我们第一种方法也就这么来了，我们计算 $\text{action}_a$ 在过去所有步骤中出现的时候得到的reward signal平均值，作为$\text{action}_a$ 的value，这就产生了下面这个value function:

$$
Q_t(a)\doteq\frac{\text{a出现的时候reward signal的和}}{\text{a出现的次数}}=\frac{\sum^{t-1}_{i=1}R_i\cdot1_{A_i=a}}{\sum^{t-1}_{i=1}1_{A_i=a}}
$$
这里面唯一不好理解的就是符号 $1_{A_i=a}$ 这里的1不是一个数值，你可以把它理解为一个布尔变量，他不是值，他是个指示变量，当第 $t$ 步骤中 $A_t$ 采用的是a的话，那么这个值$1_{A_t=a}=1$ 否则 $1_{A_t=a}=0$
 $R_i$ 就是对应的第 $i$ 步获得的reward signal。所以这个奖励信号就和一个指示函数相乘了，最后求出来当a被使用的时候的reward signal的和了，分母是同样的原理，是一个计数器。
如果你学过数字信号或者模拟信号，你会了解有一个叫做使能信号的东西，$1_{A_t=a}$ 与之类似。
但是这里有除法就要注意如果我们之前一直没有采用过$\text{action}_a$  那么分母就是0了，所以我们可以硬性规定一下，如果分母为0，那么 $Q_t(a)=0$
根据大数定理，当我们的action使用的足够多的时候，样本的期望就是随机变量的期望，对大数定理不太了解的同学可以参考:[大数定理](https://www.face2ai.com/Math-Probability-6-2-The-Law-of-Large-Numbers/)也就是说，当我们步骤足够多的话，action也都被大量使用时，$Q_t(a)$ 收敛于 $q_\ast(a)$ 。
以上只是一种非常简单的value function的设计，有效与否要等到后面的试验，但是从理论上来说，他是可以满足我们的需求的。
那么上面我们就完成value function的设计，接着我们要考虑的就是如何选择 action 了，是 exploitation 和 exploration 呢？
## $\varepsilon$-greedy方法
第一种，最传统的办法，选择期望最高的，也就是贪心的选择，对应的 action 就属于 exploitation，使用数学表达这个过程就是 在$t$ 步选择$A^{\ast}_{t}$使得 $Q_{t}(A^{\ast}_{t})=\text{max}_aQ_t(a)$ 贪心选择的整体被写作：

$$
A_t\doteq \mathop{\arg\max}_{a} Q_t(a)
$$
上面这个符号 $\mathop{\arg\max}_{a}$ 会在你的人工智能历程中一直跟你纠缠不休，用中文解释就是在所有可行的 $a$ 中，找到一个能使 $Q_t(a)$ 最大的，并返回这个 $a$ ，也就是$A_t$ 每一步都是使用时 $Q_t(a)$ 最大的 $a$。

**贪心方法最大的问题就是，他每次都使用前面所有行为中看起来最大收益的行为，但是他无法保证这个行为确实是最好的，比如，有一种行为更好，但是前面的几次都发挥失常，没有得到较好的reward signal，这种情况是完全有可能的，而贪心算法解决不了这个问题。**

一种改进就是以一定概率 $\varepsilon$ (相对较小)执行随机选择的行为，选择范围是所有可行的行为，且他们被选择的概率相等,这种随机选择的action就是我们前面提到的exploration，这种方法叫做$\varepsilon$-greedy方法。
这种方法的优点是随着学习的不断增长，所有的行为的被执行次数都是趋近于无穷的，那么这时候可以保证 $Q_t(a)$ 收敛于 $q_\ast(a)$。因为我们执行exploitation的概率是 $1-\varepsilon$ 其数值是接近1的，而exploration的概率$\varepsilon$ 是接近0的，当执行了非常多的过程后$Q_t(a)$ 收敛于 $q_\ast(a)$，此时我们有$1-\varepsilon$ 的比例是执行的贪心过程，那么我们的所有步骤中就有不小于 $1-\varepsilon$ 的比例是选择的最大收益的行为，这种结果将会是相当客观的。
上面这套理论从字面上看起来近乎完美，但是这个收敛是个渐进的保证，换句话说学习次数在现实中不可能趋近于无限，我们的训练次数也是有限的，我们目前还不能根据一个理论上的极限情况来断定在实际执行过程中也获得很好的表现。


## 总结
本文介绍了value function的设计，以及两种方法来选择action，我们从此篇开始，正式进入强化学习的大门。
## References
1. Sutton R S, Barto A G. Reinforcement learning: An introduction[J]. 2011.



--------------------------
原文地址: [https://face2ai.com/RL-RSAB-2-2-Action-value-Methods](https://face2ai.com/RL-RSAB-2-2-Action-value-Methods)
