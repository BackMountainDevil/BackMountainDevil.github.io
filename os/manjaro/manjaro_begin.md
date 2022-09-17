# Manjaro 安装系统与配置
- date: 2021-09-05
- lastmod: 2022-09-17

# 安装系统

manjaro-kde-21.1.1-minimal-210827-linux54.iso lts 版本安装一直卡在 boot 之后的终端界面，到达 reach graphic interface 就不动了。下载 manjaro-kde-21.1.1-minimal-210827-linux513.iso 又可以安装了。

pamac 具有Alpm、AUR、Flatpak和Snap支持的软件包管理器
這下子可以在同一個搜索框框裏直接搜索多個庫中的 pkg 了。有 AUR 了還要啥 alatpak 和 Flatpak和Snap

2022-09-17 可以安装 manjaro-kde-21.1.1-minimal-210827-linux513.iso,但是 pamac 更新会显示"无效或已损坏的软件包"；使用 sudo pacman -Syyu 看起来是可以更新，然而在提示一大堆“签名是勉强信任的、文件已损坏 (无效或已损坏的软件包 (PGP 签名))、签名是未知信任的、是否添加GPG、是否删除已损坏的...”，最后都是面临无法更新的问题 - “错误：无法提交处理 (无效或已损坏的软件包 (PGP 签名)) 发生错误，没有软件包被更新。“。 或许这就是传说中的滚挂，不过并没有完全挂，已经安装的软件可以用，只是无法安装新软件/升级。于是赶紧下载当时最新的 manjaro-kde-21.3.7-minimal-220816-linux515.iso，然而无法进入安装界面，下载好和复制后都进行了SHA1校验的。。。重新校验之后发现文件损坏了，既然原因找到了，不过我重下的是 manjaro-kde-21.3.7-220816-linux515.iso ，下载好和复制后都SHA1校验正常。安装语言新手选中文好一点，不过中文输入法还是要自己安装。

分区这回我把 /home 和 / 分开装，这样重装系统还可以保留 /home 下的文件

Mnajaro 安装后的用户账户类型是管理员，而 Debian 安装后则的用户账户类型是标准。

<details>
<summary>System Info</summary>


奇怪的是 Network、Bluetooth 的 driver 和 Debian 一样，但是 Debian 却不支持额的蓝牙耳机。。。
```bash
$ uname -a
Linux 82dm 5.15.65-1-MANJARO #1 SMP PREEMPT Mon Sep 5 10:15:47 UTC 2022 x86_64 GNU/Linux

$ inxi --admin --verbosity=7 --filter --width
System:
  Kernel: 5.15.65-1-MANJARO arch: x86_64 bits: 64 compiler: gcc v: 12.2.0
    parameters: BOOT_IMAGE=/boot/vmlinuz-5.15-x86_64
    root=UUID=5d6e2a51-4876-44ba-9f6e-acc3e0bf2767 rw quiet
    udev.log_priority=3
  Desktop: KDE Plasma v: 5.25.5 tk: Qt v: 5.15.5 wm: kwin_x11 vt: 1 dm: SDDM
    Distro: Manjaro Linux base: Arch Linux
Machine:
  Type: Laptop System: LENOVO product: 82DM v: Lenovo XiaoXinPro-13ARE 2020
     Chassis: type: 10 v: Lenovo XiaoXinPro-13ARE
    2020 
  Mobo: LENOVO model: LNVNB161216 v: SDK0L77769WIN
     UEFI: LENOVO v: F0CN36WW date: 05/16/2022
Battery:
  ID-1: BAT0 charge: 49.0 Wh (100.0%) 
    volts: 12.9 min: 11.5 model: SMP L19M3PD3 type: Li-poly 
    status: full cycles: 247
Memory:
  RAM: total: 14.99 GiB used: 3.67 GiB (24.5%)
  RAM Report: permissions: Unable to run dmidecode. Root privileges
    required.
CPU:
  Info: model: AMD Ryzen 5 4600U with Radeon Graphics bits: 64 type: MT MCP
    arch: Zen 2 gen: 3 level: v3 built: 2020-22 process: TSMC n7 (7nm)
    family: 0x17 (23) model-id: 0x60 (96) stepping: 1 microcode: 0x8600106
  Topology: cpus: 1x cores: 6 tpc: 2 threads: 12 smt: enabled cache:
    L1: 384 KiB desc: d-6x32 KiB; i-6x32 KiB L2: 3 MiB desc: 6x512 KiB L3: 8 MiB
    desc: 2x4 MiB
  Speed (MHz): avg: 1507 high: 2292 min/max: 1400/2100 boost: disabled
    scaling: driver: acpi-cpufreq governor: schedutil cores: 1: 1397 2: 1397
    3: 2292 4: 1587 5: 1565 6: 1397 7: 1397 8: 1474 9: 1397 10: 1397 11: 1398
    12: 1392 bogomips: 50320
  Flags: 3dnowprefetch abm adx aes aperfmperf apic arat avic avx avx2 bmi1
    bmi2 bpext cat_l3 cdp_l3 clflush clflushopt clwb clzero cmov cmp_legacy
    constant_tsc cpb cpuid cqm cqm_llc cqm_mbm_local cqm_mbm_total
    cqm_occup_llc cr8_legacy cx16 cx8 de decodeassists extapic extd_apicid
    f16c flushbyasid fma fpu fsgsbase fxsr fxsr_opt ht hw_pstate ibpb ibrs ibs
    irperf lahf_lm lbrv lm mba mca mce misalignsse mmx mmxext monitor movbe
    msr mtrr mwaitx nonstop_tsc nopl npt nrip_save nx osvw overflow_recov pae
    pat pausefilter pclmulqdq pdpe1gb perfctr_core perfctr_llc perfctr_nb
    pfthreshold pge pni popcnt pse pse36 rapl rdpid rdpru rdrand rdseed rdt_a
    rdtscp rep_good sep sha_ni skinit smap smca smep ssbd sse sse2 sse4_1
    sse4_2 sse4a ssse3 stibp succor svm svm_lock syscall tce topoext tsc
    tsc_scale umip v_spec_ctrl v_vmsave_vmload vgif vmcb_clean vme vmmcall
    wbnoinvd wdt xgetbv1 xsave xsavec xsaveerptr xsaveopt xsaves
  Vulnerabilities:
  Type: itlb_multihit status: Not affected
  Type: l1tf status: Not affected
  Type: mds status: Not affected
  Type: meltdown status: Not affected
  Type: mmio_stale_data status: Not affected
  Type: retbleed mitigation: untrained return thunk; SMT enabled with STIBP
    protection
  Type: spec_store_bypass mitigation: Speculative Store Bypass disabled via
    prctl and seccomp
  Type: spectre_v1 mitigation: usercopy/swapgs barriers and __user pointer
    sanitization
  Type: spectre_v2 mitigation: Retpolines, IBPB: conditional, STIBP:
    always-on, RSB filling, PBRSB-eIBRS: Not affected
  Type: srbds status: Not affected
  Type: tsx_async_abort status: Not affected
Graphics:
  Device-1: AMD Renoir vendor: Lenovo driver: amdgpu v: kernel arch: GCN-5.1
    code: Vega-2 process: TSMC n7 (7nm) built: 2018-21 pcie: gen: 4
    speed: 16 GT/s lanes: 16 ports: active: eDP-1 empty: DP-1,DP-2
    bus-ID: 03:00.0 chip-ID: 1002:1636 class-ID: 0300
  Device-2: Chicony Integrated Camera type: USB driver: uvcvideo
    bus-ID: 3-2:2 chip-ID: 04f2:b67c class-ID: 0e02 
  Display: x11 server: X.Org v: 21.1.4 compositor: kwin_x11 driver: X:
    loaded: amdgpu unloaded: modesetting alternate: fbdev,vesa gpu: amdgpu
    display-ID: :0 screens: 1
  Screen-1: 0 s-res: 2560x1600 s-dpi: 96 s-size: 677x423mm (26.65x16.65")
    s-diag: 798mm (31.43")
  Monitor-1: eDP-1 mapped: eDP model-id: CSO 0x076d built: 2019
    res: 2560x1600 hz: 60 dpi: 227 gamma: 1.2 size: 286x179mm (11.26x7.05")
    diag: 337mm (13.3") ratio: 16:10 modes: max: 2560x1600 min: 640x480
  OpenGL: renderer: AMD RENOIR (LLVM 14.0.6 DRM 3.42 5.15.65-1-MANJARO) v: 4.6
    Mesa 22.1.7 direct render: Yes
Audio:
  Device-1: AMD Renoir Radeon High Definition Audio vendor: Lenovo
    driver: snd_hda_intel v: kernel pcie: gen: 4 speed: 16 GT/s lanes: 16
    bus-ID: 03:00.1 chip-ID: 1002:1637 class-ID: 0403
  Device-2: AMD ACP/ACP3X/ACP6x Audio Coprocessor vendor: Lenovo driver: N/A
    alternate: snd_pci_acp3x, snd_rn_pci_acp3x, snd_pci_acp5x pcie: gen: 4
    speed: 16 GT/s lanes: 16 bus-ID: 03:00.5 chip-ID: 1022:15e2 class-ID: 0480
  Device-3: AMD Family 17h/19h HD Audio vendor: Lenovo driver: snd_hda_intel
    v: kernel pcie: gen: 4 speed: 16 GT/s lanes: 16 bus-ID: 03:00.6
    chip-ID: 1022:15e3 class-ID: 0403
  Sound Server-1: ALSA v: k5.15.65-1-MANJARO running: yes
  Sound Server-2: JACK v: 1.9.21 running: no
  Sound Server-3: PulseAudio v: 16.1 running: yes
  Sound Server-4: PipeWire v: 0.3.57 running: yes
Network:
  Device-1: Realtek RTL8822CE 802.11ac PCIe Wireless Network Adapter
    vendor: Lenovo driver: rtw_8822ce v: N/A modules: rtw88_8822ce pcie: gen: 1
    speed: 2.5 GT/s lanes: 1 port: 2000 bus-ID: 01:00.0 chip-ID: 10ec:c822
    class-ID: 0280
  IF: wlp1s0 state: up mac: <filter>
  IP v4: <filter> type: dynamic noprefixroute scope: global
    broadcast: <filter>
  IP v6: <filter> type: noprefixroute scope: link
  WAN IP: <filter>
Bluetooth:
  Device-1: Realtek Bluetooth Radio type: USB driver: btusb v: 0.8
    bus-ID: 1-4:2 chip-ID: 0bda:c123 class-ID: e001 
  Report: rfkill ID: hci0 rfk-id: 2 state: up address: see --recommends
Logical:
  Message: No logical block device data found.
RAID:
  Message: No RAID data found.
Drives:
  Local Storage: total: 476.94 GiB used: 208.09 GiB (43.6%)
  SMART Message: Unable to run smartctl. Root privileges required.
  ID-1: /dev/nvme0n1 maj-min: 259:0 vendor: Western Digital model: PC SN730
    SDBPNTY-512G-1101 size: 476.94 GiB block-size: physical: 512 B
    logical: 512 B speed: 31.6 Gb/s lanes: 4 type: SSD 
    rev: 11130001 temp: 36.9 C scheme: GPT
  Message: No optical or floppy data found.
Partition:
  ID-1: / raw-size: 40 GiB size: 39.08 GiB (97.69%) used: 13.42 GiB (34.4%)
    fs: ext4 dev: /dev/nvme0n1p2 maj-min: 259:2 label: root
    uuid: 5d6e2a51-4876-44ba-9f6e-acc3e0bf2767
  ID-2: /boot/efi raw-size: 260 MiB size: 256 MiB (98.46%) used: 103.5 MiB
    (40.4%) fs: vfat dev: /dev/nvme0n1p1 maj-min: 259:1 label: SYSTEM_DRV
    uuid: 92C4-DD2F
  ID-3: /home raw-size: 80 GiB size: 78.19 GiB (97.74%) used: 815 MiB (1.0%)
    fs: ext4 dev: /dev/nvme0n1p4 maj-min: 259:4 label: N/A
    uuid: a3a2ba69-8213-4723-ba8d-b4b8470dd7eb
  ID-4: /run/media/kearney/a raw-size: 235.67 GiB size: 231.41 GiB (98.19%)
    used: 193.77 GiB (83.7%) fs: ext4 dev: /dev/nvme0n1p3 maj-min: 259:3
    label: a uuid: 3409b138-e5fc-412d-b1d5-bfb236f93257
Swap:
  Alert: No swap data was found.
Unmounted:
  ID-1: /dev/nvme0n1p5 maj-min: 259:5 size: 55 GiB fs: ntfs label: C
    uuid: 0AB232A4B2329463
  ID-2: /dev/nvme0n1p6 maj-min: 259:6 size: 65.98 GiB fs: ntfs label: D
    uuid: 127AE7467AE72567
USB:
  Hub-1: 1-0:1 info: Hi-speed hub with single TT ports: 4 rev: 2.0
    speed: 480 Mb/s chip-ID: 1d6b:0002 class-ID: 0900
  Device-1: 1-4:2 info: Realtek Bluetooth Radio type: Bluetooth
    driver: btusb interfaces: 2 rev: 1.0 speed: 12 Mb/s power: 500mA
    chip-ID: 0bda:c123 class-ID: e001 
  Hub-2: 2-0:1 info: Super-speed hub ports: 2 rev: 3.1 speed: 10 Gb/s
    chip-ID: 1d6b:0003 class-ID: 0900
  Hub-3: 3-0:1 info: Hi-speed hub with single TT ports: 4 rev: 2.0
    speed: 480 Mb/s chip-ID: 1d6b:0002 class-ID: 0900
  Device-1: 3-2:2 info: Chicony Integrated Camera type: Video
    driver: uvcvideo interfaces: 4 rev: 2.0 speed: 480 Mb/s power: 500mA
    chip-ID: 04f2:b67c class-ID: 0e02 
  Hub-4: 4-0:1 info: Super-speed hub ports: 2 rev: 3.1 speed: 10 Gb/s
    chip-ID: 1d6b:0003 class-ID: 0900
Sensors:
  System Temperatures: cpu: 49.2 C mobo: N/A gpu: amdgpu temp: 43.0 C
  Fan Speeds (RPM): N/A
Info:
  Processes: 302 Uptime: 2h 48m wakeups: 1 Init: systemd v: 251
  default: graphical tool: systemctl Compilers: gcc: 12.2.0 clang: 14.0.6
  Packages: pm: pacman pkgs: 1235 libs: 318 tools: pamac pm: flatpak pkgs: 0
  Shell: Bash v: 5.1.16 running-in: codium inxi: 3.3.21
```
</details>

# 系统微调
## 系统更新

[Pacman-mirrors](https://wiki.manjaro.org/index.php/Pacman-mirrors/zh-cn)
> 更新 5个镜像列表，以获得最快的镜像,并更新系统
    sudo pacman-mirrors --fasttrack 5 && sudo pacman -Syyu

有时候更新的情况不是很符合我个人的喜好(在 xjtu 下测试有时候只能获取到一个国内镜像站)，我喜欢把 ustc 和 tuna 排在前面，可以修改文件 /etc/pacman.d/mirrorlist 中的顺序

[Manjaro Linux 源使用帮助](https://mirrors.ustc.edu.cn/help/manjaro.html): sudo pacman-mirrors -i -c China -m rank

## 系统启动选择菜单 grub2

配置文件为 `/etc/default/grub`，每次修改后需要 `sudo update-grub` 使更新生效。

GRUB_TIMEOUT 等待用户操作的超时时间（秒），0 -> 10， -1 -> 无穷。目前我设置 1

## 主目录下文件夹中文名称改为英文

不更改不影响系统正常运行和使用，只影响 teminal 中的 tab 补全功能。

- [Manjaro主目录下文件夹由中文改为英文. thepoy. 2020.01.31](https://www.jianshu.com/p/2f05a5583609)

```bash
sudo pacman -S xdg-user-dirs-gtk
export LANG=en_US
xdg-user-dirs-gtk-update	# 更新语言

sudo pacman -Rs xdg-user-dirs-gtk
```

## 中文输入法

- [Fcitx5](https://wiki.archlinux.org/title/Fcitx5_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E8%BE%93%E5%85%A5%E6%B3%95%E6%A8%A1%E5%9D%97)
> `sudo pacman -S fcitx5-im fcitx5-chinese-addons`

不得不说 deian 默认配置好了中文输入法真舒服，manjaro 虽然安装选择了区域，但也只是做了语言包的下载替换没有整对于的输入法。

中文 wiki 里是这么做的，编辑 `~/.bash_profile` 文件，写入下面的内容，但是我测试感觉还是无法在 dolphin、kwrite、wps、konsole 中输入中文，在 firefox、codium 中是可以的

```bash
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=@im=fcitx
INPUT_METHOD  DEFAULT=fcitx
SDL_IM_MODULE DEFAULT=fcitx
```

# FAQ
## 关机显示 Backlight Brightness

[AMD 安装 Manjaro KDE 驱动安装后续及BackLight:ACPI故障解决. grsharp. 2020-04-24](https://blog.csdn.net/grsharp/article/details/105735792)
> boot启动显示错误信息Failed to start Load/Save Screen Backlight Brightness of backlight:acpi_video0，虽然不影响正常体验（其实系统使用了systemd-backlight@backlight:amdgpu_b10来补充了)所以让报错消失的办法直接屏蔽该服务：
sudo systemctl mask systemd-backlight@backlight:acpi_video0

## pamac 显示无法锁定数据库 unable to lock database

[Pamac update: unable to lock database, Failed to synchronize databases ](https://forum.manjaro.org/t/pamac-update-unable-to-lock-database-failed-to-synchronize-databases/114762)
> pamac update --force-refresh

## 关闭蜂鸣器的 Bee 声

搜索不存在、Tab补全、关机等操作 突然B一声，心脏都吓到了，参考 [PC speaker](https://wiki.archlinux.org/title/PC_speaker)关闭就行，如KDE在设置-个性化/无障碍辅助-声音提示 取消启用

## 开机启动时间分析与优化

https://forum.manjaro.org/t/manjaro-booting-is-very-slow-40sec/32489/4

```bash
systemd-analyze	# 开机时间总览
systemd-analyze blame # 开机时间细节
systemctl disable --now lvm2-monitor.service	# 关闭该服务
systemctl mask lvm2-monitor.service
```

## konsole 改 zsh 为 bash

konsole 标题显示 zsh.。编辑配置方案显示命令是 /bin/zsh。但是根据用户目录下的 history 文件显示 konsole 里敲的都记录在 `～/.zhistory`，而 vscodium（ 默认 linux 终端 bash） 敲的都记录在 `～/.bash_history.`。自己更偏向于 bash 多一些，所以就该了，当然使用 zsh 也没有啥太大问题。如果要卸载 zsh,还会有点小小问题，因此我习惯于保留 zsh 和 bash。

[How to find which shell I am using on Linux. July 7, 2020 by Dan Nanni](https://www.xmodulo.com/which-shell-am-i-using.html):echo $0

切换bash： chsh -s /bin/bash
切换zsh： chsh -s /bin/zsh
    在终端app的系统偏好设置里手动设置。

在配置文件方面：

    bash读取的配置文件：~/.bash_profile文件
    zsh读取的配置文件：~/.zshrc文件

tab 补全：zsh 会在列出来的可选名称自动遍历替换，而 bash 只是把可选列出来，确定好所选后都可以遍历下一级

VS Codium 在终端+的下拉箭头很容易选择zsh,有的时候会显示图标乱码，说明字体包不全或者设置不对，但是 konsole zsh在同一目录下显示正常，说明字体包齐全，是 codium zsh 配置字体样式的问题，在 editor font family 最前加入 Hack 还是不太行。


# Refer

- [ Manjaro Linux 踩坑调教记录 2019-11-23 PRIN BLOG](https://prinsss.github.io/setting-up-manjaro-linux/)