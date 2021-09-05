---
title: "DeepinGrub"
date: 2020-11-16T14:13:22+08:00
lastmod: 2020-11-16T14:13:22+08:00
keywords: ['Deepin', 'grub']
description: "解决Deepin启动时无法进入系统出现grub>的问题"
tags: ['Deepin']
categories: [OS]
author: "筱氚"
---

# Intro
此前我打算从Win世界再次回到Linux，于是从win的磁盘管理压缩出了130G 给Ubuntu，尝试了Ubuntu的脑壳疼问题之后抛弃了Ubuntu，然后就卸载了Ubuntu（在win下删除了130G的卷，删除efi里面的ubuntu）。然后在130G的区上装了Deepin，尝试了两天，爱了爱了，然后今天借是室友的电脑查看没装过双系统的efi同目录下有没有boot（因为有的博客说卸载Ubuntu的时候也要删除boot，但我没删），室友的电脑上Efi同级目录下有Boot,然后我打开我的电脑查看了一下，发现居然还有ubuntu，然后就把它删除了，结果就是系统进不去deepin了，现实GNU Grub，进入win后到efi建立ubutnu文件夹也于是无补。
# How
```grub
# 查看所有盘及其分区
grub> ls
(hd0) (hd0,gpt6) (hd0,gpt5)..... (hd0,gpt1) 
# 逐个测试分区文件系统，找到Deepin OS 所在的分区，返回Filesystem is ext2的就是了
grub> ls (hd0,1)
Filesystem is unknown
grub> ls (hd0,2)
Filesystem is unknown
grub> ls (hd0,3)
Filesystem is unknown
....
grub> ls (hd0,6)
Filesystem is ext2  # 就是它

# 设置引导分区位置
grub> set root =(hd0,6)
grub> set prexfix=(hd0,6)/boot/grub
grub> insmod normal
grub> normal
```
然后就可以正常进入Deepin了，然后打开终端，修复grub
```bash
# 查找efi所在盘符，我这里只有一块硬盘
$ lsblk
# 修复grub，dev后面需要更换！！！
sudo grub-install /dev/nvme0n1
```
如果显示No error就说明grub安装成功，但是我重启之后发现还是进入了grub>，说明是我把Deepin创建的ubuntu引导误删除了，而不是删除了玩耍ubuntu的ubuntu引导（这个早就被我删除了）。所以下面直接暴力把Deepin的efi引导粘贴到了ubuntu里面，结果是拯救回了Deepin，但是这应该不是正解。有没有装一个Deepin看一下两个efi是不是相同的？？？
```bash
$ cd /boot/efi/EFI
$ sudo cp deepin/* ubuntu/
```
这下重启就再也看不到那烦人的grub了呢。但是还是很疑惑这两efi到底一不一样。
# conclusion
Deepin 目前会创建ubuntu的引导，并且需要依赖它

# References
- [deepin开机进入grub> 解决办法](https://zhuanlan.zhihu.com/p/56661009)
- [修复启动 ](https://wiki.deepin.org/wiki/%E4%BF%AE%E5%A4%8D%E5%90%AF%E5%8A%A8)
- [【改进建议】 安装后EFI里多了Ubuntu ](https://bbs.deepin.org/phone/post/205094)
- [Linux命令的复制，移动/重命名、删除](https://blog.csdn.net/weixin_41646716/article/details/82082148)
