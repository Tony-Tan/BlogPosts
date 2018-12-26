---
title: 【Hexo】Hexo下next主题畅言评论同步问题的解决方案
categories:
  - Other
tags:
  - Hexo
  - Next
  - 畅言
toc: true
date: 2018-04-02 20:36:17
---

**Abstract:** 本文介绍如何解决畅言在Hexo下无法同步移动端和PC端评论的问题
**Keywords:** Hexo，next，畅言

<!--more-->
# 解决畅言评论在移动端和PC端同步的问题
移动端的兴起使得互联网信息上的信息被告诉交换，随着博客的不断增多，评论也有若干个了，之前有个问题一直没解决就是pc端的评论移动端看不到，移动端的评论PC端也看不到，今天到了解决问题的时刻
## 思路
首先我的文章生成的地址都是这个样子的：
[http://www.face2ai.com/Math-Set-Theory-1-Sample-Sets/](http://www.face2ai.com/Math-Set-Theory-1-Sample-Sets/)
默认情况下，next是吧url当做文章的标识，但是在移动端，或者有些情况下，url后面会增加某些参数，这样畅言后台就把他当做两篇文章了，所以我们根据畅言的帮助文档：
[帮助中心SourceID](http://changyan.kuaizhan.com/static/help/f-source-id.html)的提示下，搜到了这篇仁兄的博文：
[https://segmentfault.com/a/1190000008091729](https://segmentfault.com/a/1190000008091729)
不过由于他的博客比较早，不适用于我们现在的next，我们进行如下修改
```shell
/hexo/themes/next/layout/_partials/comments.swig
```
其中
```html
<div class="comments" id="comments">
  <div id="SOHUCS"></div>
</div>
```
修改为:
```html
<div class="comments" id="comments">
  <div id="SOHUCS" sid="{{ page.title }}"></div>
</div>
```
这样畅言就会根据文章标题判断是不是一篇文章了，这样我们只要确保网站内没有同名文章就好了。
## 总结
这样我们就能看到来自不同平台的评论了。





原文地址：[https://www.face2ai.com/other-Hexo-next-changyan](https://www.face2ai.com/other-Hexo-next-changyan)转载请标明出处
