# windwos11 更新 BIOS 覆盖了 endevaourOS linux 的启动引导 grub 重建修复
- date: 2022-10-30
- lastmod: 2023-3-24

# 问题为什么发生

我在 windows11 从官网下载并更新了 BIOS 固件，发现大部分 BIOS 设置被重置了（BIOS密码没有被重置），需要重新设置安全按启动、UMA size等。启动项里边看不到 endevaourOS 了，只剩下 windows 了，开始直接进入 win，之前设置的开机 log（OEM log）也被重置了。

# 怎么办

在 [forum](https://forum.endeavouros.com)搜索了一波，发现 [Restore grub after window 8 installation 2021-10](https://forum.endeavouros.com/t/restore-grub-after-window-8-installation/18268) 中的回答还蛮仔细

1. `efibootmgr -v` 看 en 启动项还在不在，在就调整顺序即可。我的无拉
2. 第一步不在就需要从新生成引导。关键在于分清楚根分区在那个盘块

## 工具

- 硬件：U盘（存放系统镜像）
- 软件：Ventory（给 U 盘加上装系统的功能）、系统镜像（liveCD 急救）

# livsCD 重建 grub

插上 u盘，从 u 盘进入 endevaourOS 的 liveCD. 查看en引导已不在、查看分区重建引导

```bash
$ efibootmgr    # 查看启动项， en 已经不见了
BootCurrent: 0017
Timeout: 0 seconds
BootOrder: 0000,0013,0014,0015,0016,0017,0018,0019
Boot0000* Windows Boot Manager	HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\Microsoft\Boot\bootmgfw.efi)
Boot0010  Setup	FvFile(721c8b66-426c-4e86-8e99-3457c46ab0b9)
Boot0011  Boot Menu	FvFile(86488440-41bb-42c7-93ac-450fbf7766bf)
Boot0012  UEFI Diagnostics	FvFile(f8397897-e203-4a62-b977-9e7e5d94d91b)
Boot0013* NVMe: WDC PC SN730 SDBPNTY-512G-1101         	PciRoot(0x0)/Pci(0x2,0x4)/Pci(0x0,0x0)/NVMe(0x1,00-1B-44-8B-46-3A-AB-8F){99191c00-d932-4e4c-ae9a-a0b6e98eb8a4}
Boot0017* USB HDD: SMI USB DISK	PciRoot(0x0)/Pci(0x8,0x1)/Pci(0x0,0x3)/USB(5,0){aa21e833-33af-47bc-89bd-419f88c50803}

# 首先在文件管理器里挂载 endeavour 所在的分区，可以在文件管理器看到内容，40G是我的系统盘，80G是我的用户数据盘
[liveuser@eos-2022.09.10 ~]$ df -h
Filesystem          Size  Used Avail Use% Mounted on
dev                 5.8G     0  5.8G   0% /dev
run                 5.8G  9.6M  5.8G   1% /run
/dev/mapper/ventoy  1.9G  1.9G     0 100% /run/archiso/bootmnt
/dev/loop0          1.7G  1.7G     0 100% /run/archiso/airootfs
/dev/nvme0n1p4       79G   33G   42G  44% /run/media/liveuser/a3a2ba69-8213-4723-ba8d-b4b8470dd7eb
/dev/nvme0n1p2       40G   18G   20G  49% /run/media/liveuser/ff0d5e61-82bc-425b-b4dd-2fed5ad072bc


[liveuser@eos-2022.09.10 ~]$ sudo mount /dev/nvme0n1p2 /mnt # 挂载系统根分区到 /mnt
[liveuser@eos-2022.09.10 ~]$ sudo mount /dev/nvme0n1p1 /mnt/efi # 尝试挂载 efi 分区到 /mnt/efi，运行失败
mount: /mnt/efi: mount point does not exist.
       dmesg(1) may have more information after failed mount system call.
[liveuser@eos-2022.09.10 ~]$ sudo mount /dev/nvme0n1p1 /mnt/boot/efi    # 挂载 efi 分区到 /mnt/boot/efi
[liveuser@eos-2022.09.10 ~]$ sudo arch-chroot /mnt  # 不是很懂，大概就是把这个挂载点认为是根分区
[root@EndeavourOS /]# os-prober # 尝试探测其它系统，运行失败
ERROR: mkdir /var/lock/dmraid

[root@EndeavourOS /]# grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=EnOS   # 将 grub 安装为 bootloader
Installing for x86_64-efi platform.
Installation finished. No error reported.

[root@EndeavourOS /]# grub-mkconfig -o /boot/grub/grub.cfg  # 生成配置文件
Generating grub configuration file ...
Found background: /usr/share/endeavouros/splash.png
Found linux image: /boot/vmlinuz-linux
Found initrd image: /boot/amd-ucode.img /boot/initramfs-linux.img
Found fallback initrd image(s) in /boot:  amd-ucode.img initramfs-linux-fallback.img
Found linux image: /boot/vmlinuz-linux-lts
Found initrd image: /boot/amd-ucode.img /boot/initramfs-linux-lts.img
Found fallback initrd image(s) in /boot:  amd-ucode.img initramfs-linux-lts-fallback.img
Warning: os-prober will be executed to detect other bootable partitions.
Its output will be used to detect bootable binaries on them and create new boot entries.
Adding boot menu entry for UEFI Firmware Settings ...
done

[root@EndeavourOS /]# efibootmgr # 有上面生成的 EnOS 并且是第一启动项
BootCurrent: 0017
Timeout: 0 seconds
BootOrder: 0001,0017,0000,0013,0014,0015,0016,0018,0019
Boot0000* Windows Boot Manager	HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\Microsoft\Boot\bootmgfw.efi)57494e444f5753000100000088000000780000004200430044004f0042004a004500430054003d007b00390064006500610038003600320063002d0035006300640064002d0034006500370030002d0061006300630031002d006600330032006200330034003400640034003700390035007d00000000000100000010000000040000007fff0400
Boot0001* EnOS	HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\EnOS\grubx64.efi)
Boot0010  Setup	FvFile(721c8b66-426c-4e86-8e99-3457c46ab0b9)
Boot0011  Boot Menu	FvFile(86488440-41bb-42c7-93ac-450fbf7766bf)
Boot0012  UEFI Diagnostics	FvFile(f8397897-e203-4a62-b977-9e7e5d94d91b)
Boot0013* NVMe: WDC PC SN730 SDBPNTY-512G-1101         	PciRoot(0x0)/Pci(0x2,0x4)/Pci(0x0,0x0)/NVMe(0x1,00-1B-44-8B-46-3A-AB-8F){99191c00-d932-4e4c-ae9a-a0b6e98eb8a4}
Boot0014* ATA HDD:	VenMsg(bc7838d2-0f82-4d60-8316-c068ee79d25b,91af625956449f41a7b91f4f892ab0f600)
Boot0015* ATA HDD1:	VenMsg(bc7838d2-0f82-4d60-8316-c068ee79d25b,91af625956449f41a7b91f4f892ab0f601)
Boot0016* ATAPI CD:	VenMsg(bc7838d2-0f82-4d60-8316-c068ee79d25b,aea2090adfde214e8b3a5e471856a354)
Boot0017* USB HDD: SMI USB DISK	PciRoot(0x0)/Pci(0x8,0x1)/Pci(0x0,0x3)/USB(5,0){aa21e833-33af-47bc-89bd-419f88c50803}
Boot0018* USB FDD:	VenMsg(bc7838d2-0f82-4d60-8316-c068ee79d25b,6ff015a28830b543a8b8641009461e49)
Boot0019* USB CD:	VenMsg(bc7838d2-0f82-4d60-8316-c068ee79d25b,86701296aa5a7848b66cd49dd3ba6a55)
[root@EndeavourOS /]# cat /etc/fastab   # 和上次结果不一样，不过最后还是可以进系统
cat: /etc/fastab: No such file or directory
```

最后成功进入了原来的 endeavouros，又重新生成了一次 grub（`sudo grub-mkconfig -o /boot/grub/grub.cfg`），这次就找到了 win11 并将其加入 grub 中。

# os 探测 与 log 修改

现在启动会进入 en,但是没有 win11了，还有连线狗头图标。
启动之后不行，进入了 win11 并开始了 windows 的自动修复。。。晒晒我的 grub 配置（）

```bash
$ sudo nano /etc/default/grub

GRUB_DEFAULT=0
GRUB_TIMEOUT=1
GRUB_TIMEOUT_STYLE=hidden
GRUB_DISTRIBUTOR="EndeavourOS"
#GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 nowatchdog nvme_load=YES"
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""
GRUB_DISABLE_OS_PROBER=false    # 加上这一句


$ sudo grub-mkconfig -o /boot/grub/grub.cfg # 从新生成配置
```

修改开机 logo： [Customize Lenovo PC boot logo (screen) without installing "Lenovo PC Manager".](https://github.com/Coxxs/LogoDiy/)，不需要装管家，tnnd 管家卸载了背景就重置了

# Q&A
1. 怎么确定分区的 uuid 和 fstab 中一致？

    /etc/fstab 中 uuid 与 lsblk 一致，不知道为什么在 liveCD 里获取的不一致

    ```bash
    $ lsblk -fs
    NAME      FSTYPE FSVER LABEL      UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
    nvme0n1p1 vfat   FAT32 SYSTEM_DRV 92C4-DD2F                             152.5M    40% /boot/efi
    └─nvme0n1                                                                             
    nvme0n1p2 ext4   1.0              ff0d5e61-82bc-425b-b4dd-2fed5ad072bc   19.1G    46% /
    └─nvme0n1                                                                             
    nvme0n1p3 ext4   1.0   a          3409b138-e5fc-412d-b1d5-bfb236f93257   40.8G    77% /run/media/kearney/a
    └─nvme0n1                                                                             
    nvme0n1p4 ext4   1.0              a3a2ba69-8213-4723-ba8d-b4b8470dd7eb   41.4G    42% /home
    └─nvme0n1                                                                             
    nvme0n1p5 ntfs         C          0AB232A4B2329463                                    
    └─nvme0n1                                                                             
    nvme0n1p6 ntfs         D          127AE7467AE72567                                    
    └─nvme0n1
    ```

2. 删除 efi 下的旧配置是否安全？

    我这里有如下几个，endeavouros 是修复之前的，修复之后应该只用 EnOS了，Lenovo这个是修改 OEM log用的，Manjaro 是卸载残留。

    ```bash
    $ ls /boot/efi/EFI/
    Boot  endeavouros  EnOS  Lenovo  Manjaro  Microsoft
    ```

    [What to do with grub old config?](https://forum.endeavouros.com/t/what-to-do-with-grub-old-config/33317/2) 无用可删
    ```bash
    $ sudo rm /boot/efi/EFI/Manjaro/ -r
    $ sudo rm /boot/efi/EFI/endeavouros/ -r
    ```

# 参考

- [Chroot](https://wiki.archlinux.org/title/Chroot)

- [Arch-chroot BTRFS, System rescue. Joekamprad. December 19, 2022](https://discovery.endeavouros.com/system-rescue/arch-chroot/2022/12/):`sudo lsblk -f` 看不出分区大小以及路径，例如 `/dev/nvme0n1p2` 显示为 `/nvme0n1/nvme0n1p2`

- [Repair a non-booting GRUB by adminMarch 24, 2021](https://discovery.endeavouros.com/system-rescue/repair-a-non-booting-grub/2021/03/)

- [[Succeed]rEFind安装之在Deepin上的一番折腾~怀疑联想~Could not prepare Boot variable: No space left on device. Kearney. 2021-02-19](https://blog.csdn.net/weixin_43031092/article/details/113855135)
