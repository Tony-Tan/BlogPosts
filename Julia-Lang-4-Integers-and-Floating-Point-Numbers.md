---
title: 【Julia】整型和浮点型数字
categories:
    - Julia
    - Freshman
tags:
    - Julia
    - 整型数字
    - 浮点型数字
keywords:
    - Julia
    - 整型数字
    - 浮点型数字
toc: true
date: 2018-10-03 18:04:56
---

**Abstract:** 本文介绍Julia语言的整型和浮点型数字的
**Keywords:**
<!--more-->
# 整型和浮点型数字
整数或者浮点数在编程中被大量使用，由于大部分程序面对的问题都是处理数字计算相关的问题，所以数字的表示变成了代码中最重要的一部分。不论是加减乘除，还是积分微分，在计算机中，都要使用整型和浮点型来完成，至于整型和浮点型计算过程中需要注意的事项，这是在数值分析，数值计算课程中需要考虑的，[数值分析课程传送门](https://www.face2ai.com/categories/Mathematic/Numerical-Analysis/)，整形变量例如 <code>1</code> 浮点型变量例如<code>1.0</code>。
这两个例子中的数字是我们在日常和编码的时候使用的，我们称之为**字面值**（immediate values，对于翻译成立即数的那些教材，我们不予评价，嘻嘻）。而他们在内存中的二进制形式，被称为**原始值**（numeric primitives）。
Julia提供了宽泛的原始值类型，和定义在他们之上的一个完整数字计算操作、按位计算操作以及数值函数操作。这些原始值类型都是现在计算机原生支持的，所以Julia可以高效利用计算机的计算资源。Julia也提供了主观定义的计算操作（arbitrary precision arithmetic），这些操作不被硬件支持，在计算过程中速度将会变得比较慢。
- 整型变量

<table><tbody><tr><th>类型</th><th>是否有符号?</th><th>所占位数</th><th>最小值</th><th>最大值</th></tr><tr><td><code>Int8</code></td><td>✓</td><td>8</td><td>-2^7</td><td>2^7 - 1</td></tr><tr><td><code>UInt8</code></td><td></td><td>8</td><td>0</td><td>2^8 - 1</td></tr><tr><td><code>Int16</code></td><td>✓</td><td>16</td><td>-2^15</td><td>2^15 - 1</td></tr><tr><td><code>UInt16</code></td><td></td><td>16</td><td>0</td><td>2^16 - 1</td></tr><tr><td><code>Int32</code></td><td>✓</td><td>32</td><td>-2^31</td><td>2^31 - 1</td></tr><tr><td><code>UInt32</code></td><td></td><td>32</td><td>0</td><td>2^32 - 1</td></tr><tr><td><code>Int64</code></td><td>✓</td><td>64</td><td>-2^63</td><td>2^63 - 1</td></tr><tr><td><code>UInt64</code></td><td></td><td>64</td><td>0</td><td>2^64 - 1</td></tr><tr><td><code>Int128</code></td><td>✓</td><td>128</td><td>-2^127</td><td>2^127 - 1</td></tr><tr><td><code>UInt128</code></td><td></td><td>128</td><td>0</td><td>2^128 - 1</td></tr><tr><td><code>Bool</code></td><td>N/A</td><td>8</td><td><code>false</code> (0)</td><td><code>true</code> (1)</td></tr></tbody></table>

- 浮点型变量

<table><tbody><tr><th>类型</th><th>精度</th><th>所占位数</th></tr><tr><td><code>Float16</code></td><td><a href="https://en.wikipedia.org/wiki/Half-precision_floating-poInt_format">half</td><td>16</td></tr><tr><td><code>Float32</code></td><td><a href="https://en.wikipedia.org/wiki/Single_precision_floating-poInt_format">single</td><td>32</td></tr><tr><td><code>Float64</code></td><td><a href="https://en.wikipedia.org/wiki/Double_precision_floating-poInt_format">double</td><td>64</td></tr></tbody></table>

以上浮点型的三种类型的精度可以点击进入wikipedia查看详情。
## 整型
在Julia编程中字面值以标准形式表示，比如我们想输入数值1，只需要输入<code>1</code>就可以了，而不需要转换成存储在计算机内存或硬盘中的二进制。
比如在交互模式下：

<pre><code>
$ julia
               _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.0.0 (2018-08-08)
 _/ |\__'_|_|_|\__'_|  |  Official https://julialang.org/ release
|__/                   |

julia> 1
1

julia> 123
123

julia>
</code></pre>


### typeof
<code>typeof</code>的用法和c语言中的 <code>sizeof</code>类似，但是<code>sizeof</code>在c/c++中是操作符，而<code>typeof</code>在Julia这里可能是个函数，这需要我们后面深入的研究确定，我们先画个问号在这。在交互模式下我们来看看我们的整形变量1在Julia中的类型：
<pre><code>
julia> typeof(1)
Int64

julia> typeof(123)
Int64

julia>
</code></pre>
输出结果是<code>Int64</code>，不是<code>Int32</code>的原因是我们的操作系统是64位操作系统，所以基础类型就是64位的了。

### Sys.WORD_SIZE
<code>Sys.WORD_SIZE</code>这个内置变量用于指示当前系统是32位还是64位：
<pre><code>
julia> Sys.WORD_SIZE
64

julia>
</code></pre>

### Int 和 UInt
这两种类型的不同在于是否有符号，数值范围也有区别，具体的可以从上面的表中得出，如果想看当前系统内的<code>Int</code>和<code>UInt</code>对应位数，可以直接在交互模式下输入：
<pre><code class="language-julia-repl hljs"># 32-bit system:
<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-built_in">Int</span>
</span>Int32
<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-built_in">UInt</span>
</span>UInt32

# 64-bit system:
<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-built_in">Int</span>
</span>Int64
<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-built_in">UInt</span>
</span>UInt64</code></pre>
32位系统和64位系统对应如上.
**注意：Julia大小写敏感，所以uInt和UInt是不同**
<pre><code>
julia> uInt
ERROR: UndefVarError: uInt not defined

julia> UInt
ERROR: UndefVarError: UInt not defined

julia> UInt
UInt64
</code></pre>

### 输入类型
如果我们的系统是32位系统，所以对应的整数默认类型是<code>Int32</code>那么如果整数值超过了32-bit有符号型整数的范围，在C语言中就会发生溢出，但是在Julia中，系统会自动将超过32-bit的数字转换成64-bit形式（C语言中在代码中定义类型，所以输入可能会溢出，但是Julia中不事先定义变量类型，所以可以自主转换）：
<pre><code class="language-julia-repl hljs"># 32-bit or 64-bit system:
<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(<span class="hljs-number">3000000000</span>)
</span>Int64</code></pre>

所以Julia中不会出现溢出这种风险。


### 16进制(base 16)
常用的16进制当然也是支持的，16进制以 <code>0x</code> 开头，使用<code>0-9a-f</code>作为每一位字面值的数字，这里的 <code>a-f</code>与<code>A-F</code>是等价的：
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0x1</span>
</span>0x01

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(ans)
</span>UInt8

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0x123</span>
</span>0x0123

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(ans)
</span>UInt16

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0x1234567</span>
</span>0x01234567

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(ans)
</span>UInt32

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0x123456789abcdef</span>
</span>0x0123456789abcdef

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(ans)
</span>UInt64

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0x11112222333344445555666677778888</span>
</span>0x11112222333344445555666677778888

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(ans)
</span>UInt128</code></pre>

16进制输入的数字最终都会被系统解释为无符号的类型。
这是因为16进制输入一般都不是表示数值，而是一个数字序列。

**注意：**<code>ans</code>**只有在交互模式下可以如此使用**

### 2进制，8进制
16进制是允许的，那么2进制和8进制当然也是可以的：
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0b10</span>
</span>0x02

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(ans)
</span>UInt8

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0o010</span>
</span>0x08

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(ans)
</span>UInt8

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0x00000000000000001111222233334444</span>
</span>0x00000000000000001111222233334444

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(ans)
</span>UInt128</code></pre>

- 二进制的表示方式:<code>0b</code>开头，每一位只能是<code>0-1</code>
- 二进制的表示方式:<code>0o</code>开头，每一位只能是<code>0-7</code>

与16进制相同2进制和8进制的输入，系统也是按照无符号类型处理的。
处理输入的2进制或者8进制的位数时则是选择能保存该数值的最小位数，开头的0将会被省略（这样可以节约内存空间），如果想保存原始位数，那么使用1开头即可。
如果输入数字不能被<code>UInt128</code>存储下，那么这个字面值不合法。
如果输入的2，8，16进制数字前面有符号，那么将会转换成无符号的对应值存储：
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> -<span class="hljs-number">0x2</span>
</span>0xfe

<span class="hljs-meta">julia&gt;</span><span class="julia"> -<span class="hljs-number">0x0002</span>
</span>0xfffe</code></pre>

### <code>typemin</code>函数,<code>typemax</code>函数
如果想获得某类型的最大最小值，那么就可以使用<code>typemin</code>函数,<code>typemax</code>函数：
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> (typemin(<span class="hljs-built_in">Int32</span>), typemax(<span class="hljs-built_in">Int32</span>))
</span>(-2147483648, 2147483647)

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-keyword">for</span> T <span class="hljs-keyword">in</span> [<span class="hljs-built_in">Int8</span>,<span class="hljs-built_in">Int16</span>,<span class="hljs-built_in">Int32</span>,<span class="hljs-built_in">Int64</span>,<span class="hljs-built_in">Int128</span>,<span class="hljs-built_in">UInt8</span>,<span class="hljs-built_in">UInt16</span>,<span class="hljs-built_in">UInt32</span>,<span class="hljs-built_in">UInt64</span>,<span class="hljs-built_in">UInt128</span>]
           prIntln(<span class="hljs-string">"<span class="hljs-subst">$(lpad(T,<span class="hljs-number">7</span>)</span>): [<span class="hljs-subst">$(typemin(T)</span>),<span class="hljs-subst">$(typemax(T)</span>)]"</span>)
       <span class="hljs-keyword">end</span>
</span>   Int8: [-128,127]
  Int16: [-32768,32767]
  Int32: [-2147483648,2147483647]
  Int64: [-9223372036854775808,9223372036854775807]
 Int128: [-170141183460469231731687303715884105728,170141183460469231731687303715884105727]
  UInt8: [0,255]
 UInt16: [0,65535]
 UInt32: [0,4294967295]
 UInt64: [0,18446744073709551615]
UInt128: [0,340282366920938463463374607431768211455]</code></pre>
<code>for</code>是循环控制关键字
<code>prIntln()</code>是打印函数
还有一些其他的细节，我们还没有学习，具体的用法我们将在后面介绍。
**注意：<code>typemin</code>函数,<code>typemax</code>函数只支持已经数值类型**


### 溢出特性
对于有限位数的数值都会存在溢出的现象，这种现象是由于计算机结构的特性造成的，无法避免，可以当错误看待也可以当特性看待：
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> x = typemax(<span class="hljs-built_in">Int64</span>)
</span>9223372036854775807

<span class="hljs-meta">julia&gt;</span><span class="julia"> x + <span class="hljs-number">1</span>
</span>-9223372036854775808

<span class="hljs-meta">julia&gt;</span><span class="julia"> x + <span class="hljs-number">1</span> == typemin(<span class="hljs-built_in">Int64</span>)
</span>true</code></pre>

这种溢出现象，可以看做是模运算，模运算也是现计算机的重要特性。
在应用中出现溢出是可能的，检测溢出现象也是必须的，或者可以尝试使用更高级的类型<code>BigInt</code> 但是虽然溢出问题可以通过该类型解决，但是效率可能会下降，所以，这里就需要程序员自己平衡了。

### 除法错误
除法错误的根本原因是除数为<code>0</code>,但是这只是其中一种，或者说，这是错误的根基，但是表现出来有以下几种：
- <code>div</code>函数中除数是<code>0</code>
- 最小的负数（typemin(Int32)）除以<code>-1</code>
- 取模操作的操作数是<code>0</code>也会出现除法错误：

<pre><code>
julia> div(100,0)
<font color="ff0000"><b>ERROR: DivideError: Integer division error<b></font>
Stacktrace:
 [1] div(::Int64, ::Int64) at ./Int.jl:232
 [2] top-level scope at none:0

julia> div(typemin(Int64),-1)
<font color="ff0000"><b>ERROR: DivideError: Integer division error<b></font>
Stacktrace:
 [1] div(::Int64, ::Int64) at ./Int.jl:232
 [2] top-level scope at none:0

julia> mod(20,0)
<font color="ff0000"><b>ERROR: DivideError: Integer division error<b></font>
Stacktrace:
 [1] div at ./Int.jl:232 [inlined]
 [2] fld at ./Int.jl:241 [inlined]
 [3] mod(::Int64, ::Int64) at ./Int.jl:221
 [4] top-level scope at none:0

julia> rem(20,0)
<font color="ff0000"><b>ERROR: DivideError: Integer division error<b></font>
Stacktrace:
 [1] rem(::Int64, ::Int64) at ./Int.jl:233
 [2] top-level scope at none:0

julia>

</code></pre>

这里面唯一有问题的就是为什么最小的数字除以<code>-1</code>会出现错误，原因很简单，我们假设我们操作4-bit的有符号数字，那么符号位占一位，我们就有如下表格：
|十进制数|符号位+ 二进制绝对值的表示方式|ones' complement|two's complement|
| --- | --- | --- | --- |
|+7|0111|表示方式不变|表示方式不变|
|+6|0110|表示方式不变|表示方式不变|
|+5|0101|表示方式不变|表示方式不变|
|+4|0100|表示方式不变|表示方式不变|
|+3|0011|表示方式不变|表示方式不变|
|+2|0010|表示方式不变|表示方式不变|
|+1|0001|表示方式不变|表示方式不变|
|+0|0000|表示方式不变|表示方式不变|
|-0|1000|1111|(1)0000|
|-1|1001|1110|1111|
|-2|1010|1101|1110|
|-3|1011|1100|1101|
|-4|1100|1011|1100|
|-5|1101|1010|1011|
|-6|1110|1001|1010|
|-7|1111|1000|1001|
|-8	|超出4个bit所能表达范围|	超出4个表达范围|	1000|


注：要设计硬件区分符号位，比较绝对值大小。无需设计硬件比较大小，但零存在两种表示方法。
较好的解决上述问题
。由于零只有一种表达方式，所以，可以比别的方式多表达一个-8.
(上表来自百度百科：https://baike.baidu.com/item/补码)
由于在操作系统中以补码形式存储有符号数，所以，最小的负数值的绝对值比相应的最大的正数值大1，所以，当他除以-1的时候，结果溢出，故除法抛出错误。


--------------------------
[原文地址: https://face2ai.com/Julia-Lang-4-Integers-and-Floating-PoInt-Numbers](https://face2ai.com/Julia-Lang-4-Integers-and-Floating-PoInt-Numbers)
