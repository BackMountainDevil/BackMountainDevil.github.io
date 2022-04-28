# windwos11 覆盖了 manjaro linux 的启动引导 grub
- date: 2022-04-28
- lastmod: 2022-04-28

# 问题为什么发生

以往整 linux 我都是双系统或者虚拟机，但是后来觉得可以不再使 windows 了，就完全把 windows 删除了。有啥跑不起来的软件整 wine 或者 虚拟机，但是直到更新 BIOS，wine 或者 虚拟机都没有办法，想着旧版 BIOS 也行，但是直到最近联想爆出 BIOS 存在重大隐患紧急更新了一批 BIOS，到驱动页面真的发现了新版的 BIOS，没想到啥好办法，只好装个 windows。

双系统中一般都推荐先装 windows 再装 linux，因为顺序反过来的话 windows 会覆盖掉 linux 的启动引导，就很恶心，而 linux 不会覆盖其它系统的启动引导，检测到其它系统会将其添加到启动选项里边。

# 怎么办

在 [manjaro forum](https://forum.manjaro.org/)搜索了一波没找到很满意的答案，bing 搜索出来的答案也不是十全十美，wiki 里搜索了 grub，找到这两 - [GRUB](https://wiki.archlinux.org/title/GRUB) [GRUB/Restore the GRUB Bootloader](https://wiki.manjaro.org/index.php/GRUB/Restore_the_GRUB_Bootloader/en)。总的来说，先用安装盘进入 liveSystem，然后修复引导。

吐槽一下，windows 的安装盘没有 live，反倒是 linux 的大都有 live 可以先体验后安装。在官网上下载了 manjaro-kde-21.2.6-minimal-220416-linux510.iso 发现无论用啥驱动都没法进去 live，这才想起第一次安装 manjaro 也是这样，就下了 manjaro-kde-21.2.6-220416-linux515.iso，这个时候进去 live 易如反掌，也不知道是啥原因。

## 工具

- U盘：存放系统安装文件
- Ventory：给 U 盘加上装系统的功能
- 系统镜像：到官网下，manjaro-cd 现在不提供同步了

# 从 grub 进入 linux 系统

这个以前自己遇到过：[解决Deepin启动时无法进入系统出现grub>的问题](/os/deepin/DeepinGrub.md)

用 manjaro-kde-21.2.6-minimal-220416-linux510.iso 是没法进入 live，但是可以按 C 进入 grub 命令行模式。先用 ls 查看所有盘及其分区，再逐个检验哪一个是 linux 所在的主分区，然后设置引导分区位置，再用 normal 启动。用这个办法我确实启动了原有的 manjro。

![manjaro安装引导界面](https://img-blog.csdnimg.cn/95d8c0319a314b0f9ea4aa996816828d.png#pic_center)

![Detect efi 的部门结果](https://img-blog.csdnimg.cn/ada34845eb1d4843aa500700d97efddf.png#pic_center)Detect efi bootloaders 的结果我都尝试了一下，发现还是进不去，后面 grub 的尝试也说明了为什么会失败，因为不是 gpt1 而是 gpt4.下面按下 C 进入 grub 界面
![boot with driver1的启动错误](https://img-blog.csdnimg.cn/7a115749b42a42f287e9c131ab76c84f.png#pic_center)

![boot with driver2的启动错误](https://img-blog.csdnimg.cn/c08c43af01774a3e9ebdd6364698caea.png#pic_center)
没法进入 live 只好尝试一下 grub 了

![从grub启动manjaro](https://img-blog.csdnimg.cn/26473518a1c94bbdb1583dcc980e19a4.png#pic_center)

进去之后就直接重新生成引导（`sudo grub-mkconfig -o /boot/grub/grub.cfg`），然后想起以前用过的 `sudo efibootmgr -v` 查看了一下没有 manjaro，也不好到底有没有百分之百的把握完全成功，重启之后发现进入的还是 win11,而且这一招从 grub 进入 manjaro 不能重现了，敲 normal 启动之后就一直黑屏，尝试了几次还是黑屏。（长摁电源键可强制关机）

接下来尝试过 [BCDEdit 命令行选项](https://docs.microsoft.com/zh-cn/windows-hardware/manufacture/desktop/bcdedit-command-line-options?view=windows-11)、[bcdedit](https://docs.microsoft.com/zh-cn/windows-server/administration/windows-commands/bcdedit)，但是没查到怎么用这玩意添加 manjaro 启动项。遂只好下 manjaro-kde-21.2.6-220416-linux515.iso 去尝试进入 live了。


# livsOS 修复 grub
## 失败的修复

```bash
$ sudo nano /etc/default/grub # 编辑配置文件，非必须（我之前把 menu 设置成 hidden 了，现在多系统要改回去）

$ sudo grub-mkconfig -o /boot/grub/grub.cfg # 重新生成引导，奇怪的是下面没有发现 win11
Generating grub configuration file ...
Found theme: /usr/share/grub/themes/manjaro/theme.txt
Found linux image: /boot/vmlinuz-5.15-x86_64
Found initrd image: /boot/amd-ucode.img /boot/initramfs-5.15-x86_64.img
Found initrd fallback image: /boot/initramfs-5.15-x86_64-fallback.img
Found linux image: /boot/vmlinuz-5.10-x86_64
Found initrd image: /boot/amd-ucode.img /boot/initramfs-5.10-x86_64.img
Found initrd fallback image: /boot/initramfs-5.10-x86_64-fallback.img
Warning: os-prober will be executed to detect other bootable partitions.
Its output will be used to detect bootable binaries on them and create new boot entries.
Found Windows Boot Manager on /dev/nvme0n1p1@/EFI/Microsoft/Boot/bootmgfw.efi
Adding boot menu entry for UEFI Firmware Settings ...
Found memtest86+ image: /boot/memtest86+/memtest.bin
/usr/bin/grub-probe: warning: unknown device type nvme0n1.
done

$ sudo efibootmgr -v
BootCurrent: 0017
Timeout: 0 seconds
BootOrder: 0000,0013,0014,0015,0016,0017,0018,0019
Boot0000* Windows Boot Manager  HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\Microsoft\Boot\bootmgfw.efi)WINDOWS.........x...B.C.D.O.B.J.E.C.T.=.{.9.d.e.a.8.6.2.c.-.5.c.d.d.-.4.e.7.0.-.a.c.c.1.-.f.3.2.b.3.4.4.d.4.7.9.5.}...a................
Boot0010  Setup FvFile(721c8b66-426c-4e86-8e99-3457c46ab0b9)
Boot0011  Boot Menu     FvFile(86488440-41bb-42c7-93ac-450fbf7766bf)
Boot0012  UEFI Diagnostics      FvFile(f8397897-e203-4a62-b977-9e7e5d94d91b)
Boot0013* NVMe: WDC PC SN730 SDBPNTY-512G-1101          PciRoot(0x0)/Pci(0x2,0x4)/Pci(0x0,0x0)/NVMe(0x1,00-1B-44-8B-46-3A-AB-8F)....2.LN........
Boot0014* ATA HDD:      VenMsg(bc7838d2-0f82-4d60-8316-c068ee79d25b,91af625956449f41a7b91f4f892ab0f600)
Boot0015* ATA HDD1:     VenMsg(bc7838d2-0f82-4d60-8316-c068ee79d25b,91af625956449f41a7b91f4f892ab0f601)
Boot0016* ATAPI CD:     VenMsg(bc7838d2-0f82-4d60-8316-c068ee79d25b,aea2090adfde214e8b3a5e471856a354)
Boot0017* USB HDD: SMI USB DISK PciRoot(0x0)/Pci(0x8,0x1)/Pci(0x0,0x3)/USB(1,0)/USB(1,0)3.!..3.G..A.....
Boot0018* USB FDD:      VenMsg(bc7838d2-0f82-4d60-8316-c068ee79d25b,6ff015a28830b543a8b8641009461e49)
Boot0019* USB CD:       VenMsg(bc7838d2-0f82-4d60-8316-c068ee79d25b,86701296aa5a7848b66cd49dd3ba6a55)

# 新增一个引导，说实话 -p 后面跟 1 还是 4 我不太理解，我都试验过了，发现 1 可以。删除用（删除 0001 为例）： sudo efibootmgr -b 0001 -B
$ sudo efibootmgr -c -w -L Manjaro -d /dev/nvme0n1 -p 1 -l /mnt/boot/efi/EFI/Manjaro/grubx64.efi
```

启动之后不行，进入了 win11 并开始了 windows 的自动修复。。。晒晒我的 grub 配置（/etc/default/grub）

```bash
GRUB_DEFAULT=saved
GRUB_TIMEOUT=3
#GRUB_TIMEOUT_STYLE=hidden
GRUB_TIMEOUT_STYLE=menu
GRUB_DISTRIBUTOR="Manjaro"
#GRUB_CMDLINE_LINUX_DEFAULT="quiet udev.log_priority=3"
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""
```

## 成功的修复

主要操作和 [GRUB/Restore the GRUB Bootloader](https://wiki.manjaro.org/index.php/GRUB/Restore_the_GRUB_Bootloader/en) 一致，不同之处在于 efibootmgr 添加引导，路径和分区整的我都混乱了，所以这里的添加引导顺序、路径啥的我也不记得哪个是对的了

```bash
$ lsblk -o PATH,PTTYPE,PARTTYPE,FSTYPE,PARTTYPENAME
PATH               PTTYPE PARTTYPE                             FSTYPE   PARTTYPENAME
/dev/loop0                                                     squashfs
/dev/loop1                                                     squashfs
/dev/loop2                                                     squashfs
/dev/loop3                                                     squashfs
/dev/sda           dos
/dev/sda1          dos    0x7                                  exfat    HPFS/NTFS/exFAT
/dev/sda2          dos    0xef                                 vfat     EFI (FAT-12/16/32)
/dev/mapper/ventoy dos                                         iso9660
/dev/nvme0n1       gpt
/dev/nvme0n1p1     gpt    c12a7328-f81f-11d2-ba4b-00a0c93ec93b vfat     EFI System
/dev/nvme0n1p2     gpt    e3c9e316-0b5c-4db8-817d-f92df00215ae          Microsoft reserved
/dev/nvme0n1p3     gpt    0fc63daf-8483-4772-8e79-3d69d8477de4 ext4     Linux filesystem
/dev/nvme0n1p4     gpt    0fc63daf-8483-4772-8e79-3d69d8477de4 ext4     Linux filesystem
/dev/nvme0n1p5     gpt    ebd0a0a2-b9e5-4433-87c0-68b6b72699c7 ntfs     Microsoft basic data
/dev/nvme0n1p6     gpt    ebd0a0a2-b9e5-4433-87c0-68b6b72699c7 ntfs     Microsoft basic data

# 清楚的记得我的硬盘 /dev/nvme0n1，从 EFI System 就可以知道 UEFI 装在这个盘里
$  sudo fdisk -l /dev/nvme0n1
Disk /dev/nvme0n1: 476.94 GiB, 512110190592 bytes, 1000215216 sectors
Disk model: WDC PC SN730 SDBPNTY-512G-1101
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 614A2A42-ED51-4C2D-9F82-A1F3105BA07D

Device             Start        End   Sectors   Size Type
/dev/nvme0n1p1      2048     534527    532480   260M EFI System
/dev/nvme0n1p2    534528     567295     32768    16M Microsoft reserved
/dev/nvme0n1p3    616448  494850047 494233600 235.7G Linux filesystem
/dev/nvme0n1p4 494850048  746508287 251658240   120G Linux filesystem
/dev/nvme0n1p5 746508288  861851647 115343360    55G Microsoft basic data
/dev/nvme0n1p6 861851648 1000212479 138360832    66G Microsoft basic data

$ sudo su

# pamac install manjaro-tools-base
Error: target not found: manjaro-tools-base
# 没有找到这个实属意外，但是实测不影响接下来的操作

# manjaro-chroot -a
==> Mounting (ManjaroLinux) [/dev/nvme0n1p4]
 --> mount: [/mnt]
 --> mount: [/mnt/boot/efi]

# pacman -Syu grub
# grub-install --force --target=i386-pc --recheck --boot-directory=/boot /dev/nvme0n1
Installing for i386-pc platform.
grub-install: warning: this GPT partition label contains no BIOS Boot Partition; embedding won\'t be possible.
grub-install: warning: Embedding is not possible.  GRUB can only be installed in this setup by using blocklists.  However, blocklists are UNRELIABLE and their use is discouraged..
Installation finished. No error reported.

# grub-mkconfig -o /boot/grub/grub.cfg
enerating grub configuration file ...
Found theme: /usr/share/grub/themes/manjaro/theme.txt
Found linux image: /boot/vmlinuz-5.15-x86_64
Found initrd image: /boot/amd-ucode.img /boot/initramfs-5.15-x86_64.img
Found initrd fallback image: /boot/initramfs-5.15-x86_64-fallback.img
Found linux image: /boot/vmlinuz-5.10-x86_64
Found initrd image: /boot/amd-ucode.img /boot/initramfs-5.10-x86_64.img
Found initrd fallback image: /boot/initramfs-5.10-x86_64-fallback.img
Warning: os-prober will be executed to detect other bootable partitions.
Its output will be used to detect bootable binaries on them and create new boot entries.
Adding boot menu entry for UEFI Firmware Settings ...
Found memtest86+ image: /boot/memtest86+/memtest.bin
/usr/bin/grub-probe: warning: unknown device type nvme0n1.
done

# grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=manjaro --recheck
Installing for x86_64-efi platform.
Installation finished. No error reported.

# grub-mkconfig -o /boot/grub/grub.cfg
Generating grub configuration file ...
Found theme: /usr/share/grub/themes/manjaro/theme.txt
Found linux image: /boot/vmlinuz-5.15-x86_64
Found initrd image: /boot/amd-ucode.img /boot/initramfs-5.15-x86_64.img
Found initrd fallback image: /boot/initramfs-5.15-x86_64-fallback.img
Found linux image: /boot/vmlinuz-5.10-x86_64
Found initrd image: /boot/amd-ucode.img /boot/initramfs-5.10-x86_64.img
Found initrd fallback image: /boot/initramfs-5.10-x86_64-fallback.img
Warning: os-prober will be executed to detect other bootable partitions.
Its output will be used to detect bootable binaries on them and create new boot entries.
Adding boot menu entry for UEFI Firmware Settings ...
Found memtest86+ image: /boot/memtest86+/memtest.bin
/usr/bin/grub-probe: warning: unknown device type nvme0n1.
done
```

最后成功进入了原来的 manjaro，又重新生成了一次 grub，这次就找到了 win11。

# 参考

- [[Succeed]rEFind安装之在Deepin上的一番折腾~怀疑联想~Could not prepare Boot variable: No space left on device. Kearney. 2021-02-19](https://blog.csdn.net/weixin_43031092/article/details/113855135)

