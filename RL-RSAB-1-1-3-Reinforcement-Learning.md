---
title: 【强化学习】1-1-3 强化学习基本框架
categories:
    - Reinforcement Learning
    - RL-An Introduction
tags:
    - 强化学习
keywords:
    - agent
    - real-time
    - organism
    - robot
    - framwork
toc: true
date: 2018-08-29 23:18:29
---

**Abstract:** 本文简要介绍强化学习的框架，以及框架中几个概念的基本关系
**Keywords:** agent，real-time，organism，robot，framwork

<!--more-->
## Reinforcement Learning Framework
上来就把这篇的核心知识点讲出来吧，对于一个RL任务，其框架从总体上分，包括：
1. agent
2. agent's environment

我不知道怎么翻译agent这个词，所以就一直用英文了，代理，或者特工都不太合适，而且我总能想到Agent Hunter。。agent我们已经用了好多次了，到现在都不知道是什么，是算法，还是算法和其他的什么的合集，就像模型一样，可能用了很久都不知道所谓模型，架构到底是什么，而我们在后面会用详细的例子来形容agent是什么。
就像数学分析里面的定义一样，一个限定加命名而已。所以不要过于担心这一点。

## Agent
虽然不知道agent到底是什么，有没有枪什么的，但是我们知道他有以下几个特点：
- explicit goal(明确的目标)
- sense aspect of their environment(对他们的环境敏感)
- choose action to influnce their environment(选择action来改变environment)

即使在算法的刚开始，agent没有任何经验，比如对于一个刚学会下棋规则的人来说，他没有任何经验，但是他也要对棋局做出反应，瞎弄都可以，但是你不能楞在那，这是不可以的，agent要对环境做出action，即使是未知环境。

如果包含planning的过程，agent不能一直planning，要平衡planning和real-time之间的关系，还有环境模型如何生成和提升等（这几句话如果不懂，不用急，因为这个是更复杂的RL，后面回头看会好一些）

如果RL包含监督学习的部分，agent还有个任务就是判断哪个监督学习模型的能力强，哪个弱（这个同样是复杂版本的RL，也需要后面的知识来融汇贯通）

还是继续说agent，agent不是我们想象中organism或者robot，就是agent并不是一个完整的有智慧的个体，或者一个器官，agent更像是一个复杂系统中的一个组成部分，比如对于一个完整robot系统，其中一个agent就像是电池系统，负责管理充电程度的，这个agent不和机器人的外部环境直接interact，而是和更大的系统（包含他的那个机器人系统）直接interact。这时候这个agent的environment就是robot所处的大environment，以及robot内部出了自己以外的其他部分。

## Conclusion
本文有点小凌乱，介绍了RL的框架，是Agent和他的environment，以及agent的几个小特点，以及environment是什么，总体来说比较抽象，我也不知道为啥这本书开头就给出了这么多没解释的东西，但是可能作者的风格就是让你先猜一下，后面再公布答案。
## References
1. Sutton R S, Barto A G. Reinforcement learning: An introduction[J]. 2011.





原文来自：[https://face2ai.com/RL-RSAB-1-1-3-Reinforcement-Learning/](https://face2ai.com/RL-RSAB-1-1-3-Reinforcement-Learning/)转载标明出处
