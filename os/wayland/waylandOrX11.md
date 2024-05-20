# 判断使用的是 x11 还是 wayland
- date: 2024-05-20

之前的判断方式是运行命令 `echo "$XDG_SESSION_TYPE"` 来查看环境变量的值，如果返回的是 wayland 则说明用的 wayland，如果返回的是 x11 说明用的 x11。

但是，如果返回的是 tty 则不能说明是哪个。例如通过 ssh 连接到使用 x11 的远程主机，会返回 tty。

## ps

[如何判断linux 显示服务器是xorg (x11) 还是wayland 2023-02-24 Tarzen](https://www.cnblogs.com/tarzen213/p/17151656.html)中提出使用 ps -ef |grep x11 或者 ps -ef |grep wayland 来查找相关进程，但是实测存在问题，如下所示，第一行显示用的是 x11，但是后面查找相关进程时，却没有找到 x11 或 wayland 相关进程。

```bash
$ echo $XDG_SESSION_TYPE
x11
$ ps -ef | grep wayland
mifen     277467  252819  0 15:26 pts/0    00:00:00 grep wayland
$ ps -ef | grep x11
mifen     277475  252819  0 15:26 pts/0    00:00:00 grep x11
$ ps -ef | grep *x11*
mifen     277903  252819  0 15:36 pts/0    00:00:00 grep *x11*
$ ps -ef | grep *wayland*
mifen     277906  252819  0 15:36 pts/0    00:00:00 grep *wayland*
```

按理说这个方法是管用的，然后我手打发现好像不用加短线，然后这个方法就正常了，用 ssh 时也是能够判断用的是 x11 还是 wayland，有个缺陷就是输出有点长，在下面里用 LONG_LONG 替代了很多字符，于是想着可以用 wc -l 来统计输出的行数，下面例子的最后两行就是

```bash
$ ps ef | grep x11
 278341 pts/3    Ss     0:00 bash SSH_AUTH_SOCK LONG_LONG XDG_SEAT=seat0
 278349 pts/3    R+     0:00  \_ ps ef SHELL=/bin/bash LONG_LONG _=/usr/bin/ps
 278350 pts/3    S+     0:00  \_ grep x11 SHELL=/bin/bash LONG_LONG _=/usr/bin/grep
 277868 pts/1    Ss+    0:00 bash SSH_AUTH_SOCK LONG_LONG XDG_SEAT=seat0
 252819 pts/0    Ss+    0:00 bash SSH_AUTH_SOCK LONG_LONG XDG_SEAT=seat0

$ ps ef | grep wayland
 278353 pts/3    S+     0:00  \_ grep wayland SHELL=/bin/bash LONG_LONG _=/usr/bin/grep

$ ps ef | grep x11 | wc -l
5
$ ps ef | grep wayland | wc -l
1
```

## loginctl

[如何检查： 是 Xorg 还是 Wayland 显示服务器？作者： Arindam 译者： LCTT geekpi | 2022-11-05](https://linux.cn/article-15216-1.html) 中提出使用 loginctl，然后把 SESSION 的值传递给 loginctl 来查看当前会话的类型

```bash
$ loginctl
SESSION  UID USER  SEAT  TTY STATE  IDLE SINCE
      2 1000 mifen seat0 -   active no   -    

$ loginctl show-session 2 -p Type
Type=x11
```

这个方法也存在刚开始我们说的 ssh 问题，将会得到 `Type=tty`。如果存在多用户，这个还能看到其它人的会话类型，比如我是用 ssh 连接到远程主机，把我的参数丢进去得到 tty,看到某个用户的参数带字母（c2），和其他人不一样，把 c2 丢进去得到的结果是 x11。也就是说用这个办法的话则是尽量变量所有的会话，忽略掉返回 tty 的，才知道答案，比如下面的例子：

```bash
$ loginctl
SESSION  UID USER  SEAT  TTY   STATE  IDLE SINCE
     12 1000 mifen -     pts/2 active no   -
      2 1000 mifen seat0 -     active no   -

$ loginctl show-session 12 -p Type
Type=tty
$ loginctl show-session 2 -p Type
Type=x11
```

上面的例子中，用 ssh 连接到远程主机，loginctl 得到的结果有两个，分别查看类型后是 tty、x11，因此可以判断远程主机采用的是 x11
