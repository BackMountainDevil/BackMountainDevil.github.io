# 添加开机自动启动任务的四种方式
- date: 2021-05-21
- lastmod: 2021-09-19

# 目的
要运行的程序：/home/pi/Desktop/uc2-tkinter/gui.py，运行方式也比较简单：python3 /home/pi/Desktop/uc2-tkinter/gui.py。

一个简单的启动脚本（start.sh）,需要加上执行权限(chmod +x start.sh)，切换路径是程序做了些设置，不得不这样做。

```sh
cd /home/pi/Desktop/uc2-tkinter
/usr/bin/python3 /home/pi/Desktop/uc2-tkinter/gui.py
```

# 四种办法
## rc.local

```bash
sudo nano /etc/rc.local
# 在 exit 0 的前面一行加上你要执行的指令
cd /home/pi/Desktop/uc2-tkinter && /usr/bin/python3 /home/pi/Desktop/uc2-tkinter/gui.py
exit 0
```

重启没有发现程序正在运行...

## cron

```bash
crontab -e
# 第一次使用会提问用什么文本编辑器，不懂就直接回车
@reboot cd /home/pi/Desktop/uc2-tkinter && /usr/bin/python3 /home/pi/Desktop/uc2-tkinter/gui.py
```

重启emm

## autostart

```bash
# 创建应用自启动项 start.desktop
sudo nano /etc/xdg/autostart/start.desktop

# 粘贴进去下面的内容后保存
[Desktop Entry]
Name=uc2
Exec=cd /home/pi/Desktop/uc2-tkinter && /usr/bin/python3 /home/pi/Desktop/uc2-tkinter/gui.py
```

启动之后就会执行 Exec 所指带的指令，指令测试是有效的，但是重启发现这个 GUI 程序似乎没有被启动，

## service

```bash
sudo nano /etc/systemd/system/uc2.service

# 内容
[Unit]
Description=uc2
After=network.target

[Service]
ExecStart=/usr/bin/python3 /home/pi/Desktop/uc2-tkinter/gui.py
WorkingDirectory=/home/pi/Desktop/uc2-tkinter
StandardOutput=inherit
StandardError=inherit
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
# 内容结束

# 刷新一下服务
sudo systemctl daemon-reload
sudo systemctl start uc2.service  # 启动服务
sudo systemctl status uc2.service # 查看服务状态
sudo systemctl stop uc2.service # 终止服务
sudo systemctl enable uc2.service # 启动服务、并设置自启动
sudo systemctl disable uc2.service # 和上面相反
```


然而在我启动服务的时候啥也没有。。。。里面的 after 写的是在网络启动之后启动这个服务，会不会是因为我这个程序启动的时候图形界面就没有准备好（程序里最这种情况会捕获异常，然后结束）。把 network.target 改成graphical.target 之后也不行。。

# 相关参考

- [The systemd Daemon](https://www.raspberrypi.org/documentation/computers/using_linux.html#the-systemd-daemon)： Cron（@reboot）、Service
- [3 Ways to Run a Raspberry Pi Program or Script at Startup. By Yash Wate. Apr 16, 2021 ](https://www.makeuseof.com/how-to-run-a-raspberry-pi-program-script-at-startup/#:~:text=Autostart%20is%20the%20best%20way%20to%20run%20GUI-based,the%20system%20runs%20any%20of%20the%20scheduled%20programs.):rc.local、Cron、/etc/xdg/autostart/display.desktop
