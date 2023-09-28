# adb 安卓调试工具
- date: 2022-08-29
- lastmod: 2023-09-28

卸载无法在手机上卸载的软件、冻结软件、强制安装旧版应用

## 手机开启USB调试

- [免 root 玩转 Android 设备：如何从零开始使用 adb 克莱德 2019 年 11 月 14 日](https://sspai.com/post/57427)
> 要在 Android 设备上执行 adb 指令，在此之前我们还需要在手机的系统设置中启用「USB 调试」功能。USB 调试功能位于「开发者选项」当中，在搭载 Android 4.2 及更高版本的设备上，开发者选项默认隐藏，要将其显示出来，我们需要在「设置 > 关于手机」中找到并连续点击七次版本号。

然后进入开发者选项打开USB调试功能，用数据线把手机和电脑连接起来，若手机有弹窗则选择调试/测试模式

手机上可选USB模式为充电、传输文件（MTP）、传输照片（PTP），需要注意选择 MTP,不然下面使用 adb 查看设备时可能会出现 no permisson

## 在PC上安装ADB

- [SDK Platform Tools 版本说明](https://developer.android.google.cn/studio/releases/platform-tools?hl=zh-cn):adb+fastboot 下载

- adb devices : 查看设备列表
- adb shell getprop ro.product.cpu.abi : 查看cpu架构，有64就是64位，没有就是32位
- adb shell dumpsys activity | grep mFocusedActivity : 查看当前正在运行应用的包名
- adb shell pm disable-user 包名 : 停用软件包
- adb shell pm enable 包名  : 启用软件包
- adb shell pm list packages -s : 列出系统所有软件包名
- adb shell pm list packages -s -e : 列出系统所有已启用的软件包名
- adb shell pm list packages -s -d :  列出系统所有已停用的软件包名

系统自带的软件包优先停用而不是卸载，有可能因为停用而引发其它暂未发现的问题，这个时候可以重新启用确定是哪个出问题了，我有回是安装软件发现桌面没有出现图标而打算再次安装时显示已安装，考虑到刚冻结了一些系统包，然后发现是returnWidget包被我冻结了。

# 参考

- [Android 设备刷机通用指南 青雪唐元 2019 年 05 月 17 日](https://sspai.com/post/54763)
- [我的内存去哪了？安卓为何一定要大内存，而苹果不用，教你优化大招！2020-11-11 epcdiy](https://www.bilibili.com/video/BV19T4y1F7EN):pm disable-user pkgname
- [用了此方法，2G内存安卓手机也能快得起飞 2020-11-19 epcdiy](https://www.bilibili.com/video/BV115411V7v1):到googlePlay下载旧版app
- [[Windows] 【搞机助手】可能是最好用的安卓手机助手！（更新至4.25版本）2019-7-24 helaer](https://www.52pojie.cn/thread-996129-1-1.html)

- [gnirehtet](https://github.com/Genymobile/gnirehtet):计算机通过 adb 向手机提供网络连接。安装好之后运行 `gnirehtet run` 就会自动在手机上安装软件，然后共享网络，如果报错 `Permission Denial: starting Intent requires android.permission.WRITE_SECURE_SETTINGS`，一种解决办法打开在开发者选项中的“禁止权限监控（disable permission monitoring）”，小米6开发板没找到这个选项。在 Coolpad A8-32 安卓 5.1.1 上测试成功
