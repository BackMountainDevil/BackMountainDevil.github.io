# GNU 的神器 screen（程序后台运行 vs nohup）附带串口通信功能
date: 2021-04-27  
lastmod: 2021-09-19

# 介绍
通过阿里云连接云服务器ECS部署cms网站的时候，发现关闭ssh连接之后网站就不能访问了，重新连接开启服务才能访问，于是推断是断开ssh连接的时候程序被中止了。

于是寻求能够保持程序运行在后台的办法，这里不得不说一下，远程连接 ssh 后进行操作的话，开一个 screen 的好处是万一当前 ssh 被中断，操作也不会被中断。

搜索得到以下两个办法
- **nohup** （no hang up）
- **screen**

使用 nohup 可以把程序放到后台运行，查看 nohup.out 可以查看程序运行的状况，但是使用 nohup 把程序放到后台，就再也无法切换程序到前台了，而 screen 甚至可以反复使唤。
 
# nohup
## 启动
```c
nohup 待运行程序的命令 &
```
例如我本身要执行的服务是`npm start`，但是这个程序部署运行之后会一直输出内容占用当前会话导致我不能做其它事情，而且退出 ssh 连接之后这个 npm服务 就会中断。

采用nohup时就是下面这样。此时该程序被放到了后台继续进行，且不会在看到一直不断的输出了，下面输出了它的进程号，不过一般人会记得吗


```c
nohup npm start &
[1] 10749
[root@iZ CmsWing]# nohup: ignoring input and appending output to ‘nohup.out’
```
此时原来的输出被定向到nohup.out文件中
 
## 结束
结束用 nohup 放在后台的进程需要手动结束，使用 `kill -9+进程id` 强制歇菜（有更好的办法吗），比如上面的结束指令是 kill -9 10749

```bash
# 查询 mysql 进程id，查啥就替换啥 ： ps aux | grep 啥
ps aux | grep mysql
》u0_a92   21924  0.1  2.4 521632 67376 pts/1    S<l+  1970   0:01 mysqld                                                                                                                                   
》u0_a92   22463  2.0  0.0   9764  1740 pts/2    S<+   1970   0:00 grep mysqld 
# 可以从第一行看出 mysqld的进程是21924， 第二行是我们查询这件事情的进程id

# 结束数据库进程，-9 后面的的id需要进行替换哈
kill -9 21924
```

# screen
 
安装十分简便，用系统自带的包管理器直接装就完事了（yum install screen, pacman -S screen）

## 常见操作

```bash
screen --help                   # 查看帮助
screen -list		 	# 显示所有会话的进程号、名称、状态
screen -S sessionName 		# 新建一个会话窗口，名称为 sessionName
screen -r sessionName	        # 切换到名称为 sessionName 的会话，没有就切不过去，-R 则是没有就创建
screen -wipe	                # 清除状态为dead的会话
CTRL + a p: 切换到上一个会话
CTRL + a n: 切换到下一个会话
Control + a + d：暂时离开当前会话，留程序在后台运行，再切换回来用screen -r

screen -S sessionName -X quit	# 结束名称为 sessionName 的会话
Ctrl+a，之后按 k ，根据提示按 y 结束当前会话
Ctrl+a，之后输入 :quit 回车结束当前会话（冒号也要输入）
```

举个例子

```bash
$ screen -list  # 查看所有会话，显示没有
No Sockets found in /run/screens/S-kearney.

$ screen -S temp        # 创建会话
$ screen -S chat
$ screen -S blog
$ screen -list  # 查看所有会话，显示刚才创建的三个会话的进程号和名称，可通过 “ps aux | grep 会话名称“ 确认进程号
There are screens on:
        3806.blog       (Attached)
        3626.chat       (Attached)
        3545.temp       (Attached)
3 Sockets in /run/screens/S-kearney.
Ctrl+a，之后按 k 

$ screen -list
There are screens on:
        3806.blog       (Attached)
        3626.chat       (Detached)
2 Sockets in /run/screens/S-kearney

$ screen -S chat -X quit
$ screen -list
There is a screen on:
        3806.blog       (Detached)
1 Socket in /run/screens/S-kearney.

$ screen -r blog
$ screen -list
There is a screen on:
        3806.blog       (Attached)
1 Socket in /run/screens/S-kearney.
Ctrl+a，之后输入 :quit 回车
$ screen -list
No Sockets found in /run/screens/S-kearney.
```

## 串口通信

screen 除了能将程序“隔离运行“，还能使用 USB-UART 进行串口通信，如树莓派 3B 的 Serial Port（需要额外开启）。假设要连接的串口是 /dev/ttyUSB0，那么就这样连接到串口 screen /dev/ttyUSB0 115200。默认的波特率是 9600,因此其它数值需要指定，前面的案例指定为 115200。


# 相关参考

- [Serial Console - Screen. Arch wiki](https://wiki.archlinux.org/title/Working_with_the_serial_console)
- [GNU Screen. Arch wiki](https://wiki.archlinux.org/title/GNU_Screen)
- [Centos解决退出连接又不退出程序的问题 screen || nohup.Kearney form An idea 2020-04-16](https://blog.csdn.net/weixin_43031092/article/details/105564949)
- [How to install and use screen, a remote session management tool for CentOS 7 system?Time：2021-3-1](https://developpaper.com/how-to-install-and-use-screen-a-remote-session-management-tool-for-centos-7-system/)
- [A Basic Understanding Of screen On Centos  CentOS Help / Resources / Scripts & Tools / A Basic Understanding Of screen On Centos](https://centoshelp.org/resources/scripts-tools/a-basic-understanding-of-screen-on-centos/)