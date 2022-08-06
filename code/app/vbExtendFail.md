# 手动安装 VirtualBox 增强扩展包及 USB 设备无法识别的问题
- date: 2022-03-19
- lastmod: 2022-08-06

## 从 VB 设置里下载增强包

启动虚拟机之后，在虚拟机顶部的菜单栏 -> 设备 -> 安装增强功能，然后会从网络上搜索对应版本的扩展包，点击确认就会开始下载。一般来说，这个下载安装过程不会很久，但是有时候就会遇到不一般的情况 -- 下载速度太慢了，此时需要找别的办法，切换网络或者手动下载增强包进行安装。

## 手动下载增强包

当前 VB 版本为 6.1.32 r149290，是最新版（需要确保主体和扩展包的版本一致），[Download VirtualBox](https://www.virtualbox.org/wiki/Downloads) -> Extension Pack -> All supported platforms. 然后就得到了 Oracle_VM_VirtualBox_Extension_Pack-6.1.32.vbox-extpack

### 双击增加包进行安装

双击该增强包，会自动打开 vb 进行安装，同意之后输入密码，输入正确还要我在输入一次。。。然后报错，查看详情如下

```
The installer failed with exit code 1: [sudo] kearney 的密码：
sudo: /usr/lib/virtualbox/VBoxExtPackHelperApp --stdout /tmp/VBoxExtPackHelper-n44o63/stdout --stderr /tmp/VBoxExtPackHelper-n44o63/stderr --elevated install --base-dir /usr/lib/virtualbox/ExtensionPacks --cert-dir /usr/share/virtualbox/ExtPackCertificates --name 'Oracle VM VirtualBox Extension Pack' --tarball /home/kearney/Downloads/Oracle_VM_VirtualBox_Extension_Pack-6.1.32.vbox-extpack --sha-256 ba9dd35434c5cac3b99c53708b10ab07fa8a2fb2995b4515ad81f2d7c465c5ec：找不到命令

[sudo] kearney 的密码：sudo: /usr/lib/virtualbox/VBoxExtPackHelperApp --stdout /tmp/VBoxExtPackHelper-n44o63/stdout --stderr /tmp/VBoxExtPackHelper-n44o63/stderr --elevated install --base-dir /usr/lib/virtualbox/ExtensionPacks --cert-dir /usr/share/virtualbox/ExtPackCertificates --name 'Oracle VM VirtualBox Extension Pack' --tarball /home/kearney/Downloads/Oracle_VM_VirtualBox_Extension_Pack-6.1.32.vbox-extpack --sha-256 ba9dd35434c5cac3b99c53708b10ab07fa8a2fb2995b4515ad81f2d7c465c5ec：找不到命令.


Result Code:
NS_ERROR_FAILURE (0x80004005)
Component:
ExtPackManagerWrap
Interface:
IExtPackManager {70401eef-c8e9-466b-9660-45cb3e9979e4}
```

### 从 VB 设置里添加增强包

VB -> File -> Preferences -> Extensions -> 右侧绿色方框内含加号的 Add new package。选择下载好的增强包，之后的过程和上面一样错误了。

## 解决办法 - 命令行安装

[[SOLVED] Virtualbox adding extention pack- in Manjaro](https://forum.manjaro.org/t/solved-virtualbox-adding-extention-pack-in-manjaro/25918)
> 1. sudo VBoxManage extpack install <Version from Oracle's download page>
  2. installing AUR package (http://aur.archlinux.org/packages/virtualbox-ext-oracle-manjaro 745)


```bash
# 安装扩展包（已经切换到扩展包所在目录）
$ sudo VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-6.1.32.vbox-extpack
# 显示扩展包列表
$ VBoxManage list extpacks
```

于是一行命令安装完成，但是打开 vb 启动 win11 虚拟机，发现增强功能没有开启，但是在 vb 的全局设置-扩展里可以看到该增强包已经添加成功。打开虚拟机的文件管理器，点击此电脑，在里面可以看到有一个盘（默认是空的 CD 驱动器）显示的是 vb 的标志（如果没有先重启电脑，启动虚拟机，顶部菜单栏 -> Devices(设备) -> Insert Guest Additions CD Images(安装增强功能)。

双击显示 VB Guest Additions 的 CD 驱动器进行安装增强包，一路安装完重启即可。

## USB 找不到设备

我已经确认安装好了增强包，已经实现了剪贴板、拖放、共享文件夹，USB 现在已经也有三代了（默认只有 USB 1.0），但是点击绿色加号显示没有找到 USB 设备，然而我的主机都显示出来了。查阅了一下是没有将自己添加到 vboxusers 用户组的缘故，这个问题和使用 arduino 无法烧录的很类似，都是没有加入用户组，当然加入用户组之后不会立马生效，需要退出登录重新登录或者重启才会生效。

[Virtualbox USB Devices Aren’t Recognized?](https://forum.manjaro.org/t/virtualbox-usb-devices-arent-recognized/57361/3)：总之是将当前用户加入 vboxusers 用户组，命令好像在不同系统不太一样，比如[VirtualBox中没有可用的USB设备](https://qastack.cn/superuser/956622/no-usb-devices-available-in-virtualbox)是 sudo adduser $USER vboxusers
> sudo gpasswd -a $USER vboxusers

### USB 控制器导致的无法开机

启动win11虚拟机报错显示，已经确定BIOS中开启虚拟化功能了，错误原因如下

```
不能为虚拟电脑 win11 打开一个新任务.

The VM session was aborted.

返回 代码: NS_ERROR_FAILURE (0x80004005)
组件: SessionMachine
界面: ISession {c0447716-ff5a-4795-b57a-ecd5fffa18a4}
```

搜索有restart vb的，但是测试发现不存在该重启脚本，在论坛[VirtualBox Is Aborting](https://forum.manjaro.org/t/virtualbox-is-aborting/102451/3)中提到，测试后成功开启
  >  one probable cause is because you have the extension pack enabled because you use usb3 controller.Disable usb2 and usb3 in the vm properties and try again.
