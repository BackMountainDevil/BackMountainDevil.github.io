# KDE Plasma Desktop freeze
- date: 2022-10-18
- lastmod: 2022-10-26

我在[endeavouros论坛的发帖提问](https://forum.endeavouros.com/t/kde-plasma-desktop-freeze/32921)

# 10.18 桌面卡住
## problem
My latop freeze when I am setup webdav on openwrt via ssh. I just copy from firefox web and it suddenly freeze. Last time DE freeze because qbitottent freeze. This time I kill it by kill -9 pid. not work. Then I kill the program I use(firefox, vscodium-bin, Typora, konsole/ssh) . Still freeze

Beside my lattop. There is one more pc. So I can let it stay freeze and tty2. Until figure out what happen
system info

## log

```bash
$ sudo journalctl -b -1 | eos-sendlog
https://clbin.com/zRgCS
```

## system info

```bash
$ inxi --admin --verbosity=7 --filter --width
System:
  Kernel: 5.15.74-1-lts arch: x86_64 bits: 64 compiler: gcc v: 12.2.0
    parameters: BOOT_IMAGE=/boot/vmlinuz-linux-lts
    root=UUID=ff0d5e61-82bc-425b-b4dd-2fed5ad072bc rw loglevel=3 nowatchdog
    nvme_load=YES
  Console: tty 2 DM: 1: LightDM v: 1.32.0 note: stopped 2: SDDM
    Distro: EndeavourOS base: Arch Linux
Machine:
  Type: Laptop System: LENOVO product: 82DM v: Lenovo XiaoXinPro-13ARE 2020
    serial: <superuser required> Chassis: type: 10 v: Lenovo XiaoXinPro-13ARE
    2020 serial: <superuser required>
  Mobo: LENOVO model: LNVNB161216 v: SDK0L77769WIN
    serial: <superuser required> UEFI: LENOVO v: F0CN36WW date: 05/16/2022
Battery:
  ID-1: BAT0 charge: 47.2 Wh (100.0%) condition: 47.2/56.0 Wh (84.2%)
    volts: 12.9 min: 11.5 model: SMP L19M3PD3 type: Li-poly serial: <filter>
    status: full cycles: 254
Memory:
  RAM: total: 14.99 GiB used: 5.89 GiB (39.3%)
  RAM Report: permissions: Unable to run dmidecode. Root privileges
    required.
CPU:
  Info: model: AMD Ryzen 5 4600U with Radeon Graphics bits: 64 type: MT MCP
    arch: Zen 2 gen: 3 level: v3 note: check built: 2020-22 process: TSMC n7
    (7nm) family: 0x17 (23) model-id: 0x60 (96) stepping: 1
    microcode: 0x8600106
  Topology: cpus: 1x cores: 6 tpc: 2 threads: 12 smt: enabled cache:
    L1: 384 KiB desc: d-6x32 KiB; i-6x32 KiB L2: 3 MiB desc: 6x512 KiB L3: 8 MiB
    desc: 2x4 MiB
  Speed (MHz): avg: 1397 high: 1398 min/max: 1400/2100 boost: enabled
    scaling: driver: acpi-cpufreq governor: schedutil cores: 1: 1398 2: 1397
    3: 1397 4: 1397 5: 1397 6: 1397 7: 1397 8: 1396 9: 1397 10: 1397 11: 1397
    12: 1397 bogomips: 50306
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
    bus-ID: 03:00.0 chip-ID: 1002:1636 class-ID: 0300 temp: 32.0 C
  Device-2: Chicony Integrated Camera type: USB driver: uvcvideo
    bus-ID: 3-2:2 chip-ID: 04f2:b67c class-ID: 0e02 serial: <filter>
  Display: x11 server: X.org v: 1.21.1.4 compositor: kwin_x11 driver: X:
    loaded: amdgpu unloaded: modesetting alternate: fbdev,vesa dri: radeonsi
    gpu: amdgpu tty: 160x50
  Monitor-1: eDP-1 model-id: CSO 0x076d built: 2019 res: 2560x1600 dpi: 227
    gamma: 1.2 size: 286x179mm (11.26x7.05") diag: 337mm (13.3") ratio: 16:10
    modes: max: 2560x1600 min: 640x480
  Message: GL data unavailable in console. Try -G --display
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
  Sound API: ALSA v: k5.15.74-1-lts running: yes
  Sound Server-1: PulseAudio v: 16.1 running: no
  Sound Server-2: PipeWire v: 0.3.59 running: yes
Network:
  Device-1: Realtek RTL8822CE 802.11ac PCIe Wireless Network Adapter
    vendor: Lenovo driver: rtw_8822ce v: N/A modules: rtw88_8822ce pcie: gen: 1
    speed: 2.5 GT/s lanes: 1 port: 2000 bus-ID: 01:00.0 chip-ID: 10ec:c822
    class-ID: 0280
  IF: wlan0 state: up mac: <filter>
  IP v4: <filter> type: dynamic noprefixroute scope: global
    broadcast: <filter>
  IP v6: <filter> type: dynamic noprefixroute scope: global
  IP v6: <filter> type: noprefixroute scope: global
  IP v6: <filter> type: noprefixroute scope: link
  WAN IP: <filter>
Bluetooth:
  Device-1: Realtek Bluetooth Radio type: USB driver: btusb v: 0.8
    bus-ID: 1-4:2 chip-ID: 0bda:c123 class-ID: e001 serial: <filter>
  Report: rfkill ID: hci0 rfk-id: 3 state: up address: see --recommends
Logical:
  Message: No logical block device data found.
RAID:
  Message: No RAID data found.
Drives:
  Local Storage: total: 484.49 GiB used: 195.54 GiB (40.4%)
  SMART Message: Unable to run smartctl. Root privileges required.
  ID-1: /dev/nvme0n1 maj-min: 259:0 vendor: Western Digital model: PC SN730
    SDBPNTY-512G-1101 size: 476.94 GiB block-size: physical: 512 B
    logical: 512 B speed: 31.6 Gb/s lanes: 4 type: SSD serial: <filter>
    rev: 11130001 temp: 29.9 C scheme: GPT
  ID-2: /dev/sda maj-min: 8:0 type: USB vendor: SMI (STMicroelectronics)
    model: USB DISK size: 7.55 GiB block-size: physical: 512 B logical: 512 B
    type: N/A serial: <filter> rev: 1100 scheme: MBR
  SMART Message: Unknown USB bridge. Flash drive/Unsupported enclosure?
  Message: No optical or floppy data found.
Partition:
  ID-1: / raw-size: 40 GiB size: 39.08 GiB (97.69%) used: 16.64 GiB (42.6%)
    fs: ext4 dev: /dev/nvme0n1p2 maj-min: 259:2 label: N/A
    uuid: ff0d5e61-82bc-425b-b4dd-2fed5ad072bc
  ID-2: /boot/efi raw-size: 260 MiB size: 256 MiB (98.46%) used: 103.6 MiB
    (40.5%) fs: vfat dev: /dev/nvme0n1p1 maj-min: 259:1 label: SYSTEM_DRV
    uuid: 92C4-DD2F
  ID-3: /home raw-size: 80 GiB size: 78.19 GiB (97.74%) used: 5.45 GiB
    (7.0%) fs: ext4 dev: /dev/nvme0n1p4 maj-min: 259:4 label: N/A
    uuid: a3a2ba69-8213-4723-ba8d-b4b8470dd7eb
  ID-4: /home/<filter>/Public raw-size: 7.55 GiB size: 7.53 GiB (99.80%)
    used: 1.39 GiB (18.4%) fs: vfat dev: /dev/sda1 maj-min: 8:1 label: USB
    uuid: A900-D1FD
  ID-5: /run/media/kearney/a raw-size: 235.67 GiB size: 231.41 GiB (98.19%)
    used: 171.95 GiB (74.3%) fs: ext4 dev: /dev/nvme0n1p3 maj-min: 259:3
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
  Device-1: 1-2:3 info: Silicon Motion - Taiwan (formerly Feiya ) Flash
    Drive type: Mass Storage driver: usb-storage interfaces: 1 rev: 2.0
    speed: 480 Mb/s power: 300mA chip-ID: 090c:1000 class-ID: 0806
    serial: <filter>
  Device-2: 1-4:2 info: Realtek Bluetooth Radio type: Bluetooth
    driver: btusb interfaces: 2 rev: 1.0 speed: 12 Mb/s power: 500mA
    chip-ID: 0bda:c123 class-ID: e001 serial: <filter>
  Hub-2: 2-0:1 info: Super-speed hub ports: 2 rev: 3.1 speed: 10 Gb/s
    chip-ID: 1d6b:0003 class-ID: 0900
  Hub-3: 3-0:1 info: Hi-speed hub with single TT ports: 4 rev: 2.0
    speed: 480 Mb/s chip-ID: 1d6b:0002 class-ID: 0900
  Device-1: 3-2:2 info: Chicony Integrated Camera type: Video
    driver: uvcvideo interfaces: 4 rev: 2.0 speed: 480 Mb/s power: 500mA
    chip-ID: 04f2:b67c class-ID: 0e02 serial: <filter>
  Hub-4: 4-0:1 info: Super-speed hub ports: 2 rev: 3.1 speed: 10 Gb/s
    chip-ID: 1d6b:0003 class-ID: 0900
Sensors:
  System Temperatures: cpu: 32.8 C mobo: N/A gpu: amdgpu temp: 33.0 C
  Fan Speeds (RPM): N/A
Info:
  Processes: 235 Uptime: 23h 48m wakeups: 1 Init: systemd v: 251
  default: graphical tool: systemctl Compilers: gcc: 12.2.0 Packages:
  pm: pacman pkgs: 1105 libs: 278 tools: pamac,yay Shell: Bash (login)
  v: 5.1.16 running-in: tty 2 inxi: 3.3.22
```

# 10.24 无法唤醒登录

早上开机挂pt就离开了，晚上回来发现卡死了，只显示锁屏界面但没有密码框输入密码进行登录，鼠标在中间但无法移动，蓝牙键鼠失效，触摸板失效，笔记本键盘可以进入 tty2, 进入 tty2 后蓝牙键盘恢复工作，再切回桌面蓝牙就断掉了。top 查看没有僵尸进程。

10.18 号那次是强制重启，论坛说了不好，论坛给出了 REISUB 方法，但是在测试最终办法之前想试验之前查到但是没有试验的办法，通过`systemctl status display-manager` 发现最底部一行有一个 stop 

```bash
$ systemctl status display-manager
● sddm.service - Simple Desktop Display Manager
     Loaded: loaded (/usr/lib/systemd/system/sddm.service; enabled; preset: disabled)
     Active: active (running) since Mon 2022-10-24 23:52:12 CST; 24min ago
       Docs: man:sddm(1)
             man:sddm.conf(5)
   Main PID: 9402 (sddm)
      Tasks: 17 (limit: 16582)
     Memory: 67.1M
        CPU: 1min 9.254s
     CGroup: /system.slice/sddm.service
             ├─9402 /usr/bin/sddm
             └─9405 /usr/lib/Xorg -nolisten tcp -background none -seat seat0 vt1 -auth /var/run/sddm/{d4568f49-f5b6->

10月 24 23:52:19 82dm sddm-helper[9469]: pam_systemd_home(sddm:auth): systemd-homed is not available: Unit dbus-org.>
10月 24 23:52:19 82dm sddm-helper[9469]: [PAM] Preparing to converse...
10月 24 23:52:19 82dm sddm-helper[9469]: [PAM] Conversation with 1 messages
10月 24 23:52:19 82dm sddm-helper[9469]: [PAM] returning.
10月 24 23:52:19 82dm sddm[9402]: Authenticated successfully
10月 24 23:52:19 82dm sddm-helper[9469]: pam_unix(sddm:session): session opened for user kearney(uid=1000) by (uid=0)
10月 24 23:52:19 82dm sddm-helper[9469]: Starting: "/usr/share/sddm/scripts/Xsession \"/usr/bin/startplasma-x11\""
10月 24 23:52:19 82dm sddm[9402]: Session started
10月 24 23:52:20 82dm sddm[9402]: Auth: sddm-helper exited successfully
10月 24 23:52:20 82dm sddm[9402]: Greeter stopped.
```

但是其颜色并不是红色，不过 stop 也不是什么好词，怀疑这个 display-manager 服务出了问题，起初是通过 `ps aux | grep display-manager` 发现没有这个进程，还会测试了 systemctl 发现了这个服务。

在 tty2 查看蓝牙服务状态时发现两行红色，更加确认了蓝牙确实在桌面停止了工作，restart 并没有使得它在卡死的桌面恢复工作。

```bash
10月 24 23:52:27 82dm bluetoothd[511]: src/profile.c:record_cb() Unable to get Hands-Free Voice gateway SDP record: Host is down
10月 24 23:52:32 82dm bluetoothd[511]: src/profile.c:record_cb() Unable to get Hands-Free Voice gateway SDP record: Host is down
```

`kquitapp5 plasmashell` 在 tty2 我也试验了，返回方块字，猜测是未找到命令。最后通过 `systemctl restart display-manager` 使得桌面恢复正常，可以登录了，但似乎我的 qb 被退出了，恢复正常后的桌面没看到 qb，但是手段挂载的盘没有被取消挂载。

恢复正常时候我在 codium 的终端里面试验了 `kquitapp5 plasmashell` 是可以的，桌面变黑色，但是 firefox、codium 还在运行，`kstart5 plasmashell` 则会显示一堆提示信息，然后桌面恢复了。

这次还有个收获就是 tty2 输错了密码锁三分钟，错三次锁九分钟，离谱的是我也不知道咋错了的，输入的都是同一串。

## log 分析

`sudo journalctl -b -1 >log.txt` 1.7万行，L1682~L16660 都是 rslsync ，时间上从 10.23 23:02:26 - 10.24 00:00:48 ，大概就是很多 thread 工作后因为网络原因同步失败。一般来说设备都直接处于校园网下是没什么同步问题的，但是如果我加了一个路由器，就会极大影响 rslsync 的同步工作。 

# 10.26 无法唤醒登陆

系统状态和10.24 一样，display-manager、sddm、bluetooth 都一样的问题，程序（firefox、qb）还在运行，最后是 `systemctl restart sddm` 部分解决问题的，但是这样就是导致 firefox、qb 都被关闭了，在 tty2 尝试 sddm-greeter 命令，没啥帮助

日志导出有 17M，但是时间上是 10.24 08:45 ~ 10.25 08:36，最后一条是 Journal stopped。 但是崩溃发生在26号中午我去吃午饭的时候。。。去吃午饭前，qb在做种、firefox 挂着网页没动、konsole里ffmpeg在转码，rslsync 已经被我关了。也就是说上次存在的原因这次提前被我剔除了、然后日志显示日志系统停止工作了，我是昨晚回来才开机的，晚上睡觉的时候没关机，硬盘扩展坞在拷贝数据。早上起来没啥问题。

wiki 中 [SDDM](https://wiki.archlinux.org/title/SDDM) 也有一条关于  no greeter shows，我的硬盘空间还够,重启 ssdm 我试验过有缺点，`loginctl` 可以查看 sessionID（推测是最左侧一列），`loginctl unlock-session session_id` 下次再试验吧。

# 参考

[Desktop freezes makeixo 2021.3.7](https://forum.endeavouros.com/t/desktop-freezes/12528)
> sudo journalctl -b -1 | eos-sendlog

    and show the returned small URL.

[Plasma小部件导致plasmashell卡死自动killed解决方法 明月与信 于 2021-10-03](https://blog.csdn.net/qq_40738478/article/details/120595826)：没有这个文件
> sudo vim ~/.config/plasma-org.kde.plasma.desktop-appletsrc 

[KDE Plasma 卡住  2022-01-07  cralor](https://www.cnblogs.com/cralor/p/15773876.html) 重启图形终端界面后黑屏，只有鼠标能动，切换几次后好了
> sudo systemctl restart display-manager

[重启崩溃的 KDE 2020-08-15 飞舞的冰龙 ](https://www.cnblogs.com/flyingicedragon/p/13508283.html):比下面的参考多一个5
> kstart5 plasmashell    # 重新启动 plasma 桌面会话

[kde 崩溃，如何重启 kde plasma 5 桌面 哈哈餐馆 2017-09-01](https://blog.csdn.net/aaazz47/article/details/77775572)
> kquitapp5 plasmashell    # 退出kde桌面 或 killall plasmashell    # 杀死 ked plasma 的进程
  kstart plasmashell

## not helpful

[ArchLinux KDE 桌面环境频繁卡死怎么破 ？](https://www.zhihu.com/question/490374634)

[[SOLVED!] DWM desktop freezes when logged in when installed on kde plasma](https://forum.endeavouros.com/t/solved-dwm-desktop-freezes-when-logged-in-when-installed-on-kde-plasma/19366): 

[Randomly wayland gnome desktop freezes. jeyhunn. 2022.8.12](https://forum.endeavouros.com/t/randomly-wayland-gnome-desktop-freezes/30171)

[ Plasma freezes Oct 05, 2010 Guilo](https://forum.kde.org/viewtopic.php?t=90688)

[KDE (Plasma) desktop freezes, but machine remains operable 20 Oct 2020 JetForMe](https://forum.linuxcnc.org/18-computer/40384-kde-plasma-desktop-freezes-but-machine-remains-operable?start=10)