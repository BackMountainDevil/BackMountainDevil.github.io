# 小米6刷lineageos root
- date: 2023-10-13
- lastmod: 2023-10-16

# lineageos20 感受

简洁。自带应用没有要多余信息；没有自动识别并复制验证码；自带相机录制时没有防50Hz闪烁

在小米6上目前测试功能都可以使用，比如 gps 定位、通话、通话录音、流量上网、nfc、红外、蓝牙、WiFi、WiFi“中继”（不开流量时可以开热点，连接WiFi时可以开热点）

从 残血se 切到小米6，通话录音有了，nfc有了。感谢小米所有人的付出。

## app

应用市场：google play/fdroid/apkpure

中文输入法：fcitx5

nfc：com.yuanwofei.cardemulator（需要root才能模拟

相机：Google相机（解决频闪

手机互联：kde connect

文件管理器：com.simplemobiletools.filemanager.pro（root后可查看根目录文件

浏览器：Fennec(火狐

## 使用体验

现在我才是手机的主人

电信卡，插上不能打电话，开启 VoLTE 高清才可以打电话，关了 VoLTE 打电话就显示无法连接到移动网络，但实际上开了移动网络（流量）就可以上网，这一点在原装 MIUI 上一样。我在 ios12.4 没见过 volte

nfc 需要自己主动唤醒，就是亮屏后才能模拟，没解锁息屏怼刷卡机开不了门的，记得MIUI是被动唤醒

夜间拍照效果和 se 一样拍不清楚，不影响我扫码

使用lineageos自带的系统更新更新后，gapp、magisk大概率会失效，目前失效一回，影响了需要 root 功能的 nfc，也影响了 google camera，需要重装 Magisk、gapp。

# 刷机
## 第一轮刷机的个人捣鼓过程

不建议照抄！！！

初始状态：刷了 TWRP(3.3.2b-0308 by wzsx150)，开发板系统(MIUI 11 9.12.12 Android 9)，没法切稳定版系统

结果状态： TWRP(3.7.0_9) lineageos

过程简要：线刷稳定版失败，线刷 twrp 新版失败，刷 twrp 新版成功。线刷 lineageos 成功

阅读 [个人安卓刷机经验（附小米6刷机指南）之一 2021-11-19 02:23 OldUncle251](https://zhuanlan.zhihu.com/p/435031254)及其他相关资料


### 解 bl 锁

开发者选项，打开oem解锁

小米官方解锁工具（我装了在最后一步没确认解锁，原机主解好了，开机可见 Unlocked）

关闭查找手机、退出小米账号、关闭密码

### 备份

装 [含adb、fastboot的工具](https://developer.android.google.cn/tools/releases/platform-tools?hl=zh-cn#downloads)

备份用户资料

备份其它分区

用 TWRP 压缩备份了所有分区，但是有一些分区没显示(如fsc，modemst1，modemst2)。然后用 adb shell 备份了分区，参考 [备份手机字库基带方法（使用命令备份）](https://miuiver.com/backup-phone-partition/)

```bash
mkdir /sdcard/ddBackups

ls -1 /dev/block/bootdevice/by-name | grep -ixvE "userdata|cache" | while IFS= read -r name; do echo "dd if=/dev/block/bootdevice/by-name/$name of=/sdcard/ddBackups/$name.img" >> /sdcard/ddBackups/ddBackup.sh; echo "fastboot flash $name $name.img" >> /sdcard/ddBackups/ddRestore.bat; done

sh /sdcard/ddBackups/ddBackup.sh
```

### 开发板刷稳定版（未遂）

没法从设置里把稳定版切换为开发版，线刷个稳定版

进入 fastboot 线刷模式，解压刷机包,线刷包后缀通常是

打开MiFlash，选择解压后的刷机包，加载设备，下面勾选全部删除（默认是全部删除并lock）。十多分钟了，命令还卡在第一条，虚假的进度条。关闭线刷软件，还可以开机

### 刷 rec（升级成功）

刷 recovery，其作用主要用于恢复出厂设置与OTA安装/升级。进入旧版twrp 清除（双清 Data Cache Dalvik)

下新版对应机型的 twrp，https://twrp.me/xiaomi/xiaomimi6.html

命令刷没进展，怀疑刷失败了。重启尝试用旧版刷新版，复制img文件，进入卡刷模式。安装-刷入Image镜像-选twrp-3.7.0_9-0-sagit.img-选Recovery分区，滑动后刷完重启，再进入卡刷模式就是新版twrp了。

```bash
adb reboot bootloader
fastboot flash recovery  twrp-3.7.0_9-0-sagit.img
fastboot reboot
```

### 刷 lineageos 成功

[lineageos Xiaomi Mi 6](https://wiki.lineageos.org/devices/sagit)

[小米手机刷入LineageOS 2023 2023 南城搞机实验室](https://www.bilibili.com/video/BV1gs4y1m75K)
> 安装官方提供的 recovery，从 recovery 清除数据，然后 adb sideload

参考[小米手机刷 LineageOS 系统教程](https://miuiver.com/how-to-flash-lineageos)之后，那么我可以用 TWRP 双清再 sideload。成功刷入，然后开启开发者模式，可以使用电信卡上网。

```
PS D:\Downloads> adb devices
List of devices attached
fe03d016        sideload

PS D:\Downloads> adb sideload D:\Downloads\lineage-20.0-20230921-nightly-sagit-signed.zip
Total xfer: 0.98x
```

此时电话、流量、wifi、红外正常，而 nfc 需要额外软件，模拟卡软件需要 root。没有中文输入法

下个应用商店，如 fdroid，国内的话wandoujia可以下旧版，酷安不推荐，比应用商店复杂

### google

[小米手机刷入Gapps 2023 南城搞机实验室](https://www.bilibili.com/video/BV1es4y1N7sU)：2:21有说几个版本的区别
> https://sourceforge.net/projects/nikgapps/files/Canary-Releases/NikGapps-T/31-Mar-2023/

```
PS D:\Downloads> adb sideload D:\Downloads\NikGapps-full-arm64-13-20230331-signed.zip
Total xfer: 2.70x
```

重启后桌面布局改变了，往上拉是全部的应用

### 输入法

goboard 不显示，从 https://apkbent.net 下载 gboard.apk.v.12.3.05.apk 安装后也不管用，然后在系统里停止删除这个应用，卸载更新发现系统带的版本是 12.4.05

```
PS C:\Users\kearney> adb shell pm list packages  | findstr input
package:com.android.inputdevices
package:com.android.inputmethod.latin.auto_generated_rro_product__
package:com.android.inputmethod.latin
```

从[酷安](https://www.coolapk.com/apk/com.osfans.trime)下载 rime 输入法，adb 安装重启。然后参考[把 RIME 装进 Android 手机](https://client.sspai.com/post/77499#!) 选择输入法，在输入-方案 选择明月拼音，不然没法输出中文，旧版输入法在新版安卓上不显示候选栏，从fdroid下载最新版可以。不过推荐安装fcitx5

### camera

要求安装了 google 服务、已 root

https://www.bilibili.com/video/BV1fN411c7bF
https://www.celsoazevedo.com/files/android/google-camera/developers/

### root Magisk

[小米手机安装 Magisk 获取 Root 权限指南 2023 南城搞机实验室](https://www.bilibili.com/video/BV13g4y137bq)：fastboot 刷的很流畅

[小米手机安装 Magisk 获取 Root 权限指南](https://miuiver.com/install-magisk-for-xiaomi/):操作和上面一样，文字版，

`adb install Magisk.v26.3.apk``

也可以使用 adb sideload Magisk-v26.3.zip 来安装 面具，修补 boot.img ,拷贝出magisk_patched-26300_MD8jt.img，adb reboot bootloader，fastboot devices 检测到手机，开刷，卡在这里（换绝对路径也卡住） `fastboot flash boot .\magisk_patched-26300_MD8jt.img`,然后使用 fastboot reboot recovery 重启，居然进了系统，然后  adb reboot recovery 进入 TWRP，想从里面刷 boot，但是里面看不到内部存储里的文件，这很奇怪。不管了，尝试 sideload，终端 adb sideload .\Magisk-26.3.zip 没啥异常，但是 twrp 显示红色失败，具体信息为“无效的 Zip 文件格式”，尝试五次（含两种路径方式、usb2/3）也是失败

此时进入 twrp 后发现无法读取内置存储了？难道是加入了谷歌框架就内置存储加密了？还是我刷 boot 把 twrp 整坏了？试一下重刷 twrp（fastboot flash recovery D:\Downloads\mi6\twrp-3.7.0_9-0-sagit.img），尴尬是这回连 twrp 半天也刷不进去。。。

## 第二轮刷机

重新来，备份格式化双清，刷lineageos的recovery（之前刷的twrp），刷magisk

### 备份

一些从play下的应用，没找到apk，用adb pull

```
PS C:\Users\kearney> adb shell pm list packages | findstr pay
package:com.eg.android.AlipayGphone
package:hk.alipay.wallet
PS C:\Users\kearney>  adb shell pm path com.eg.android.AlipayGphone
package:/data/app/~~tuVw_DxUwBLsBBSAseyGJQ==/com.eg.android.AlipayGphone-BHmrBsHzwdc-JbAGfADDZw==/base.apk
package:/data/app/~~tuVw_DxUwBLsBBSAseyGJQ==/com.eg.android.AlipayGphone-BHmrBsHzwdc-JbAGfADDZw==/split_config.arm64_v8a.apk
PS C:\Users\kearney> adb pull /data/app/~~tuVw_DxUwBLsBBSAseyGJQ==/com.eg.android.AlipayGphone-BHmrBsHzwdc-JbAGfADDZw==/base.apk
/data/app/~~tuVw_DxUwBLsBBSAseyGJQ==...27.2 MB/s (58841120 bytes in 2.063s)
PS C:\Users\kearney> adb pull /data/app/~~tuVw_DxUwBLsBBSAseyGJQ==/com.eg.android.AlipayGphone-BHmrBsHzwdc-JbAGfADDZw==/split_config.arm64_v8a.apk
/data/app/~~tuVw_DxUwBLsBBSAseyGJQ==...27.8 MB/s (74002021 bytes in 2.542s)
PS C:\Users\kearney> adb shell pm list packages | findstr tencent
package:com.tencent.tim
package:com.tencent.mm
package:com.tencent.soter.soterserver
PS C:\Users\kearney> adb shell pm path com.tencent.mm
package:/data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==/com.tencent.mm-7uTAInkFOtcg3XZCqOwUng==/base.apk
package:/data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==/com.tencent.mm-7uTAInkFOtcg3XZCqOwUng==/split_config.arm64_v8a.apk
package:/data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==/com.tencent.mm-7uTAInkFOtcg3XZCqOwUng==/split_config.xxhdpi.apk
package:/data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==/com.tencent.mm-7uTAInkFOtcg3XZCqOwUng==/split_config.zh.apk
PS C:\Users\kearney> adb pull /data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==/com.tencent.mm-7uTAInkFOtcg3XZCqOwUng==/base.apk
/data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==...27.4 MB/s (96451866 bytes in 3.359s)
PS C:\Users\kearney>  adb pull /data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==/com.tencent.mm-7uTAInkFOtcg3XZCqOwUng==/split_config.arm64_v8a.apk
/data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==...7.6 MB/s (171635175 bytes in 5.936s)
PS C:\Users\kearney> adb pull /data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==/com.tencent.mm-7uTAInkFOtcg3XZCqOwUng==/split_config.xxhdpi.apk
/data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==.... 20.9 MB/s (665030 bytes in 0.030s)
PS C:\Users\kearney> adb pull /data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==/com.tencent.mm-7uTAInkFOtcg3XZCqOwUng==/split_config.zh.apk
/data/app/~~C-Ku-ANA4xZ36KcFQl-2OA==... 21.7 MB/s (1937876 bytes in 0.085s)
PS C:\Users\kearney> adb backup mm.backup com.tencent.mm
WARNING: adb backup is deprecated and may be removed in a future release
Now unlock your device and confirm the backup operation...
PS C:\Users\kearney> adb backup mmbackup.ab com.tencent.mm
WARNING: adb backup is deprecated and may be removed in a future release
Now unlock your device and confirm the backup operation...
PS C:\Users\kearney> adb backup alipaybackup.ab com.eg.android.AlipayGphone

WARNING: adb backup is deprecated and may be removed in a future release
Now unlock your device and confirm the backup operation...
```

恢复一个应用有多个apk时可以用 adb install-multiple -r D:\Downloads\mi6\Alipaybase.apk .\Alipaysplit_config.arm64_v8a.apk

### 格式化+双清

TWRP 格式化 Data 分区，高级清除（Dalvik / ART Cache，System，Cache）

### sideload 系统

adb sideload D:\Downloads\mi6\lineage-20.0-20230928-nightly-sagit-signed.zip

### sideload Magisk

随后继续sideload刷面具（adb sideload D:\Downloads\Magisk-26.3.zip），显示红色无效的格式

```
PS C:\Users\kearney> adb install D:\Downloads\Magisk.v26.3.apk
PS C:\Users\kearney> adb reboot fastboot
fastboot flash boot D:\Downloads\mi6\magisk_patched-26300_j2gmW.img
```

卡在上一步刷修补的img，终端后查验 boot.img 和官网校验一致。进入 twrp 发现内置存储还是读不到，猜测被加密了。因此排除前面root失败猜测是因为谷歌框架的嫌疑

```
PS D:\Downloads\mi6> Get-FileHash .\boot.img
SHA256          97C456BB0DB47079BEF9CAAB6425E4936FEA5A1F43FB484F7637EB74DD7919C2

PS D:\Downloads\mi6> Get-FileHash .\magisk_patched-26300_j2gmW.img
SHA256          755FC7CB4470FF857421A57CADFC4BE73BE34DD4AE03F2713D7091CAE1725C50
```

再来 格式化+双清，进入 fastboot

```
PS D:\Downloads\mi6> fastboot devices
fe03d016         fastboot
PS D:\Downloads\mi6> fastboot flash recovery recovery.img
PS D:\Downloads\mi6> fastboot flash recovery D:\Downloads\mi6\recovery.img
```

？？？ 咋刷recover都刷不进去，中断尝试 boot

```
PS D:\Downloads\mi6> fastboot boot D:\Downloads\mi6\recovery.img
Sending 'boot.img' (25524 KB)                      FAILED (remote: 'Requested download size is more than max allowed
')
fastboot: error: Command failed
PS D:\Downloads\mi6> fastboot flash recovery D:\Downloads\mi6\sagit_images_9.8.22_20190822.0000.00_9.0_cn\images\recovery.img
```

直接刷官方宝里面包里面的rec也不行。官方线刷也是卡住的，确定 bl 锁解开了。twrp还在。在qq群寻求帮助后，把面具apk后缀改成zip，用sideload刷入获取到了root权限（`adb sideload .\Magisk.v26.3.zip`），然后进系统后打开面具重新安装面具即可

### google

full太多应用了，基本上我用不着，core就装了服务框架和商店

```
PS D:\Downloads\mi6> adb sideload D:\Downloads\mi6\NikGapps-core-arm64-13-20230331-signed.zip
Total xfer: 2.45x
```



## google camera

# 参考

[小米6官方刷机包汇总 2019-10-11 CONLYOUC](http://web-alpha.vip.miui.com/page/info/mio/mio/detail?postId=5894245)

[小米手机 MIUI 国际版/EU 刷机教程 2021-09-08 15:07 孤独的旅行者](https://zhuanlan.zhihu.com/p/408114647)

[手机怎么root啊，看了网上一些软件没用啊？Freed-wzy 2023-09-09](https://www.zhihu.com/question/449190010/answer/3204386804)：从 https://github.com/MiCode/Xiaomi_Kernel_OpenSource 发现 Mi 6 有三个分支，对于三个安卓版本N(7)、O(8)、P(9)，比如https://github.com/MiCode/Xiaomi_Kernel_OpenSource/tree/sagit-p-oss 是 Mi 6 安卓9的代码
> 首先：除了 XDA 论坛以外的绝大多数帖子都是东抄西抄没有价值。靠提权漏洞实现 root 的方案随着 Android 的不断完善漏洞不断减少已经不可能了。现在必须先解锁 bootloaer。正常的 Android 可以 sudo fastboot flashing unlock 解锁，但大多数 Android OEM 都会封锁 fastboot 模式导致不能直接通过此方法解锁。有些 OEM 提供官方申请的方法来解锁，比如 xiaomi Apply for permissions to unlock Mi devicesOPPO 2021 年以前的部分手机也可以 帖子详情 - OPPO社区联发科的 CPU 的手机可以通过 mtkclient 而不止 fastboot 来解锁 https://github.com/bkerler/mtkclientvivo 的一些手机可以通过 https://github.com/ChristophHaag/vivo_fastboot 解锁  How To Unlock Bootloader Of Vivo Phones ?解锁完了需要刷入 recovery.img 。twrp 官方提供的下载镜像往往落后最新的手机型号 2 年。（但 XDA 论坛上可以找到更新的，通常都是xiaomi，原因是解锁 bootloader 容易）Devices如果没有找到支持你手机型号的 twrp 就需要自己编译。根据 GNU/Linux 的 GPL ，所有 Android OEM 都必须 开源 内核源码和设备树代码，所以可以找到 OEM 在 github 的代码仓库。例如：https://github.com/MiCode/Xiaomi_Kernel_OpenSource oppo-source根据手机的CPU model name 找到对应的代码仓库的对应 手机型号的 git branch。也可以修改和和你手机相同型号的设备树代码。之后编译烧录即可。例如 这是我以前为我手机编译 recovery.img 的设备树代码 https://github.com/Freed-Wu/android_device_oppo_R15x烧完 TWRP 之后 就可以下载  https://github.com/topjohnwu/Magisk 获取 root 权限了。实际上大多数人的水平根本不足以走到这一步。所以最好的方法就是选择 一个有事前编译好的 TWRP 甚至是 Lineage OS ROM 的手机型号购买，因为有人事先编译好就代表有人已经成功了，可以直接跳过所有弯路

[MIUI 国际版/EU 安装小米钱包 傻瓜教程 2021-09-08 15:24 孤独的旅行者](https://zhuanlan.zhihu.com/p/264800660)

[备份手机字库基带方法（使用命令备份）](https://miuiver.com/backup-phone-partition/)

[备份手机 QCN 防范 IMEI 丢失无信号](https://miuiver.com/backup-phone-qcn)

[小米6 ROM仓库](https://mylovezcy.gitee.io/mylovezcy1/devices/sagit.html)
