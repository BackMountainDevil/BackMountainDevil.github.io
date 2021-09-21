# GitConfigUseage
date: 2020-11-16
lastmod: 2020-11-16

# Intro
记录Git的配置过程
# Down
```bash
sudo apt install git
```

# configSSH
## basic
设置用户名和邮箱，这样就可以在本地记录变化了
```bash
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"
# 查看git用户配置，看下有没有打错
$ git config --global  --list
```
## SSH 
现在Github、Gitee都支持SSH，GPG这里就不说了，反正把本地代码push到远程仓库需要靠SSH来确认身份。
```bash
$ cd ~/.ssh
# 没有那个文件说明还没有配置过
$ ssh-keygen -t rsa -C "$(git config user.email)"
# 参数 -t rsa 表示使用 rsa 算法进行加密, -C 表示邮箱，这里使用 git 的配置邮箱，可替换具体的邮箱
# 三次回车
    # 确认秘钥的保存路径（如果不需要改路径则直接回车）；如果已经有秘钥文件，则需要确认是否覆盖（如果之前的秘钥不再需要则直接回车覆盖，如需要则手动拷贝到其他目录后再覆盖）；
    # 创建密码（如果不需要密码则直接回车）； 
    # 确认密码；
# 在 ~/.ssh（默认路径）下有两个文件：id_rsa(私钥)和id_rsa.pub(公钥)
# 查看公钥
$ cat ~/.ssh/id_rsa.pub 
```
然后将显示出的公钥（ssh-rsa开头的）复制后添加到Gituhub的SSH and GPG keys里面即可。
### Test
测试下能否正常通过ssh协议握手，回车后会看到yes/no，输入yes继续回车即可
```bash
$ ssh -T git@github.com
// 国内码云则用下面的进行测试
$ ssh -T gitee@gitee.com
```
成功的样子
```bash
The authenticity of host gitee.com  can not  be established.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added gitee.com (ECDSA) to the list of known hosts.      
Hi Kearney! You have successfully authenticated, but GITEE.COM does not provide shell access.
# 或者是这样子
Hi BackMountainDevil! You have successfully authenticated, but GitHub does not provide shell access.
```
失败的模样，还没有添加ssh公钥
```bash
git@github.com: Permission denied (publickey).
```

# 指令
## 基本指令
```bash
# 新建仓库
git init
# 查看仓库状态
git status
# 添加全部更新的文件（通常是添加某个具体的文件）
git add .
# 提交更新
git commit -m "更新了xx更能，修复了xx问题"

# 添加远程仓库，命名为origin
git remote add origin Rpo_Url

# 将main分支推送到远程仓库origin
git push -u origin main

# 查看远程仓库
git remote -v

# 从远程仓库拷贝代码到本地
git clone Rpo_Url
# 从远程仓库拷贝代码到本地的dir文件夹中
git clone Rpo_Url dir
# 从远程仓库拷贝最新的代码到本地（不包含历史记录）
git clone Rpo_Url --depth=1
```

## 进阶指令
```bash
# 将本地的 tag 同步到远程仓库，单 push 不会同步 tag
git push --tags
```

# References

- [git](https://git-scm.com/)
- [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)
