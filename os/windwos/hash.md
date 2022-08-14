# windows 计算文件哈希值
- date: 2022-08-14
- lastmod: 2022-08-14

需要打开Powershell进行操作,cmd测试不支持。原因是下载了一本教程，格式是exe,比较存疑，下载页面提供了哈希值进行校验，在KDE下的文件管理器通过属性就可以方便校验，然而win还没有做这个功能。。

```
get-filehash [路径] -Algorithm [算法类型]
```

- 路径：支持相对路径、绝对路径
- 算法类型：默认是SHA256，可以是SHA1、SHA256、SHA384、SHA512、MD5.（MD5和SHA1不再认为是安全的）

例子，计算tmp.txt的哈希值并将输出格式化为列表

```
get-filehash tmp.txt -Algorithm SHA256 | Format-List
```
