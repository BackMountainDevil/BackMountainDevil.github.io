---
title: "把系统中的 296 款字体删除到只剩下 17 款"
date: 2020-11-15T12:27:18+08:00
lastmod: 2020-11-15T12:27:18+08:00
keywords: ['Font']
description: ""
tags: []
categories: [Writing]
author: "筱氚"
---
# Intro
`fc-list` 可以查看本机有哪些字体，但是输出有点多，我这里将近两千行（1997）。可以用 `fc-list >temp.txt` 将输出定向到 temp.txt。可以显然看出开头的路径都是 `/usr/share/fonts/`（如果没有单独安装用户字体的话）。

在 [Font configuration Arch Wiki](https://wiki.archlinux.org/title/Font_configuration) 中提到了字体配置文件的路径。
> Fontconfig configuration globally `/etc/fonts/local.conf`

手动编辑文件配置必然不是一种现代化操作，我用的是 KDE 桌面环境，因此可以在 “设置-外观-字体管理” 中进行管理，全选全部字体，在右下角显示了计数，我这里是 296 款字体（其中三款用户字体：仿宋 GB2312、方正宋体、楷体）。我记忆中安装过 ttf-sarasa-gothic texlive-langchinese wps-office-mui-zh-cn ttf-wps-fonts 。noto-fonts 则不知道是啥时候安装的了。

## 烦恼
这都是在 WPS 中遇到的问题

1. 字体选择下拉框长的一皮，一大堆 Noto、Sarasa
2. 缺少学校要求的字体：隶书、宋体
3. 选中文本可以识别宋体，然而无法在字体中选择宋体

## 简化
我需要什么字体？至少一款中文字体和一款英文字体，除此之外还有数字和符号字体。
中文：思源黑体简体中文 + 思源宋体简体中文
英文：GNU FreeFont  -  free sans, sans serif and monospace fonts.  not include CJK
等宽字体(Monospaced): GNU FreeFont. 终端编程字体多用等款字体，Plasma 默认使用 Hack，我现在用 FreeMono
Emoji：Google's open-source Emoji. Plasma 有 emoji 表情选择器，不怕输入法打不出来
数字和公式符号字体：暂无特殊需求

## 卸载字体
wps 的两个我还要留着用，textlive 的尝试卸载没发现这个包，可能早就卸载掉了吧。

```bash
sudo pacman -R ttf-sarasa-gothic
sudo pacman -R noto-fonts
删除 noto-fonts 破坏依赖 'noto-fonts' （plasma-integration 需要）
```
这下子只好到字体管理看心情删除那几个不顺眼的 noto 了,现在全部字体 251 款，重启也正常。卸载不了的字条包可以在字体管理里删除，全选系统字体删除即可（全部删除后再装一些必要的）

根据 [Fonts (简体中文) Arch wiki](https://wiki.archlinux.org/title/Fonts_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E5%AD%97%E4%BD%93%E7%B1%BB%E5%9E%8B) 中的说法，然而我并没有找到这几个缓存文件，可能是我没用 Matplotlib 吧。
  > Matplotlib (python-matplotlib 或 python2-matplotlibAUR) 使用自己的字体高速缓冲，因此更新字体后记得删除 $HOME/.matplotlib/fontList.cache，$HOME/.cache/matplotlib/fontList.cache, $HOME/.sage/matplotlib-1.2.1/fontList.cache 等文件。这样它才会再一次产生高速缓冲并找到新字体

## 加装
Sans-serif：无衬线字体  
Serif：衬线字体  

[“无衬线字体”和“衬线字体”. 中流自在心](https://zhuanlan.zhihu.com/p/368830068)：把两者讲得还挺简单易懂
  > 无衬线字体：指的是笔画粗细一致，没有笔锋的字体。比如微软雅黑、思源黑体。装饰性一般，但阅读性较好  
  衬线字体：指的是笔画粗细不一，有明显笔锋的字体。这类字体的装饰性很强，但作为文章正文排布的时候，会略显凌乱，在阅读上有所欠缺，在商务型PPT中不适用，但可以用在少量文字装饰的场合。

```bash
# GNU FreeFont
sudo pacman -S gnu-free-fonts
# Google's open-source Emoji
sudo pacman -S noto-fonts-emoji
# 思源宋体简体中文部分 衬线字体
sudo pacman -S adobe-source-han-serif-cn-fonts
# 思源黑体简体中文部分 无衬线字体
sudo pacman -S adobe-source-han-sans-cn-fonts
# 针对 WPS 的公式字体
yay -S ttf-wps-fonts
```

C:/Windows/Fonts  
Arial: arial.ttf
Times New Roman: times.ttf
隶书：SIMLI.TTF
删删装装最后 12 款系统字体，2 款用户字体。

# 版权与商用
## 免费可商用
Times New Roman
隶书

## 需授权
仿宋
宋体
新宋体
黑体
楷体
方正系列
华文系列
微软系列

# 参考
- [WPS. Arch Wiki](https://wiki.archlinux.org/title/WPS_Office_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))：后两个是中文字体和公式符号字体
  > yay -S wps-office-cn wps-office-mui-zh-cn ttf-wps-fonts
- [360查字体](https://fonts.safe.360.cn/)
- [Font configuration Arch Wiki](https://wiki.archlinux.org/title/Font_configuration)
- [Fonts (简体中文) Arch wiki](https://wiki.archlinux.org/title/Fonts_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E5%AD%97%E4%BD%93%E7%B1%BB%E5%9E%8B)
- [Pandoc 使用及踩坑. 点半九.  2019-08-31](https://www.dianbanjiu.com/post/pandoc-%E4%BD%BF%E7%94%A8%E5%8F%8A%E8%B8%A9%E5%9D%91/)
  > 使用 fc-list :lang=zh 命令查看一下你自己系统上都安装了哪些中文字体：
- [“无衬线字体”和“衬线字体”. 中流自在心](https://zhuanlan.zhihu.com/p/368830068)
- [Font Manager：一个简单的 GTK+ 桌面的开源应用 作者： Ankush Das 译者： LCTT geekpi| 2020-12-30](https://linux.cn/article-12968-1.html):yay -S font-manager
  > 添加字体源，以便在安装前进行预览
  开箱即用的谷歌字体集成
- [font-manager Github](https://github.com/FontManager/font-manager)
  > While designed primarily with the Gnome Desktop Environment in mind, it should work well with other GTK desktop environments.
  Font Manager is NOT a professional-grade font management solution.
- [如何在 Linux 上管理字体 Linux中国](https://zhuanlan.zhihu.com/p/51941625):KDE 桌面自带的字体管理工具，GNOME 中的 Font Manager
- [在linux下管理字体](https://www.cnblogs.com/hugetong/p/6103526.html):中文显示用 adobe-source
