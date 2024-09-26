# BIOS 更新导致蓝屏且无响应 SYSTEM_SERVICE_EXCEPTION TdkLib64.sys
- date: 2023-3-24

# 问题详情

BIOS 由 F0EC38WW 升级到 F0EC39WW，下载插电安装，蓝屏。

- OS：win11 家庭版
- else：已关闭快速启动，UMA分给显卡4G内存

## 蓝屏信息

- Stop code：SYSTEM_SERVICE_EXCEPTION
- What failed：TdkLib64.sys

## 解决办法

蓝屏进度一直卡在 0%，长时间无响应后强制关机，开机后仍可以进入 bios、linux、windows，(进BIOS把UMA修改为默认的512M,性能模式由省电修改为高性能，这一步不是重点)。然后进入win11的设置-隐私和安全性-Windows安全暗中心-设备安全性-内核隔离-内核隔离详细信息-内存完整性，关闭内存完整性后重启，充满电且保持电源充着，运行BIOS更新程序，塔台允许起飞，更新完成。

然后更改 logo,开启内存完整性，重启。

我没有重启，而是关机进入BIOS关闭安全启动、修改UMA为4G、修改SSD速度策略（PSPP）等（更新BIOS后设置会被重置），发现linux引导消失了，后开机蓝屏代码：DPC_WATCHDOG_VIOLATION。收集进度到100%后重启发现可以进入系统。系统更新完成后重建 grub。

# 参考

[小新 Pro-13 2020(AMD平台：ARE版) 驱动列表](https://newsupport.lenovo.com.cn/driveList.html?fromsource=driveList&selname=%E5%B0%8F%E6%96%B0%20pro-13%202020(AMD%E5%B9%B3%E5%8F%B0%EF%BC%9AARE%E7%89%88))

[小新pro13 2019 amd 升级 bios时蓝屏.  2022-4-3](https://club.lenovo.com.cn/thread-7837116-1-1.html)
> 关闭安全中心里的“内存隔离”，重启。然后再更新BISO
