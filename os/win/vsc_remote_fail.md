# Visual Studio Code 更新后远程插件失效 GLIBC_2.28 not found
- date: 2024-12-05

windows 版本的 vscode 更新到 1.95.3 之后，remote ssh 无法连接到 Ubuntu 18.04，错误原因是 `/lib/x86_64-linux-gnu/libc.so.6: version 'GLIBC_2.28' not found`。然而实测 1.92.2 linux 版本还支持 Ubuntu 18.04.6 LTS，remote ssh 插件版本为 v0.113.2024072315，于是我下载相同版本的进行测试结果表明 1.92.2 win 版本不支持 Ubuntu 18.04.6 LTS，remote ssh 插件版本为 v0.107.1，报错原因还是 “GLIBC_2.28 not found”，我也懒得挨个降级了，于是直接下载 1.85.2 win 版本，可以远程连接 Ubuntu 18.04 了，就是插件开启但无效，于是把插件卸载重装就好了。

正解是升级服务器系统版本或者升级 libc，但是服务器使用人数众多，不好协调。

# refer

[vscode 1.86版本远程ssh不兼容旧服务器问题解决 萌萌哒赫萝](https://zhuanlan.zhihu.com/p/681066025): 回退版本到1.85.2

https://update.code.visualstudio.com/{version}/win32-x64-archive/stable
