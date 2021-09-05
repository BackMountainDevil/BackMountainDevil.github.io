---
title: "从Hexo到Hugo"
date: 2020-11-03T22:01:31+08:00
lastmod: 2020-11-04T11:19:13+08:00
keywords: [Hugo, Github Page, Hexo]
description: "使用Hugo重新建立我的博客网站"
tags: [Hugo]
categories: [Blog]
author: "筱氚"
---
# 博客网站
刚开始只是在知乎、CSDN上写帖子，后面试过微信公众号、Wordpress、云服务器等等。略懂一些写网页的知识，但是身为程序员深知重复造轮子的无用，于是找了些其它办法 - 静态网站生成器，配合Github Page简直爽歪歪（后来国内也出了Gitte Page）。
在知乎上搜一搜静态网站生成工具就能查到不少帖子，大致都包含Jekyll、Hugo、Hexo、VuePress等。这里以Windows10配置Hugo到Github Page为例作为记录。
## Hexo
Hexo是我一个尝试的静态网页生成方案，第一次配置的时候还是花了不少时间的，后面加上域名更加爽歪歪。虽然用的时候文章分类还是个问题，但一直没太在意，后来换电脑之后尝试恢复hexo失败了。。。。重建一个也行，但是我想试一下其它的静态网页方案。
Hexo最方便的就是更换主题，下载更改配置即可，也支持很多插件扩展。唯一的难受之处就是我如何把CSDN上的帖子都一次性转到里面呢？？
## Hugo
> There is lots of talk about “Hugo being written in Go”, but you don’t need to install Go to enjoy Hugo. Just grab a precompiled binary!

Hugo安装不需要安装Go环境，而Hexo还需要额外安装node.js，使用感受嘛，以后再写。

# Hugo的安装和配置Path
## 下载
在[Hugo-Install](https://gohugo.io/getting-started/installing/)页面中，对于Windows系统来说，Hugo提供了zip包、choco、scoop等安装方式，我直接从[Hugo-Releases](https://github.com/gohugoio/hugo/releases)下载了win-64的zip包。
### Path
> Once you have installed Hugo, make sure it is in your PATH.

根据英文文档的描述，需要将Hugo加入path环境，解压hugo_0.77.0_Windows-64bit.zip之后会有一个exe、licese、readme，把文件夹hugo_0.77.0_Windows-64bit挪到其它任意目录，我是挪到了D:\ProgramFiles；然后编辑系统环境变量，在Path中加入`D:\ProgramFiles\hugo_0.77.0_Windows-64bit\`
### 测试
win+R输入cmd回车后输入`hugo version`，然后回车，显示对应版本号说明Hugo已经被正确安装啦

# 建立网站框架
## 建立站点
在想要存放网站根目录的文件夹打开控制台窗口，比如我想存放在目录`D:\Documents\Git\Lion`下面的Blog文件夹中，那就打开资源管理至Lion目录下（此时暂未建立Blog文件夹），按住Shift后右键鼠标打开Powershell，就会发现此时控制台的目录已经且换到了我们想要的目录下。(或者cmd后输入`cd /d D:\Documents\Git\Lion`就会且换到对应路径)
```Bash
> hugo new site Blog
```
成功之后会提示
```Bash
Congratulations! Your new Hugo site is created in D:\Documents\Git\Lion\Blog.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>\<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.
```
如果失败的话，很有可能是存在同名文件夹，更名或者转移位置都可。 
进入网站目录中，可以看到存在一些文件
```Bash
archetypes  # 存放生成博客的模版
content     # 存放 markdown 文件
data        # 存放 Hugo 处理的数据
layouts     # 存放布局文件
static      # 存放静态文件：图片、css、js
themes      # 主题文件夹
config.toml # 配置文件
```
## 配置主题
新建立的Hugo网站默认没有布局文件、模板文件，因此需要到[Hugo主题](https://themes.gohugo.io/)市场上找一个自己喜欢的主题（选择主题困难整。。。而且网速感人。。）
我想要的主题功能：
1. 首页展示最近的文章列表
2. 文章页面有浮动目录导航栏
3. 文章支持Github Issue留言
4. 有标签和归档功能
看到有个[LoveIt主题](https://github.com/dillonzq/LoveIt)不错,由于网速问题，最后我选择了[even主题](https://gitee.com/myki/hugo-theme-even?_from=gitee_search)，在其主页的README有描述安装、更新方式，不过这个主题没有Issue留言功能。
```Bash
$ cd Blog
$ git clone https://github.com/olOwOlo/hugo-theme-even themes/even
或者
$ git clone https://gitee.com/myki/hugo-theme-even.git themes/even
```
第一个路径有点慢，我就用第二个路径，快的一批；然后把themes/even/exampleSite下的onfig.toml覆盖到Blog下的 config.toml 
对于这个even主题，其文档中有几点值得注意：
1. 本主题用到了 Hugo Pipes 功能。如需修改 assets 目录下的文件，请安装 extended 版。
2. 对于这个主题，你应该使用 post 而不是 posts，即 hugo new post/some-content.md。
3. 更新主题的方式看原文吧。
## 新建文章
建立我们的第一篇博客
```Bash
>hugo new post/HelloHugo.md
```
在content/post下面有刚才新建的HelloHugo.md，打开它写入几句话即可
## 本地Hugo测试
```Bash
hugo server -D
```
参数-D表示连草稿也一起预览,也可以不加这个参数;然后在浏览器输入http://localhost:1313/即可浏览，你会发现修改文件后hugo可以立马更新预览,在控制台按住Ctrl+C即可关闭本地服务.
# 生成HTML
首先要把Front Matter中的draft=true删除,因为生成的静态文件里不会包含草稿的(还有其它两种类型的也不会生成).
```Bash
hugo
```
然后就会生成public文件夹,里面存放着生成的静态网站文件
> Running hugo does not remove generated files before building. This means that you should delete your public/ directory (or the publish directory you specified via flag or configuration file) before running the hugo command. If you do not remove these files, you run the risk of the wrong files (e.g., drafts or future posts) being left in the generated site.

上面这段话出自[Hugo - Basic Usage](https://gohugo.io/getting-started/usage/)，意思是在用hugo生成静态网站文件之前一定要删除public目录，不然就会Error
# 部署到Github Page
主要参考这个文档[Hugo Deploy - Host on Github](https://gohugo.io/hosting-and-deployment/hosting-on-github/),其它中文文档简直就是瞎写瞎抄,怎么可能在public下init一个仓库呢?每次生成前都会删除public.
## 新建两个仓库
> After running `hugo server` for local web development, you need to do a final hugo run without the server part of the command to rebuild your site. You may then deploy your site by copying the public/ directory to your production web server.

上面说的是把public目录下的文件都拷贝到网站服务器根目录即可，然而我都选择了静态网站了，整啥服务器嘛，肯定整静态托管服务呀。
首先新建一个博客工程仓库，用来存放整个博客的工程文件，然后新建一个User Github Page仓库，用来存放生成的网站静态文件。我建立的工程仓库叫BlogHugo，Page仓库叫做BackMountainDevil.github.io。BackMountainDevil是我的用户名。
## clone工程仓库
新建的BlogHugo空仓库，使用` git clone git@github.com:BackMountainDevil/BlogHugo.git`克隆到本地，然后把整个博客的工程文件全剪切到BolgHugo里面
然后换到BlogHugo目录下，测试Hugo还能不能正常运行
```Bash
$ cd BlogHugo
$ hugo server
```
能正常运行的就退出吧，然后删除public目录，修改主配配置文件config.toml中的baseURL为BackMountainDevil.github.io

## git-submodule
```Bash
git submodule add -b master https://github.com/BackMountainDevil/BackMountainDevil.github.io.git public
```
> This creates a git submodule. Now when you run the hugo command to build your site to public, the created public directory will have a different remote origin (i.e. hosted GitHub repository).

当我敲这个命令的时候，git把我hexo以前生成的静态文件都pull下来了。。。所以下次还是先情况Page仓库的内容吧。
确认删除public目录，然后运行
```Bash
hugo
```
然而这里报错
```Bash
Error: Error building site: TOCSS: failed to transform "sass/main.scss" (text/x-scss): resource "xx" not found in file cache
```
根据[FAQ中给出的解决办法](https://gohugo.io/troubleshooting/faq/#i-get-tocss--this-feature-is-not-available-in-your-current-hugo-version)，让我安装extend增强版的hugo,还好[Hugo-Releases](https://github.com/gohugoio/hugo/releases)页面中有win64_extend版本，下载解压添加Path（当然要删除之前的）
我装了win_extend的之后运行hugo确实生成了静态文件。。但是没有自动部署到仓库？？？
```Bash
$ git add .
$ git rm --cached themes/even -f
$ git rm --cached public -f
$ git commit -m "Blog工程文件，无theme、无public"
$ git push --set-upstream origin master
```
这样就把工程文件都puhs到仓库中了，后面可以发现用命令避免public目录被add
```Bash
$ echo "public" >> .gitignore
$ git add .gitignore
```
但是pubic目录呢？？？每次手动一堆git吗？？
### 小问题
我发现生成的文章没有tags和categories，后来才知道是没有修改模板，见Conclusion-3
# 重头再来
清空github Page仓库（BackMountainDevil.github.io）；建立一个空仓库（HugoPageSource）存放全部工程文件。
```Bash
$  git clone git@github.com:BackMountainDevil/HugoPageSource.git
cd HugoPageSource
$ hugo new site . --force
git add .
git commit -m "init hugo site template"
git push
git submodule add git@github.com:BackMountainDevil/BackMountainDevil.github.io.git public
# 现在public里面只有page仓库的一个.git文件
hugo
cd public
git add .
git commit -m "first build with hugo, just empty web file"
cd ../
git add .
git commit -m "first build with hugo, just empty web file - update submodule reference"
git push
cd public
git push
cd ../
```
然后fork一个hugo主题到自己仓库，又作为submodule使用。我fork到自己组织仓库下（自己的仓库也行，我看我的组织啥库也没有）,尝试了下组织的网速感人。。又fork到自己的仓库。结果速度还是感人
最后跑到gitee下载zip去掉commit记录传到自己的（私密）仓库（加上来源备注）

git submodule add git@gitee.com:anidea/hugo-theme-even.git themes/even
修改source的配置文件和模板，修改baslUrl为https://backmountaindevil.github.io/
写入几个md文档，修改好头部信息Front Matter，删除public下面的内容
```Bash
hugo server
hugo
git add .
git commit -m ""
git push
然而Source是更新了，page仓库还没有更新。。于是把public的内容剪切到Page仓库并push
```
# 域名配置
购买、备案就不描述了，在static文件夹下新建一个文件CNAME，在里面填入自己的域名，比如我的就是`kearney.club`,不要加http，http亲测无效。以后每次运行hugo生成的时候都会把CNAME文件copy到public里面。
# Conclusion
1. Hugo主题要好好选择（网速很重要。。卡的选不了，不然向我一样找gitte吧）。
2. 可以直接把以前的md文档拷贝到content/posts下，但是要把文档内的抬头(Front Matter)修改和配置文件中一样的，不然Hugo会报错.
3. hugo默认的模板太简单了，可以到主题下面archetypes把模板文件default覆盖到主目录的archetypes，然后再做点修改，默认的author啊啥的

# References
- [Hugo 从入门到会用](https://olowolo.com/post/hugo-quick-start/)
- [Hugo英文文档-Install](https://gohugo.io/getting-started/installing/)
- [Hugo-Releases](https://github.com/gohugoio/hugo/releases)
- [Hugo主题](https://themes.gohugo.io/)
- [Hugo Deploy](https://gohugo.io/hosting-and-deployment/)
- [Hugo Deploy - Host on Github](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
- [Hugo - Basic Usage](https://gohugo.io/getting-started/usage/)
- [使用了静态网页生成器：从WordPress 到 Hugo（主题安装方式有雾ma?）](https://zhuanlan.zhihu.com/p/109284344)
- [Hugo + Github Pages 搭建个人博客](https://www.cnblogs.com/stevexu/p/10779375.html)
- [even主题](https://gitee.com/myki/hugo-theme-even?_from=gitee_search)
- [LoveIt主题](https://github.com/dillonzq/LoveIt)
- [LoveIt-gitee](https://gitee.com/choicky/LoveIt?_from=gitee_search)
- [Hugo 自动化部署到 Github Pages](https://emacsist.github.io/2016/11/13/hugo-%E8%87%AA%E5%8A%A8%E5%8C%96%E9%83%A8%E7%BD%B2%E5%88%B0-github-pages/)
- [git同步代码至github和gitee(码云)](https://www.jianshu.com/p/d00c36ed1a03)
- [hugo安装及部署](https://www.artacode.com/posts/hugoinstall/)
- [Hugo中文文档不咋地](https://www.gohugo.org/doc/)