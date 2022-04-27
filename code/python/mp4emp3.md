# 借助 ffmpeg 从视频中批量提取音频后做字幕
- date: 2022-04-27
- lastmod: 2022-04-27

## 前言

下了一部小众外文连续剧，在那几个经典字幕站没发现对应的中文字幕，干脆连字幕都没有。虽然略懂外文，但是要跟上速度和理解还不是很好，就想着借助现代科技的力量实现加字幕。

目前有这么两个方案：

1. AI 实时字幕。参考 Youtube、Bilibili 的 CC 字幕，据说华为也有，但是我没有华为设备，版权问题也不可能上传的。
2. 先从视频中提取音频，再用 AI 对音频做语音识别，自动打点翻译生成字幕文件。不直接用视频是因为音频体积更小，节约识别时间，这里采取这个办法

## 代码


```python
""" 
作用：用 ffmpeg 从视频中批量提取音频，默认提取 mp3，若想提取 m4a，把两行 cmd 的注释对调即可
运行：安装 ffmpeg ，把代码放在视频目录中，用 python3 运行即可
ffmpeg -i test.mp4 -f mp3 -vn test.mp3      https://blog.csdn.net/fanyun_01/article/details/109408501
ffmpeg -i "%%~sa" -y -vn -codec copy -q:v 1 "%%~na.m4a"     https://blog.csdn.net/mj412828668/article/details/120914158
"""
import os

for dirpath, dirnames, filenames in os.walk(os.getcwd()): # os.getcwd() 为当前代码所在位置
    for filename in filenames:  # 遍历所有文件，包括子文件夹
        try:
            if filename[-3:] == "mp4": # 后缀鉴别是否是 mp4 文件
                cmd = "ffmpeg -i \"{}\" -f mp3 -vn \"{}.mp3\"".format(filename, filename[:-4])  # 提取 mp3
                # cmd = "ffmpeg -i  \"{}\" -vn -codec copy -q:v 1 \"{}.m4a\"".format(filename, filename[:-4]) # 提取 m4a
                os.system (cmd)
        except Exception as e:
            pass
            print("Exception: ",e,". Filename: ", filename)
```

## AI 字幕

目前使用的是每日免费提取两小时的[网易见外](https://jianwai.youdao.com)，当然也可以用互联网公司的字幕提取相关 API。