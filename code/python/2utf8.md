# 文本编码另存为 utf-8（字幕编码格式转换
- date: 2022-12-07
- lastmod: 2022-12-07

# 起因

下载的字幕压缩包是 gbk 编码，vlc 直接导入会显示乱码，转换为 utf-8 编码之后正常显示，因此需要将所有字幕都转换，一个个点开再另存为太繁琐了，考虑还是十多季的字幕，还是脚本好使

# 代码

```python
"""
作用：将文件夹中的文本文件全都转化为 utf-8 编码，另存在新文件夹中

使用方法:
首先修改代码中 52 行的 dirPath，改成字幕文件夹路径即可 
pip install chardet
python main.py

发现非 utf-8 编码文件“CSI.S03E23.Inside.the.Box.720p.WEB-DL.DD5.1.H.264-DNR.srt”，其编码为 GB2312
....
发现非 utf-8 编码文件“CSI.S03E02.The.Accused.Is.Entitled.720p.WEB-DL.DD5.1.H.264-DNR.srt”，其编码为 GB2312
发现非 utf-8 编码文件“CSI.S03E01.Revenge.Is.Best.Served.Cold.720p.WEB-DL.DD5.1.H.264-DNR.srt”，其编码为 GB2312
总计23个文件，其中23个非 utf-8 编码，转换后的保持路径为 /tmp/CSI.S03.720p.WEB-DL.DD5.1.H.264-DNR-utf

参考: https://blog.csdn.net/qq_42992919/article/details/100100371
"""

from chardet.universaldetector import UniversalDetector
import os


def get_encode_info(file):
    with open(file, "rb") as f:
        detector = UniversalDetector()
        for line in f.readlines():
            detector.feed(line)
            if detector.done:
                break
        detector.close()
        return detector.result["encoding"]


def read_file(file):
    with open(file, "rb") as f:
        return f.read()


def write_file(content, file, path):
    with open(os.path.join(path, file), "wb") as f:
        f.write(content)


def convert_encode2utf8(dirPath, file, original_encode, des_encode, path):
    fullPath = os.path.join(dirPath, f)
    file_content = read_file(fullPath)
    file_decode = file_content.decode(original_encode, "ignore")
    file_encode = file_decode.encode(des_encode)
    write_file(file_encode, file, path)


if __name__ == "__main__":
    dirPath = "/tmp/CSI.S03.720p.WEB-DL.DD5.1.H.264-DNR"    # 字幕文本文件的文件夹路径
    targetPath = dirPath + "-utf"
    os.mkdir(targetPath)
    count = 0

    for f in os.listdir(dirPath):
        fullPath = os.path.join(dirPath, f)
        encode_info = get_encode_info(fullPath)
        if encode_info != "utf-8":
            print("发现非 utf-8 编码文件“%s”，其编码为 %s" % (f, encode_info))
            count = count + 1
            convert_encode2utf8(dirPath, f, encode_info, "utf-8", targetPath)

    print(
        "总计%d个文件，其中%d个非 utf-8 编码，转换后的保持路径为 %s"
        % (len(os.listdir(dirPath)), count, targetPath)
    )
```