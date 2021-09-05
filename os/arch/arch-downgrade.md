---
title: "Arch 安装旧版软件的办法（软件降级）"
date: 2021-05-28T22:55:11+08:00
lastmod: 2021-05-28T22:55:11+08:00
keywords: ['linux', 'arch', 'downgrade']
description: "因为要使用旧版的qemu,于是就有了这个记录"
tags: ['linux', 'arch']
categories: [os]
author: "筱氚"
---
# 简介

介绍在 Arch Linux 安装旧版软件或者软件降级的办法

## 为什么

Arch 默认是滚动发行的，更新速度那就是一周更新好多次，软件版本都是最新版。但这样也会存在其弊端，如版本太高不兼容会崩溃、开发版本远高于市场上的普及版本。

# 怎么降级

在[如何在 Arch Linux 中降级软件包](https://linux.cn/article-9730-1.html)中如此说到
> 此实用程序将检查你的本地缓存和远程服务器（Arch Linux 仓库）以查找所需软件包的旧版本。
也就是说旧版本还是存在的。


于是在几乎无所不能的 Arch Wiki 中搜索了一下，发现了[Downgrading packages Arch Wiki](https://wiki.archlinux.org/title/Downgrading_packages)中有利用本地的 pacman 缓存（/var/cache/pacman/pkg） 或者官方管理的包档案 [Arch Linux Archive](https://archive.archlinux.org/)等 5 种办法。

总结一下简单的办法：  
1. 本地 pacman 缓存，通常会有最近的两个版本
2. Arch Linux Archive，存有全部的版本
3. 借助自动化软件  downgrade

以上几种办法可以安装旧版的软件包，问题在于安装之后能否正常运行那就是另一回事了。比如我下面的 demo 降级这三种办法都因为某些依赖库版本比较高而无法正常使用；当然也有使用上面方法可以正常使用的时候。

## pacman cache

```bash
# 切换到缓存目录文件下
cd /var/cache/pacman/pkg/
# 查找有哪些版本
find ./*PACKAGE_NAME*
# 从本地的 pkg.tar.zst 进行安装
sudo pacman -U /var/cache/pacman/pkg/package-old_version.pkg.tar.zst
```

## Arch Linux Archive

到 [Arch Linux Archive](https://archive.archlinux.org/) 找到 package 对于的版本的路径，然后使用 pacman 进行安装。当然下载到本地再安装也是可以的。

```bash
sudo  pacman -U https://archive.archlinux.org/packages/path/packagename.pkg.tar.xz
```

## downgrade

这个软件软件把上面两个办法结合起来，省去其中麻烦的过程，实现几行代码自动化。[如何在 Arch Linux 中降级软件包](https://linux.cn/article-9730-1.html)中描述的办法由于 downgrade 已经转移到 AUR 中，因此原文中的办法有关于 downgrade 的安装办法已经失效了。以下是通过 yay 安装的办法

```bash
# 安装 downgrade
yay -S downgrade
# 对包 PACKAGE_NAME 查询可降级的版本，选择对于的版本即可
sudo downgrade PACKAGE_NAME
```
# Demo

我需要对 qemu, qemu-arch-extra 两个包进行降级到 5.0.0，以适应学习需要。

```bash
$ sudo pacman -S downgrade
$ sudo downgrade qemu-arch-extra
- 25) qemu-arch-extra    5.1.0  3  /var/cache/pacman/pkg
$ sudo downgrade qemu
- 21) qemu    5.0.0  8  /var/cache/pacman/pkg
## 测试安装效果
$ qemu-riscv64 -version
qemu-riscv64 version 5.1.0
Copyright (c) 2003-2020 Fabrice Bellard and the QEMU Project developers
$ qemu-system-riscv64 -version
qemu-system-riscv64: error while loading shared libraries: liburing.so.1: cannot open shared object file: No such file or directory
# 晚年依赖库问题，把库也进行降级
$ sudo downgrade liburing
-  5)  liburing    0.7  2  /var/cache/pacman/pkg
```

在降级过程中，downgrade 会询问要不要把该软件包加入更新白名单，这样子更新系统的时候就不会更新这个包。最好还是加入白名单，说不准小手一抖就更新了呢。这个配置项会在 `/etc/pacman.conf` 中的 `IgnorePkg`，可以手动修改。



# 参考

[如何在 Arch Linux 中降级软件包 作者： Sk 译者： LCTT MjSeven | 2018-06-08 22:27](https://linux.cn/article-9730-1.html)：archlinuxfr 仓库中的 downgrade

[Downgrading packages Arch Wiki](https://wiki.archlinux.org/title/Downgrading_packages)

[Arch Linux Archive](https://archive.archlinux.org/)：使用 pacman 安装旧版包需要自己解决依赖问题，倒是说一下如何解决嘛。
>Package verification is controlled by pacman's RemoteFileSigLevel option. Note that if you use pacman, you have to figure out the dependencies yourself. 

[Frequently_asked_questions Arch Wiki](https://wiki.archlinux.org/title/Frequently_asked_questions)：没找到合适的库
>If_I_need_an_older_version_of_an_installed_library,_can_I_just_symlink_to_the_newer_version?
Instead, e.g. use/write a compat package, which provides the required library version. 

[[rustsbi-panic] system shutdown scheduled due to RustSBI panic #18](https://github.com/rcore-os/rCore-Tutorial-v3/issues/18):recore 学习中遇到的qemu与rustsbi版本问题