# 小新pro13 ARE2020 windows 11 21H2 手动升级到 23H2 BIOS升级绿屏与内存完整性
- date: 2024-9-26

买到手是 win10，后来 win11 推送符合条件就更新了下。之后的系统更新也是推送啥会尽快更新啥。可选更新则基本不会装。

为什么发现这个 win11 版本落伍了呢？最早是 wsl2 兼容问题，不以为意，后来是一个漏洞的帖子，按理说 windows11 会尽快推送这个漏洞的修复，过了半个月也没有推送，我按照系统版本自己去下载修复包，安装报错说系统不兼容。最后在手册发现是 21H2 停止维护了...22H2 10月份也将停止安全维护了，建议更新一波

## 设备信息

- 硬件信息：[小新 Pro-13 2020(AMD平台：ARE版)](https://newsupport.lenovo.com.cn/driveList.html?fromsource=driveList&selname=%E5%B0%8F%E6%96%B0%20Pro-13%202020(AMD%E5%B9%B3%E5%8F%B0%EF%BC%9AARE%E7%89%88))

- Windows 系统信息：Windows 11 Home 21H2 22000.2538

## 方法
### 自带更新

设置里的检查更新尝试好多回了，不同时间段/日期的都尝试过，重启也尝试过，并没有新的更新推送。14个可选更新都是驱动相关的，不过联想驱动官网上的并没有啥新变化，最近几次的 BIOS 更新都是漏洞补丁。

排除了网络问题，尝试了三个不同市的 WIFI。硬件设备我也没有进行更换，更换硅脂不影响的。

网上有说可能是因为硬件兼容性问题导致联想没有推更新。

### 加入Windows 预览体验计划

有几个不同的体验版。Canary 面向硬核技术玩家；Dev 面向热心体验玩家，没那么稳定；Beta 则是稍微稳定一些的版本；Release Preview 则是正式发布前一版。

我选择 Beta 版本，都确认好之后进行重启。然后再次进行检查更新，咋还没有？刚才加入的体验计划感觉回到了没加入的阶段

发现红色提示我没有开启发送诊断数据，然后遇到了[每次打开后自动关](https://answers.microsoft.com/zh-hans/windows/forum/all/%E6%88%91%E6%83%B3%E6%89%93%E5%BC%80%E5%8F%91/6f98d725-8429-49f0-99f4-48f0fb28e79a)的问题，我没有安装其它管家、杀毒软件，因此按照回答进行操作：

```powershell
PS C:\Users\kearney> sfc /SCANNOW

Beginning system scan.  This process will take some time.

Beginning verification phase of system scan.
Verification 100% complete.

Windows Resource Protection found corrupt files but was unable to fix some of them.
For online repairs, details are included in the CBS log file located at
windir\Logs\CBS\CBS.log. For example C:\Windows\Logs\CBS\CBS.log. For offline
repairs, details are included in the log file provided by the /OFFLOGFILE flag.
PS C:\Users\kearney> Dism /Online /Cleanup-Image /ScanHealth

Deployment Image Servicing and Management tool
Version: 10.0.22000.653

Image Version: 10.0.22000.2538

[==========================100.0%==========================] The component store is repairable.
The operation completed successfully.
PS C:\Users\kearney> Dism /Online /Cleanup-Image /CheckHealth

Deployment Image Servicing and Management tool
Version: 10.0.22000.653

Image Version: 10.0.22000.2538

The component store is repairable.
The operation completed successfully.
PS C:\Users\kearney> DISM /Online /Cleanup-image /RestoreHealth

Deployment Image Servicing and Management tool
Version: 10.0.22000.653

Image Version: 10.0.22000.2538

[==========================100.0%==========================] The restore operation completed successfully.
The operation completed successfully.
PS C:\Users\kearney> $path = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection"
PS C:\Users\kearney> # Telemetry level: 1 - basic, 3 - full
PS C:\Users\kearney> $value = "3"
PS C:\Users\kearney> New-ItemProperty -Path $path -Name AllowTelemetry -Value $value -Type Dword -Force


AllowTelemetry : 3
PSPath         : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Polic
                 ies\DataCollection
PSParentPath   : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Polic
                 ies
PSChildName    : DataCollection
PSDrive        : HKLM
PSProvider     : Microsoft.PowerShell.Core\Registry



PS C:\Users\kearney> New-ItemProperty -Path $path -Name MaxTelemetryAllowed -Value $value -Type Dword -Force


MaxTelemetryAllowed : 3
PSPath              : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\
                      Policies\DataCollection
PSParentPath        : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\
                      Policies
PSChildName         : DataCollection
PSDrive             : HKLM
PSProvider          : Microsoft.PowerShell.Core\Registry
```

完成后说是要重启电脑，重启之前我看了下设置，诊断数据这块直接消失了，于是重启。

再操作一下：

```powershell
PS C:\Users\kearney> sfc /SCANNOW

Beginning system scan.  This process will take some time.

Beginning verification phase of system scan.
Verification 100% complete.

Windows Resource Protection found corrupt files and successfully repaired them.
For online repairs, details are included in the CBS log file located at
windir\Logs\CBS\CBS.log. For example C:\Windows\Logs\CBS\CBS.log. For offline
repairs, details are included in the log file provided by the /OFFLOGFILE flag.
```

说是修复好了。然后重新加入预览体验后重启。在检查更新就有了。下载了部分更新后提示空间不足。。。安装部分更新重启挪出一点空间，再次下载，可以下载完成，但是安装失败，提示C盘空间不足。它就不能先检测空间够不够再下载嘛？看样子 10G 不够 下载完 Windows 11 Insider Preview 10.0.22635.4225 (ni_release) 之后再进行安装。稍微清理了一下，15.8 GB 能够下载并安装了。实际的安装时间是有点长的，建议吃饭的时候进行安装，安装好之后好就升级到了 Windows 11 Home 23H2 22635.4225。

现在通过预览体验计划更新到了 23H2，说明这版本还没有正式推送给我这个设备，那至少推 22H2 吧，毕竟 21H2 已经停止维护了。

从 39 更新到 BIOS-F0CN41WW 会导致系统崩溃，上一个版本也是一样崩溃，只能强制关机。在 2k 屏幕上绿屏代码看不清楚。然后关闭不充电模式，让电量充到90%以上，UMA从4G调整为2G，从官网重新下BIOS再次更新依旧绿屏，由于外接显示器1080P，可以看到 SYSTEM_SERVICE_EXCEPTION TdkLib64.sys，解决办法是暂时关闭内存隔离

# 相关阅读

- [Windows 11 Version 21H2 终止服务（家庭版和专业版） 2023/07/26 ](https://learn.microsoft.com/zh-CN/lifecycle/announcements/windows-11-21h2-end-of-servicing)

- [4 种方法：手把手教你升级 Windows 11 23H2 2024-05-10](https://www.sysgeek.cn/windows-11-version-23h2-manual-upgrade/)

- [关于Windows IPv6远程拒绝服务及代码执行漏洞(CVE-2024-38063)的安全公告 2024-08-16](http://wlaq.dlut.edu.cn/info/1095/5264.htm)

- [如何更改Windows 10更新下载文件夹位置 culintai3473 2020-09-15](https://blog.csdn.net/culintai3473/article/details/108779667)：没测试过

- [小新pro13 2019 amd 升级 bios时蓝屏](https://club.lenovo.com.cn/thread-7837116-1-1.html): 管用，之前确实用过这个办法，后来忘记了。。。windows security - device security - core isolation - memory integrity。
> 先关闭安全中心里的“内存隔离”，重启。然后再更新BISO，就可以顺利更新了。自己的电脑刚刚也通过这个设置顺利更新了最新版BIOS，更新完进入系统后，再重新打开“内存隔离”即可
