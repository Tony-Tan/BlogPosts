---
title: 【线性代数】3-6:四个子空间的维度(Dimensions of the Four Subspaces)
categories:
  - Mathematic
  - Linear Algebra
keywords:
  - Four Subspaces
toc: true
date: 2017-09-25 15:21:01
---

**Abstract:** 四个向量空间的dimensions的一些性质
**Keywords:** Dimensions,Four Subspaces

<!--more-->
# 开篇废话
这几天在一边完成线性代数的博客一边研究微积分，还有一部分时间在看概率论，这写课程现在感觉看起来很容易，也有可能是公开课讲的比较简单，没有大学本科老师讲的那么深入，所以做起来感觉还没什么阻力，希望花这么多时间补习的结果能帮助后面对机器学习算法和人工智能知识学习有所帮助。
痛苦一直在持续，因为我们没有看到光明之前，放弃努力就等于放弃光明，等待和虚度不会得到任何你想要的东西，继续学习，继续努力。
# Four Subspaces
本篇应该不长，因为关于矩阵的四个子空间的难点在后面所有章节，我们这张主要讲线性独立，基，维度这些基本概念，同时联系到rank，融合前面elimination的基本知识。
上面这句话如果不看前面的文章基本被干晕了，但是如果前面每篇都完全读懂了，这句话就变得非常好理解。
线性代数核心问题就是这四个子空间：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-3-6/bigpicture.png)
矩阵的四个子空间分别是：column space ，row space ，nullspace，left nullspace，
左nullspace就是矩阵转置的nullspace：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-3-6/fourSpace.png)

对于行空间和列空间，他们的dimensions就是rank，上一篇已经有所说明。
nullspace就是free columns的数量n-r，left nullspace就是free rows的数量m-r
## for R
R是经过消元的矩阵，可以很容易的看出矩阵的rank，假设矩阵A是m by n的规模：
1) $N(R)$ 的dimensions是n-r
2) $C(R)$ 的dimensions是r
3) $C(R^T)$ 的dimensions是m-r
## for A
对于没elimination的矩阵A与R有相同的row space，但是column space不同，但是elimination改变rows，但是row space不变。column space不同，但是column space的dimension相同都是rank。对于A矩阵的子空间的dimension总结如下

>1) A has the same row space as R.Same dimension r and same basis
>2) The column space of A has dimension r.For every matrix this is essential.
>3) A has the same nullspace as R.Same dimension n-r and same basis
>4) The left nullspace of A(the nullspace of$A^T$) has dimension m-r

***Fundamental Theorem of Linear Algebra ,Part 1***
The column space and row space both have dimension r
The nullspaces have dimensions n-r and m-r

凡是叫fundamental开头的定理都是非常非常重要。
# conclusion
总结这篇就是简单的总结下A和R的dimensions之间的关系，没有新的知识更像是个总结：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-3-6/conclusion.png)
下章开始研究总结正交，各位加油。





原文地址1：[https://www.face2ai.com/Math-Linear-Algebra-Chapter-3-6](https://www.face2ai.com/Math-Linear-Algebra-Chapter-3-6)转载请标明出处
