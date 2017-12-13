---
layout: post
title: 非 EFI 启动的 Linux 却出现了 symbol 'grub_efi_secure_boot' not found 错误而无法启动
excerpt: 产生原因尚未明确，但考虑 grub 就可以正常启动先了。
modified: 2017-12-09
tags: [HP ProLiant MicroServer Gen8, grub2, Linux, GPT, EFI, Linux, RedHat Enterprise Linux]
comments: true
pinned: false
---

考虑了好几天，还是决定在 Gen8 上只装个 Linux 系统就好了。

之前在跑 RedHat Enterprise Linux 7.3 ，最近查了一下订阅相关的问题，发现其实有个完全免费的开发者订阅（ Developer Subscription ）。

……红帽你咋不早告诉我呢！

兴致勃勃的注册了账号下载了最新的 7.4 版，反正我还没有什么重要数据于是就全新安装了。

谁知道装完就傻眼了。

----

## 喜闻乐见的 tl;dr 环节：

我在 /boot 分区发现了一个叫 efi 的文件夹。

大胆删掉，保险起见我重新安装了 grub ，就解决了。

----

## 以下是折腾流水账。

### 谈及 Linux 系统启动，不先说清楚分区布局是不对的

因为之前也验证过了 Gen8 其实可以从 GPT 分区的硬盘进行引导，所以这次安装的分区布局是这样的：

* BIOS boot 分区，位于 /dev/sda1
* /boot 分区，位于 /dev/sda3
* /srv 分区，位于 /dev/sda5
* /var 分区，位于 /dev/sda6
* swap 分区，位于 /dev/sda7
* / 分区，位于 /dev/sdb1

其中，sda 是位于 SATA 1 的 3T HDD ，sdb 是位于光驱位的 SSD 。没有使用 LVM 。

### 发现问题

有 [RedHat 的 bugzilla](https://bugzilla.redhat.com/show_bug.cgi?id=1350049) 提到是 grub2 识别的硬盘顺序和 BIOS 识别的不一样，得按 grub2 识别的顺序把线倒过来。

而且不止我的 RHEL ，起码这个 bug 反馈的 Fedora 24 ，还有我看到说 Debian 也有出现，看来这锅应该是 grub2 的。

然而我不觉得 Gen8 会把光驱位的 SATA 5 搞到前面去，用 grub2 的 ls 查证了一下也的确是 SSD 在 HDD 的后面，UUID 的指向也没有问题。

奇怪了，硬盘位置也对，Gen8 又根本就不支持 UEFI 启动，怎么会出来的这条 error: symbol 'grub_efi_secure_boot' not found ？

看看引导项的详细…… 嗯？那个 --hint-efi=(hd0,gpt3) 是啥？赶紧删掉然后 Ctrl-X ，然而并没有什么卵用……

于是开始怀疑是 /boot 拆出来可能引发了什么混乱，导致 grub2 可能跑去 SSD 上找 kernel 了？

虽然觉得能加载到 grub.cfg 所以上面这个猜测根本就是胡猜，但还是挂上了 iso 镜像戳进 Trouble Shooting 。

先看看 /boot 分区有啥…… 啥？ efi 文件夹？

RHEL 7.3 安装的时候也没有过啊！

### 解决问题

既然这都发现了，也没什么好说的了。

以防万一，我先把这个 efi 文件夹改了个名字。反正不是 efi 就对了。

虽然上面删了那个 --hint-efi 的参数也没什么用，还是就上面说的，保险起见，我执行了 grub2-install 来重新安装了一下 grub2 。

等了一会儿，没有出现错误，成功的结束了安装。很好。

马上重启试试，就正常的引导起来了！

### 新的问题

虽然系统是正常引导起来了，但我似乎还是太天真了。

关联好了订阅账户，很快就收到了系统更新的消息。

更新完毕正常启动，想把之前改了名但并没有删除的 efi 文件夹直接删掉，结果发现 /boot 下面又一次出现了 efi 文件夹。

这…… 看了一下，下面只有 EFI/redhat 这两个子文件夹。确认了一下之前改名的文件夹也是相同的结构，并没有 efi 的程序文件。

奇怪了，这样看来大概不止是 grub2 的问题————虽然确实想起了 grub2-install 的时候针对 EFI 启动的系统有个选项，

也就是说，其实安装进 BIOS boot 分区的 coreboot.img ，针对 BIOS 引导和 EFI 引导，可能存在两个版本。

那么，给出 grub2 安装参数的安装程序，是不是因为 GPT 分区的硬盘而导致误判了呢？毕竟之前 RHEL 7.3 的安装过程中没有发生过这样的问题。

我是不是该重现一下去给 RedHat 提个 bug ……

以上。
