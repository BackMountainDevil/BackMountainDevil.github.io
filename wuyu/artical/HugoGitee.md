---
title: "HugoGitee"
date: 2020-12-08T10:17:02+08:00
lastmod: 2020-12-08T10:17:02+08:00
keywords: ["Gitee","Blog","Hugo"]
description: "使用hugo生成静态网站并部署到Gitee Page"
tags: []
categories: [Blog]
author: "筱氚"
---
# 简介
# 配置Git和VS Code
安装Git、VS Code，注册gitee帐号，配置Git到giteessh密钥。
# 建立Gitee Page公开仓库
建立Page公开空仓库，复制SSH链接`git clone git@gitee.com:anidea/anidea.git`，然后进入目录中，新建一个index.html，内容随便，例如`<p>这是Kearney瞎写的</p>`，然后通过VS Code的源代码管理commit并push到gitee上面，然后在浏览器中打开仓库，点击服务中的Gitee Pages，不需要修改啥（我选中了HTTPS），点击黄色的按钮启动部署即可，然后浏览对于的网址`https://anidea.gitee.io/`即可，出现了我刚才编辑的内容，但是为啥中文是乱码？？？？  明明Code使用的是UTF-8编码了。。
`è¿™æ˜¯KearneyçžŽå†™çš„`
Gitee Page有个**缺点**就是每次更新博客都要到服务里点击重新部署，当然自动化部署的事情这里不讲。
# Hugo安装
正确安装Hugo增强版（extend），这里不描述了
# 建立私有工程仓库
这个私有空仓库用来存放整个博客的文件（不包含静态网页文件），静态网页文件存放在page仓库里面，这样做的好处是，既保留了原始md文档，又能在新环境里快速搭建hugo站点（装个hugo就行）。page仓库作为工程仓库的submodule来使用。我建立的私有工程仓库地址`git@gitee.com:anidea/hugo-source.git`

# Fork一个主题
[Hugo主题](https://themes.gohugo.io/)市场,[LoveIt主题](https://github.com/dillonzq/LoveIt)、[even主题](https://gitee.com/myki/hugo-theme-even?_from=gitee_search)，最好是市场找一个，然后导入到gitee里面，就会快很多。Fork完之后cv下载地址`git@gitee.com:anidea/hugo-theme-love-it.git`
# 动手吧
```bash
hugo new site HugoSource
cd HugoSource
git init
git remote add origin git@gitee.com:anidea/hugo-source.git
git submodule add git@gitee.com:anidea/anidea.git public
git submodule add git@gitee.com:anidea/hugo-theme-even.git themes/even
# 替换配置文件：到HugoSource\themes\even\exampleSite下复制config.toml到HugoSource下，替换原来的配置文件，打开该文件，将第一行的baseURL修改为`https://anidea.gitee.io/`
# 替换文章模板：到HugoSource\themes\even\archetypes复制default.md到HugoSource\archetypes下进行覆盖
hugo new post/2020conclusion.md # 基于模板生成的文件位于HugoSource\content\posts\
# 编辑文章并保存
hugo server # 预览，在浏览器输入http://localhost:1313/进行查看，Ctrl+C退出预览
hugo # 生成html文件，生成前要删除HugoSource\public下的html文件
# 把生成的html文件push到Page公开仓库并在gitee中部署
```
# 总结
## 私有域名
如果有的话，比如我的是http://kearney.club/，就把配置文件里的baseUrl改为它，然后在HugoSource\static中加入CNAME文件（内容为kearney.club），这样每次生成html文件的时候hugo就会自动把CNAME拷贝到public中
# References
- [Gitee 帮助中心/ 静态页面托管](https://gitee.com/help/articles/4136#article-header3)
- [使用Gitee Pages+hugo免费搭建你的博客](https://blog.csdn.net/qq_34688164/article/details/104615706)
- [Gitee 码云 pages 搭建个人网站](https://blog.csdn.net/qq_36667170/article/details/79318578)
- []()