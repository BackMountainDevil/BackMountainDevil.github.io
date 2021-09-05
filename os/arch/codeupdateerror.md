---
title: "Arch 下 VS Code, Ciano 升级导致 lib*.so* 错误以及安装旧版（软件降级）的办法"
date: 2021-04-29T09:13:42+08:00
lastmod: 2021-04-29T09:13:42+08:00
keywords: ['texteditor', 'arch', 'code', 'update']
description: "Arch 下 VS Code、Ciano 升级导致 lib*.so* 错误以及安装旧版（软件降级）的办法"
tags: ['texteditor', 'arch', 'code']
categories: [vscodium]
author: "筱氚"
---

# 问题描述

## 背景

将近一个多月没有更新软件了，打开 Discover 发现六百多个没更新的，就选了几个常用的软件进行更新（fcitix5、VS Code、Ciano）。更新完成之后发现无法启动 VS Code，重启之后依然如此。

- code version: 1.55.2.1
- Ciano version：0.2.4-2, released on 2020/7/12
- os: Linux arch 5.11.6-arch1-1 #1 SMP PREEMPT Thu, 11 Mar 2021 13:48:23 +0000 x86_64 GNU/Linux
- 启动方式：菜单栏或者 Task Manager

## 错误信息

点击启动图标啥反应也没有，没有错误提示、警告，没有闪现，啥也没有。于是乎想起了终端大法。

```bash
$ code
electron11: error while loading shared libraries: libicui18n.so.69: cannot open shared object file: No such file or directory
```

## 问题探究

这类 so 文件的问题之前也遇到过，一种是软件更新没及时更新对于的文件，第二种是依赖关系导致的暂不能更新这个文件，还有一种是压根没有这个文件。

常规套路是文件没有更新，创建个软链接即可。

### 软链接大法

```bash
$ whereis code
code: /usr/bin/code /usr/lib/code /usr/share/man/mann/code.n.gz
$ which code
/usr/bin/code

# 看缺啥动态库，模仿别人
$ ldd /usr/bin/code
        不是动态可执行文件

$ whereis libicui18n
libicui18n: /usr/lib/libicui18n.so

$ cd /usr/lib
$ whereis libicui18n.so.*
libicui18n.so: /usr/lib/libicui18n.so.68 /usr/lib/libicui18n.so
libicui18n.so.68: /usr/lib/libicui18n.so.68 /usr/lib/libicui18n.so.68.2

# 看看到底有啥，上面看不太懂
$ find /usr/lib libicui18n* 
libicui18n.so
libicui18n.so.68
libicui18n.so.68.2

# 看看真身是谁
$ file libicui18n.so
libicui18n.so: symbolic link to libicui18n.so.68.2
$ file libicui18n.so.68
libicui18n.so.68: symbolic link to libicui18n.so.68.2
$ file libicui18n.so.68.2
libicui18n.so.68.2: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, BuildID[sha1]=7d9f41d382792b6d6eb0d2fd54dd0ffadf654464, stripped

# 上面结果显示.so，.so.68都是软链接，都指向.so.68.2
$ sudo ln -s libicui18n.so.68.2 libicui18n.so.69
```

然后启动很尴尬啊。。看来不止一个 so文件 。。。链接太多也不是个办法

```bash
$ code
electron11: error while loading shared libraries: libicuuc.so.69: cannot open shared object file: No such file or directory
```

罢了罢了，删除刚才建立的软链接，恢复原状

`sudo rm libicui18n.so.69`

> [更新系统](https://wiki.archlinux.org/index.php/System_maintenance_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E6%9B%B4%E6%96%B0%E7%B3%BB%E7%BB%9F)  
> 如果进行了部分升级，二进制包因为找不到链接库而损坏，不要通过简单的符号链接进行修正。库升级 soname 是因为它们不再向前兼容。只要 pacman 可以运行，使用更新的源进行 pacman -Syu 就能修复这些问题。 

### 重装大法

后面在 Discover 卸载 code,打算重装的时候提示找不到仓库无法安装。。然后用 `sudo pacman -S code` 来安装，发现安装的还是 code-1.55.2-1 ，运行还是会存在上面遇到的问题。最后用 `sudo pacman -Rs code` 卸载了 electron11-11.4.3-2  code-1.55.2-1，好奇怪。。上次我打算卸载atom 的时候提示我 electron 冲突无法卸载。这次顺手（`sudo pacman -Rs atom`）把 atom （apm-2.6.1-2  electron6-6.1.12-5  ripgrep-12.1.1-1  atom-1.55.0-1
）也卸载了。

### 安装旧版大法

既然新版不行，那我把 code 版本降级回去，找到了一个关于 [downgrade](https://linux.cn/article-9730-1.html) 的办法，不过尝试之后已经失效了，找不到该目标软件包。最后求助 Arch Wiki 中关于降级（[Downgrading_packages Arch Wiki](https://wiki.archlinux.org/index.php/Downgrading_packages)）的描述

```bash
$ cd /var/cache/pacman/pkg/

# 查找有几个版本
$ find ./ code* 
code-1.53.2-1-x86_64.pkg.tar.zst
code-1.54.2-1-x86_64.pkg.tar.zst
code-1.55.2-1-x86_64.pkg.tar.zst

# 选择54版
$ sudo pacman -U /var/cache/pacman/pkg/code-1.54.2-1-x86_64.pkg.tar.zst

# 启动测试
$ code
```

启动成功，发现 code 插件都还在，卸载的时候没有自动卸载对应的插件

# Ciano

解决完 code 的问题之后才发现 Ciano（一款格式转换工具） 无法启动，命令行里也不支持 ciano 。。。。

```bash
$ ciano
bash: ciano：未找到命令
$ Ciano
bash: Ciano：未找到命令
$ whereis ciano
ciano: /usr/share/ciano
# 打开之后发现是个图片文件夹

$ cd /usr/bin
$ find ./ *ciano*
com.github.robertsanseries.ciano

# 从终端启动 ciano 
$ com.github.robertsanseries.ciano
com.github.robertsanseries.ciano: error while loading shared libraries: libgranite.so.6: cannot open shared object file: No such file or directory
```

后来在 `/usr/share/applications` 找到其文件描述中有 `Exec=com.github.robertsanseries.ciano`， 才知道这个是应用的终端。不过这不重要，重点是升级导致库文件没及跟上导致互不相认。。。卸载重装吧
```bash
$ sudo pacman -Rs com.github.robertsanseries.ciano
错误：未找到目标：com.github.robertsanseries.ciano
# 这就离谱了，包名不是这样的吗？？？
# 在 Discover 中可以卸载，输入密码确认前我打算试一下ciano

$ sudo pacman -Rs ciano
正在检查依赖关系...
软件包 (3) granite-5.5.0-1  libgee-0.20.3-2  ciano-0.2.4-2
全部移去体积：  3.39 MiB
# 佛了，包名和终端启动名还不一样哈。。。

$ cd /var/cache/pacman/pkg/

# 查找有几个版本，用 find /var/cache/pacman/pkg/ *ciano* 查找不到的原因我还不知道。。
$ find ./ *ciano* 
ciano-0.2.4-1-x86_64.pkg.tar.zst
ciano-0.2.4-2-x86_64.pkg.tar.zst

# 选择54版
$ sudo pacman -U /var/cache/pacman/pkg/ciano-0.2.4-1-x86_64.pkg.tar.zst

# 启动测试
$ com.github.robertsanseries.ciano
com.github.robertsanseries.ciano: error while loading shared libraries: libgranite.so.5: cannot open shared object file: No such file or directory
```

我佛慈悲。。。看来版本不够低，看了 Discoer 中关于 Ciano 的评论中 Stark 说这个项目死了两年，到 [ciano - github/release](https://github.com/robertsanseries/ciano/releases)看了下确实 0.2.1 到 0.2.2 之间跨越了两年，但在 2020 生到了 0.2.4, 2021 还没有新版（有在维护），release 中也没有 最近版本的 deb 可以装（只有个 0.1.4）。。源码编译安装吗？？下次一定

软件更新日至可以在 pacman 日志文件（`/var/log/pacman.log`）中进行查找，我搜了下 ciano 发现我最早安装这个软件是在 2021-03-01，版本是 0.2.4-1，那为什么我重装回这个版本却无法使用了呢？再次看[更新系统 Arch Wiki](https://wiki.archlinux.org/index.php/System_maintenance_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E6%9B%B4%E6%96%B0%E7%B3%BB%E7%BB%9F)中的描述才知道我这样的行为属于部分更新，然而 arch 对部分更新的支持性是不太友好的。

# 软链接

类似 windwos 下的快捷方式

## 创建

`ln -s 源文件名 软链接文件名`

## 修改

`ln –snf 新源文件名 软链接文件名`

## 删除

`rm 软链接文件名`

## 查找

### 全部软链接

`ls -alR | grep ^l`

### 失效软链接

原文件已经被删除了，但是软链接还在

`find . -xtype l`

# 总结

用 where 查找是在 linux 每日自动生成的数据库里查找，有时候心文件还来不及收录进数据库，知道在哪一个地方用 find 查找会更好

打算更新 Discover 中六百多个更新的时候提示 `Dependency resolution failed libcap=2.48`。。。。之前只有几个更新的时候打算更新，也是遇到类似的问题，一直没有更新积累到现在有六百多个更新。。。悄咪咪只更新了几个软件还无法使用。。。啊这

所幸这次安装旧版的 code 解决了，但是这还是不可理喻，不更新系统的前提下更新软件居然不可用。。。

得思考下解决这些依赖问题然后全盘升级了。

# 参考

[Arch Linux上使用 pandoc 将 markdown 转为 pdf 以及如何查看本机的中文字体 fc-list :lang=zh Kearney form An idea 2021-03-17](https://blog.csdn.net/weixin_43031092/article/details/114584385):之前在这里遇到过 so 文件的类似问题，软链接治标不治本，后面通过升级解决的
>pdftex: error while loading shared libraries: libpoppler.so.108: cannot open shared object file

[uwsgi: error while loading shared libraries: libicui18n.so.58: cannot open shared object file Alex 007 2020-07-24](https://blog.csdn.net/weixin_43336281/article/details/107561560)

[Linux建立软链接、硬链接 menghefang 2019-03-17](https://blog.csdn.net/menghefang/article/details/88624161)

[Linux查找文件系统中的失效链接 Zombie's blog 2018-12-27](https://tenglog.com/posts/linux-find-all-broken-symlinks.html)：三种find查找，一种（| xargs rm -rf）删除

[Linux删除软链接 linux凯 2016-03-23 ](https://blog.csdn.net/chenghuikai/article/details/50961622)

[Downgrading_packages Arch Wiki](https://wiki.archlinux.org/index.php/Downgrading_packages)：亲测有效

[downgrade 如何在 Arch Linux 中降级软件包 作者： Sk 译者： LCTT MjSeven| 2018-06-0](https://linux.cn/article-9730-1.html)：测试结果显示该软件没了。。后面从[downgrader-git](https://aur.archlinux.org/packages/downgrader-git/)看到这是一个 AUR 中的包，下载方式应该是借助其它 AUR helper 来下载，比如 `yay -S downgrade`，文中的方式可能是以前的下载方式吧。

[更新系统 Arch Wiki](https://wiki.archlinux.org/index.php/System_maintenance_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E6%9B%B4%E6%96%B0%E7%B3%BB%E7%BB%9F)
