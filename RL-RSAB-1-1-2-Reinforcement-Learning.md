---
title: 【强化学习】1-1-2 “探索”(Exploration)还是“ 利用”(Exploitation)都要“面向目标”(Goal-Direct)
categories:
    - Reinforcement Learning
    - RL-An Introduction
tags:
    - 强化学习
    - Exploration or Exploitation
keywords:
    - Trade-Off
    - Exploration
    - Exploitation
    - Goal-Direct
    - 平衡
    - 探索
    - 利用
    - 目标导向
toc: true
date: 2018-08-27 22:55:15
---

**Abstract:** 本文介绍强化学习中最重要的一个挑战—— “探索”(Exploration)还是“ 利用”(Exploitation)
**Keywords:** Trade-Off，Exploration，Exploitation，Goal-Direct，平衡，探索，利用，目标导向

<!--more-->
## Trade-off between Exploitation and Exploration(利用和探索之间的平衡)
在强化学习中会遇到一个伴随一生的问题，这个问题其实也出现在我们的生活中，也会遇到这种问题，当你遇到一个问题，一个你以前已经遇到过的问题，你有两种选择，第一种，按照以前的方法（其中之一）来完成这件事（Exploitation）；或者，你可以尝试另一种方法，一种全新的方法（Exploration）；前者可以获得稳定的效果，但是不一定是最优的，后者可能会得到更优的方法，但是也可能得到一个不如以前方法的效果。

同样的情况在强化学习中会一直伴随我们，两种action，选择其中一个是困难的。在下棋的过程中，针对当前的environment，我们的agent以前有类似的经历，是按照过去的经验完成，还是创新一下，采用一种以前没有经验的方法，这个问题dilemma的，而且这两种方法都没有办法保证自己不会失效（fail）
对于一个随机性的任务，更是要经过无数的尝试，才能得到一个稳定的期望，所以那个🐶经过了这么久才能在围棋这种困难的项目上打败人类，而更早的深蓝只能在较简单的项目上打败人类（没错，是什么棋我忘了）。这里所谓的随机性的任务，通俗理解，可以想象成打麻将😆
对于Exploration 和 Exploitation之间的平衡在第二章中详细分析，这个问题经过了几十年大量数学研究，似乎还是没研究明白。
我们只需要简单的记住，我们要平衡他们就可以了。

监督学习，非监督学习则没有这个问题，所以RL跟他们没有附属关系。

## Goal-Direct & Uncertain Environment（目标导向和未知环境）
强化学习的另一个重要的特征就是要面向目标就是goal-direct，这也是强化学习最吸引人的特征。在一个未知的environment里agent要做出他认为正确的action，并且对interaction做出后续的反应。

这与其他那些妖艳的机器学习算法不同，他们“聪明的”分解问题，通过近似的解决子问题了来近似的解决的整个问题。比如监督学习中，很多算法是对最后其自身解决问题的能力是没有评估的。另外的一些方法过于沉迷于找到问题的最优解，而忘记时间的重要，走一步棋要一年，那么我敢保证，机器完胜人类，毕竟能下两盘的人不多。还有就是预测模型从哪来（人为设计了结构的CNN，还是怎么生成一个任意结构的模型）。虽然这些算法在不同问题上都有不错的表现，但是过于注意独立的子问题，是他们自身的一个Limitation。

## Conclusion
今天介绍了两个强化学习的重要特点，EE的平衡，GD目标导向的思想，都决定了强化学习会在未来的研究上更近似智能。

## References
1. Sutton R S, Barto A G. Reinforcement learning: An introduction[J]. 2011.



原文来自：[https://face2ai.com/RL-RSAB-1-1-2-Reinforcement-Learning/](https://face2ai.com/RL-RSAB-1-1-2-Reinforcement-Learning/)转载标明出处
