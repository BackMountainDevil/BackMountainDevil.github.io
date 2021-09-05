---
title: "完全卸载 Ubuntu 操作系统的办法"
date: 2020-11-15T02:57:35+08:00
lastmod: 2020-11-15T02:57:35+08:00
keywords: ['Uninstall', 'Ubuntu']
description: "记录用在windows 10 OS下完全卸载Ubuntu OS的过程，win+ubuntu双系统"
tags: ['Ubuntu']
categories: [OS]
author: "筱氚"
---
# Intro
记录用在windows 10 或者 arch 下完全卸载 Ubuntu 的过程，win + ubuntu + arch 多系统。

大体思路是删除系统对于的硬盘分区，然后删除启动引导中的引导文件，然后更新引导菜单。
# How
## disk
打开磁盘管理，找到ubuntu所安装的磁盘，右键删除卷。我之前安装Ubuntu的时候留了130G的空间给Ubuntu，所以很好找到那个是Ubuntu所在位置。

此步骤也可以在 Arch 打开KDE自带的 KDE 分区管理器（KDE Partition Manager），可以清晰的看到磁盘的分区状况，选择 Ubuntu 所在的磁盘分区，鼠标右键该分区选择删除（Delete），检查一下所删除分区是 Ubuntu 所在分区，不是 win 的ntfs也不是fat32启动分区，确认无误之后点击左上角的Apply（应用）即可删除该分区。然后该分区会变为未分配分区。

## uefi
win+R 输入diskpart回车打开。
```bash
# 列出所有硬盘
list disk
# 选择ubuntu所在的硬盘，我这里只有一块硬盘，需要更改最后的数字
select disk 0

# 显示分区
list partition
# 选中windows efi分区，根据磁盘管理的大小对比就知道了，需要需要更改最后的数字
select partition 1

# 暂时分配盘符J，J可以变，不和电脑里的其它盘符冲突即可
assign letter=J
# 现在我的电脑里有了一个J盘，当然权限不够打不开
# win然后搜索记事本，右键管理员身份打开
# 记事本菜单栏-文件-打开，选择J盘/EFI，右键删除ubuntu即可

# 取消J盘挂载
remove letter=J
```
到此就完全卸载了ubuntu，可以关闭磁盘管理、diskpart、记事本啦。如果你想把删除的卷归入win中，可以右键刚才变成黑色的卷的左边的卷（盘），选择扩展卷，或者留着一会折腾其它OS。

在 linux 下由于会直接挂载 EFI 分区，因此可以在 Arch 中直接删除其对于 efi 文件
```bash
sudo rm -fr /boot/EFI/ubuntu
```

## boot menu
此时开机并不会显示系统选择菜单了，但是打开BIOS仔细看的boot menu话还是会有ubuntu选项在！！！目前我把它的顺序下调到了最末尾

在 Linux 中使用 efibootmgr 来进行启动项管理。
```bash
sudo efibootmgr -v
# 找到 ubuntu 对于的 BootOrder,如 Boot0000*
sudo efibootmgr -b 0000 -B 
```

# References
- [从零开始不愉快玩耍Ubuntu 20.04 LTS到卸载. Kearney form An idea 2020-11-11](https://blog.csdn.net/weixin_43031092/article/details/109628977)
- [在win10+Ubuntu双系统下，完美卸载Ubuntu](https://blog.csdn.net/guikunchen/article/details/88077330)
- [如何卸载一个操作系统-以卸载Linux Deepin为例. Kearney form An idea 2021-03-01](https://blog.csdn.net/weixin_43031092/article/details/114263189)