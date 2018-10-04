---
title: 【Julia】Julia编程语言介绍
categories:
    - Julia
    - Freshman
tags:
    - Julia
keywords:
    - Julia编程语言
    - Julia简介
    - Julia特点
toc: true
date: 2018-09-25 12:04:01
---

**Abstract:本文是介绍Julia编程语言系列博客的第一篇，本系列用短小的博客介绍Julia编程的基础知识**
**Keywords:Julia编程语言,Julia简介,Julia特点**

<!--more-->
# Julia 编程语言
## Julia 简介
Julia语言是MIT的几个科学家，觉得Matlab还有Python不太适合自己的行业，所以自己搞出来的一种编程语言，和另外几百中编程语言一样，其有独特的受众，那就是 —— 数据科学。Julia是小众的，其诞生以来的基本目标就是，能像C语言编写的程序一样快，但是又要有Ruby一样的动态性。Julia主要的目标用户是数据科学家，统计学习，机器学习从业者等。
### Julia 与 Python
平时的工作，基本都是跟TB，PB级别的数据打交道，所以，Python速度从一开始就受到诟病，虽然你可以让python结合c语言来提速，但是对于特异性很高，没有现有库支持的任务，那种工作量和直接写c语言差不多。
而且，Julia程序及其好写，我个人觉得Python的面向对象的机制对于数据科学家来说，基本没什么用，我们更多的是按照某个流程处理全部数据，而不需要用到类；
由于Python通用性，对于数据科学家来说，干点什么事都需要import一大堆各种各样的包，读个csv都要先import点什么。但是Julia针对数据处理，所以很多包都内置了，相对于Python会简洁很多。
Python用途更广泛，而Julia面向数据处理，所以这些差异都在情理之中。
### Julia 与 c++
当然C++的特性基本更用不到了，如果使用C++没有用到多态和继承，那么就相当于在写c语言，而数据科学，想来想去，也不太会用到多态和继承。
### Julia 与 R
至于R语言，个人不太了解，但是Julia肯定比他快。
Julia可以在使用过程中轻松的集成R或者Python等语言编写的库，这个可以大大的提高工作效率。

## Julia 有什么特点
Julia的特点很多，但是主要概括如下，如果你的工作需要这些特点，那么Julia是你的好选择。
1. 卓越的性能
    - 某些任务速度堪比C语言
2. 强大的基础库
    - 内置线性代数运算，高效
3. 支持分派
    - 同一个函数可以实现不同的过程(比c++的多态更简单)
4. 容易上手
    - Julia语言非常简单，学过c语言的同学可以在24小时内上手
5. 用户友好的界面
    - 本地还是远程的Julia用户界面都很好用
6. 与其他语言对接
    - 无缝拼接，R，Python和C
7. 开源
    - 所有文档都可以找到
8. 开发者承诺
    - 不会开发到半路然后跑路
9. 自定义函数的性能
    - 自己写的函数也很快，不会只有内置的函数快，而自定义的速度慢
10. 并行
    - 轻松的并行，数据科学最需要的就是并行，而Python真的不太好用
11. 灵活性
    - 开发程序灵活，可以找到各种问题的方案解决

## 为什么选择Julia
如果你是跟数据打交道的，从事机器学习，数据处理类的工作，那么Julia的高效，并行，绝对是你的不二之选。
活跃的社区和丰富的功能，持续的更新，也是这门年轻的语言将会成功的先兆。
最主要的，就是效率，减少一分钟的运行时间，对于有些项目，可能是至关重要的，对于处理数据，我们需要提高的就是：效率，效率，还是效率
## 总结
本文介绍了Julia的基本性质，如果Julia适合你，那么快加入吧。

## 引用
1. Voulgaris Z. Julia for Data Science[M]. Technics Publications, 2016.
2. [https://en.wikipedia.org/wiki/Julia_(programming_language)](https://en.wikipedia.org/wiki/Julia_(programming_language))
3. [https://julialang.org](https://julialang.org)



[完整文章访问原文地址:https://face2ai.com/Julia-Lang-0-Introduction](https://face2ai.com/Julia-Lang-0-Introduction)
