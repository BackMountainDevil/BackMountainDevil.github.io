# Arduino 开发板管理器报错 Error downloading https://downloads.arduino.cc/packages/package_index.json
- date: 2021-09-05
- lastmod: 2021-09-05

单单看错误没看出来啥，到日志文件（～/.arduino15/logs/application.log）里查一下

```log
ERROR c.a.c.p.ContributionInstaller:304 [Timer-0] 下载 https://downloads.arduino.cc/packages/package_index.json 时出错
java.lang.Exception: 下载 https://downloads.arduino.cc/packages/package_index.json 时出错
	at cc.arduino.contributions.DownloadableContributionsDownloader.download(DownloadableContributionsDownloader.java:149) ~[arduino-core.jar:?]
	at cc.arduino.contributions.DownloadableContributionsDownloader.downloadIndexAndSignature(DownloadableContributionsDownloader.java:165) ~[arduino-core.jar:?]
	at cc.arduino.contributions.packages.ContributionInstaller.updateIndex(ContributionInstaller.java:302) [arduino-core.jar:?]
	at cc.arduino.contributions.ContributionsSelfCheck.updateContributionIndex(ContributionsSelfCheck.java:215) [pde.jar:?]
	at cc.arduino.contributions.ContributionsSelfCheck.run(ContributionsSelfCheck.java:75) [pde.jar:?]
	at java.util.TimerThread.mainLoop(Timer.java:555) [?:1.8.0_292]
	at java.util.TimerThread.run(Timer.java:505) [?:1.8.0_292]
Caused by: java.io.IOException: Received invalid http status code from server: 403
	at cc.arduino.utils.network.FileDownloader.openConnectionAndFillTheFile(FileDownloader.java:239) ~[arduino-core.jar:?]
	at cc.arduino.utils.network.FileDownloader.downloadFile(FileDownloader.java:182) ~[arduino-core.jar:?]
	at cc.arduino.utils.network.FileDownloader.download(FileDownloader.java:129) ~[arduino-core.jar:?]
	at cc.arduino.contributions.DownloadableContributionsDownloader.download(DownloadableContributionsDownloader.java:147) ~[arduino-core.jar:?]
```

Caused by: java.io.IOException: Received invalid http status code from server: 403
这一行指出了错误原因，服务器接受但拒绝了请求。。。我 firefox 都能正常访问。。。esp两个开发板都能正确获取到，也就是说原因出在了 arduinocc的服务器上？不对吧，浏览器都可以。。在昨晚安装的多系统的 Debian 中的 arduino 日志中发现了同样的错误。。。说明不是 Manjaro 系统的问题，但是浏览器却可以，说明不是 ip 封锁啊。。看着窗外的夕阳还有插座上的数据线，想起来了或许是网络问题，从移动宽带切换切换到电信 4G 网络，果然这个问题就没有了，成功下载官方开发板的 pkg.json，不过诞生了不能下载 8266 的错误，说明每个网络都有各自的问题。。。切换回宽带之后啥事也没有了。

总结一下，错误 `Error downloading https://downloads.arduino.cc/packages/package_index.json` 尽管相同，各自的原因却不一定相同， arduino cc 和 cn 里有一些相同的错误，看了下原因不同我就没有摘录了，还有的是外链失效也不记录了。

# 参考

- [arduino 下载 https://downloads.arduino.cc/packages/package_index.json error 出错的解决方法](https://blog.csdn.net/xutianruo/article/details/86591189)：删除 arduino15 目录下的 *.tmp 文件，如 package_index.json.tmp、package_esp8266com_index.json.tmp
- [ Web Editor Error 403 Forbidden #5846 ](https://github.com/arduino/Arduino/issues/5846):原因是还不支持 Chromebook，后续更新会支持
- [ Board Manager JSON (http status code from server: 403) #8494 ](https://github.com/arduino/Arduino/issues/8494)：相同的报错。由于某些原因服务器开启了验证码导致的错误，已经关闭验证码了，可以正常使用了。（所以就不知道我的错误咋解决了）
- [2016. Error downloading http://downloads.arduino.cc/packages/package_index.json #5188 ](https://github.com/arduino/Arduino/issues/5188)：错误原因和我不同，但结果相同。它改了 IP 协议就好。不过里面 agdl  说了可以尝试清空  .Arduino15/staging/packages，不过实验了一下无效
    > Caused by: java.net.SocketException: Address family not supported by protocol family: connect
