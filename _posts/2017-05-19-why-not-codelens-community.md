---
layout: post
title: 为啥 Visual Studio 2017 community 就不能有 CodeLens
excerpt: …万一能多卖一份 Professional 呢！
modified: 2017-05-19
tags: [Visual Studio 2017 community, CodeLens]
comments: true
pinned: false
---

CodeLens 真真儿是个很酷的功能。

Visual Studio community 真真儿是个良心的产品。

我有个 community ，我有个 CodeLens ，uh ……

----

## 喜闻乐见的 tl;dr 环节：

别做梦了，上次 Visual Studio 2015 的那种歪门邪道还当日常了不成？

----

## 以下是折腾流水账。

### 前言

大家可能都知道之前在 Visual Studio 2015 系列的时候，就发生过本来是 Professional 版才有的 CodeLens 功能却因为捆在了 SQLServer Data Tools 里面，<br />
于是免费的 community 版就搭车解锁了 CodeLens 技能的故事。

燃鹅，目前的 Visual Studio 2017 community 却并没有这种车可搭。

在某知道网站看到甚至国际友人也纷纷祈求解锁 CodeLens 技能的时候，我觉得，是时候脱离一次伸手党了！

### 怎么着也得有个环境吧？

我很想批评一下巨硬社~~（虽然一点儿卵用都没有）~~，为啥就不能可怜可怜我们这种镜像爱好党，像曾经那样定期发布一下 iso 镜像呢……

就，反正挂着也是挂着，干脆一口气把 community 和 Professional 还有 Enterprise 三个版本全离线了。

事不宜迟，先装为敬。

![先装为敬](/assets/img/2017-05-19-why-not-codelens-community/p001.png "不管三七二十一我就先装了个 community")

And ，一边装，一边在 Professional 的 layout 里发现了这个：

![堂堂正正站在眼前的vsix](/assets/img/2017-05-19-why-not-codelens-community/p002.png "你好哇，CodeSense ！")

咩哈哈哈哈哈哈，明目张胆的放个安装包在这里，事情才不会这么简单！能把巨硬社当傻子吗？不知道特洛伊木马也好歹听说过空城计吧？

……倒是，我干嘛要跟自己过不去啊。TuT

![喜大普奔](/assets/img/2017-05-19-why-not-codelens-community/p003.png "你就不能给我个惊喜？非要给我个惊吓？")

### 要搞破坏总共分几步？别分了，拆就对了

要是早个几年，我可能还考虑看看这文件是不是 Cab 。似乎现在巨硬社也较少会用 Cab 格式的文件了？

嘛，总之，当 Zip 一拆就开。也就没什么好多说的了。

![拆拆拆](/assets/img/2017-05-19-why-not-codelens-community/p004.png "就跟拆礼物一样的爽快")

不过话说回来，缺少 manifest 这种事情怎么想也不太科学。

况且还有个 json 版的 manifest ，再怎么说也还是同一系列的产品，总不可能是两套标准的 vsix 吧？

算了，我先凑个 manifest 进来看看错误消息会不会变再说。

![缺啥补啥](/assets/img/2017-05-19-why-not-codelens-community/p005.png "补 patch 嘛，我补就是了")

![二次包装](/assets/img/2017-05-19-why-not-codelens-community/p006.png "你现在的身份是 vsix")

![以假乱真](/assets/img/2017-05-19-why-not-codelens-community/p007.png "去吧，少年！")

……嗯，果然变了。（手动允悲）

![再次喜大普奔](/assets/img/2017-05-19-why-not-codelens-community/p008.png "毫无意外地变了，然而并不是变成功")

### 没有自动挡还没有手动挡吗

反正文件都解出来了，community 我也装了，我自己把文件一个一个补进去还不行。

![还缺啥再补啥](/assets/img/2017-05-19-why-not-codelens-community/p009.png "补 patch 嘛，我继续补就是了")

![翻山越岭](/assets/img/2017-05-19-why-not-codelens-community/p010.png "你看目标文件夹的路径都对的上")

根据 manifest 的记载，整个儿把饱含了魔法 dll 的 CodeSense 文件夹拿进来。

![谁还不会个复制粘贴](/assets/img/2017-05-19-why-not-codelens-community/p011.png "再去一次吧，少年！")

对了吧！这下总行了吧！

……就不行。

代码窗口里既没有相关变化，设置里也没有多出来任何选项。

### 开不了车我坐车还不行

事已至此，懒得再截图了。<br />
总之就交代一下其他尝试过的手段：

* 看 manifest 里写的一些配置项似乎是有关注册表的，但其实不是。Visual Studio 2015 似乎这些都放在注册表里没错，但这次 2017 好像用了个 SQLite 的本地数据库。并没有试图解析这个（不过也许能行呢？）
* 安装程序的配置 json 里有 Professional 以上会安装 CodeSense 的部分。但是并不能改，改了就完全没法执行安装了。可能是安装程序会校验这个文件的 md5 或者 sha1 什么的？又或者是跟安装前导入的证书有关？
* 我都想办法 google 了，你还想让我怎么样。

### ……

去他喵的 CodeLens 。

老子去用 Visual Studio Code 了。

![债见](/assets/img/2017-05-19-why-not-codelens-community/p014.png "如果说伤心总是难免的，你又何苦一往情深")