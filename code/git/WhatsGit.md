# WhatsGit
- date: 2020-08-27
- lastmod: 2024-11-06

# Git是什么
[Git Book](https://git-scm.com/book/zh/v2)

分布式版本管理工具



# 怎么用Git

## 安装

## 初始化用户

```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```



## 创建版本库

git init

备注：不要使用Windows自带的**记事本**编辑任何文本文件，使用Notepad++（编码设置为UTF-8 without BOM）。



## 添加文件到版本库

git add "filename"



## 提交文件到仓库

git commit -m "版本解释说明"



## 查看状态和修改内容

git status

会显示哪些文件修改了还没有提交

git diff "filename"

查看未提交的文件修改了哪些内容



## 查看版本日志

git log

查看已经提交的所有版本commit

如果觉得现实内容太多，可以使用 git log --pretty=oneline



## 版本回退

HEAD 当前版本

HEAD^  上一个版本

HEAD^^  上上个版本

...有几个^就回退到上几个版本

例如回退到上一版 

git reset --hard HEAD^

此时再看日志最新版的已经没有了，如果想恢复最新版的话，只要Git命令行窗口没关，往上找刚才的log里面最新版的版本号如 50c43671c4e496e6827cd228a3617c3c0b4aa9e8 ，采用如下命令

git reset --hard 50c4

hard后面一般是跟上版本号前几位，前提是没有相同开头的其它版本号，多了就复制粘贴全部版本号

如果第二天起床才想起回到未来，要到哪里去找版本号commit呢？

git reflog



# 远程仓库

## SSH key

新建添加

## 从远程仓库克隆

git clong git@github.com:"name/repo"

例如： git clone https://github.com/python/cpython.git

上面的例子中，仓库地址用的 https，一般针对公开仓库使用，私有仓库用 https 的话还要输入密码，不方便，因此私有仓库大多配置 ssh。

指定分支的话，在仓库地址前用参数 `-b` (等同于 `--branch`)指定就好了，比如 git clone -b 3.12 https://github.com/python/cpython.git

如果仓库太大，全部克隆会很花时间，那么可以在仓库地址前用参数 `--depth` 指定克隆深度，数字越小则克隆的越少，如 git clone --depth 1 https://github.com/python/cpython.git

## 查看远程仓库

```console
git remote -v
```

## 添加远程仓库

```console
git remote add 本地仓库名字 URL
```

## 推送本地仓库到远程仓库

git push

# 分支管理

## 新建分支

git branch "newname"



## 切换分支

git checkout "newname"

或 git switch "newname"

## 查看分支

git branch

列出的所有分支中，前面有*的表示当前分支

## 新建并切换分支

git checkout -b "newname"

或 git switch -c "newname"

## 合并分支

git merge "newname"



## 删除分支

git branch -d "newname"



# 标签

## 创建标签

首先切换到需要打标签的分支

git tag "tagname"

标签名多为版本号

## 删除标签

git tag -d "tagname"

