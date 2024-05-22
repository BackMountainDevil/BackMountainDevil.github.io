# 查看磁盘使用空间和文件大小
- date: 2024-5-22

SpaceSniffer 则适用于 windows 查看某路径下的文件大小，用矩形面积大小来表示文件大小占用，并且还可以一次显示多个文件层级的大小。

[Filelight 磁盘占用查看器](https://apps.kde.org/zh-cn/filelight/) 以多层同心圆示意图显示电脑磁盘使用情况，适用于 windows、linux

在 linux 上查看硬盘的使用情况： `df -h` . 参数 -h 表示空间大小的单位就近转换，而不是一律显示字节数，du 中的 -h 参数也是这个含义

在 linux 上查看所有文件(含文件夹)的大小，并以降序显示： `du -sh * | sort -hr` . 参数 -s 表示只显示最上层文件的总计大小，不显示子文件的大小。参数 * 表示当前目录下的所有非隐藏文件。如果要同时查看以 `.` 开头的隐藏文件大小，则可以使用这个指令 `du -sh .[!.]* * | sort -hr` , 如果没有隐藏文件时，该指令会提示 `No such file or directory`，但不影响其统计非隐藏文件大小 。如果要查看大小的总和，可以在 du 中加入参数 -c，如 `du -sch *`。

# 参考

[【linux】显示文件夹大小|包含隐藏的文件|文件排序 bandaoyu于 2023-01-18](https://blog.csdn.net/bandaoyu/article/details/112882343)
