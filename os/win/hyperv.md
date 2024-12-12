# Windows Hyper-V 初探
- date: 2024-12-11

Windows Hyper-V 可以在 Windows 上以虚拟机形式运行多个操作系统。和 VMware、VirtualBox 类似。

[Hyper-V 简介](https://learn.microsoft.com/zh-cn/virtualization/hyper-v-on-windows/about/)有说需要 GPU 的应用可能无法正常运行。

[在 Windows 上安装 Hyper-V](https://learn.microsoft.com/zh-cn/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) 很简单，比安装 vm/vb 简单。

[使用 Hyper-V 创建虚拟机](https://learn.microsoft.com/zh-cn/virtualization/hyper-v-on-windows/quick-start/create-virtual-machine) 的过程也不难，和 VMware 差不多。因此需要提前准备好操作系统的 iso 文件。

使用 hyperv 安装 win 10 ltsc 2021，选好 iso 后，启动时会从 cd 启动，屏幕提示要摁任意键，记得激活运行窗口后快点摁，不然就会进入 “Start PXE over IPv4”，然后就不动弹了，这种清空下关闭虚拟机，再次启动，咔咔摁键盘，才会进入安装系统界面，不然就往复循环 PXE...

从宿主机复制文件到虚拟机，直接复制粘贴即可。

使用体验感觉没啥，因为暂时没有遇到什么问题。
