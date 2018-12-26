---
title: 【强化学习】1.6 本章总结、强化学习历史简述
categories:
    - Reinforcement Learning
    - RL-An Introduction
tags:
    - 强化学习
keywords:
    - 强化学习历史
    - 强化学习总结
toc: true
date: 2018-10-02 16:56:30
---

**Abstract:** 强化学习第一章小结
**Keywords:** 强化学习历史，强化学习总结
<!--more-->
# 本章总结、强化学习历史简述
## 总结
强化学习就是一种通过计算方式来理解和进行目标导向学习的方法。其直接表现是通过agent和环境的交互进行学习，而不需要对环境做任何限制或者需要复杂的环境模型，从本书作者来看，强化学习是第一个研究agent在和环境交互的时候产生的问题计算化的领域，通过研究和环境的交互，达到长期的目标。
强化学习有一个非常明显的框架，就是agent和环境之间的action、state和reward之间的相互关系。这个框架尝试着从一种简单的方式来反应人工智能问题的基本特点，而这些特点包括：“诱因”（cause） 和 “结果”（effect），“不确定”（uncertainty）和 “非决定论”（nondeterminism） 以及 “清晰目标的存在性”（existence of explicit goal）。
## 强化学习历史
强化学习的历史不是很久远，但是由于研究的方向很多，所以没办法把每条只限都列举出来，这里我们主要分成三个方向：
1. 研究 “trial” 和 “error”
    - 起源于早期对动物学习的研究
    - 早期人工智能的主要方向
    - 1980s强化学习复苏的主要动力
2. 优化控制
    - 使用 **value function** 求解
    - 使用 **dynamic programming** 求解
3. 1和2的混合
    - 1和2看起来相互独立，而且独立程度很高，但是我们前面说到的井字棋中使用到了“[时序差分方法](https://face2ai.com/RL-RSAB-1-5-An-Extended-Example/)”（temporal-difference method）

相关论文见引用1中的1.7节

## References
1. Sutton R S, Barto A G. Reinforcement learning: An introduction[J]. 2011.






--------------------------
原文地址:[https://face2ai.com/RL-RSAB-1-6-Summary-History](https://face2ai.com/RL-RSAB-1-6-Summary-History)
