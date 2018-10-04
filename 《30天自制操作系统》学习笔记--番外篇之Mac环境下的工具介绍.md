---
title: 【30天自制操作系统】 Mac环境下的工具介绍
list_number: false
date: 2013-12-09 20:20
categories:
    - 30天自制操作系统
---
**Abstract:** Mac 下 《30天自制操作系统》环境工具介绍
**Keywords:** 《30天自制操作系统》，Macbook，环境，工具
<!--more-->
这几天又有点不务正业了，书也没看，一直在搞这个破环境，尝试各种做法，网上各种垃圾信息，浪费了很多时间，说的基本都是废话，不过还是找到了一些，赶紧写下来，不然这个过几天又忘了
首先是环境，我用的是Max os Maverick 64，就是10.9，硬件（有点低0.0，对于小菜的我已经完全够用啦）：
10.9刚发布，新出来的东西总问题一大堆，然后解决办法没几个。但是还是能用了，编译器NASM（xcode中command line tools带的那个）xcode真的很大，但是不可否认，安装比vs2010快多了。基本指令是dd指令，和cat指令，这两个和linux下差不多.
首先建立一个.img的镜像文件：
```
dd if=xxxxx of=xxxx.img count=20000         
```
注意，if后面的参数是个文件夹，用空的，of参数是输出的img文件 count 是大小，自己十几次就知道了
然后编写ipl
注意，后缀用.s而不用nas哦，因为.s的文件vi编辑器会自动语法高亮
然后要说一下ipl中的一句话

```
RESB0x7fde-$
```

这句话汇编通不过：提示这一行有非法操作符。。我也不知道咋回事。。。。。
改成：
![Center][]
就可以顺利的汇编成bin文件了
接着，用CAT指令把bin塞到img中

```
cat ill.bin > xxxx.img  
```

然后用qemu执行以下

```
qemu -fda xxxx.img                              
```

结果如图：
![Center 1][]
用到的工具软件我会上传，安装比较简单，都是dmg的，双击就好。。。后天继续把c语言弄进来。。。


[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/《30天自制操作系统》学习笔记--番外篇之Mac环境下的工具介绍/20131209201250890.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/《30天自制操作系统》学习笔记--番外篇之Mac环境下的工具介绍/20131209201546875.png





原文地址1：[https://www.face2ai.com/《30天自制操作系统》学习笔记--番外篇之Mac环境下的工具介绍](https://www.face2ai.com/《30天自制操作系统》学习笔记--番外篇之Mac环境下的工具介绍)转载请标明出处
