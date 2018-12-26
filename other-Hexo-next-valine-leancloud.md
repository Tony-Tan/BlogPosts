---
title: 【Hexo】Hexo下next主题valine强化版本的改造
categories:
  - Other
tags:
  - Hexo
  - Next
  - Valine评论系统
  - Valine邮件
toc: true
date: 2018-10-10 15:03:36
---

**Abstract:** 本文介绍Valine评论系统的自定义
**Keywords:** Hexo，Next，Valine评论系统，Valine邮件
<!--more-->
# Hexo下next主题valine强化版本的改造
使用Hexo下Next主题会遇到评论设置上的麻烦，好用的被墙了，剩下的都不太好用。但是Next集成了一个valine评论很有改造空间。
我们这里只提供一个改造思路，具体的执行细节我会给出参考网址。





## 使用valine
按照next或者官方说明，安装是不需要安装什么的，只是要设置下leancloud上的数据段，如果你的文章浏览数据是用leancloud存储的，那么这个过程你应该很了解了，具体的过程可以参考：
1. 官网：[https://valine.js.org](https://valine.js.org)
2. hexo-theme-next上的issues:[https://github.com/iissnan/hexo-theme-next/pull/1983](https://github.com/iissnan/hexo-theme-next/pull/1983)

## 第一个Bug
这是基本的，如果你的网页做过大型优化，比如参考过：
[https://reuixiy.github.io/](https://reuixiy.github.io/)优化过next主题的同学会在topx中出现Bug,页面会变成只有title和阅读次数的状态，解决办法是修改<code>themes/next/layout/_third-party/comments/valine.swig</code>中的第一行为:
```c++
if theme.valine.enable and theme.valine.appid and theme.valine.appkey and page.title !=== '阅读排行'
```
阅读排行是你topx的page的title根据你的命名适当修改。

## 增强Valine
Valine的评论系统轻量级，所以功能就那么完善，比如邮件通知，你都找不到评论在哪篇文章，所以我找到了一个增强版的Valine：
- 赵俊同学的杰作[http://www.zhaojun.im/hexo-valine-modify/](http://www.zhaojun.im/hexo-valine-modify/)

赵同学给出了一个后台的强力解决方案，让我们的邮件通知不那么简陋，也能找到评论再哪篇文章了，为了稳妥起见，一下内容为赵俊同学的博客原文摘录：

----------------------------------
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article"><div class="post-block" style="opacity: 1; display: block;"><link itemprop="mainEntityOfPage" href="http://www.zhaojun.im/hexo-valine-modify/"><span hidden="" itemprop="author" itemscope="" itemtype="http://schema.org/Person"><meta itemprop="name" content="赵俊"><meta itemprop="description" content="一个 Java 学习者的博客，分享 Java 干货, VPS 知识, 软件推荐等。"><meta itemprop="image" content="/images/avatar.gif"></span><span hidden="" itemprop="publisher" itemscope="" itemtype="http://schema.org/Organization"><meta itemprop="name" content="赵俊的博客"></span><header class="post-header" style="opacity: 1; display: block; transform: translateY(0px);"><h2 class="post-title" itemprop="name headline">Hexo 优化 --- 支持邮件通知的评论 Valine 增强版</h2><div class="post-meta"><span class="post-time"><span class="post-meta-item-icon"><i class="fa fa-calendar-o"></i> </span><span class="post-meta-item-text">发表于</span> <time title="创建时间：2018-01-11 18:15:21" itemprop="dateCreated datePublished" datetime="2018-01-11T18:15:21+08:00">2018-01-11</time> <span class="post-meta-divider">|</span> <span class="post-meta-item-icon"><i class="fa fa-calendar-check-o"></i> </span><span class="post-meta-item-text">更新于</span> <time title="修改时间：2018-10-06 14:17:08" itemprop="dateModified" datetime="2018-10-06T14:17:08+08:00">2018-10-06</time> </span><span class="post-category"><span class="post-meta-divider">|</span> <span class="post-meta-item-icon"><i class="fa fa-folder-o"></i> </span><span class="post-meta-item-text">分类于</span> <span itemprop="about" itemscope="" itemtype="http://schema.org/Thing"><a href="/categories/Hexo/" itemprop="url" rel="index"><span itemprop="name">Hexo</span></a></span></span></div></header><div class="post-body" itemprop="articleBody" style="opacity: 1; display: block; transform: translateY(0px);"><h2 id="简介"><a href="#简介" class="headerlink" title="简介"></a>简介</h2><p>此项目是一个对 <a href="https://valine.js.org" target="_blank" rel="noopener">Valine</a> 评论系统的拓展应用，可增强 <code>Valine</code> 的邮件通知功能。基于 Leancloud 的云引擎与云函数。可以提供邮件 <code>通知站长</code> 和 <code>@ 通知</code> 的功能，而且还支持自定义邮件通知模板。</p><p><a href="https://github.com/zhaojun1998/Valine-Admin/blob/master/%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE.md#邮件通知展示" target="_blank" rel="noopener">点击查看演示</a></p><blockquote><p><strong>注：本项目修改于 panjunwen 的项目 : <a href="https://github.com/panjunwen/Valine-Admin" target="_blank" rel="noopener">Valine-Admin</a>，原作者博客: <a href="https://panjunwen.com/valine-admin-document/" target="_blank" rel="noopener">Valine Admin 配置手册</a></strong>, (部分逻辑于功能不同，还请读者不要搞混配置项.)</p></blockquote><a id="more"></a><h2 id="快速开始"><a href="#快速开始" class="headerlink" title="快速开始"></a>快速开始</h2><p>首先需要确保 Valine 的基础功能是正常的，参考 <a href="https://valine.js.org" target="_blank" rel="noopener">Valine Docs</a>。</p><p>然后进入 <a href="https://leancloud.cn/dashboard/applist.html#/apps" target="_blank" rel="noopener">Leancloud</a> 对应的 Valine 应用中。</p><p>点击 <code>云引擎 -&gt; 设置</code> 填写代码库并保存：<code>https://github.com/zhaojun1998/Valine-Admin</code></p><p><a data-fancybox="group" href="https://cdn.jun6.net/201804211508_545.png" class="fancybox fancybox.image" rel="group"><img width="700" src="https://cdn.jun6.net/201804211508_545.png"></a></p><p>切换到部署标签页，分支使用 master，点击部署即可：<br><a data-fancybox="group" href="https://cdn.jun6.net/201801112055_212.png" class="fancybox fancybox.image" rel="group"><img width="700" src="https://cdn.jun6.net/201801112055_212.png"></a><br><a data-fancybox="group" href="https://cdn.jun6.net/201804211336_271.png" class="fancybox fancybox.image" rel="group"><img width="700" src="https://cdn.jun6.net/201804211336_271.png"></a></p><h2 id="配置项"><a href="#配置项" class="headerlink" title="配置项"></a>配置项</h2><p>此外，你需要设置云引擎的环境变量以提供必要的信息，点击云引擎的设置页，设置如下信息：</p><p><a data-fancybox="group" href="https://cdn.jun6.net/201806062257_798.png" class="fancybox fancybox.image" rel="group"><img width="700" src="https://cdn.jun6.net/201806062257_798.png"></a></p><p><strong>必选参数</strong></p><ul><li><code>SITE_NAME</code> : 网站名称。</li><li><code>SITE_URL</code> : 网站地址, <strong>最后不要加 <code>/</code> 。</strong></li><li><code>SMTP_USER</code> : SMTP 服务用户名，一般为邮箱地址。</li><li><code>SMTP_PASS</code> : SMTP 密码，一般为授权码，而不是邮箱的登陆密码，请自行查询对应邮件服务商的获取方式</li><li><code>SMTP_SERVICE</code> : 邮件服务提供商，支持 <code>QQ</code>、<code>163</code>、<code>126</code>、<code>Gmail</code>、<code>"Yahoo"</code>、<code>......</code> ，全部支持请参考 : <a href="https://nodemailer.com/smtp/well-known/#supported-services" target="_blank" rel="noopener">Nodemailer Supported services</a>。</li><li><code>SENDER_NAME</code> : 寄件人名称。</li></ul><h2 id="高级配置"><a href="#高级配置" class="headerlink" title="高级配置"></a>高级配置</h2><p><a href="https://github.com/zhaojun1998/Valine-Admin/blob/master/%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE.md#%E8%87%AA%E5%AE%9A%E4%B9%89%E9%82%AE%E4%BB%B6%E6%A8%A1%E6%9D%BF" target="_blank" rel="noopener">自定义邮件模板</a></p><p><a href="https://github.com/zhaojun1998/Valine-Admin/blob/master/%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE.md#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%94%B6%E4%BB%B6%E9%82%AE%E7%AE%B1" target="_blank" rel="noopener">自定义收件邮箱</a></p><p><a href="https://github.com/zhaojun1998/Valine-Admin/blob/master/%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE.md#%E8%87%AA%E5%AE%9A%E4%B9%89%E9%82%AE%E4%BB%B6%E6%9C%8D%E5%8A%A1%E5%99%A8" target="_blank" rel="noopener">自定义邮件服务器</a></p><p><a href="https://github.com/zhaojun1998/Valine-Admin/blob/master/%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE.md#web-%E8%AF%84%E8%AE%BA%E7%AE%A1%E7%90%86" target="_blank" rel="noopener">Web 评论管理</a></p><p><a href="https://github.com/zhaojun1998/Valine-Admin/blob/master/%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE.md#leancloud-%E4%BC%91%E7%9C%A0%E7%AD%96%E7%95%A5" target="_blank" rel="noopener">Leancloud 休眠策略(必看)</a></p><h2 id="更新历史"><a href="#更新历史" class="headerlink" title="更新历史"></a>更新历史</h2><ul><li>7.7 兼容 <code>valine v1.2.0-beta</code> 版本对 at 的更改 <a href="https://valine.js.org/changelog.html#v1-2-0-beta-2018-06-30" target="_blank" rel="noopener">点击查看</a>。</li><li>7.1 修复 <code>Web</code> 后台登录安全 <code>bug</code></li><li>6.14 添加自定义邮件服务器功能. <a href="/高级配置.md#自定义邮件服务器">点击查看</a></li></ul><h2 id="升级-FAQ"><a href="#升级-FAQ" class="headerlink" title="升级 FAQ"></a>升级 FAQ</h2><p><strong>部署最新代码 :</strong></p><p><a data-fancybox="group" href="https://cdn.jun6.net/201806070911_388.png" class="fancybox fancybox.image" rel="group"><img src="https://cdn.jun6.net/201806070911_388.png" width="600"></a></p><p><strong>重启容器:</strong></p><p><a data-fancybox="group" href="https://cdn.jun6.net/201807081507_968.png" class="fancybox fancybox.image" rel="group"><img width="500" src="https://cdn.jun6.net/201807081507_968.png"></a></p><blockquote><p><strong>注: 更新新版本与更改环境变量均需要重启容器后生效。</strong></p></blockquote><h2 id="LeanCloud-休眠策略"><a href="#LeanCloud-休眠策略" class="headerlink" title="LeanCloud 休眠策略"></a>LeanCloud 休眠策略</h2><p>免费版的 LeanCloud 容器，是有强制性休眠策略的，不能 24 小时运行：</p><ul><li>每天必须休眠 6 个小时</li><li>30 分钟内没有外部请求，则休眠。</li><li>休眠后如果有新的外部请求实例则马上启动（但激活时此次发送邮件会失败）。</li></ul><p>分析了一下上方的策略，如果不想付费的话，最佳使用方案就设置定时器，每天 7 - 23 点每 20 分钟访问一次，这样可以保持每天的绝大多数时间邮件服务是正常的。</p><p>附 <code>Linux crontab</code> 定时器代码：</p><div class="highlight-wrap"><button class="copy-btn">复制</button><figure class="highlight bash"><table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br></pre></td><td class="code"><pre><span class="line">*/20 7-23 * * * curl https://你配置的域名前缀.leanapp.cn</span><br></pre></td></tr></tbody></table></figure></div><p>注 : 此 <code>crontab</code> 不是<code>LeanCloud</code> 后台的定时任务，如果你没有 <code>Linux</code> 机器来配置此定时器，那么可以在此 <a href="https://github.com/zhaojun1998/Valine-Admin/issues/1" target="_blank" rel="noopener">issues</a> 中回复我，我帮你加上。</p><blockquote><p>如对本项目有意见或建议，欢迎去 Github 提 <a href="https://github.com/zhaojun1998/Valine-Admin/issues" target="_blank" rel="noopener">issues</a>。</p></blockquote></div><footer class="post-footer"><div class="post-tags"><a href="/tags/Hexo优化/" rel="tag"># Hexo优化</a> <a href="/tags/Valine/" rel="tag"># Valine</a> <a href="/tags/评论系统/" rel="tag"># 评论系统</a></div><div class="post-nav"><div class="post-nav-next post-nav-item"><a href="/nice_blog/" rel="next" title="怎样才算一个好的技术博客？"><i class="fa fa-chevron-left"></i> 怎样才算一个好的技术博客？</a></div><span class="post-nav-divider"></span><div class="post-nav-prev post-nav-item"><a href="/hexo-lazyload/" rel="prev" title="Hexo 优化 --- lazyload 图片懒加载">Hexo 优化 --- lazyload 图片懒加载 <i class="fa fa-chevron-right"></i></a></div></div></footer></div></article>

---------------------------
希望赵同学的博客不会关闭github也不会删库，这样的结果就是我们的邮件通知会美观很多，而且还附加了评论所在地址，非常方便。
如果使用上述后台设置那么在主题下的配置文件，valine选项中的邮件通知要关掉，不然会收到两份通知：
```bash
valine:
  enable: true
  appid: xxxxxx
  appkey: xxxxxxx
  notify: false # mail notifier , https://github.com/xCss/Valine/wiki
  verify: true # Verification code
  placeholder: 无需注册，填写正确的邮箱，评论被回复就有邮件通知了~ # comment box placeholder
  avatar: retro # gravatar style
  guest_info: nick,mail,link # custom comment header
  pageSize: 10 # pagination size
  visitor: false
```
--------------------------
原文地址: [https://face2ai.com/other-Hexo-next-valine-leancloud](https://face2ai.com/other-Hexo-next-valine-leancloud)
