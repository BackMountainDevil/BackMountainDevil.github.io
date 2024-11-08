# 微信 Linux 测试版
- date: 2024-11-08

[微信 Linux 测试版](https://linux.weixin.qq.com/):支持三种架构（X86、ARM、LoongArch），网页有官方 QQ 交流群

我下载的 x86 AppImage，通过下面的命令给予可执行权限后才能运行。

```bash
$ chmod +x WeChatLinux_x86_64.AppImage
```

启动后扫码登录，支持朋友圈、小程序、截图、深色模式、微信浏览器（可设置使用默认浏览器）、聊天记录查询。比 wechat-uos 好很多，聊天记录备份暂不支持、朋友圈有的内容无法正常展示。

我用的 x11，fcitx5输入法正常，网上有说 [wayland 下不正常](https://www.zhihu.com/question/3360014967/answer/25869182898)，其评论区有解决方案，本人未测试

设置里可见版本是 4.0.0.30，文件哈希如下

```bash
$ sha1sum WeChatLinux_x86_64.AppImage 
274601ccf81f0c18cd5a74f4353ec871441f4338  WeChatLinux_x86_64.AppImage
```
