# OCR:tesseract, paddleocr
- date: 2023-12-09

OCR(Optical Character Recognition):光学字符识别

# tesseract

[tesseract](https://github.com/tesseract-ocr/tesseract)

arch 里安装好 tesserac、tesseract-data-chi_sim、tesseract-data-eng

指令格式： tesseract imagename outputbase [-l lang] [--oem ocrenginemode] [--psm pagesegmode] [configfiles...]

参数含义：
- imagename 输入文件名
- outputbase 输入文件名，默认后缀 .txt，不需要加后缀，不然会重复
-l 指定语言，可指定多个。可以使用 tesseract --list-langs 查看本机安装了哪些语言包

例子： tesseract test.png ocr_result -l chi_sim+eng
例子含义：使用中英语言识别 test.png 中的字符，并将结果存到 ocr_result.txt 文件中。如果不想保存到文件，而是输出的话，把 ocr_result 修改为 stdout 或者 -，如果不想输出调试信息，可以加上参数 -c debug_file=/dev/null

一些第三方的 [GUI](https://tesseract-ocr.github.io/tessdoc/User-Projects-%E2%80%93-3rdParty.html) 列表，比如 normcap 可以做到启动截图然后把识别结果复制到剪切板并通知用户，但是 normcap 调用 tesseract 识别感觉没有我自己截图识别的效果好，normcap 的好处就是把识别结果去掉空格和换行了

封装：scrot -s -e 'tesseract $f stdout -l chi_sim -c debug_file=/dev/null | xclip -sel clip'  

TODO : 去掉空格换行

# PaddleOCR

[PaddleOCR](https://www.paddlepaddle.org.cn/hub/scene/ocr)

CPU 版本安装：pip install paddlepaddle "paddleocr>=2.0.1"

使用： paddleocr --image_dir test.png --use_angle_cls true --use_gpu false

tesseract v5.3.3-1 使用起来比 paddleocr v2.7.0.3 快一些，但是起识别中文效果不如 paddleocr

## 封装

linux 上把下面代码保存为 ocr，然后给可执行权限。安装 scrot，用来截图，指令为 scrot -s -f /tmp/.scrot_ocr_temp.png -o -q 100 -e '/home/mifen/.conda/envs/310/bin/ocr -i $f'。然后给这个指令加键盘快捷方式，通过组合按键调用截图，然后指令会把截图存到指定位置，然后把路径传给下面代码调用 paddleocr 进行识别文字然后粘贴到剪贴板，每次调用会覆盖上一次的图片

```python
#!/home/mifen/.conda/envs/310/bin/python
# -*- coding: utf-8 -*-
"""
识别图片里的文字，只要结果，不要位置信息、置信度

filename: ocr

usage: ocr -i test.png
"""
from paddleocr import PaddleOCR
import pyperclip
import argparse

parser = argparse.ArgumentParser(description='Get image parameter')
parser.add_argument('-i', '--image_dir', type=str, required=True, help='Path to the image')
args = parser.parse_args()

img_path = args.image_dir
ocr = PaddleOCR(use_angle_cls=True, lang="ch")
result = ocr.ocr(img_path, cls=True)
for idx in range(len(result)):
    res = result[idx]
    ret=''
    for line in res:
        ret+=line[1][0]
    pyperclip.copy(ret) # 将字符串粘贴到剪贴板
```


## 相关阅读

[安装 paddlepaddle](https://www.paddlepaddle.org.cn/install/quick?docurl=/documentation/docs/zh/install/pip/linux-pip.html)

[paddleoc quickstart](https://gitee.com/paddlepaddle/PaddleOCR/blob/release/2.6/doc/doc_ch/quickstart.md#11)

[pypi paddleoc](https://pypi.org/project/paddleocr/)
