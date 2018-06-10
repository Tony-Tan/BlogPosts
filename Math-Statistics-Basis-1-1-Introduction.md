---
title: 【数理统计理论基础】 1.1 导言
categories:
    - Mathematic
    - Statistics
tags:
    - 数理统计
    - 数理统计应用
toc: true
date: 2018-05-30 14:15:45
---

**Abstract:** 本文主要介绍数理统计学简史，以及数理统计学怎么应用
**Keywords:** 数理统计，数理统计应用

<!--more-->
## 开篇废话
某乎上有人说《数理统计学教程》非常适合入门，实际情况是看了其中大多数内容两边，例子和讲解及其清晰，但是有些证明比较复杂，比较不好理解，并不是说作者给出的证明方法冗杂，而是那些结论本身证明就十分困难，所以内心就会有个矛盾，如果深入研究证明，会发现看不懂，因为涉及到一些技巧和知识可能我们并没有掌握，如果不看，又怕错过什么，这可能就是自学的另一个麻烦，当然，我在写博客的时候会把证明全写出来，并把每一步都解释清楚，当然这很耗时，但是应该会有很大的帮助。
这是导言，但是我觉得这个导言比后面的每一课都重要，但是我上学的时候是不看导言的，但是后来开始自学就开始看导言和介绍了，因为需要在里面找到本书的大致结构和安排，而本书的导言上来就是干货，而且是big picture级别的。

<font color="0000ff">所本文对于本系列的理解十二分重要！</font>
## 什么是数理统计学
### 没有定义
大多数数理统计学教材都对数理统计学的性质，任务，应用等做了详细的描述，这些在业界没有问题，大家都比较认可，但是如果试图用简单的语言正式定义数理统计学，那么就会引起不少麻烦，因为少量语言没办法准确的描述，这时候你无论落下谁的观点，都会被打击，所以定义什么是数理统计的定义，这里没有，当然如果你在哪本神书中看到相关的定义，请仔细揣摩，别被忽悠，当然，也别背，很有肯能记住一个错的定义。本文就是想通过描述一些数理统计的实质内容，来告诉大家我们研究来研究去，数理统计到底是个啥东西（至于定义是“专家”们喜欢卖弄的，我们不研究）
### 研究内容
研究一件事简单的说可以用两种方法，一种是研究其所有前提，推导出结论，也就是正向推导，另一种方法是观察实验结果，来反推原理。
说个真事，应该是我小学五年级或者初一的时候，在学三角的时候，我觉得三角形角和边之间一定存在着某种关系，于是我用量角器，尺子开始在纸上画三角形，量数据，当时我记得似乎已经知道固定其中若干个变量，然后只改变一个或者两个变量，来统计结果，具体做法可能忘了，而且不可能算出余弦定理，因为我连sin和cos都没学，都十几年前的事了，这个思路就是观察结果，推原理的过程。
在观察和实验的方式下研究问题，有固定的几个步骤：
第一步：通过观察或实验收集必要的数据（小谭年轻的时候画三角，量某边（角）的长度（度数））
第二步：对采集的数据进行分析，对研究的问题作出某种形式的推论（小谭的方法是找规律，某种形式的推论就是角的大小和边之间的关系）
这两步有数学问题，也有别的问题，比如画三角形就是绘画问题。我们关注的是数学问题，为了解决这些数学问题发展起来的理论和方法就构成了数理统计学的内容。数理统计学是数学分支是没问题的，其研究怎么用有效的方法去收集和使用带有随机性影响的数据。
#### 数据必须带有随机性
数据没有随机性，其不能成为数理统计的研究对象，比如我们记录了去年每一天的花销（假定全部正确无误，没有任何随机因素在其中），然后研究平均每天花了多少，这个问题就不是数理统计问题，这时候求平均值问题，数据是准确无误的，所以没有随机性。区别数理统计方法和其他数学方法的办法就是确定数据是否有随机性。
作为对比，确定一批产品的合格率，从一批产品中抽样，得到 $N$ 个产品，其中 $m$ 个废品，废品率是 $p=\frac{m}{N}$ 这个就有随机性在里面，因为抽样会得到随机结果，因为抽到哪个产品是偶然的。
这是我们观察的时候得到的随机因素，在实验过程中随机性更多，比如我们在进行物理化学实验的时候，每一个操作都有可能带入误差，比如控制温度和气压，混入一定量溶液，这些过程都有随机误差在里面，在不同的条件以及误差的影响下，我们得到两个结论 $t_1,t_2$ 其中 $t_1$ 优于 $t_2$ 那么我们是否能说 $t_1$ 优于 $t_2$ 的原因是因为不同的条件，还是误差也起到了至关重要的作用。这也要用数理统计的方法进行分析。
#### 有效的方式收集数据
有效地才是重要的，理解有效的可以从两个方面：
1. 这组数据是数学上可以处理的，或者用我们可以接受的模型来处理的，太过复杂或者复杂到不可理解的数据，对于我们来说等同于无效
2. 数据重要尽可能的包含研究问题的有关信息

举个例子，调查某地区10000个家庭的收入情况，理论上我们不能每家每户都调查，不现实切不实际，所以我们想要抽取一部分样本，比如100户，那么有效性就要得到保证了，就是这个样本大小100是否能给我们足够的信息。以及这100户如何选取才能保证有效性，比如我们调的一百户都是什么比尔盖茨家族，摩根家族之类的，这个就扯淡了，或者一百户都是没有劳动能力的低收入家庭，这些数据对于我们研究整个地区所包含的信息太少，或者说不全面，这是无效的，所以如何挑选这100户也变得非常重要。一种有效地方法是随机选取，前提是保证每一户被抽取的可能性相同，但是这要求样本应该要大一些，而100够不够是个问题，另一种就是先分类，分成高收入家庭，和低收入家庭，按比例从两个类中随机挑选不同的样本。后一种方法我们认为比前一种随机挑选要有效。
在设计实验时要考虑可实施的性质，比如在一个产品实验中我们考虑温度和压强对产品质量的影响，我们可能需要实验来测试，如果我们选择4个温度和4个压强，就要进行16次试验，这可能还是可以接受的，但是如果我们要考察三个元素，而且每个元素可能的取值不止四个，那么这个试验次数基本就是不可接受的，所以选择哪些元素，每个元素取哪些值，这个都是需要谨慎思考和研究的，并且需要充分的使用学科背景知识，尽可能的挑选对问题有实质性反应的信息。

上面两个例子并不是我随便说的，而是陈老师书上写的，并且这两个问题对应于统计学中两个重要分支：
- 抽样理论
- 实验设计(试验设计)

这两个分支对应于上面例子的那个系列问题的研究。
#### 有效地使用数据
当我们得到数据了以后就要使用数据了，我们收集数据的目的不是为了收集数据然后存储起来，我们收集数据的目的是解决一个相对应的问题的，这就使得使用数据需要更加谨慎，因为通过使用数据我们要得出问题的结论，这个结论我们在统计上称之为 “**推断**” ，强调一点，我们手机来的有效地数据直观上不可能看出我们问题的答案，如果能看出来，那么这就不是一个统计学问题，而是一个看数字的结论的问题。我们要做的是在有效的数据上，使用某种有效地方法，尽可能的提炼数据中能够解答问题的有关信息。尽可能是指我们不可能完全提炼出所有的有关信息，因为这些数据（信息）在收集的时候包含随机性，随机的添加无用信息，随机的丢掉有用信息，我们的数理统计方法就是要尽可能的缩小这些随机错误对我们结果的干扰，但是不可能完全消除。
关于“**推断**”的相关问题我们会在后面连续讨论，并且这也是数理统计中的一个核心问题。
接下来我们要说“**模型**”，但是并没有严格的定义这个概念，我们就把它当做一个算法或者一套算法来对待，后面会有严格的定义：
为有效地使用数据进行统计推断，涉及不少数学问题，我们需要建立一个数学模型，并且制定某些准则，才能根据模型和准则得出这个统计方法的优劣；比如对于某一组数据，我们用A，B两个模型进行建模，考虑他们的性质 $n$ 并且理论上来将， $n$ 越大越好，在这个对比中 $n_A > n_B$ 那么我们就说在某种衡量标准下，A模型优于B模型。
举个例子，称重，我们称一个物体9次，得到9个值 $x_0,\cdots,x_8$ ，每个值都不同，因为称量，观察都有误差被引进，那么我们建立以下三种模型来降低误差干扰：
1. 使用平局值 $\overline{x}=\frac{1}{9}\sum_{i=0}^{8}x_i$ 作为物体的重量
2. 将这些测量值排序： $x^{0}\leq \cdots x^{8}$ 然后取中间值 $x^{5}$ 作为物体重量
3. 依旧使用2中的排序结果，而将 $\frac{x^{0}+x^{8}}{2}$ 作为物体重量

这就是3个不同的模型，但是哪种好可能每个人都有不同的看法，比如你可能认为1中均值比较好，也有可能认为2中的中位数好，当然3也可以好，但是每一个在什么条件下最优是不一样的，在什么条件下，为什么最优成为数理统计学的中心内容，这个研究过程需要使用大量的数学，概率的工具，在后面我们看到在不同的随机（随机性的概率结构，或者统计模型）影响下，在不同的衡量指标下，都可以作为此条件下的最优结果。

#### 数理统计是数学问题
一直以来不少人都想说数理统计学不属于数学分支，当然我也不知道他们想把概率弄去哪个分支，不会被加入到CS中去吧😆 。但是数理统计只处理在收集和使用过程中带随机性影响的数据中的数学问题，所以，他就是一个数学分支。
接下来我们说说数理统计学研究的对象是什么
简单来说是数据，数理统计学的应用不局限于数学各学科，物理化学生物计算机航天等，只要你能根据你们学科的知识，设计有效地试验，并且采集到有效地数据，给出有效地模型。
接下来数理统计学就可以在不知道任何背景下，通过数据和模型对问题进行分析和研究，并且这步是完全数学的——这就是数理统计学研究的对象，这些超脱于试验的数据和给定的模型。
一个数理统计学者可以不问相关专业的知识，但是这有非常多的坏处。因为只有对问题有深入的研究才能帮助提出有效的模型以及有效收集数据的方法。
比如一个遗传学方面的学者很了解数理统计，那么会对其研究有非常大的帮助，相反一个数理统计学者不了解遗传学，可以肯定他不会在遗传方面有什么成就。
数理统计的方法的使用不需要高深的数理统计学理论知识，这也使得统计能够被很多行业的研究者从业者使用，而他们所使用的准确的是时统计学的方法，或者叫做应用统计，这些并不需要他们有很多的数理统计知识。美国将统计学中涉及数学基础的部分叫做数理统计学，这是狭义的定义，我们国家是为了区分和社会学科的统计学，才有了数理统计这个名字，所以我们的数理统计不光有数学基础，还有应用，这相当于美国等国的统计学。

## 数理统计学的应用
应用有很多，我们说多了啰嗦，说少了有没啥意思，简单的说，人类所有活动里都有数理统计，这么说一点不过分。
农业上某个指标对产量的额影响，这个问题可以用数理统计解决。
工业上，同样的在某个生产过程，某个因素对产量质量的影响，也要考数理统计分析。
天气预报，金融，地质，社会学，等也有很多应用。

## 总结
本文是第一篇，应该也是非常非常重要的一篇，希望大家不止局限于本文，而是要查阅相关更多的书籍资料，博客和别人的讲述最多只能给你一个骨架，让你不会跑偏，吃肉还是要去看书。