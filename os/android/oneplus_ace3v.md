# oneplus ace 3v root
- date: 2025-01-9

外观无明显异常，三码合一，查询未激活。

对于预装软件的数量大为震撼，重置手机后也还是有预装软件。打开开发者模式，关闭内存扩展。联网更新下系统（ColorOS 更新包几百M）。随后查询激活时间显示还是未激活。更新不到半小时，又检测到更新，这次是大更新 3.5GB。

ColorOS 15.0 PJF110_15.0.0.40(CN01) 6.1.75-android14

## 不适

1. 预装应用极多

自带的视频应用，说实话，和抖音没有什么区别。

下拉的搜索栏全都是垃圾推广，搜索引擎只能选百度或头条。搜索设置里的首页卡片管理，将所有的都关闭，包括热搜榜单，但是下拉后发现其它卡片关闭了，唯独热搜榜单还在，重启手机后这个设置才生效。但是搜索框里依旧在滚动热搜。。。在重置手机后，首页卡片管理只有一项可用，联网之后其它四项才出现关闭选项

软件商店一打开就是推荐安装几十个预装应用。查了下红米14C的几个测评，没发现讲预装应用的。朋友说K80也有很多预装应用。这个预装应用无法直接卸载，adb 也不行。

应用商店我选择 foxy-droid，使用 tuna 镜像，没有的去官网下载。

```shell
> adb shell dumpsys window | findstr mCurrentFocus
  mCurrentFocus=Window{e4338be u0 com.heytap.market/com.heytap.cdo.client.cards.page.main.maintab.MainTabActivity}
> adb shell pm uninstall --user 0 com.heytap.market
Failure [DELETE_FAILED_INTERNAL_ERROR]
```

在承重墙的评论区看到“精简系统清除系统Cust分区无用自带推广APP”

2. 输入法混乱

预装应用有搜狗输入法，但是打开一些应用或者在设置里进行输入的时候弹出的是百度输入法定制版的个人隐私政策，但是应用列表里并没有百度输入法，卸载搜狗输入法之后弹的也还是百度输入法。在广告跟踪列表（显示系统应用）里终于发现百度输入法定制版了。

应用列表打开显示系统应用才显示百度输入法。

默认的时间卡片的数字1是红色的，其它数字都是白色，感觉不舒服。

系统设置的输入法里没有原装安卓输入法，这里安装 fcitx5-android 后卸载百度

```shell
> adb shell getprop ro.product.cpu.abi  # 查看 cpu 架构
arm64-v8a
> adb shell pm list packages -s -e | findstr baidu  # 寻找百度
package:com.baidu.input_oppo
> adb shell pm uninstall --user 0 com.baidu.input_oppo  # 卸载百度输入法  
Success
```

3. 录入nfc门禁需要一加账号
4. 无法在离线状态更改手机名称
5. 安装软件会联网检测，需要手动断网来阻住检测

# root

计划流程：

1. 解锁 bl
2. 安装 KernelSU 管理器应用，查看是否支持
3. 备份 boot.img (KernelSU 建议备份)
4. 刷入 KernelSU

实际流程：
1. 解锁 bl
2. 安装 KernelSU 管理器应用，确认支持
3. 刷 twrp 备份 boot、init_boot 分区
4. 测载刷面具，完成 root
5. 分析备份文件的内核压缩格式

## 解锁 bl

下面出错是因为没有在开发者模式打开 OEM 解锁，打开之后就能正常解锁了。解锁会格式化数据！解锁前记得备份数据！！！

```shell
> adb reboot bootloader
> fastboot flashing unlock
FAILED (remote: 'Flashing Unlock is not allowed
')
```

解锁之后，数据被清空，系统还原到上次更新后的系统，预装应用也会重新安装上，这就离谱了。此时还没有 root 权限。

## 确认支持

[ 萌新向的root方案:KernelSU使用教程 2023年7月7日 下午 ](https://wzk0.github.io/ksu-for-beginner/)
> 下载安装包 安装之后, 打开会显示未安装, 这也说明了, 你的手机是可以root的

[KernelSU 下载地址](https://github.com/tiann/KernelSU/releases)

## 备份 boot.img

dd 访问 boot 需要 root权限

```shell
1|OP5CFBL1:/dev/block/by-name $ ls -l | grep boot
lrwxrwxrwx 1 root root 16 1970-01-01 16:55 boot_a -> /dev/block/sde11
lrwxrwxrwx 1 root root 16 1970-01-01 16:55 boot_b -> /dev/block/sde41
lrwxrwxrwx 1 root root 16 1970-01-01 16:55 init_boot_a -> /dev/block/sde29
lrwxrwxrwx 1 root root 16 1970-01-01 16:55 init_boot_b -> /dev/block/sde58
lrwxrwxrwx 1 root root 16 1970-01-01 16:55 vendor_boot_a -> /dev/block/sde22
lrwxrwxrwx 1 root root 16 1970-01-01 16:55 vendor_boot_b -> /dev/block/sde52
lrwxrwxrwx 1 root root 16 1970-01-01 16:55 vm-bootsys_a -> /dev/block/sde19
lrwxrwxrwx 1 root root 16 1970-01-01 16:55 vm-bootsys_b -> /dev/block/sde49

$ dd if=boot_a of=/sdcard/boot_a.img
dd: boot_a: Permission denied
0+0 records in
0+0 records out
0 bytes (0 B) copied, 0.001 s, 0 B/s
```

想用 twrp 来备份的话，在 twrp.me 上没有找到 ace 3v 的型号。到[代码库](https://github.com/TeamWin/android_device_emulator_twrp)一看发现九年没更新了，不更网页的代码库还偶尔有更新

```shell
adb reboot bootloader
fastboot flash recovery TWRP-14-OPAce3V-Color597-V1.1.img
fastboot reboot
adb reboot recovery
```

在 TWRP 备份了槽A的 init_boot 和 boot，也备份了槽B的 init_boot，随后在 TWRP 里重启系统，变黑砖了。过一会长按电源出现安卓logo，但不进入系统，连电脑的话会识别高通串口。按音量下和电源可以进入 fastboot，且由 fastboot 可以进入 recovery，但进入的是官方的 recover，并不是 twrp，从官方 rec 重启系统进入黑砖状态。于是再次进入 fastboot 后重刷 twrp，刷完重启黑砖，再进入 fastboot 启动 recovery，这下子进入 twrp 了，且进入备份的默认槽位是 B，黑砖之前我印象默认是 A 的，然后就切回槽位 A，信息显示无法挂载存储，再切B也是无法挂载存储，然后想着可能是 data 分区被锁了或清空了，于是再次备份，但显示错误信息“挂载/data失败”，无法备份。想着要不趁机刷一下第三方系统，结果[lineageos](https://wiki.lineageos.org/devices/#oneplus) 中没有一加 ace3v，也就合适的系统。最后操弄半天 twrp，重启进入 EDL，发现进入系统了，但是在文件里无法找到刚才的备份，文件里没有 `/data/media/TWRP/BACKUPS/`，有 Android/data、Android/media，就是没有 TWRP。遂又进入 recovery，是 twrp，这次发现 data 被挂载上了，于是再次备份，然后用 `adb pull /data/media/TWRP/BACKUPS/bafc3969 backup` 拷贝到电脑上，发现之前的3个备份也还在，备份出来的后缀都是 emmc.win。此时我并不确定这个就是 boot.img，然后我想都进入 twrp 了，直接测载面具完成 root 了（`adb sideload .\Magisk-v28.1.apk`），因为检测 boot.img 格式这个我不太懂且相关教程不明了。此时重启进入面具再安装一遍就显示超级用户为可用了。此时 adb shell 运行 su 时，手机就可以赋予 root 权限了，可以卸载自带的软件商店了。

## 刷入 KernelSU

[玩机的必备操作 —— 一加 13 解锁并获得 root 权限 Lufs's Avatar 2024-12-10](https://blog.isteed.cc/post/oneplus-13-root-guide/)：根据这个找对应版本
> 解锁 Bootloader
> KernelSU 一般选 boot.img.gz

[KernelSU 安装](https://kernelsu.org/zh_CN/guide/installation.html)
> 请检查您原有 boot.img 的内核压缩格式，您应该使用正确的格式，例如 lz4、gz；如果是用不正确的压缩格式，刷入 boot 后可能无法开机。  
> 您可以通过 magiskboot 来获取你原来 boot 的压缩格式

deepseek 告诉我的方法，它根据下面的输出说这个 kernel 文件是未压缩的。但其说法也有点和我的输出不符合，它说解包后会生成 kernel、ramdisk.cpio 等文件，但我解包 boot.emmc.win 只得到 kernel，解包 init_boot.emmc.win 只得到 ramdisk.cpio。如果 boot.emmc.win 就是 boot.img，和 Uotan 所描述的不符合。

```shell
>magiskboot.exe unpack boot.emmc.win
>wsl
$ file kernel
kernel: Linux kernel ARM64 boot executable Image, little-endian, 4K pages
kearney@xx-win:/mnt/d/Downloads/magiskboot$ xxd -l 16 kernel
00000000: 4d5a 40fa 38f1 6e14 0000 0000 0000 0000  MZ@.8.n.........
```

暂未鲁莽操作 TODO

# 参考

[真伪及激活日期查询](https://support.oppo.com/cn/check/)：输入的是 IMEI 1/2，不是 S/N。实测手机联网后不会自动激活，莫非我这个假的？
> 产品激活日期一般指产品首次联网使用时间

[一加 OEM 安卓相关开源代码](https://github.com/OnePlusOSS?q=sm7675&type=all&language=&sort=)：ace3v 的芯片是 sm7675。代码仓库版本比手机上落后一点

[谁家系统最臃肿？国产手机系统纯净度对比 机智猫 2024-04-16](https://news.qq.com/rain/a/20240416A05UJ800)：都有不少的预装

[fcitx5-android](https://github.com/fcitx5-android/fcitx5-android)：输入法，app 是本地，plugin 是插件

[OnePlus 12 安装 KernelSU 04-08, 2024](https://bbs.oneplus.com/thread/1569840074059677696)

[OnePlus Ace 3V & Nord 4, some basic guides](https://xdaforums.com/t/oneplus-ace-3v-nord-4-some-basic-guides.4679715/)：24年8月生产的有点问题，需要等待系统更新。color597 是一位在维护相关工具的佬
> 14.0.1.630 (A.41) can't enter bootloader  

[一加 Oneplus Nord 4 ( 一加 Ace 3V 海外版 )发布 ](https://post.smzdm.com/talk/p/aqqgqrw7/)：和上文对应上，是略微不同的款式

[大侠阿木](https://yun.daxiaamu.com/OnePlus_Roms/%E4%B8%80%E5%8A%A0OnePlus%20ACE%203V/)
> 海外有售OnePlus Nord 4，硬件配置和3V略有不同，目前确认并不通刷

[获取Root权限 Uotan](https://wiki.uotan.cn/index.php?title=%E8%8E%B7%E5%8F%96Root%E6%9D%83%E9%99%90)
> 获取boot镜像的一般方式是从刷机包中提取  
> 注:如果设备的内核版本为5.15.XX – 6.1.XX，且内核版本中含有"android13"或更高android版本的字符串，请提取init_boot.img而不是boot.img，此时下文的"boot镜像"指的是init_boot.img;否则提取boot.img即可 

[TWRP Recovery For #一加Ace3V# V1.1](https://www.coolapk.com/feed/56433866?shareKey=ODcwZmUzMGQ2ODk4NjZhMTBjMzg~&shareUid=642425&shareFrom=com.coolapk.market_14.2.5)

[晨钟网络科技](https://jamcz.com/)：高质量教程和工具

[ 删除 ColorOS 预装 APP 2022-12-31 ](https://homulilly.com/post/delete-app-from-coloros.html)

[死妈OPPO系统内置软件卸载指南 CrazyChen 2021年8月10日](https://blog.sunflyer.cn/archives/689)
> dumpsys window | grep mCurrentFocus

[MobileModels](https://github.com/KHwang9883/MobileModels/issues/93)
> 高通没有要求过 TEE 熔断，都是 OEM 自己做的
