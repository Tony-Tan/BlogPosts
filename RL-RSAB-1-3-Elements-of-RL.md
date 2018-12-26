---
title: 【强化学习】1.3 强化学习的基础元素
categories:
    - Reinforcement Learning
    - RL-An Introduction
tags:
    - 强化学习
keywords:
    - Policy
    - 策略
    - Reward Signal
    - 奖励
    - Value Function
    - 评价函数
    - Model of Environment
    - 环境模型
toc: true
date: 2018-09-12 21:51:01
---

**Abstract:** 本文介绍除了agent和environment以外的，对于强化学习最重要的最基础的四个元素。
**Keywords:** Policy，策略，Reward Signal，奖励，Value Function，评价函数，Model of Environment，环境模型

<!--more-->
# 强化学习的基础元素

前面我们说到了几个生活中的例子，和几个RL中基本模型，本文我们继续深入，探索目前强化学习中最基本的组成元素。
## 策略(Policy)

策略是我的翻译，我不知道“正确”的翻译是什么，但是我们还是老原则，用英文。书中的定义是
> A policy defines the learning agent's way of behaving at a given time.

换成中文，我觉得更接地气一点的就是Policy就是agent的在任意时刻的产生行为的依据，也就是说agent，或者人或者动物或者robot，在某一时刻，有明确的最终目的，但是此时此刻，面对这个时间点的环境的时候，他要产生某种规则的方式，就是policy。如果用数学的的形式就是
$$
f(环境)\to 行为
$$
这里面的 $f$ 是个映射，是不是函数不一定，这个映射，综合当前的所有环境信息，产生一个action。最简单的例子，这个policy可以是个映射表，当出现什么环境的时候，就做出什么样的决定，如果你穷举了下棋的所有棋局，那么每种盘面都对应了最佳的走法，我们把这个对应走法记录在一章表上，那么这个表（中的内容）就是policy

心理学中把这个叫做刺激反应法则（stimulus-response rule）或者叫做 association（翻译不出来）
Policy可以是非常简单的规则，比如自动驾驶看到红灯要停车，也可以及其复杂，比如自动驾驶要把在菜市场里的车开出来。
Policy是agent面对环境做出action的核心，但是这个核心有时候也可以是随机的，换句话说，上面那张“表”里面的信息可能是随机——有时候随机并不代表不可靠




## 奖励信号(Reward Signal)
激励信号，或者奖励信号，书中的定义：
> A reward signal defines the goal in a reinforcement leaning problem

激励信号，就是agent的得分，目前我们的研究的agent在每一步都有来自环境的反馈，由于我们没有所谓的有teacher，所以我们会通过一个我们设计的[reward函数]，来计算出每一步我们得到的评分，而agent存在的目的就是在他的生命周期内最大化这个得分的总和。
reward signal是agent来自每一个action之后环境给与的反馈，得分定义这个action的好坏，以及好坏的程度；类似于生物中，对于刺激的反应，是表示舒服还是难受。
这个goal是立刻的明确的，不能有延迟以及模棱两可的评分，或者说是客观公正的，不能被agent改变的，换句话说，这个goal是公正的，agent不能又当运动员又当裁判。agent只能通过根据环境改变自己的行为来最大化自己的goal，评判有专门的[裁判]（后面会说到）。
agent不断学习新的知识来提高自己的得分，其工作方法是改变自己的policy，所以reward signal 就成了调整policy的主要依据了。
同样这个裁判也有可能是随机，没错，一个疯了的裁判，至于为什么会有这种情况，我也不知道后面会不会涉及。
第三章我们会介绍这个reward 函数对于agent不变是有生物学原理的，原理来自我们的大脑。

## 值函数(Value Function)
注意区分，Reward Function 和Value Function是不同的两个函数，作用不同，性质不同。书中的定义：
>Whereas the reward signal indicates what is good in an immediate sense, a value function specifies what is good in the long run.

用中文说就是，reward是每次的结果，value function是来预测这个agent到最后能得到的总分。value function有一定的预测行为在里面，包括后面可能出现的情况，而这些情况是否发生目前是不确定的，一个类似的例子就是，你在准备高考，参加了两次模拟考试，reward是你这两次模拟考试的好坏，而value函数是要预测你最终高考结果的。
很有可能出现这种情况，前面几个step的reward 都很低，但是value function却依然很好。反之亦然会出现。

在RL中reward是最主要的，value function次之。如果没了reward那么agent就是无头苍蝇，而且没有reward就没了value的概念。value更像是辅助agent 来得到更多reward的参考书。
然而，agent做决定的时候有时候更多的考虑value function，这个原因是我们的RL最终目标是得到更高的reward总和，而不是某一步的reward，所以这会给我们一个错觉，就是value function比reward要更重要。
作为得到更多reward的参考，value的获得更加复杂，比获得reward要复杂的多，reward依据当前环境给出，而value是依据后面的环境给出，一个是静态的，一个是动态的（环境在变，agent的policy也在变）
在一般的RL算法的重要组成就是要找到有效value预测值，这个工作会是未来几十年的重点研究方向
## 环境模型(Model of Environment)

最后一个就是环境模型，环境是个很大的很抽象的概念，环境模型和环境并不是一会儿事，但是关系密切。

某个state下，agent的某个action可能会获得什么样的反馈，环境模型可以预测这个反馈，而实际的反馈是环境根据state和action给出的，所以环境模型越接近环境，就会越准确，环境模型更多的用来做Planning，就是agent在做action之前“思考，计划”
使用环境模型和planning进行的RL，我们称之为“基于模型的方法”，相反，就是“不基于模型的方法”
不基于环境模型的方法是更直接的trail-and-error 方式，经常被看做是planning方式的对立方法。但是第八章，会介绍同时使用 trail-and-error 和 模型以及planning 的方法。
基于模型的方法会跨越我们RL中各种层级：low-level, trail-and-error, high level以及deliborating planning。

## Conclusion

本文介绍了强化学习的四种基本组成元素，不但基础而且非常重要，基本就是后面所有的研究对象了。需要好好研究。


## References
1. Sutton R S, Barto A G. Reinforcement learning: An introduction[J]. 2011.



原文来自：[https://face2ai.com/RL-RSAB-1-3-Elements-of-RL](https://face2ai.com/RL-RSAB-1-3-Elements-of-RL)转载标明出处
