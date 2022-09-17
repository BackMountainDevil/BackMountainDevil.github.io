# debian 零基础安装与配置
- date: 2021-09-06
- lastmod: 2022-09-16

# 使用感受

1. 蓝牙固件：2022使用nonfree可以正常识别网卡了，蓝牙可以连接键盘鼠标，但是两幅蓝牙耳机都不行，而在manajro下两幅耳机都是可以正常使用的，这一点导致我切换回了Manjaro,蓝牙耳机还是很重要的。
2. 休眠Sleep：Debian 11.5 可以休眠，Manjaro 测试点击休眠后不久就自动唤醒了

# 安装系统

可以从 [TUNA Mirror](https://mirrors.tuna.tsinghua.edu.cn/) 的右侧蓝色按钮“下载链接”选择Debian non-free下载iso（non-free版本可能含有我需要的固件，free版本试过了发现无网卡驱动）,我是64位pc选择amd64,桌面环境我偏爱KDE,于是我选择了 debian-live-11.5.0-amd64-kde+nonfree.iso，校验文件可以从[对于目录](https://mirrors.tuna.tsinghua.edu.cn/debian-nonfree/images-including-firmware/current-live/amd64/iso-hybrid/)下获取。

将校验过的iso拷贝到Ventoy处理过的U盘，很容易进入live模式进行安装。

安装语言选择中文会自动安装中文输入法，有个小问题就是/home/$UserName下的几个目录会是中文的，对于tab补全来说这是一个烦恼，当然不影响正常使用，想把中文目录修改为英文可以参考1进行操作。如果选择英语作为安装首选语言，需要自己添加中文输入法。

分区以前我都是一个根分区就万事了，后面更换系统的时候都要提前把/home下的都先备份，这次学会了分两个区，一个装/home，一个装/，在 Ubuntu 22 LTS 上尝试是可以的，然后如法炮制在 Debian 11.5 上就出现了一点问题，同样的分区操作，Debian 无法进入系统，显示的是grub界面，我以为是grub损坏，尝试了[d](../deepin/DeepinGrub)的方法，试了好几遍重启也无效，最后重装 Debian，这回分区操作依旧一样，但是点击下一步出现了警告提示，没有指定efi，一下子就明白上一回安装的问题出在哪里了，就是没有指定efi分区。kde安装时我选择最小安装。

根据 [KDE - deb wiki](https://wiki.debian.org/zh_CN/KDE) 的记录表示 Debian KDE 默认安装的是 kde-full 包，但是通过 `apt list *kde* --installed` 查询的结果中并无 kde-full,有 kde-standard、kde-plasma-desktop,说明我看 wiki 有误解，从结果来看 Debian KDE 默认安装的应该是 kde-standard（依赖于 kde-plasma-desktop）。从 [kde-standard - deb pkg](https://packages.debian.org/bullseye/kde-standard) 中可以看出 kde-standard 比其依赖包 kde-plasma-desktop 增加了 15 个包，归档工具 ark、图像查看器 gwenview、截图 spectacle、文档浏览 okular 是自己之前在 Arch KDE 中用的比较多的，虽然多了也些很少使用的包（有一个原因是不知道用来干啥、咋用，更多是没需求）。其中自己不需要的有视频播放器 dragonplayer、音频播放器 juk、高级文本编辑器 kate、邮件 kmail、钱包管理 kwalletmanager、磁盘刻录 k3b、系统清理器 sweeper,播放器可以整个 vlc,国内的在线音乐可以用 listen1,文本编辑器 KDE 中还有的 Kwrite。

<details>
<summary>System Info</summary>


```bash
kearney@debian:~$ uname -a
Linux debian 5.10.0-8-amd64 #1 SMP Debian 5.10.46-4 (2021-08-03) x86_64 GNU/Linux

$ uname -a  # 2022-09-16
Linux 82dm 5.10.0-18-amd64 #1 SMP Debian 5.10.140-1 (2022-09-02) x86_64 GNU/Linux

$ inxi --admin --verbosity=7 --filter --width
System:
  Kernel: 5.10.0-18-amd64 x86_64 bits: 64 compiler: gcc v: 10.2.1 
  parameters: BOOT_IMAGE=/boot/vmlinuz-5.10.0-18-amd64 
  root=UUID=687a53e3-f5da-4b87-a86d-a6349824a419 ro quiet splash 
  Desktop: KDE Plasma 5.20.5 tk: Qt 5.15.2 wm: kwin_x11 dm: SDDM 
  Distro: Debian GNU/Linux 11 (bullseye) 
Machine:
  Type: Laptop System: LENOVO product: 82DM v: Lenovo XiaoXinPro-13ARE 2020 
  Chassis: type: 10 v: Lenovo XiaoXinPro-13ARE 2020 
  Mobo: LENOVO model: LNVNB161216 v: SDK0L77769WIN 
  UEFI: LENOVO v: F0CN36WW date: 05/16/2022 
Battery:
  ID-1: BAT0 charge: 49.0 Wh  volts: 12.9/11.5 
  model: SMP L19M3PD3 type: Li-poly status: Full cycles: 247 
Memory:
  RAM: total: 15.06 GiB used: 3.19 GiB (21.2%) 
  RAM Report: permissions: Unable to run dmidecode. Root privileges required. 
CPU:
  Info: 6-Core model: AMD Ryzen 5 4600U with Radeon Graphics bits: 64 
  type: MT MCP arch: Zen 2 family: 17 (23) model-id: 60 (96) stepping: 1 
  microcode: 8600106 L2 cache: 3 MiB bogomips: 50304 
  Speed: 1396 MHz min/max: 1400/2100 MHz boost: disabled Core speeds (MHz): 
  1: 1396 2: 1397 3: 1397 4: 1397 5: 1400 6: 1397 7: 1397 8: 1397 9: 1397 
  10: 1397 11: 1397 12: 1395 
  Flags: 3dnowprefetch abm adx aes aperfmperf apic arat avic avx avx2 bmi1 
  bmi2 bpext cat_l3 cdp_l3 clflush clflushopt clwb clzero cmov cmp_legacy 
  constant_tsc cpb cpuid cqm cqm_llc cqm_mbm_local cqm_mbm_total cqm_occup_llc 
  cr8_legacy cx16 cx8 de decodeassists extapic extd_apicid f16c flushbyasid 
  fma fpu fsgsbase fxsr fxsr_opt ht hw_pstate ibpb ibrs ibs irperf lahf_lm 
  lbrv lm mba mca mce misalignsse mmx mmxext monitor movbe msr mtrr mwaitx 
  nonstop_tsc nopl npt nrip_save nx osvw overflow_recov pae pat pausefilter 
  pclmulqdq pdpe1gb perfctr_core perfctr_llc perfctr_nb pfthreshold pge pni 
  popcnt pse pse36 rdpid rdpru rdrand rdseed rdt_a rdtscp rep_good sep sha_ni 
  skinit smap smca smep ssbd sse sse2 sse4_1 sse4_2 sse4a ssse3 stibp succor 
  svm svm_lock syscall tce topoext tsc tsc_scale umip v_vmsave_vmload vgif 
  vmcb_clean vme vmmcall wbnoinvd wdt xgetbv1 xsave xsavec xsaveerptr xsaveopt 
  xsaves 
  Vulnerabilities: Type: itlb_multihit status: Not affected 
  Type: l1tf status: Not affected 
  Type: mds status: Not affected 
  Type: meltdown status: Not affected 
  Type: mmio_stale_data status: Not affected 
  Type: retbleed 
  mitigation: untrained return thunk; SMT enabled with STIBP protection 
  Type: spec_store_bypass 
  mitigation: Speculative Store Bypass disabled via prctl and seccomp 
  Type: spectre_v1 
  mitigation: usercopy/swapgs barriers and __user pointer sanitization 
  Type: spectre_v2 mitigation: Retpolines, IBPB: conditional, STIBP: 
  always-on, RSB filling, PBRSB-eIBRS: Not affected 
  Type: srbds status: Not affected 
  Type: tsx_async_abort status: Not affected 
Graphics:
  Device-1: AMD Renoir vendor: Lenovo driver: amdgpu v: kernel bus ID: 03:00.0 
  chip ID: 1002:1636 class ID: 0300 
  Device-2: Chicony Integrated Camera type: USB driver: uvcvideo bus ID: 3-2:2 
  chip ID: 04f2:b67c class ID: 0e02 
  Display: x11 server: X.Org 1.20.11 compositor: kwin_x11 driver: 
  loaded: amdgpu,ati unloaded: fbdev,modesetting,vesa display ID: :0 
  screens: 1 
  Screen-1: 0 s-res: 2560x1600 s-dpi: 96 s-size: 677x423mm (26.7x16.7") 
  s-diag: 798mm (31.4") 
  Monitor-1: eDP res: 2560x1600 hz: 60 dpi: 227 size: 286x179mm (11.3x7.0") 
  diag: 337mm (13.3") 
  OpenGL: renderer: AMD RENOIR (DRM 3.40.0 5.10.0-18-amd64 LLVM 11.0.1) 
  v: 4.6 Mesa 20.3.5 direct render: Yes 
Audio:
  Device-1: AMD vendor: Lenovo driver: snd_hda_intel v: kernel bus ID: 03:00.1 
  chip ID: 1002:1637 class ID: 0403 
  Device-2: AMD Raven/Raven2/FireFlight/Renoir Audio Processor vendor: Lenovo 
  driver: N/A alternate: snd_pci_acp3x, snd_rn_pci_acp3x bus ID: 03:00.5 
  chip ID: 1022:15e2 class ID: 0480 
  Device-3: AMD Family 17h HD Audio vendor: Lenovo driver: snd_hda_intel 
  v: kernel bus ID: 03:00.6 chip ID: 1022:15e3 class ID: 0403 
  Sound Server: ALSA v: k5.10.0-18-amd64 
Network:
  Device-1: Realtek RTL8822CE 802.11ac PCIe Wireless Network Adapter 
  vendor: Lenovo driver: rtw_8822ce v: N/A modules: rtw88_8822ce port: 2000 
  bus ID: 01:00.0 chip ID: 10ec:c822 class ID: 0280 
  IF: wlp1s0 state: up mac: <filter> 
  IP v4: <filter> type: dynamic noprefixroute scope: global 
  broadcast: <filter> 
  IP v6: <filter> type: noprefixroute scope: link 
  WAN IP: <filter> 
Bluetooth:
  Device-1: Realtek Bluetooth Radio type: USB driver: btusb v: 0.8 
  bus ID: 1-4:2 chip ID: 0bda:c123 class ID: e001 
  Report: ID: hci0 state: up running bt-v: 3.0 lmp-v: 5.1 sub-v: 7253 
  hci-v: 5.1 rev: 99a address: <filter> 
  Info: acl-mtu: 1021:6 sco-mtu: 255:12 link-policy: rswitch hold sniff park 
  link-mode: slave accept service-classes: object transfer 
RAID:
  Message: No RAID data was found. 
Drives:
  Local Storage: total: 476.94 GiB used: 205.45 GiB (43.1%) 
  SMART Message: Unable to run smartctl. Root privileges required. 
  ID-1: /dev/nvme0n1 maj-min: 259:0 vendor: Western Digital 
  model: PC SN730 SDBPNTY-512G-1101 size: 476.94 GiB block size: 
  physical: 512 B logical: 512 B speed: 31.6 Gb/s lanes: 4 rotation: SSD 
  rev: 11130001 temp: 37.9 C scheme: GPT 
  Message: No Optical or Floppy data was found. 
Partition:
  ID-1: / raw size: 91.39 GiB size: 89.39 GiB (97.82%) used: 10.77 GiB (12.0%) 
  fs: ext4 dev: /dev/nvme0n1p4 maj-min: 259:4 label: N/A 
  uuid: 687a53e3-f5da-4b87-a86d-a6349824a419 
  ID-2: /boot/efi raw size: 260 MiB size: 256 MiB (98.46%) 
  used: 111.3 MiB (43.5%) fs: vfat dev: /dev/nvme0n1p1 maj-min: 259:1 
  label: SYSTEM_DRV uuid: 92C4-DD2F 
  ID-3: /home raw size: 28.61 GiB size: 27.99 GiB (97.84%) 
  used: 497.2 MiB (1.7%) fs: ext4 dev: /dev/nvme0n1p2 maj-min: 259:2 
  label: N/A uuid: c8a1c404-992c-4102-8881-80cd3af0d03f 
  ID-4: /media/kearney/a raw size: 235.67 GiB size: 231.41 GiB (98.19%) 
  used: 194.08 GiB (83.9%) fs: ext4 dev: /dev/nvme0n1p3 maj-min: 259:3 
  label: a uuid: 3409b138-e5fc-412d-b1d5-bfb236f93257 
Swap:
  Alert: No Swap data was found. 
Unmounted:
  ID-1: /dev/nvme0n1p5 maj-min: 259:5 size: 55 GiB fs: ntfs label: C 
  uuid: 0AB232A4B2329463 
  ID-2: /dev/nvme0n1p6 maj-min: 259:6 size: 65.98 GiB fs: ntfs label: D 
  uuid: 127AE7467AE72567 
USB:
  Hub-1: 1-0:1 info: Full speed (or root) Hub ports: 4 rev: 2.0 
  speed: 480 Mb/s chip ID: 1d6b:0002 class ID: 0900 
  Device-1: 1-4:2 info: Realtek Bluetooth Radio type: Bluetooth driver: btusb 
  interfaces: 2 rev: 1.0 speed: 12 Mb/s chip ID: 0bda:c123 class ID: e001 
  
  Hub-2: 2-0:1 info: Full speed (or root) Hub ports: 2 rev: 3.1 speed: 10 Gb/s 
  chip ID: 1d6b:0003 class ID: 0900 
  Hub-3: 3-0:1 info: Full speed (or root) Hub ports: 4 rev: 2.0 
  speed: 480 Mb/s chip ID: 1d6b:0002 class ID: 0900 
  Device-1: 3-2:2 info: Chicony Integrated Camera type: Video driver: uvcvideo 
  interfaces: 4 rev: 2.0 speed: 480 Mb/s chip ID: 04f2:b67c class ID: 0e02 
  
  Hub-4: 4-0:1 info: Full speed (or root) Hub ports: 2 rev: 3.1 speed: 10 Gb/s 
  chip ID: 1d6b:0003 class ID: 0900 
Sensors:
  System Temperatures: cpu: 54.9 C mobo: N/A gpu: amdgpu temp: 44.0 C 
  Fan Speeds (RPM): N/A 
Info:
  Processes: 258 Uptime: 30m wakeups: 1 Init: systemd v: 247 runlevel: 5 
  Compilers: gcc: 10.2.1 alt: 10 Packages: apt: 2702 lib: 1390 Shell: Bash 
  v: 5.1.4 running in: codium inxi: 3.3.01

# /lib/firmware/
$ tree rtl_bt rtlwifi
rtl_bt
├── rtl8192ee_fw.bin
├── rtl8192eu_fw.bin
├── rtl8723a_fw.bin
├── rtl8723b_fw.bin
├── rtl8723bs_config-OBDA8723.bin
├── rtl8723bs_fw.bin
├── rtl8723d_config.bin
├── rtl8723d_fw.bin
├── rtl8761a_fw.bin
├── rtl8812ae_fw.bin
├── rtl8821a_fw.bin
├── rtl8821c_config.bin
├── rtl8821c_fw.bin
├── rtl8822b_config.bin
├── rtl8822b_fw.bin
├── rtl8822cs_config.bin
├── rtl8822cs_fw.bin
├── rtl8822cu_config.bin
├── rtl8822cu_fw.bin
├── rtl8852au_config.bin
└── rtl8852au_fw.bin
rtlwifi
├── rtl8188efw.bin
├── rtl8188eufw.bin
├── rtl8192cfw.bin
├── rtl8192cfwU_B.bin
├── rtl8192cfwU.bin
├── rtl8192cufw_A.bin
├── rtl8192cufw_B.bin
├── rtl8192cufw.bin
├── rtl8192cufw_TMSC.bin
├── rtl8192defw.bin
├── rtl8192eefw.bin
├── rtl8192eu_ap_wowlan.bin
├── rtl8192eu_nic.bin
├── rtl8192eu_wowlan.bin
├── rtl8192sefw.bin
├── rtl8712u.bin
├── rtl8723aufw_A.bin
├── rtl8723aufw_B.bin
├── rtl8723aufw_B_NoBT.bin
├── rtl8723befw_36.bin
├── rtl8723befw.bin
├── rtl8723bs_ap_wowlan.bin
├── rtl8723bs_bt.bin
├── rtl8723bs_nic.bin
├── rtl8723bs_wowlan.bin
├── rtl8723bu_ap_wowlan.bin
├── rtl8723bu_nic.bin
├── rtl8723bu_wowlan.bin
├── rtl8723defw.bin
├── rtl8723fw_B.bin
├── rtl8723fw.bin
├── rtl8812aefw.bin
├── rtl8812aefw_wowlan.bin
├── rtl8821aefw_29.bin
├── rtl8821aefw.bin
├── rtl8821aefw_wowlan.bin
└── rtl8822befw.bin

0 directories, 58 files
```
</details>

# 系统微调
## 输入设备

触摸板开启轻触点击功能，不用再摁下实体按键了。高分辨率调节屏幕缩放。连接蓝牙设备。修改主题颜色、图标

## apt 更新系统

```bash
$ sudo apt update
[sudo] kearney 的密码：
kearney 不在 sudoers 文件中。此事将被报告。
```

这个问题可以在设置-用户-帐号类型中解决，将“标准”改为“管理员”

```bash
$ sudo apt update
$ sudo apt upgrade
```

## 添加中文输入法

如果安装系统时，语言选择中文都会自动安装中文输入法；当然有时候我安装时会选择英语，所有默认没有中文输入法。默认安装了fcitx和fcitx5,不是很理解两个都装干啥，arch wiki都推荐使用fcitx5来替代fcitx了，于是卸载了fcitx,然后打开fcitx5添加新的输入法，敲如chi会出现“汉语-Mongolian（Bichig）”，但是添加这个输入法之后打出来的明显不是中文，于是把这个假冒汉语删除掉，把添加输入法左下角的“Only Show Current Language"给取消掉勾选，然后输入法列表里会显示出更多的输入法，拉到最底部就可以看到简体中文了，有拼音双拼五笔，选中之后添加再确认即可。要是高分辨率屏幕的话，再设置下字体大小啥的。最后把fcitx5加入自启动项目中。

但是随着使用，发现在kwrite、dolphin中无法使用中文输入法，然而在codium、firefox却能够正常使用。说明某个地方还需要进一步配置。而按照arch wiki的配置了之后连codium也无法输入中文了，测试了其它软件、重启、关机开机也不行，最后删除配置重新登录又可以了，这下子kwrite也能使用了，很奇怪啊，跟当初相比的差别在于现在多了一个空白文件 .bash_profile

## Discover

不知道为啥Debian-KDE中有了 Discover, 还要再装一个 Apper，这下子不就有俩个软件中心了吗？把Apper卸载掉。当然还有一些我不用的也删除掉（Dragon Player、Juk、K3b、LibreOffice、Kontrast、GIMP），Discover只能逐个卸载逐个输入密码真的是心累，这下子我有点怀念Manjaro的pamac了，还能显示依赖，连依赖也一起清楚，搜索了下发现apt autoremove可能会破坏系统，谨慎一些先不用。怀念pamac的依赖查询和依赖清除。

有些软件在 Discover、Apper 不好找，命令行安装方式也比较烦琐（如vs code），可以在 Discover 中搜索 discover,安装 Snap backend、Flatpak backend 了，Discover 会安装对于的软件包，之后就可以直接在 Discover中搜索 flathub、snap 里的包了。要是再来个 AUR 后端就好了。

观察和搜索了下 appimage、flathub、snap 的几个跨发行版软件仓库，snap 打包完比较庞大，appimage 目前是最方便的

也不喜欢 snap 在跟目录下额外创建了一个文件目录，opt 不行吗

但是我安装Ubuntu 22LTS后转到Debian就是不喜欢默认设置的Snap，deb或者源码编译也行啊，整这种虚的干啥，体积大，效率也有损失。Discover搜索不到wps、tencent，得自己下载deb安装，这一点AUR做得比较出色

## MPR

一个类似 AUR 的玩意 [MPR](https://mpr.makedeb.org)，但是22.9.16搜索还是没有tencent，腾讯会议官方已经发布了linux版本了

## codium

[vscodium](https://vscodium.com/#install)页面的针对 Debian 的安装办法，如果提示 “wget：未找到命令“ 则要安装 wget(sudo apt install wget)，当然手动下载 deb 包进行安装也行，比如 [github release](https://github.com/VSCodium/vscodium/releases)、[TUNA Mirror](https://mirrors.tuna.tsinghua.edu.cn/github-release/VSCodium/vscodium/LatestRelease/)

```bash
wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg \
    | gpg --dearmor \
    | sudo dd of=/usr/share/keyrings/vscodium-archive-keyring.gpg

echo 'deb [ signed-by=/usr/share/keyrings/vscodium-archive-keyring.gpg ] https://download.vscodium.com/debs vscodium main' \
    | sudo tee /etc/apt/sources.list.d/vscodium.list

sudo apt update && sudo apt install codium
```

在xjtu测试下官方办法安装还算顺利

# python

Debian 11 Bullseye 中已经除去了 py2，只有一个 py3，无 pip。[Python - deb wiki](https://wiki.debian.org/Python?action=show&redirect=DebianPython) 中也有对此的说明，需要注意的是没有建立动态链接 /usr/bin/python 到 py3，因此直接敲 python 是无效的。

下面安装 pip3 并配置镜像源，以后使用 pip3 之前要先建立一个虚拟环境，不然最后就是列出来一大堆。。。

```bash
kearney@debian:~$ python3 -V
Python 3.9.2
kearney@debian:~$ sudo apt install python3-pip

$ pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
$ pip config set install.trusted-host https://pypi.tuna.tsinghua.edu.cn
# 在 Debian 上不装它用不了 venv 模块。。。
$ sudo apt install python3-venv
# 不需要安装 pip 也能在 venv 中使用 pip
# why deb、arch 这些发行版的 python 默认都不带 pip 呢？
$ sudo apt remove python3-pip
```

# 参考

1. [安装debian 9.1后，中文环境下将home目录下文件夹改为对应的英文](https://www.cnblogs.com/samgg/p/7501199.html):
  ```
  #安装需要的软件
  sudo apt install xdg-user-dirs-gtk
  #临时转换系统语言为英文，重启后会自动恢复原值的
  export LANG=en_US
  #执行转换命令，弹出的窗口中会询问是否将目录转化为英文路径，同意并关闭
  xdg-user-dirs-gtk-update
  #转换回系统语言为中文，也可以不执行下面的命令，直接重启也一样的
  export LANG=zh_CN
  ```