---
title: 【Julia】Julia环境搭建（Mac,Windows,Linux）
categories:
    - Julia
    - Freshman
tags:
    - Julia
keywords:
    - Julia环境搭建
    - Julia Mac
    - Julia Windows
    - Julia Linux
    - Juno
toc: true
date: 2018-09-29 21:56:02
---

**Abstract:** 本介绍Julia环境的搭建，包括在Mac，Linux以及在Windows下的安装过程，最后我们使用atom搭建一个Julia IDE
**Keywords:** Julia环境搭建, Mac,Julia Windows,Julia Linux,Juno
<!--more-->
# Julia环境搭建（Mac,Windows,Linux）
上文我们说到Julia是一种适合数据科学的语言，那么今天我们就研究一下怎么安装Julia，以及完成一套IDE的搭建，很多人，尤其是写程序有一段时间，但是时间又不长的同学经常会纠结各种问题，比如为啥大牛都用VIM，是不是要学会VIM才能继续进步，说实话，不管是IDE还是VIM还是EMACS，不过是个工具，哪个你用的效率最高哪个就是最好的，至于vim的神话，我的解读是，vim必然是个利器，入了门的人得心应手，但是入门困难的就一直处于困难阶段，我们的最终目的是为了完成我的设计的算法，评估测试算法，至于用什么编辑器，什么环境，我觉得你找个最顺手的，那就是最好的。
本文我们用到的编辑器是Atom，因为atom中的插件可以直接把一个编辑器扩展成IDE，如果你是资深vim用户，那么你一定有办法自己把编辑器调整到顺手状态。
所以我们今天只讲安装和atom下的IDE搭建。
## 安装Julia
安装Julia并不难，到地址 [https://julialang.org/downloads/](https://julialang.org/downloads/)
下就能看到：

![Jupiter-web](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/julia-web.png)


可以看到我当前的版本是1.0，目前1.1在测试，所以你们使用的时候可能都2.x了，如果基本套路变了，那么你就要仔细看一下说明了。
### Mac下安装Julia
我们在表格中找到 Mac对应的安装包 .dmg 格式
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929222929641.png)
然后点击下载（速度有点慢），
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929223057287.png)
按照Mac程序的一般安装方法，拖到应用里面即可。
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-2018092922315751.png)
这个时候就算安装完了，但是使用mac的程序员启动这种没界面的应用喜欢使用命令行，这里的做法是，建立一个从应用到命令行搜索目录的软连接：
```bash
ln -s /Applications/Julia-1.0.app/Contents/Resources/julia/bin/julia /usr/local/bin/julia
```
当前julia版本是1.0。
<font color="ff0000">注意这个"Julia-1.0.app" 会随版本不同，不要照抄这段命令。</font>

在命令行中启动julia如果出现下图，恭喜，成功了：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929223755799.png)



### Windows下安装Julia
Windows下安装就要注意一下操作系统版本了：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929223855214.png)
windows7 和server 2012 需要特别的组件，原因我也不知道，我目前只有一台Windows 10 的pc，所以我就下载64-bit的安装包，安装结果没测试，大家有问题留言吧。
如果想在cmd中启动，需要添加环境变量，但是windows程序员一般不用cmd启动程序

### Linux下安装Julia
Linux下我目前只有ubuntu
### 方式一：apt
```bash
sudo apt-get install julia
```
输入sudo密码就可以完成安装了
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929224835566.png)
但是问题是，我的ubuntu版本比较低，所以对应源上的Julia还是老版本的
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929224930362.png)

看到了吧，官网的版本都到1.0.0 了，
### 方式二：下载tar.jz
对应的办法就是卸掉这个然后去下载官网上的通用版本，然后解压编译：

![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929225148724.png)
注意你的平台，是X86 还是arm以及操作系统的位数。
如果你是linux程序员，我觉得后面的事我没有一步一步跟你说了，因为都是linux下通用的编译安装步骤，这里不再赘述了。

## 安装Atom，搭建IDE
接着我们要安装一款编辑器，github出品，良心之作 —— atom，这款编辑器的优点就是开源，速度快，核心小，尤其是对于从windows刚来linux，或者mac的同学们，离开vs有可能水土不服，但是相信我，如果你用了atom或其他类似的编辑器，你就会忘记那个好多G的vs。
下载atom: [https://atom.io](https://atom.io)
有多重平台，Mac，Linux，Windows对应的安装包，安装套路和Julia类似，都是普通的安装过程，然后，最关键的一步
注意，这是我目前正在写文章的atom的状态：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929225947832.png)
比较正常，接着我们要安装juno了: [http://junolab.org](http://junolab.org) 注意区分juno，有好多叫juno的项目，比如NASA什么的。还有就juno要求的atom平台版本号。
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929230139168.png)
Juno是一个atom上的插件，Windows 或者Linux 使用快捷键 ```Ctrl+,```
Mac下使用```Cmd+,``` 打开atom的插件安装界面
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929230510192.png)
选择```install```
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929230703482.png)
安装完juno后，他会自己给你安装一些他需要的扩展：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929231412150.png)

右上角安装完成后重启Atom就有Juno可以用了，就这样：
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/Julia-Lang-1-Install/markdown-img-paste-20180929232623129.png)

然后我们就成功了，当网速不那么流畅的时候稍微等一会儿，juno安装完成会提示你。不要着急。
## 总结
今天我们完成了Julia的环境搭建，有了环境，有了IDE，我们就能大展拳脚，看看Julia是不是真的那么高效便捷了。

## Reference
1. [https://docs.julialang.org/en/v1/manual/getting-started/](https://docs.julialang.org/en/v1/manual/getting-started/)


--------------------------
[原文地址: https://face2ai.com/Julia-Lang-1-Install](https://face2ai.com/Julia-Lang-1-Install)
