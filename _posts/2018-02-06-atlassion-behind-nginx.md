---
layout: post
title: 使用 NGINX 反向代理 Atlassian 全家桶
excerpt: 除了局域网内可以使用域名，还能启用 https 呢。
modified: 2018-02-06
tags: [NGINX, Crowd, JIRA, Bitbucket, Bamboo, https]
comments: true
pinned: false
---

虽然是因为工作原因认识的 JIRA ，不过据说在 issue 管理上也是很厉害了，决定自己也来一套玩玩。

而且反正有了 SSL 证书，顺便了解一下 NGINX 的操作。

基本安装就不赘述了，Atlassian 全家桶的 Setup 简直不能更友好了。NGINX 也是加入 repo 直接使用 rpm 就好。

----

## 喜闻乐见的 tl;dr 环节：

请参阅下列官方指导文档：

* [NGINX 反向代理 Crowd](https://confluence.atlassian.com/crowdkb/how-to-use-nginx-to-proxy-requests-for-crowd-416583345.html)
* [NGINX 反向代理 JIRA](https://confluence.atlassian.com/jirakb/integrating-jira-with-nginx-426115340.html)
* [NGINX 反向代理 Bitbucket](https://confluence.atlassian.com/bitbucketserver/securing-bitbucket-server-behind-nginx-using-ssl-776640112.html)

关于 Crowd 多说一句，从 3.0 版开始，这篇 Guide 中提到的 crowd.properties 文件已经弃用了。实践证明只要在 General 页面中配置好 Base URL ，然后记得配置好 Trusted proxy servers 就可以了。

----

## 以下是折腾流水账。

### 一个不先去找官方文档的傻子分别尝试了以下的配置

    proxy_pass_header X-XSRF-TOKEN;
    proxy_set_header Origin "https://xxxx.example.com";
    port_in_redirect off;
    proxy_redirect http://127.0.0.1:8080 https://xxxx.example.com;
    autoindex on;
    autoindex_exact_size off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $http_x_forwarded_for;
    sub_filter_once off;

……emmmm。

### 重点其实不止在于 NGINX 的转发 header

按照官方 Guide 来看，其实还需要配置 Tomcat 的 Connector ，使其允许反向代理。

Crowd ，JIRA 和 Bamboo 都是这种套路。

Bitbucket 的新版因为已经切换到了 Spring boot 框架，所以不是修改 Connector 而是在 bitbucket.properties 指定相应的配置（ server.proxy-port 和 server.proxy-name ）。

然后，NGINX 里起码加入这些配置：

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

比如 Crowd 的 Guide 里还要求加入 proxy_redirect ，具体的请查阅对应产品的 Guide 文档。

### 理论上说到此为止应该结束了

最后就是，把各种 Application link 的 URL 换成 https 的就可以了。如果你也用了 Crowd 来统一管理用户，那么可以把 OAuth 设置成 impersonation 的（似乎是可以减少通信处理）。

不过我还有个问题是，似乎 NGINX 和 OpenSSL 的版本都对，但是不能开启 HTTP/2 。

虽然不知道具体是哪里的问题，而且局域网里其实也无所谓啦……
