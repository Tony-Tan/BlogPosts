---
title: 【30天自制操作系统】 Mac环境搭建
list_number: false
date: 2013-12-13 14:20
categories:
    - 30天自制操作系统
---
**Abstract:** Mac 下 《30天自制操作系统》环境
**Keywords:** 《30天自制操作系统》，Macbook，环境
<!--more-->

弄了三天了，终于弄好了，先说结果，就是作者在网站上放了os x的工具（hrb.osask.jp，也有linux下的工具，可以自己去下载），也就是说我白忙活了三天。。。
再说一下这几天都干啥了，主要是想把c语言和nasm汇编连在一起。这个很多人都做过，但在网上现有的资料很少有在os X上做的的，也或者做了大家都没人说。。。。先贴代码：
```c++
    extern void swap(int *,int *);
    void main(){
    	int a=1;
    	int b=2;
    	swap(&a,&b);
    	while(a==2)
    	;


    }
```
这是c代码，调用swap交换两个值，为了不调用标准库，我没写显示函数，而是用了一个死循环代替，如果程序停住了，说明运行成功，再贴下汇编，这是我第一次写汇编哦。。啦啦啦啦啦
```nasm
    GLOBAL _swap
    [section .text]
    _swap:
    	mov EDX,[ESP+4]
    	mov EAX,[ESP+8]
    	mov EBX,[EDX]
    	mov ECX,[EAX]
    	mov [EDX],ECX
    	mov [EAX],EBX
    	ret
```

代码很简单，但是和书上格式有些不同，作者说的他用的是nask是他自己改版的nasm所以有些关键字用不了。。。

然后是编译成obj文件，这个很纠结，一开始不会用gcc编译出32位obj后来发现要加：
```
\-m32 
```

就可以了。

编译过程如下图：

![Center][]

整个编译连接过程，最后光标停止，说明函数执行成功，如果nasm中写了什么中断或者什么其他系统不允许的可能会有总线错误（bus error）或者段错误（详情可以去看《c专家编程》，有相关说明）。
值得注意的是nasm -f 的参数：
```
valid output formats for -f are (\`\*' denotes default):

  \* bin       flat-form binary files (e.g. DOS .COM, .SYS)

    aout      Linux a.out object files

    aoutb     NetBSD/FreeBSD a.out object files

    coff      COFF (i386) object files (e.g. DJGPP for DOS)

    elf       ELF32 (i386) object files (e.g. Linux)

    as86      Linux as86 (bin86 version 0.3) object files

    obj       MS-DOS 16-bit/32-bit OMF object files

    win32     Microsoft Win32 (i386) object files

    rdf       Relocatable Dynamic Object File Format v2.0

    ieee      IEEE-695 (LADsoft variant) object file format

    macho     NeXTstep/OpenStep/Rhapsody/Darwin/MacOS X object files
```
这个参数纠结了好久，最后还是看帮助搞定的，因为linux下都是elf，但是os x用elf参数最后ld会报错，说找不到xxx函数定义。。
ld的相关问题：

```
1：ld: symbol(s) not found for inferred architecture i386
2：ld: symbol(s) not found for inferred architecture x86\_64
3：ld: warning: ignoring file xxxx.o, file was built for unsupported file format
```

1，2和3的问题原因都是-f参数选的不对，或者gcc编译出来的是64位obj，nasm只能编译出来32或者16位目标代码。

如果和系统可运行程序不对应，ld不会给你链接的哦。。
最后是objcopy，这个是GNU 的binutils的工具包的一部分。作用是操作二进制文件，可以任意改格式，具体参考说明，吧之前链接好的用objcopy 生成纯二进制文件后，和作者的比较发现，不一样，运行时qemu卡死，得到结论就是这两天又白忙活了。。。还好算是找到了工具，也有源代码，值得好好学习。


[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/《30天自制操作系统》学习笔记--Mac环境搭建/20131213140747343.png





原文地址1：[https://www.face2ai.com/《30天自制操作系统》学习笔记--Mac环境搭建](https://www.face2ai.com/《30天自制操作系统》学习笔记--Mac环境搭建)转载请标明出处
