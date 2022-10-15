# 小米路由器Pro R3p 刷机 Breed Padavan
- date: 2022-10-08
- lastmod: 2022-10-15

一般过程：

1. 小米路由器开发版ROM+小米路由器开启SSH工具文件：rom官网直接下载，含ssh的则需要小米账号在小米WiFi中绑定路由器
2. 刷引导（可跳过
3. 刷固件（系统）

## rom-dev ssh

这一步首先是给路由器刷入开发板的 rom，然后开启 ssh。开启无线 ssh 有官方办法，漏洞办法，实在不行就拆机 TTL 跳线办法。

获取官方 ssh 开启固件需要现在小米 WIFI app 中绑定路由器，我重置路由器的时候 app 可以识别到路由器，让我设置，我设置完名称密码后重启就识别不到了，最后发现是因为我网线没到，所以路由器没联网，连上没有网络的网导致 app 无法识别绑定，我的解决办法另外一个设备开热点，小米路由器中继模式，这样 app 就可以识别出来了。

也就是说这个 app 需要在有网的时候才能识别出来路由器进行设置，但这个逻辑是反常的，完全可以不使用互联网就可以识别路由器并在 app 里面进行控制、读取。有个小心眼的猜测是小米偷跑服务了。举个例子，我可以建立一个和互联网断绝的子网，子网内部完全是互通的，但是小米路由器默认不行。。。

[【路由刷机教程】适用于带USB的小米路由](https://web.vip.miui.com/page/info/mio/mio/detail?postId=19154125)：我用的是 vfat 格式的U盘也行（以前用ventory处理过的）

	FAT或FAT32的U盘;
	3.将下载的ROM包复制到U盘的根目录，并重命名为miwifi.bin ，同时确保该目录下不存在其它“.bin”文件，若存在会导致刷机失败；
	4.断开小米路由器的电源，将U盘插入路由器USB接口
	5.按住reset键，接通电源，等待指示灯变为黄色闪烁状态后松开reset键，路由器开始刷机；
	6.等待刷机完成，整个过程约为3-5分钟，完成后系统会自动重启。路由器指示灯变蓝刷机完成；如果出现异常、失败、U盘无法读取的状况，会进入红灯状态，建议重试或更换U盘再试。

[小米路由器官方固件/app下载](http://miwifi.com/miwifi_download.html)，从固件更新日期可以看出产品最后维护的时间
    
	工具包使用方法：小米路由器需升级到开发版0.5.28及以上，小米路由器mini需升级到开发版0.3.84及以上，小米路由器3即将支持。注意：稳定版不支持。

    请将下载的工具包bin文件复制到U盘（FAT/FAT32格式）的根目录下，保证文件名为miwifi_ssh.bin；
    断开小米路由器的电源，将U盘插入USB接口；
    按住reset按钮之后重新接入电源，指示灯变为黄色闪烁状态即可松开reset键；
    等待3-5秒后安装完成之后，小米路由器会自动重启，之后您就可以尽情折腾啦 ：）

[ tomsiwik/xiaomi-router-patch](https://github.com/tomsiwik/xiaomi-router-patch)：利用 CVE-2019-18370 漏洞开启小米路由器R3G的ssh，漏洞在 2019-03-09 被发现，可以实现无需登录，任意远程命令执行。我测试失败，更换为 dev rom 后成功

[ acecilia/OpenWRTInvasion ](https://github.com/acecilia/OpenWRTInvasion):小米路由器 root shell 漏洞。我测试失败，更换为 dev rom 后成功

[小米路由器 Pro 刷机教程 Powersee  2022-05-02 ](https://www.bilibili.com/video/BV1KY4y1h7Cj):通过py脚本开启ssh、breed、Padavan，一个dd备份命令

[小米路由器Pro刷OpenWrt固件 lambdacalculus 2019.10.07](https://www.jianshu.com/p/8429be3a7b6b)：官刷ssh、OpenWrt，没做备份

[小米路由器Pro（R3P）刷Padavan固件 2021-08-21 华-尔特  ](https://www.bilibili.com/video/BV1kh411i7rn)：不一样的 py 代码，检查后发现是bat脚本带动py，原理和手敲代码一样，自动化处理
> 	刷机备份工具 	链接：https://pan.baidu.com/s/1jSWhNduNLXH63IRJjrg8CA 	提取码：atf8
	padavan固件	https://opt.cn2qq.com/padavan/MI-R3P_3.4.3.9-099.trx

# 检查闪存与备份

先检查闪存品牌和有无坏块

[mi 3pro openwrt](https://openwrt.org/toh/xiaomi/mi_router_3_pro):尽量使用ESMT的闪存,Micron(镁光)的容易因为坏快变砖

> dmesg | grep "Manufacturer ID: "
	ESMT chip will have a “Manufacturer ID” of 0xc8, while a Micron chip will have 0x2c

```
# dmesg | grep "Manufacturer ID: "
[    3.420000] NAND device: Manufacturer ID: 0xc8, Chip ID: 0xda (ESMT NAND 256MiB 3,3V 8-bit), 256MiB, page size: 2048, OOB size: 64
```

## 备份

慎重一点，备份以防万一。全备份，看到两种方式 dd、nanddump ，也都尝试。原厂uboot、eeprom

ALL.bin(255.5M) 和 mtd0.bak (263.5M)经过 sha1 校验是不同的文件,尽管两者都是 mtd0 的备份；eeprom.bin 是 mtd4 的备份，看了 createbackup.py 中的代码，全部备份只备份了 mtd0，要么是代码问题，要么是 mtd0 就是全盘备份

<details>
<summary>备份详细记录</summary>

建立文件夹 xmr3p，使用 dd 命令备份到 u 盘 xmr3p/dd;为了对比,也使用 nanddump 备份到 xmr3p/nd.因此 u 盘要留有 1G 的空闲空间存放备份
```
# cat /proc/mtd	# 读取MTD分区表
dev:    size   erasesize  name
mtd0: 0ff80000 00020000 "ALL"
mtd1: 00040000 00020000 "Bootloader"
mtd2: 00040000 00020000 "Config"
mtd3: 00040000 00020000 "Bdata"
mtd4: 00040000 00020000 "Factory"
mtd5: 00040000 00020000 "crash"
mtd6: 00080000 00020000 "crash_syslog"
mtd7: 00040000 00020000 "reserved0"
mtd8: 00400000 00020000 "kernel0"
mtd9: 00400000 00020000 "kernel1"
mtd10: 02800000 00020000 "rootfs0"
mtd11: 02800000 00020000 "rootfs1"
mtd12: 0a580000 00020000 "overlay"


# df -h	# 根据大小查找 u 盘路径
Filesystem                Size      Used Available Use% Mounted on
rootfs                   19.3M     19.3M         0 100% /
none                    249.1M         0    249.1M   0% /dev
tmpfs                   249.9M      3.6M    246.3M   1% /tmp
/dev/ram0                19.3M     19.3M         0 100% /
tmpfs                   249.9M      3.6M    246.3M   1% /tmp
tmpfs                   249.9M      3.6M    246.3M   1% /extdisks
ubi1_0                  141.3M      6.7M    129.9M   5% /data
ubi1_0                  141.3M      6.7M    129.9M   5% /userdisk
/dev/ram0                19.3M     19.3M         0 100% /userdisk/data
ubi1_0                  141.3M      6.7M    129.9M   5% /etc
/dev/sda1                 7.5G      3.5G      4.0G  47% /extdisks/sda1
/dev/sda2                31.9M     20.3M     11.6M  64% /extdisks/sda2

# cd /extdisks/sda1
# ls
miwifi-dev.bin  breed-mt7621-xiaomi-r3g-r3p.bin  

# mkdir -p xmr3p/dd
# mkdir -p xmr3p/nd
# ls xmr3p/
dd  nd
# cd xmr3p/dd/
# dd if=/dev/mtd0 of=ALL.bin
523264+0 records in
523264+0 records out
267911168 bytes (255.5MB) copied, 90.880289 seconds, 2.8MB/s
# dd if=/dev/mtd1 of=Bootloader.bin
512+0 records in
512+0 records out
262144 bytes (256.0KB) copied, 0.086554 seconds, 2.9MB/s
# dd if=/dev/mtd2 of=Config.bin
512+0 records in
512+0 records out
262144 bytes (256.0KB) copied, 0.086277 seconds, 2.9MB/s
# dd if=/dev/mtd3 of=Bdata.bin
512+0 records in
512+0 records out
262144 bytes (256.0KB) copied, 0.086819 seconds, 2.9MB/s
# dd if=/dev/mtd4 of=Factory.bin
512+0 records in
512+0 records out
262144 bytes (256.0KB) copied, 0.086280 seconds, 2.9MB/s
# dd if=/dev/mtd5 of=crash.bin
512+0 records in
512+0 records out
262144 bytes (256.0KB) copied, 0.086380 seconds, 2.9MB/s
# dd if=/dev/mtd6 of=crash_syslog.bin
1024+0 records in
1024+0 records out
524288 bytes (512.0KB) copied, 0.168056 seconds, 3.0MB/s
# dd if=/dev/mtd7 of=reserved0.bin
512+0 records in
512+0 records out
262144 bytes (256.0KB) copied, 0.086529 seconds, 2.9MB/s
# dd if=/dev/mtd8 of=kernel0.bin
8192+0 records in
8192+0 records out
4194304 bytes (4.0MB) copied, 1.312416 seconds, 3.0MB/s
# dd if=/dev/mtd9 of=kernel1.bin
8192+0 records in
8192+0 records out
4194304 bytes (4.0MB) copied, 1.331081 seconds, 3.0MB/s
# dd if=/dev/mtd10 of=rootfs0.bin
81920+0 records in
81920+0 records out
41943040 bytes (40.0MB) copied, 13.526462 seconds, 3.0MB/s
# dd if=/dev/mtd11 of=rootfs1.bin
81920+0 records in
81920+0 records out
41943040 bytes (40.0MB) copied, 13.473753 seconds, 3.0MB/s
# dd if=/dev/mtd12 of=overlay.bin
338944+0 records in
338944+0 records out
173539328 bytes (165.5MB) copied, 57.000779 seconds, 2.9MB/s
# ls
ALL.bin           Bootloader.bin    Factory.bin       crash_syslog.bin  kernel1.bin       reserved0.bin     rootfs1.bin
Bdata.bin         Config.bin        crash.bin         kernel0.bin       overlay.bin       rootfs0.bin

# cd ../nd
# ls
# nanddump -f mtd0.bak /dev/mtd0
nanddump: warning!: you did not specify a default bad-block handling
  method. In future versions, the default will change to
  --bb=skipbad. Use "nanddump --help" for more information.
nanddump: warning!: in next release, nanddump will not dump OOB
  by default. Use `nanddump --oob' explicitly to ensure
  it is dumped.
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x0ff80000...
# nanddump -f mtd1.bak /dev/mtd1
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x00040000...
r# nanddump -f mtd2.bak /dev/mtd2
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x00040000...
# nanddump -f mtd3.bak /dev/mtd3
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x00040000...
# nanddump -f mtd4.bak /dev/mtd4
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x00040000...
# nanddump -f mtd5.bak /dev/mtd5
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x00040000...
# nanddump -f mtd6.bak /dev/mtd6
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x00080000...
# nanddump -f mtd7.bak /dev/mtd7
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x00040000...
# nanddump -f mtd8.bak /dev/mtd8
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x00400000...
# nanddump -f mtd9.bak /dev/mtd9
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x00400000...
# nanddump -f mtd10.bak /dev/mtd10
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x02800000...
# nanddump -f mtd11.bak /dev/mtd11
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x02800000...
# nanddump -f mtd12.bak /dev/mtd12
ECC failed: 0
ECC corrected: 0
Number of bad blocks: 0
Number of bbt blocks: 0
Block size 131072, page size 2048, OOB size 64
Dumping data starting at 0x00000000 and ending at 0x0a580000...
# ls
mtd0.bak   mtd1.bak   mtd10.bak  mtd11.bak  mtd12.bak  mtd2.bak   mtd3.bak   mtd4.bak   mtd5.bak   mtd6.bak   mtd7.bak   mtd8.bak   mtd9.bak

# dmesg>dmesg.log
# dmesg | grep "bad block"
[    3.440000] Scanning device for bad blocks
# dmesg | less
```
</details>

[ [AC2100(RM2100)] 适配RM2100的pb-boot正式版（大雕）  2020-5-26 BI4TMS](https://www.right.com.cn/forum/thread-4032659-1-1.html)
	
	想要备份路由原厂分区的，可以先在电脑以管理员权限运行/rm2100中的 ftpdmin.exe，之后回到telnet命令行
	例如：想备份mtd1分区，就在命令行输入
	./busybox nanddump -f mtd1.bak /dev/mtd1	# 备份到文件
	./busybox ftpput 192.168.31.177 mtd1.bak ./mtd1.bak	# 
	备存文件存在你rm2100文件所在的硬盘根目录。不想备份也可以，我给的文件中有mtd1和eeprom的备份。

	注意：保险起见，暂时不要通过pb-boot"固件更新"功能直接刷回官方boot，官方的u-boot有毒!!!刷错锁了串口即使能开机也只能编程器!!!

[小米路由器刷入Padavan系统 程屁凹 2018.11.04](https://www.jianshu.com/p/e49f4e19396a)：官刷ssh、breed、Padavan，14个dd备份命令

[nanddump读出nandflash包括坏块 bewinged 2017-11-23](https://blog.csdn.net/zengzhihao/article/details/78617296)

[How is erasing MTD with "dd if=/dev/zero" different from "flash_eraseall"? 2016-4-4](https://unix.stackexchange.com/questions/274217/how-is-erasing-mtd-with-dd-if-dev-zero-different-from-flash-eraseall):MTD 设备不该使用 dd

[MTD - openwrt doc](https://openwrt.org/docs/techref/mtd)
> The differences between dd and mtd are … TODO 

# 刷引导
## breed

`刷 breed 前确认有网线和路由器进行连接，breed 刷固件时不能通过无线连接！！！`

把 breed 拷贝到 U 盘，插到路由器上，通过 `df -h` 查看挂载位置，并进入 U 盘的挂载位置

```
# ls
System Volume Information        
TDDOWNLOAD	miwifi-dev.bin	xmr3p	breed-mt7621-xiaomi-r3g-r3p.bin

# mtd write breed-mt7621-xiaomi-r3g-r3p.bin Bootloader	# 写入breed.bin
Unlocking Bootloader ...

Writing from breed-mt7621-xiaomi-r3g-r3p.bin to Bootloader ... 
# reboot	# 重启命令
# 刷 bl 失败，还是原装管理界面
# mtd write breed-mt7621-xiaomi-r3g-r3p.bin Bootloader -r	# 写入breed.bin 后重启
# 刷 bl 二失败，还是原装管理界面
# cp breed-mt7621-xiaomi-r3g-r3p.bin /tmp/breed.bin
# mtd -r write /tmp/breed.bin Bootloader
# 刷 bl 三失败，还是原装管理界面

# 使用 OpenWRTInvasion 再破 ssh
# cd /extdisks/sda1/
# mtd write breed-mt7621-xiaomi-r3g-r3p.bin Bootloader -r
# 刷 bl 四失败，还是原装管理界面

# cp /extdisks/sda1/xmr3p/breed-mt7621-xiaomi-r3g-r3p.bin /tmp/breed.bin	
# mtd -e Bootloader write /tmp/breed.bin Bootloader	# 先擦除！！！再刷设备
# 刷 bl 第五次失败，还是原装管理界面
```

经历了五次失败，决定今天直接刷 openwrt,反正不是镁光的闪存。然而当我刷完 ow，路由器重启后，热点没看到，插网线（今天刚到）没有出现 ow 的界面，ip 也不通，于是按住 reset 断电重启重置，这回 ip ping 通了，打开却是 breed 的界面。

当终端输出Rebooting ... 5-10s后，将路由器断电重启，等到路由器出现蓝色灯闪烁时，在浏览器输入 192.168.1.1 后回车，即可进入breed的管理页面。选择固件更新时，可以更新 Bootloader、固件、EEPROM，不过固件备份只支持备份EEPROM和编程器固件。（ps,，为了避免变砖，请勿更新编程器固件）

Breed 更新 Bootloader 有大小限制，超过 512 KB 会提示错误。

当刷入固件之后，再输入这个网址不能进入 breed 了，需要按住 reset 断电重启重置后才能由该网址进入 breed。至于如何卸载 breed 恢复官方固件，在 Powersee 的视频里说的是 由 breed 刷 pb-boot 的 bootloader,然后按住 reset 断电重启进入 pb-boot(192.168.1.1), 选择官方引导，然后将官方开发板固件拷贝 u 盘，重命名，按住 reset 断电重启刷回官方开发固件的（和开头的刷 rom-dev 一样）。

## pb-boot

在 Breed 更新 Bootloader 时，上传 pb-boot 固件，更新后断电，按住 reset 重启，5s 后松开，在浏览器输入 192.168.1.1 后回车，即可进入 pb-boot 的管理页面（页面显示的是 PandoraBox）。

这里的选择固件可以是 bootloader. 奇怪的是我上传自己备份的 kernel0.bin、openwrt 下载的 kernel0.bin（和我备份的校验是一样的）、Powersee 提供的 kernel0.bin（和我的校验不一样，但是 Powersee 的闪存和我的是一样的）。总是就是刷了 kernel0, u盘格式化 fat32 复制好 miwifi.bin，断电reset之后也还是 pb-boot。反倒是现在可以在 pb-boot 和 breed 之前反复横跳。然后又根据 pb、breed 的文件大小，我尝试把dd备份时一些体积相似的 bin 也刷过但也无疾而终

### 相关阅读

[Breed](https://www.right.com.cn/forum/thread-161906-1-1.html) [首发地址](https://blog.hackpascal.net/)[ breed 引导固件下载地址](https://breed.hackpascal.net)： Weijie Gao 作品

	这是楼主从去年年中自行设计开发的一个全新的 Bootloader，并用于取代 U-Boot。
	此 Bootloader 暂取名为 Breed，不是 U-Boot，也不是 U-Boot 的改进版，是全新、独立的、跟 U-Boot 平级的 Bootloader
	科普一下：
	Bootloader 意思为引导加载器，即为用于加载操作系统的程序。它是一大类此类功能程序的统称。现在的 BIOS、UEFI、GRUB、RedBoot、U-Boot、CFE、Breed 等都是 Bootloader。
	因此，还是上面那句话，Breed 不是由什么东西改名出来的，这就是一个新的东西。看着有些人的话我真的觉得很搞笑。
	此外，由上面两句话，如果想从 Breed 刷到其他任何 Bootloader，例如 U-Boot，请在 Breed 固件更新页面选择更新 Bootloader。。。。。。。。。。。。
	免费、无限制、不开源

[【路由器】Breed 介绍、刷入和使用 ywang_wnlo 2022-04-06](https://blog.csdn.net/CoolBoySilverBullet/article/details/121077410)
> Breed 是国内个人 hackpascal 开发的闭源 Bootloader，也被称为“不死鸟”

	因为有些官方升级固件自带 bootloader，如果从官方固件升级，会导致现有 bootloader 被覆盖。而当 Breed 更新固件时，它会自动删除固件附带的引导加载程序，因此可以防止 Breed 被覆盖

[ [R3G] 基于r3g的r3p breed RedLeaves 2020-3-10](https://www.right.com.cn/forum/thread-3208008-1-1.html)
	根据 https://www.right.com.cn/forum/thread-2590989-1-1.html guo4qing 提供的思路
	理论上拿breed-mt7621-hiwifi-hc5962.bin作为本体修改内存参数即可
	测试发现r3p刷breed-mt7621-r6220.bin也是可以启动的 波特率57600
	pbr-m1的内存参数有5个 确定是哪一个太麻烦了 直接从breed-mt7621-r6220.bin提取ddr3的参数
	我的r3p改的双启动，测试应该是没有问题的，但是现在引导pandorabox、openwrt之类启动还有问题
	经楼下fyi2000指点，以r3b breed为本体，重新制作了breed-mt7621-xiaomi-r3g-r3p.bin用于r3p
	基于r3g魔改breed好处是大部分现有的固件就都可以刷了，无需重新编译

	breed-mt7621-xiaomi-r3g-r3p.zip sha1:092082b544a00b47d051c5c4b109edc59ad492dd
	不过由于openwrt分区布局的原因，该breed并无法直接刷如固件，需要使用特殊手法刷入
	可以参考https://www.right.com.cn/forum/thread-987254-1-1.html
	简单的说就在breed刷如内存版的openwrt(刷入kernel0)
	重启后进入内存版的openwrt更新固件(更新会被刷入kernel1)
	设置breed环境变量xiaomi.r3g.bootfw值为2从kernel1启动

	小白请勿轻易尝试哈，全砖之后必须拆机才能救，除非你具备救砖能力，改SPI和镁光NAND双启动的可以随便刷

[ [R3G] 小米路由器R3G用Breed安装原生OpenWrt详解   MIRROR-D	 2019-9-8](https://www.right.com.cn/forum/thread-987254-1-1.html)

[小米路由器R3P从潘多拉刷回官方，小白帖 2019-7-30 wanghucehng](https://www.right.com.cn/forum/thread-845287-1-1.html)：kernel0.bin 和 powersee 中一致，与我和openwrt不一致（md5sum）
	电脑浏览器进192.168.1.1，上传kernel0，点恢复固件，
	等一会儿，大概30秒吧，断电，上U盘【u盘根目录放入miwifi.bin这个文件，这个文件是带uboot的】，
	上电，大概5秒【反正就是大概吧，不用那么精确，我都反复刷了好几次】按下reset按钮，指示灯变为黄色闪烁状态后松开reset键；
	好像是等个一两分钟，黄灯变蓝【那个灯我觉得真的不是蓝色，不过大家都那么说的，那就是蓝色吧】就OK，

    fyi2000 2020-4-24
	老毛子已经可以支持R3P，强烈建议刷老毛子，如果要刷回官方，可参考我的签名8楼
	pb-boot不能直接刷老毛子，必须先刷潘多拉或OpenWrt，再以 mtd 命令刷老毛子，或是按照以下方法制作兼容pb-boot的固件，老毛子不支持RootFS以后分区有坏块，所以建议先检查闪存有无坏块以及坏块的位置

[ [PRO(R3P)] 6月4日-小米路由器PRO(R3P)刷入PandoraBox 19.02，测试完美，稳定 laomao9000 2019-6-4](https://www.right.com.cn/forum/thread-701501-1-1.html):12+2 个dd备份指令。官方开发板刷openwrt以及ow刷回官方的办法
> 务必用这个PB BOOT，可以支持镁光Miron Nand和EMST NAND，其他breed会变砖！！！
	
[教你简单的方法给路由器刷入不死breed u-boot 只需这三条命令 MT7620 2019-06-11 机电烧卖 ](https://www.bilibili.com/video/BV1K4411K7YQ)

	mtd -r write /tmp/breed.bin u-boot
	mtd_write write breed.bin u-boot
	mtd_write write breed.bin Bootloader
	mtd -r write /tmp/breed.bin Bootloader
	以上四条命令都是刷入breed的，路由器分区表不同命令不同，我也是一个个试出来

[路由器刷了不死breed就可以为所欲为了？维修师：姿势不对照样变砖！2021-02-25 猫猫无线](https://www.bilibili.com/video/BV1my4y1J7oK)

	breed 也不是真不死，针对不同闪存芯片效果不一样，也能挂
	breed 后刷 openwrt 后升级 ow 在 ow 里面操作，千万不要在 breed 里升级 ow，
	不要在 breed 搞编程器固件，变砖警告
	主要是NAND有ECC区域，这个也就是不同于NOR闪存的区别。（不懂，看起来很高级）

尝试 scp 上传文件到路由器，失败

    ```bash
    $ scp breed-mt7621-xiaomi-r3g-r3p.bin root@192.168.31.1:/tmp/breed.bin  # 将 breedFile.bin 上传到路由器中的 /tmp/breed.bin
    root@192.168.31.1's password: 
    ash: /usr/libexec/sftp-server: not found
    scp: Connection closed
    ```

[小米路由器3刷机教程：刷入潘多拉 和 刷回官方。2020-02-04 蚣蛭艾闽蛛也艾髭鼬](https://www.bilibili.com/read/cv4570903/)

    刷回官方
    将小米路由器官方固件重命名为miwifi.bin放入fat32格式的u盘

    还是需要进入pb-boot界面，在这里刷入Kernel0（潘多拉官方有备份，也可以是个人备份的），系统自动重启后指示灯会闪灭一次，指示灯蓝色闪过后变为黄色常亮，此时断电。或者干脆重启后多喝杯茶，再断电

    路由器断电状态下，将u盘插入usb接口

    长按重置按钮，通电开机，直至指示灯黄色快速闪烁。

    几分钟后你就可以在手机上看到那个消失很久的wifi名称 作者：蚣蛭艾闽蛛也艾髭鼬 https://www.bilibili.com/read/cv4570903/ 出处：bilibili

# 刷固件
## openwrt

由 breed 引导刷 openwrt 有不少麻烦，还容易变砖，先刷回官方开发固件

```bash
cd /extdisks/sda1 #(can be different if you remove and reinsert the usb stick)
mv openwrt-ramips-mt7621-xiaomi_mir3p-squashfs-factory.bin factory.bin # give it a shorter name
nvram set flag_try_sys1_failed=1 
nvram set flag_try_sys2_failed=0 
nvram set flag_boot_success=0 
nvram commit
dd if=factory.bin bs=1M count=4 | mtd write - kernel1
mtd erase rootfs0
mtd erase rootfs1
mtd erase overlay
dd if=factory.bin bs=1M skip=4 | mtd write - rootfs0
reboot
```

<details>
<summary>实操</summary>


```bash
# cd /extdisks/sda1
root@XiaoQiang:/extdisks/sda1# ls
System Volume Information  factory.bin                software                   ??                         ???.txt
TDDOWNLOAD                 music                      uc2-tkinter                ??????????????.doc         ??
ThunderDB                  os                         xiaomi_config              ??
root@XiaoQiang:/extdisks/sda1# nvram set flag_try_sys1_failed=1 
root@XiaoQiang:/extdisks/sda1# nvram set flag_try_sys2_failed=0 
root@XiaoQiang:/extdisks/sda1# nvram set flag_boot_success=0
root@XiaoQiang:/extdisks/sda1# nvram commit
root@XiaoQiang:/extdisks/sda1# dd if=factory.bin bs=1M count=4 | mtd write - kernel1
Unlocking kernel1 ...

Writing from <stdin> to kernel1 ...  [e]4+0 records in
4+0 records out
4194304 bytes (4.0MB) copied, 1.867296 seconds, 2.1MB/s
    
root@XiaoQiang:/extdisks/sda1# mtd erase rootfs0
Unlocking rootfs0 ...
Erasing rootfs0 ...
root@XiaoQiang:/extdisks/sda1# mtd erase rootfs1
Unlocking rootfs1 ...
Erasing rootfs1 ...
root@XiaoQiang:/extdisks/sda1# mtd erase overlay
Unlocking overlay ...
Erasing overlay ...
root@XiaoQiang:/extdisks/sda1# dd if=factory.bin bs=1M skip=4 | mtd write - rootfs0
Unlocking rootfs0 ...

Writing from <stdin> to rootfs0 ...  [e]5+1 records in
5+1 records out
5505024 bytes (5.3MB) copied, 2.381508 seconds, 2.2MB/s
    
root@XiaoQiang:/extdisks/sda1# reboot
root@XiaoQiang:/extdisks/sda1# Connection to 192.168.31.1 closed by remote host.
Connection to 192.168.31.1 closed.
```
</details>

第一次刷ow,是刷完 breed 多次失败后直接刷 ow,刷完 ow 固件后很尴尬，用朋友的电脑插上网线发现重置之后是 breed 的引导，ow 没刷上。

- [OpenWRT (LEDE) 镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/openwrt/)
> sed -i 's_downloads.openwrt.org_mirrors.tuna.tsinghua.edu.cn/openwrt_' /etc/opkg/distfeeds.conf

- [OpenWrt有哪些实用的插件？](https://www.zhihu.com/question/29637794):上网行为管理、广告净化、下载

[openwrt下实现校园网的web认证 a1094174619 2018-10-16](https://blog.csdn.net/a1094174619/article/details/83068307)：curl后总结写了个web

[openwrt下实现校园网的web认证 ccpd_1 2021-08-09](https://blog.csdn.net/qq_30763587/article/details/119523231)：如何从 firefox F12 获取 curl 参数

[ruijanlee/h3cc](https://github.com/ruijanlee/h3cc)：H3c connect - 强力插入你的校园网

[使用OpenWrt +mentohust打造锐捷校园网无线路由 weixin_33872660 2011-04-08](https://blog.csdn.net/weixin_33872660/article/details/93273582)
> 要用openwrt上校园网，只要把libpcap和mentohust装上路由就好

## Padavan

安装了 breed 后，进入 breed 控制台把潘多拉固件上传即可完成安装。然后连接网络（有线可，无线默认wifi是PDCN,密码1234567890）进入管理页面：http://192.168.123.1/，默认账号：admin，默认密码：admin。（刷机不恢复默认值）

我第二次刷 Padavan 的时候密码就不是默认值了，而是我上一次刷机存的密码，尽管我已经把引导反复刷了好几次

[7620老毛子Padavan固件 恩山无线论坛](https://www.right.com.cn/forum/thread-161324-1-1.html) [Padavan固件下载](http://opt.cn2qq.com/padavan/)

[PandoraBox下载地址，含pb-boot](http://downloads.pangubox.com:6380/)

[中国科学技术大学测速网站](https://test.ustc.edu.cn/)
> MI R3P Padavan 3.4.3.9-099_22-10-1 2022-10-09 16:12:29    下载51.63 Mbps	上传56.23 Mbps

<details>
<summary>内网测试</summary>

一台笔记本(RTL8822CE)连接路由器 5G wifi, 作为 iperf 的服务端。一台台式机(RTL8111/8168/8411)通过一米超五类网线连接路由器，双方防火墙测试时关闭。ip 信息已经剔除。可以 ping 通，服务端开启 web 服务端，客户端可以访问。TCP 双向可以有 220～500Mbps，UDP 只有 1Mbps。看来路由器默认封杀 UDP，但是封杀的话不就是没有速度了嘛

```bash
$ ping ip
PING ip (ip) 56(84) 字节的数据。
64 字节，来自 ip: icmp_seq=1 ttl=64 时间=78.2 毫秒
64 字节，来自 ip: icmp_seq=2 ttl=64 时间=101 毫秒
64 字节，来自 ip: icmp_seq=3 ttl=64 时间=21.8 毫秒
64 字节，来自 ip: icmp_seq=4 ttl=64 时间=43.2 毫秒
64 字节，来自 ip: icmp_seq=5 ttl=64 时间=14.5 毫秒
64 字节，来自 ip: icmp_seq=6 ttl=64 时间=88.0 毫秒
64 字节，来自 ip: icmp_seq=7 ttl=64 时间=107 毫秒
64 字节，来自 ip: icmp_seq=8 ttl=64 时间=30.2 毫秒
64 字节，来自 ip: icmp_seq=9 ttl=64 时间=9.19 毫秒
64 字节，来自 ip: icmp_seq=10 ttl=64 时间=74.8 毫秒
64 字节，来自 ip: icmp_seq=11 ttl=64 时间=97.4 毫秒
^C
--- ip ping 统计 ---
已发送 11 个包， 已接收 11 个包, 0% packet loss, time 10016ms
rtt min/avg/max/mdev = 9.185/60.511/107.441/35.596 ms

$ curl ip:8000
.....
  <!-- For Internet -->
  <!-- <script src="//cdn.jsdelivr.net/npm/docsify@4"></script>
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/search.min.js"></script>
  <script src="//cdn.jsdelivr.net/npm/docsify-copy-code/dist/docsify-copy-code.min.js"></script> -->
</body>

$ wget ip:8000
--2022-10-14 21:39:22--  http://ip:8000/
正在连接 ip:8000... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：1886 (1.8K) [text/html]
正在保存至: “index.html.1”

index.html.1                                    100%[=====================================================================================================>]   1.84K  --.-KB/s  用时 0s      

2022-10-14 21:39:22 (76.3 MB/s) - 已保存 “index.html.1” [1886/1886])

$ iperf -c ip
------------------------------------------------------------
Client connecting to ip, TCP port 5001
TCP window size: 16.0 KByte (default)
------------------------------------------------------------
[  1] local ip2 port 41804 connected with ip port 5001 (icwnd/mss/irtt=14/1448/55286)
[ ID] Interval       Transfer     Bandwidth
[  1] 0.0000-10.0532 sec   666 MBytes   556 Mbits/sec

$ iperf -c ip -d
------------------------------------------------------------
Server listening on TCP port 5001
TCP window size:  128 KByte (default)
------------------------------------------------------------
------------------------------------------------------------
Client connecting to ip, TCP port 5001
TCP window size: 16.0 KByte (default)
------------------------------------------------------------
[  1] local ip2 port 48748 connected with ip port 5001 (icwnd/mss/irtt=14/1448/3630)
[  2] local ip2 port 5001 connected with ip port 56066
[ ID] Interval       Transfer     Bandwidth
[  1] 0.0000-10.0614 sec   422 MBytes   351 Mbits/sec
[  2] 0.0000-10.0662 sec   268 MBytes   223 Mbits/sec

$ iperf -c ip -u
-----------------------------------------------------------
Client connecting to ip, UDP port 5001
Sending 1470 byte datagrams, IPG target: 11215.21 us (kalman adjust)
UDP buffer size:  208 KByte (default)
------------------------------------------------------------
[  1] local ip2 port 49869 connected with ip port 5001
[ ID] Interval       Transfer     Bandwidth
[  1] 0.0000-10.0154 sec  1.25 MBytes  1.05 Mbits/sec
[  1] Sent 896 datagrams
[  1] Server Report:
[ ID] Interval       Transfer     Bandwidth        Jitter   Lost/Total Datagrams
[  1] 0.0000-9.9964 sec  1.25 MBytes  1.05 Mbits/sec   0.585 ms 0/895 (0%)

$ iperf -c ip -u -d
------------------------------------------------------------
Server listening on UDP port 5001
UDP buffer size:  208 KByte (default)
------------------------------------------------------------
------------------------------------------------------------
Client connecting to ip, UDP port 5001
Sending 1470 byte datagrams, IPG target: 11215.21 us (kalman adjust)
UDP buffer size:  208 KByte (default)
------------------------------------------------------------
[  1] local ip2 port 42857 connected with ip port 5001
[  2] local ip2 port 5001 connected with ip port 53089
[ ID] Interval       Transfer     Bandwidth
[  1] 0.0000-10.0155 sec  1.25 MBytes  1.05 Mbits/sec
[  1] Sent 896 datagrams
[ ID] Interval       Transfer     Bandwidth        Jitter   Lost/Total Datagrams
[  2] 0.0000-10.0155 sec  1.25 MBytes  1.05 Mbits/sec   0.103 ms 0/894 (0%)
[  1] Server Report:
[ ID] Interval       Transfer     Bandwidth        Jitter   Lost/Total Datagrams
[  1] 0.0000-9.9944 sec  1.25 MBytes  1.05 Mbits/sec   0.460 ms 0/895 (0%)
```
</details>

### 搞事情
#### ssh

高级设置 - 系统管理 - 服务 - 启用 ssh 服务。可以选择密码或者公钥来进行登陆认证

#### FTP

能够管理用户权限（读/写/禁止），测试了下 sata2usb3.0 通过 5G wifi 传输速度是 40M，还行，比我 bt 下载快多了。

#### ipv6

我是网线连接上级路由和小米路由，本想着是把 v6 下方，这样子连接 r3p 的设备也有 v6 地址，按照贴吧的进行设置后发现只能路由器有 v6,没法下播，要么就是 r3p 也没有 v6。最后就让 r3p 自己有 v6 就行，能挂 bt 也算达成目的了

在 高级设置-系统管理-服务-其它服务中，可以选择开启natp66来进行v6转发

- [老毛子PADAVAN固件IPV6设置&上海联通获取PD前缀让局域网设备获取全球唯一IPV6地址 admin 10月 12, 2021](https://www.huntersong.com/index.php/2021/10/12/156/):pppoe-nd-01与pppoe-pd-01模式.NAPT66
- [[AC2100(RM2100)] 开启IPv6及老毛子padavan开启IPV6设置 xcl52130 2020-11-17](https://www.right.com.cn/FORUM/thread-4059321-1-1.html)
- [h大老毛子ipv6的wan口地址获取不到 p86391753 2019-2-28 ](https://www.right.com.cn/forum/thread-473835-1-1.html)

#### bt 下载

transmission：首先在硬盘新建一个文件夹 transmission，然后到 “USB 应用程序 - 其他设置 - 磁力链接 Transmission”进行开启，然后控制面板在9091端口

```bash
# transmission.sh restart
Stopping Transmission:..[  OK  ]
Starting Transmission:.sed: can't move '/mnt/transmission/config/settings.jsonPJwZCh' to '/mnt/transmission/config/settings.json': Permission denied
sed: /mnt/transmission/config/settings.json: Input/output error
[FAILED]

权限不足，那就手动重命名，发现两个都是空文件夹，直接删除
# ls
ls: ./settings.json.tmp.FR7eFB: Input/output error
app                  etc                  settings.json28vRZi  stats.json
bin                  o_p_t.img            settings.jsontKY8FC
# rm settings.json28vRZi/ -r
# rm settings.jsontKY8FC/ -r

# transmission.sh restart
Starting Transmission:.mv: can't stat '/mnt/transmission/config/settings.json': Input/output error
chown: /mnt/transmission/config/settings.json.tmp.FR7eFB: Input/output error
sed: /mnt/transmission/config/settings.json: Input/output error
sed: /mnt/transmission/config/settings.json: Input/output error
[FAILED]
```

硬盘没有问题啊，ntfs 2T 还能通过 FTP 拷贝影片呢

```bash
# df -h
Filesystem                Size      Used Available Use% Mounted on
/dev/sda1                 1.8T      1.6T    242.0G  87% /media/P300
/dev/sda1                 1.8T      1.6T    242.0G  87% /opt
# cd /
[MI-R3P-breed /]# cd /mnt/transmission
[MI-R3P-breed /media/P300/transmission]# pwd
/mnt/transmission
```

已在 usb 应用中关闭 ftp、transmission，网络地图中的 usb 硬盘取消挂载好几次都没有效果，一会变成未挂载，但是硬盘还在转（正常情况下硬盘取消挂载状态是会停止转的）

```bash
# fsck
-sh: fsck: not found
# debugfs
-sh: debugfs: not found
# fuser -m /dev/sda1
1 421 423 561 563 649 654 671 678 684 686 706 4077 10462 10531 10638 16724 16793 18639 18751 20559 20592 27614
# fuser -km /dev/sda1
Connection to 192.168.123.1 closed by remote host.
Connection to 192.168.123.1 closed.
```

emm ssh 断开了，管理控制界面也挂掉了。。看来是这个 k 参数删除了太多重要进程，重启路由器吧

```bash
# lsof -n | grep delete 
# transmission.sh start -h
Starting Transmission:./media/P300/
mkdir: can't create directory '/mnt/transmission/downloads': File exists
sed: can't move '/mnt/transmission/config/settings.jsonROHY0O' to '/mnt/transmission/config/settings.json': Permission denied
sed: /mnt/transmission/config/settings.json: Input/output error
Couldn't (re)open log file "/mnt/transmission/transmission.log": Is a directory
[FAILED]
# ls /mnt/transmission/config
ls: /mnt/transmission/config/settings.json.tmp.FR7eFB: Input/output error
app         bin         etc         o_p_t.img   stats.json
# lsof -n | grep delete 
# ls /mnt/transmission -ag
lrwxrwxrwx    1 root            25 Oct  9 15:58 /mnt/transmission -> /media/P300//transmission
# rmdir /mnt/transmission/transmission.log
rmdir: '/mnt/transmission/transmission.log': Not a directory
# rm /mnt/transmission/transmission.log -r
rm: can't stat '/mnt/transmission/transmission.log': Input/output error
# transmission.sh stop
```

看样子硬盘多少出了点问题，等新硬盘到手把这个盘里的资料挪走先再格式化修复成 ext4,现在还是ntfs

用U盘试验一下，第一次下载目录自动识别到 u 盘，然后下载错误，啥提示也没有，第二次下载保存目录直接出bug,显示undefined。

```bash
# cat transmission/transmission.log
Unable to create session lock file (89): Function not implemented (session-id.c:96)
```

#### sambda

- [老毛子/潘多拉/Padavan路由固件设置smb/samba indEmpire 2019.08.26](https://www.jianshu.com/p/f1e928ea826d)：无法进行分组权限管理

#### opkg

```bash
# opkg.sh
ERROR! Directory "/opt" not mounted!
# mount /media/___е_U__/opt /opt
# opkg.sh
# opkg install iperf
Package iperf (2.1.3-1) installed in root is up to date.
Configuring entware-opt.
ln: /opt/sbin/ifconfig: Operation not permitted
ln: /opt/sbin/route: Operation not permitted
ln: /opt/bin/netstat: Operation not permitted
ln: /opt/bin/sh: Operation not permitted
ln: /opt/bin/ash: Operation not permitted
Collected errors:
 * pkg_run_script: package "entware-opt" postinst script returned status 1.
 * opkg_configure: entware-opt.postinst returned 1.
# ls -g /opt
drwxrwxrwx    2 root          4096 Oct  9 20:31 bin
drwxrwxrwx    4 root          4096 Oct  9 20:27 etc
drwxrwxrwx    2 root          4096 Mar 19  2021 home
drwxrwxrwx    3 root          4096 Mar 19  2021 lib
drwxrwxrwx    2 root          4096 Oct  9 20:27 root
drwxrwxrwx    2 root          4096 Oct  9 20:28 sbin
drwxrwxrwx    4 root          4096 Mar 19  2021 share
drwxrwxrwx    2 root          4096 Oct  9 20:44 tmp
drwxrwxrwx    4 root          4096 Mar 19  2021 usr
drwxrwxrwx    6 root          4096 Mar 19  2021 var
# su
su: unknown user root
# whoami
admin
```

查了下 padavan 以及梅林的 faq 中也没有类似的问题，暂时找不到解决办法

# 刷回官方

到底是把 kernel0 刷入bootloader，还是刷入 Firmware_Stub

```bash
mtd_write unlock Bootloader
mtd_write write pb-boot-xiaomi3-20190317-61b6d33.img Bootloader
```

[ [小米其它型号路由器] 小米路由器3从第三方固件刷回官方固件 醉老仙 2020-5-8](https://www.right.com.cn/forum/thread-4023509-1-1.html)
> 当前为pandorabox、X-wrt、openwrt刷回官方，uboot为pb-oot  
    进入pb-boot刷官方固件包  
    完成后执行U盘刷机  
    下载官方固件，放在u盘根目录，命名为miwifi.bin
    路由器断电，插入U盘，插电，按住reset，等待指示橙色慢闪后松开

[小米路由器R3G刷回原厂固件  2022-05-24 奔跑的咸鱼](https://zhuanlan.zhihu.com/p/392456500):512KB 的bl。刷完原厂bl之后还有breed？原厂bl不会覆盖掉breed吗？实验结果表面到刷原厂固件的时候，breed 无法识别固件类型，没法更新固件。原厂 bootloader 和 eeprom 都可以更新。
> 进入breed控制台 -> 更新固件 -> 常规固件，勾选Bootloader选择下载好的小米原厂Bootloader，点击上传按钮  
> 进入breed控制台 -> 更新固件 -> 常规固件，选择之前下载好的小米原厂固件，点击上传按钮

看了不少阅读材料之后发现原厂的 kernel0 带了一个功能就是从 u 盘恢复，于是在 breed 刷了 padavan 后开ssh，想把之间备份的 kernel0 刷回去，刷着返回 `Could not open MTD device: kernel0` 才发现刷了 padavan 之后 mtd 分区表和原厂的不一样了。kernel0 的 erasesize 和 原厂kernel0 一样，但是 size 不一样，这下我就不敢继续刷了

```bash
# uname -a
Linux MI-R3P-breed 3.4.113 #2 SMP Mon Oct 3 14:07:10 CST 2022 mips GNU/Linux

# mtd_write unlock kernel0
# mtd_write write kernel0ow.bin kernel0
Could not open MTD device: kernel0

# cat /proc/mtd
dev:    size   erasesize  name
mtd0: 00040000 00020000 "Bootloader"
mtd1: 00080000 00020000 "CFG_Bdata"
mtd2: 00040000 00020000 "Factory"
mtd3: 00500000 00020000 "Reserved"
mtd4: 00180000 00020000 "Kernel"
mtd5: 01680000 00020000 "RootFS"
mtd6: 00080000 00020000 "Config"
mtd7: 00400000 00020000 "Storage"
mtd8: 0dd00000 00020000 "RWFS"
mtd9: 01800000 00020000 "Firmware_Stub"
mtd10: 0ff80000 00020000 "ALL"
```

按住reset断电重启发现没出现 breed，好几次都不行，还好 padavan 还在，就通过 ssh 再次刷入 breed 。这下子就又可以进入 breed 了。

```bash
# mtd_write unlock Bootloader
Unlocking 'Bootloader' ...

# mtd_write write breed-mt7621-xiaomi-r3g-r3p.bin Bootloader
Unlocking 'Bootloader' ...
Writing from 'breed-mt7621-xiaomi-r3g-r3p.bin' to MTD 'Bootloader' ...  [ok]
```

# Q&A
1. 刷引导时在刷哪一个分区

Bootloader。无论是刷 breed 还是 pb-boot，刷的都是 Bootloader 分区

2. eeprom 对应 mtd 中哪一个分区

[请教EEPROM是哪个分区 李丑男 2017-12-13](https://www.right.com.cn/FORUM/thread-264540-1-1.html)
> di0506 2017-12-13 Factory分区就是EEPROM

但是我把从 breed 备份出来的 eeprom 和我用 dd 备份出来的 Factory 进行 md5 校验，两者不一样。

3. 双清是什么意思？

[ [PRO(R3P)] 小米路由器Pro 超频1200Mhz Padavan固件 酸奶+多拨+流控  2022-3-9 无量天尊](https://www.right.com.cn/forum/thread-8193640-1-25.html)：这样可以重置 padavan 的默认密码。因此在 padavan的基础上刷机，还是双清吧。
> 刷完重启后记得双清
> 系统管理-配置管理，路由器设置-恢复出厂模式，路由器内部存储-恢复出厂模式

# 相关阅读

[ 小米R3P刷入魔改版Breed并刷入Lean最新源码固件教程 wifi之路 • 2020年12月9日](https://www.wifilu.com/262.html)：其外部链接解释了r3p的breed固件来源

[路由器篇：小白必备小米路由器PRO（R3P）刷机＋避坑教程（OpenWrt＆Pandora）2021/03/11](https://new.qq.com/rain/a/20210311A09A4300)

[2019-09-13 K3c如何备份MTD分区](http://www.wujunjie.net/index.php/2019/09/13/k3c%E5%A6%82%E4%BD%95%E5%A4%87%E4%BB%BDmtd%E5%88%86%E5%8C%BA)
> 刷前必须备份mtd0-16除data_vol,不备份的我也救不了你 

[一台路由器引发的血案    2022-06-24](https://nevercode.cn/post/46.html):小米路由器PRO 型号R3P 不刷breed后升级用 ttl 救砖记录，起初在酷安看到的，文章日期是 6.28 by 腕豪，但是搜索引擎进不去酷安，就找了个bing能搜到的

[ [Redmi AX6] [0825:AC2100新固件有效AX6无效]AX3600/AX1800/AX5/AC2100官方固件开启SSH方法[原创]     LonGDikE 2020-5-26](https://www.right.com.cn/forum/thread-4032490-1-1.html)：小米路由器Pro 2.16.29 测试返回的是 No page is registered，此路不通

	昨天终于找到漏洞，web注入方式，不过该漏洞在最新版本应该已经堵上了 
有漏洞固件，AX3600 1.0.17版本/AX1800 1.0.34/1.0.328/1.0.336版本/AX5 1.0.16/1.0.26/AC2100 2.0.722版本，AX1800 1.0.34，1.0.328, 1.0.336仍然有效, 红米AX5 1.0.16版本
	管理密码登录管理页面: http://192.168.31.1/cgi-bin/luci/;stok=<STOK>/web/home#router  
	改nvram设置ssl_en=1的: http://192.168.31.1/cgi-bin/luci/;stok=<STOK>/api/misystem/set_config_iotdev?bssid=Xiaomi&user_id=longdike&ssid=-h%3Bnvram%20set%20ssh%5Fen%3D1%3B%20nvram%20commit%3B  
	一步到位代码: http://192.168.31.1/cgi-bin/luci/;stok=<STOK>/api/misystem/set_config_iotdev?bssid=Xiaomi&user_id=longdike&ssid=-h%3B%20nvram%20set%20ssh_en%3D1%3B%20nvram%20commit%3B%20sed%20-i%20's%2Fchannel%3D.*%2Fchannel%3D%5C%22debug%5C%22%2Fg'%20%2Fetc%2Finit.d%2Fdropbear%3B%20%2Fetc%2Finit.d%2Fdropbear%20start%3B  
	改root账号密码为admin: http://192.168.31.1/cgi-bin/luci/;stok=<STOK>/api/misystem/set_config_iotdev?bssid=Xiaomi&user_id=longdike&ssid=-h%3B%20echo%20-e%20'admin%5Cnadmin'%20%7C%20passwd%20root%3B

[【小米路由器SSH】小米路由器官方ROM强制打开SSH功能并配置静态路由（适用于AX1800、AC2100等型号）2022-01-23 404NotFound](https://zhuanlan.zhihu.com/p/460949138)：和上面恩山的一样

[ [PRO(R3P)] 最新R3P从OPENWRT刷回原厂 mingwei123 于 2021-9-7](https://www.right.com.cn/forum/thread-5614985-1-1.html)

	新方法：
	1、从小米下载开发固件，并将其重命名为miwifi.bin放到U盘(FAT/FAT32)根目录。
	2、打开WINSCP，上传kernel0.bin（原厂固件里备份出来的）到路由器/tmp目录下。
	   然后SSH连接到路由器输入
	   cd /tmp
	   mtd write kernel0.bin kernel
	3、拔掉电源将U盘插到路由器。
	4、重新插入电源，按住复位键等待黄灯闪烁再松开复位键,等待5分钟安装固件。

	kernel0.bin文件下载：
	链接：https://pan.baidu.com/s/1uJWBxTMTHGK1B4eKeTqj-A
	提取码：uu6i

[在小米路由器mini上安装Transmission挂BT/PT 帥氣的胖紙鍋 2016-01-26](https://blog.csdn.net/xiaopang1122/article/details/50586858):opkg

[cygnus-x1 	08-15-2006 03:55 PM Cannot stat Input/Output error](https://www.linuxquestions.org/questions/linux-general-1/cannot-stat-input-output-error-474166-print/):debugfs

[ls 命令出现 Input/output error 解决思路 LowCoder 2020-03-11 ](https://blog.csdn.net/qq_37797316/article/details/104789367)

    ```bash
    解决办法思路：
    1.lsof、fuser命令查找出还在损坏磁盘进行读写的进程，全部杀掉
    2.umount -l 卸载磁盘
    3.xfs_repair修复磁盘
    ```

[Linux 强制卸载硬盘 (Device is busy)](https://www.cnblogs.com/flyingicedragon/p/15136383.html)
> fuser -m -k -i -v /mnt/hdd0

[lsof查看占用高_lsof解决磁盘占用过高，查询却无大文件处理一例！时常抠脚的隔壁老 2021-01-16](https://blog.csdn.net/weixin_33921444/article/details/112998678)
> “就是在Linux的文件系统中删除一个文件，系统并不会真的立刻把这个文件丢弃掉，而只是把它从文件的目录系统中移除， 只有确保所有使用这个文件的程序全部都退出后，才会真的把文件彻底删除掉。”

[linux磁盘或内存被占用问题——lsof -n | grep delete的使用 飞天小老头 2021-03-25](https://blog.csdn.net/AnameJL/article/details/115204592)
> lsof -n | grep delete  kill -9 pid

[ [lsof]lsof查看哪些设备/文件被占用或者打开](https://www.cnblogs.com/aaronLinux/p/8191510.html)

[hiboy-padavan使用opkg安装tmux教程 书三四 2021-10-07](https://zhuanlan.zhihu.com/p/418199351)
> 安装okpg: opkg.sh升级opkg: opkg update

[新路由3老毛子Padavan固件opkg命令-sh: opkg: Permission denied无法使用解决方法,Padavan固件opkg.sh无法使用](https://ssrvps.org/archives/5033)
> 能解决问题就是看到日志中一直在提示下载 opt.gz文件，但是分区/opt空间不足，一直报错

[ Unable to create session lock file: Invalid argument (session-id.c:96) #1391 9 Aug 2020](https://github.com/transmission/transmission/issues/1391)
> Fixed by #2629.

[ build: use -DHAVE_FLOCK in the macOS Xcode build #2629 15 Feb 2022](https://github.com/transmission/transmission/pull/2629):半年前解决的问题，现在还存在，可能是现在我用的版本太旧了

[ [PRO(R3P)] 小米PRO普通版，现在刷什么固件最稳定？黄先生的账号 2022-10-14 ](https://www.right.com.cn/forum/thread-8258252-1-1.html)
> [ [PRO(R3P)] 【2022-06-11】R3P 闭源无线+SDK硬件加速驱动+多拨 Openwrt，pb boot直刷   2022-6-11 237176253](https://www.right.com.cn/FORUM/thread-8238333-1-1.html)
>> 固件源码https://github.com/padavanonly/immortalwrt

[ [PRO(R3P)] 6月4日-小米路由器PRO(R3P)刷入PandoraBox 19.02，测试完美，稳定  2019-6-4 22:16  laomao9000](https://www.right.com.cn/forum/thread-701501-1-14.html)

[ [PRO(R3P)] R3P-自编译openwrt R22.5.5 -6.10 一条寂寞的鱼 	2022-6-10](https://www.right.com.cn/forum/thread-8238195-1-2.html)
> https://github.com/coolsnowwolf/lede

[ [PRO(R3P)] R3P支持PB-boot的基于4.4内核padavan来啦~ ](https://www.right.com.cn/forum/thread-8224616-1-8.html):8258252 提到 4.4 upnp有问题，打不开

[ [PRO(R3P)] 小米路由器pro r3p 最全最新openwrt固件及全套插件库 2021/7/3  openwrtr3p ](https://www.right.com.cn/forum/thread-4615447-1-12.html)
> https://github.com/coolsnowwolf/lede  
> 包含openwrt所有插件300M.无法识别USB 如U盘 硬盘 是不是没有安装 kmod-usb-storage   kmod-usb-storage-extras

[ [PRO(R3P)] 小米R3P 魔改Breed 多种内存频率 PandoraBox lyh802 2021-10-21 22:44](https://www.right.com.cn/forum/thread-6331347-1-12.html)

[pangubox r3p 的备份 2018](http://downloads.pangubox.com:6380/%E5%88%B7%E6%9C%BA%E8%AF%B4%E6%98%8E/%E5%B0%8F%E7%B1%B3%E8%B7%AF%E7%94%B1%E5%99%A8Pro/xiaomi-pro/)

[ [PRO(R3P)] 小米路由器R3P拆机_NAND改SPI(内含高清图慎入)   2018-11-13  zyguowei](https://www.right.com.cn/forum/thread-355307-1-19.html)

[ [PRO(R3P)] 【20220701：每周六更新】固件发布 R3P OP (小米路由 3 Pro)  2020-8-28 然后七年](https://www.right.com.cn/forum/thread-4049182-1-20.html)

[ 【教程】亲测小米路由器3成功从padavan刷回原厂固件   ago 2017-5-30 ](https://www.right.com.cn/forum/thread-216748-1-28.html)：原厂固件自带usb恢复功能
> [PROMETHEUS 俄罗斯作品](http://prometheus.freize.net/)：Xiaomi MI-MINI, MI-NANO, MI-3  ASUS...代码也不是很长
> 原理是修改数据强制 UBoot 进入恢复模式（小米官方固件自带的功能），然后插 U 盘就可以恢复了。