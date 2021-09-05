---
title: "使用 Github Action 自动部署 Hugo 博客"
date: 2021-08-24T16:36:20+08:00
lastmod: 2021-08-24T16:36:20+08:00
keywords: ['Github Action', 'hugo']
description: "简单的说，懒人大法，用 github action 省去一次生成和 push。"
tags: ['hugo']
categories: [Blog]
author: "筱氚"
---
# 干啥
每次 Hugo 生成完静态网页文件之后，需要分别 push 两次（源码和网页），听说 Github 有了自动化集成方案 Action 来减少一些工作。查了一下，现在是可以只需要 push 源码，剩下的静态网站生成和更新 Github Page 交给 Action。简单的说，懒人大法，用 github action 省去一次生成和 push。

当然，本地的 hugo-extend 还是要保留的，调试一下效果啥的。

# 动手
## Hugo

我这里是一个仓库（BackMountainDevil / HugoSource- ）存放源码，一个仓库（BackMountainDevil / BackMountainDevil.github.io ）存放网页作为 Page 仓库。
源码仓库中的 themes、public 均使用 submodule。主分支都是 main。 

## SSH 

这里需要重新生成一对密钥，使用之前用来配置过 Github 的密钥不能在这里使用啦，会报错提示已经被使用过啦。

```bash 
ssh-keygen -t rsa - -C "$(git config user.email)"
# 注意：这次不要直接回车，以免覆盖之前生成的
# 确认秘钥的保存路径（如果不需要改路径则直接回车）；如果已经有秘钥文件，则需要换一个路径，避免覆盖掉，如我更改之后的路径为 /home/kearney/.ssh_action/id_rsa；
# 创建密码（如果不需要密码则直接回车）； 
# 确认密码（如果不需要密码则直接回车），生成结束； 

# 查看公钥，路径需要改为你上面的设置
cat ~/.ssh_action/id_rsa.pub 
# 查看私钥
cat ~/.ssh_action/id_rsa
```

Page 仓库 BackMountainDevil / BackMountainDevil.github.io 中，点击 Setting - Deploy keys - Add deploy key，名称随意，粘贴进去刚生成的公钥，务必勾选 Allow write access 。点击保存。


源码仓库 BackMountainDevil / HugoSource- 。点击 Setting - Secrets - New repo secrets ，名称务必设置为 ACTIONS_DEPLOY_KEY， 添加刚刚生成的私钥(id_rsa)，变量名称是要在 action 的配置文件中使用的，因此要保持统一，可修改为别的名称，但要相同即可。


## 配置 Github Action
在源码跟目录下新建 `/.github/workflows/github-actions-demo.yml`，内容如下，需要更改的地方为 Page 仓库、分支。

```yml
name: Auto Deploy hugo
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository code
        uses: actions/checkout@v2
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: latest
      - name: Build 
        run: hugo
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }} # secret 中设置好私钥
          external_repository: BackMountainDevil/BackMountainDevil.github.io  # Page 仓库
          publish_branch: main  # Page 仓库的分支
          publish_dir: ./public # 静态网页路径
          commit_message: ${{ github.event.head_commit.message }}

```

如果有 submodule,上面的需要加上两三行，如下

```yml
      - name: Check out repository code
        uses: actions/checkout@v2
        with:
          submodules: recursive  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod
      - name: Setup Hugo
```

将 yml push 上去之后，再更新下文章，直接 push 源码，后台就会自动生成并发布到 Page 仓库。

测试良好，但也发现了一些问题，本人对 Action 了解不够深，猜测是 yml 没抄好。一个问题是源码仓库中 public 指向的网页仓库的 commit 标志未更新，但实际上对于的网页已经更新到 Page 中了，不过这不影响网页发布，暂不考虑解决这个问题。

其次是我的 submodule 主题中有个在 gitee 中，需要移回到 github 并设置，还有 public 也是，那么干脆就将这些 submodule 取消掉的了，直接并入源码仓库，也不要考虑啥 commit 指针标记了。

最后成果 - [筱氚 物语](https://backmountaindevil.github.io/)

# 参考
- [GitHub Actions/GitHub Actions 快速入门](https://docs.github.com/cn/actions/quickstart)：介绍是啥，有个案例可以实操
- [使用Github(Action)+Hugo搭建自己的博客。JiahongWu 2020-07-25](https://blog.csdn.net/weixin_41263449/article/details/107584336)：Page + 源码 两仓库
- [Hugo + Github Actions 实现自动化部署。林木木。 2020](https://immmmm.com/hugo-github-actions/)：一仓库两分支，附带同步到服务器宝塔后台的操作。
- [GitHub Action + Hugo 自动构建发布个人博客。曜灵。2020-09-13](https://zhuanlan.zhihu.com/p/240522090)：Page + 源码 两仓库，涉及到 submodule
