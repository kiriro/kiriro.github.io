---
layout: post
title: Data was successfullly saved.
excerpt: 存储是个坑。天坑。
modified: 2017-09-16
tags: [WD My Cloud]
comments: true
pinned: false
---

据说掉进了存储的坑就别想再有机会出来了。

于是我一直都没正儿八经的研究群晖、QNAP 甚至 HP Gen8 这些名副其实甚至超越了 NAS 本身的产品。

不过我还是买了台西数的 My Cloud 。

----

## 喜闻乐见的 tl;dr 环节：

身为一个极轻度 user ，目前在用原版系统。

我的使用场景非常简单，树莓派上跑 Kodi 能找到电影和剧，PC 和 mac 还有另外一只充当 web 服务器的树莓派能有公用的存储空间就行了。所以其实一个 Samba 就能解决。

不过鉴于性能问题，目前 Gen 1 似乎只有 04 版本的官方系统能跑到千兆网络的满速，纯 Debian 读取可以满速但写最多 70M 好像。后面会详说。

当然，过段时间我考虑拆个 Gen 2 的主板继续跑纯 Debian 。

----

## 以下是折腾流水账。

### 不管怎么说是不是应该先晒一波

不好意思，其实我一共买了四台。

![而且全部来自闲鱼](/assets/img/2017-09-16-cloud-in-my-room/IMG_8362.JPG "…是不是有点儿任性了")

从左到右依次是：
* Gen 1，国行，3T，九月之前在用的一台。收来之前大概用了一个多月。15年产。
* Gen 1，可能是美版，收的主板和外壳。买了个 2T 蓝盘放在里面，目前在用。主板 14 年产。LED 的蓝色灯好像快坏了。
* Gen 2，从型号上推断是美版，4T。收来的时候全新。16年产。
* Gen 2，国行，3T。收来之前据说写入不到 1T ，但是看了下通电有 4 个多月，还有十几次的不安全断电（可能是升了新固件的导致的找不到关机功能的 bug 所以……）。不过全盘格式化一遍怎么说写入也上 4T 了。17 年产。

之所以标注了一下生产年份，其实是收到第二台 Gen 1 的时候发现的配件的变化：

![CPU 首当其冲的吸引了我的视线](/assets/img/2017-09-16-cloud-in-my-room/IMG_83178.JPG "左边 14 年，右边 15 年")

虽然 CPU 这里确实很醒目，不过后来一查其实是飞思卡尔在 14 年下旬就已经收购了 MindSpeed 的 CPU 产品，于是乎 15 年的货上就有了飞思卡尔的 logo ……

另外，根据歪果大神 Fox_exe 的情报，日版有一个单独的型号 (BAGX | Mirror Man) 。

不过我查证了一下，虽然是 15 年开始出货，不过硬件方案和 Gen 2 应该是一样的。但是，日版到现在也没有提供过新固件，也没有任何可以通刷 Gen 2 (Glacier) 的报告。但是，这个版本却提供了和 Mirror 还有 EX2 那样的可以安装的官方应用，比如 Transmission 和 Wordpress 等（我知道 Gen 2 也能装，但是需要一点 hack 手段，而日版是默认就可以的）。不知道有机会要不要收一台回来。

另外一点关于日版的小事情。日版这款是由 IO Data 代理的，日本国内的产品名称是“WD Cloud”。在日本这么喜欢 i 啦 my 啦的地方为啥去掉了“My”呢，是因为…… 有个大企业叫做富士通，富士通个人电脑的产品线里，会员的在线服务的部分就叫“My Cloud”。想必是跟这种大企业争个商标也没什么意思，干脆就给产品改个名算了……

### 有什么不一样

简单再提一下硬件差异吧：
* Gen 1：MindSpeed Comcerto C2000 (650 MHz x 2 | A7l), 256MB Ram
* Gen 2：Marvell Armada 370 (800 MHz x 2 | A7l), 512MB Ram

先说上面提到的关于 Samba 的性能问题。Fox_exe 在 WD 的讨论区里有提到可能是 CPU 的瓶颈，也就是之所以 WD 会在 04 版固件里宁可放弃默认兼容也要使用 64k 内存分页内核的原因，就是可以减少 CPU 的换页工作量来避免对文件传输效率的影响。而且根据他的实际测试，在不对 Samba 进行参数优化的情况下，默认 4k 分页的内核已经和 64k 分页的内核有了不小的差距。优化之后读取速度可以稳定在 90MB 左右，写速度大约是在 70MB 。剩下的就是 WD 对于 Samba 本身的优化了，主要是针对 tsocket 和 recvfile 的部分（很明显了吧）。具体处理我没仔细看，但应该是针对 64k 大页面的处理次数之类的优化什么的。

顺便吐槽一下 Samba 代码的组织…… source3 和 source4 这两个文件夹分别是指 Samba 3 和 Samba 4 的代码么？然而看起来 WD 只改了 Samba 3 的部分，也就是说虽然升级到了 Samba 4 的程序但是根本就没在用新特性？不是很懂这种操作……

Gen 2 的官方固件虽然在 UI 上实现了统一（甚至包括 Mirror Gen 2 和 EX2 Ultra 这些），首页增加了 CPU 和 RAM 的使用状况，只可惜“应用”这里默认只能用官方给的 miniDLNA 和 HTTP 下载这些也算不上有多大用途的应用。目前有两种 hack 方式，一种是直接用浏览器的开发者工具设置标记并同意协议（把那个“同意”按钮改成有效还不如直接调用 js 方法快），另一种是使用 Fox_exe 提供的 define.js 覆盖掉官方固件里的。经过考证我还是更推荐第二种，除了其实第一种里改的那个标记其实就是从 define.js 里获取的这点以外，顺便还解决了最近的版本里没有关机功能的 bug 。而且都好几个版本了 WD 也没把关机功能弄回来，更何况 Gen 1 的关机功能也还好好的在那儿，完全说不上什么要统一去掉的东西，也不知道他们到底是怎么想的……

到了 Gen 2 ，Samba 就不存在性能瓶颈了。使用纯 Debian 的 Samba 4 也可以轻松跑满千兆有线网。同时得益于多出来的内存，Gen 2 甚至可以跑一跑简单的 Java Web 应用。纯 Debian Jessie 加 JDK 8 ，然后 Tomcat 8 的默认管理控制台的时候占用了大约 60MB 的内存。除了 CPU 的提升，应该也跟磁盘 IO 性能沾了不少光，结果是跟树莓派的 Tomcat 体验比起来流畅了不少（树莓派 2B+ 用 C10 卡只跑管理控制台都有点反应迟钝）。

### 最后是一片碎碎念

为什么上面说还是要拆个 Gen 2 的主板出来装纯 Debian 用呢？

起因主要是打算固定放个 MySQL 的数据库在上面，结果我没事找抽的直接在官方固件上装了 Fox_exe 编译的 64k MySQL ，没一会儿 SSH 和 Web UI 就没反应了……（虽说欣喜的发现 Samba 竟然还没挂，于是还看了部电影……）

嘛，Linux 内核本身的特性是有为了保证不会整体崩掉而 kill 进程的事情我在[默默的这篇文章](https://www.mobibrw.com/2014/1477)里其实有看到过啦。所以应该是内存根本就不够的原因。

不过我又不太想为了省出那么一点内存来就放弃官方固件优化好的最高 Samba 速度（虽说其实也不差那点），外加默默的那个方法之前用的时候发觉可能会阻止硬盘进入休眠状态（不完全确定但确实做了这个处理之后就很久都不休眠），就不打算在官方固件上魔改了。

Gen 2 上纯 Debian 其实试过，但是之前翻的资料比较老，所以后来才发现 ram2fs 在 Jessie 上不能运行根本就是正常情况。外加其他一些略有点搞砸了的操作，干脆就暂时放下了。（否则也不会有上面 Tomcat 试运行体验的结论了。）

而且有一点，Gen 2 上纯 Debian 有个好处是，系统分区的大小实际上可以分小一些，多给数据区留几个 G …… 相比之下 Gen 1 的分区结构就比较死板，不过据说 Raid 对于安装纯 Debian 来说是可以取消的，实际上也能达成系统占用空间的削减。

剩下的问题，就是 Ext4 文件系统 inode 的占用空间问题了。2T 大小的分区差不多要吃掉 30G ，确实有点儿心疼。但要是改 largefile 的 1M block ，就只好专注大文件了，对于还打算放放 PS3 游戏的计划来说可能会产生不少浪费（虽说应该也不会浪费到 30G 这种程度……）。也许会先在某测试盘上试着找找平衡点吧。

以上。