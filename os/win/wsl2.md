# WSL2 在 windows 中使用 linux
- date: 2024-10-26

安装参考[官网](https://learn.microsoft.com/zh-cn/windows/wsl/install)操作即可，安装完，可在设置/应用里看到显示 Ubuntu 1.16GB，Ubuntu-22.04 LTS 1.22G

# cmd

`--list` 等同于 `-l`，表示列出所有 wsl 虚拟机

```bash
> wsl -l -v
  NAME            STATE           VERSION
* Ubuntu-22.04    Stopped         2
  Ubuntu          Stopped         2
> wsl --list
Windows Subsystem for Linux Distributions:
Ubuntu-22.04 (Default)
Ubuntu
```



# 安装位置

默认是 `C:\Users\<用户名>\AppData\Local\Packages`

```bash
> cd C:\Users\kearney\AppData\Local\Packages
PS C:\Users\kearney\AppData\Local\Packages> ls | findstr Ubuntu
d-----         2023/9/19     13:51                CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc	1.21 GB
d-----         2023/9/19     13:48                CanonicalGroupLimited.Ubuntu_79rhkp1fndgsc		1.15 GB
```

初始只有 1 GB 多一些，之后使用会增大，看了下有 4.15 GB

wsl --unregister Ubuntu

在文件管理器的地址栏输入 `\\wsl$` 可以看到有几个 linux，并且可以直接访问其分区，访问时会激活对应的 wsl

### 移动安装路径

这里使用的是 [move-wsl](https://github.com/pxlrbt/move-wsl) 脚本，具体细节也就 4 个命令，脚本可以省一点操作

```bash
> wsl -l -v
  NAME                   STATE           VERSION
* Ubuntu-22.04           Stopped         2
  docker-desktop-data    Stopped         2
  docker-desktop         Stopped         2
> wsl --list
Windows Subsystem for Linux Distributions:
Ubuntu-22.04 (Default)
docker-desktop-data
docker-desktop
> docker ps
error during connect: Get "http://%2F%2F.%2Fpipe%2FdockerDesktopLinuxEngine/v1.45/containers/json": open //./pipe/dockerDesktopLinuxEngine: The system cannot find the file specified.
```

Ubuntu-22.04 安装在上面提到的默认路径。这里确认 wsl 和 docker 服务都已经关闭了。然后复制上面的脚本（`move-wsl.ps1`），接下来就是运行脚本，等待结果就行

```bash
> ./move-wsl.ps1
Getting distros...
Select distro to move:
1: Ubuntu-22.04
2: docker-desktop-data
3: docker-desktop
1
Enter WSL target directory:
E:\wsl\ubuntu22
Move Ubuntu-22.04 to "E:\wsl\ubuntu22"? (Y|n): Y
Exporting VHDX to "E:\wsl\ubuntu22\Ubuntu-22.04.tar" ...
Export in progress, this may take a few minutes...wsl: Unknown key 'wsl2.dnsTunneling' in C:\Users\kearney\.wslconfig:3
wsl: Unknown key 'wsl2.firewall' in C:\Users\kearney\.wslconfig:4
wsl: Unknown key 'wsl2.autoProxy' in C:\Users\kearney\.wslconfig:5
wsl: Invalid networking mode 'mirrored' in C:\Users\kearney\.wslconfig

The operation completed successfully.
Unregistering WSL ...
Importing Ubuntu-22.04 from E:\wsl\ubuntu22...
Import in progress, this may take a few minutes.
The operation completed successfully.
Cleaning up ...
Done!
```

上面过程中出现了一些未知/无效的配置和迁移脚本没啥关系，是我本地配置文件有问题（建议提前删除错误的配置文件）。迁移完成之后的文件为 `E:\wsl\ubuntu22\ext4.vhdx`。脚本里也说明了迁移后默认发行版本可能发生变更，比如下面默认发行版就变成了 docker（前面的星号可以看出来是默认），直接运行 wsl 的话就不会默认启动 ubuntu了，如下所示运行了 docker-desktop-data 然后发送错误。这个稍微设置一下（`wsl -s Ubuntu-22.04`）即可将默认启动修改为 ubuntu

```bash
> wsl -l -v
  NAME                   STATE           VERSION
* docker-desktop-data    Stopped         2
  Ubuntu-22.04           Stopped         2
  docker-desktop         Stopped         2
> wsl --list
Windows Subsystem for Linux Distributions:
docker-desktop-data (Default)
Ubuntu-22.04
docker-desktop

> wsl
Processing fstab with mount -a failed.

<3>WSL (8) ERROR: CreateProcessEntryCommon:370: getpwuid(0) failed 2
<3>WSL (8) ERROR: CreateProcessEntryCommon:374: getpwuid(0) failed 2
<3>WSL (8) ERROR: CreateProcessEntryCommon:577: execvpe /bin/sh failed 2
<3>WSL (8) ERROR: CreateProcessEntryCommon:586: Create process not expected to return

> wsl -s Ubuntu-22.04 # 设置默认发行版
The operation completed successfully. 
> wsl -l -v 
  NAME                   STATE           VERSION
* Ubuntu-22.04           Stopped         2
  docker-desktop-data    Stopped         2
  docker-desktop         Stopped         2
> wsl
#
```


然后发现登录 wsl2 的默认用户改成 root 了，然后需要通过下面的指令设置回我之前的用户 kearney

```
ubuntu2204.exe config --default-user kearney
```

# docker

- [WSL 2 上的 Docker 远程容器入门](https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/wsl-containers)

安装好之后会看到 docker-desktop 发行版（之前我装的时候还有 docker-desktop-data），默认安装路径是 `C:\Users\<用户名>\AppData\Local\Docker\wsl`，由于之前安装后装镜像导致 C 盘炸裂，于是将 Docker 路径（`C:\Users\kearney\AppData\Local\Docker`）映射到了别的盘，看了一下文件夹体积大小有 52 GB，`Docker\wsl` 有 36 GB

# 挂载 Linux 磁盘

- [在 WSL 2 中装载 Linux 磁盘](https://learn.microsoft.com/zh-cn/windows/wsl/wsl2-mount-disk)：测试了用 wsl2 挂载硬盘盒里的两块 ext4 硬盘。文章有误导的地方是用 `blkid <BlockDevice>` 检测文件系统类型，我加上设备名称后都没有输出，直到我啥设备名、参数都不加，直接运行 blkid 就可以了

需要打开具有管理员权限的终端，不然会报错，比如 vsc 自带的终端就会得到错误

```
Administrator access is needed to mount a disk.
Error code: Wsl/Service/AttachDisk/WSL_E_ELEVATION_NEEDED_TO_MOUNT_DISKPS
```

## brtfs

[BTRFS on WSL2. December 14, 2020. ](https://blog.bryanroessler.com/2020-12-14-btrfs-on-wsl2/)

[在Windows中读取你的Btrfs分区的硬盘](https://hxyuki.github.io/posts/read-your-btrfs-disk-in-windows/)

以上两篇文章都是 bare 挂载，进入 wsl2 再手动挂载，取消时先再 wsl2 里面解挂(这一步我在下面的实操中忘了)，退出 wsl2 后再解挂

```powershell
# 列出 Windows 中的可用磁盘，需要自己根据名字或分区来区分，比如下面 SN730 512G 推测是我电脑硬盘，另外一个则是我外接的硬盘盒里插的 ST4000VX 4T
> GET-CimInstance -query "SELECT * from Win32_DiskDrive"

DeviceID           Caption                        Partitions Size     
--------           -------                        ---------- ----     
\\.\PHYSICALDRIVE0 WDC PC SN730 SDBPNTY-512G-1101 5          512105...
\\.\PHYSICALDRIVE1 ST4000VX 015-3CU104 USB Device 2          400078...

> wsl --mount \\.\PHYSICALDRIVE1 --bare
The operation completed successfully.
```

到此看似挂载成功，其实进入 mnt 发现没有啊，我尝试加分区格式、分区号（`wsl --mount \\.\PHYSICALDRIVE1 --partition 0 --type brtfs`）来进行挂载反而提示错误 - `The disk was attached but failed to mount: No such device. For more details, run 'dmesg' inside WSL2.`. 后来反复测试得到一个结论，不需要先指定分区、格式，直接 bare 挂上，再进入 wsl2 进行手动挂载就好了，如下所示


```bash
> wsl
$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda      8:0    0 363.3M  1 disk
sdb      8:16   0     2G  0 disk [SWAP]
sdc      8:32   0   3.6T  0 disk
├─sdc1   8:33   0   1.6T  0 part
└─sdc2   8:34   0     2T  0 part
sdd      8:48   0     1T  0 disk /snap
                                 /mnt/wslg/distro
                                 /
$  blkid
...
/dev/sdc2: LABEL="vx152" UUID="3...3" UUID_SUB="6...1" BLOCK_SIZE="4096" TYPE="btrfs" PARTUUID="a...b"
/dev/sdc1: LABEL="vx151" UUID="f...2" UUID_SUB="6...a" BLOCK_SIZE="4096" TYPE="btrfs" PARTUUID="d...8"
/dev/sda: BLOCK_SIZE="4096" TYPE="ext4"
$ cd /mnt
$ ls
c  d  e  wsl  wslg
$ sudo mkdir vx151
$ sudo mount /dev/sdc1 vx151/
$ sudo mkdir vx152
$ sudo mount /dev/sdc2 vx152/
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
...
/dev/sdd       1007G  3.2G  953G   1% /
drvfsa           55G   45G  9.5G  83% /mnt/c
/dev/sdc1       1.7T  1.7T   23G  99% /mnt/vx151
/dev/sdc2       2.0T  1.5T  575G  72% /mnt/vx152
$ sudo umount vx151 # 传输完成用 umount 取消挂载
$ sudo umount vx152
# # ctrl+d 退出 wsl2
logout
PS C:\Users\kearney> wsl --unmount \\.\PHYSICALDRIVE1 # 取消挂载
The operation completed successfully.
```

wsl2 用 umount 取消挂载后，再用 `wsl --unmount`，最后在系统托盘弹出硬盘盒

# Q&A
## 网络设置镜像模式

[使用 WSL 访问网络应用程序 2023/12/06](https://learn.microsoft.com/zh-cn/windows/wsl/networking)

设置网络镜像模式的两个 powershell 命令都会出现 CommandNotFoundException，如下所示

```bash
> Set-NetFirewallHyperVVMSetting
Set-NetFirewallHyperVVMSetting : The term 'Set-NetFirewallHyperVVMSetting' is not recognized as the name of a cmdlet, f
unction, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the p
ath is correct and try again.
At line:1 char:1
+ Set-NetFirewallHyperVVMSetting
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (Set-NetFirewallHyperVVMSetting:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```

阅读[相关问题](https://github.com/microsoft/WSL/discussions/11380)后尝试运行 `import-module -name netsecurity` 后再次尝试还是无法识别，根据里面的查看安全模块的组件，确实没有对应的组件，可是我这个也是 win11

```bash
> import-module -name netsecurity
> Set-NetFirewallHyperVVMSetting
    + FullyQualifiedErrorId : CommandNotFoundException

> (get-module -name netsecurity).exportedcommands

Key                                     Value
---                                     -----
Get-DAPolicyChange                      Get-DAPolicyChange
...
Set-NetFirewallAddressFilter            Set-NetFirewallAddressFilter
Set-NetFirewallApplicationFilter        Set-NetFirewallApplicationFilter
Set-NetFirewallInterfaceFilter          Set-NetFirewallInterfaceFilter
Set-NetFirewallInterfaceTypeFilter      Set-NetFirewallInterfaceTypeFilter
Set-NetFirewallPortFilter               Set-NetFirewallPortFilter
Set-NetFirewallProfile                  Set-NetFirewallProfile
Set-NetFirewallRule                     Set-NetFirewallRule
Set-NetFirewallSecurityFilter           Set-NetFirewallSecurityFilter
Set-NetFirewallServiceFilter            Set-NetFirewallServiceFilter
Set-NetFirewallSetting                  Set-NetFirewallSetting
...
Update-NetIPsecRule                     Update-NetIPsecRule
```

暂时忽略这个问题，编辑好 `.wslconfig` 后启动会显示配置错误，不过依旧可以进入系统，如下所示

```bash
> wsl
wsl: Unknown key 'wsl2.dnsTunneling' in C:\Users\kearney\.wslconfig:3
wsl: Unknown key 'wsl2.firewall' in C:\Users\kearney\.wslconfig:4
wsl: Unknown key 'wsl2.autoProxy' in C:\Users\kearney\.wslconfig:5
wsl: Invalid networking mode 'mirrored' in C:\Users\kearney\.wslconfig
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.90.1-microsoft-standard-WSL2 x86_64)
```

上面这些未识别的原因是 win11 21H2 版本导致的，在 23H2 上就没有这个问题。文档里也有 “镜像模式网络 运行 Windows 11 22H2 及更高版本”

# refer

[配置 WSL2  April 30, 2024](https://ofoacimr.github.io/posts/wsl2-setup/#%E8%AE%BE%E7%BD%AE-wsl2)

[ wsl docker 安装位置迁移 - windows bigroc2024-08-15](https://www.cnblogs.com/bigroc/p/18361112)

[【WSL2教程】WSL Ubuntu子系统迁移到非系统盘（4个命令迁移）赛say 2023-06-10](https://zhuanlan.zhihu.com/p/636079359)

