---
title: "Soundeffort"
date: 2021-07-08T20:11:13+08:00
lastmod: 2021-07-08T20:11:13+08:00
keywords: []
description: ""
tags: []
categories: [Blog]
author: "筱氚"
---

# 参考
- [Dolby equivalent for Ubuntu](https://askubuntu.com/questions/984109/dolby-equivalent-for-ubuntu)
- [ ThePBone /Viper4Linux-GUI-Legacy ](https://github.com/ThePBone/Viper4Linux-GUI-Legacy#arch)：用校园网下载好几次也失败了
> yay -S viper4linux-gui-git 
- [wwmm / easyeffects](https://github.com/wwmm/easyeffects)
```bash
sudo pacman -Sy pulseeffects
正在解析依赖关系...
正在查找软件包冲突...
:: pipewire-pulse 与 pulseaudio 有冲突。删除 pulseaudio 吗？ [y/N] y
:: pipewire-pulse 与 pulseaudio-bluetooth 有冲突。删除 pulseaudio-bluetooth 吗？ [y/N] y

软件包 (13) calf-0.90.3-4  gst-plugin-gtk-1.18.4-2  gst-plugin-pipewire-1:0.3.31-1  lsp-plugins-1.1.30-1
            pipewire-media-session-1:0.3.31-1  pipewire-pulse-1:0.3.31-1  pulseaudio-14.2-3 [删除]  pulseaudio-bluetooth-14.2-3 [删除]
            rnnoise-0.4.1-1  yelp-40.2-1  yelp-xsl-40.2-1  zita-convolver-4.0.3-2  pulseeffects-5.0.4-1

下载大小：      33.92 MiB
全部安装大小：  83.42 MiB
净更新大小：    77.05 MiB
```