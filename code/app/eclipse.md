---
title: "在 Arch 上安装 eclipse-jee 并配置 tomcat7"
date: 2021-05-24T08:27:43+08:00
lastmod: 2021-05-24T08:27:43+08:00
keywords: ['eclipse', 'tomcat']
description: "在 Arch 上安装 eclipse-jee 并配置 tomcat7，以及配置 tomcat7 服务器遇到的问题"
tags: [eclipse]
categories: [dev-init]
author: "筱氚"
---
# 正文

软件工程需要用 jsp 开发写网站，老师 PPT 要求装 SVN、Tomcat、MyEclipse。SVN 是集中式的版本控制器，我打算用 Git 替代它，Tomcat 据说可以用 Nginx 替代，后面再尝试; MyEclipse 破解版网上好多，但是想用个 Eclipse,于是查一下 Arch Linux 上安装开发 jsp 的 Eclipse，整了老半天终于装上了，装上了还发现新建工程里面没有我要的 动态网站工程(Dynamic Web Project)，整了不少镜像和插件还是没得。最后又查了查才知道动态网站工程开发应该装 eclipse-jee，而不是 eclipse-jsp。

## 我的环境

```bash
$ screenfetch
                   -`                 
                  .o+`                 kearney@arch
                 `ooo/                 OS: Arch Linux 
                `+oooo:                Kernel: x86_64 Linux 5.12.5-arch1-1
               `+oooooo:               Uptime: 7m
               -+oooooo+:              Packages: 1412
             `/:-:++oooo+:             Shell: bash
            `/++++/+++++++:            Resolution: 2560x1440
           `/++++++++++++++:           DE: KDE 5.82.0 / Plasma 5.21.5
          `/+++ooooooooooooo/`         WM: KWin
         ./ooosssso++osssssso+`        GTK Theme: Breeze [GTK2/3]
        .oossssso-````/ossssss+`       Icon Theme: bloom-classic
       -osssssso.      :ssssssso.      Disk: 42G / 116G (38%)
      :osssssss/        osssso+++.     CPU: AMD Ryzen 5 4600U with Radeon Graphics @ 12x 2.1GHz
     /ossssssss/        +ssssooo/-     GPU: AMD/ATI
   `/ossssso+/:-        -:/+osssso+-   RAM: 3387215872-
  `+sso+:-`                 `.-/+oso: 
 `++:.                           `-/+/
 .`                                 `/
```

# Tomcat

```bash
sudo pacman -S tomcat7
# 启动tomcat服务，网址为 http://localhost:8080/，能正常浏览表面安装成功
sudo systemctl start tomcat7

# 删除注释，设置密码和角色
sudo nano /usr/share/tomcat7/conf/tomcat-users.xml

sudo systemctl restart tomcat7
```

修改之后的用户配置文件（tomcat-users.xml）大概涨下面这个样子，密码请自便，主要是角色上加上 manager-gui 才可以在网友管理，当然 admin-gui 是最高级的管理角色。

```xml
<?xml version="1.0" encoding="UTF-8"?>
  <role rolename="tomcat"/>
  <role rolename="role1"/>
  <user username="tomcat" password="121A=-1:*56" roles="tomcat"/>
  <user username="both" password="^SX3U456" roles="tomcat,role1,manager-gui,admin-gui"/>
  <user username="kearney" password="1a$2v^5*(6" roles="role1"/>
</tomcat-users>
```

## 文件系统机构层次

- /var/lib/tomcat7/webapps/ROOT/ 默认网址跟目录

# yay 安装 eclipse-jsp

看到不少帖子写的是用 pacman 下载，尝试了一下找不到目标，看了 Arch Wiki 发现 eclipse 的包已经转移到 AUR 中了。而且细分为 6 中不同语言对于的包，下面也标注了不支持同时安装好几个不同的 eclipse，存在冲突...

```bash
yay -S eclipse-javascript
```

但是这一下咋就把所有的都下载了呢？？？下个 java 我可以理解，但是把 cpp, php,rust 都下载了这整啥玩意哦？？？

这种方式自己验证后发现太浪费时间（将近 40 mins）和流量了，把所有版本的 eclipse 都下载一遍，然后再清理，二愣子操作还是我修行不够看不懂。。

```bash
# 查看 yay 版本
$ yay -V
yay v10.1.2 - libalpm v12.0.2
```

```bash
# 安装过程记录示意
$ yay -S eclipse-javascript
:: 正在检查冲突...
:: 正在检查内部冲突...
[Aur:1]  eclipse-2:4.18-2 (eclipse-javascript)

  1 eclipse (eclipse-javascript)             (构建文件已存在)
==> 清理哪些软件包的构建？
==> [N]没有 [A]全部 [Ab]中止 [I]已安装 [No]未安装 或 (1 2 3, 1-3, ^4)
==> N
:: PKGBUILD 是最新的，跳过 (1/1): eclipse (eclipse-javascript)
  1 eclipse (eclipse-javascript)             (构建文件已存在)
==> 显示哪些差异？
==> [N]没有 [A]全部 [Ab]中止 [I]已安装 [No]未安装 或 (1 2 3, 1-3, ^4)
==> N
:: (1/1) 正在解析 SRCINFO: eclipse (eclipse-javascript)
==> 正在创建软件包：eclipse 2:4.18-2 (2021年05月24日 星期一 08时24分21秒)
==> 获取源代码...
  -> 找到 commonify
  -> 找到 eclipse-java-2020-12-R-linux-gtk-x86_64.tar.gz
  -> 正在下载 eclipse-jee-2020-12-R-linux-gtk-x86_64.tar.gz...
** Resuming transfer from byte position 5976064
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  494M  100  494M    0     0   739k      0  0:11:25  0:11:25 --:--:--  727k
  -> 正在下载 eclipse-cpp-2020-12-R-linux-gtk-x86_64.tar.gz...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  358M  100  358M    0     0   616k      0  0:09:55  0:09:55 --:--:--  806k
  -> 正在下载 eclipse-php-2020-12-R-linux-gtk-x86_64.tar.gz...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  246M  100  246M    0     0   668k      0  0:06:16  0:06:16 --:--:--  806k
  -> 正在下载 eclipse-javascript-2020-12-R-linux-gtk-x86_64.tar.gz...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  304M  100  304M    0     0   577k      0  0:09:00  0:09:00 --:--:-- 1459k
  -> 正在下载 eclipse-rust-2020-12-R-linux-gtk-x86_64.tar.gz...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  266M  100  266M    0     0   635k      0  0:07:09  0:07:09 --:--:--  639k
==> 正在验证 source 文件，使用sha256sums...
    commonify ... 通过
==> 正在验证 source_x86_64 文件，使用sha256sums...
    eclipse-java-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
    eclipse-jee-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
    eclipse-cpp-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
    eclipse-php-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
    eclipse-javascript-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
    eclipse-rust-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
==> 正在创建软件包：eclipse 2:4.18-2 (2021年05月24日 星期一 09时08分17秒)
==> 正在检查运行时依赖关系...
==> 正在检查编译时依赖关系
==> 获取源代码...
  -> 找到 commonify
  -> 找到 eclipse-java-2020-12-R-linux-gtk-x86_64.tar.gz
  -> 找到 eclipse-jee-2020-12-R-linux-gtk-x86_64.tar.gz
  -> 找到 eclipse-cpp-2020-12-R-linux-gtk-x86_64.tar.gz
  -> 找到 eclipse-php-2020-12-R-linux-gtk-x86_64.tar.gz
  -> 找到 eclipse-javascript-2020-12-R-linux-gtk-x86_64.tar.gz
  -> 找到 eclipse-rust-2020-12-R-linux-gtk-x86_64.tar.gz
==> 正在验证 source 文件，使用sha256sums...
    commonify ... 通过
==> 正在验证 source_x86_64 文件，使用sha256sums...
    eclipse-java-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
    eclipse-jee-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
    eclipse-cpp-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
    eclipse-php-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
    eclipse-javascript-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
    eclipse-rust-2020-12-R-linux-gtk-x86_64.tar.gz ... 通过
==> 正在删除现存的 $srcdir/ 目录...
==> 正在释放源码...
==> 正在开始 prepare()...
==> 源代码已就绪。
==> 正在创建软件包：eclipse 2:4.18-2 (2021年05月24日 星期一 09时08分41秒)
==> 正在检查运行时依赖关系...
==> 正在检查编译时依赖关系
==> 警告： 使用现存的 $srcdir/ 树
==> 正在开始 build()...
==> 正在进入 fakeroot 环境...
==> 正在开始 package_eclipse-java()...
==> 正在清理安装...
  -> 正在删除 libtool 文件...
  -> 正在清除不打算要的文件...
  -> 正在移除静态库文件...
  -> 正在从二进制文件和库中清除不需要的系统符号...
  -> 正在压缩 man 及 info 文档...
==> 正在检查打包问题...
==> 正在构建软件包"eclipse-java"...
  -> 正在生成 .PKGINFO 文件...
  -> 正在生成 .BUILDINFO 文件...
  -> 正在生成 .MTREE 文件...
  -> 正在压缩软件包...
==> 正在开始 package_eclipse-jee()...
==> 正在清理安装...
  -> 正在删除 libtool 文件...
  -> 正在清除不打算要的文件...
  -> 正在移除静态库文件...
  -> 正在从二进制文件和库中清除不需要的系统符号...
  -> 正在压缩 man 及 info 文档...
==> 正在检查打包问题...
==> 正在构建软件包"eclipse-jee"...
  -> 正在生成 .PKGINFO 文件...
  -> 正在生成 .BUILDINFO 文件...
  -> 正在生成 .MTREE 文件...
  -> 正在压缩软件包...
==> 正在开始 package_eclipse-cpp()...
==> 正在清理安装...
  -> 正在删除 libtool 文件...
  -> 正在清除不打算要的文件...
  -> 正在移除静态库文件...
  -> 正在从二进制文件和库中清除不需要的系统符号...
  -> 正在压缩 man 及 info 文档...
==> 正在检查打包问题...
==> 正在构建软件包"eclipse-cpp"...
  -> 正在生成 .PKGINFO 文件...
  -> 正在生成 .BUILDINFO 文件...
  -> 正在生成 .MTREE 文件...
  -> 正在压缩软件包...
==> 正在开始 package_eclipse-php()...
==> 正在清理安装...
  -> 正在删除 libtool 文件...
  -> 正在清除不打算要的文件...
  -> 正在移除静态库文件...
  -> 正在从二进制文件和库中清除不需要的系统符号...
  -> 正在压缩 man 及 info 文档...
==> 正在检查打包问题...
==> 正在构建软件包"eclipse-php"...
  -> 正在生成 .PKGINFO 文件...
  -> 正在生成 .BUILDINFO 文件...
  -> 正在生成 .MTREE 文件...
  -> 正在压缩软件包...
==> 正在开始 package_eclipse-javascript()...
==> 正在清理安装...
  -> 正在删除 libtool 文件...
  -> 正在清除不打算要的文件...
  -> 正在移除静态库文件...
  -> 正在从二进制文件和库中清除不需要的系统符号...
  -> 正在压缩 man 及 info 文档...
==> 正在检查打包问题...
==> 正在构建软件包"eclipse-javascript"...
  -> 正在生成 .PKGINFO 文件...
  -> 正在生成 .BUILDINFO 文件...
  -> 正在生成 .MTREE 文件...
  -> 正在压缩软件包...
==> 正在开始 package_eclipse-rust()...
==> 正在清理安装...
  -> 正在删除 libtool 文件...
  -> 正在清除不打算要的文件...
  -> 正在移除静态库文件...
  -> 正在从二进制文件和库中清除不需要的系统符号...
  -> 正在压缩 man 及 info 文档...
==> 正在检查打包问题...
==> 正在构建软件包"eclipse-rust"...
  -> 正在生成 .PKGINFO 文件...
  -> 正在生成 .BUILDINFO 文件...
  -> 正在生成 .MTREE 文件...
  -> 正在压缩软件包...
==> 正在离开 fakeroot 环境。
==> 完成创建：eclipse 2:4.18-2 (2021年05月24日 星期一 09时19分15秒)
==> 清理中...

正在加载软件包...
正在解析依赖关系...
正在查找软件包冲突...

软件包 (1) eclipse-javascript-2:4.18-2

全部安装大小：  443.90 MiB

:: 进行安装吗？ [Y/n] 
(1/1) 正在检查密钥环里的密钥                                       [####################################] 100%
(1/1) 正在检查软件包完整性                                         [####################################] 100%
(1/1) 正在加载软件包文件                                           [####################################] 100%
(1/1) 正在检查文件冲突                                             [####################################] 100%
(1/1) 正在检查可用存储空间                                         [####################################] 100%
:: 正在处理软件包的变化...
(1/1) 正在安装 eclipse-javascript                                  [####################################] 100%
:: 正在运行事务后钩子函数...
(1/3) Arming ConditionNeedsUpdate...
(2/3) Updating icon theme caches...
(3/3) Updating the desktop file MIME type cache...

```

安装之后确实能启动编辑 jsp 文件，也后续改镜像装 jsp 的插件，但还是没有课程需要的动态网页工程(Dynamic Web Project)和设置里的 Tomcat（已安装）。 

## 卸载

```bash
$ yay -Rs eclipse-javascript
正在检查依赖关系...
:: wxgtk3可选依赖于webkit2gtk: for webview support

软件包 (9) java-environment-common-3-3  jdk-openjdk-15.0.2.u7-1  libmanette-0.2.6-2
           libwpe-1.10.0-1  unzip-6.0-14  webkit2gtk-2.32.1-1  wpebackend-fdo-1.9.92-1
           xdg-dbus-proxy-0.1.2-3  eclipse-javascript-2:4.18-2

全部移去体积：  632.35 MiB
```

然后删除用户目录下的 `.eclipse`，过了一阵子我才发现上面这一步不小心把其它包给删除了，本来应该只删除 eclipse-javascript-2:4.18-2 的，**正确**的卸载方式是 `yay -R eclipse-javascript`

### 补救

后面发现 java 没有了。。。回头看是上面那不小心卸载了，虽然不知道其它的是啥，但还是补上为好（我的arch前几天滚动到最新版的了，所以补救安装没有带版本号，如果你的不是最新，最好还是带上版本号）

```bash
sudo pacman -S java-environment-common jdk-openjdk libmanette libwpe unzip webkit2gtk wpebackend-fdo xdg-dbus-proxy
```

# tar 安装

## 下载安装包

[Eclipse  IDE 2021-03 R Packages](https://www.eclipse.org/downloads/packages/release/2021-03/r)：选择对于的版本之后可以选择比较近的镜像，下载速度飞起。我下载的是 Eclipse IDE for Enterprise Java and Web Developers（519 M），选择隔壁的 TUNA 镜像下载好 `eclipse-jee-2021-03-R-linux-gtk-x86_64.tar.gz`。

## 文件完整性校验

从刚才的地方下载那个文件对于的校验码，如我下的是 `eclipse-jee-2021-03-R-linux-gtk-x86_64.tar.gz.md5`，当然也可以下载其他的校验码（sha1、sha512）

KDE 自带的文件管理器 Dolphin 有个比较好的功能，右键文件属性有个校验和校验功能。即可计算又可粘贴校验。

下面是命令行校验方法示意  
```bash
# md5 校验
$ md5sum eclipse-jee-2021-03-R-linux-gtk-x86_64.tar.gz
8428cf7b84b2c2988678f93250fc85a6  eclipse-jee-2021-03-R-linux-gtk-x86_64.tar.gz
# sha1 校验
$ shasum eclipse-jee-2021-03-R-linux-gtk-x86_64.tar.gz
a757f27a65587a8e61335d49725a672cf3e703ea  eclipse-jee-2021-03-R-linux-gtk-x86_64.tar.gz
```

## 安装

```bash
cd ~/Downloads
sudo mv eclipse-jee-2021-03-R-linux-gtk-x86_64.tar.gz /opt/
cd /opt
sudo tar -zxvf eclipse-jee-2021-03-R-linux-gtk-x86_64.tar.gz
# 删除压缩包
sudo rm eclipse-jee-2021-03-R-linux-gtk-x86_64.tar.gz
```

然而使用 ls 命令一看压根没生成 eclipse-installer 包，直接一个 eclipse 文件夹。进去发现有一个叫 eclipse 的可执行文件，双击就打开了 eclipse-jee 版的 IDE，那问题就是缺少一个快捷启动方式了。

```bash
cd eclipse
./eclipse
```

## 桌面快捷方式

```bash
cd /usr/share/applications/
sudo nano org.eclipse.desktop
```

往里面粘贴这些内容后保存退出 nano，这样打开菜单里的‘工具’或者‘开发’一栏就能找到它了。

```txt
[Desktop Entry]
Name=Eclipse
Comment=Eclipse IDE For JEE
Exec=/opt/eclipse/eclipse
Icon=/opt/eclipse/icon.xpm
Type=Application
StartupNotify=true
Categories=Utility;Development;IDE;
Keywords=eclipse;
```

## 配置 tomcat7

pacman 安装的 tomcat 可以正常运行，在默认网站根目录下写的文件也可以正常访问。但是在 eclipse 中添加该 tomcat7 却报错无法正常加载其配置文件，按照网上的办法试了不少，均已失败告终：添加动态链接文件、删除 eclipse workspace 中某两文件（我这里本来就没有）、service.xml中的拼写错误、重装 tomcat7、配置 jre、删除重新添加。。。

查了 tomcat7、java 的版本，也都是兼容没问题的，愣是没想明白问题出在哪里了。然后我又尝试用 eclipse 安装 tomcat7,打算安装在 /opt 下失败了，用 root 权限安装到那也失败了，最后是换了个自己目录（/home/kearney/app/tomcat7/）龟速安装成功了。

查看通过 eclipse 安装的 tomcat7 发现它的目录结构是就是全部在一个路径下，不像通过 pacman 安装的 tomcat7, 各种文件分布在不同的地方，所以网上最高赞的回答是用动态链接，尝试了几次动态链接也没成功（本身就有一个 conf 动态链接），不知道这样子失败的原因，但还好找到了一直解决办法。

虽然无法在 eclipse 里安装到 /opt/ 下，用 sudo 运行 eclipse 也不行，于是先安装在自己目录下，可以正常配置运行环境，然后尝试复制一份到 /opt/ 下，测试之后也是可以的。自己目录下的先留着做一个备份，过一阵子再删除也不迟。当然一直用自己目录下的也没啥问题，不一定非得移动到 /opt/ 下，个人习惯而已。

```bash
cd /home/kearney/app/tomcat7/
sudo cp -r apache-tomcat-7.0.47/ /opt/apache-tomcat-7.0.47/
```

# 参考

[Tomcat  Arch Wiki](https://wiki.archlinux.org/title/Tomcat#Initial_configuration)

[Eclipse Arch Wiki](https://wiki.archlinux.org/title/Eclipse#Installation)

[Eclipse  IDE 2021-03 R Packages](https://www.eclipse.org/downloads/packages/release/2021-03/r)：（2021-03在2021-05是最新的稳定版，以后不一定是了）

[Eclipse 各版本下载地址 TUNA 镜像](https://mirrors.tuna.tsinghua.edu.cn/eclipse/technology/epp/downloads/release/):M1, M2 都是不稳定版（测试开发ing）。目录结构不是很好看懂。

[Eclipse IDE for Enterprise Java and Web Developers ](https://www.eclipse.org/downloads/packages/release/2021-03/r/eclipse-ide-enterprise-java-and-web-developers)：249M.不可以自由选择镜像！右侧的橙色下载按钮极具误导性（点击下载的是Eclipse IDE 而不是正在展示的 eclipse-jee），真正的下载链接在中间不起眼的位置 Download Links

[Eclipse jee下载，提供国内清华大学镜像点下载 废人一枚 2020-02-27](https://blog.csdn.net/xunxue1523/article/details/104545260)

[[ubuntu-linux]安装eclipse全流程 小梅冲冲冲 2021-03-29](https://blog.csdn.net/qq_43713904/article/details/115303620)：我没出现 eclipse-installer

[Linux下安装Eclipse以及Java  Cherrison_Time  2019-9](https://www.cnblogs.com/Cherrison-Time/p/11559784.html)：我没出现 eclipse-installer;快捷方式教程好评

# End
