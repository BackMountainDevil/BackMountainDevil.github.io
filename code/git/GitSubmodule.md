---
title: "GitSubmodule"
date: 2020-11-04T20:45:34+08:00
lastmod: 2020-11-04T20:50:13+08:00
keywords: [Git]
description: "git中submodule的介绍和案例"
tags: [Git]
categories: [VCS]
author: "筱氚"
---
# 什么是submodule
> gitsubmodules - Mounting one repository inside another

submodule翻译过来就是“子模块”的意思。上面这句英文出自Git官网的文档中，它是这样描述gitsubmodules的：在一个仓库里嵌套另一个仓库。以最近我所接触的静态博客生成工具Hugo为例，整个博客工程仓库是一个仓库，主题theme中包含多个别人的主题仓库，生成的静态网站文件public又是一个网站仓库，说白了就是一个仓库中包含了别的仓库（疯狂套娃）。

被嵌套的仓库又被称为subproject（子工程）
## 特点
相互独立：主仓库和子仓库都有各自的git记录，克隆主仓库时不会克隆子仓库的文件，只是克隆一个快链（gitlink）
便于维护：不需要再把子仓库的文件拷贝过来，子仓库多了还容易拷错
# 怎么用
- `git submodule  add [<options>] [--] <repository> [<path>]`

在主仓库中添加子仓库到path中，不指定path的话就拷贝到当前目录。

- `git submodule update --init --recursive`

拉取子仓库的代码，用在更新/拉取主仓库时未拉取子仓库  
如果pull一个含有子仓库的仓库，是不会克隆子仓库的代码的，如果想连子仓库的代码也克隆，就需要加上参数`--recurse-submodules`
- git clone git@gitee.com:back-toy/main.git --recurse-submodules
# 案例
## 建立空仓库
我在github建立了两个空仓库，主仓库main，子仓库sub
```Bash
$ git clone git@github.com:BackMountainDevil/main.git
$ cd main
$ git submodule add git@github.com:BackMountainDevil/sub.git  public
Cloning into 'D:/Documents/CAU/Lion/main/public'...
warning: You appear to have cloned an empty repository.
fatal: You are on a branch yet to be born
Unable to checkout submodule 'public'
```
报错.....sub为空仓库，下面为sub仓库添加点内容,首先删除main文件夹，从头做起，然后再为sub仓库添加内容
## 为sub添加内容
```Bash
$ cd ../
$ mkdir sub
$ cd sub
echo "# sub" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:BackMountainDevil/sub.git
git push -u origin main
```
此时sub仓库已经有了main主分支（以前是master分支，2020黑人运动就改成了main），然后内容和历史也有了
## 为main主仓库添加submodule
下面对主仓库进行操作
```Bash
$ cd ../
$ git clone git@github.com:BackMountainDevil/main.git
$ cd main
$ git submodule add git@github.com:BackMountainDevil/sub.git  public
```
这个时候主仓库就会把子仓库嵌入到到main/public，并且多了一个.gitmodules文件，其内容为
```Bash
[submodule "public"]
	path = public
	url = git@github.com:BackMountainDevil/sub.git
```
除此之外，.git/config中末尾也多出了一段配置
```Bash
[submodule "public"]
	url = git@github.com:BackMountainDevil/sub.git
	active = true
```
此时我们对主仓库做一点改动，更新到github，可以从github上看到存储的public指向了对应仓库，而不是存储了sub仓库的文件
```Bash
$ echo "# main" >> README.md
$ git init
$ git add README.md
$ git commit -m "first commit on main"
$ git push
```
## 在本地主仓库修改子仓库
```Bash
$ cd public
$ echo "# sub edited by main" >> README.md
$ git commit -a -m "modify sub in main"
$ git push
```
push成功之后主仓库和子仓库都没有变化？？？这怎么回事？？再来一次
```Bash
echo "# sub edited by main 2" >> README.md
git add .
git commit  -m "modify sub in main 2"
git push
```
咋还是没变化，检查本地sub仓库的log居然是第一次commit的，但是在public下面查log却已经是第三次了，查github网站却还是显示第一次，这是为啥？？好一会才知道是网站缓存没那么快更新到。。。。等一下sub就会显示最新版本了。然而主站main指向的还是旧版的sub。
进入main运行`git submodule update --init --recursive`之后主仓库还是没有变化。。sub仓库反倒是被回滚到了第一版。。网站上的sub也变成了第一版。。。log记录查不到第三版了。。阿这？关闭了终端就没有办法了。。还好网页迟缓找到了第三版的版本号的开头ff5c，于是通过一下命令恢复到第三版的sub
`git reset --hard ff5c`

## 修改主仓库和子仓库
```Bash
echo "# sub edited by main 3" >> README.md
git add .
git commit -m "modify sub in main 3"

cd ../
echo "# main edited by main 1" >> README.md
git add .
git commit -m "main edited by main 1"
git push
```
刷新GitHub的网站，main已经更新了README.md和指向了新一版的sub，但是网页上sub还没有更新。。感觉不对劲。。又操作了一波才把sub推到GitHub上
```Bash
cd public
git push origin HEAD:main
```

## 删除子模块

```bash
# 查看子模块
$ git submodule 
 6a5b4068fc389ba7e33e1bfec2b5b97e21ca3c01 public (heads/main)
 564697987de962672112b910422682eb6f9c26ba themes/even (v4.1.0-3-g5646979)

# 拿删除 public 作为例子
$ git submodule deinit public
$ git rm --cached public
$ rm -rf .git/modules/public
$ rm -rf public
$ nano .git/config
$ nano .gitsubmodule
```

# References
- [gitsubmodules](https://git-scm.com/docs/gitsubmodules)
- [git-submodule](https://git-scm.com/docs/git-submodule)
- [git submodule 使用小结](https://www.jianshu.com/p/f8a55b972972/)
- [How to Set Up a Hugo Site on Github Pages - with Git Submodules!](https://www.adamormsby.com/posts/how-to-set-up-a-hugo-site-on-github-pages-with-submodules/)
- [Using Github Pages with Hugo](https://goparker.com/post/2018-05-08-github-pages-and-hugo/)
- [Git 如何移除 submodule.骆神 2018-02-23](https://blog.csdn.net/pcjustin/article/details/79350987)
