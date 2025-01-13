# 小米6 lineageos 20 升级到 21
- date: 2024-04-18
- lastmod: 2025-01-13

[sagit wiki](https://wiki.lineageos.org/devices/sagit/)

> Warning: The Xiaomi Mi 6 is no longer maintained. A build guide is available for developers that would like to make private builds, or even restart official support.

wiki 还说到已知的问题有任何依赖于某种设备完整性检查的应用程序都不能在 LineageOS 上工作，需要额外操作，这个问题在该系统上都存在。

下载 [Latest build](https://download.lineageos.org/devices/sagit/builds) 的三个文件并哈希校验完整性。

windwos 上获取文件哈希的指令如下，将结果和下载页面对比一致：

    get-filehash boot.img -Algorithm SHA256 | Format-List
    get-filehash recovery.img -Algorithm SHA256 | Format-List
    get-filehash lineage-21.0-20240315-nightly-sagit-signed.zip -Algorithm SHA256 | Format-List

备份数据！备份数据！备份数据！尽管说明大版本更新系统不会清楚数据，不过并不保证不会出意外，因此请提前备份重要数据

```bash
# 在 TWRP 进入 sidelod 模式
$ adb devices
List of devices attached
fe03d016        recovery
$ adb sideload lineage-21.0-20240315-nightly-sagit-signed.zip
Total xfer: 0.99x
$ mv Magisk-Stable-27000.apk Magisk-Stable-27000.zip
# 在 TWRP 再次进入 sidelod 模式
$ adb devices
List of devices attached
fe03d016        recovery
$ adb sideload Magisk-Stable-27000.zip 
Total xfer: 3.99x 
```

重启进入 Lineageos 21 Android 14，进入 Magisk 重新安装即可完全 root。此外，按下指纹解锁这个需要重新开启，nfc 需要重新模拟，还有不少应用打开就会提示“此应用专门为旧版安卓打造”，目前还没有发现啥问题。除了 FakeLocation 模拟定位会造成重启，且模拟失败，应该是触发 SELinux 了。

# 隐藏 root

一些软件会检测 root 之后会不让使用或者有些异常现象，如 12306 的人脸识别验证失败。

- [隐藏 Magisk 应用（随机包名隐藏）](https://magiskcn.com/hide-magisk-app)

后续是装了新版面具之后隐藏列表管用了。

# LSPosed

参考 [LSPosed 安装教程](https://magiskcn.com/lsposed-install)

[LSPosed wiki 如何使用 Apr 23, 2023](https://github.com/LSPosed/LSPosed/wiki/%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8)

1. 打开 Magisk – 设置 – 开启 Zygisk
2. 打开 Magisk – 模块 – 从本地安装，选从 [github](https://github.com/LSPosed/LSPosed/releases) 下载的 LSPosed-zygisk.zip
3. 重启手机，下拉通知栏，点开 LSPosed（如果没显示，可以通过拨号键输入 *#*#5776733#*#* 进入LSPosed）。我在这一步打开就卡死了，尝试几次还是卡死，版本 LSPosed-v1.9.2-7024-zygisk-release.zip，Magisk 27.0，LineageOS 21
