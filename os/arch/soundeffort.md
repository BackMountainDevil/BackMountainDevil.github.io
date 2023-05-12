# Soundeffort 多设备音频输出 音效
- date: 2021-07-08
- lastmod: 2023-05-12

之前使用的是 pulseaudio，后面尝试切换为 pipewire。使用效果自己能接受就行

# 多设备音频输出

是因为考虑到两个人一块看视频且不方便外放的时候，搜索了一下发现有说用 paprefs 的，折腾了一下不太行，最后发现原因是 paprefs 是用来配置 pulseaudio 的，怪不得报错呢。然后`一刀斩の小窝`的博客提到了 helvum，在 wiki 中确认了是 pipewire 的图形化配置软件，我根据自己的桌面环境选择了 qpwgraph，启动软件把音源连线到想要输出的设备上即可

# 音效

没有杜比音效，那就整一个 linux 版的音效呗。安装 easyeffects 再安装音效即可，社区音效文件解压后放到对应路径即可，然后在软件里载入音效，就是效果听起来没啥差别。

# 参考

- [Dolby equivalent for Ubuntu](https://askubuntu.com/questions/984109/dolby-equivalent-for-ubuntu)
- [ ThePBone /Viper4Linux-GUI-Legacy ](https://github.com/ThePBone/Viper4Linux-GUI-Legacy#arch)：用校园网下载好几次也失败了
> yay -S viper4linux-gui-git 
- [wwmm / easyeffects](https://github.com/wwmm/easyeffects)
- [在 Linux 上迁移到全新的音频系统，PipeWire 使用攻略 一刀斩の小窝 2023](https://blog.yidaozhan.top/2022/08/18/pipewire/):pipewire helvum easyeffects
- [PipeWire - Arch Wiki](https://wiki.archlinux.org/title/PipeWire)