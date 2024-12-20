# windows 设置utf8编码
- date: 2024-12-19

在终端输入 chcp，默认情况返回的是 936，查询相关文档可其含义为中文

```
> chcp
活动代码页: 936
```

设置-时间和语言-语言和区域-管理语言设置，弹窗后单击非Unicode程序的语言中的“更改系统区域设置”，勾选“Beta：使用Unicode UTF-8”，然后点击确定。之后需要重启电脑。

重新打开终端

```
> chcp
Active code page: 65001
```

# refer

[把windows系统的默认编码改成UTF-8 老朱-yubing 2020-05-11](https://blog.csdn.net/robinhunan/article/details/106047345)

[chcp](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/chcp)
> 936 	中文  
