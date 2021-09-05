---
title: "将 Arch Linux 内核从 stable 更换为 LTS 的办法与案例"
date: 2021-07-01T20:28:24+08:00
lastmod: 2021-07-01T20:28:24+08:00
keywords: ['arch', 'linux', 'kernel']
description: "将 Arch Linux 内核从 stable 更换为 LTS。"
tags: ['arch']
categories: ['os']
author: "筱氚"
---
# 干啥

将 Arch Linux 内核从 stable 更换为 LTS。以此降低内核更新的频率。

## Q&A
1. 安装内核只需要安装内核的包吗？对应的头文件包要不要装？比如 `linux-lts` 与 `linux-lts-headers`。

    在中文帖子里是两个都装，在英文帖却是只装前者。也就是说不装新内核对应的头文件包也是可以的，非必须安装。

2. 假设装内核和其对应的头文件包，删除旧内核的时候要不要删除其头文件包？

    在 [Arch Linux 更换到稳定版LTS内核。Adin](https://poemdear.com/2019/03/27/arch-linux-%e6%9b%b4%e6%8d%a2%e5%88%b0%e7%a8%b3%e5%ae%9a%e7%89%88lts%e5%86%85%e6%a0%b8/)一文中两个都安装了，但是删除旧内核的时候并没有删除旧内核的头文件包，只删除旧内核包！！不明所以。

    在英文帖子里由于只安装了新内核并没有安装新内核对应的头文件包，因此也就没有卸载旧内核的头文件包。之前整deepin内核的时候用的是 purge 自动级联删除功能，也就是需要删除头文件包。

    由于自己无法检测当前内核使用的是哪一个头文件包，因此该问题有待继续探究。


# 怎么做
中文版、英文版资料都看了几个，发现中文版的会大多装一个`linux-lts-headers`包。这里我选择了也安装一个，但这并不是必须的，也可以不装。其次是大多数帖子是装完新内核就立马删除旧内核更新grub，这其实是不合理的，因为在没有切换到新内核之前，还不知道新内核能不能用，有没有存在不兼容问题等。因此这里我会先切换到新内核使用一阵子再对旧内核进行清理。

下面是从最新的稳定版 `5.12.13-arch1-2` 切换到最新的 LTS 版 `5.10.47-1-lts` 的案例

## 下载 LTS 内核

```bash
$ uname -a
Linux arch 5.12.13-arch1-2 #1 SMP PREEMPT Fri, 25 Jun 2021 22:56:51 +0000 x86_64 GNU/Linux
# 安装最新的 LTS 内核 及其头文件
$ sudo pacman -S linux-lts linux-lts-headers
软件包 (2) linux-lts-5.10.47-1  linux-lts-headers-5.10.47-1

$ uname -a
Linux arch 5.12.13-arch1-2 #1 SMP PREEMPT Fri, 25 Jun 2021 22:56:51 +0000 x86_64 GNU/Linux
```

## 更新 grub

展开子菜单参考了这篇[文章](https://itsfoss.com/switch-kernels-arch-linux/)的思路。这里我选择展开，不展开直接更新 grub 配置也是可以的。

```bash
# 修改 grub 配置不是必须的，下面那个才是必须的。
$ sudo nano /etc/default/grub
# 这三行是将子菜单展开，这样不用点击 advanced 进去了
GRUB_DISABLE_SUBMENU=y
GRUB_DEFAULT=saved
GRUB_SAVEDEFAULT=true
# 等待时间我之前设置为 0s 的，0 就不显示了，这里改大一点 3s
GRUB_TIMEOUT=3

# 改完保存退出 -----------------------------------------


# 更新下 grub 的配置文件，让上面的设置生效，这一步是一定要做的。
$ sudo grub-mkconfig -o /boot/grub/grub.cfg
正在生成 grub 配置文件 ...
找到 Linux 镜像：/boot/vmlinuz-linux-lts
找到 initrd 镜像：/boot/amd-ucode.img /boot/initramfs-linux-lts.img
Found fallback initrd image(s) in /boot:  amd-ucode.img initramfs-linux-lts-fallback.img
找到 Linux 镜像：/boot/vmlinuz-linux
找到 initrd 镜像：/boot/amd-ucode.img /boot/initramfs-linux.img
Found fallback initrd image(s) in /boot:  amd-ucode.img initramfs-linux-fallback.img
警告： os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
完成
```

## 重启界面选择 LTS
从 grub 中选择 linux-lts 即可，我的第一个就是 lts 了（grub默认启动的也是第一个）。可以看出内核已经切换到 LTS 版本。

```bash
$ uname -a
Linux arch 5.10.47-1-lts #1 SMP Wed, 30 Jun 2021 13:52:19 +0000 x86_64 GNU/Linux
```

tip：查看 grub 启动菜单的顺序可以用下面的命令，这样可以设置 grub 默认启动哪一个
>awk -F\' '$1=="menuentry " {print i++ " : " $2}' /boot/grub/grub.cfg

## 卸载不需要的内核
这一步可以稍微延后一点，过一阵子再整。万一发现 LTS 哪里不兼容的时候，这个内核还能继续用。

```bash
$ sudo pacman -R linux linux-headers

# 更新 Grub，这样子之后引导界面只会看到 lts 内核选项了
$ sudo grub-mkconfig -o /boot/grub/grub.cfg
```


# 参考
其它中英文帖子没贴出来因为一是觉得参考意义不大，二是觉得写的质量不行。明天考试，今天写个笔记压压惊。
- [Kernel Arch wiki](https://wiki.archlinux.org/title/Kernel)：四种官方支持内核
- [Arch Linux 更换到稳定版LTS内核 March 27, 2019 by Adin](https://poemdear.com/2019/03/27/arch-linux-%e6%9b%b4%e6%8d%a2%e5%88%b0%e7%a8%b3%e5%ae%9a%e7%89%88lts%e5%86%85%e6%a0%b8/):cv error;`-Rsdd` 会跳过所有依赖检查，看上去过于激进了哈，查了一下 linux、linux-lts 这些包确实都有不少依赖 -.- 
- [How to switch arch linux to lts kernel? 2015](https://unix.stackexchange.com/questions/284617/how-to-switch-arch-linux-to-lts-kernel): two good answers
- [Different Types of Kernel for Arch Linux and How to Use Them. October 19, 2020 By Dimitrios ](https://itsfoss.com/switch-kernels-arch-linux/)
- [【解决】深度操作系统Linux Deepin 20 内核5.10手动降级-附上大佬解决5.10中失去蓝牙的办法。Kearney form An idea 2021-02-06 22:08:03](https://blog.csdn.net/weixin_43031092/article/details/113729957?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162514633016780261949519%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=162514633016780261949519&biz_id=0)：之前整 deepin linux 的内核更换记录