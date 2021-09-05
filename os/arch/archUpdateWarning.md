---
title: "Arch 升级中的警告是什么：Possibly missing firmware for module: wd719x、aic94xx、xhci_pci"
date: 2021-07-25T15:09:44+08:00
lastmod: 2021-07-25T15:09:44+08:00
keywords: ['linux', 'arch', 'update', 'warning']
description: "Arch 升级中的警告的原因探究以及解决办法"
tags: ['arch']
categories: [os]
author: "筱氚"
---
# 问题
今天顺手升级，仔细一看发现了三个 waning（平时懒得看的），不过更新顺利还是完成了。最后查了一波是正常 Warning，我也没有这几个硬件，因此不需要处理这几个 WARNING.

```bash
$ sudo pacman -Syu
==> WARNING: Possibly missing firmware for module: wd719x
==> WARNING: Possibly missing firmware for module: aic94xx
==> WARNING: Possibly missing firmware for module: xhci_pci
```

# 探究过程

直接 Bing 搜索了警告信息，在 [Possibly Missing Firmware Module wd719x. atczaja. 2015-03-19](https://bbs.archlinux.org/viewtopic.php?id=194977)中还是找到了一些有用信息：
  > Welcome to the forums.  Please check out the wiki - you'll often find the answers you need there:https://wiki.archlinux.org/title/Mkinitcpio#Possibly_missing_firmware_for_module_XXXX  

  tomk: Do `modinfo wd719x`to find out what that module is for  
  tomk: `lspci` to list such hardwares

顺藤摸瓜的进入 [Mkinitcpio#Possibly_missing_firmware_for_module_XXXX. Arch wiki](https://wiki.archlinux.org/title/Mkinitcpio#Possibly_missing_firmware_for_module_XXXX)：
  >Currently, the only solution for suppressing warnings for wd719x and aic94xx modules is actually installing firmware packages for them. For aic94xx, install aic94xx-firmwareAUR. For wd719x, install wd719x-firmwareAUR. For xhci_pci, install upd72020x-fwAUR  
  中文：对于所有的Linux用户来说，尤其是那些没有安装这些固件模块的用户。如果您不使用使用这些固件的硬件，您可以安全地忽略此消息。目前的办法是给 ta 装上对应的软件。

那么就需要知道查一下我有没有这些固件模块了。根据上面 tomk 提供的思路

```bash
$ modinfo wd719x
description:    Western Digital WD7193/7197/7296 SCSI driver
# 西部数据 WD7193/7197/7296 的驱动

$ modinfo aic94xx
description:    Adaptec aic94xx SAS/SATA driver

$ modinfo xhci_pci
description:    xHCI PCI Host Controller Driver
```

然后搜索 WD7193 发现了 [Arch Linux 更新出现模块固件缺失的警告. Ozaaa. 2020-12-31](https://zhuanlan.zhihu.com/p/340918736)
  > wd719x-firmware 估摸着是一个西部数据的 SCSI 适配器的驱动。参考该讨论，我的电脑上应该不存在此硬件。  
  aic94xx-firmware 是一个企业级硬件 RAID 控制器卡的驱动。可参考该讨论。  
  xhci_pci 貌似是和 USB 3 有关，如果 USB 3 的接口都使用正常，可以忽视这个问题。该模块缺失的解决方法，最终也是在一个 bbs 中找到的。解决办法在这条回复中。

通过 HardInfo 查看 PCI 设备得知我的西数 SN7x0 用 nvme 驱动，而且用了半年多啥问题没的，企业级 15k+rpm 硬盘驱动和我没关系，我机械硬盘是家庭级别的 7.2k rpm。唯一一个 USB 口用的没毛病。

# 结论
没有这些硬件以及硬件能正常使用的可以直接忽略这三个警告，啥事没有。强迫症解决办法看参考四

# Refer
- [ Arch Linux 更新系统时内核警告问题. 柒 .February 18, 2020 ](https://www.vvave.net/archives/fix-the-waring-issues-when-the-archlinux-upgrade-kernel.html)：aic94xx 的安装过程，但是没有说明原因
- [Possibly Missing Firmware Module wd719x. atczaja. 2015-03-19](https://bbs.archlinux.org/viewtopic.php?id=194977)
- [Mkinitcpio#Possibly_missing_firmware_for_module_XXXX. Arch wiki](https://wiki.archlinux.org/title/Mkinitcpio#Possibly_missing_firmware_for_module_XXXX)
- [Arch Linux 更新出现模块固件缺失的警告. Ozaaa. 2020-12-31](https://zhuanlan.zhihu.com/p/340918736)
