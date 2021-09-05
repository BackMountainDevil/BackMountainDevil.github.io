---
title: "Autostart"
date: 2021-05-21T20:01:37+08:00
lastmod: 2021-05-21T20:01:37+08:00
keywords: []
description: ""
tags: []
categories: [Blog]
author: "筱氚"
---
#

run.sh

```bash
cd /home/kearney/Documents/code/python/flask/flask-chat
source /home/kearney/.local/share/virtualenvs/flask-chat-128w9Wfh/bin/activate
flask run
```

fail

```bash
cd /home/kearney/Documents/code/python/flask/flask-chat
pipenv shell
flask run
```

输入重定向失败

```bash
cd /home/kearney/Documents/code/python/flask/flask-chat
pipenv shell <'flaskrun.txt'
```

Arch 下没有 /etc/rc.local，pi OS 有。
# 

[树莓派设置开机自启动程序(可执行文件与python脚本) Bonennult 2020-07-20](https://blog.csdn.net/weixin_41024483/article/details/107468692):/etc/rc.local + 绝对路径

[树莓派Linux 开机自启动脚本 设置方法 快乐虫 2021-03-05](https://blog.csdn.net/jason_cdd/article/details/114399643):/etc/rc.local + 不推荐绝对路径

[Linux系统如何设置开机自动运行脚本？ 作者：良许来源：良许Linux|2020-06-11 07:57](https://os.51cto.com/art/202006/618603.htm)：/etc/rc.d/rc.local（？？？）、 crontab、systemd

[]()

[]()
