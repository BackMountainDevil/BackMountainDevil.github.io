# 在连接 WiFi 的同时开热点
- date: 2023-8-11

需求：连接 wifi 网络的同时还能开热点给手机。这项需求在 windows 上可以轻松实现，而在 linux 上还不太可行

## system info

```bash
$ inxi -F
System:
  Host: 82dm Kernel: 6.4.8-arch1-1 arch: x86_64 bits: 64 Desktop: KDE Plasma
    v: 5.27.7 Distro: EndeavourOS
Machine:
  Type: Laptop System: LENOVO product: 82DM v: Lenovo XiaoXinPro-13ARE 2020
    serial: <superuser required>
  Mobo: LENOVO model: LNVNB161216 v: SDK0L77769WIN
    serial: <superuser required> UEFI: LENOVO v: F0CN39WW date: 12/07/2022
Battery:
  ID-1: BAT0 charge: 47.0 Wh (100.0%) condition: 47.0/56.0 Wh (83.9%)
CPU:
  Info: 6-core model: AMD Ryzen 5 4600U with Radeon Graphics bits: 64
    type: MT MCP cache: L2: 3 MiB
  Speed (MHz): avg: 1653 min/max: 1400/2100 cores: 1: 1396 2: 2275 3: 1644
    4: 1398 5: 1397 6: 1400 7: 2564 8: 1397 9: 1980 10: 1596 11: 1397 12: 1397
Graphics:
  Device-1: AMD Renoir driver: amdgpu v: kernel
  Device-2: Chicony Integrated Camera driver: uvcvideo type: USB
  Display: x11 server: X.Org v: 21.1.8 driver: X: loaded: amdgpu
    unloaded: modesetting dri: radeonsi gpu: amdgpu resolution: 2560x1600~60Hz
  API: OpenGL v: 4.6 Mesa 23.1.5 renderer: AMD Radeon Graphics (renoir LLVM
    15.0.7 DRM 3.52 6.4.8-arch1-1)
Audio:
  Device-1: AMD Renoir Radeon High Definition Audio driver: snd_hda_intel
  Device-2: AMD ACP/ACP3X/ACP6x Audio Coprocessor driver: N/A
  Device-3: AMD Family 17h/19h HD Audio driver: snd_hda_intel
  API: ALSA v: k6.4.8-arch1-1 status: kernel-api
  Server-1: PipeWire v: 0.3.77 status: active
Network:
  Device-1: Realtek RTL8822CE 802.11ac PCIe Wireless Network Adapter
    driver: rtw_8822ce
  IF: wlan0 state: up mac: xx:xx:xx:xx:xx:xx
Bluetooth:
  Device-1: Realtek Bluetooth Radio driver: btusb type: USB
  Report: rfkill ID: hci0 state: up address: see --recommends
Drives:
  Local Storage: total: 476.94 GiB used: 250.84 GiB (52.6%)
  ID-1: /dev/nvme0n1 vendor: Western Digital model: PC SN730
    SDBPNTY-512G-1101 size: 476.94 GiB
Partition:
  ID-1: / size: 39.08 GiB used: 18.77 GiB (48.0%) fs: ext4 dev: /dev/nvme0n1p2
  ID-2: /boot/efi size: 256 MiB used: 103.5 MiB (40.4%) fs: vfat
    dev: /dev/nvme0n1p1
  ID-3: /home size: 78.19 GiB used: 11.32 GiB (14.5%) fs: ext4
    dev: /dev/nvme0n1p4
Swap:
  Alert: No swap data was found.
Sensors:
  System Temperatures: cpu: 56.6 C mobo: N/A gpu: amdgpu temp: 43.0 C
  Fan Speeds (RPM): N/A
Info:
  Processes: 288 Uptime: 28m Memory: total: 12 GiB available: 11.54 GiB
  used: 2.9 GiB (25.2%) Shell: Bash inxi: 3.3.28
```

## iw list

```bash
[kearney@82dm ~]$ iw list
Wiphy phy0
        wiphy index: 0
        max # scan SSIDs: 4
        max scan IEs length: 323 bytes
        max # sched scan SSIDs: 4
        max # match sets: 0
        Retry short limit: 7
        Retry long limit: 4
        Coverage class: 0 (up to 0m)
        Device supports T-DLS.
        Supported Ciphers:
                * WEP40 (00-0f-ac:1)
                * WEP104 (00-0f-ac:5)
                * TKIP (00-0f-ac:2)
                * CCMP-128 (00-0f-ac:4)
                * CCMP-256 (00-0f-ac:10)
                * GCMP-128 (00-0f-ac:8)
                * GCMP-256 (00-0f-ac:9)
                * CMAC (00-0f-ac:6)
                * CMAC-256 (00-0f-ac:13)
                * GMAC-128 (00-0f-ac:11)
                * GMAC-256 (00-0f-ac:12)
        Available Antennas: TX 0x3 RX 0x3
        Configured Antennas: TX 0x3 RX 0x3
        Supported interface modes:
                 * IBSS
                 * managed
                 * AP
                 * AP/VLAN
                 * monitor
                 * mesh point
        Band 1:
                Capabilities: 0x19ef
                        RX LDPC
                        HT20/HT40
                        SM Power Save disabled
                        RX HT20 SGI
                        RX HT40 SGI
                        TX STBC
                        RX STBC 1-stream
                        Max AMSDU length: 7935 bytes
                        DSSS/CCK HT40
                Maximum RX AMPDU length 65535 bytes (exponent: 0x003)
                Minimum RX AMPDU time spacing: 2 usec (0x04)
                HT Max RX data rate: 300 Mbps
                HT TX/RX MCS rate indexes supported: 0-15, 32
                Bitrates (non-HT):
                        * 1.0 Mbps
                        * 2.0 Mbps
                        * 5.5 Mbps
                        * 11.0 Mbps
                        * 6.0 Mbps
                        * 9.0 Mbps
                        * 12.0 Mbps
                        * 18.0 Mbps
                        * 24.0 Mbps
                        * 36.0 Mbps
                        * 48.0 Mbps
                        * 54.0 Mbps
                Frequencies:
                        * 2412 MHz [1] (20.0 dBm)
                        * 2417 MHz [2] (20.0 dBm)
                        * 2422 MHz [3] (20.0 dBm)
                        * 2427 MHz [4] (20.0 dBm)
                        * 2432 MHz [5] (20.0 dBm)
                        * 2437 MHz [6] (20.0 dBm)
                        * 2442 MHz [7] (20.0 dBm)
                        * 2447 MHz [8] (20.0 dBm)
                        * 2452 MHz [9] (20.0 dBm)
                        * 2457 MHz [10] (20.0 dBm)
                        * 2462 MHz [11] (20.0 dBm)
                        * 2467 MHz [12] (20.0 dBm)
                        * 2472 MHz [13] (20.0 dBm)
                        * 2484 MHz [14] (disabled)
        Band 2:
                Capabilities: 0x19ef
                        RX LDPC
                        HT20/HT40
                        SM Power Save disabled
                        RX HT20 SGI
                        RX HT40 SGI
                        TX STBC
                        RX STBC 1-stream
                        Max AMSDU length: 7935 bytes
                        DSSS/CCK HT40
                Maximum RX AMPDU length 65535 bytes (exponent: 0x003)
                Minimum RX AMPDU time spacing: 2 usec (0x04)
                HT Max RX data rate: 300 Mbps
                HT TX/RX MCS rate indexes supported: 0-15, 32
                VHT Capabilities (0x03d071b2):
                        Max MPDU length: 11454
                        Supported Channel Width: neither 160 nor 80+80
                        RX LDPC
                        short GI (80 MHz)
                        TX STBC
                        SU Beamformee
                        MU Beamformee
                        +HTC-VHT
                VHT RX MCS set:
                        1 streams: MCS 0-9
                        2 streams: MCS 0-9
                        3 streams: not supported
                        4 streams: not supported
                        5 streams: not supported
                        6 streams: not supported
                        7 streams: not supported
                        8 streams: not supported
                VHT RX highest supported: 780 Mbps
                VHT TX MCS set:
                        1 streams: MCS 0-9
                        2 streams: MCS 0-9
                        3 streams: not supported
                        4 streams: not supported
                        5 streams: not supported
                        6 streams: not supported
                        7 streams: not supported
                        8 streams: not supported
                VHT TX highest supported: 780 Mbps
                VHT extended NSS: not supported
                Bitrates (non-HT):
                        * 6.0 Mbps
                        * 9.0 Mbps
                        * 12.0 Mbps
                        * 18.0 Mbps
                        * 24.0 Mbps
                        * 36.0 Mbps
                        * 48.0 Mbps
                        * 54.0 Mbps
                Frequencies:
                        * 5180 MHz [36] (20.0 dBm) (radar detection)
                        * 5200 MHz [40] (20.0 dBm) (radar detection)
                        * 5220 MHz [44] (20.0 dBm) (radar detection)
                        * 5240 MHz [48] (20.0 dBm) (radar detection)
                        * 5260 MHz [52] (20.0 dBm) (radar detection)
                        * 5280 MHz [56] (20.0 dBm) (radar detection)
                        * 5300 MHz [60] (20.0 dBm) (radar detection)
                        * 5320 MHz [64] (20.0 dBm) (radar detection)
                        * 5500 MHz [100] (disabled)
                        * 5520 MHz [104] (disabled)
                        * 5540 MHz [108] (disabled)
                        * 5560 MHz [112] (disabled)
                        * 5580 MHz [116] (disabled)
                        * 5600 MHz [120] (disabled)
                        * 5620 MHz [124] (disabled)
                        * 5640 MHz [128] (disabled)
                        * 5660 MHz [132] (disabled)
                        * 5680 MHz [136] (disabled)
                        * 5700 MHz [140] (disabled)
                        * 5720 MHz [144] (disabled)
                        * 5745 MHz [149] (33.0 dBm)
                        * 5765 MHz [153] (33.0 dBm)
                        * 5785 MHz [157] (33.0 dBm)
                        * 5805 MHz [161] (33.0 dBm)
                        * 5825 MHz [165] (33.0 dBm)
        Supported commands:
                 * new_interface
                 * set_interface
                 * new_key
                 * start_ap
                 * new_station
                 * new_mpath
                 * set_mesh_config
                 * set_bss
                 * authenticate
                 * associate
                 * deauthenticate
                 * disassociate
                 * join_ibss
                 * join_mesh
                 * remain_on_channel
                 * set_tx_bitrate_mask
                 * frame
                 * frame_wait_cancel
                 * set_wiphy_netns
                 * set_channel
                 * tdls_mgmt
                 * tdls_oper
                 * probe_client
                 * set_noack_map
                 * register_beacons
                 * start_p2p_device
                 * set_mcast_rate
                 * connect
                 * disconnect
                 * set_qos_map
                 * set_multicast_to_unicast
                 * set_sar_specs
        WoWLAN support:
                 * wake up on disconnect
                 * wake up on magic packet
                 * wake up on pattern match, up to 12 patterns of 1-128 bytes,
                   maximum packet offset 0 bytes
                 * can do GTK rekeying
                 * wake up on GTK rekey failure
                 * wake up on network detection, up to 4 match sets
        software interface modes (can always be added):
                 * AP/VLAN
                 * monitor
        valid interface combinations:
                 * #{ managed } <= 1, #{ AP } <= 1,
                   total <= 2, #channels <= 1
        HT Capability overrides:
                 * MCS: ff ff ff ff ff ff ff ff ff ff
                 * maximum A-MSDU length
                 * supported channel width
                 * short GI for 40 MHz
                 * max A-MPDU length exponent
                 * min MPDU start spacing
        Device supports TX status socket option.
        Device supports HT-IBSS.
        Device supports SAE with AUTHENTICATE command
        Device supports scan flush.
        Device supports per-vif TX power setting
        Driver supports full state transitions for AP/GO clients
        Driver supports a userspace MPM
        Device supports configuring vdev MAC-addr on create.
        Device supports randomizing MAC-addr in scans.
        max # scan plans: 1
        max scan plan interval: -1
        max scan plan iterations: 0
        Supported TX frame types:
                 * IBSS: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
                 * managed: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
                 * AP: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
                 * AP/VLAN: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
                 * mesh point: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
                 * P2P-client: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
                 * P2P-GO: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
                 * P2P-device: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
        Supported RX frame types:
                 * IBSS: 0x40 0xb0 0xc0 0xd0
                 * managed: 0x40 0xb0 0xd0
                 * AP: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0
                 * AP/VLAN: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0
                 * mesh point: 0xb0 0xc0 0xd0
                 * P2P-client: 0x40 0xd0
                 * P2P-GO: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0
                 * P2P-device: 0x40 0xd0
        Supported extended features:
                * [ RRM ]: RRM
                * [ SET_SCAN_DWELL ]: scan dwell setting
                * [ FILS_STA ]: STA FILS (Fast Initial Link Setup)
                * [ CONTROL_PORT_OVER_NL80211 ]: control port over nl80211
                * [ TXQS ]: FQ-CoDel-enabled intermediate TXQs
                * [ SCAN_RANDOM_SN ]: use random sequence numbers in scans
                * [ CAN_REPLACE_PTK0 ]: can safely replace PTK 0 when rekeying
                * [ CONTROL_PORT_NO_PREAUTH ]: disable pre-auth over nl80211 control port support
                * [ DEL_IBSS_STA ]: deletion of IBSS station support
                * [ SCAN_FREQ_KHZ ]: scan on kHz frequency support
                * [ CONTROL_PORT_OVER_NL80211_TX_STATUS ]: tx status for nl80211 control port support
```


Supported interface modes 中存在 AP 表面支持 AP 模式，上面的输出表面支持 6 种工作模式。

valid interface combinations 中 `#{ managed } <= 1, #{ AP } <= 1, total <= 2, #channels <= 1` 表面可以工作在两个模式下，但是限制一个模式的数量，并且限制两者在同一频道。

# 不同方法对比测试

## 系统设置

此方法大概率仅适用于 KDE Plasma(5.27.7.1-1)，其它 DE 的操作大同小异

步骤：设置 - 连接 - 添加新连接 - WiFI（共享），改好名称确认即可。此时网络不会自动连接上，在手机上也看不到这个热点。需要在电脑上手动连接刚创建好的网络，之后才能在手机上看到该热点，手机连接之后显示不安全的网络（可能是因为我没有设置密码的缘故）,在电脑上开 web 服务并开启防火墙，在手机上却无法访问，可以确认手机分配了ip，在电脑上也可以ping到手机。rslsync 也没法在两者之间建立连接。

这种方式下不像 windows 那么方便，可以在设置里看到连接了哪些设备及其ip、mac。诡异的是任务栏的网络中的热点按钮也没有被激活。

## 任务栏-网络-热点

在上一个方法的过程，就是保持手机还连着前面设置的热点，然后激活热点按钮，前面的连接便会被中断，建立一个新的 AP 并自动连接（根据AP信息推测是配置在`~/.config/plasma-nm`，但是修改配置重新激活热点却没有变，重启才生效，如果设置密码了会让输入密码），手机上初次需要手动连接。结局和上一个方法一样，ping通但是web、rslsync不通

## linux-wifi-hotspot

比一年前有进步，电脑可以在连接 wifi 的同时创建热点了，但 iphone 上显示该连接无互联网连接。于此同时在任务栏的网络中无相关信息显示，电脑无法 ping 到手机 ip（`ip addr` 命令显示主机ip是 192.168 网段的，而手机 ip 却是 169.254 的网段，同时手机上的路由器 ip 不显示了），手机也无法访问到电脑的 web 服务。在手机上将手动配置手机和路由器的ip之后，手机可以访问到 web 服务了（当然要开防火墙），rslsync 则不能联通，可能是后者的端口比较随机，尝试开放其gui中写的侦听端口 29537 后还是不通的，甚至我尝试关闭防火墙，rslsync 都是不通

以下案例的 mac 信息已打码

```bash
$ sudo create_ap wlan0 lo MyAccessPoint
Config dir: /tmp/create_ap.wlan0.conf.HQmR52Bj
PID: 4155
Network Manager found, set ap0 as unmanaged device... DONE
wlan0 is already associated with channel 157 (5785 GHz)
multiple channels not supported,

fallback to channel 157
Creating a virtual WiFi interface... ap0 created.
Sharing Internet using method: nat
hostapd command-line interface: hostapd_cli -p /tmp/create_ap.wlan0.conf.HQmR52Bj/hostapd_ctrl
ap0: interface state UNINITIALIZED->ENABLED
ap0: AP-ENABLED 
Low entropy detected, starting haveged
haveged: command socket is listening at fd 4
ap0: STA 2e:d2:c2:5x:12:01 IEEE 802.11: authenticated
ap0: STA 2e:d2:c2:5x:12:01 IEEE 802.11: associated (aid 1)
ap0: AP-STA-CONNECTED 2e:d2:c2:5x:12:01
ap0: STA 2e:d2:c2:5x:12:01 RADIUS: starting accounting session 23D638927494DC7E
```

# 参考

[Software access point](https://wiki.archlinux.org/title/Software_access_point)

[Ask for RTL8822CE AP and Station mixed mode. 2022.1](https://forum.manjaro.org/t/ask-for-rtl8822ce-ap-and-station-mixed-mode/100216/5)：配置文件配置热点、系统设置开热点均无法混合双开
