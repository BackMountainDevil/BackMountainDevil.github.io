---
title: "在 Linux 上使用人脸识别来登陆和认证"
date: 2021-04-15T21:36:44+08:00
lastmod: 2021-04-15T21:36:44+08:00
keywords: ['howdy', 'linux']
description: "在 Arch Linux 上使用人脸识别来解锁，如锁屏、sudo等"
tags: ['linux']
categories: [os]
author: "筱氚"
---
# 干啥

是这样子的，我的笔记本使用的键盘是蓝牙的，然后无论是 win 还是 arch,蓝牙功能都是在登陆进系统之后才开启，这样每次输入密码我就得把身子往前靠、双手伸向笔记本输入密码。。。而 win 提供了一种优雅的方式登陆进入系统：Windows Hello - 人脸识别

这么便捷的方式 Linux 大法没有的话，咋说的过去呢？于是在一番搜索之下发现了 [howdy](https://github.com/boltgolt/howdy)，而且也支持我当前使用的 Arch Linux,废话不多说，开始捣鼓。在开始之前，有一点需要明确，也是作者们一直提醒的一点： 不要将 howdy 作为唯一的登陆手段，万一 howdy 崩了，你进不去系统就哭吧。

## 相关信息

- OS: Linux arch 5.11.6
- Laptop: Lenovo XiaoXin Pro 13 AMD 2020

# 动手
## 安装
### AUR

由于 howdy 的官方发行安装包只有 deb，懒人大法已经准备好在 AUR 中了，需要一个 AUR 下载助手，我这里用的是 yay,以下以 yay 为例，还没有安装 yay 的可以参考[Arch Linux yay -  Kearney 2021-03-17](https://blog.csdn.net/weixin_43031092/article/details/113944349)进行安装


### howdy

```bash
yay -S howdy
```

21：53 - 
第一次下载了二十多分钟下载出错。。
22：16 - 
23：04在下face_recognition_models-0.3.0.tar.gz剩下4.5h。。
11:56 - 
 shape_predictor_5_face_landmarks下载出错
 12：19 - 12：30
 安装完成

## 配置

### 配置 PAM

在[Howdy - ArchWiki](https://wiki.archlinux.org/index.php/Howdy)中是这么说的：你想用 howdy 通过 pam 来认证啥就在哪一个配置文件里的首行加入这行配置。

`auth sufficient pam_python.so /lib/security/howdy/pam.py`

看了一下 `/etc/pam.d` 下有39个配置文件，想认证 `sudo` 就改 `/etc/pam.d/sudo`，想做人脸图像识别登陆(KDE, GNOME)就改 `/etc/pam.d/system-local-login`

```bash
sudo nano /etc/pam.d/sudo
# 在首行加入上面那行配置， Ctrl + S 保存， Ctrl + X 退出。

sudo nano /etc/pam.d/system-local-login
# 同上
sudo nano /etc/pam.d/sddm
# 同上
```

测试过 system-login、login、kde、sddm-greeter、sddm-autologin 无效。。。

### 配置 howdy
### 摄像头选择
> [IR emitters not working on Dell Inspiron 5567 #543](https://github.com/boltgolt/howdy/issues/543)  
[EmixamPP/linux-enable-ir-emitter](https://github.com/EmixamPP/linux-enable-ir-emitter)

在第一次配置完 howdy 之后，我发现它辨识人脸的时候不会像 windows 一样开启红外，起初我以为是 howdy 做到这样就不错了，然后看了上面两帖子，才发现是我没有正确配置好摄像头。

#### 方法一

```bash
# 查看本机所有摄像设备，一般列出来都是/dev/videX
v4l2-ctl --list-devices

# 逐个测试上面的结果,我上面得到的结果是 0～3，其中 1、3 测试异常退出
ffplay /dev/video0
# 观察到摄像头正常拍摄（1280x720），旁边的白灯保持开启状态
ffplay /dev/video2
# 观察到内容全是黑白的了，没有色彩，暗了很多，旁边的白灯、红灯保持开启状态
# 且拍摄的图形（640x360）比 /dev/video0 拍到的小很多
```

#### 方法二

用 VLC 中的 媒体 - 打开捕获设备 - 高级选项（advance options）， 里面的**视频捕获设备**默认是 `/dev/video0`， 确定 - 播放 之后确实显示的摄像头的画面，因此确定了摄像头标识 `/dev/video0` 可以正常使用，然后在**视频捕获设备**右侧选择浏览，选择 `/dev/videoX`（ X 表示任一数字 ），逐个测试播放，最后得到的结果和方法一相同。 

#### 修改摄像头配置

`/dev/video0` 的拍摄精度（单位面积内采集的像素数量）更高一些，但是在光线不足的情况下采集到的画面暗了不少，噪点很多，丢失了不少细节。 `/dev/video2` 的精度虽然下降了一半，但是拥有红外的加持，在黑暗环境下和自然光状态下表现差不多，但是从自己的感觉上看，还是 `/dev/video0` 表现更好，虽然都是同一个摄像头，为什么加上了红外就拍摄精度下降了呢？？不能锦上添花呢？当然如果你觉得红外加持效果好，那就填上红外模式对于的 X 吧。

```bash
sudo howdy config
```

找到并补全 `device_path = /dev/video0` ，保存 Ctrl + S ，退出 Ctrl + X。 先保存再退出。

### 添加人脸模型

```bash
sudo howdy -U kearney add

# 这里发生了一个错误。。。
Traceback (most recent call last):
  File "/usr/bin/howdy", line 95, in <module>
    import cli.add
  File "/usr/lib/security/howdy/cli/add.py", line 11, in <module>
    from recorders.video_capture import VideoCapture
  File "/usr/lib/security/howdy/recorders/video_capture.py", line 6, in <module>
    import cv2
ModuleNotFoundError: No module named 'cv2'
```

这个错误是缺少opencv.。。但是用`pacman -Qi howdy`和`pacman -Qi opencv`发现两者已经安装上了。。到[AUR - howdy](https://aur.archlinux.org/packages/howdy/)下的 Comments 发现缺少的不是一个依赖。。。

补上依赖
```bash
pacman -S python-opencv
# 继续添加我的面子
sudo howdy -U kearney add
```

## 测试

重启一下先

```bash
sudo howdy test
```

如果出现 3 个 `WARN` 是正常的，是 GStreamer 的 warning 597、1034、2056,至于为什么可以看[Howdy - ArchWiki](https://wiki.archlinux.org/index.php/Howdy)中的 Troubleshooting，暂时的解决办法看 wiki，添加一个环境变量 `OPENCV_LOG_LEVEL=ERROR` 。

```bash
$ nano ~/.bashrc
# 在最后添加上下面这句话
export OPENCV_LOG_LEVEL=ERROR

# 完成后 Ctrl + S 保存， Ctrl + X 退出 nano
$ source ~/.bashrc
```

根据 [Howdy - LinuxReviews](https://linuxreviews.org/Howdy#ssdm_.28The_KDE_login_manager.29) 中的描述，KDE 在登陆时并不会自动调用人脸识别登陆，需要点击一下登陆按钮才会启动 howdy（不需要输入密码哈）

# 进阶

```bash
# 查看本机所有摄像设备
v4l2-ctl --list-devices
# 查看 /dev/video0 的可用分辨率及其对于的fps，一般来说分辨率越高，帧率越低。
v4l2-ctl -d /dev/video0 --list-formats-ext
```

修改配置中的分辨率可以获取更高清的图像，但是否会提高识别速度和精度就不得而知了。设置里可以设置识别时间、是否显示识别结果、人脸识别模型等。

# 总结

看了好几个不同的软件，大体上都是用的 PAM，只是实现方式上有所不同。目前我这里可以用人脸替代密码的情况有sudo、人脸识别登陆，其ta用途暂未想到。

在 howdy 的 [issue](https://github.com/boltgolt/howdy/issues/448) 中，提到了和自己一样的情况，也是无法在锁屏、休眠、睡眠的情况下使用 howdy 解锁，但是在测试中我发现可以曲线使用 howdy 进行解锁，点击 “切换用户（ Switch User ）” 就可以切换到使用 howdy 的初始登陆页面。

## 命令

> howdy [-U user] [-y] command [argument]

| Command  |	Description |
| :------  |  :------  |
| add       | 	为用户添加新的人脸模型  |
| clear      |  	删除某用户的全部人脸模型  |
| config      | 	在默认编辑器中打开配置文件  |
| disable     | 	禁用或者开启 howdy  |
| list        | 	列出该用户的全部人脸模型  |
| remove       | 	删除该用户的某一特定人脸模型 |
| snapshot     |  拍摄摄像头的快照  |
| test        | 	测试摄像头和识别方法  |
| version     | 	输出当前版本号  |

举个例子

```bash
$ sudo howdy version
Howdy 2.6.1

$ sudo howdy -U kearney list
Known face models for kearney:

        ID  Date                 Label
        0   2021-04-17 10:45:15  kearney
        1   2021-04-17 10:46:34  kearney-glasses
        2   2021-04-28 14:26:21  glassedinfooice

# 关闭 howdy
$ sudo howdy disable true
# 开启 howdy
$ sudo howdy disable false
```

# 参考

[WSL Hello sudo brings Windows Hello authentication to Linux on WSL](https://winaero.com/wsl-hello-sudo-brings-windows-hello-authentication-to-linux-sudo/#:~:text=Windows%20Hello%20makes%20a%20signature%20of%20the%20given,user%20who%20corresponds%20to%20the%20given%20Linux%20user.)：可惜的是这个插件是用在 WSL 里的

[boltgolt/howdy - Windows Hello™ style facial authentication for Linux ](https://github.com/boltgolt/howdy)：已支持 Debian/Ubuntu, Arch Linux, Fedora and openSUSE

[Howdy - ArchWiki](https://wiki.archlinux.org/index.php/Howdy)

[Howdy - Linux 的人脸识别](https://www.bilibili.com/read/cv7824113)

[face authentication for linux](https://www.linuxjournal.com/content/face-authentication-03-linux)：2009 年谷歌出品，网络卡我还没看到

[rushabh-v/linux_face_unlock](https://github.com/rushabh-v/linux_face_unlock): Ubuntu 人脸认证

[saanuregh/hola](https://github.com/saanuregh/hola)：Windows Hello™ style facial authentication for Linux written in Rust 

[AUR - howdy](https://aur.archlinux.org/packages/howdy/)

[Howdy - LinuxReviews](https://linuxreviews.org/Howdy#ssdm_.28The_KDE_login_manager.29):sudo nano /etc/pam.d/sddm
>Note that ssdm will not not activate the web camera and try to identify the user in front of it before a user account is selected and the > button to login is pressed.

[IR emitters not working on Dell Inspiron 5567 #543](https://github.com/boltgolt/howdy/issues/543)

[EmixamPP/linux-enable-ir-emitter](https://github.com/EmixamPP/linux-enable-ir-emitter): instructions to enable the IR emitter. 
