# HackBGRT 更改开机logo以及如何修复
- date: 2022-07-26
- lastmod: 2022-08-03

# 摘要

想要更改联想小新13 pro ARE 2020的开机logo，联想电脑管家有这个功能，但是一旦卸载了管家这个功能就失效了（ex）。后来看到HackBGRT，问了下支持linux的，实操一下不太成功，开启debug修正了一下稍微成功了一次就失败了，windows还得修复，还好manjaro还可以正常启动。

如果是Lenovo电脑可以尝试参考末尾的LogoDiy，问了下原理是通过dnSpy逆向，测试简单有效

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

从逻辑上来说，win启动项是没有问题的，它只是按照我设置的进行操作罢了，一进入win就会进入hackBGRT的引导，而这个引导的目标又被我改成manjaro,所以进入win就会跳到manjaro,从manjaro的grub2界面进入win也是会跳转到manjaro，死循环了属实是，那么问题就是如何把win的启动项修改回win，查了下大多是bcdedit、bcdedit之类的，当然最简单粗暴的办法是重装，在官网上免费下载到win11的系统安装包和刚开始安装时一样就行了，但是这样会覆盖安装C盘的软件，虽然没啥，不过CSGO在D盘，在不得不重装之前尝试下其它办法，在参考[Windows 11 UEFI引导修复详细教程](https://www.disktool.cn/content-center/repair-windows-11-uefi-bootloader-2111.html)的方法一和方法二通过diskpart和bcdboot进行尝试起初不太成功，最后也是通过这个办法修复的

### 启动修复 失败

通过系统安装包的修复计算机中自带的启动修复功能，检测修复后还是启动manjaro,多次尝试该办法均以失败告终。

### bcdboot 成功

此办法起初失败有几个原因，EFI分区默认不分配驱动器号导致不知道是哪个盘符（要使用assign先分配）、拒绝访问和复制失败（路径有误）

```bash
> diskpart
DISKPART> list disk
DISKPART> select disk 0
DISKPART> list vol  # 此时显示EFI分区大小260M，卷号为3,标签是SYSTEM_DRV,格式为FAT32,没有分配驱动器号
DISKPART> select volume 3 # 根据上一步的结果判断EFI分区的卷号
DISKPART> assign letter=z # 分配驱动器号为 z
DISKPART> exit  # 推出 diskpart

> cd/d Z:\EFI\Microsoft\Boot\ 

> bootrec/fixboot
拒绝访问
> ren BCD BCD.bak
> bcdboot C:\Windows /1 zh-cn /s x: /f ALL (请根据自己的设置更改内容)
尝试复制启动文件失败
> bootrec /rebuildbcd
在所有磁盘上扫描 Windows 安装。。。
已标识的 Windows 安装总数:1
[1] E:\Windows
是否要将安装添加到启动列表？是Y否N全部A:A
```

添加之后启动的还是manjaro，刚开始以为这个办法只是把原启动项复制加入BIOS,后来添加成功才知道这个是用唯一的win引导覆盖旧的win引导。上面拒绝访问那里查了下有的解决方案要进入系统里打开服务项注册表进行操作的然而我都进不去系统好吧，然后想着要不运行安装时的exe进行卸载好了，又得知切换目录cd显示文件列表dir运行exe是start,然而运行exe时显示错误提示为并行配置有误，既然不能运行安装的exe进行卸载，那能不能直接删除efi呢，通过dir发现efi分区和linux下的efi是一样的东西，把东西删除还要修改启动项的，然后切换目录后dir发现该C盘并非是我安装win的系统盘，而是我的U盘，通过遍历字母表发现装有系统的C盘被分配到了E盘（写记录的时候才发现上面扫描出来的结果也提示了这一点），然后一句指令就搞定了 `bcdboot E:\Windows`，之后正常进入了win,连manjaro都没有显示，正当我以为要给manjaro重做grub引导时，进入BIOS的启动项设置里看到manjaro还在，把ta的顺序上升到win之前就搞定了。

# 参考
- [超简单方法修改笔记本开机logo，三分钟搞定！iamjpw 2020-05-16](https://post.smzdm.com/p/apz3vw07/):HackBGRT 
  > 安装完重启发现开机先显示默认logo，再显示自定义logo：这个可能属于安装不完全
- [BIOS LOGO 太单调怎么办？不要急，看完这篇文章，你就会改了 热心市民描边怪  2019-05-28](https://post.smzdm.com/p/a25rpw52/): 用 UEFI Tool 修改BIOS rom文件，再安装BIOS
  > CTRL+F 搜索GUID 7BB28B99-61BB-11D5-9A5D-0090273FC14D.没有搜索到，那么这个方法就不能用
- [Step to UEFI (137） 通过 BGRT 取得当前系统的 LOGO ziv2013发布于 2018 年 1 月 15 日](https://www.lab-z.com/stu137/):以防万一，把这个代码备份到[gitee](https://gitee.com/anidea/find-bgrt)了
  > BIOS解压自己的Logo在内存中，然后通过ACPI Table将这个Logo传递给Windows.具体的Table就是 Boot Graphics Resource Table。在 ACPI 6.1的5.2.22有专门的介绍。
- [ BIOS Boot-Logo - Howto: create your own BIOS boot logo](https://www.biosflash.com/e/bios-boot-logo.htm):CBROM v2.15 暂未尝试
- [Customize Lenovo PC boot logo (screen) without installing "Lenovo PC Manager".](https://github.com/Coxxs/LogoDiy/):测试有效