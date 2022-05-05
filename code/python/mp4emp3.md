# 借助 ffmpeg 从视频中批量提取音频后做字幕
- date: 2022-04-27
- lastmod: 2022-05-05

## 前言

下了一部小众外文连续剧，在那几个经典字幕站没发现对应的中文字幕，干脆连字幕都没有。虽然略懂外文，但是要跟上速度和理解还不是很好，就想着借助现代科技的力量实现加字幕。

目前有这么两个方案：

1. AI 实时字幕。参考 Youtube、Bilibili 的 CC 字幕，据说华为也有，但是我没有华为设备，版权问题也不可能上传的。
2. 先从视频中提取音频，再用 AI 对音频做语音识别，自动打点翻译生成字幕文件。不直接用视频是因为音频体积更小，节约识别时间，这里采取这个办法

## 代码

m4a 比 mp3 体积小很多，但是网易见外暂不支持 m4a 格式的音频文件

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

修改序号的话将 L86 替换为  srt_strs.append(int(rlines[i].strip())+行号偏移量)。假设后移 911 行就是： srt_strs.append(int(rlines[i].strip())+911)
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

    def parse(self, item_strs):
        """解析时间"""
        srt_item = subtitle_item()
        srt_item.index = int(item_strs[0])
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
            line = rlines[i].strip()  # 不懂干啥？后面都没用 line
            if rlines[i].strip() == "":
                i += 1
                continue

            srt_strs = []

            srt_strs.append(
                rlines[i].strip()
            )  # 序号, srt_strs.append(int(rlines[i].strip())+行号偏移量)
            i += 1
            srt_strs.append(rlines[i].strip())  # 时间
            i += 1

            text_str = ""  # 字幕所含的文字

            try:
                """这个异常我也没调明白，调试中 i 明显没溢出，但是愣是报错溢出"""
                while rlines[i].strip() != "":
                    text_str = text_str + rlines[i]
                    i += 1
            except Exception as e:
                print("Exception: ", e)

            srt_strs.append(text_str)  # 一小段字幕（对应三行）

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