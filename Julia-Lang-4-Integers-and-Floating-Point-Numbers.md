---
title: 【Julia】整型和浮点型数字
categories:
    - Julia
    - Freshman
tags:
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

julia></code></pre>

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
## 浮点数
整型表示的数字都是整数，现在我们该研究一下小数了，带小数的字面值，用浮点型表示
### 常用浮点类型
#### Float64

正常输入不加任何修饰、转化的小数字面值都用 <code>Float64</code> 类型存储
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">1.0</span>
</span>1.0

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">1.</span>
</span>1.0

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0.5</span>
</span>0.5

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">.5</span>
</span>0.5

<span class="hljs-meta">julia&gt;</span><span class="julia"> -<span class="hljs-number">1.23</span>
</span>-1.23

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">1e10</span>
</span>1.0e10

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">2.5e-4</span>
</span>0.00025</code></pre>


#### Float32
以上这些数字以<code>Float64</code> 形式存储，<code>e</code>就是数学中的科学计数法，如果想用单精度浮点数表示，那么就要在数字中加<code>f0</code> 或者如果已经是科学计数法的数字，用<code>f</code>来来替代<code>e</code>，对于单精度双精度不太了解的同学，可以参考：[ 数值分析-浮点数](https://www.face2ai.com/Math-Numerical-Analysis-0-3-Float/)或者google查询更详细的关于浮点数的介绍，Julia中的用法如下：

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0.5f0</span>
</span>0.5f0

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(ans)
</span>Float32

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">2.5f-4</span>
</span>0.00025f0</code></pre>

转化成<code>Float32</code>的方式如下：

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-built_in">Float32</span>(-<span class="hljs-number">1.5</span>)
</span>-1.5f0

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(ans)
</span>Float32</code></pre>

#### Float16
<code>Float16</code>当然也是支持的，虽然可能使用不多，但是有时候在要在更小的内存设备上使用一些数字16位类型就派上用场了，但是注意，在计算的时候，16位的浮点数会被补全成32位的Float32进行计算，这是因为硬件上的浮点数加法器，乘法器，一般都是32位或者更高的，16位已经退出历史舞台了

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> sizeof(<span class="hljs-built_in">Float16</span>(<span class="hljs-number">4.</span>))
</span>2

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">2</span>*<span class="hljs-built_in">Float16</span>(<span class="hljs-number">4.</span>)
</span>Float16(8.0)</code></pre>


16 位的<code>Float16</code>占两个字节，共16位
<code>sizeof()</code> 函数用来计算变量在内存中使用的空间，等效于c/c++ 中的 sizeof()。


#### 下划线 '_'
在数字中，下划线在数字中间以分隔符的作用出现，就像我们平时写论文或者写其他比较正式的文章的时候，超过三位的数字，会没三位加逗号，这个逗号在程序里被改成了下划线
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">10_000</span>, <span class="hljs-number">0.000_000_005</span>, <span class="hljs-number">0xdead_beef</span>, <span class="hljs-number">0b1011_0010</span>
</span>(10000, 5.0e-9, 0xdeadbeef, 0xb2)</code></pre>

### 0的浮点数
浮点数的0有两种表示方法：
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0.0</span> == -<span class="hljs-number">0.0</span>
</span>true

<span class="hljs-meta">julia&gt;</span><span class="julia"> bitstring(<span class="hljs-number">0.0</span>)
</span>"0000000000000000000000000000000000000000000000000000000000000000"

<span class="hljs-meta">julia&gt;</span><span class="julia"> bitstring(-<span class="hljs-number">0.0</span>)
</span>"1000000000000000000000000000000000000000000000000000000000000000"</code></pre>

正负0相等，但是浮点的二进制形式差一个符号位。
<code>bitstring</code> 函数的作用是将变量在内存中的二进制形式按位形成一个字符串。
### 特殊的浮点数
有三个特殊的标准浮点值，这三个值在实数轴上没有对应点，可能大家已经猜到了， $\pm \infty$ 还有就是 "not a number"

<table><tbody><tr><th><code>Float16</code></th><th><code>Float32</code></th><th><code>Float64</code></th><th>Name</th><th>Description</th></tr><tr><td><code>Inf16</code></td><td><code>Inf32</code></td><td><code>Inf</code></td><td>正无穷</td><td>大于所有能表示出来的浮点数值</td></tr><tr><td><code>-Inf16</code></td><td><code>-Inf32</code></td><td><code>-Inf</code></td><td>负无穷</td><td>小于所有能表示出来的浮点数值</td></tr><tr><td><code>NaN16</code></td><td><code>NaN32</code></td><td><code>NaN</code></td><td>不是一个数字</td><td>结果不等于任何一个浮点数字</td></tr></tbody></table>

上表就是<code>Float16</code>、<code>Float64</code>、<code>Float32</code> 对应的无穷和Nan的表格，这三个非标准浮点数会经常出现在编程中，尤其是有bug的地方。
更多相关内容我们会在后面 "Numeric Comparisons" 中介绍。
IEEE 754 标准规定包含特殊浮点数的计算结果如下：

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">1</span>/<span class="hljs-literal">Inf</span>
</span>0.0

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">1</span>/<span class="hljs-number">0</span>
</span>Inf

<span class="hljs-meta">julia&gt;</span><span class="julia"> -<span class="hljs-number">5</span>/<span class="hljs-number">0</span>
</span>-Inf

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0.000001</span>/<span class="hljs-number">0</span>
</span>Inf

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0</span>/<span class="hljs-number">0</span>
</span>NaN

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">500</span> + <span class="hljs-literal">Inf</span>
</span>Inf

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">500</span> - <span class="hljs-literal">Inf</span>
</span>-Inf

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-literal">Inf</span> + <span class="hljs-literal">Inf</span>
</span>Inf

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-literal">Inf</span> - <span class="hljs-literal">Inf</span>
</span>NaN

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-literal">Inf</span> * <span class="hljs-literal">Inf</span>
</span>Inf

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-literal">Inf</span> / <span class="hljs-literal">Inf</span>
</span>NaN

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">0</span> * <span class="hljs-literal">Inf</span>
</span>NaN</code></pre>

<code>typemin()</code> 和 <code>typemax()</code> 函数可以用于浮点类型，结果如下：
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> (typemin(<span class="hljs-built_in">Float16</span>),typemax(<span class="hljs-built_in">Float16</span>))
</span>(-Inf16, Inf16)

<span class="hljs-meta">julia&gt;</span><span class="julia"> (typemin(<span class="hljs-built_in">Float32</span>),typemax(<span class="hljs-built_in">Float32</span>))
</span>(-Inf32, Inf32)

<span class="hljs-meta">julia&gt;</span><span class="julia"> (typemin(<span class="hljs-built_in">Float64</span>),typemax(<span class="hljs-built_in">Float64</span>))
</span>(-Inf, Inf)</code></pre>

### 机器精度（<code>eps</code> 函数）
大多数实数是不能用机器数准确表达的，所以这就涉及到精度问题，本站[数值分析](https://www.face2ai.com/categories/Mathematic/Numerical-Analysis/)主要研究这方面内容，而每种浮点类型的误差已经在其设计时就已经被确定了，当我们要用到相关的准确度的时候，只需要查询语言内置的函数就可以，这属于系统误差，一直都在，如果这个误差影响了程序，那么就要通过数值分析来找办法，<code>eps</code> 函数就是语言内置的查询精度值的函数。
#### <code>eps(Float)</code>
其参数是浮点类型时，结果如下：
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> eps(<span class="hljs-built_in">Float32</span>)
</span>1.1920929f-7

<span class="hljs-meta">julia&gt;</span><span class="julia"> eps(<span class="hljs-built_in">Float64</span>)
</span>2.220446049250313e-16

<span class="hljs-meta">julia&gt;</span><span class="julia"> eps() <span class="hljs-comment"># same as eps(Float64)</span>
</span>2.220446049250313e-16</code></pre>
[ 数值分析-浮点数](https://www.face2ai.com/Math-Numerical-Analysis-0-3-Float/)文中有介绍这个精确度是怎么计算出来的。这个精度的字面定义是 浮点数 1.0和与他最近的另一个该类型的浮点数之间的差。

#### <code>eps(100.0)</code>
<code>eps</code> 函数也能接受浮点数作为参数：

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> eps(<span class="hljs-number">1.0</span>)
</span>2.220446049250313e-16

<span class="hljs-meta">julia&gt;</span><span class="julia"> eps(<span class="hljs-number">1000.</span>)
</span>1.1368683772161603e-13

<span class="hljs-meta">julia&gt;</span><span class="julia"> eps(<span class="hljs-number">1e-27</span>)
</span>1.793662034335766e-43

<span class="hljs-meta">julia&gt;</span><span class="julia"> eps(<span class="hljs-number">0.0</span>)
</span>5.0e-324</code></pre>

当<code>eps</code> 函数以浮点数作为输入的时候，结果是输入的浮点数与最接近的他的浮点数之间的差。

#### <code>prevfolat()</code>和<code>nextfloat()</code>函数

使用上面<code>eps</code> 函数的结果可以计算出当前浮点数的最近的上一个浮点数和下一个浮点数：

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> x = <span class="hljs-number">1.25f0</span>
</span>1.25f0

<span class="hljs-meta">julia&gt;</span><span class="julia"> nextfloat(x)
</span>1.2500001f0

<span class="hljs-meta">julia&gt;</span><span class="julia"> prevfloat(x)
</span>1.2499999f0

<span class="hljs-meta">julia&gt;</span><span class="julia"> bitstring(prevfloat(x))
</span>"00111111100111111111111111111111"

<span class="hljs-meta">julia&gt;</span><span class="julia"> bitstring(x)
</span>"00111111101000000000000000000000"

<span class="hljs-meta">julia&gt;</span><span class="julia"> bitstring(nextfloat(x))
</span>"00111111101000000000000000000001"</code></pre>

相邻的浮点数的背后是相邻的二进制机器数，二进制的特性导致了浮点数误差的性质，而我们经常忽视这些误差，这明显是不对的，这么小小的误差就会导致火箭会坠毁。


#### “近似” 模型
当我们的字面值没有对应的准确浮点表达的时候，我们就要进行近似了 —— 向上取近似还是向下取近似。IEEE 754标准有明确的相关的操作。
<code>RoundNearest</code>对这个过程进行操作，在进阶部分我们会对其过程进行剖析。
#### 背景知识和引用
<ul><li>The definitive guide to floating point arithmetic is the <a href="http://standards.ieee.org/findstds/standard/754-2008.html">IEEE 754-2008 Standard</a>; however, it is not available for free online.</li><li>For a brief but lucid presentation of how floating-point numbers are represented, see John D. Cook's <a href="https://www.johndcook.com/blog/2009/04/06/anatomy-of-a-floating-point-number/">article</a> on the subject as well as his <a href="https://www.johndcook.com/blog/2009/04/06/numbers-are-a-leaky-abstraction/">introduction</a> to some of the issues arising from how this representation differs in behavior from the idealized abstraction of real numbers.</li><li>Also recommended is Bruce Dawson's <a href="https://randomascii.wordpress.com/2012/05/20/thats-not-normalthe-performance-of-odd-floats/">series of blog posts on floating-point numbers</a>.</li><li>For an excellent, in-depth discussion of floating-point numbers and issues of numerical accuracy encountered when computing with them, see David Goldberg's paper <a href="http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.22.6768&amp;rep=rep1&amp;type=pdf">What Every Computer Scientist Should Know About Floating-Point Arithmetic</a>.</li><li>For even more extensive documentation of the history of, rationale for, and issues with floating-point numbers, as well as discussion of many other topics in numerical computing, see the <a href="https://people.eecs.berkeley.edu/~wkahan/">collected writings</a> of <a href="https://en.wikipedia.org/wiki/William_Kahan">William Kahan</a>, commonly known as the "Father of Floating-Point". Of particular interest may be <a href="https://people.eecs.berkeley.edu/~wkahan/ieee754status/754story.html">An Interview with the Old Man of Floating-Point</a>.</li></ul>



## 任意准确度数字
以上我们研究的数字都受到设计时的精度限制，Julia提供一种不限制大小的数字表达方式，换句话说，这种精度是自定义的，和你输入的数字永远一致，听起来更像把数字字符串化，看起来类似，但是这些字符串是可以进行计算的，Julia打包了GMP(GNU Multiple Precision Arithmetic Library)协议和GNU MPER Library 开发出了 <code>BigInt</code>和<code>BigFloat</code>类型，这两个类型可以表示任意精度的整数或浮点数。
<code>parse</code>函数提供了将字符串转化成<code>BigInt</code>和<code>BigFloat</code>类型的一种方式，为什么要用这个函数，因为你没办法用其他数值类型保存任意长度的数字，这样操作，一旦<code>BigInt</code>和<code>BigFloat</code>类型的数字完成定义就可以和任意类型的数字进行计算了，这要归功于Julia的内部类型转换机制（后面进阶部分会介绍）
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-built_in">BigInt</span>(typemax(<span class="hljs-built_in">Int64</span>)) + <span class="hljs-number">1</span>
</span>9223372036854775808

<span class="hljs-meta">julia&gt;</span><span class="julia"> parse(<span class="hljs-built_in">BigInt</span>, <span class="hljs-string">"123456789012345678901234567890"</span>) + <span class="hljs-number">1</span>
</span>123456789012345678901234567891

<span class="hljs-meta">julia&gt;</span><span class="julia"> parse(<span class="hljs-built_in">BigFloat</span>, <span class="hljs-string">"1.23456789012345678901"</span>)
</span>1.234567890123456789010000000000000000000000000000000000000000000000000000000004

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-built_in">BigFloat</span>(<span class="hljs-number">2.0</span>^<span class="hljs-number">66</span>) / <span class="hljs-number">3</span>
</span>2.459565876494606882133333333333333333333333333333333333333333333333333333333344e+19

<span class="hljs-meta">julia&gt;</span><span class="julia"> factorial(<span class="hljs-built_in">BigInt</span>(<span class="hljs-number">40</span>))
</span>815915283247897734345611269596115894272000000000</code></pre>

虽然可以进行<code>BigInt</code>和<code>BigFloat</code>类型和常规类型的数值计算的时候，Julia会自己进行类型转换，但是如果操作中没有<code>BigInt</code>和<code>BigFloat</code>类型则还是会产生溢出，即当计算式中没有<code>BigInt</code>或<code>BigFloat</code>类型Julia不会自动的调整数值到<code>BigInt</code>或<code>BigFloat</code>类型：

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> x = typemin(<span class="hljs-built_in">Int64</span>)
</span>-9223372036854775808

<span class="hljs-meta">julia&gt;</span><span class="julia"> x = x - <span class="hljs-number">1</span>
</span>9223372036854775807

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(x)
</span>Int64

<span class="hljs-meta">julia&gt;</span><span class="julia"> y = <span class="hljs-built_in">BigInt</span>(typemin(<span class="hljs-built_in">Int64</span>))
</span>-9223372036854775808

<span class="hljs-meta">julia&gt;</span><span class="julia"> y = y - <span class="hljs-number">1</span>
</span>-9223372036854775809

<span class="hljs-meta">julia&gt;</span><span class="julia"> typeof(y)
</span>BigInt</code></pre>
常规类型的数字计算还是会溢出。

前面提到的近似模型，在</span>BigFloat</code>中将会被改变，其可以被手工指定，利用<code>setprecision</code> 和 <code>setrounding</code> 函数可以进行全局设置。或者如果只想在某个代码块内改变近似模型，则可以使用<code>do</code> 代码块：
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> setrounding(<span class="hljs-built_in">BigFloat</span>, <span class="hljs-literal">RoundUp</span>) <span class="hljs-keyword">do</span>
           <span class="hljs-built_in">BigFloat</span>(<span class="hljs-number">1</span>) + parse(<span class="hljs-built_in">BigFloat</span>, <span class="hljs-string">"0.1"</span>)
       <span class="hljs-keyword">end</span>
</span>1.100000000000000000000000000000000000000000000000000000000000000000000000000003

<span class="hljs-meta">julia&gt;</span><span class="julia"> setrounding(<span class="hljs-built_in">BigFloat</span>, <span class="hljs-literal">RoundDown</span>) <span class="hljs-keyword">do</span>
           <span class="hljs-built_in">BigFloat</span>(<span class="hljs-number">1</span>) + parse(<span class="hljs-built_in">BigFloat</span>, <span class="hljs-string">"0.1"</span>)
       <span class="hljs-keyword">end</span>
</span>1.099999999999999999999999999999999999999999999999999999999999999999999999999986

<span class="hljs-meta">julia&gt;</span><span class="julia"> setprecision(<span class="hljs-number">40</span>) <span class="hljs-keyword">do</span>
           <span class="hljs-built_in">BigFloat</span>(<span class="hljs-number">1</span>) + parse(<span class="hljs-built_in">BigFloat</span>, <span class="hljs-string">"0.1"</span>)
       <span class="hljs-keyword">end</span>
</span>1.1000000000004</code></pre>



## 数字系数
### 数值作为系数
在其他编程语言中，数字变量就是一个操作数，当他作为式子的参数的时候，必须要用操作符连接，比如在c++或者python中我们想表达 $y=2x+1$ 这个表达式的时候一定是:
```c++
y=2*x+1
```
但是在Julia中数字参数可以用另一种形式表示，回归到其参数的地位，而不是c++或者python中的操作数的身份，在Julia中，数字参数可以这么写：

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> x = <span class="hljs-number">3</span>
</span>3

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">2</span>x^<span class="hljs-number">2</span> - <span class="hljs-number">3</span>x + <span class="hljs-number">1</span>
</span>10

<span class="hljs-meta">julia&gt;</span><span class="julia"> <span class="hljs-number">1.5</span>x^<span class="hljs-number">2</span> - <span class="hljs-number">.5</span>x + <span class="hljs-number">1</span>
</span>13.0</code></pre>

虽然就差一个操作符号，但是这种系数形式更加贴近原始公式，当然这里交换律不行，不能把数字放到后面，那样就成了一个新的变量了：

<pre><code>
julia> x=2
2

julia> y=2x+1
5

julia> y=x2+1
<B><font color="ff0000">ERROR: UndefVarError: x2 not defined</B></font>
Stacktrace:
 [1] top-level scope at none:0

julia>
</code></pre>
以下形式表示 $2^{2x}$ 而不是 $2^2x$
<pre><code class="language-julia-repl hljs">
julia> x=3
3

julia> 2^2x
64</code></pre>

这种表达的一个问题就是我们将会对结合顺序产生疑惑，比如：
- <code>-2x</code> 表示<code>(-2)x</code>
- <code>√2x</code>是表示 <code>(√2)x</code>
- <code>2x^3</code>是表示 <code>2*(x^3)</code>

前两个比较好理解，对于幂，系数的结合方式类似于一元操作符，比如 <code>-x^2</code>大家都会理解为<code> -(x^2)</code> 这里的符号和系数的语法上的解释是一样的

{% note info%}
注意：数值系数操作(乘法)优先级高于其他二元操作，比如乘法或者除法：
- <code>1 / 2im == -0.5im</code> 而不是<code>0.5im</code>
- <code>6 // 2(2 + 1) == 1 // 1</code> 而不是 <code>(6 // 2)*3 = 9</code>
{% endnote %}



### 加了括号的表达式也可以作为系数

加了括号的表达式可以作为系数，比如：

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> (x-<span class="hljs-number">1</span>)x
</span>6</code></pre>

但是没有加括号的表达式，或者两个操作数都有括号，就不可以这么写了：

<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> (x-<span class="hljs-number">1</span>)(x+<span class="hljs-number">1</span>)
</span>ERROR: MethodError: objects of type Int64 are not callable

<span class="hljs-meta">julia&gt;</span><span class="julia"> x(x+<span class="hljs-number">1</span>)
</span>ERROR: MethodError: objects of type Int64 are not callable</code></pre>


上面这两种写法报错的原因是，这两种写法和函数的写法撞车了，所以不可能允许这种计算。

上面这两种写法给我们写公式的时候带来了很大的方便，注意不要在系数后面加空格，也不要在括号系数后加空格。

### 句法争议
上面的参数写法不被其他语言采纳的原因是会造成其他句法混淆，比如十六进制和科学计数法：
- 十六进制数字 <code>0xff</code> 将会被解释成：参数 <code>0</code> 乘以变量 <code>xff</code>.
- 浮点数 <code>1e10</code>会被解释成：参数 <code>1</code> 乘以变量 <code>e10</code>， <code>E</code>形式类似
- 32位浮点数 <code>1.5f22</code> 被解释为：参数<code>1.5</code> 乘以变量<code>f22</code>.

对于上述争端，Julia给出的解释是：
- <code>0x</code>被解释为16进制字符，不会被理解成别的
- 一个数字后面跟了 <code>e</code> 或者 <code>E</code> 将会被解释为浮点数
- 一个数字后面跟了 <code>f</code>将会被解释为32-bit浮点数，**但是F不会被解释为32-bit浮点数，而是按照系数解释**



## “清零” 和 “归一”

Julia提供了一个函数来产生0，和1，这个作用听起来有点费解，这不就是赋值么？为什么还要函数，因为不同的数值类型对应着不同的0的表示法，比如不同长度的浮点数的0就有很多种，所以一个函数搞定还是方便的：
<table><tbody><tr><th>Function</th><th>Description</th></tr><tr><td><a href="../../base/numbers/#Base.zero"><code>zero(x)</code></a></td><td>Literal zero of type <code>x</code> or type of variable <code>x</code></td></tr><tr><td><a href="../../base/numbers/#Base.one"><code>one(x)</code></a></td><td>Literal one of type <code>x</code> or type of variable <code>x</code></td></tr></tbody></table>

其操作结果：
<pre><code class="language-julia-repl hljs"><span class="hljs-meta">julia&gt;</span><span class="julia"> zero(<span class="hljs-built_in">Float32</span>)
</span>0.0f0

<span class="hljs-meta">julia&gt;</span><span class="julia"> zero(<span class="hljs-number">1.0</span>)
</span>0.0

<span class="hljs-meta">julia&gt;</span><span class="julia"> one(<span class="hljs-built_in">Int32</span>)
</span>1

<span class="hljs-meta">julia&gt;</span><span class="julia"> one(<span class="hljs-built_in">BigFloat</span>)
</span>1.0</code></pre>



## Reference
1. https://docs.julialang.org/en/v1/manual/integers-and-floating-point-numbers/


--------------------------
[原文地址: https://face2ai.com/Julia-Lang-4-Integers-and-Floating-PoInt-Numbers](https://face2ai.com/Julia-Lang-4-Integers-and-Floating-PoInt-Numbers)
