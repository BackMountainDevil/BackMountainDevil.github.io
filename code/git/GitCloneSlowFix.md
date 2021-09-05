---
title: "GitCloneSlowFix"
date: 2020-11-04T10:32:34+08:00
lastmod: 2020-11-04T11:19:13+08:00
keywords: [Git]
description: "用绑定IP的办法加快git速度，不在pull到一半就暂停啦"
tags: [Git]
categories: [VCS]
author: "筱氚"
---

# What
最近重建博客网站，需要pull代码下来删除重建，谁知道git clone下来老半天都没成功，根据各大网右的描述，有说是因为防火墙卡住了，也有说是github.global.ssl.fastly.net域名被限制了，无论是啥，大神们的方案都是找到域名对应的IP并绑定。
这里以windows系统为例，linux可以看参考一
# How
## 查询域名对应IP
参考一的使用nslookup命令查询，然而我使用这个命令查询的结果每次都不一样（还不如ping的准确）。最后使用这个网站[IPAddress](https://www.ipaddress.com/)查询域名对应的IP。
查询的结果为
```Bash
140.82.113.4    github.com
199.232.69.194  github.global.ssl.fastly.net
185.199.108.153    github.com
185.199.109.153    github.com
185.199.110.153    github.com
185.199.111.153    github.com
```
最后四个是assets-cdn.github.com的IP地址
## 绑定IP
Windows上的hosts文件路径在`C:\Windows\System32\drivers\etc\hosts`，用管理员权限打开该文件在末尾添加上上面查询的结果
## 刷新DNS缓存
Win+R输入cmd回车打开控制台，输入`ipconfig /flushdns`后回车即可，然后尝试下git clone吧
# Conclusion
1. 速度由几 KiB/s 提升到 20   KiB/s，也不会像上次一样pull到11%就停止了。。还不错。
2. 查询了一下nslookup的用法，需要加上domain才是查询一个域名的A记录，`nslookup domain github.com`。不加上domain的话用的是系统默认的DNS服务器。

# References
- [git clone速度太慢的解决办法（亲测还有效）](https://www.linuxidc.com/Linux/2019-05/158461.htm)
- [github clone或访问慢](https://www.cnblogs.com/zhaoqingqing/p/13109706.html)
- [IPAddress](https://www.ipaddress.com/)
- [nslookup命令详解](https://blog.csdn.net/violet_echo_0908/article/details/52033725)