---
layout: post
title: Shadowsocks的原理
date: 2018-02-26
tags: Shadowsocks
---

　相信很多喜欢Google、Youtube、FaceBook...又具有折腾精神的墙内常住民，都有一段和GFW作战从乐此不疲到疲惫不堪的VPN折腾史，直到clowwindy大神说"要有光"，于是便有了Shadowsocks，迅速成为了比VPN更适合翻墙的利器，在这里把Shadowsocks的原理简单梳理一下

### 很久很久以前...

　很久很久以前，当天朝还没有GFW(Great Firewall of China，中国国家防火墙，俗称“墙”)，网民们访问各种网站都是简单而直接的，用户的请求通过互联网直接发送到服务提供方，服务提供方直接将信息反馈给用户

![](/images/posts/Shadowsocks/image1.png)

### 出现了一堵墙

　然后有一天，GFW就出现了，GFW像一堵墙一样夹在了在用户和海外服务之间，每当用户需要获取海外服务，都需要经过GFW，GFW将它不喜欢的内容统统过滤掉，于是客户当触发GFW的过滤规则的时候，就会收到 `Connection Reset` 这样的响应内容，而无法接收到正常的内容 

![](/images/posts/Shadowsocks/image2.png)

### 用代理绕过墙

　聪明的人们想到了利用海外服务器代理的方法来绕过GFW的过滤规则，其中包含了各种HTTP代理服务、Socks服务、VPN服务...其中以ssh tunnel的方法比较有代表性：

1) 首先用户和境外服务器基于ssh建立起一条加密的通道；

2~3) 用户通过建立起的隧道进行代理，通过ssh server向真实的服务发起请求；

4~5) 服务通过ssh server，再通过创建好的隧道返回给用户

![](/images/posts/Shadowsocks/image3.png)

### 然而...

　由于ssh本身基于RSA加密技术，所以GFW无法从数据传输的过程中，获取用户真实的服务请求与过滤规则进行比较，避免了被重置链接的问题

　但由于创建隧道和数据传输的过程中，ssh本身的特征是明显的，所以GFW通过分析连接的特征进行干扰，导致ssh存在被定向进行干扰的问题

### Shadowsocks出现

　于是clowwindy大神创造并开源了他的解决方案Shadowsocks，简单理解的话，Shadowsocks是将原来 ssh创建的Socks5协议拆开成Server端和Client端，所以下面这个原理图基本上和利用ssh tunnel大致类似：

1&6) 客户端发出的请求基于Socks5协议跟SS Local端进行通讯，由于这个SS Local一般是本机或路由器或局域网的其他机器，不经过GFW，所以解决了上面被GFW通过特征分析进行干扰的问题；

2&5) SS Local和SS Server两端通过多种可选的加密方法进行通讯，经过GFW的时候是常规的TCP包，没有明显的特征码而且GFW也无法对通讯数据进行解密；

3&4) SS Server将收到的加密数据进行解密，还原原来的请求，再发送到用户需要访问的服务，获取响应原路返回

![](/images/posts/Shadowsocks/image4.png)

### Shadowsocks未来的一小片乌云

　Shadowsocks近乎完美，然而随着机器学习和AI的越发成熟，干扰Shadowsocks也已经成为了可能，基于机器学习的随机森林算法(Random Forest Algorithm)在探测Shadowsocks流量时已经取得了超过85%的准确率 [IEEE论文地址](http://ieeexplore.ieee.org/document/8048116/?reload=true#full-text-section)

### 题外话

　曾经出现过所谓“增强版”ShadowsocksR引发争议，ShadowsocksR基于Shadowsocks代码进行二次开发，又违反GPL开源协议闭源，clowwindy大神曾经有过如下评论 [GitHub评论原文](https://github.com/shadowsocks/shadowsocks-windows/issues/293#issuecomment-132253168)：

> 我一直想象的那种大家一起来维护一个项目的景象始终没有出现，也没有出现的迹象。维护这个项目的过程中，遇到 @chenshaoju 这样主动分享的同学并不多。很多来汇报问题的人是以一种小白求大大解决问题，解决完就走人的方式来的，然而既不愿提供足够的信息，也不愿写一些自己尝试的过程供后人参考。互帮互助的气氛就是搞不起来。对比下国外的社区差好远。

> 最适合这个民族的其实是一群小白围着大大转，大大通过小白的夸奖获得自我满足，然后小白的吃喝拉撒都包给大大解决的模式。通过这个项目我感觉我已经彻底认识到这个民族的前面为什么会有一堵墙了。没有墙哪来的大大。所以到处都是什么附件回帖可见，等级多少用户组可见，一个论坛一个大大供小白跪舔，不需要政府造墙，网民也会自发造墙。这尼玛连做个翻墙软件都要造墙，真是令人叹为观止。这是一个造了几千年墙的保守的农耕民族，缺乏对别人的基本尊重，不愿意分享，喜欢遮遮掩掩，喜欢小圈子抱团，大概这些传统是改不掉了吧。

> 


