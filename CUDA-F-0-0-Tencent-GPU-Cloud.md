---
title: 【CUDA FRESHMAN】0.0 腾讯云CUDA环境搭建
categories:
  - CUDA
  - Freshman
tags:
  - CUDA 环境搭建
toc: true
date: 2018-02-13 22:04:54
---

**Abstract:** 本文主要为不支持CUDA的电脑用户提供一个可供选择的CUDA学习环境
**Keywords:** 腾讯云,CUDA,GPU云

<!--more-->

## 开篇废话
今天我们开始学习CUDA，又到了一年一次的，逼婚，催生，问工资，问成绩，炫富，炫孩子，炫工作的节日了。我还是想找个地方安安静静的看看书，身在世俗，岂能不染。
## Macbook
我没有意思想要炫耀macbook（前几年确实是高端货），这玩意去星巴克，一桌子都是，但那里用来写程序的不多，最近几年Mac在中国普及度非常高，以前见不到几个，现在真的是到处都是，对于开发者，大部分用来开发IOS应用，也有小部分用于开发其他，我对Macbook有着更多的恩怨情仇，这里就不说了。
我为什么选择Macbook？
我的第一个mac里是有NVIDIA显卡的，为了这个mac我算是搭上了身家性命了，用mac研究了《三十天自制操作系统》，刚萨雷斯的数字图像处理，写了一百多篇CSDN博客（虽然质量不太高），当时坚持要买带显卡的，就为了学习cuda，然而，我已经换了第二台macbook了，这才刚刚开始深入研究CUDA，Macbook用久了真的感觉效率很高，linux桌面版说实话，我感觉稳定性并没有网上宣传的那么高（网上多半是说服务器版本的）桌面版本的谁用谁知道，天天给我弹出内核错误的消息，而macbook基本没啥问题，内核又是类unix，所以所有工具和linux很相似，但又更顺畅，当然价格也要高出很多。
但是，macbook上已经没有nvidia显卡了，肿么办？
我的办法是又买了一台有nvidia显卡的笔记本电脑，事实证明，脑残了，Lenovo下的linux做日常工作很不顺利，windows又有很多工具用不爽（比如我们的后面要用到的cuda的一些调试工具）。
就在决定过年带哪个电脑的时候，我想起来了，可以找找有没有可以用的云服务器，这样，我们就能愉快的在mac上编码，然后远程调试运行就可以了，于是我先写了一篇搭建环境的文章，来供大家参考
## 云
这个云那个云多到不行，提供GPU服务的也越来越多，第一选择本来是google云，但是google那边要申请gpu资源，还有先交35 USD 的费用，还要翻墙，然后我觉果断的拒绝了，同样的功能，腾讯这边能便宜不少，于是我开始配置在腾讯云上的环境。
首先就是买买买买，贵不贵？不便宜，但是看起来不错，每小时十几块钱（前几天去了一次网吧，我还以为两三块钱一个小时，结果服务员小妹妹说三十，一脸😳）
## 腾讯云GPU购买
按照下面这个基本过程操作（2018年1月完成以下操作，如果过段时间腾讯界面修改，那么以当时的为准）
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/1.jpg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/2.jpg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/3.jpg)
这里必须要注意一下，因为有的区没有GPU服务器，所以一定要选一个有的地区买，我之前就傻傻的在北京区折腾半天，我以为从“GPU云服务器”那个按钮点进来的，就每个机器都有GPU呢。。结果折腾半天也安装不上CUDA，后来发现，naive啊！
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/4.jpg)
选一个你能接受的，我们目前这点小知识，最最最简陋的就够了，后面多GPU的试验估计我已经回到公司了，机器多得是，哈哈
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/5.jpg)
点击服务市场，里面有带有CUDA的操作系统（搜索就可以了），我们选ubuntu的CUDA7.5。这次我们会自己制作一个镜像，把我们安装的一些东西打包起来，下次再买的时候可以直接安装我们自己的镜像。就不用配置环境了，安装好的全家桶。
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/6.jpg)
配置一些基本信息，包括登录密码，记住开启ssh 22端口，不然没办法登录了
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/7.jpg)
等待云端配置。
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/8.jpg)
配置完成，注意你的公网IP，我们一会儿要用这个地址登录。
## 连接到云
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/10.jpg)
简单的ssh登录，第一次要确认输入个yes，然后输入密码就可以了
登录成功后我们要做的就是安装CUDA和CMAKE，这两个是我们后面整个工程都需要的。
### 安装CUDA7.5
安装CUDA比较简单，因为操作系统已经安装好NVIDIA驱动了，我们直接输入
```bash
nvcc
```
就会有提示让你安装CUDA-Tookite，按照他的提示安装就好了。
### 安装CMAKE
1. 去官网下载linux版的源代码
2. 解压源代码，进入源代码目录，
3. 执行：./bootstrap
4. 执行：make
5. 执行：sudo make install

### 验证环境
验证环境可以很多中方法，编译CUDA例子，运行也可以，用自己的也可以，我们用我们自己写的小代码来测试下，看运行是否成功
先用nvidia-smi命令来看一下驱动是否正确安装
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/12.jpg)

从我们的代码库中clone一份我们的学习cuda的代码，cmake一下
```
git clone https://github.com/Tony-Tan/CUDA_Freshman.git
```
别忘了给我来个星星
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/11.jpg)
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/13.jpg)
看起来一帆风顺，继续下一步，cmake完后make一下，所有都生成成功了，执行一个例子，发现运行成功了，获取到的GPU型号是Tesla M40 24G 足够我们干各种各样的试验了

## 制作一个镜像
我们是按照小时租的服务器，所以我们争取在线下写的正确些，然后买了服务器以后就马上做各种试验观察结果，因为服务器是按照小时收费的，我们尽可能的要快一些，所以，如果每次都配置环境，浪费时间太多，我们用搭建好的环境做个镜像出来，下次再买服务器的时候可以直接安装，美滋滋
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/15.jpg)
更多里面有个只做镜像的功能，写好描述就可以，别到时候自己都忘了

## 及时销毁
我们是按照小时收费的，所以用完了以后马上去销毁掉
![](https://tony4ai-1251394096.cos.ap-hongkong.myqcloud.com/blog_images/CUDA-F-0-0-Tencent-GPU-Cloud/15.jpg)
更多操作里面有销毁，等你确定备份好镜像了，可以关机了，就销毁就可以了。
## 总结
好久没写这种流水账一样的文章，感觉写起来确实很流畅，没有啥精神压力，这篇文章只是写给那些没有nvidia设备还想学习CUDA的同学们，有nvidia显卡的同学可以直接在本地装CUDA和CMAKE方法类似。
