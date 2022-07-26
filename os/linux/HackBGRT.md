# HackBGRT 更改开机logo
- date: 2022-07-26
- lastmod: 2022-07-26

# 摘要

想要更改联想小新13 pro ARE 2020的开机logo，联想电脑管家有这个功能，但是一旦卸载了管家这个功能就失效了（ex）。后来看到HackBGRT，问了下支持linux的，实操一下不太成功，开启debug修正了一下稍微成功了一次就失败了，windows还得修复，还好manjaro还可以正常启动。

- Device: Lenovo XiaoXinPro-13ARE 2020
- BIOS: F0CN36WW
- OS: 5.15.55-1-MANJARO + Windows 11
- HackBGRT: v1.5.1

## 原理

[HackBGRT](https://github.com/Metabolix/HackBGRT)的summary中说到：“在基于 UEFI 的计算机上启动时，Windows 可能会显示供应商定义的徽标，该徽标存储在 UEFI 固件中称为启动图形资源表 (BGRT) 的部分中。永久更改映像通常非常困难，但可以使用自定义 UEFI 应用程序在引导期间覆盖它。”

我的理解是既然是使用自定义UEFI固件程序覆盖，那可以把这个程序加入到linux的grub里边唉

# 实操
## 总结

从Release下载最新版的压缩包zip，解压，修改配置文件(config.txt)，将boot的值修改为`\EFI\Manjaro\grubx64.efi`（因系统而异），image按照文档可以修改为black（我喜欢极简一些）。重启就可以发现manjaro启动后的logo不再是某头了。但是开机画面的logo依旧

```bash
nano /tmp/HackBGRT-1.5.1/config.txt
sudo cp -r /tmp/HackBGRT-1.5.1/ /boot/efi/EFI/HackBGRT/ # 复制到efi
sudo efibootmgr -c -w -L "HackBGRT" -d /dev/nvme0n1 -p 1 -l \EFI\EFI\HackBGRT\bootx64.efi # 添加启动项，不可直接复制！！！参考efibootmgr使用文档修改路径
sudo update-grub
```

## 过程记录

从[Release](https://github.com/Metabolix/HackBGRT/releases)下载v1.5.1的压缩包zip（内含exe、efi。source code需要自行编译，从issue看出不太好编译），解压，修改配置文件(config.txt)，将boot的值修改为`\boot\efi\EFI\Manjaro\grubx64.efi`（因系统而异），图片先保持默认配图试试水

```bash
$ sudo cp -r /tmp/HackBGRT-1.5.1/ /boot/efi/EFI/HackBGRT/ # 复制到efi
$ sudo efibootmgr # 查看启动项

$ sudo efibootmgr -c -w -L "HackBGRT" -d /dev/nvme0n1 -p 1 -l \EFI\EFI\HackBGRT\bootx64.efi # 添加启动项，参考efibootmgr使用文档，不可直接复制
Boot0000* Windows Boot Manager  HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\Microsoft\Boot\bootmgfw.efi)
Boot0001* manjaro       HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\manjaro\grubx64.efi)Boot0002* HackBGRT      HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(EFIEFIHackBGRTbootx64.efi)

$ sudo update-grub # 更新 grub
正在生成 grub 配置文件 ...
找到主题：/usr/share/grub/themes/manjaro/theme.txt
找到 Linux 镜像：/boot/vmlinuz-5.15-x86_64
找到 initrd 镜像：/boot/amd-ucode.img /boot/initramfs-5.15-x86_64.img
Found initrd fallback image: /boot/initramfs-5.15-x86_64-fallback.img
找到 Linux 镜像：/boot/vmlinuz-5.10-x86_64
找到 initrd 镜像：/boot/amd-ucode.img /boot/initramfs-5.10-x86_64.img
Found initrd fallback image: /boot/initramfs-5.10-x86_64-fallback.img
警告： os-prober 将运行以探测其它可引导分区。
其输出将用来探测分区中可用于引导的二进制文件并为其创建新的引导项。
找到 Windows Boot Manager 位于 /dev/nvme0n1p1@/EFI/Microsoft/Boot/bootmgfw.efi
正在添加 UEFI 固件设置的引导菜单项……
Found memtest86+ image: /boot/memtest86+/memtest.bin
/usr/bin/grub-probe：警告： 未知的设备类型 nvme0n1.
```

重启正常进入manjaro,但是exlogo还在，从启动项列表中发现manjaro和win后边的efi文件路径都是正常的，但是HackBGRT的efi文件名都不对，怀疑路径问题，于是修正一下路径

```bash
$ sudo efibootmgr -b 0002 -B	# del HackBGRT!!! 注意序号，容易误删！！！
$ sudo efibootmgr -c -w -L "HackBGRT" -d /dev/nvme0n1 -p 1 -l "/boot/efi/EFI/HackBGRT/bootx64.efi"
$ sudo update-grub
正在生成 grub 配置文件 ...
找到主题：/usr/share/grub/themes/manjaro/theme.txt
找到 Linux 镜像：/boot/vmlinuz-5.15-x86_64
找到 initrd 镜像：/boot/amd-ucode.img /boot/initramfs-5.15-x86_64.img
Found initrd fallback image: /boot/initramfs-5.15-x86_64-fallback.img
找到 Linux 镜像：/boot/vmlinuz-5.10-x86_64
找到 initrd 镜像：/boot/amd-ucode.img /boot/initramfs-5.10-x86_64.img
Found initrd fallback image: /boot/initramfs-5.10-x86_64-fallback.img
警告： os-prober 将运行以探测其它可引导分区。
其输出将用来探测分区中可用于引导的二进制文件并为其创建新的引导项。
找到 Windows Boot Manager 位于 /dev/nvme0n1p1@/EFI/Microsoft/Boot/bootmgfw.efi
正在添加 UEFI 固件设置的引导菜单项……
Found memtest86+ image: /boot/memtest86+/memtest.bin
/usr/bin/grub-probe：警告： 未知的设备类型 nvme0n1.
完成
$ sudo efibootmgr
Boot0000* Windows Boot Manager  HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\Microsoft\Boot\bootmgfw.efi)
Boot0001* manjaro       HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\manjaro\grubx64.efi)
Boot0002* HackBGRT      HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\boot\efi\EFI\HackBGRT\bootx64.efi)
```

然而还是那个logo，看了下debug输出怀疑是配置文件的路径配置错了，将boot的值修改为`\EFI\Manjaro\grubx64.efi`，将image的值修改为`n=1,path=\EFI\HackBGRT\splash.bmp`（照抄文件提示）。启动之后有那么一次显示了HackBGRT的图像，之后就还是exlogo,启动manjrao倒是一切正常，把HackBGRT启动项删除，文件夹删除，然而win还是进不去，至于如何修复win11的boot,想着用winPE，但是手头上没有，也很少用win，不着急。洗完澡想着如果把 hackBGRT正确安装的话，win是不是还可以正常启动呢？试了一下，添加启动项的方式这次也把路径修改了（`sudo efibootmgr -c -w -L "HackBGRT" -d /dev/nvme0n1 -p 1 -l "/EFI/HackBGRT/bootx64.efi"`），manjrao启动的时候确实把exlogo替换掉了，但是win还是不能启动，这下子连提示也没有了。

# 参考
- [超简单方法修改笔记本开机logo，三分钟搞定！iamjpw 2020-05-16](https://post.smzdm.com/p/apz3vw07/):HackBGRT 
  > 安装完重启发现开机先显示默认logo，再显示自定义logo：这个可能属于安装不完全
- [BIOS LOGO 太单调怎么办？不要急，看完这篇文章，你就会改了 热心市民描边怪  2019-05-28](https://post.smzdm.com/p/a25rpw52/): 用 UEFI Tool 修改BIOS rom文件，再安装BIOS
  > CTRL+F 搜索GUID 7BB28B99-61BB-11D5-9A5D-0090273FC14D.没有搜索到，那么这个方法就不能用
- [Step to UEFI (137） 通过 BGRT 取得当前系统的 LOGO ziv2013发布于 2018 年 1 月 15 日](https://www.lab-z.com/stu137/):以防万一，把这个代码备份到[gitee](https://gitee.com/anidea/find-bgrt)了
  > BIOS解压自己的Logo在内存中，然后通过ACPI Table将这个Logo传递给Windows.具体的Table就是 Boot Graphics Resource Table。在 ACPI 6.1的5.2.22有专门的介绍。