---
layout: post
title: PostgreSQL in Linux
excerpt: 服务器真厉害。
modified: 2017-12-11
tags: [PostgreSQL, RedHat Enterprise Linux]
comments: true
pinned: false
---

之前有些试用，还有自己做了点东西，后来整个儿都集中在了 PostgreSQL 上。

反正现在有了正儿八经的服务器，就干脆移过去好了。

----

## 喜闻乐见的 tl;dr 环节：

「sudo」。

----

## 以下是折腾流水账。

### 你们怎么都不用系统自带的 PostgreSQL ？

翻了好多篇基本上都是说“如何卸载 RHEL / CentOS 自带的 PostgreSQL 然后装 x.y 版”，嘛，总的来说参考价值还是有的。

如果是安装系统的时候就选了 PostgreSQL 的软件包，那么除了执行相关命令的时候不需要带版本号以外，大概没什么区别。

### 账号系统是闹哪样

因为我连了半天都连不上。说好的默认密码为空呢？

结果总算翻到 [这篇文章](http://blog.csdn.net/shangzwz/article/details/8601700) ，实际上切换到 postgres 这个账号的时候，如果不是 root ，那么是需要 sudo su postgres 的……

……不说了，我去登 root 了。

### 连不上，怎么也连不上

一直都在反问我 IP 有没有搞错服务端口是不是正确。

然而并没有毛病啊，监听地址也改成了“ * ”，端口其实默认有环境变量，但我还是解开了配置文件里的注释。

如果是 hba 配置有误应该是 refuse 之类的错误吧，难道是防火墙没开？但翻了几篇都没说要开……

最后还是在 [这篇文章](http://www.cnblogs.com/qiyebao/p/4562557.html) 的后面发现了关于防火墙的设置。果然。

话说我都装了软件包了你都不帮我开一下防火墙…… 算了我还是去学 iptables 的使用好了。

### 新的问题

这么晚了，该睡了。

以上。
