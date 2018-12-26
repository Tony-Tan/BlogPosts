---
title: 【Julia】开始使用Julia
categories:
    - Julia
    - Freshman
tags:
    - Julia
    - Julia
    - Julia使用
    - Julia命令行
    - Julia执行文件
toc: true
date: 2018-10-01 09:16:49
---

**Abstract:** 本文介绍如何在命令行中开启、使用、退出Julia，如何执行文件，以及执行Julia时候的可选选项（参数列表）
**Keywords:** Julia，Julia使用，Julia命令行，Julia执行文件
<!--more-->
# 开始使用Julia
本文我们来学习Julia的几种用法，包括便捷的终端交互，以及对于复杂功能的文件执行。
## 终端使用Julia(**交互模式**)
在终端启动Julia比较简单，上文我们完成了Julia的安装，并在Mac下完成了命令行下启动的设置，那么我们可直接在命令行提示符后输入<code>julia</code> 完成Julia环境的启动（**交互模式**），如果出现下图，表示启动成功了。
### 在交互模式下执行命令
在Julia的命令提示符后输入<code>1 + 2</code> 后回车，就会显示计算结果。
```shell
$ julia

               _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.0.0 (2018-08-08)
 _/ |\__'_|_|_|\__'_|  |
|__/                   |


julia> 1 + 2
3

julia> ans
3```

<code>ans</code>变量表示上次**计算**的结果，如果在这个<code>ans</code>下再次输入<code>ans</code>并回车，显示结果是：

```shell
julia> 1 + 2
3

julia> ans
3

julia> ans
3```
注意：<code>ans</code>只能在**交互模式**下使用

### 在交互模式下执行文件
如果你有一个julia文件 <code>file.jl</code>，里面写的是计算过程:
```julia
1+3
```

你可以在**交互模式**下执行这个文件，使用命令 <code>include("file.jl")</code>

```shell
$julia
               _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.0.0 (2018-08-08)
 _/ |\__'_|_|_|\__'_|  |  Official https://julialang.org/ release
|__/                   |

julia> include("file.jl")
4

julia> ```

### 退出交互模式
退出交互模式回到终端有两种方法：
1. <code>CTRL-d</code> linux和windows下同时按下 <code>ctrl</code> 键和<code>d</code> 键
2. 输入 <code>exit()</code>


## 终端执行Julia文件
在终端下执行Julia文件的方式和其他脚本的执行方式类似：

```shell
$ julia script.jl arg1 arg2...
```

如果我们执行上面我们说到的 <code>file.jl</code>那么在终端下的执行结果如下：
```shell
$ julia file.jl
$
```

没有显示结果，因为我们在文件中只有 <code>1+3</code>这条指令，而没有要求他输出什么，在交互模式下，程序自动显示计算结果，但是在终端下执行脚本，只执行脚本中的命令，而不会自己显示什么。
接着我们编辑另一个文件<code>script.jl</code>，内容如下：
```julia
println(PROGRAM_FILE);
for x in ARGS;
    println(x);
end
```
然后我们加上参数，这里是**输入给脚本的参数** —— <code>arg1 arg2 ...</code>
```shell
$ julia script.jl foo bar
script.jl
foo
bar
$  ```

这里的三部分分别是 <code>julia</code> (程序名，用于在终端中启动程序，类似于python脚本运行前的python，sh等)、 <code>script.jl</code>（脚本文件名） 以及 <code>foo bar </code>（输入脚本的参数）。

## Julia的参数
在执行脚本时我们给脚本了两个个参数（<code>foo bar</code>），同时，我们也可以给julia多个参数(上面的例子中 <code>script.jl foo bar</code> 是Julia程序的一个参数 )，加参数方式是在多个参数中间用 <code>--</code> 划分 ,例如上面的例子可以加上如下参数
```shell
julia --color=yes -O --script.jl foo bar
```
julia程序包含两个参数，分别是：
- <code>--color=yes -O </code>
- <code>--script.jl foo bar</code>

script.jl 包含两个参数：
- <code>foo</code>
- <code>bar</code>

## Julia 的并行模式（本机和集群）
Julia可以以并行模式启动，启动参数是 <code>-p</code>或者 <code>--machine-file</code>
<code>-p n</code> 将会启动n个工作进程
<code>--machine-file file</code> 将会按照file中记录的机器地址，启动对应机器上的任务。
<code>--machine-file</code> 模式注意以下要求：
1. 这些机器的登录方式必须是<code>ssh</code>无密码登录(不需要输入手工输入密码，而是通过ssh 秘钥登录)
2. 这些机器上的Julia安装目录必须和当前执行命令的主机位置相同
3. <code>file</code>中的每一条机器记录格式如下
    - <code>[count*][user@]host[:port] [bind_addr[:port]]</code>
        - <code>count *</code>是节点要执行的工作进程数量（类似本机的</code>-p n</code>）
        - <code>user</code> 默认是当前用户
        - <code>port</code> 是标准ssh的端口号，默认是1
        - <code>bind-to bind_addr[:port]</code> 是其他机器使用的IP地址和端口号，可以用来连接到本机。

## Julia更多执行参数列表
Julia的更多参数列表如下，执行方法：
```shell
julia [switches] -- [programfile] [args...]
```
参数列表：

<table><tbody><tr><th>Switch</th><th>Description</th></tr><tr><td><code>-v</code>, <code>--version</code></td><td>Display version information</td></tr><tr><td><code>-h</code>, <code>--help</code></td><td>Print this message</td></tr><tr><td><code>-J</code>, <code>--sysimage &lt;file></code></td><td>Start up with the given system image file</td></tr><tr><td><code>-H</code>, <code>--home &lt;dir></code></td><td>Set location of <code>julia</code> executable</td></tr><tr><td><code>--startup-file={yes|no}</code></td><td>Load <code>~/.julia/config/startup.jl</code></td></tr><tr><td><code>--handle-signals={yes|no}</code></td><td>Enable or disable Julia's default signal handlers</td></tr><tr><td><code>--sysimage-native-code={yes|no}</code></td><td>Use native code from system image if available</td></tr><tr><td><code>--compiled-modules={yes|no}</code></td><td>Enable or disable incremental precompilation of modules</td></tr><tr><td><code>-e</code>, <code>--eval &lt;expr></code></td><td>Evaluate <code>&lt;expr></code></td></tr><tr><td><code>-E</code>, <code>--print &lt;expr></code></td><td>Evaluate <code>&lt;expr></code> and display the result</td></tr><tr><td><code>-L</code>, <code>--load &lt;file></code></td><td>Load <code>&lt;file></code> immediately on all processors</td></tr><tr><td><code>-p</code>, <code>--procs {N|auto</code>}</td><td>Integer value N launches N additional local worker processes; <code>auto</code> launches as many workers as the number of local CPU threads (logical cores)</td></tr><tr><td><code>--machine-file &lt;file></code></td><td>Run processes on hosts listed in <code>&lt;file></code></td></tr><tr><td><code>-i</code></td><td>Interactive mode; REPL runs and <code>isinteractive()</code> is true</td></tr><tr><td><code>-q</code>, <code>--quiet</code></td><td>Quiet startup: no banner, suppress REPL warnings</td></tr><tr><td><code>--banner={yes|no|auto}</code></td><td>Enable or disable startup banner</td></tr><tr><td><code>--color={yes|no|auto}</code></td><td>Enable or disable color text</td></tr><tr><td><code>--history-file={yes|no}</code></td><td>Load or save history</td></tr><tr><td><code>--depwarn={yes|no|error}</code></td><td>Enable or disable syntax and method deprecation warnings (<code>error</code> turns warnings into errors)</td></tr><tr><td><code>--warn-overwrite={yes|no}</code></td><td>Enable or disable method overwrite warnings</td></tr><tr><td><code>-C</code>, <code>--cpu-target &lt;target></code></td><td>Limit usage of cpu features up to &lt;target>; set to <code>help</code> to see the available options</td></tr><tr><td><code>-O</code>, <code>--optimize={0,1,2,3}</code></td><td>Set the optimization level (default level is 2 if unspecified or 3 if used without a level)</td></tr><tr><td><code>-g</code>, <code>-g &lt;level></code></td><td>Enable / Set the level of debug info generation (default level is 1 if unspecified or 2 if used without a level)</td></tr><tr><td><code>--inline={yes|no}</code></td><td>Control whether inlining is permitted, including overriding <code>@inline</code> declarations</td></tr><tr><td><code>--check-bounds={yes|no}</code></td><td>Emit bounds checks always or never (ignoring declarations)</td></tr><tr><td><code>--math-mode={ieee,fast}</code></td><td>Disallow or enable unsafe floating point optimizations (overrides @fastmath declaration)</td></tr><tr><td><code>--code-coverage={none|user|all}</code></td><td>Count executions of source lines</td></tr><tr><td><code>--code-coverage</code></td><td>equivalent to <code>--code-coverage=user</code></td></tr><tr><td><code>--track-allocation={none|user|all}</code></td><td>Count bytes allocated by each source line</td></tr><tr><td><code>--track-allocation</code></td><td>equivalent to <code>--track-allocation=user</code></td></tr></tbody></table>



## 总结
本文介绍Julia基础的启动，以及并行模型，执行参数的选择等知识。
## Reference
1. [https://docs.julialang.org/en/v1/manual/getting-started/](https://docs.julialang.org/en/v1/manual/getting-started/)


--------------------------
[原文地址: https://face2ai.com/Julia-Lang-2-Getting-Started](https://face2ai.com/Julia-Lang-2-Getting-Started)
