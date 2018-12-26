---
title: 【强化学习】1.1.0 强化学习介绍
categories:
    - Reinforcement Learning
    - RL-An Introduction
tags:
    - 强化学习
keywords:
    - Reinforcement Learning
    - Situation
    - Action
    - Enviroment
    - Closed-loop
    - Optimal Control
    - Markov Decision Processes
    - MDPs
toc: true
date: 2018-08-25 22:41:02
---

**Abstract:** 本文介绍Reinforcement Learning的具体特点和与其他机器学习算法不同之处，本文是一个骨架性的文章，所有专有名词都保持英文原始单词，具体内容会在后续中给出详细解答。
**Keywords:** Reinforcement Learning，Situation，Action，Enviroment，Closed-loop，Optimal Control，Markov Decision Processes，MDPs

<!--more-->
## Reinforcement Learning
中文翻译Reinforcement Learning为强化学习，不知道为啥这么翻译，也没想去了解给这个词命名的人是否非常了解这个领域的知识，既然这么叫了那就这样吧。
上面谈到命名是为了介绍其内容，有些东西的命名，可以直接看出其内容，但是强化学习，显然不是这类的，而“土豆片”，“薯条”这种名字则可以。

本文后面把强化学习简写成RL或者直接写英文名，希望大家理解。

Reinforcement Learning 像Machine Learning一样，名字里都有Learning这个词，表明，RL也是一个问题和解决方案的集合。

值得注意的是，RL属于machine learning的一种。

对于问题和解决方案的集合这种描述，我们有很多经典的例子：

1. “拟合房子的面积大小和价格”的问题
2. “识别手写数字”的问题

这些问题对应了一系列解决方案，比如直线拟合就有最小二乘法，svm等算法来解决；识别手写数字可以有神经网络（Naive版，全连接的那种神经网络），CNN等方法。问题和方法一起构成了一个监督学习（机器学习中的一种）问题。
对于RL一样，一个问题，和一些列解决方案，找出各种方案的优缺点，提出更好的方案，这就是强化学习的内容，而与其他机器学习（监督学习，非监督学习）的具体区别在于解决问题的策略和问题的条件。

RL在解决问题的过程中的基本策略是：
1. 如何对应Situation和Action
2. 如何最大化Rewarding Signal

如果你不懂这四个名词，不要着急，后面有详细的解释，而且，负责任的告诉你，你以后研究的所有问题里面都是围绕他们几个展开的。

在解决RL的问题过程中又会产生一个新的问题，这个问题是在我们还没有看到一个具体的例子之前就能想到的，也是一个基础问题，就是我们的RL学习过程是个closed-loop的过程，通俗点说，像一个死循环，就是当我们用一个输入调整了模型以后，这个模型又会反过来影响这个输入产生的结果，具体点就是这个系统是不稳定的，当你输入在时间 $t_0$ 输入数据 $A_0$ 时产生的结果是 $R_0$ 但是当你在 $t_1$ 输入数据 $A_0$ 时产生的结果是 $R_1$ 并且在大多数情况下， $R_0\neq R_1$ 而专业一些的用词就是：
input会影响这个agent的action，而这个action会改变这个agent，当input再次来袭的时候，新的action会不同，因为这个agent已经变了。
有点绕，没关系，后面看了具体的例子，或者有具体的数据，我会提醒大家回来看这里的。有点抽象，但是复合逻辑。

这个过程中有一个和其他机器学习算法巨大的差别：agent或者learner并不知道每次Input对应的“正确的”Action是什么，也不知道哪个 “Action”能产生最大的Rewarding，而监督学习是知道的。
并且这个Action影响本次迭代的Rewarding并且影响下一个Situation。

这里其实可以想象一下下棋，不管什么棋，学习下棋就是一个RL的过程，我这里不想提某狗的例子是因为，我觉得这只狗已经被一些俗人写的烂大街了（一条好狗被糟蹋了），蹭热度，蹭关键词与我无益，所以，想象一下象棋或者五子棋，每一步都是一个action，而最后的输赢才是唯一的结果，每个action没有一个确定结果与其对应，为了衡量action的好坏，给出一个Rewarding，然后action做出以后，当前situation已经和action之前的不一样了（比如棋局中的形式就是situation）
简单的例子，漏洞百出，但是可以大概这么理解。

## Reinforcement Learning Features

上面大概介绍了一些内容，可能云里雾里，没关系，都是些名词而已，后面内容会详细描述每一个细节，我们要知道的是RL的一些特征，使其与众不同的特征：
1. Closed-Loop是基本的学习方式
2. 学习过程中在每一步并不直接知道应该采用哪个Action
3. Action的结果，Reward Signal并不是实时产生的，需要等待一些步骤后，才能知道。


## Markov Decision Processes
Markov Decision Processes是一个非常经典的全面的对RL中Optimal Control问题的理解，但是要等到第三章才会完整的描述，其中会详细介绍，Agent，Environment等具体的形式，以及如何达到我们的RL目的。其中的Environment非常主要，因为我们的Goal（目的，目标）就是environment的一个state（状态）。
在第三章中，MDPs将会介绍：
- Sensation
- Action
- Goal

等部分的具体内容，而且会明确的直接解决一个RL问题，而不是将原始问题转化为小问题，比如下棋就是要最后获胜而不是把棋局分成几种，分开解决，再合成最后的方案，RL要直接解决问题，而不是细分再整合这种硬套路。

## 总结
中英文穿插写，比较让人讨厌，但是没办法，我觉得如果不看原版书籍，就记住一些翻译版本的关键词是非常有害的。
本文很多关键词和具体过程都没办法现在给出，所以，如果你觉得迷惑，没关系，看懂多少都无所谓，毕竟是个开头介绍，当你看完第一个例子就会豁然开朗，第一个例子在1.3中出现，敬请期待吧。






原文来自：[https://face2ai.com/RL-RSAB-1-1-0-Reinforcement-Learning](https://face2ai.com/RL-RSAB-1-1-0-Reinforcement-Learning)转载标明出处
