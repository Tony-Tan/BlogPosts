---
title: \[线性代数\]1-0:向量(Vector)
toc: true
categories:
  - Mathematic
  - Linear Algebra
date: 2017-08-28 10:01:20
keywords:
  - Vector
  - 向量
---
**Abstract:** 本文主要介绍向量和线性组合的相关知识
**Keywords:** 向量，线性组合
<!--more-->



# 向量
## 线性代数的核心：向量(The Heart of Linear Algebra)
很少有这么开门见山的教材，第一章第一句话就是告诉你，“嘿小子！凭借老夫多年经验线性代数的核心，就是***两种计算***，而且这两种计算都是对 ***向量*** 进行的！”
（插播一句，知乎上有人说看这个公开课对考研用处不大，所以要根据这个考试的人，请你走开了，这里不适合你）。
### 加法
$$
\textbf{v}+\textbf{w}
$$
上式就是两个向量相加，注意 $ \textbf{v} $这种画风的是向量，$ c $这种的是常量，向量和常量都是啥这个我就不介绍了。
### 乘法
$$
c\textbf{v}
$$
这种乘法，不是向量乘以向量，这个是一个常数乘以向量，英文也叫scalar multiplication。
## 线性组合(Linear Combinations)
有了上面两种根基，我们就发展出了一个超级无敌牛的组合：
$$
c\textbf{v}+d\textbf{w}\\
$$
Suppose:
$$
v=\begin{bmatrix} 1\\1 \end{bmatrix}\\
w=\begin{bmatrix} 2\\3 \end{bmatrix}
$$
so
当c=2，d=1的时候，我们可以根据乘法和向量加法原则，先乘法后加法，得到
$$
\begin{bmatrix} 4\\5 \end{bmatrix}
$$
这是当c，d确定的时候，我们可以用两个向量来组合出另一个向量
## The Whole Plane
当v和w不在一条直线上的时候（我默认你知道向量在坐标系里的表示），通过调整c和d，我们可以得到二维平面上的任一一个向量，这个是线性代数最核心的概念，对于一维空间，线性组合的图形表现是一条直线，二维是一个plane，三维或更多维度下就是空间。

## 总结
给出线性代数的核心，线性组合（通过我们知识点图谱也能看出这一点，Linear Combination在树的根部）





原文地址1：[https://www.face2ai.com/Math-Linear-Algebra-Chapter-1-0](https://www.face2ai.com/Math-Linear-Algebra-Chapter-1-0)转载请标明出处
