---
title: 【Julia】变量
categories:
    - Julia
    - Freshman
tags:
    - Julia
    - Julia变量
    - Julia变量名
    - Julia命名一般规则
toc: true
date: 2018-10-02 09:13:40
---

**Abstract:** 本文介绍Julia变量的相关内容，包括Julia的变量命名，以及命名的惯用法则（命名风格）
**Keywords:** Julia变量，Julia变量名，Julia命名一般规则
<!--more-->
# Julia 变量
如果Julia不是你的入门编程语言，那么对于编程语言中的变量应该已经习以为常，在C语言中，变量名对应于一个内存地址（需要声明变量类型）而在高级一些的语言中，比如python，变量名更抽象，他对应于一个值，或者可以理解为它存储了这个值，这个值可以是该语言允许的任何类型（不需要声明变量类型）。
Julia的变量属于后者，不需要声明变量类型，而是可以让你的合法变量名随时存储任何Julia变量类型的值。
如果Julia是你的第一门编程语言，那么你只要知道，变量名存储了一个变量值就可以了。
在交互模式下，我们进行一下操作，比如输入 <code>x=10</code> 后回车，就会输出10这个结果，接着你可以输入指令 <code>x+1</code> 回车，就会显示 <code>10+1</code> 的结果，如下：


<pre><code class="language-julia-repl hljs"># Assign the value 10 to the variable x
<span class="hljs-meta">julia&gt;</span><span class="julia"> x = <span class="hljs-number">10</span>
</span>10

# Doing math with x's value
<span class="hljs-meta">julia&gt;</span><span class="julia"> x + <span class="hljs-number">1</span>
</span>11

# Reassign x's value
<span class="hljs-meta">julia&gt;</span><span class="julia"> x = <span class="hljs-number">1</span> + <span class="hljs-number">1</span>
</span>2

# You can assign values of other types, like strings of text
<span class="hljs-meta">julia&gt;</span><span class="julia"> x = <span class="hljs-string">"Hello World!"</span>
</span>"Hello World!"</code></pre>

任何编译器解释器都不会关心这个变量叫什么名字，所有变量在解释器面前一视同仁，所以你不用担心你的变量名字不好听而影响程序执行效果。
接着再看几个例子（例子都来自Julia文档）
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> x = <span class="hljs-number">1.0</span>
</span>1.0

<span class="hljs-meta">julia&gt;</span><span class="julia"> y = -<span class="hljs-number">3</span>
</span>-3

<span class="hljs-meta">julia&gt;</span><span class="julia"> Z = <span class="hljs-string">"My string"</span>
</span>"My string"

<span class="hljs-meta">julia&gt;</span><span class="julia"> customary_phrase = <span class="hljs-string">"Hello world!"</span>
</span>"Hello world!"

<span class="hljs-meta">julia&gt;</span><span class="julia"> UniversalDeclarationOfHumanRightsStart = <span class="hljs-string">"人人生而自由，在尊严和权利上一律平等。"</span>
</span>"人人生而自由，在尊严和权利上一律平等。"</code></pre>

更厉害的是Julia的变量名不止局限于英文字符的组合，unicode字符通过 UTF-8 编码也可以成为变量，换句话说，希腊字母，中文字符，日文，韩文等这些都可以做变量名，这就厉害了，python 和c/c++ 是不行的：

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> δ = <span class="hljs-number">0.00001</span>
</span>1.0e-5

<span class="hljs-meta">julia&gt;</span><span class="julia"> 你好地球 = <span class="hljs-string">"Hello world"</span>
</span>"Hello world"

<span class="hljs-meta">julia&gt;</span><span class="julia"> 안녕하세요 = <span class="hljs-string">"Hello"</span>
</span>"Hello"</code></pre>
怎么样，可以用中文命名变量名了以后，你会在你的工程中使用中文命名你的变量名么？

注意到上面 $\delta$ 的同学们可能在想这个变量在实际中怎么使用呢？好消息，用latex就可以，当然只有部分编辑器或者IDE支持这个功能，Julia REPL 是支持的，上面的 $\delta$ 在Julia REPL的写法就是 <code>\delta</code>-*tab* ，如果你发现那个字符不知道怎么写，比如你看别人的代码中有个 $\zeta$ 不知道怎么写，你只需要在Julia REPL 中输入 <code>?</code> 然后在后面粘贴那个字符即可。

有一些编程语言会为编译器或者编辑器定义一些常量以及函数，这些常量或函数一般不允许用户修改，但是Julia可以，如果你明确知道自己在做什么，那么这个操作可以帮助你的程序更好更符合你想象的方式进行，但是如果你还是个新手，建议不要随意修改内置常量或函数，因为你很有可能忘记自己修改过这个常量或函数，而在别的部分再次当做原始功能使用，就会造成很隐蔽的bug。

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-literal">pi</span> = <span class="hljs-number">3</span>
</span>3

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-literal">pi</span>
</span>3

<span class="hljs-meta">julia&gt;</span><span class="julia"> sqrt = <span class="hljs-number">4</span>
</span>4</code></pre>

但是如果这个常量或函数先被使用，而你在之后尝试重新定义或者修改，那么Julia就会抛出错误：

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-literal">pi</span>
</span>π = 3.1415926535897...

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-literal">pi</span> = <span class="hljs-number">3</span>
</span>ERROR: cannot assign variable MathConstants.pi from module Main

<span class="hljs-meta">julia&gt;</span><span class="julia"> sqrt(<span class="hljs-number">100</span>)
</span>10.0

<span class="hljs-meta">julia&gt;</span><span class="julia"> sqrt = <span class="hljs-number">4</span>
</span>ERROR: cannot assign variable Base.sqrt from module Main</code></pre>

## Julia 变量命名规则
变量名的命名规则：
{% note info %}
1. 变量名以字母 **A-Z or a-z，下划线，或者Unicode中大于00A0的部分** 开头（更过详细的Unicode参考4）
2. 像 <code>+</code> 这类操作符也是可以用作标识符的，但是这种用法非常特殊，比如再从新定义加法操作的时候，加法操作就是用<code>(+)</code> 来定义的，<code>(+)=f</code> 就是重新定义加法操作（更过详细的Unicode参考4）
3. 语言内置的状态符是不可以用作变量名的，比如你不可把 <code>if</code>,<code>else</code>这类关键字用作变量名：
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-keyword">else</span> = <span class="hljs-literal">false</span>
</span>ERROR: syntax: unexpected "else"

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-keyword">try</span> = <span class="hljs-string">"No"</span>
</span>ERROR: syntax: unexpected "="</code></pre>
4. Unicode变量名的有一些注意细节，如果要大量使用，可以参考文档：[https://docs.julialang.org/en/latest/manual/variables/](https://docs.julialang.org/en/latest/manual/variables/)
{% endnote %}
## Julia 变量名的常用风格
命名风格不是语法内容，这些内容是一些常见的比较利于编程的习惯，如果你有自己的更好的习惯，也可以用你的方法来写，但是代码的作用一是为了驱动程序，得出我们想要的结果，第二则是让别人了解你的思想，如果大家都看不懂你的什么，那么这个代码其实是不成功的。
常见的命名风格有以下几条：
1. 变量名采用小写字母
2. 每个单词之间用 <code>_</code> 分开，但一般不要使用，除非这个变量名特别长，且不好读。
3. <code>Type</code>s或者<code>Module</code>s的名字以大写字母开头，每个单词首字母大写，用驼峰形式代替下划线
4. <code>function</code>s或者<code>macro</code>s用小写形式，不要使用下划线。
5. <code>function</code>如果要对其参数进行写的操作的话，函数名后面要加<code>!</code>，这种函数叫做 “变异函数”(mutating)或者“取代函数”(in-place)因为这种函数不只返回一个结果，还会在过程中修改参数的值。
6. 更多编程风格可以参考[https://docs.julialang.org/en/latest/manual/style-guide/](https://docs.julialang.org/en/latest/manual/style-guide/)


## 总结
本文介绍了Julia变量和简单的命名风格，好的编程风格是很重要的，无论对于编码还是调试。

## Reference
1. [https://docs.julialang.org/en/latest/manual/variables/](https://docs.julialang.org/en/latest/manual/variables/)



--------------------------
[原文地址: https://face2ai.com/Julia-Lang-3-Variables](https://face2ai.com/Julia-Lang-3-Variables)
