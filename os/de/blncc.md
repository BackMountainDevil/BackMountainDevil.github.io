# 蓝牙不正常因为 unmet condition check
- date: 2022-12-22
- lastmod: 2022-12-23

![无已配对设备](https://img-blog.csdnimg.cn/06bd21f1ff674d01bdd1c7a2fce243cb.jpeg#pic_center)

现象：蓝牙键盘鼠标均不工作，图标也不显示，KDE系统设置显示“无已配对设备”，但是配对设备的按钮没有显示，啥按钮也没有显示

事前：捣鼓 ch9328 没有成功，导致蓝牙断联，拔出 ch9328 后蓝牙偶尔恢复工作，重启后蓝牙正常，反复这个过程，捣鼓9328,蓝牙g,重启，最后重启蓝牙没恢复，出现了上面的现象

解决办法：关机后开机，恢复正常，已配对设备都显示出来了

结论：

1. 重启 和 关机后开机 不一样

## systemctl

unmet condition check 翻译为“未满足条件检查”

```bash
# systemctl status bluetooth
○ bluetooth.service - Bluetooth service
     Loaded: loaded (/usr/lib/systemd/system/bluetooth.service; enabled; preset: disabled)
     Active: inactive (dead)
       Docs: man:bluetoothd(8)

12月 22 10:58:56 82dm systemd[1]: Bluetooth service was skipped because of an unmet condition check (Condi>
12月 22 10:59:26 82dm systemd[1]: Bluetooth service was skipped because of an unmet condition check (Condi>
12月 22 11:03:08 82dm systemd[1]: Bluetooth service was skipped because of an unmet condition check (Condi>
# systemctl cat bluetooth|grep Condition
ConditionPathIsDirectory=/sys/class/bluetooth
# systemctl status systemd-firstboot.service 
○ systemd-firstboot.service - First Boot Wizard
     Loaded: loaded (/usr/lib/systemd/system/systemd-firstboot.service; static)
     Active: inactive (dead)
  Condition: start condition failed at Thu 2022-12-22 10:58:29 CST; 8min ago
             └─ ConditionFirstBoot=yes was not met
       Docs: man:systemd-firstboot(1)

12月 22 10:58:29 82dm systemd[1]: First Boot Wizard was skipped because of an unmet condition check (Condi>
# systemctl cat bluetooth|grep Condition
ConditionPathIsDirectory=/sys/class/bluetooth
```

bug 复现，插上 ch9328 就蓝牙挂了，拔掉 9328 重启蓝牙服务也不得行

```bash
$ sudo systemctl start bluetooth
$ sudo systemctl status bluetooth
● bluetooth.service - Bluetooth service
     Loaded: loaded (/usr/lib/systemd/system/bluetooth.service; enabled; preset: disabled)
     Active: active (running) since Thu 2022-12-22 12:33:29 CST; 4h 16min ago
       Docs: man:bluetoothd(8)
   Main PID: 522 (bluetoothd)
     Status: "Running"
      Tasks: 1 (limit: 14160)
     Memory: 3.8M
        CPU: 2min 2.344s
     CGroup: /system.slice/bluetooth.service
             └─522 /usr/lib/bluetooth/bluetoothd

12月 22 16:48:35 82dm bluetoothd[522]: Endpoint unregistered: sender=:1.49 path=/MediaEndpoint/A2DPSource/aptx_ll_1
12月 22 16:48:35 82dm bluetoothd[522]: Endpoint unregistered: sender=:1.49 path=/MediaEndpoint/A2DPSource/aptx_ll_0
12月 22 16:48:35 82dm bluetoothd[522]: Endpoint unregistered: sender=:1.49 path=/MediaEndpoint/A2DPSource/aptx_ll_duplex_1
12月 22 16:48:35 82dm bluetoothd[522]: Endpoint unregistered: sender=:1.49 path=/MediaEndpoint/A2DPSource/aptx_ll_duplex_0
12月 22 16:48:35 82dm bluetoothd[522]: Endpoint unregistered: sender=:1.49 path=/MediaEndpoint/A2DPSource/faststream
12月 22 16:48:35 82dm bluetoothd[522]: Endpoint unregistered: sender=:1.49 path=/MediaEndpoint/A2DPSource/faststream_duplex
12月 22 16:48:35 82dm bluetoothd[522]: Endpoint unregistered: sender=:1.49 path=/MediaEndpoint/A2DPSink/opus_05
12月 22 16:48:35 82dm bluetoothd[522]: Endpoint unregistered: sender=:1.49 path=/MediaEndpoint/A2DPSource/opus_05
12月 22 16:48:35 82dm bluetoothd[522]: Endpoint unregistered: sender=:1.49 path=/MediaEndpoint/A2DPSink/opus_05_duplex
12月 22 16:48:35 82dm bluetoothd[522]: Endpoint unregistered: sender=:1.49 path=/MediaEndpoint/A2DPSource/opus_05_duplex

$ sudo systemctl stop bluetooth
$ sudo systemctl start bluetooth
$ sudo systemctl status bluetooth
● bluetooth.service - Bluetooth service
     Loaded: loaded (/usr/lib/systemd/system/bluetooth.service; enabled; preset: disabled)
     Active: active (running) since Thu 2022-12-22 16:53:05 CST; 4s ago
       Docs: man:bluetoothd(8)
   Main PID: 39362 (bluetoothd)
     Status: "Running"
      Tasks: 1 (limit: 14160)
     Memory: 840.0K
        CPU: 27ms
     CGroup: /system.slice/bluetooth.service
             └─39362 /usr/lib/bluetooth/bluetoothd

12月 22 16:53:05 82dm bluetoothd[39362]: Bluetooth daemon 5.66
12月 22 16:53:05 82dm systemd[1]: Started Bluetooth service.
12月 22 16:53:05 82dm bluetoothd[39362]: Starting SDP server
12月 22 16:53:05 82dm bluetoothd[39362]: profiles/audio/vcp.c:vcp_init() D-Bus experimental not enabled
12月 22 16:53:05 82dm bluetoothd[39362]: src/plugin.c:plugin_init() Failed to init vcp plugin
12月 22 16:53:05 82dm bluetoothd[39362]: profiles/audio/mcp.c:mcp_init() D-Bus experimental not enabled
12月 22 16:53:05 82dm bluetoothd[39362]: src/plugin.c:plugin_init() Failed to init mcp plugin
12月 22 16:53:05 82dm bluetoothd[39362]: profiles/audio/bap.c:bap_init() D-Bus experimental not enabled
12月 22 16:53:05 82dm bluetoothd[39362]: src/plugin.c:plugin_init() Failed to init bap plugin
12月 22 16:53:05 82dm bluetoothd[39362]: Bluetooth management interface 1.22 initialized
$ rfkill
ID TYPE      DEVICE                 SOFT      HARD
 0 wlan      ideapad_wlan      unblocked unblocked
 1 bluetooth ideapad_bluetooth unblocked unblocked
 3 wlan      phy0              unblocked unblocked
```

# 参考

[[SOLVED] Condition check resulted in Bluetooth service being skipped. 2019-02-26 blackfedora](https://bbs.archlinux.org/viewtopic.php?id=244532)
> Another cold boot resolved the issue.