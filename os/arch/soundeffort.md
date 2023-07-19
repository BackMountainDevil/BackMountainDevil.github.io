# Soundeffort 多设备音频输出 音效
- date: 2021-07-08
- lastmod: 2023-07-19

之前使用的是 pulseaudio，后面尝试切换为 pipewire。使用效果自己能接受就行

# 多设备音频输出

是因为考虑到两个人一块看视频且不方便外放的时候，搜索了一下发现有说用 paprefs 的，折腾了一下不太行，最后发现原因是 paprefs 是用来配置 pulseaudio 的，怪不得报错呢。然后`一刀斩の小窝`的博客提到了 helvum，在 wiki 中确认了是 pipewire 的图形化配置软件，我根据自己的桌面环境选择了 qpwgraph，启动软件把音源连线到想要输出的设备上即可

qpwgraph(0.5.1) 有保存功能并且可以持久化设置，在vlc、mpv上测试良好，感谢作者的不断更新。

helvum(0.34-1) 暂时不支持设置持久化，切歌切视频就要重新设置，代码仓库有人很久前就提了这个需求了。catia(cadence 0.9.2-2) 是在 qpwgraph 的README中看到的，简单使用了一下没明白怎么设置双音频输出，就把这个排除了。而系统混音器（pavucontrol）目前只支持单个设备输出。可以肯定的是 pipewire(0.3.71-2) 支持设置多个音频输出

> https://wiki.debian.org/PipeWire
>
> playing audio through multiple audio output devices at the same time

## pactl

sources：输入设备（麦克风）
sinks：输出设备
sink-inputs：输出设备的音源

`pactl list sinks` 证实蓝牙耳机的 sink id 不固定， `pactl list sink-inputs` 查看 sink 的输入，证明vlc切歌的时候编号变了。假设 vlc 播放时的 sink-inputs id 是 423，有线耳机、蓝牙耳机的 sinks id 分别是 48、393,通过 `pactl move-sink-input 381 48` 可以把音频输出到有线耳机，而 `pactl move-sink-input 381 393` 可以把音频输出到蓝牙耳机

## pw-link

靠命令将一个音频输出到多个设备。通过 pw-link -li 和 pw-link -lo 查看输入输出对应关系，在系统混音器切换不同的设备输出后再查看输出关系作为对比。然后用 pw-link 创建链接（不幸的是，这样的链接在切歌之后会断开），如下所示

```bash
# 此时 vlc 将音频输出到蓝牙耳机，有线耳机是没有声音的
pw-link "VLC media player (LibVLC 3.0.18):output_FL" "alsa_output.pci-0000_00_1f.3.analog-stereo:playback_FL" # 连接左声道
pw-link "VLC media player (LibVLC 3.0.18):output_FR" "alsa_output.pci-0000_00_1f.3.analog-stereo:playback_FR" # 连接右声道
```

# 音效

没有杜比音效，那就整一个 linux 版的音效呗。安装 easyeffects 再安装音效即可，社区音效文件解压后放到对应路径即可，然后在软件里载入音效，就是效果听起来没啥差别。

## 降噪

[noise-suppression-for-voice](https://github.com/werman/noise-suppression-for-voice):README有步骤，效果没有那么惊艳

[noisetorch](https://github.com/noisetorch/NoiseTorch)

# 参考

- [Dolby equivalent for Ubuntu](https://askubuntu.com/questions/984109/dolby-equivalent-for-ubuntu)
- [ ThePBone /Viper4Linux-GUI-Legacy ](https://github.com/ThePBone/Viper4Linux-GUI-Legacy#arch)：用校园网下载好几次也失败了
> yay -S viper4linux-gui-git 
- [wwmm / easyeffects](https://github.com/wwmm/easyeffects)
- [在 Linux 上迁移到全新的音频系统，PipeWire 使用攻略 一刀斩の小窝 2023](https://blog.yidaozhan.top/2022/08/18/pipewire/):pipewire helvum easyeffects
- [PipeWire - Arch Wiki](https://wiki.archlinux.org/title/PipeWire)

[PulseAudio实用手册 李花开 2022-12-06](https://zhuanlan.zhihu.com/p/589527476)

[pw-link. ManPage](https://docs.pipewire.org/page_man_pw_link_1.html)

[How to achieve (automated) simultaneous outputs with Pipewire? 2021.12.5](https://askubuntu.com/questions/1379376)
> https://gitlab.freedesktop.org/pipewire/pipewire/-/wikis/Migrate-PulseAudio#etcpulsedefaultpa
>
> https://docs.pipewire.org/page_module_combine_stream.html#autotoc_md177
