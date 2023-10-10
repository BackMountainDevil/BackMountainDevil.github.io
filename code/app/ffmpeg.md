# ffmpeg 基本命令
- date: 2022-07-02
- lastmod: 2023-10-10

## 常用命令

### 安装

针对 debian 系的 linux OS

```shell
sudo apt install ffmpeg
```

### 上手(FFmpeg FFprobe FFplay)

(1)查看ffmpeg的帮助说明，提供的指令

```shell
ffmpeg -h
```

(2)播放媒体的指令

```shell
ffplay video.mp4
ffplay music.mp3
```

快捷键

- 按键"Q"或"Esc"：退出媒体播放
- 键盘方向键：媒体播放的前进后退
- 点击鼠标右键：拖动到该播放位置
- 按键"F"：全屏
- 按键"P"或空格键：暂停
- 按键"W":切换显示模式

(3)查看媒体参数信息

```shell
ffprobe video.mp4
ffprobe -v quiet -print_format json -show_format -show_streams video.mp4
```

### 转换格式(文件格式,封装格式)

(1)文件名可以是中英文，但不能有空格。

(2)转换格式

```shell
ffmpeg -i video.mp4 video_avi.avi
```
-i 输入的文件

### 改变编码 上(编码,音频转码)

(1)查看可用编解码器

```shell
ffmpeg -codecs
```

(2)网站常用编码

- MP4封装：H264视频编码+ACC音频编码
- WebM封装：VP8视频编码+Vorbis音频编码
- OGG封装：Theora视频编码+Vorbis音频编码
- 音频无损编码为：WAV 和 AIFF，音频有损编码为：MP3 和 aac 

(3)无损编码格式.flac转换编码

```shell
ffmpeg -i music_flac.flac -acodec libmp3lame -ar 44100 -ab 320k -ac 2 music_flac_mp3.mp3
```

**说明：**

* acodec:audio Coder Decoder 音频编码解码器
* libmp3lame:mp3解码器
* ar:audio rate：音频采样率
* 44100:设置音频的采样率44100。若不输入，默认用原音频的采样率
* ab:audio bit rate 音频比特率
* 320k：设置音频的比特率。若不输入，默认128K
* ac: aduio channels 音频声道
* 2:声道数。若不输入，默认采用源音频的声道数

概括：设置格式的基本套路-先是指名属性，然后跟着新的属性值

### 改变编码 中(视频压制)

(1)视频转码

```shell
ffmpeg -i video.mp4 -s 1920x1080 -pix_fmt yuv420p -vcodec libx264 -preset medium -profile:v high -level:v 4.1 -crf 23 -acodec aac -ar 44100 -ac 2 -b:a 128k video_avi.avi
ffmpeg -i videoIn.mp4 -vcodec libx265 -r 30 videoOut.mp4
```

-vcodec 可以缩写为 -c:v

**说明:**

* -s 1920x1080：缩放视频新尺寸(size)
* -pix_fmt yuv420p：pixel format,用来设置视频颜色空间。参数查询：ffmpeg -pix_fmts
* -vcodec libx264：video Coder Decoder，视频编码解码器，可将 libx264 换成 libx265
* -preset medium: 编码器预设。参数：ultrafast,superfast,veryfast,faster,fast,medium,slow,slower,veryslow,placebo
* -profile:v high :编码器配置，与压缩比有关。实时通讯-baseline,流媒体-main,超清视频-high
* -level:v 4.1 ：对编码器设置的具体规范和限制，权衡压缩比和画质。
* -crf 23 ：设置码率控制模式。constant rate factor-恒定速率因子模式。范围0~51,默认23。数值越小，画质越高。一般在8~28做出选择。
* -r 30 :设置视频帧率
* -acodec aac :audio Coder Decoder-音频编码解码器
* -b:a 128k :音频比特率.大多数网站限制音频比特率128k,129k
  其他参考上一个教程

### 改变编码 下(码率控制模式)

ffmpeg支持的码率控制模式：-qp -crf -b

(1)  -qp :constant quantizer,恒定量化器模式 

无损压缩的例子（快速编码）

```shell
ffmpeg -i input -vcodec libx264 -preset ultrafast -qp 0 output.mkv
```

无损压缩的例子（高压缩比）

```shell
ffmpeg -i input -vcodec libx264 -preset veryslow -qp 0 output.mkv
```

(2) -crf :constant rate factor,恒定速率因子模式

(3) -b ：bitrate,固定目标码率模式。一般不建议使用

3种模式默认单遍编码

VBR(Variable Bit Rate/动态比特率) 例子
  veryslow 是真的非常非常慢

```shell
ffmpeg -i input -vcodec libx264 -preset veryslow output
```

ABR(Average Bit Rate/平均比特率) 例子

```shell
ffmpeg -i input -vcodec libx264 -preset veryslow -b:v 3000k output
```

CBR(Constant Bit Rate/恒定比特率) 例子，将码率压缩到 4M的案例，一个 12Mpbs 的片源想压到 3Mbps，最后压得 2.5

```shell
ffmpeg -i input.mp4 -vcodec libx265 -b:v 4000k  -maxrate 4500k -bufsize 4000k  -acodec copy  output.mp4
```

### 合并,提取音视频

(1)单独提取视频（不含音频流）

```shell
ffmpeg -i video.mp4 -vcodec copy -an video_silent.mp4
```

-an 表示剔除音频流

(2)单独提取音频（不含视频流）

```shell
ffmpeg -i video.mp4 -vn -acodec copy video_novideo.m4a
```

-vn 表示剔除视频流

-acodec 可以缩写为 -c:a

具备多个音频流的，如

Stream #0:2[0x81]:Audio:ac3,48000Hz,5.1,s16,384kb/s
Stream #0:3[0x82]:Audio:ac3,48000Hz,5.1,s16,384kb/s
Stream #0:4[0x80]:Audio:ac3,48000Hz,5.1,s16,448kb/s

针对性的单一的提取，例如提取第2条，用指令： -map 0:3

(3)合并音视频

```shell
ffmpeg -i video_novideo.m4a -i video_silent.mp4 -c copy video_merge.mp4
```

### 截取，连接音视频

1. 截取视频长度

```shell
ffmpeg -i music.mp3 -ss 00:00:30 -to 00:02:00 -acodec copy -copyts music_cutout.mp3
```

-c copy 用原来的编码并复制到新文件中，测试表明只有截取时间段参数时码率会被压缩，因此最好是加上复制原编码
-ss	开始时间
-t	持续时间     -t xx           // 单位：秒    -t xx:xx:xx  // 时:分:秒
-to 到达某个时间 xx:xx:xx
如果不表示持续时间或者终止时间，则默认持续到末尾


截取60秒

```shell
ffmpeg -i music.mp3 -ss 00:00:30 -t 60 -acodec copy music_cutout60s.mp3
```

-sseof : 从媒体末尾开始截取

```shell
ffmpeg -i in.mp4 -ss 00:01:00 -to 00:01:10 -c copy out.mp4
ffmpeg -ss 00:01:00 -i in.mp4 -to 00:01:10 -c copy out.mp4
ffmpeg -ss 00:01:00 -i in.mp4 -to 00:01:10 -c copy -copyts out.mp4
```

把-ss放到-i之前，启用了关键帧技术，加速操作。但截取的时间段不一定准确。可用最后一条指令，保留时间戳，保证时间准确。

2. 裁剪视频画面(修改画布大小)

`-vf crop=w:h:x:y` 四个参数分别代表输出宽度、输出高度、方框左上角距离左侧边缘的距离、方框左上角距离上边缘的距离

比如一个 900x720 的视频，只想要竖排三等分中间的那份

`ffmpeg -i in.mp4 -vf crop=300:720:300:0 out.mp4`

3. 连接音视频

```shell
ffmpeg -i "concat:01.mp4|02.mp4|03.mp4" -c copy out.mp4
```

不同格式的音视频可以连接在一起，但不推荐不同格式连接在一起。
建议使用Avidemux软件连接

### 截图,水印,动图

(1)截图.

截取第7秒第1帧的画面

```shell
ffmpeg -i video.mp4 -ss 7 -vframes 1 video_image.jpg
```

(2)水印

```shell
ffmpeg -i video.mp4 -i qt.png -filter_complex "overlay=20:80" video_watermark.mp4
```

(3)截取动图

```shell
ffmpeg -i video.mp4 -ss 7.5 -to 8.5 -s 640x320 -r 15 video_gif.gif
```

10.录屏,直播

(1)录屏

windows: 

```shell
ffmpeg -f gdigrab -i desktop rec.mp4
```

ubuntu: 

```shell
sudo ffmpeg -f fbdev -framerate 10 -i /dev/fb0 rec.mp4
```

gdigrab ：ffmpeg中的一个组件。

只捕获视频.若要录屏，录音，获取摄像头，麦克风，换组件，用OBS Studio软件

(2)直播

```shell
ffmpeg -re i rec.mp4 按照网站要求编码 -f flv "你的rtmp地址/你的直播码"
```

# 视频参数
## 帧率

## 分辨率

- 扫描：P逐行扫描 I隔行扫描。如 1080p 与 1080i

## 码率

看了下手头的视频，1920x1080p 的码率有 12.4、6.01、4.41、1.17 Mbit/s。太低就会糊，太高文件体积大，如何找到合适的码率进行存储是一个问题。

# 参考
- [【FFmpeg 分P教学】转码、压制、录屏、裁切、合并、提取 … 统统不是问题。](https://www.bilibili.com/video/av40146374)

- [ffprobe输入与输出信息详解 HugoforAndroid 于 2020-06-04](https://blog.csdn.net/lipengshiwo/article/details/106552579)

- [ffmpeg：码率控制模式、编码方式. ETalien_ 于 2020-03-08](https://blog.csdn.net/ETalien_/article/details/102931065)

- [完全解析：编码与封装 2016-03-1 影视飓风](https://www.bilibili.com/video/av4101573)

- [【硬核科普】奇妙的帧率增加了！2020-03-21 影视飓风](https://www.bilibili.com/video/BV1kE411c7yZ)

- [视频编码完全指南 K.R. Vijayanagar LiveVideoStack 2021-10-09](https://mp.weixin.qq.com/s?__biz=MzU1NTEzOTM5Mw==&mid=2247515283&idx=1&sn=1f6ac100ad020fb8e8238aefa5e89d19&chksm=fbda14bdccad9dabe8dfe7e90401acb04d906c56805c76f72fab242b763efd6aca70b4884e07&scene=21#wechat_redirect)

- [ffmpeg-libav-tutorial](https://github.com/leandromoreira/ffmpeg-libav-tutorial)

- [VapourSynth（VS） 视频处理压制入门教程 2021-07-01 EddyStorm](https://www.bilibili.com/read/cv11955002/)

- [最强 AI 人声伴奏分离工具 UVR5，更新5.5版本 2023年02月23日 略懂的大龙猫](https://www.bilibili.com/read/cv21997904/)

- [如何屏蔽每次调用ffmpeg都要显示的一大段文件信息？](https://www.zhihu.com/question/604564230):`-loglevel quiet` 可以省略绝大多数信息，是否覆盖会显示询问，缺点是不会显示进度

- [FFmpeg H.264编码器指南[译] 2023-10-10 乔达摩(嘿~) ](https://www.cnblogs.com/xiaxiaolu/p/17752560.html)
