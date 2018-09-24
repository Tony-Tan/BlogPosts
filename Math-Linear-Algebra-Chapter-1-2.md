---
title: \[线性代数\]1-2:点乘和长度(Dot Products and Length)
toc: true
categories:
  - Mathematic
  - Linear Algebra
date: 2017-08-28 12:12:18
keywords:
  - Length
  - 长度
  - Dot Product
  - 点乘
---
**Abstract:** 介绍点乘，length
**Keywords:** dot product，length
<!--more-->


# 点乘和长度
## 点乘
点乘，也就是说向量乘法不止一种，我们今天来介绍的是比较常用的点乘，出了乘法，其实里面还有加法：
定义
dot product or inner product of $\textbf{v}$ and $\textbf{w}$
That is
$$
\textbf{v}=\begin{bmatrix} v_1\\v_2 \end{bmatrix}\\
\textbf{w}=\begin{bmatrix} w_1\\w_2 \end{bmatrix}\\
$$

$$
\textbf{v}\cdot \textbf{w} = v_1 \times w_1+v_2 \times w_2
$$

v和w和互换位置，也就是交换律存在。对于多维向量
点乘的结果是：

$$
\textbf{v}\cdot\textbf{w}=\sum_{i=1}^{n}v_i*w_i
$$
## 几何理解

其实代数是完全不需要借助几何图形，也能够非常严谨完备证明所有之间的内在关系，但是将代数与几何相互结合的情况下，可以比较轻松的解决一些几何问题，通过简单的数字计算
### 长度
长度，向量是有长度有方向的，方向，我们按照坐标起点和终点来确定的，长度也是，按照二维平面，两点之间距离（更准确的说是笛卡尔坐标系下，利用勾股定理来计算的距离）
比如 $(0,0)$ 到 $(v_1,v_2)$ 的距离 $|\textbf{v}|=\sqrt{v_1^2+v_2^2}$
so
$|\textbf{v}|^2=v_1^2+v_2^2$
眼熟不？厉害不？意外不？
没错就是$\textbf{v}\cdot\textbf{v}$

自己和自己的点乘结果就是向量长度的平方。
### 单位长度向量(Unit Vector)
长度为1的向量，获得方法，非零向量，所有分量除以自己的长度
### Angles
#### 90°
点乘结果为0的时候，两个向量夹角为直角，证明：

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-1-2/90.png)

#### 其他角度
后面我们使用到矩阵以后，这个夹角基本没用，但是单位长度的向量相乘，其结果是他们夹角的cos值

## Conclusion
这一章讲了线性代数的核心，也就是我们知识树的根基算是讲完了，然后顺着根不断的遍历，这样先后顺序能使知识贯通，顺便吐个槽，我tm就不明白了，为啥上学的时候老师上来就干行列式，干了三个星期，直接干迷糊了，所以，我劝大家，看线性代数的书，如果前三章就有行列式了，这书就不用看了！





原文地址1：[https://www.face2ai.com/Math-Linear-Algebra-Chapter-1-2](https://www.face2ai.com/Math-Linear-Algebra-Chapter-1-2)转载请标明出处
