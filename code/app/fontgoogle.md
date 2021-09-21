# fontgoogle 加载速度慢的解决办法
- date: 2021-07-18
- lastmod: 2021-09-21

在使用[docsify](https://docsify.js.org/)做项目文档、[博客](https://backmountaindevil.github.io/)的时候，发现每次强制刷新的时候需要加载好长时间，有时候干脆在那里一直加载中，不动了，你知道 IE 的感觉吗？比 IE 还要慢。

究其原因，Firefox 在其页面左下角显示了一个网址关于 
可是打开，在 [vue.css](https://cdn.jsdelivr.net/npm/docsify@4/lib/themes/vue.css) 中发现了 `@import url("https://fonts.googleapis.com/css?family=Roboto+Mono|Source+Sans+Pro:300,400,600");`，而 fonts.googleapis 就好像 github，大部分情况下是 404 not found。所以才会出现开头那种时常加载不出来的情况。

那如何解决这个问题呢？经过互联网的搜索，有人说把字体文件下载下来放到本地，从本地加载；也有人说换成谷歌中国字体站的地址就好了，把 fonts.googleapis.com 换成 fonts.font.im。

本来把修改了一点点的 vue.css 放在本地去推送而不是采用 cdn 就已经增加了一个文件，加上字体又得多四个文件，最终这个方案我没有采取。最后是采取了 MrtBian 提供的办法，换成中国站，虽然这个站点我自己都不一定上的去，但是实际效果比原来好很多了，比如现在的这篇博客就是采取这种办法处理的。

# 相关参考

- [网站卡在“fonts.googleapis.com”谷歌字体，解决方案。xiangjai 2018-06-01](https://blog.csdn.net/xiangjai/article/details/80541465):放到本地加载
  > `@import url("https://fonts.googleapis.com/css?family=Open+Sans:300,400,600,700"); `   
  > `@import url("googleapis-fonts/Open+Sans.css");`
- [fonts.googleapis.com 加载慢解决方案。MrtBian 2019-11-09](https://blog.csdn.net/MrtBian/article/details/102991383)
  > fonts.googleapis.com 换成 fonts.font.im
