# 关闭/禁止 snap

- [为什么Ubuntu的Snap是不受欢迎的 御剑 2022-06-07](https://cloud.tencent.com/developer/article/2017496)
- [什么是Snap应用？by liam zheng on 21 October 2020](https://cn.ubuntu.com/blog/what-is-snap-application)
- [How do I turn off snap in Ubuntu?  Sam U](https://linuxhint.com/turn-off-snap-ubuntu/)

```bash
$ snap list
Name               Version          Rev    Tracking         Publisher   Notes
bare               1.0              5      latest/stable    canonical✓  base
core20             20220719         1587   latest/stable    canonical✓  base
firefox            103.0.1-1        1635   latest/stable/…  mozilla✓    -
gnome-3-38-2004    0+git.891e5bc    112    latest/stable/…  canonical✓  -
gtk-common-themes  0.1-81-g442e511  1535   latest/stable/…  canonical✓  -
snapd              2.56.2           16292  latest/stable    canonical✓  snapd
$ sudo snap remove gtk-common-theme
[sudo] password for kearney:
snap "gtk-common-theme" is not installed
$ sudo snap remove gtk-common-themes
gtk-common-themes removed
$ $sudo apt purge snapd
E: Could not open lock file /var/lib/dpkg/lock-frontend - open (13: Permission denied)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), are you root?
```

- [How to disable Snaps in Ubuntu by Derrik Diener Jul 10, 2020](https://www.addictivetips.com/ubuntu-linux-tips/disable-snaps-ubuntu/#:~:text=Snapd%20is%20the%20background%20program%20that%20handles%20all,run%20the%20sudo%20apt%20remove%20snapd%20%E2%80%93purge%20command.)

