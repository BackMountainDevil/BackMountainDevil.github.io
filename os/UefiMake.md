---
title: "UefiMake"
date: 2020-11-10T22:59:36+08:00
lastmod: 2020-11-10T22:59:36+08:00
keywords: ['Ventoy', 'Uefi']
description: "这里要介绍的是2020开源界的一大清流(最好的U盘启功制作工具) - Ventoy"
tags: [UBoot]
categories: [OS]
author: "筱氚"
---
# Introduction
作为世界最大头秃大军中的一员，操作系统的安装是一项必备技能，自己亲自操刀的OS就有不少：win7、win10、Centos、Ubuntu、Deepin、kali等。其中最头疼的就是每一个OS都要重新刻录U盘。。。真的是繁琐。其中不少工具都内置很多垃圾广告和应用，比如：大白菜、老毛桃、WinPE，而且有的还收费Ultra ISO。。

下面要介绍的是2020开源界的一大清流(最好的U盘启功制作工具) - Ventoy
## Ventoy
如果你知道什么是大白菜、老毛桃，那通俗的介绍Ventoy就是它们的替代品；如果你不知道，Ventoy就是用U盘装系统的刻录软件。在以往，想用一个U盘安装系统，就需要上面的某个软件刻录iso到u盘里面，而且一旦想换一个系统，比如win10_i386换到win10_amd64还要重新刻录，操作繁琐且耗时。有的iso还需要专门的刻录工具（如Deepin）。
### 优点
1. 无需反复地格式化U盘。你只需要把 ISO/WIM/IMG/VHD(x)/EFI 等类型的文件拷贝到U盘里面就可以使用，无需其他操作。
2. 可以一次性拷贝很多个不同类型的镜像文件，Ventoy 会在启动时显示一个菜单来供你进行选择。
3. 无差异支持Legacy BIOS和UEFI模式。
4. 支持大部分OS。如Ubuntu、Debian、Deepin、Windows、FreeBSD。
5. 开源免费无广告无捆绑。
6. 热更新Ventoy不影响U盘内容（可以放安心学习资料了）。
### 缺点
1. 不支持IA32的UEFI,比如Mac。

# How to do
参考[Ventoy-Usage](https://www.ventoy.net/cn/doc_start.html)操作即可。亲测小新13pro安装Ubuntu成功。
# References
- [Ventoy](https://www.ventoy.net/cn/index.html)
- [Ventoy-Github](https://github.com/ventoy/Ventoy)
- [Ventoy-Usage](https://www.ventoy.net/cn/doc_start.html)