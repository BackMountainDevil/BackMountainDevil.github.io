# 面具 magisk
- date: 2025-01-11

# 安装

[2023 Android 最新 magisk、root、xposed、lsposed 教程 wilinz 2023-01-11](https://juejin.cn/post/7187392961230372921)
> 刷 Magisk 有两种方式:  
>    Twrp 方式  
>    修改 boot.img 方式

[Magisk 下载地址](https://github.com/topjohnwu/Magisk/releases)：无论哪种方式，都要下载最新版的 apk

进入 twrp，在高级中启动 sideload

```
adb sideload .\Magisk-v28.1.apk
```

如果上面运行出问题，可以尝试把面具apk后缀改成zip，然后运行 `adb sideload .\Magisk.v28.1.zip`，安装完进系统后打开面具重新安装一下即可。

# 隐藏 root

面具内就自带随机包名功能，但米家能检测出已 root，说明这一招不是很厉害。剩下就要靠一些模块了。

面具28.1 启用遵守排除列表，在配置排除列表里选上想排除的应用，测试可以过米家、12306了，之前26版测试不太行。

## LSPosed

[LSPosed 下载地址](https://github.com/LSPosed/LSPosed/releases)：停更了，停在 1.9.2，推荐下载下面的

[升级coloros15 Lsposed无法使用解决方法（安卓15适用）林北今年起大厝 2024年11月21日](https://www.bilibili.com/opus/1002064695381721109)
> LSPosed v1.10.1： https://github.com/JingMatrix/LSPosed/releases/tag/v1.10.1  
> ZygiskNext v1.2.4： https://github.com/Dr-TSNG/ZygiskNext/releases/tag/v1.2.4

在面具设置里关闭 Zygisk，复制压缩包到手机，在面具里安装模块 LSPosed、ZygiskNext，重启。在通知栏可以看到 LSPosed，点击可进入 LSPosed 管理器。概览页显示“已激活”则成功刷入。

[lsposed图文安装](https://lsposed.cn/229)
> 重启设备，通知栏 点开（如果没显示，可以通过拨号键输入 *#*#5776733#*#* 进入LSPosed）

下面则是一些有意思的插件，LSPosed 管理器的仓库页面也有相同的，不过最终还是要去 GitHub 下载。

[Xposed Module Repository](https://modules.lsposed.org/)

[LSPosed模块](https://lsposed.cn/lsposed%e6%a8%a1%e5%9d%97)

## Zygisk版Magisk+Shamiko

[Shamiko 下载地址](https://github.com/LSPosed/LSPosed.github.io/releases)

1. 没有启用 ZygiskNext 模块的话，则需要打开 Magisk – 设置 – 开启 Zygisk
2. 面具设置里关闭遵守排除列表
3. 安装模块 Shamiko，重启，此时默认黑名单模式

[隐藏root保姆级教程第(二)期之用“Shamiko”模块白名单模式隐藏root（转自酷安）小猴玩机 2023年07月12日](https://www.bilibili.com/opus/730861545410527240)
> 在黑名单模式下，你想对哪个应用隐藏root，就必须在排除列表勾选哪个应用

[Shamiko 白名单模式](https://magiskcn.com/shamiko-whitelist.html)
> 创建文件 /data/adb/shamiko/whitelist ，重启手机，显示 whitelist mode 就是切换成 白名单模式  
> 白名单 比 黑名单 有什么优势？  
> 全局隐藏，更彻底，也不用设置排除列表
> 所有 app 都检测不到 root，已授权 root 的 app 可以正常使用 root 权限。
> 需要授权 root，关闭 Shamiko 模块重启手机，授权完再启用 shamiko 模块重启手机即可。

奇怪的是，我已经启动了白名单，配置排除列表里把米家的所有子选项都勾选上了，但是米家还是会报已 root。关闭 LSPosed、Shamiko，重启后开启遵守排除列表，进入米家就不报 root

# 参考

[Magisk如何针对性隐藏Root避免被检测 疯人院的院长大人](https://zhuanlan.zhihu.com/p/570744839)
> Delta面具 https://github.com/HuskyDG/magisk-files/releases  
> Zygisk版Magisk+Shamiko 不是开源的   
> Zygisk版Magisk+Riru  
> 同无解，随机包名，hide隐藏没啥用，其他银行APP都好着，就这个工商app提示风险

[Shamiko 安装](https://magiskcn.com/shamiko-install.html)：图文并茂的安装过程
