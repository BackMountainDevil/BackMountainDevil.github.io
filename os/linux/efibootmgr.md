# efibootmgr 管理启动项
- date: 2022-09-19
- lastmod: 2022-09-19

朋友的某款设备是联想小新 Pro-13 2020，BIOS（F0CN35WW）中只能对启动顺序进行调整，并不能删除某一项启动项。

- 查看启动项：sudo efibootmgr -v
- 删除启动项000x：sudo efibootmgr -b 000x -B 

## 例子

```bash
BootCurrent: 0001
BootOrder: 0001,0000,0013,0003,0014,0015,0016,0017,0018,0019
Boot0000* Windows Boot Manager  HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\Microsoft\Boot\bootmgfw.efi)
Boot0001* Manjaro       HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\Manjaro\grubx64.efi)
Boot0003* Debian        HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\Debian\shimx64.efi)

$ sudo efibootmgr -b 3 -B   # 删除3号启动项 Debian
BootCurrent: 0001
Timeout: 0 seconds
BootOrder: 0001,0000,0013,0014,0015,0016,0017,0018,0019
Boot0000* Windows Boot Manager  HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\Microsoft\Boot\bootmgfw.efi)
Boot0001* Manjaro       HD(1,GPT,171ffb3e-6c37-4801-8b11-3ba3cea2e27a,0x800,0x82000)/File(\EFI\Manjaro\grubx64.efi)
Boot0010  Setup FvFile(721c8b66-426c-4e86-8e99-3457c46ab0b9)
Boot0011  Boot Menu     FvFile(86488440-41bb-42c7-93ac-450fbf7766bf)
```
# 参考

- [efibootmgr.8 Arch Man](https://man.archlinux.org/man/efibootmgr.8)：帮助手册和例子，从案例中可以看出删除启动项的时候前面不需要补0
- [如何卸载一个操作系统-以卸载Linux Deepin为例. Kearney form An idea. 2021-03-01](https://blog.csdn.net/weixin_43031092/article/details/114263189)
- [[Succeed]rEFind安装之在Deepin上的一番折腾~怀疑联想~Could not prepare Boot variable: No space left on device. Kearney form An idea.于 2021-02-19](https://blog.csdn.net/weixin_43031092/article/details/113855135):添加启动项报错 fibootmgr: Could not prepare Boot variable: No space left on device，解决方案： sudo grub-install -v
