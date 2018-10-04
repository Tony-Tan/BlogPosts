---
title: 【数理统计学简史】2.2 狄莫弗的初步结果
categories:
    - Mathematic
    - Statistics
keywords:
    - 二项分布
    - 斯特林公式
toc: true
date: 2018-05-03 17:14:58
---

**Abstract:** 本文介绍狄莫弗上一篇之后得出的关于二项分布的近似计算
**Keywords:** 二项分布，斯特林公式

<!--more-->
# 狄莫弗的初步结果
狄莫弗接着前面写的，狄莫弗继续他的研究，二项分布，三个参数，使用函数的写法 $b(N,p,i)$ ,三个变量明显研究起来困难，所以控制变量，只研究一个参数 $b(2m,\frac{1}{2},m)$ 然后再研究中心项与任意一项的比，也就是 $\frac{b(m)}{b(m+d)}$ 这里的 $b$ 进行了简化，把三个参数变成了一个参数，也就是 $b(2m,\frac{1}{2},m)$
狄莫弗在 <font color="006600">1721年</font>得到下面的结果，当 $N\to \infty$ 时有：
$$
b(m)\sim 2.168\frac{(1-\frac{1}{N})^N}{\sqrt{N-1}}\tag{5}
$$
$$
\text{log}(\frac{b(m)}{b(m+d)})\sim (m+d-\frac{1}{2})\text{log}(m+d-1)\\
+(m-d+\frac{1}{2})\text{log}(m-d+1)-2m\text{log}m+\text{log}(\frac{m+d}{m})\tag{6}
$$
狄莫弗的证明在《统计学简史》的书中p44，有狄莫弗的证明，虽然这个结论至今没什么用了，但是证明过程相当有用，因为我们这个系列的目的是让大家了解数理统计的发展，但是有些喜欢细节的同学们可以去查下原文。
（5）的结论现在我们已经在书上见不到了，因为我们有更好的近似结论了，而且(5)对狄莫弗后续研究也没啥作用，
另外（6）中的 $d$ 可以随 $N$ 变化，有一定限制，在斯特林公式下，可以明确的看到在极限情况下 $\frac{d}{N}\to 0$ 。虽然狄莫弗没有给出这个条件，但是他在证明的时候也没有违反这个条件。
## 总结
本文虽短，但是狄莫弗的研究进入到了关键阶段，后面与斯特林沟通后，更好的结果出现了。





原文地址1：[https://www.face2ai.com/Math-Statistics-2-2-De-Moivre-Naive-Result](https://www.face2ai.com/Math-Statistics-2-2-De-Moivre-Naive-Result)转载请标明出处
