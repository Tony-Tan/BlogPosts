---
title: 【线性代数】2-2:消元(Eliminate)
toc: true
categories:
  - Mathematic
  - Linear Algebra
date: 2017-08-31 16:14:14
tags:
  - 消元
  - 主值
  - 行变换
  - 上三角矩阵
---
**Abstract:** 从小学解方程的消元开始，推导出线性代数的知识，包括先关矩阵计算
**Keywords:** Eliminate 消元，Pivot 主元，Row Exchange 行交换，Upper Triangular 上三角矩阵
<!--more-->



# 消元
小学老师教我们解方程，受限就是把两个未知数变换成一个未知数，或者说用另一个未知数来表示当前未知数。

$$
x+y=1\\
x-y=0\\
$$
我们会把第一个方程变形，然后和第一个方程做减法或者加法计算
$$
-x-y=-1........(1_{temp})
$$
左右同时和(2)相加
$$
-2y=-1.........(2)
$$
当这步完成时，第一步方程没变，我们减去的变形版本是个中间版本，所以方程组：
$$
x+y=1\\
0x+y=\frac{1}{2}
$$
消元的顺序很多，这个只是我的习惯，不过和线性代数书上刚好差不多，经过消元，我们得到了Upper Triangular Matrix $A=\begin{bmatrix}1&1\\0&1 \end{bmatrix}$
## 上三角矩阵(Upper Triangular Matrix)
只有对角线，及对角线上方数字不为0，其他部分都是0的矩阵
$$
\left(\begin{array}{cccc}
a_{11} & a_{12} & \cdots & a_{1n} \\
       &a_{22}  & \cdots &a_{2n}  \\
       &        & \ddots & \vdots \\
       &        &        &a_{nn}
\end{array}\right)
$$
没错，a不能全为0
消元的最后终结果就是上一篇中的 $A\textbf{x}=\textbf{b}$ 变成了
$$
U\textbf{x}=\textbf{c}
$$
## 带回(Back Substitution)
从最后一行开始，把未知数一个个回带解出来
## 主值(Pivot)
消元过程中，我们要看列的未知数，和未知数的系数，策略就是首先干掉第一行一下的所有的第一个未知数，通过对第一行进行缩放，来得到中间（temp）表达式，使得第一行下面的所有行的第一个未知数全部消失，然后对第二行做类似的操作，消掉第二行以下的所有第二个未知数。最后是系数矩阵白城上三角矩阵。
那么问题来了，有的时候，你消元到第二行的时候，第二个未知数在刚才的消元过程中牺牲了，顺带也给干掉了，很不幸，我们这时候为了得到标准的上三角矩阵，肯定要用其他行（第一行不行）的方程和第二行的方程进行置换，来保证第二个未知数存在。

***在消元过程中，系数矩阵每行中第一个不为0的数字叫主元（pivot）***
消元后，主元在主对角线上。
eg：
上面upper triangular matrix的对角线元素就是主元。

***注意：主元不等于零，如果一行全是0，那么没有主元。***
消元过程中当前行乘以某个系数得到temp过程中，系数
$$ l_{ij}=\frac{entry\,to\,eliminate\,in\,row\,i}{pivot\,in\,row\,j} $$
Pivot in Row j 就是当前的主元

如果主元个数小于未知数个数，又会引出一个新的概念，***singular***后面再说
## 消元失败(Eliminate Faile)
消元失败，我们记得解方程会有两种特殊情况，一种是无数个解，一种是没有解。
比如你有三个方程，两个未知数，就很有可能没有解
如果你有一个方程，两个未知数，肯定有英菲尼迪个解
但是这个描述是小学的，根据back substitution的过程，我们确定，方程解的个数和pivot直接相关：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Math-Linear-Algebra-Chapter-2-2/eliminate_faile.png)
## 总结
消元那步看不懂，就想象着把矩阵变成三角的，然后按照这个思路自己写个方程试试，过程中感受下主元的威力，主元是个非常重要的概念，用处特别多。。。





原文地址1：[https://www.face2ai.com/Math-Linear-Algebra-Chapter-2-2](https://www.face2ai.com/Math-Linear-Algebra-Chapter-2-2)转载请标明出处
