# Firefox 下网页无法正常显示探究过程 UserAgent AdBlock
- date: 2022-10-10
- lastmod: 2022-10-10

有一些网站在 firefox 中没法显示，比如[这个](https://share.weiyun.com/vQgjQ4uO)，但是把网址复制到 chromium 打开确实是有内容的，在 firefox 开发者模式进入 响应式设计模式(Ctrl+Shift+M) 中更换设备类型为手机、平板啥的，网页又可以正常显示了

# User-Agent



## 获取本浏览器的 User-Agent

这里有两个获取浏览器 UserAgent 的网站：[获取浏览器UA(userAgent)信息](http://service.spiritsoft.cn/ua.html) [UserAgentString.com](https://www.useragentstring.com/index.php)

如果没有网络，可以直接打开开发者模式，点进网络，然后随便打开一个网站，比如 [localhost](http://127.0.0.1)，然后网络调试栏目里只有一个GET请求，点进去就可以看到该请求的 User-Agent

## UserAgent 例子

Firefox Linux laptop 105.0.3-1

>  Mozilla/5.0 (X11; Linux x86_64; rv:105.0) Gecko/20100101 Firefox/105.0 

Chromium Linux laptop 106.0.5249.103-1

>  Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0 Safari/537.36

## 修改 UserAgent

[How To Change User Agent in Firefox  Sergey Tkachenko March 25, 2018](https://winaero.com/change-user-agent-firefox/):文章还给出了旧版 firefox、edge、ie 的userAgent

    url: about:config
    general.useragent.override
    string add new

按照上面的办法我将 firefox 的 userAgent 暂时替换为 chromium，并且在获取 ug 网站进行了修改是成功的，但是网页还是无法正常显示，但是 响应式设计模式 中的手机平板都能呢个正常显示。

# 原因

通过替换 userAgent 发现不是该原因，在 stw 的时候发现可能是网页标准导致的，但是更换浏览器却ok,又排除了一个原因，看到这篇（ [明明有网络，火狐打不开任何网页，ie和edge正常 2022-1-12](http://mozilla.com.cn/space-uid-1208992.html)）的时候我觉得也不是火狐版本原因，然后思索了一下可能是某个扩展导致的，随后便一个个关闭扩展尝试，最后确认是 Adblock Plus 导致的该网页无法显示，在 AbBP 扩展设置里把该网站加入白名单就好了。

通过简单的步骤得知，AbBP 在 响应式设计模式 中的手机、平板不生效，但是在 Laptop 中会生效。



# 相关阅读

- [Firefox 用户代理字符串 (user agent string) 参考](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/User-Agent/Firefox)


- [ 修改firefox/chrome浏览器的UserAgent 2018-05-11 Leone-](https://www.cnblogs.com/doseoer/p/9024575.html)：试了下没有对应的修改路径


- [网站显示不正常 Firefox Support](https://support.mozilla.org/zh-CN/kb/%E7%BD%91%E7%AB%99%E6%98%BE%E7%A4%BA%E4%B8%8D%E6%AD%A3%E5%B8%B8)


- [Firefox为什么不能正常浏览部份网页](Firefox为什么不能正常浏览部份网页)
> 这是网站本身在设计的时候以IE的标准做的网页，而Firefox所遵循的网页标准是W3C标准

