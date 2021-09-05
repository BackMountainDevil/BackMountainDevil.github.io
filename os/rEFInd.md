---
title: "rEFInd - 一款优美的多系统引导软件"
date: 2021-06-03T12:04:58+08:00
lastmod: 2021-06-03T12:04:58+08:00
keywords: ['linux', 'rEFind', 'grub', 'legacy', 'UEFI']
description: "介绍 rEFInd（一款优美的多系统引导软件）的安装和使用方法"
tags: ['linux']
categories: ['app']
author: "筱氚"
---
# rEFInd

这一个用来自定义引导操作系统的软件。通常用在多操作系统的电脑中选择使用哪一个 OS。类似于 Linux 的开机选择操作系统的 grub，可以选择不同内核、安全模式、windows os、其它 linux 系统等。rEFInd 的优点在于美观、简洁、容易配置，虽然 grub 也可以设置，但设置错了的话就不太好弄了，rEFInd 配置错了还有 grub 和 BIOS 兜底。

# 安装

可以从 [sourceforge - refind](https://sourceforge.net/projects/refind/) 下载，然后安装即可。Arch 用户可以直接用 pacman 安装。

# 配置

`refind-install` 的时候会使用 efibootmgr 将 rEFInd 设置为默认的 UEFI 启动项。重启之后就会直接进入  rEFInd 的界面。

## efibootmgr
如果没有重启之后不是进入 rEFind 的界面，可能是 rEFInd 的启动顺序没有排在第一位，也有可能是压根没有把 rEFInd 加入启动顺序里。这两种情况都可以使用 efibootmgr 手动设置。

```bash
# 查看启动项
$ sudo efibootmgr -v

# 删除多余的启动项 - BootNumber，引导项对应的标号，通过上一步查看
$ sudo efibootmgr -b BootNumber  -B 

# 添加一个引导项，参数需要做对应的修改，参数详情看[这个](https://blog.csdn.net/wolaiyeptx/article/details/103447855)
$ efibootmgr -c -w -L “BootOptionName” -d /dev/sda -p 1 -l \EFI\refind\refind_x64.efi
```

## 参数设置

配置文件：`/boot/EFI/refind/refind.conf`
>    timeout 设置默认时间20s，时间到后进入默认操作系统。 0表示一直等待选择
    screensaver 设置在进入引导前的屏保时间，通常不启用(加#注释掉即可)
    hideui 隐藏界面功能选项，可以隐藏的选项有：
        banner eEFInd标识图 ->自己决定
        label 每个标签的文字描述以及timeout设置的倒数计时器 ->自己决定
        singleuser 苹果系统子菜单中单用户选项 ->不需要显示
        safemode 苹果系统子菜单中安全模式选项 ->不需要显示
        hwtest Mac硬件测试选项 ->不需要显示
        arrows 无法显示所有的引导菜单时的左右指示箭头 ->不需要显示
        hints 基本按键的简要说明 ->自己决定
        editor 选项编辑器 ->自己决定
        or all 隐藏所有选项
    icons_dir 指定自定义图标目录
    resolution 屏幕分辨率
    default_selection 默认进入的系统选项
    include 引导界面美化常用
    dont_scan_dirs, dont_scan_files, dont_scan_volumes 设置引导器过滤那些目录、文件、卷类型（分区）


这是一个过滤目录的例子
```bash
dont_scan_dirs ESP:/EFI/Boot,/EFI/boot,EFI/ubuntu,EFI/UOS,EFI/tools
```

## 主题
如果不喜欢默认主题可以到 github上 的相关话题 [refind-theme](https://github.com/topics/refind-theme) 就有29个匹配的仓库，当时直接搜索的话更多，挑一个喜欢的然后看 README.md 进行安装。

如果只是对图标不满意，去别的主题里单独找图标存到当前主题的 icons 文件夹里覆盖即可。

# 总结
如果你实在闲着无聊，那就整一个吧，为了获得更好的体验，可以尝试把 os 自带的引导界面隐藏起来，这样就会只有 rEFInd 的引导界面了，没必两次引导选择。

## 隐藏 grub 引导
这里是修改菜单的超时时间为 0，在 Arch 上直接设置为 0，但是在 Ubuntu 下直接设置 0 会被重置为 10（具体见参考四）。
```bash
sudo nano /etc/default/grub
# 将GRUB_TIMEOUT修改为0，-1是无限期等待（和refind有点不同哈），
# 开启了启动延时是5，关闭了是1，单位是秒，


#GRUB_TIMEOUT_STYLE=hidden
GRUB_TIMEOUT=0



# 更新 grub，以下方式选择一个更新即可
sudo update-grub  ## deepin 或者 ubuntu
sudo grub-mkconfig -o /boot/grub/grub.cfg   ## arch 或者 ubuntu
```

### ubuntu
为了取消引导界面，ubuntu 中 timeout 设置为 0 和会被重置为 10，亲测 GRUB_RECORDFAIL_TIMEOUT=0 无效，编辑 /boot/grub/grub.cfg 中的重置语句也是无效的，因为这个文件在更新 grub 会被自动重写。下面是亲测之后得出的一个可行的办法，修改 `/etc/grub.d/30_os-prober` 中的重置部分。

```bash
sudo nano /etc/grub.d/30_os-prober
# set timeout=10 修改为 set timeout=0

sudo nano /etc/default/grub

# GRUB_TIMEOUT=10 修改为 GRUB_TIMEOUT=0

sudo update-grub
```

# 参考
- [[Succeed]rEFind安装之在Deepin上的一番折腾~怀疑联想~Could not prepare Boot variable: No space left on device Kearney form An idea 2021-02-19](https://blog.csdn.net/weixin_43031092/article/details/113855135)：rEFInd install & beautify example. **efibootmgr manage BootOrder** - add fail and solution.
- [rEFind配置忽略项以及主题～win/deepin/arch -- update-grub：未找到命令 Kearney form An idea 2021-02-21](https://blog.csdn.net/weixin_43031092/article/details/113870562):rEFInd theme + 取消 deepin、arch 的开机引导界面
- [REFInd wiki.archlinux](https://wiki.archlinux.org/title/REFInd)
- [ubuntu 修改开机等待 grub时间. Cookie_Cheng 2020-07-30](https://blog.csdn.net/qq_20538071/article/details/107688351):看了那个文件 line 36，确实是 0 的时候会被重置为 10，怪不得我设 0 没有生效。但是修改 /etc/grub.d/30_os-prober 可不是个好主意。还有就是本来就没有 GRUB_HIDDEN_TIMEOUT 
  > 如果这个时间是0，在统一设置中，还会被 /etc/grub.d/30_os-prober 调整。修改等待时间，将GRUB_TIMEOUT改成你想要的时间，单位是秒。一定要注意注释掉GRUB_HIDDEN_TIMEOUT，不然修改不起作用
- [ubuntu中修改grub的启动时间.刀口 2019-10-31](https://blog.csdn.net/nicechao/article/details/102835680):实测无效。又发现在 /boot/grub/grub.cfg line 323 中timeout 如果是 0 就会被重置为 10
  > /etc/grub.d/00_header 可以看到这超时30秒是通过GRUB_RECORDFAIL_TIMEOUT这个值设置的.
所以只需要在 /etc/default/grub 加上或修改 GRUB_RECORDFAIL_TIMEOUT=0
