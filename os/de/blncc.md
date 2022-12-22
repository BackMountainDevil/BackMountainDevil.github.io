# 蓝牙不正常因为 unmet condition check
- date: 2022-12-22
- lastmod: 2022-12-22

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

# 参考

[[SOLVED] Condition check resulted in Bluetooth service being skipped. 2019-02-26 blackfedora](https://bbs.archlinux.org/viewtopic.php?id=244532)
> Another cold boot resolved the issue.