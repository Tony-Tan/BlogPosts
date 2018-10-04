---
title: 【信息论、推理与学习算法】本系列博客介绍
categories:
  - Information Theory
keywords:
  - Information Theory
  - 信息论
  - Inference
  - 推理
  - Learning Algorithms
  - 算法
toc: true
date: 2018-09-21 11:53:59
---

**Abstract:** 本文是本系列的第一篇，介绍本系列的主要内容
**Keywords:** Information Theory，信息论，Inference，推理，Learning Algorithms，学习算法

<!--more-->
# 信息论、推理与学习算法介绍
这个系列是信息论相关内容的介绍，信息论是什么可能有些做机器学习或者AI的同学们不太了解，而做通信的同学应该是非常清楚的，如何准确的定义信息论是什么，不在我的能力范围内，但是我们平时接触到的图像，或者简单点说灰度图像，一个8bit的像素点能有多少阶灰度，为什么有256，而不是258或者其他的，这个其实就属于信息论的知识范围，而信息论和机器学习有关系么？答案是肯定的，凡是处理信息，传递信息的过程，都多多少少跟信息论有那么点关系。
本系列主要面对的读者是： **从事机器学习，人工智能类内容研究的同学，工程师，或者爱好者**
需要的背景知识：工程专业，科学类专业，或者数学类本科1，2年级的数学知识，包括，微积分，概率论，线性代数的基本知识（本站已经完成这些基础知识的全部博客，可以随时查阅）。
本书封面：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/ITILA-Introduction-to-This-Series/cover.jpg)
## 机器学习，信息论
传统的信息论课程不仅包括Shannon的信息化思想，也有实际解决问题的现实应用，我们这个系列更加进一步的包括了：
- Bayesian Data Modelling
- Monte Carlo Methods
- Variational Methods
- Clustering ALgorithms
- Neural Networks

为什么要把信息论和机器学习弄到一起？
信息论和机器学习是一个硬币的两面！
60年代一个领域 —— 控制理论（cybernetics）在信息论，计算机科学，和神经科学等学科中非常火爆，这些科学家们都在研究一个相同的问题，那时候信息论和机器学习还是属于同一类。大脑是一个压缩信息，进行沟通的系统，而在数据压缩（data conpression）和纠错码上（error-correcting code）表现最好的（state-of-the-art）的算法上使用的工具，在机器学习中也会使用。
这些种种迹象都表明，机器学习和信息论有着密切的关联，而我们本系列更关注的就是信息论在机器学习方面的应用，或者帮助我们理解一些算法的特点和局限。
## 学习地图
本书的目录如下,当然这些课不是我们所有要学的，我画了个地图，大概应该是按照这个地图来完成我们的博客的：
### Preface
1 Introduction to Information Theory
2 Probability, Entropy, and Inference
3 More about Inference

### I Data Compression
4 The Source Coding Theorem
5 Symbol Codes
6 Stream Codes
7 Codes for Integers

### II Noisy-Channel Coding
8 Dependent Random Variables
9 Communication over a Noisy Channel
10 The Noisy-Channel Coding Theorem
11 Error-Correcting Codes and Real Channels

### III Further Topics in Information Theory
12 Hash Codes: Codes for Ecient Information Retrieval
13 Binary Codes
14 Very Good Linear Codes Exist
15 Further Exercises on Information Theory
16 Message Passing
17 Communication over Constrained Noiseless Channels
18 Crosswords and Codebreaking
19 Why have Sex? Information Acquisition and Evolution

### IV Probabilities and Inference
20 An Example Inference Task: Clustering
21 Exact Inference by Complete Enumeration
22 Maximum Likelihood and Clustering
23 Useful Probability Distributions
24 Exact Marginalization
25 Exact Marginalization in Trellises
26 Exact Marginalization in Graphs
27 Laplace's Method
28 Model Comparison and Occam's Razor
29 Monte Carlo Methods
30 Ecient Monte Carlo Methods
31 Ising Models
32 Exact Monte Carlo Sampling
33 Variational Methods
34 Independent Component Analysis and Latent Variable Modelling
35 Random Inference Topics
36 Decision Theory
37 Bayesian Inference and Sampling Theory

### V Neural networks
38 Introduction to Neural Networks
39 The Single Neuron as a Classier
40 Capacity of a Single Neuron
41 Learning as Inference
42 Hopeld Networks
43 Boltzmann Machines
44 Supervised Learning in Multilayer Networks
45 Gaussian Processes
46 Deconvolution

### VI Sparse Graph Codes
47 Low-Density Parity-Check Codes
48 Convolutional Codes and Turbo Codes
49 Repeat{Accumulate Codes
50 Digital Fountain Codes

### 地图

根据我们的目标和书上给出的建议，我们要学习下面这些章节，箭头之间表示先后关系，箭头指向的课程需要在前面的课程完成后才能进行：
![](https://raw.githubusercontent.com/Tony-Tan/MachineLearningMath/master/information.png)

github: [https://github.com/Tony-Tan/MachineLearningMath](https://github.com/Tony-Tan/MachineLearningMath) 上有高清大图

## 总结
本文是信息论的第一课，后续就围绕上图展开，对于基础不是了解的同学可以去看前面的其他博客，谢谢支持。




原文地址1：[https://www.face2ai.com/ITILA-Introduction-to-This-Series](https://www.face2ai.com/ITILA-Introduction-to-This-Series)转载请标明出处
