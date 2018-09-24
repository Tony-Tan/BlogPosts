---
title: 【概率论】Probability Big Picture
sticky: 10
categories:
  - Mathematic
  - Probability
keywords:
  - tag
toc: true
date: 9999-10-30 23:59:59
---

**Abstract:** 关于概率论的整体框架(2017-11-13 15:57:45)
**Keywords:** 概率论

<!--more-->
# 开篇废话
本篇可能都是废话（没有具体知识点），但是先说点废话中的废话，还是吐槽自己之前上学没有好好学习的吧，说实话，概率论和数理统计基本上学学到的知识等于几个名词，我认为当讲授一门课程的时候，作为讲授者，必须对这套知识背后的理论逻辑，以及实际应用有充分的理解和经验，不然没办法把这套完整的数学分支在几十节课的时间让一群完全没有先验知识的人正确的入门，并走上正确的方向，所以入门传道者一定要非常资深，因为如果你入门就走歪了，后面更麻烦，所以我觉得我很幸运，老子还是一张白纸，哈哈哈。
在一个从应用角度，机器学习用处非常多，有一类用户只负责调用工具或者封装好的算法进行一些既定工作，我们把他们定义为强应用用户，他们不关心算法正确性，速度以及结果，只是按照指定的步骤，使用工具；第二类，弱应用用户，负责找出合适问题的算法，并把使用方式告诉第一类用户，这类用户就需要知识或者经验了，可以没有知识，但要有经验，或者没有经验，只有知识，通过大量的实验找出合适的算法；第三类，进入到算法层，理解算法背后的数学逻辑，以及算法失效的情况，以及速度等底层问题，这类人需要较好的数学基础，因为在阅读论文的时候会有很多基础处理方法，作者会略过，所以掌握好基础很重要。
这三类人没有歧视链，大家都是干工作没有高低贵贱，但是如果你是第一类人但讲的都是第三类的话，而且漏洞百出，这样就贻笑大方了。
# 概率论
概率论不是数理统计，这两个有明显的区别，概率论是已经知道了内部机制，推算结果，数理统计是通过观察结果反推内部机制，机器学习更倾向于后者，但是概率论是数理统计的基础，所以概率论在我们的big big picture里面是五星的，与微积分线性代数同样重要。
![](https://raw.githubusercontent.com/Tony-Tan/MachineLearningMath/master/Mathematic.png)
与线性代数和微积分不同，微积分更注重计算，也就是当我们有一个算式的时候可以用各种技巧得出最后的答案，这里的微积分是说初级的，高级的到了分析层面就是另一回事了，那个就是探索真理了，线性代数背后有一套完整的理论体系，而且包含了一些可以应用在实际场景的模型，所以线性代数属于基础与实践的边缘部分，故而在工程里显得尤为有用处。
概率论则更加偏向应用，因为其提出就是为了赌博，概率论的公式都比较复杂，但是每一个公式背后都有非常明显的事物关系，也就是说概率论中的公式能清楚的反应一些事物的本来面目。
# 教材
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Probability-Big-Picture/probability_book.jpeg)
## 《Probability and Statistics》 M.H.DeGroot
《Probability and Statistics(Fourth Edition)》(Morris H. DeGroot)这本书也很适合入门，书中有大量的例题和课后习题，语言也比较清晰明确，美国教材和中国教材以及俄罗斯前苏联的教材风格迥异，感觉美利坚的教材能让你更有自信，苏联教材能让你怀疑人生，而大部分中国教材能让你考上研究生，哈哈，不黑了
## 《概率论与数理统计》陈希孺
条理非常清晰，能很清楚的解释公式背后的理论和应用，作为DeGroot的辅助，来完善整体知识结构。
## 《初等概率论》 钟开莱
钟开莱是概率论开天辟地的人之一，而其写的书也是很简单明了，利于入门学习，还有一本《概率论教程》更适合进阶，涉及到了测度论等高级的分析工具。
## 《概率导论》 Dimitri P. Bertsekas
MIT的教材，比较简单，适合入门，也是作为补充，来辅助我们入门
# 博客章节目录
- Introduction
- Conditional Probability
- Random Variables and Distributions
- Expectation
- Speacial Distributions
- Large Random Samples
# 总结
先放个图
![](https://raw.githubusercontent.com/Tony-Tan/MachineLearningMath/master/Probability.png)
这个是根据DeGroot的教材总结出的基本知识重点框架。
机器学习相关数学知识可以参考项目,以上图片皆出自此项目：
[https://github.com/Tony-Tan/MachineLearningMath](https://github.com/Tony-Tan/MachineLearningMath)

全部文章目录：
- [1.0 概率介绍、试验、事件、公理化的概率](http://www.face2ai.com/Math-Probability-1-0-Introduction/)
- [1.1 样本空间、柯氏公理、概率的性质](http://www.face2ai.com/Math-Probability-1-1-Definition-of-Probability/)
- [1.2 古典概率、乘法原理、排列](http://www.face2ai.com/Math-Probability-1-2-Counting-Methods/)
- [1.3 组合、二项式定理、多项式定理](http://www.face2ai.com/Math-Probability-1-3-Combinatorial-Methods/)
- [1.4 有限事件并的概率、概率欺骗了你](http://www.face2ai.com/Math-Probability-1-4-Union-of-Event/)
- [2.1 条件概率、全概率公式](http://face2ai.com/Math-Probability-2-1-Conditional-Probability/)
- [2.2 事件独立、条件独立](http://www.face2ai.com/Math-Probability-2-2-Independent-Events/)
- [2.3 Bayes’ Theorem](http://www.face2ai.com/Math-Probability-2-3-Bayes-Teorem/)
- [3.1 随机变量和离散分布](http://www.face2ai.com/Math-Probability-3-1-Random-Variables-and-Discrete-Distributions/)
- [3.2 连续分布](http://www.face2ai.com/Math-Probability-3-2-Continuous-Distribution/)
- [3.3 Cumulative Distribution Function](http://www.face2ai.com/Math-Probability-3-3-Cumulative-Distribution-Function/)
- [3.4 双变量分布](http://www.face2ai.com/Math-Probability-3-4-Bivariate-Distribution/)
- [3.5 边缘分布不和独立随机变量](http://www.face2ai.com/Math-Probability-3-5-Marginal-Distributions/)
- [3.6 条件分布 (Part I)](http://www.face2ai.com/Math-Probability-3-6-Conditional-Distributions-P1/)
- [3.6 条件分布 (Part II)](http://www.face2ai.com/Math-Probability-3-6-Conditional-Distributions-P2/)
- [3.7 多变量分布（Part I）](http://face2ai.com/Math-Probability-3-7-Multivariate-Distributions-P1/)
- [3.7 多变量分布（Part II）](http://www.face2ai.com/Math-Probability-3-7-Multivariate-Distributions-P2/)
- [3.8 随机变量的函数](http://www.face2ai.com/Math-Probability-3-8-Fuctions-of-a-Random-Variable/ )
- [3.9 多随机变量的函数](http://www.face2ai.com/Math-Probability-3-9-Functions-of-Two-or-More-Random-Variables/)
- [4.1 随机变量的期望 (Part I)](http://www.face2ai.com/Math-Probability-4-1-The-Expectation-of-a-Random-Variable-P1/)
- [4.1 随机变量的期望 (Part II)](http://www.face2ai.com/Math-Probability-4-1-The-Expectation-of-a-Random-Variable-P2/)
- [4.2 期望的性质](http://www.face2ai.com/Math-Probability-4-2-Properties-of-Expectations/)
- [4.3 方差](http://www.face2ai.com/Math-Probability-4-3-Variance/)
- [4.4 距](http://www.face2ai.com/Math-Probability-4-4-Moments/)
- [4.5 均值和中值](http://www.face2ai.com/Math-Probability-4-5-The-Mean-and-the-Median/)
- [4.6 协方差和相关性](http://www.face2ai.com/Math-Probability-4-6-Covariance-and-Correlation/)
- [4.7 条件期望](http://www.face2ai.com/Math-Probability-4-7-Conditional-Expectation/)
- [5.1 分布介绍](http://www.face2ai.com/Math-Probability-5-1-Special-Distributions/)
- [5.2 伯努利和二项分布](http://www.face2ai.com/Math-Probability-5-2-the-Bernoulli-and-Binomial-Distributions/)
- [5.3 超几何分布](http://www.face2ai.com/Math-Probability-5-3-The-Hypergeomtirc-Distribution/)
- [5.4 泊松分布](http://www.face2ai.com/Math-Probability-5-4-The-Poisson-Distribution/)
- [5.5 负二项分布](http://www.face2ai.com/Math-Probability-5-5-The-Negative-Binomial-Distribution/)
- [5.6 正态分布(Part I)](http://www.face2ai.com/Math-Probability-5-6-The-Normal-Distributions-P1/)
- [5.6 正态分布(Part II)](http://www.face2ai.com/Math-Probability-5-6-The-Normal-Distributions-P2/)
- [5.6 正态分布(Part III)](http://www.face2ai.com/Math-Probability-5-6-The-Normal-Distributions-P3/)
- [5.7 Gamma分布(Part I)](http://www.face2ai.com/Math-Probability-5-7-The-Gamma-Distributions-P1/)
- [5.7 Gamma分布(Part II)](http://face2ai.com/Math-Probability-5-7-The-Gamma-Distributions-P2/)
- [5.8 Beta分布](http://face2ai.com/Math-Probability-5-8-The-Beta-Distribution/)
- [5.9 多项式分布](http://face2ai.com/Math-Probability-5-9-Multinomial-Distribution/)
- [5.10 二维正态分布](http://face2ai.com/Math-Probability-5-10-The-Bivariate-Normal-Distributions/)
- [6.1 大样本介绍](http://face2ai.com/Math-Probability-6-1-Large-Random-Samples-Introduction/)
- [6.2 大数定理](http://face2ai.com/Math-Probability-6-2-The-Law-of-Large-Numbers/)
- [6.3 中心极限定理](http://face2ai.com/Math-Probability-6-3-The-Central-Limit-Theorem/)
- [6.4 连续性修正](http://face2ai.com/Math-Probability-6-4-The-Correction-for-Continuity/)
-
***注意：本系列的博客所有定义均来自 DeGroot's book ***





原文地址1：[https://www.face2ai.com/Math-Probability-Big-Picture](https://www.face2ai.com/Math-Probability-Big-Picture)转载请标明出处
