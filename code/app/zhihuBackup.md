# 知乎备份计划
- date: 2022-08-07
- lastmod: 2022-08-07

## 知乎助手

[基于node&typescript重写知乎助手 YaoZeyuan/zhihuhelp](https://github.com/YaoZeyuan/zhihuhelp)，文章、回答、想法保存测试通过，收藏夹测试失效。  

## MaoXian Web Clipper

[一个简洁的浏览器扩展，让你看到想收藏的内容，可以直接裁剪保存下来，以避免网站挂了，网址失效，图片失效等问题。是的，没有烦人的注册，也不收费。](https://mika-cn.github.io/maoxian-web-clipper/index-zh-CN.html)

保存知乎收藏夹需要自己逐个展开内容和评论（评论只能存一页），存为多个文，想合并为一个文件需借助插件Single File，收藏家存下的可能是多个问题，存下来没有书签

## js

小知识，CSS 中 id 选择器以 "#" 来定义，类选择器以一个点 . 号显示。

在[收藏夹页面](https://www.zhihu.com/collection/433448677)，默认文章均不打开，一页是20篇，摘要末尾都是“阅读全文”，点击才可以看到全文，浏览器F12可以查到这个按钮的类属性为“Button ContentItem-more Button--plain“，通过在控制台跑两次`$(".ContentItem-more").click()`可以知道这句话可以一次展开一篇文章，那么写一个循环就可以展开全部了，当然用定时器也行，写完如下（间隔1000ms）

```js
// 间隔1s点击一个“阅读全文”按钮
(function(){setInterval(function(){$(".Button.ContentItem-more.Button--plain").click()},1000)})();
```

循环比定时器好在可以控制次数，缺点是控制时间间隔比较烦，目前测试不控制时间间隔也可以展开，这样操作容易被zh封。

```js
// 一瞬间点击20个   “阅读全文”按钮
for (i = 0; i < 20; i++) { 
    $(".Button.ContentItem-more.Button--plain").click();
}
```

而展开评论也可以通过同样办法，`$(".Button.ContentItem-action.Button--plain.Button--withIcon.Button--withLabel").click();`，然而测试这样只是会反复展开、收起同一页评论。。。于是要找到所有的按钮逐个点击

```js
// 展开所有篇幅的第一页评论，能用，warning在于getElementsByClassName返回的数量比实际按钮多，说明类筛选还需要改进
var comments = document.getElementsByClassName('Button ContentItem-action Button--plain Button--withIcon Button--withLabel');

for (i = 0; i < comments.length; i++) { 
    comments[i].click();
}
```

展开之后用MaoXian Web Clipper进行保存

# 参考

- [如何保存某位知乎用户的所有答案？](https://www.zhihu.com/question/22719537):
  > 直接去某个人的首页然后不停的点击更多.点个几十次，右击另存就行，我直接f12在浏览器的控制台放上一段代码，让浏览器自己跑(function(){setInterval(function(){$(".zg-btn-white.zu-button-more")[0].click()},3000)})();最后，另存为网页就行。
- [如何备份/下载知乎中的回答？](https://www.zhihu.com/question/19566263)：cv,截屏OCR,onenote，web scraper，简悦，MaoXian web clipper
- [知乎个人回答备份，获取html并打印到pdf 2017 MengXiangxi/zhihu_Backup ](https://github.com/MengXiangxi/zhihu_Backup):登录部分报错提示请使用手机验证码
