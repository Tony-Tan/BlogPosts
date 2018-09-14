---
title: 【强化学习】1-1-1 强化学习、监督学习和非监督学习
categories:
    - Reinforcement Learning
    - RL-An Introduction
keywords:
    - Supervised Learning
    - Unsupervised Learning
    - Reinforcement Learning
toc: true
date: 2018-08-26 22:34:32
---

**Abstract:** 本文主要介绍强化学习，监督学习，非监督学习之间的不同。
**Keywords:** Supervised Learning，Unsupervised Learning，Reinforcement Learning

<!--more-->
## Machine Learning（机器学习）
上文我们曾提到强化学习是机器学习的一种，而机器学习的定义是什么我也不记得了，而可以肯定的是下面这三类算法或者三个learning都属于机器学习，机器学习是个更大的概念：
1. 监督学习
2. 非监督学习
3. 强化学习

监督学习是最常见，也是当前比较火爆的领域，你要是不懂个CNN，神经网络的都不好意思是说自己是做研发的，这些算法都是监督学习。
非监督学习更注重通过算法来找到一些为标记的数据的背后的关系，比如常见的聚类算法。

强化学习，最形象的过程就是学下棋，目标就是赢棋，至于如何走每一步，这就是算法要解决的问题了，不管你怎么折腾，目标明确，就是要赢棋，尽量不输。

关于机器学习的相关知识强烈推荐，参考文献2的这本书，很详细和严谨。后面如果有时间，我也会接着写这本书的博客。

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/RL-RSAB-1-1-1-Reinforcement-Learning/images.jpeg)

下面我们介绍下RL对于监督学习和非监督学习的主要区别。

## Reinforcement Learning v.s. Supervised Learning（强化学习和监督学习）

首先我们要介绍一个概念，knowledge，这个单词是中学学的，表示知识，对于这两类算法，可以理解为在建立模型之前已知的所有条件，这些条件包括问题类型，已知的对于此类问题有效的方法，已知数据等等，所有我们知道的，与之相关的都是knowledge，而在这些knowledge中，RL和SL(监督学习的简称)的一个最最最显著的不同就是数据，SL的每一条数据都有明确的label，也就是模型应该对这条数据的反应，而RL没有。这就产生了巨大的差别，RL每次对input的action是不知道对错的，也不会产生什么loss或者残差，而监督学习可以，而且可以用数字精确衡量，而RL最多也就是自己估计一个好坏程度，摸着石头过河。

没错，监督学习，是有家教的好孩子，每一步都有指导方针，每天都有纠错，改错，进步，没什么随机性。而RL就是个野生的，一学期没人管，期末考试没考好，回家被揍一顿，类似于这种效果。

你可能说，监督学习里也有随机啊，随机梯度下降，放心，那是因为愚蠢的人类目前没有找到直接一步到位的优化方法，而这种方法应该是存在的，随机过程只是优化方法无奈的一种选择，而监督学习的每一步都是有准确衡量的，错了多少，对了多少都是明确的。这就是和RL的最大区别之一。

监督学习通过extrapolate，generalize，最后得到一个尽可能高准确率的分类结果（或者叫做response）
监督学习没有环境这种说法，所以其学习的就是不是环境和acition之间的interaction。

如果你一定要说interaction是损失函数算出来的数值，这里我觉得也可以，但是似乎有些怪异，比如一个baby走路，爸爸妈妈的教法应该是，站起来，别摔倒，能走多远走多远，而如果要使用损失函数，就有点类似于告诉这个小baby，你每一步必须走21.286cm，多了或少了都要有“损失”哦，损失函数更像机器，RL更像人
。
RL的另一个特点是，他学习的最终目的是对所有situation都有正确并且及时的反应，不能对于一个situation没有反应或者自己错乱了，这都是不允许的。
RL从他之前的experience中产生action这个也是SL没有的，因为SL没有经验，所有信息都在模型本身里面，没有什么记忆可以谈。


## Reinforcement Learning v.s. Unsupervised Learning （强化学习和非监督学习）

有人说RL不是监督学习，那就非监督学习喽，其实他们也有很大的不同，所以非监督学习并不是监督学习在机器学习领域内的补集。

非监督学习的主要目的，就是找到无任何标记的数据的背后的隐含的关系。所以没有对错输赢这种书法，更像是听天由命。

为出现的结构数据对于RL是有用的，但是更多的数据并不能解决RL的问题，但是更多的数据对于SL或者UL往往能产生质的飞越。

RL不是监督学习，也不是非监督学习，RL的目标很单纯:

> MAXIMUM REWARDING SINGAL


## Conclusion
本文主要介绍强化学习和监督学习，非监督学习的区别，并说明，机器学习不是简单的分成监督非监督学习两种。



## References
1. Sutton R S, Barto A G. Reinforcement learning: An introduction[J]. 2011.
2. Nasrabadi N M. Pattern recognition and machine learning[J]. Journal of electronic imaging, 2007, 16(4): 049901.


原文来自：[https://face2ai.com/RL-RSAB-1-1-1-Reinforcement-Learning](https://face2ai.com/RL-RSAB-1-1-1-Reinforcement-Learning)转载标明出处
