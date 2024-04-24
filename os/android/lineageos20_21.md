# 小米6 lineageos 20 升级到 21
- date: 2024-04-18

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

- [隐藏 Magisk 应用（随机包名隐藏）](https://magiskcn.com/hide-magisk-app)
