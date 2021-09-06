---
title: "在 Arch Linux 上使用 Docker 运行 Mac OS - Catalina"
date: 2021-07-02T19:13:35+08:00
lastmod: 2021-07-02T19:13:35+08:00
keywords: ['mac os', 'docker', 'Catalina', 'linux']
description: "在 Arch Linux 上使用 Docker 运行 Mac OS - Catalina"
tags: [docker]
categories: [os]
author: "筱氚"
---
# 背景介绍
MacOS默认不对其他电脑平台发布，在 Apple 目前的战略中不把 os 作为可交易的商品，而是一种卖硬件附送的高价值软件。因此对于非 A 家的设备，想要整个 Mac OS 就需要自己想办法了，黑苹果的驱动问题不太好解决 -.- 个人已经六岁的 se 想给它加点东西，奈何 xcode 不能跑在 other os，也没听说过类似 wine 的 mac wine。

查了一下黑苹果的几种办法：虚拟机（VM、VirtualBox）、双系统（单黑系统不推荐）、KVM、Docker。后两种办法见于参考前排。

至于采用何种办法见仁见智，最简单省事的办法是虚拟机，下载启动。缺点是占用空间大。双系统自己找资料补驱动，KVM 的速度比 docker 慢一点（该方案可在参考中学习）。所以这里我采用 docker 方案。此方案适合有耐心捣鼓的朋友，可能会遇到不少 warninig error.

## 环境介绍

- OS： Arch Linux 5.10.47-1-lts
- CPU: AMD R5-4600u(支持虚拟化技术)
- Docker version 20.10.7

## 最终效果

![Catalina 系统桌面](/images/dockerMac/Catalina.jpg)

# 动手
主要参考 [sickcodes/Docker-OSX](https://github.com/sickcodes/Docker-OSX) 的 README.md

## 安装软件
我这里的 os 是 Arch，其它 os 的慢慢看 README 找。
```bash
$ sudo pacman -S qemu libvirt dnsmasq virt-manager bridge-utils flex bison iptables-nft edk2-ovmf docker

iptables-nft 与 iptables 有冲突。删除 iptables 吗?y
软件包 (16) gtk-vnc-1.2.0-1  gtksourceview4-4.8.1-1  iptables-1:1.8.7-1 [删除]  libosinfo-1.9.0-1
            libvirt-glib-4.0.0-1  libvirt-python-1:7.3.0-1  osinfo-db-20210531-1  phodav-2.5-1
            spice-gtk-0.39-3  virt-install-3.2.0-1  yajl-2.1.0-4  dnsmasq-2.85-1  edk2-ovmf-202105-1
            iptables-nft-1:1.8.7-1  libvirt-1:7.3.0-1  virt-manager-3.2.0-1
```
有些包已经安装过了不再重复安装，所以上面没有显示 qemu bridge-utils flex bison docker

## 开启 KVM 内核模块并启动 docker

这里进行操作之前需要现在 BIOS 开启虚拟化技术。此步骤需要自行解决，比较简单就不赘述贴图了。

```bash
sudo systemctl enable --now libvirtd
sudo systemctl enable --now virtlogd

echo 1 | sudo tee /sys/module/kvm/parameters/ignore_msrs

sudo modprobe kvm

# 启动 docker, 并且设置开机自动启动 docker
sudo systemctl enable --now docker
```

## 添加用户组

这个不加入用户组的话就不能正常使用 docker、libvert、kvm，为了减少 bug 的数量还是动动小手好一点。

```bash
sudo usermod -aG docker "${USER}"
sudo usermod -aG libvirt "${USER}"
sudo usermod -aG kvm "${USER}"

# kearney 是我的用户名
xhost +SI:localuser:kearney
```

## 拉取镜像
两个最新版本（11、10）的Mac OS，挑一个喜欢的下载就行。此步骤耗时较长，与网络状态有关，建议打开电影《建国大业》观看等待。本人校园网下载了半个多小时最后卡死了。。所以特意将镜像步骤放在了这里

### 设置 docker hub 镜像
```bash
$ sudo nano /etc/docker/daemon.json
# 复制粘贴下面的内容， ctrl + x 保存退出
{
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn",
    "https://ustc-edu-cn.mirror.aliyuncs.com/",
    "https://mirror.baidubce.com",
    "https://hub-mirror.c.163.com"
  ]
}
```

下面重启 docker 服务使镜像设置生效

```bash
sudo systemctl daemon-reload 
sudo systemctl restart docker
```

### 拉取镜像
```bash
# 如果要下载 Catalina 1.5 G，运行下面这个，我这里选择的是这个
docker pull sickcodes/docker-osx:latest
# 如果要下载 Big Sur 1.8 G，运行下面这个
docker pull sickcodes/docker-osx:big-sur
```

## 启动容器
### Catalina
Catalina 运作这个

```bash
docker run -it \
    --device /dev/kvm \
    -p 50922:10022 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    sickcodes/docker-osx:latest
```

### Big Sur
Big Sur 版本则运行这个

```bash
docker run -it \
    --device /dev/kvm \
    -p 50922:10022 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -e GENERATE_UNIQUE=true \
    -e MASTER_PLIST_URL=https://raw.githubusercontent.com/sickcodes/osx-serial-generator/master/config-custom.plist \
    sickcodes/docker-osx:big-sur
```

## 修改 Mac 配置

成功启动之后选择 ‘Disk Utiliy’，左侧列表（Internal）里会有一些磁盘，选中那个 200G 左右的 QEMU HARDDISK Media,然后在上面五个按钮中点击Erase，名称你开心就好，随便填一个。

之后我分了个区，从 200 里分了个 60（本来想分 30 但是 readme 里说 xcode 至少 60G），然而硬盘只有 20G 剩余，现装试一试。

整好之后点击左上角红点返回，然后选择 Reinstall macOS 后继续。之后就一气呵成 同意、继续啥的。安装时间可以看会《觉醒年代》。

# Q&A
## 个人配置信息

当无法正常运行 Docker-OSX，提问之前需要把这些信息加上

<details>
<summary>点击查看如何获取个人配置信息</summary>

```bash
$ uname -a
Linux arch 5.10.47-1-lts #1 SMP Wed, 30 Jun 2021 13:52:19 +0000 x86_64 GNU/Linux
$ qemu-system-x86_64 --version 
QEMU emulator version 6.0.0
Copyright (c) 2003-2021 Fabrice Bellard and the QEMU Project developers

$ echo $DISPLAY
:0

$ uname -a \
; echo 1 | sudo tee /sys/module/kvm/parameters/ignore_msrs \
; grep NAME /etc/os-release \
; df -h . \
; qemu-system-x86_64 --version \
; libvirtd --version \
; free -mh \
; nproc \
; egrep -c '(svm|vmx)' /proc/cpuinfo \
; ls -lha /dev/kvm \
; ls -lha /tmp/.X11-unix/ \
; ps aux | grep dockerd \
; docker ps | grep osx \
; grep "docker\|kvm\|virt" /etc/group
Linux arch 5.10.47-1-lts #1 SMP Wed, 30 Jun 2021 13:52:19 +0000 x86_64 GNU/Linux
1
NAME="Arch Linux"
PRETTY_NAME="Arch Linux"
文件系统        容量  已用  可用 已用% 挂载点
/dev/nvme0n1p5  108G   89G   14G   87% /
QEMU emulator version 6.0.0
Copyright (c) 2003-2021 Fabrice Bellard and the QEMU Project developers
libvirtd (libvirt) 7.3.0
               total        used        free      shared  buff/cache   available
内存：       15Gi       3.9Gi       6.9Gi        99Mi       4.2Gi        10Gi
交换：      976Mi          0B       976Mi
12
12
crw-rw-rw- 1 root kvm 10, 232  7月  2 23:14 /dev/kvm
总用量 0
drwxrwxrwt  2 root root  60  7月  2 22:54 .
drwxrwxrwt 12 root root 560  7月  2 23:14 ..
srwxrwxrwx  1 root root   0  7月  2 22:54 X0
root        5084  0.1  0.5 2127756 83268 ?       Ssl  23:03   0:00 /usr/bin/dockerd -H fd://
kearney     9738  0.0  0.0   9516  2344 pts/1    S+   23:14   0:00 grep dockerd
kvm:x:992:kearney
docker:x:962:kearney
libvirt:x:960:kearney
```
</details>


## gtk initialization failed

这个是最常见的错误。。。issue里面相关的一大把。

<details>
<summary>点击展开错误信息</summary>

```bash
$ docker run -it \
    --device /dev/kvm \
    -p 50922:10022 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    sickcodes/docker-osx:latest
ssh-keygen: generating new host keys: RSA DSA ECDSA ED25519 
nohup: appending output to 'nohup.out'
++ id -u
++ id -g
+ sudo chown 1000:1000 /dev/kvm
++ id -u
++ id -g
+ sudo chown -R 1000:1000 /dev/snd
+ [[ 3 = max ]]
+ [[ 3 = half ]]
++ id -u
++ id -g
+ sudo chown -R 1000:1000 /dev/snd
+ exec qemu-system-x86_64 -m 3000 -cpu Penryn,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on,+pcid,+ssse3,+sse4.2,+popcnt,+avx,+aes,+xsave,+xsaveopt,check, -machine q35,accel=kvm:tcg -smp 4,cores=4 -usb -device usb-kbd -device usb-tablet -device 'isa-applesmc,osk=ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc' -drive if=pflash,format=raw,readonly,file=/home/arch/OSX-KVM/OVMF_CODE.fd -drive if=pflash,format=raw,file=/home/arch/OSX-KVM/OVMF_VARS-1024x768.fd -smbios type=2 -audiodev alsa,id=hda -device ich9-intel-hda -device hda-duplex,audiodev=hda -device ich9-ahci,id=sata -drive id=OpenCoreBoot,if=none,snapshot=on,format=qcow2,file=/home/arch/OSX-KVM/OpenCore-Catalina/OpenCore.qcow2 -device ide-hd,bus=sata.2,drive=OpenCoreBoot -device ide-hd,bus=sata.3,drive=InstallMedia -drive id=InstallMedia,if=none,file=/home/arch/OSX-KVM/BaseSystem.img,format=qcow2 -drive id=MacHDD,if=none,file=/home/arch/OSX-KVM/mac_hdd_ng.img,format=qcow2 -device ide-hd,bus=sata.4,drive=MacHDD -netdev user,id=net0,hostfwd=tcp::10022-:22,hostfwd=tcp::5900-:5900, -device vmxnet3,netdev=net0,id=net0,mac=52:54:00:09:49:17 -monitor stdio -vga vmware
qemu-system-x86_64: -drive if=pflash,format=raw,readonly,file=/home/arch/OSX-KVM/OVMF_CODE.fd: warning: short-form boolean option 'readonly' deprecated
Please use readonly=on instead
No protocol specified
Unable to init server: Could not connect: Connection refused
QEMU 6.0.0 monitor - type 'help' for more information
(qemu) qemu-system-x86_64: warning: host doesn't support requested feature: CPUID.01H:ECX.pcid [bit 17]
qemu-system-x86_64: warning: host doesn't support requested feature: CPUID.01H:ECX.pcid [bit 17]
qemu-system-x86_64: warning: host doesn't support requested feature: CPUID.01H:ECX.pcid [bit 17]
qemu-system-x86_64: warning: host doesn't support requested feature: CPUID.01H:ECX.pcid [bit 17]
ALSA lib confmisc.c:767:(parse_card) cannot find card '0'
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_card_driver returned error: No such file or directory
ALSA lib confmisc.c:392:(snd_func_concat) error evaluating strings
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_concat returned error: No such file or directory
ALSA lib confmisc.c:1246:(snd_func_refer) error evaluating name
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_refer returned error: No such file or directory
ALSA lib conf.c:5233:(snd_config_expand) Evaluate error: No such file or directory
ALSA lib pcm.c:2660:(snd_pcm_open_noupdate) Unknown PCM default
alsa: Could not initialize DAC
alsa: Failed to open `default':
alsa: Reason: No such file or directory
ALSA lib confmisc.c:767:(parse_card) cannot find card '0'
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_card_driver returned error: No such file or directory
ALSA lib confmisc.c:392:(snd_func_concat) error evaluating strings
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_concat returned error: No such file or directory
ALSA lib confmisc.c:1246:(snd_func_refer) error evaluating name
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_refer returned error: No such file or directory
ALSA lib conf.c:5233:(snd_config_expand) Evaluate error: No such file or directory
ALSA lib pcm.c:2660:(snd_pcm_open_noupdate) Unknown PCM default
alsa: Could not initialize DAC
alsa: Failed to open `default':
alsa: Reason: No such file or directory
audio: Failed to create voice `dac'
ALSA lib confmisc.c:767:(parse_card) cannot find card '0'
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_card_driver returned error: No such file or directory
ALSA lib confmisc.c:392:(snd_func_concat) error evaluating strings
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_concat returned error: No such file or directory
ALSA lib confmisc.c:1246:(snd_func_refer) error evaluating name
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_refer returned error: No such file or directory
ALSA lib conf.c:5233:(snd_config_expand) Evaluate error: No such file or directory
ALSA lib pcm.c:2660:(snd_pcm_open_noupdate) Unknown PCM default
alsa: Could not initialize ADC
alsa: Failed to open `default':
alsa: Reason: No such file or directory
ALSA lib confmisc.c:767:(parse_card) cannot find card '0'
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_card_driver returned error: No such file or directory
ALSA lib confmisc.c:392:(snd_func_concat) error evaluating strings
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_concat returned error: No such file or directory
ALSA lib confmisc.c:1246:(snd_func_refer) error evaluating name
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_refer returned error: No such file or directory
ALSA lib conf.c:5233:(snd_config_expand) Evaluate error: No such file or directory
ALSA lib pcm.c:2660:(snd_pcm_open_noupdate) Unknown PCM default
alsa: Could not initialize ADC
alsa: Failed to open `default':
alsa: Reason: No such file or directory
audio: Failed to create voice `adc'
gtk initialization failed

```
</details>

这是我遇到的第一个错误，查了 troubleshoot 和有关 run fail、gtk 的所有 issue，把里面提到的所有办法都尝试了一遍，于是就有了下面的“已经尝试过失败的办法”。最后不知道这个错误是如何变成了下面这个错误。。。

当这个问题转到下一个问题有回来的时候，我再次尝试失败办法中的可能，到 xhost + 就成功了。
 
### 解决办法

```bash
# 关闭 x 的安保措施
xhost +

# 运行 Catalina
docker run -it     --device /dev/kvm     -p 50922:10022     -v /tmp/.X11-unix:/tmp/.X11-unix     -e "DISPLAY=${DISPLAY:-:0.0}"     sickcodes/docker-osx:latest
# 能正常运行在往下走

# 开启 x 的安保措施
xhost -

# 将自己加入白名单。kearney 是我的用户名，注意更换
xhost +SI:localuser:kearney
```

xhost 设置会在重启后还原默认值，因此如果要经常用这个玩意，可以把`xhost +SI:localuser:kearney`加入`~/.bashrc`。这样每次打开 bash，都会自动载入这个设置。

## docker: unknown server OS

出现这个问题说明 docker 出了问题。。issue 里说是 docker 没有允许，但是我这里 docker 服务是在运行的（systemctl status docker），但是 Docker daemon 却没有跑起来。最后万能办法 - 重启一下电脑。这个错误就消失了，变回了上面的错误。

<details>
<summary>点击展开错误信息</summary>

```bash
$ docker run -it     --device /dev/kvm     -p 50922:10022     -v /tmp/.X11-unix:/tmp/.X11-unix     -e "DISPLAY=${DISPLAY:-:0.0}"     sickcodes/docker-osx:latest
docker: unknown server OS: .
See 'docker run --help'.

$ sudo systemctl start docker
$ docker info
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  app: Docker App (Docker Inc., v0.9.1-beta3)
  buildx: Build with BuildKit (Docker Inc., v0.5.1-tp-docker)

Server:
ERROR: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
errors pretty printing info

$ pgrep dockerd
103238

$ sudo systemctl stop docker
$ sudo dockerd

$ docker run -it \
    --device /dev/kvm \
    -p 50922:10022 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    sickcodes/docker-osx:latest
ssh-keygen: generating new host keys: RSA DSA ECDSA ED25519 
nohup: appending output to 'nohup.out'
++ id -u
++ id -g
+ sudo chown 1000:1000 /dev/kvm
++ id -u
++ id -g
+ sudo chown -R 1000:1000 /dev/snd
+ [[ 3 = max ]]
+ [[ 3 = half ]]
++ id -u
++ id -g
+ sudo chown -R 1000:1000 /dev/snd
+ exec qemu-system-x86_64 -m 3000 -cpu Penryn,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on,+pcid,+ssse3,+sse4.2,+popcnt,+avx,+aes,+xsave,+xsaveopt,check, -machine q35,accel=kvm:tcg -smp 4,cores=4 -usb -device usb-kbd -device usb-tablet -device 'isa-applesmc,osk=ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc' -drive if=pflash,format=raw,readonly,file=/home/arch/OSX-KVM/OVMF_CODE.fd -drive if=pflash,format=raw,file=/home/arch/OSX-KVM/OVMF_VARS-1024x768.fd -smbios type=2 -audiodev alsa,id=hda -device ich9-intel-hda -device hda-duplex,audiodev=hda -device ich9-ahci,id=sata -drive id=OpenCoreBoot,if=none,snapshot=on,format=qcow2,file=/home/arch/OSX-KVM/OpenCore-Catalina/OpenCore.qcow2 -device ide-hd,bus=sata.2,drive=OpenCoreBoot -device ide-hd,bus=sata.3,drive=InstallMedia -drive id=InstallMedia,if=none,file=/home/arch/OSX-KVM/BaseSystem.img,format=qcow2 -drive id=MacHDD,if=none,file=/home/arch/OSX-KVM/mac_hdd_ng.img,format=qcow2 -device ide-hd,bus=sata.4,drive=MacHDD -netdev user,id=net0,hostfwd=tcp::10022-:22,hostfwd=tcp::5900-:5900, -device vmxnet3,netdev=net0,id=net0,mac=52:54:00:09:49:17 -monitor stdio -vga vmware
qemu-system-x86_64: -drive if=pflash,format=raw,readonly,file=/home/arch/OSX-KVM/OVMF_CODE.fd: warning: short-form boolean option 'readonly' deprecated
Please use readonly=on instead
No protocol specified
Unable to init server: Could not connect: Connection refused
QEMU 6.0.0 monitor - type 'help' for more information
(qemu) qemu-system-x86_64: warning: host doesn't support requested feature: CPUID.01H:ECX.pcid [bit 17]
qemu-system-x86_64: warning: host doesn't support requested feature: CPUID.01H:ECX.pcid [bit 17]
qemu-system-x86_64: warning: host doesn't support requested feature: CPUID.01H:ECX.pcid [bit 17]
qemu-system-x86_64: warning: host doesn't support requested feature: CPUID.01H:ECX.pcid [bit 17]
ALSA lib confmisc.c:767:(parse_card) cannot find card '0'
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_card_driver returned error: No such file or directory
ALSA lib confmisc.c:392:(snd_func_concat) error evaluating strings
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_concat returned error: No such file or directory
ALSA lib confmisc.c:1246:(snd_func_refer) error evaluating name
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_refer returned error: No such file or directory
ALSA lib conf.c:5233:(snd_config_expand) Evaluate error: No such file or directory
ALSA lib pcm.c:2660:(snd_pcm_open_noupdate) Unknown PCM default
alsa: Could not initialize DAC
alsa: Failed to open `default':
alsa: Reason: No such file or directory
ALSA lib confmisc.c:767:(parse_card) cannot find card '0'
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_card_driver returned error: No such file or directory
ALSA lib confmisc.c:392:(snd_func_concat) error evaluating strings
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_concat returned error: No such file or directory
ALSA lib confmisc.c:1246:(snd_func_refer) error evaluating name
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_refer returned error: No such file or directory
ALSA lib conf.c:5233:(snd_config_expand) Evaluate error: No such file or directory
ALSA lib pcm.c:2660:(snd_pcm_open_noupdate) Unknown PCM default
alsa: Could not initialize DAC
alsa: Failed to open `default':
alsa: Reason: No such file or directory
audio: Failed to create voice `dac'
ALSA lib confmisc.c:767:(parse_card) cannot find card '0'
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_card_driver returned error: No such file or directory
ALSA lib confmisc.c:392:(snd_func_concat) error evaluating strings
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_concat returned error: No such file or directory
ALSA lib confmisc.c:1246:(snd_func_refer) error evaluating name
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_refer returned error: No such file or directory
ALSA lib conf.c:5233:(snd_config_expand) Evaluate error: No such file or directory
ALSA lib pcm.c:2660:(snd_pcm_open_noupdate) Unknown PCM default
alsa: Could not initialize ADC
alsa: Failed to open `default':
alsa: Reason: No such file or directory
ALSA lib confmisc.c:767:(parse_card) cannot find card '0'
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_card_driver returned error: No such file or directory
ALSA lib confmisc.c:392:(snd_func_concat) error evaluating strings
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_concat returned error: No such file or directory
ALSA lib confmisc.c:1246:(snd_func_refer) error evaluating name
ALSA lib conf.c:4745:(_snd_config_evaluate) function snd_func_refer returned error: No such file or directory
ALSA lib conf.c:5233:(snd_config_expand) Evaluate error: No such file or directory
ALSA lib pcm.c:2660:(snd_pcm_open_noupdate) Unknown PCM default
alsa: Could not initialize ADC
alsa: Failed to open `default':
alsa: Reason: No such file or directory
audio: Failed to create voice `adc'
gtk initialization failed

$ docker info
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  app: Docker App (Docker Inc., v0.9.1-beta3)
  buildx: Build with BuildKit (Docker Inc., v0.5.1-tp-docker)

Server:
 Containers: 8
  Running: 0
  Paused: 0
  Stopped: 8
 Images: 2
 Server Version: 20.10.7
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: false
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 36cc874494a56a253cd181a1a685b44b58a2e34a.m
 runc version: v1.0.0-0-g84113eef
 init version: de40ad0
 Security Options:
  seccomp
   Profile: default
  cgroupns
 Kernel Version: 5.10.47-1-lts
 Operating System: Arch Linux
 OSType: linux
 Architecture: x86_64
 CPUs: 12
 Total Memory: 15.06GiB
 Name: arch
 ID: PDMD:ZWZ3:XLJN:KJCX:PXXH:THRT:DLWM:W2J6:BRLM:E7VK:OUPS:Y4L3
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Registry Mirrors:
  https://docker.mirrors.ustc.edu.cn/
  https://ustc-edu-cn.mirror.aliyuncs.com/
  https://mirror.baidubce.com/
  https://hub-mirror.c.163.com/
 Live Restore Enabled: false
```
</details>

## ALAS 警告、错误

类似于下面这些东西，readme 中说了不用担心，忽略ta就行。

```bash
ALSA lib pcm.c:2660:(snd_pcm_open_noupdate) Unknown PCM default
alsa: Could not initialize DAC
alsa: Failed to open `default':
alsa: Reason: No such file or directory
audio: Failed to create voice `dac'
ALSA lib *.c:: cannot find card '0'
ALSA lib *.c:No such file or directory
```

##  已经尝试过失败的办法

- sudo chmod 666 /dev/kvm
- xhost +（失败后记得用 `xhost -` 开启安全保护）
- sudo pacman -S xorg-xhost

## docker 文件清理

硬盘只剩下 700 M，一看是 docker 的文件占了 30 G。可是两个 docker 镜像才 5 G 不到；因此猜测是运行这个镜像安装 mac 系统时产生的。。。先清理了再说吧。

<details>
<summary>点击展开操作过程演示</summary>

```bash
$ docker images
REPOSITORY                  TAG       IMAGE ID       CREATED        SIZE
sickcodes/docker-osx        latest    87e0808867a0   5 weeks ago    3.09GB
dinghao188/rcore-tutorial   latest    779b0c88b284   4 months ago   1.17GB

# 先运行两个 docker 镜像
$ docker run -it     --device /dev/kvm     -p 50922:10022     -v /tmp/.X11-unix:/tmp/.X11-unix     -e "DISPLAY=${DISPLAY:-:0.0}"     sickcodes/docker-osx:latest

$ docker run --rm -it --mount type=bind,source=/home/kearney/Documents/code/rust/ros/rCore-Tutorial-v3,destination=/mnt dinghao188/rcore-tutorial

# 删除未在运行的镜像及其文件，一定要先运行 image 哈，不然连 image 都没了
$ docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] y
Deleted Containers:
3f630d1695dd24eca1d9df6cef46b28f62517dccb9541094224f1a7a97b1436d
d53e208599b1e0f84de9fcb6d35b73d8ce4450043b79a6b027bc951c7af58738
2ac6d31a859e46e2fcb7f8b3d1432247a80ff0551f8bd568ab5b09124bb5c5c9
93ecc91d5299b1e3e7a4816818e4020e424cb85b06e83b6861adabc00d14d700
7b3075385213caccd59845cc3a9032a1c176a58860b40927d1c7aee12b8bec0d
1040de33193eb3239810b110947a9201a7178afb623fe8f4ee630ec3f18184e8
b0184de7b952f5fc3d5ac875def5376ad5d7be689e911acb91c35836b6dcec82
dc640723ead85c4c3adbf74e1dc150f5decf01cea91f0812192fbb348763ee3b
9344b05e2c92ab517992209eb66e31fc120c4ca6fd314c00db7734dc8223e45b
1b626f92bdc28789321cb2cc78790349e55516e8107db29a8d32a9536d8a2675
7aa8432d607e70ffa7bdad3d659731f6ccc8adeccba7e042d6c256a9e9ce2eb1
071e511d8e5b89115b13cae538ecb0e1b0035fa19233c7316ed56d8dfbeb1560
0c8dd0544f441c1de41ec729814c8e0790c4f4574a7c4c92f544a6fb71d52563
79c868f8f62dee1ba0e1cc752de1c76270fc15bef3560070011fd173c56646c3
14989b73427d9d4a7fd6df5d357051fe96f3ed15bcbeac25d47a945348a8e27a
77b676293ceed30b9bd9d7255bcbb1b611ca8080649bd121d2a69c4f128e373d
4cb1956c4e5052bb3938f00c5d8a4682c936b96cd5acedce7bdf2dcd1152a509
9fc94912b7496e942b1a015f742ab243d7e779f02d7cc9ddbf976f18e0c33c54
b281eb6a9734a9b3181b611b188322a57a8680fa4bd4271bb1b360e2ee20fabb
b90a316b86069629c43bf751d12dd9049039808dc0d65d8198062acf9133ff47
fc3ea968c0548af3224027a3ad29a526574fc1d63c6311d3e05e34f8d6ccc970
13b02d4480f996300c81eaa045abb7c44c6b6b6a5fb9dd9362fbf1880be042d5
0da1255597bb8ea26d97110be758616696491909a96043426108c2b0de2473b4
d51e9586fafb77a1a73a2efad60040c25b912816aa366d20ae6177434dda7623
97054b85fa72db2af3ca37d50b244eeac0c25c52e85b9bd3ca3e3d6391f5087d
d6ed107c6d2b193e5e9a9df76262e938f773eca605628e22c22e1aeb2ae0e758
fdb53e84b237e6c2b52064ecc4c392c58e604e38cf69b41d9dfd3ca674a9b498
fa47639a074653202acc09a0c255f738ef28d8b5cbabc252f17d4f72b9cbf8b1
ae3d573e1f4984bbc8b26e20b170dc766c48109e79eaf767d24c56e141818045
00641c61807ab6d2133a8f534ca7c407552889d11c4780af3424e2a7a6950161
b6ce564be398ef5eb2d2ae6fc371b71f5a41f0b00bbebb5177356a76b4310ea2
dcd120233428476e19eb8b30ba23a25cb802c8de12278bce6c23da647266fe87
3692f925d0017743c088029b30fcb1d9862854cec59e4b434a90ebcda26d8884
4248205a172eac018243137329e86ca9946bb97c1f1cabec4d1e44ee497ed1a5

Total reclaimed space: 29.62GB
```
</details>

## 从 QEMU 中取回鼠标、键盘控制权

真的头大，在那里一对代码滚动，也没法操作，鼠标键盘都没法用了。。同时按住 Ctrl + Alt + G 即可。


# 参考
- [foxlet/macOS-Simple-KVM](https://github.com/foxlet/macOS-Simple-KVM): Tools to set up a quick macOS VM in QEMU, accelerated by KVM.
- [sickcodes/Docker-OSX](https://github.com/sickcodes/Docker-OSX): Run Mac in a Docker!
- [Docker-OSX/issues/302](https://github.com/sickcodes/Docker-OSX/issues/302)：提问完重启我就好了。。尴尬
- [macOS 版本官方清单](https://support.apple.com/zh-cn/HT201260)