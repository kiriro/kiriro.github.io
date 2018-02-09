---
layout: post
title: 为 iLO 4 添加 Let's Encrypt 的 SSL 证书
excerpt: 反正域名我也买了。
modified: 2018-01-30
tags: [iLO, HTTPS, Let's Encrypt]
comments: true
pinned: false
---

其实本来就是打算只在自宅的局域网里用的域名，虽说其实也没什么必要在局域网里开 HTTPS 吧。

NGINX 的配置就不说了，辣么简单。

虽说 iLO 的配置也没有难就是了。

----

## 并算不上 tl;dr 的 tl;dr 环节：

“管理” ＞ “安全性”，“SSL 证书” ＞ “自定义证书”，公用名 (CN) 填上 iLO 使用的域名，其他内容没关系，一定不要勾选“包含 iLO IP 地址”。

第一次点击“生成 CSR”按钮，iLO 会提示正在生成证书（但用不了十分钟那么久），过一会儿再进来点一次，把那串 Base64 字符串保存成文本文件。

然后在这个域名的 DNS 记录指向的那台主机上装好 certbot ，用下面的这条命令获取证书文件：

    certbot certonly --csr ilo.pem -d ilo.chiiruka.place

最后把 size 最大的那个 pem 文件的内容贴进“导入证书”按钮弹出的对话框里确定就行了。

以后更新的时候不需要再生成 CSR 了，只要 90 天内用 certbot 生成一下就好。

----

## 这次不用折腾了

### 但是参考过的网页链接我忘了，贴一下参考过的东西吧

* Let's Encrypt 官网的 certbot 文档
* python-hpilo-4.1 （其实根本用不上这玩意儿）
* dehydrated

当然啦，你得把域名指向 iLO 的局域网地址。改系统 hosts 也行，我改的路由器的 dnsmasq 。

### 一个应该不太容易踩到的坑

看到很多人提到 iLO 生成的 CSR 无法被一些 CA 签发证书的事情，基本上是因为上面提到的“包含 iLO IP 地址”的问题。

这个是 iLO 2.50 版本的 bug ，似乎是即使没有勾选这个选项，生成的 CSR 里也依然包含了 IP 地址的内容。

因为 Let's Encrypt 是识别这个字段但不允许存在的，而且又不能手动修改 CSR 的内容，所以要么降级 iLO 到 2.44 ，要么升级到 2.53 以上，来获取一个正常的 CSR 。

以上。
