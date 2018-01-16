---
title: 【线性代数】1-0:Vector
toc: true
categories:
  - Mathematic
  - Linear Algebra
date: 2017-08-28 10:01:20
tags:
  - Vector
  - 向量
---
**Abstract:** 本文主要介绍向量和线性组合的相关知识
**Keywords:** 向量，线性组合
<!--more-->



## 开篇废话
有人说，你每篇博客开始都是废话，累不累。。。其实吧，废话只是说明和本文关系不大，读者可以直接跳过，但是废话里面也有一些有点用的信息，读一读说不定也有点小收获。
Linear Algebra接下来一段时间的博客主要依据Gilbert Strang教授的《introduction to linear algebra，4th》来进行，国内买不到，准备去国外淘一本，虽然国外的有点贵，但是支持正版，也是对作者的尊重，目前淘宝一本印刷版，先进行总结配合做一些练习。

## The Heart of Linear Algebra
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
## Linear Combinations
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
