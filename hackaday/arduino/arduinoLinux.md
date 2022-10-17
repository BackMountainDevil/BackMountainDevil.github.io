# arduino linux 安装配置
- date: 2021-03-01
- lastmod: 2022-10-17

1. 安装，从包管理器安装

    sudo pacman -S arduino arduino-avr-core

2. 加权，给当前赋予读写串口的权限。arch 里边是 uucp 用户组，debian 则是 dialout

    sudo usermod -aG uucp $USER

# 参考

- [Arduino Arch wiki](https://wiki.archlinux.org/title/Arduino)

- [Arduino: /dev/ttyACM0 – Permission Denied [SOLVED] January 20, 2022 admin ](https://www.shellhacks.com/arduino-dev-ttyacm0-permission-denied/)
    > `sudo usermod -a -G dialout <username>`

- [Arch Linux下启动Arduino闪退的解决办法 Kearney form An idea 2021-03-01](https://blog.csdn.net/weixin_43031092/article/details/114271132):缺 arduino-avr-core

- [Arduno on linux上传失败： avrdude: ser_open(): can‘t open device “/dev/ttyUSB0“: Permission denied Kearney form An idea 2020-12-08](https://blog.csdn.net/weixin_43031092/article/details/110874455)
    > sudo usermod -a -G dialout kearney
