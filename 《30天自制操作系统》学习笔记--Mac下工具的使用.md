---
title: \[30天自制操作系统\]Mac下工具的使用
list_number: false
date: 2013-12-13 14:42
categories:
    - 30天自制操作系统
---
**Abstract:** Mac 下 《30天自制操作系统》工具使用
**Keywords:** 《30天自制操作系统》，Macbook，环境，工具
<!--more-->
现在来介绍官网上下的工具怎么用首先是官网地址，书上有个注释上有:hrb.osask.jp
![Center][]
翻译成中文大概是这个样子滴。
上面有两个文件可以下载，一个是工具，一个是工具的源代码，很好的学习资料
下面把工具复制出来
![Center 1][]
看到很多可执行文件。。感觉好舒服。。
然后把我们随便一个project复制到z\_tools的同级目录下
project的内容可以修改，因为批处理可以下岗了：
![Center 2][]
然后可能是难度最大的部分出现了。。。修改makefile
如果没用过makefile可以先找点资料看看。很简单的语法，很强大的功能：
修改完成后是：
```
    TOOLPATH = ../z_tools/
    INCPATH  = ../z_tools/haribote/
    ###################################################     MAKE     = make
    NASK     = $(TOOLPATH)nask
    CC1      = $(TOOLPATH)gocc1 -I$(INCPATH) -Os -Wall -quiet
    GAS2NASK = $(TOOLPATH)gas2nask -a
    OBJ2BIM  = $(TOOLPATH)obj2bim
    BIM2HRB  = $(TOOLPATH)bim2hrb
    RULEFILE = $(TOOLPATH)haribote/haribote.rul
    EDIMG    = $(TOOLPATH)edimg
    IMGTOL   = $(TOOLPATH)imgtol.com
    COPY     = cp
    DEL      = rm
    ###################################################     ## ÉfÉtÉHÉãÉgìÆçÏ

    default :
    	$(MAKE) img

    ## ÉtÉ@ÉCÉãê∂ê¨ãKë•

    ipl10.bin : ipl10.nas Makefile
    	$(NASK) ipl10.nas ipl10.bin ipl10.lst

    asmhead.bin : asmhead.nas Makefile
    	$(NASK) asmhead.nas asmhead.bin asmhead.lst

    bootpack.gas : bootpack.c Makefile
    	$(CC1) -o bootpack.gas bootpack.c

    bootpack.nas : bootpack.gas Makefile
    	$(GAS2NASK) bootpack.gas bootpack.nas

    bootpack.obj : bootpack.nas Makefile
    	$(NASK) bootpack.nas bootpack.obj bootpack.lst

    bootpack.bim : bootpack.obj Makefile
    	$(OBJ2BIM) @$(RULEFILE) out:bootpack.bim stack:3136k map:bootpack.map \
    		bootpack.obj
    ## 3MB+64KB=3136KB

    bootpack.hrb : bootpack.bim Makefile
    	$(BIM2HRB) bootpack.bim bootpack.hrb 0
    ##################################################     haribote.sys : asmhead.bin bootpack.hrb Makefile
    	cat  asmhead.bin bootpack.hrb>haribote.sys
    ###################################################     haribote.img : ipl10.bin haribote.sys Makefile
    	$(EDIMG)   imgin:../z_tools/fdimg0at.tek \
    		wbinimg src:ipl10.bin len:512 from:0 to:0 \
    		copy from:haribote.sys to:@: \
    		imgout:haribote.img

    ## ÉRÉ}ÉìÉh

    img :
    	$(MAKE) haribote.img
    #################################################     run :
    	$(MAKE) img
    	qemu -fda haribote.img
    ###################################################

    clean :
    	-$(DEL) *.bin
    	-$(DEL) *.lst
    	-$(DEL) *.gas
    	-$(DEL) *.obj
    	-$(DEL) bootpack.nas
    	-$(DEL) bootpack.map
    	-$(DEL) bootpack.bim
    	-$(DEL) bootpack.hrb
    	-$(DEL) haribote.sys

    src_only :
    	$(MAKE) clean
    	-$(DEL) haribote.img
```

乱码是原来的日语。。华丽的忽视掉，一排\#之间的就是修改过的地方，
然后就是make run了。。
结果如下：
![Center 3][]
工具准备齐全了，明天开始研究理论，博客继续更新。。。
我什么时候才能成为非主流专家呢。。哇哈哈


[Center]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/《30天自制操作系统》学习笔记--Mac下工具的使用/20131213143501046.png
[Center 1]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/《30天自制操作系统》学习笔记--Mac下工具的使用//20131213143655937.png
[Center 2]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/《30天自制操作系统》学习笔记--Mac下工具的使用/20131213143514609.png
[Center 3]: https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/《30天自制操作系统》学习笔记--Mac下工具的使用/20131213144145015.png





原文地址1：[https://www.face2ai.com/《30天自制操作系统》学习笔记--Mac下工具的使用](https://www.face2ai.com/《30天自制操作系统》学习笔记--Mac下工具的使用)转载请标明出处
