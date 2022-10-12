# Arch Pacman & Yay 介绍+基本使用 & 更新中无法满足依赖关系的解决办法
- date: 2021-04-30
- lastmod: 2022-10-12

# pacman

虽然 arch 衍生自 debian, 但是在包管理上采取了一种不太一样的工具，不是常见的 apt 而是 pacman

## 常用命令

| 指令  |   说明    |
| :---- | :---- |
|   pacman -S 软件包名    |   安装软件及其依赖    |
|   pacman -Rs 软件包名   |   卸载软件及其依赖    |
|   pacman -R 软件包名    |   仅卸载该软件，保留其依赖（不推荐）    |
|   pacman -Syu    |   更新整个系统和软件    |
|   paccache -r    |   删除不使用的包，但保留最近的三个版本    |
|   pacman -Sc    |   删除 pacman 缓存的软件包和无用的软件库（旧版安装包和已经安装过的安装包）    |
|   pacman -Scc    |   删除缓存中的全部文件，一个不留，谨慎操作！！！   |

非 root 用户的管理员需要在前面加上 sudo; 如何将 deb 包转换为 arch 使用的包格式，请参考 [debtap](/os/arch/debtap.md)。

由于 pacman 的设计上不会自动移除旧的和未安装版本的软件包，所以 `/var/cache/pacman/pkg/` 中积累的软件包数量会越来越多，占用存储空间越来越大（半年多积累了18 G），因此需要自己清理这些缓存（paccache -r）。但是我懒的自己去做这件事情，开启每周自动清理功能即可 `sudo systemctl enable paccache.timer`。

一年多使用下来，感觉保留安装包没太大用处，于是就参照wiki将保留版本设置为 0，还有一个是可以在 /etc/pacman.conf 中设置 CacheDir 的路径，默认是  /var/cache/pacman/pkg/，我改成了 /tmp/pacman/，这样下载软件包的时候就不会读写到硬盘而是读写到内存中，需要注意的是内存可利用空间，我 16G 内存能够给用户自行操作的起码有个 6 G，因此保持一周更新一次的速度，内存还是可以接纳的，不过这个办法有个不足之处在于由于不存在 /tmp/pacman/ 文件夹，所以 pacman 在创建的时候会警告一次，当然，解决办法就是在更新前新建该文件夹。

## 仓库

pacman 默认只开启三个官方仓库：core, extra, community. 默认不开启 multilib（ 32 位软件库 ），配置文件为 `/etc/pacman.conf`。目前来说我只有 wine 需要开启 multilib。

更多见[Official repositories arch wiki](https://wiki.archlinux.org/index.php/Official_repositories)


## 镜像

这里不需要特别的加入某个镜像源（清华、阿里等），因为 arch 提供了一种[自动测速更新机制](https://wiki.archlinux.org/index.php/Mirrors#Ranking_an_existing_mirror_list)。

```bash
$ cd /etc/pacman.d
# 备份原始镜像配置文件
$ cp mirrorlist mirrorlist.backup
# 在国区里选 6 个速度最快的镜像源，此办法与 Wiki 中略有不同
$ sudo reflector -c China -a 6 --sort rate --save /etc/pacman.d/mirrorlist
```

# AUR helper

有一些软件并没有被收录进官方仓库中，这一类软件就会被收录进 AUR 中，要从 AUR 中安装软件需要借助一切工具，如 yay、Pakku、Pacaur 等。

## yay

### 安装

```bash
sudo pacman -S yay
```

### 使用

| 指令  |   说明    |
| :---- | :---- |
|   yay -S 软件包名    |   安装软件    |
|   yay -Rs 软件包名    |   卸载软件及其依赖    |
|   yay -Ps    |    	输出版本信息、软件包数量、总大小、前十个最大的软件包    |
|   yay -Yc    |    	清理不再需要的依赖包    |

举个例子

```bash
# 安装 UOS 版微信
yay -S wechat-uos
# 安装 chrome 浏览器
yay -S google-chrome
# 安装 WPS 中文简体版
yay -S wps-office-cn wps-office-mui-zh-cn ttf-wps-fonts 
# 安装 科学上网 的工具
yay -S lantern
# 哪天不开心就卸载
yay -Rs wechat-uos
yay -Rs google-chrome
```

和 pacman 一样，也可以将软件包缓存目录设置为内存，在 `~/.config/yay/config.json` 文件中，buildDir 默认为 `~/.cache/yay`，将其修改为 `/tmp/yay` 便可达到目的

临时解决方案

```bash
$ lantern
Running installation script...
/usr/lib/lantern/lantern-binary: 成功
/home/kearney/.lantern/bin/lantern: error while loading shared libraries: libpcap.so.0.8: cannot open shared object file: No such file or directory
$ cd /usr/lib
$ sudo ln -s libpcap.so.1.10.1 libpcap.so.0.8
```

# deb

有的软件可能只找到 deb 安装包，但是 arch 的软件包格式并不是 deb,关于用 debtap 将 deb 软件包转换成 arch 下的软件包格式请参考[用 debtap 将 deb 转换成 arch 的软件包格式，以 Raspberry Pi Imager 为例](../debtap)

# 部分升级的问题

[只升级某些软件导致的问题](./codeupdateerror.md)，

```bash
$ sudo pacman -Syu
:: 正在同步软件包数据库...
 core 已经是最新版本
 extra 已经是最新版本
 community 已经是最新版本
:: 正在进行全面系统更新...
正在解析依赖关系...
正在查找软件包冲突...
错误：无法准备事务处理 (无法满足依赖关系)
:: 安装 libcap (2.49-1) 破坏依赖 'libcap=2.48' （lib32-libcap 需要）
:: 安装 libjpeg-turbo (2.1.0-1) 破坏依赖 'libjpeg-turbo=2.0.6' （lib32-libjpeg-turbo 需要）
```

用中文啥也搜不出来

```bash
# 将语言格式切换为 ascii 字符
$ export LANG=C

$ sudo pacman -Syu
:: Synchronizing package databases...
 core is up to date
 extra is up to date
 community is up to date
:: Starting full system upgrade...
resolving dependencies...
looking for conflicting packages...
error: failed to prepare transaction (could not satisfy dependencies)
:: installing libcap (2.49-1) breaks dependency 'libcap=2.48' required by lib32-libcap
:: installing libjpeg-turbo (2.1.0-1) breaks dependency 'libjpeg-turbo=2.0.6' required by lib32-libjpeg-turbo
```

在阅读 [[SOLVED] Pacman -Syu: installing libcap breaks dependency 2021-02-11](https://bbs.archlinux.org/viewtopic.php?id=263521)时，有一句话让我似乎想起了啥，想到我之前好像在整 wine 的时候开启过这个 Multilib 仓库，装完之后好像也没管过，一看发现被我给关上了。。
> If you have things installed from multilib, you can't just disable the repo and expect things to work.

```bash
$ sudo nano /etc/pacman.conf

# 去掉下面的内容前面的 ‘#’ 注释
[multilib]
Include = /etc/pacman.d/mirrorlist
```

这么一说的话，原来是 multilib 中的某些软件需要更新，然而大门却被我关上了。开启然后就可以正常更新了


# 参考

[Pacman arch wiki](https://wiki.archlinux.org/index.php/Pacman)

[System_maintenance arch wiki](https://wiki.archlinux.org/index.php/System_maintenance)
> If a partial upgrade scenario has been created, and binaries are broken because they cannot find the libraries they are linked against, do not "fix" the problem simply by symlinking. Libraries receive soname bumps when they are not backwards compatible. A simple pacman -Syu to a properly synced mirror will fix the problem as long as pacman is not broken.

[Official repositories arch wiki](https://wiki.archlinux.org/index.php/Official_repositories)

[ArchLinux近期更新依赖问题解决 StriverLite 2020-01-13 ](https://blog.csdn.net/qq_39828850/article/details/103963706#commentBox)：在[Pacman arch wiki](https://wiki.archlinux.org/index.php/Pacman)表示这个指令有破坏系统的能力，应该尽量避免使用。最后我也没有用这个办法
>sudo pacman -Rdd xxx xxx && sudo pacman -Syu

[ArchLinux 升级导致的wifi故障 csfreebird 2016-05-01 ](https://blog.csdn.net/csfreebird/article/details/51289844):降级版本（/var/cache/pacman/pkg/） + pacman的ignore配置

[[SOLVED] libcap (2.46) breaks dependency 'libcap=2.45' required by lib32-libcap 4 December 2020](https://forum.artixlinux.org/index.php/topic,2079.0.html)
> i added lib32-libpcap to lib32-testing (multilib-testing) now.

[[SOLVED] Pacman -Syu: installing libcap breaks dependency 2021-02-11](https://bbs.archlinux.org/viewtopic.php?id=263521)

[Arch Linux wine 微信、heidissql、chrome - AUR heplers - yay Kearney form An idea 2021-03-17](https://blog.csdn.net/weixin_43031092/article/details/113944349)

[6 个用于 Arch Linux 的最佳 AUR 助手 作者： Magesh Maruthamuthu 译者： LCTT hkurj | 2020-03-21 08:30](https://linux.cn/article-12019-1.html)

[ArchLinux安装使用教程](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/advanced/troubleshooting)
>升级系统时出现形如 unable to lock database 的错误
可能存在升级系统时异常关机或程序异常退出的情况，或者多个 pacman 的相关程序在同时执行。移除 pacman 的 db 锁即可  
`sudo rm /var/lib/pacman/db.lck`

教程中关于删除孤立软件包的内容过于激进，当你不知道这个包是干嘛用的，就不要随便删除。