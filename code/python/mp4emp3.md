# 借助 ffmpeg 从视频中批量提取音频后做字幕 whisper
- date: 2022-04-27
- lastmod: 2023-08-01

## 前言

下了一部小众外文连续剧，在那几个经典字幕站没发现对应的中文字幕，干脆连字幕都没有。虽然略懂外文，但是要跟上速度和理解还不是很好，就想着借助现代科技的力量实现加字幕。

目前有这么两个方案：

1. AI 实时字幕。参考 Youtube、Bilibili 的 CC 字幕，据说华为也有，但是我没有华为设备，版权问题也不可能上传的。
2. 先从视频中提取音频，再用 AI 对音频做语音识别，自动打点翻译生成字幕文件。不直接用视频是因为音频体积更小，节约识别时间，这里采取这个办法

## 代码

m4a 比 mp3 体积小很多，但是网易见外暂不支持 m4a 格式的音频文件

```python
""" 
作用：用 ffmpeg 从 mp4视频 中批量提取 mp3音频，若想提取 m4a，就把mp3换m4a
运行：安装 ffmpeg ，把代码放在视频目录中，用 python3 运行即可
"""
import os

for dirpath, dirnames, filenames in os.walk(os.getcwd()): # os.getcwd() 为当前代码所在位置
    for filename in filenames:  # 遍历所有文件，包括子文件夹
        try:
            if filename[-3:] == "mp4": # 后缀鉴别是否是 mp4 文件
                cmd = "ffmpeg -i  \"{}\" -vn \"{}.mp3\"".format(filename, filename[:-4]) # 提取 mp3
                os.system (cmd)
        except Exception as e:
            print("Exception: ",e,". Filename: ", filename)
```

## AI 字幕

可以使用每日免费提取两小时的[网易见外](https://jianwai.youdao.com)，当然也可以用互联网公司的字幕提取相关 API。

### whisper

现在更推荐使用开源的 whisper，支持输入视频，可以本地部署来跑，远程跑的话建议先剥离音频减小网络传输时间，也不会限制音频时长。whisper 运行的时候会自动下载模型，默认生成所有格式的数据，默认导出位置为当前目录。`--verbose False` 将会取消转译的调试输出，只显示进度。`--language` 指定语言，zh和Chinese。最大的模型large-v2要占用显存 11677MiB，同时跑n个就要n倍，默认在0卡上跑，指定在1卡跑的参数为 `--device cude:1`。

`whisper --model large-v2 --output_dir audio/srt --output_format srt --verbose False --language zh audio/audio.m4a`

openai-whisper(20230314) 指定中文可能输出简体/繁体，可参考[Simplified Chinese rather than traditional](https://github.com/openai/whisper/discussions/277)中加入参数 `--initial_prompt "以下是普通话的句子"`

### whisper.cpp

没有显卡可以试试 [whisper.cpp](https://github.com/ggerganov/whisper.cpp)，用 cpu 跑，内存占用更低。这里的 large 模型指的是 openai的 large-v2（自己转换了一遍对比shasum才发现的）。

下面使用 medium vs large 中，large 输出一个长句子，而 medium 输出了两段，并且 large 的时间轴精度为一秒，medium 要更加精细

```bash
$ shasum large-v2.pt 
d7a2f7bcc4655a8723162b5810a6f7665794feeb  large-v2.pt
$ shasum ggml-large-v2.bin 
0f4c8e34f21cf1a914c59d8b3ce882345ad349d6  ggml-large-v2.bin
$ shasum ggml-medium.bin 
fd9727b6e1217c2f614f9b698455c4ffd82463b4  ggml-medium.bin

$ ./main -m models/ggml-large-v2.bin  -f samples/jfk.wav
[00:00:00.000 --> 00:00:11.000]   And so my fellow Americans, ask not what your country can do for you, ask what you can do for your country.

$ ./main -m models/ggml-medium.bin  -f samples/jfk.wav
[00:00:00.000 --> 00:00:08.940]   And so, my fellow Americans, ask not what your country can do for you, ask what you
[00:00:08.940 --> 00:00:10.440]   can do for your country.
```

# 后记

把某系列视频的音频都提取出来后，用见外提取音频里的文字并自动翻译打轴生成 srt 字幕文件，目前是每日免费两小时，对于临时使用完全足够。

对于超过两小时但不超过四小时的音频，目前我是用 ffmpeg 将其分为两段

```bash
ffmpeg -i music.mp3 -ss 00:00:00 -to 02:00:00 -acodec copy -copyts musicp1.mp3
ffmpeg -i music.mp3 -ss 02:00:00 -acodec copy -copyts musicp2.mp3
```

然后一天传一段，两天后把 p2 字幕的时间轴和序号都调整一下在复制粘贴合并为一个。

<details>
<summary>srt 字幕调时调序号代码</summary>

```python
""" 
小工具： 调整外置文本字幕的时间 gushansanren  2021-12-26 https://blog.csdn.net/gushansanren/article/details/122154065
如将 input.srt 的时间轴统一后移 2h（7200s），保存为 output.srt
python main.py -i input.srt -o output.srt -t 7200

修改序号的话修改 L57 parse(self, item_strs, lineOffset=0) 的第二个参数的值
"""
import os
import sys
import argparse
from datetime import datetime, timedelta
import pathlib
import codecs

from abc import abstractmethod


class subtitle_item(object):
    def __init__(self):
        super().__init__()
        self.index = 0
        self.stime = 0
        self.etime = 0
        self.text = ""


class subtitle_imp(object):
    def __init__(self) -> None:
        super().__init__()
        self._subItems = []

    @abstractmethod
    def load_file(self, input_file):
        pass

    @abstractmethod
    def save_file(self, output_file=None):
        pass

    def adjust_time(self, ad_time):
        """调整字幕时间"""
        for sub_tmp in self._subItems:
            sub_tmp.stime += timedelta(seconds=ad_time)
            sub_tmp.etime += timedelta(seconds=ad_time)

    def set_sub_items(self, items):
        self._subItems = items

    def get_sub_items(self):
        return self._subItems


class srt_sub_imp(subtitle_imp):  # 子类
    def __init__(self):
        super().__init__()

    def parse(self, item_strs, lineOffset=0):
        """解析一小段字幕序列
        item_strs:一段字幕序列，比如 ['0', '00:00:03,540 --> 00:00:05,670', '哦\nOh,\n']
        lineOffset:字幕序号偏移量（通常用在文件合并中），默认不偏移；比如 425"""
        srt_item = subtitle_item()
        srt_item.index = int(item_strs[0]) + lineOffset
        srt_item.text = item_strs[2]

        time_strs = item_strs[1].split("-->")

        srt_item.stime = datetime.strptime(time_strs[0].strip(), "%H:%M:%S,%f")
        srt_item.etime = datetime.strptime(time_strs[1].strip(), "%H:%M:%S,%f")
        return srt_item

    def load_file(self, input_file):
        """读取文件内容"""
        rlines = []
        with open(input_file, "r", encoding="utf8") as f:
            rlines = f.readlines()
        data = rlines[0].encode(encoding="utf-8")
        if data[:3] == codecs.BOM_UTF8:
            rlines[0] = data[3:].decode(encoding="utf-8")
        i = 0
        while i < len(rlines):
            if rlines[i].strip() == "":
                i += 1
                continue

            srt_strs = []
            srt_strs.append(rlines[i].strip())  # 序号
            i += 1
            srt_strs.append(rlines[i].strip())  # 时间
            i += 1

            text_str = ""  # 字幕所含的文字
            try:
                """这个异常在最后一行报错溢出 list index out of range，原因如下帖子的第二个
                https://blog.csdn.net/qq_43082153/article/details/108579168"""
                while rlines[i].strip() != "":
                    text_str = text_str + rlines[i]
                    i += 1
            except Exception as e:
                print("Exception: ", e)
            srt_strs.append(text_str)  # 一小段字幕（对应三行或者四行[双语]）

            self._subItems.append(self.parse(srt_strs))

    def save_file(self, output_file=None):
        """保存文件"""
        with open(output_file, "w", encoding="utf8") as f:
            for sub_tmp in self._subItems:
                f.write("%d\n" % (sub_tmp.index))
                f.write(
                    "%s --> %s \n"
                    % (
                        sub_tmp.stime.strftime("%H:%M:%S,%f")[:-3],
                        sub_tmp.etime.strftime("%H:%M:%S,%f")[:-3],
                    )
                )
                f.write("%s\n" % (sub_tmp.text))


def gen_subtitle_imp_by_name(filename):
    """检查srt文件是否存在，是就返回 srt_sub_imp 对象"""
    filterstr = pathlib.Path(filename).suffix  # 文件后缀提取
    if filterstr == ".srt":
        return srt_sub_imp()
    else:
        return None


def init_arg_table():
    """init argument table,return args."""
    parse = argparse.ArgumentParser(
        description="subtitle tools.",
        epilog="Author: renyi.zhang <renyi.zhang@amlogic.com>",
        fromfile_prefix_chars="@",
    )

    parse.add_argument(
        "-i",
        "--input",
        type=str,
        required=True,
        dest="inputfile",
        help="input subtitle file",
    )
    parse.add_argument(
        "-o",
        "--output",
        default="",
        type=str,
        required=False,
        dest="outputfile",
        help="output subtitle file",
    )
    parse.add_argument(
        "-t",
        "--time",
        default=0,
        type=float,
        required=False,
        dest="ad_time",
        help="adjustment time(seconds) ",
    )

    return parse.parse_args()


if __name__ == "__main__":
    args = init_arg_table()

    if not os.path.isfile(args.inputfile):
        print(args.inputfile + " isn't exist.\n")
        sys.exit(-1)

    if args.outputfile == "":
        args.outputfile = args.inputfile

    sub_imp = gen_subtitle_imp_by_name(args.inputfile)
    if sub_imp is None:
        print(args.inputfile + "is invalid subtitle file.\n")
        sys.exit(-1)

    sub_imp.load_file(args.inputfile)

    if args.ad_time != 0:
        sub_imp.adjust_time(args.ad_time)

    if pathlib.Path(args.inputfile).suffix == pathlib.Path(args.outputfile).suffix:
        sub_imp.save_file(args.outputfile)
    else:
        print("other format will support later..\n")

    print("save file to  %s  finished.\n " % (args.outputfile))
```

在源代码的基础上添加了异常处理、序号偏移、添加参数时间单位。将上述代码保存为 main.py。运行 `python main.py -h` 可以看到参数提示

</details>

# 参考

[ffmpeg批量提取mp4视频文件中的音频 mj412828668 2021-10-22 ](https://blog.csdn.net/mj412828668/article/details/120914158):-y：表示在处理过程中跳过确认提示（如是否覆盖）。-codec copy：表示直接复制输入文件的编码方式，不进行任何转换。-q:v 1：表示设置视频质量为 1，其中 1 表示最高质量
> ffmpeg -i "%%~sa" -y -vn -codec copy -q:v 1 "%%~na.m4a"

[FFmpeg从视频中提取音频保存为mp3文件 Geek.Fan 2020-10](https://blog.csdn.net/fanyun_01/article/details/109408501):-vn 表示剔除视频流。-f mp3 指定输出文件的格式为 MP3 音频
> ffmpeg -i test.mp4 -f mp3 -vn test.mp3
