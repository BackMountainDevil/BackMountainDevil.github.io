# linux 下无法在 ntfs 分区写入文件、保存修改。显示“只读文件系统”
- date: 2022-03-16
- lastmod: 2022-03-16

windows 下的分区之前是可以读写的，自从前几天打算切到 win 的时候启动蓝屏了，尝试了一些方法也没有修复。今天想往里面存一些文件发现没法写入了，也没法新建文件，只能读取内容。但是通过查看权限却发现是 777，可是实际操作就很奇怪了。

```bash
$ ls -g
drwxrwxrwx 1 kearney   4096  1月 21 21:13 毕设
drwxrwxrwx 1 kearney   4096  1月 20 16:25 寒假通知
drwxrwxrwx 1 kearney      0  1月 21 20:19 校长信箱
-rwxrwxrwx 1 kearney   2789 12月 27 16:49 选课.txt
drwxrwxrwx 1 kearney   4096  3月 14 09:39 practice

$ rm 选课.txt
rm: 无法删除 '选课.txt': 只读文件系统
```

# 尝试命令行带参数挂载

```bash
$ sudo fdisk -l
设备                起点       末尾      扇区   大小 类型
/dev/nvme0n1p1      2048     534527    532480   260M EFI 系统
/dev/nvme0n1p2    534528     567295     32768    16M Microsoft 保留
/dev/nvme0n1p3    567296  147367935 146800640    70G Microsoft 基本数据
/dev/nvme0n1p4 210282496  494850047 284567552 135.7G Microsoft 基本数据
/dev/nvme0n1p5 494850048  746508287 251658240   120G Linux 文件系统
/dev/nvme0n1p6 746508288  998166527 251658240   120G Linux 文件系统
/dev/nvme0n1p7 998166528 1000214527   2048000  1000M Windows 恢复环境
/dev/nvme0n1p8 147367936  210282495  62914560    30G Linux 文件系统

分区表记录没有按磁盘顺序。

# 此处在 dolphin 中卸载了该分区
$ sudo umount /dev/nvme0n1p4
umount: /dev/nvme0n1p4: 未挂载

$ sudo mount -t ntfs -w /dev/nvme0n1p4 /run/media/kearney/Data
The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
Falling back to read-only mount because the NTFS partition is in an
unsafe state. Please resume and shutdown Windows fully (no hibernation
or fast restarting.)
Could not mount read-write, trying read-only
fuse: failed to access mountpoint /run/media/kearney/Data: 没有那个文件或目录

$ sudo mount -t ntfs -w /dev/nvme0n1p4 /run/media/kearney/
The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
Falling back to read-only mount because the NTFS partition is in an
unsafe state. Please resume and shutdown Windows fully (no hibernation
or fast restarting.)
Could not mount read-write, trying read-only
```

上面显示的错误信息为 “磁盘包含不干净的文件系统 （0， 0）。元数据保存在 Windows 缓存中，拒绝装载。回退到只读挂载，因为 NTFS 分区位于不安全状态。请恢复并完全关闭窗口（无休眠）或快速重启。无法装入读写，尝试只读”

应该是 Windows11 自动修复的 bug 或者系统还原点还原失败的残留影响

## ntfsfix 修复写入问题

刚开始搜索出来的都是使用 ntfsfix 进行修复，但是不确定是不是用来修复上面显示的错误的，查了更多帖子基本可以确定有人出现过同样的状况用该办法修复了。

```bash
# 修复 D 盘。 分区路径以个人实际情况为准（参考 fisk -l 的输出）
$ sudo ntfsfix /dev/nvme0n1p4
Mounting volume... The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
FAILED
Attempting to correct errors... 
Processing $MFT and $MFTMirr...
Reading $MFT... OK
Reading $MFTMirr... OK
Comparing $MFTMirr to $MFT... OK
Processing of $MFT and $MFTMirr completed successfully.
Setting required flags on partition... OK
Going to empty the journal ($LogFile)... OK
Checking the alternate boot sector... OK
NTFS volume version is 3.1.
NTFS partition /dev/nvme0n1p4 was processed successfully.

# 修复 C 盘。 分区路径以个人实际情况为准（参考 fisk -l 的输出）
$ sudo ntfsfix /dev/nvme0n1p3
Mounting volume... The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
FAILED
Attempting to correct errors... 
Processing $MFT and $MFTMirr...
Reading $MFT... OK
Reading $MFTMirr... OK
Comparing $MFTMirr to $MFT... OK
Processing of $MFT and $MFTMirr completed successfully.
Setting required flags on partition... OK
Going to empty the journal ($LogFile)... OK
Checking the alternate boot sector... OK
NTFS volume version is 3.1.
NTFS partition /dev/nvme0n1p3 was processed successfully.
```

## 分区表问题

fdisk 显示最后一行有一个“分区表记录没有按磁盘顺序”，我个人是不想看到任何 warning 和 error 的，搜索了一下中文帖子没啥好办法，切换了一下英文输出搜索到了一些转载的帖子

```bash
$ export LANG=c

$ sudo fdisk -l
Partition table entries are not in disk order.
```

```bash
$ sudo fdisk /dev/nvme0n1

欢迎使用 fdisk (util-linux 2.37.4)。
更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。

This disk is currently in use - repartitioning is probably a bad idea.
It's recommended to umount all file systems, and swapoff all swap
partitions on this disk.


命令(输入 m 获取帮助)：
m

帮助：

  GPT
   M   进入 保护/混合 MBR

  常规
   d   删除分区
   F   列出未分区的空闲区
   l   列出已知分区类型
   n   添加新分区
   p   打印分区表
   t   更改分区类型
   v   检查分区表
   i   打印某个分区的相关信息

  杂项
   m   打印此菜单
   x   更多功能(仅限专业人员)

  脚本
   I   从 sfdisk 脚本文件加载磁盘布局
   O   将磁盘布局转储为 sfdisk 脚本文件

  保存并退出
   w   将分区表写入磁盘并退出
   q   退出而不保存更改

  新建空磁盘标签
   g   新建一份 GPT 分区表
   G   新建一份空 GPT (IRIX) 分区表
   o   新建一份的空 DOS 分区表
   s   新建一份空 Sun 分区表


命令(输入 m 获取帮助)：q
```

上面的提示是红色的，翻译了一下 “此磁盘当前正在使用中 - 重新分区可能是一个坏主意。建议卸载所有文件系统，并交换所有交换此磁盘上的分区”。鉴于目前双系统中 win 已经崩盘了，现在的 Manjaro 能跑，根据懒人原则就不要动了，输入 q 退出。

# 参考

- [Can't Mount NTFS drive "The disk contains an unclean file system" [duplicate]. 2015](https://askubuntu.com/questions/462381/cant-mount-ntfs-drive-the-disk-contains-an-unclean-file-system)
- [[SOLVED]Partition table entries are not in disk order. 2020-03-05](https://bbs.archlinux.org/viewtopic.php?id=253377)
- [解决Partition table entries are not in disk order 的问题. 2011-08-18 来源：Linux社区  作者：rainday0310](https://www.linuxidc.com/Linux/2011-08/41000.htm)