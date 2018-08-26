---
title: 【线性代数】1-1:线性组合(Linear Combinations)
toc: true
categories:
  - Mathematic
  - Linear Algebra
date: 2017-08-28 10:44:28
keywords:
  - Linear Combinations
  - 线性组合
---
**Abstract:** 线性组合详细说明
**Keywords:** Linear Combinations
<!--more-->



## 列向量
上文我们简单的看了一眼核心，核心也是最简单的东西，在我国，初中高中的小盆友们就应该已经知道向量加减法了，但是美国的小朋友们可能到高中大学才接触，所以书中给出了详细的加减乘除算法，我们必须明确一点，一般说道的向量和写出来的都是列向量，就是竖着的
like this one：
$$
\begin{bmatrix} 4\\5 \end{bmatrix}
$$

## 向量加法和乘法计算
这里简单写一下加法和乘法计算

*VECTOR ADDITION*:
$$
\textbf{v}=\begin{bmatrix} v_1\\v_2 \end{bmatrix}\\
\textbf{w}=\begin{bmatrix} w_1\\w_2 \end{bmatrix}\\
$$
add to:
$$
\textbf{v}+\textbf{w}=\begin{bmatrix} v_1+w_1\\v_2+w_2 \end{bmatrix}\\
$$

*VECTOR MULTIPLICATION*:
$$
2\textbf{v}=\begin{bmatrix} 2v_1\\2v_2 \end{bmatrix}\\
-\textbf{v}=\begin{bmatrix} -v_1\newline -v_2 \end{bmatrix}\\
$$
（写公式真累！！）
注意零向量和数字常量0的不同
## 线性组合
$$
c\textbf{v}+d\textbf{w}\\
$$
就是线性代数的基础

**定义：**
**the sum of $c\textbf{v}$ and $d\textbf{w}$ is a linear combination of $\textbf{v}$ and $\textbf{w}$**

## 向量的表示
这个大家都会，画箭头嘛，从0点，画向坐标位置，标个箭头就okay了
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-1-1/加法.png)
## Important Questions
Suppose
$\textbf{u}$ $\textbf{v}$ $\textbf{w}$ are three-dimensional non-zero:

$c\textbf{u}$ fill a Linear

$c\textbf{u}+d\textbf{v}$ fill a plane

$c\textbf{u}+d\textbf{v}+e\textbf{w}$ fill a space(3d)

前提是这三个向量不在同一直线或同一个二维平面上，上面的三个important才成立！

## 总结
此篇详细说明了线性组合的一些基本问题
