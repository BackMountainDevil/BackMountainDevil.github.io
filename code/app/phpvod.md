# CentOS 6.8 下 phpvod 搭建记录（ php52 的编译安装）

由于该项目的维护团队已不再维护该项目，因此这只是一个考古项目，目前众多的 Video CMS 有的是更好的选择。

- [CentOS 6.0下phpvod搭建教程(LAMP+phpvod)](https://www.cnblogs.com/lingerhk/p/3773982.html)

## mysql httpd

```bash
yum install -y mysql
mysql -V
# mysql  Ver 14.14 Distrib 5.1.73, for redhat-linux-gnu (x86_64) using readline 5.1
# 启动 mysql
/etc/init.d/mysqld start
## 剩下步骤参考 https://blog.csdn.net/weixin_43031092/article/details/107125167
## https://blog.csdn.net/weixin_43031092/article/details/105456843
# 设置数据库 root 密码
mysql_secure_installation

# 登陆数据库
mysql -u root -p
# 创建新账户
grant all on *.* to username@localhost IDENTIFIED by 'password';

yum install -y httpd
httpd -v
# Server version: Apache/2.2.15 (Unix)
# Server built:   Jun 19 2018 15:45:13
# 启动 apache 网站服务
service httpd start
```

## php 5.2

- [ Compiling PHP 5.2.17 on CentOS 6. Jun 03, 2016](https://community.webcore.cloud/tutorials/compiling_php_5217_on_centos_6/)

直接安装会是比较新的第七版，而老程序要求的模块必须在 5.2 之下，因此需要自行编译安装
- ubuntu 18.04 -> php 7.4
- centos 8.2 -> php 7.2
- centos 6.8/6.5 -> php 5.3

```bash
# 安装依赖包
yum -y install gcc automake autoconf libtool gcc-c++ gd zlib zlib-devel openssl openssl-devel libxml2 libxml2-devel libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libmcrypt libmcrypt-devel curl-devel httpd-devel bzip2-devel aspell-devel expat-devel libxslt-devel php-pdo php-pdo_mysql

# 下载源代码，版本发布清单 https://www.php.net/releases/
wget http://museum.php.net/php5/php-5.2.17.tar.gz
# 解压源代码
tar -zxvf php-5.2.17.tar.gz && cd php-5.2.17
# 配置参数
./configure --prefix=/usr/local/php52 --with-apxs2=/usr/sbin/apxs --with-config-file-path=/usr/local/lib/php52/  --disable-posix   --enable-bcmath   --enable-calendar   --enable-exif   --enable-fastcgi   --enable-ftp   --enable-gd-native-ttf   --enable-libxml   --enable-magic-quotes   --enable-mbstring   --enable-pdo   --enable-soap   --enable-sockets   --enable-wddx   --enable-zip  --with-bz2   --with-curl   --with-curlwrappers   --with-freetype-dir   --with-gd   --with-gettext   --with-jpeg-dir  --with-kerberos   --with-libexpat-dir   --with-libxml-dir  --with-libxml-dir   --with-mcrypt   --with-mhash   --with-mime-magic   --with-mysql  --with-mysqli   --with-openssl --with-openssl-dir --with-pcre-regex  --with-pdo-mysql   --with-pdo-sqlite   --with-pic   --with-png-dir   --with-pspell   --with-sqlite   --with-ttf   --with-xmlrpc   --with-xpm-dir=/usr/lib64/x11  --with-xsl --with-zlib   --with-zlib-dir

# 编译并安装
make -j `nproc` && make install

# 版本检查，正常的话会显示类似下面的结果
/usr/local/php52/bin/php -v
## PHP 5.2.17 (cli) (built: Oct 22 2021 21:06:41) 
## Copyright (c) 1997-2010 The PHP Group
## Zend Engine v2.2.0, Copyright (c) 1998-2010 Zend Technologies

# 加入 PATH
nano ～/.bashrc
## 把下面的内容添加到文件末尾
export PATH=/usr/local/php52/bin:$PATH
```

安装成功的话会显示一些比较重要的路径，如下所示

```bash
Installing PHP CLI binary:        /usr/local/php52/bin/
Installing PHP CLI man page:      /usr/local/php52/man/man1/
Installing build environment:     /usr/local/php52/lib/php/build/
Installing header files:          /usr/local/php52/include/php/
Installing helper programs:       /usr/local/php52/bin/
  program: phpize
  program: php-config
Installing man pages:             /usr/local/php52/man/man1/
  page: phpize.1
  page: php-config.1
Installing PEAR environment:      /usr/local/php52/lib/php/
```

### 错误解决

1. 配置报错 apxs

```bash
Sorry, I cannot run apxs.  Possible reasons follow:

1. Perl is not installed
2. apxs was not found. Try to pass the path using --with-apxs2=/path/to/apxs
3. Apache was not built using --enable-so (the apxs usage page is displayed)

The output of /usr/sbin/apxs follows:
./configure: line 6461: /usr/sbin/apxs: No such file or directory
configure: error: Aborting
```

参考[PHP编译报错​Sorry, I cannot run apxs. Possible reasons follow.科技小能手 2017-11-12](https://developer.aliyun.com/article/529750)
> yum install -y httpd-devel

2. 配置报错 BZip2

```bash
checking for BZip2 in default path... not found
configure: error: Please reinstall the BZip2 distribution
```

参考[编译安装 PHP 7.2 报错：checking for BZip2 in default path... not found configure: error: Please reinstall. PrinciplesMan 2021-07-16 10:01:35 120 ](https://blog.csdn.net/u010227042/article/details/118785890)，bzip 已经安装，缺少的是 dev 版本的
> yum install bzip2-devel -y

3. 编译报错 configure: error: libjpeg.(a|so) not found. 

参考[这个](https://blog.csdn.net/huangbaokang/article/details/86135843)
> cp -frp /usr/lib64/libjpeg.* /usr/lib

4. 编译报错 configure: error: libpng.(a|so) not found.

> cp -frp /usr/lib64/libpng* /usr/lib/

5. 配置报错 BZip2

尝试安装 libXpm-devel 后测试无效；参数指定 `--with-xpm-dir=/usr/lib` 测试无效；参考[这个](http://bbs.chinaunix.net/thread-3748557-1-1.html)
>    ln -s  /usr/lib64/libXpm.so*   /usr/lib/  
> 并且编译时指定，  --with-xpm-dir=/usr/lib64/x11

```bash
# 上面的方案其实蛮奇怪的，因为这个路径并不存在，但是它确实解决了这个问题
[root@VM-0-4-centos php-5.2.17]# cd /usr/lib64/x11
-bash: cd: /usr/lib64/x11: No such file or directory
[root@VM-0-4-centos php-5.2.17]# cd /usr/lib64/X11
[root@VM-0-4-centos X11]# 

# 查看相文件
cd /usr/lib
find ./ libXpm*
# libXpm.so.4
# libXpm.so.4.11.0

cd /usr/lib64
find ./ libXpm*
# libXpm.so
# libXpm.so.4
# libXpm.so.4.11.0
```

6. checking for U8T_DECOMPOSE... 
configure: error: utf8_mime2text() has new signature, but U8T_CANONICAL is missing. This should not happen. Check config.log for additional information.

参考[configure: error: utf8_mime2text() has new signature, but U8T_CANONICAL is missing. 2013](https://stackoverflow.com/questions/13436356/configure-error-utf8-mime2text-has-new-signature-but-u8t-canonical-is-missi)
> yum install -y libc-client-devel

于是报出了新的错误 `configure: error: Cannot find imap library (libc-client.a). Please check your c-client installation.`，干脆直接删除配置参数中的` --with-imap   --with-imap-ssl`

7. configure: error: Please reinstall libmhash - I cannot find mhash.h

参考[configure: error: Please reinstall libmhash – I cannot find mhash.h. 2013](https://addhewarman.com/2013/10/12/configure-error-please-reinstall-libmhash-i-cannot-find-mhash-h/)
> sudo yum install -y libmhash-devel

8. configure: error: Cannot find MySQL header files under yes.

```bash
checking whether to include mime_magic support... yes
checking for MING support... no
checking for mSQL support... no
checking for MSSQL support via FreeTDS... no
checking for MySQL support... yes
checking for specified location of the MySQL UNIX socket... no
checking for MySQL UNIX socket location... no
configure: error: Cannot find MySQL header files under yes.
Note that the MySQL client library is not bundled anymore!
```

[ 编译php时提示“Cannot find MySQL header files”的解决方法. vast2006. 2011-03-21](https://blog.51cto.com/summervast/521286)
> yum install -y mysql-devel 

9. 报错 configure: error: Cannot find libmysqlclient under /usr.
Note that the MySQL client library is not bundled anymore!

参考[ 编译安装php服务报错问题：configure: error: Cannot find libmysqlclient under /usr. 19/08/17](https://www.cnblogs.com/su-root/p/11370887.html),在 /usr/lib 没有 libmysqlclient，在 /usr/lib64 用 `find ./ *mysql*` 找到了不少

> ln -s /usr/lib64/mysql/libmysqlclient.so /usr/lib/libmysqlclient.so 

10. configure: error: Cannot find pspell

[Cannot find pspell – Fix PHP Configuration Error](https://techglimpse.com/cannot-find-pspell-fix-php-configuration-error/)
> yum install -y aspell-devel

11. configure: error: not found. Please reinstall the expat distribution

- [CentOS 5.5下编译php时的一些典型错误及解决办法(转载)](https://www.cnblogs.com/burningsky/articles/5065817.html)
- [Bug #21078 	configure: error: not found. Please reinstall the expat distribution.Bug #21078 	configure: error: not found. Please reinstall the expat distribution.](https://bugs.php.net/bug.php?id=21078&edit=1):not a bug，指定为 /usr 或者 /usr/lib64 无效
> yum install -y expat-devel 
> ln -s /usr/lib64/libexpat.so /usr/lib/libexpat.so

12. configure: error: xslt-config not found. Please reinstall the libxslt >= 1.1.0 distribution

> yum install -y libxslt-devel

13. apache httpd 没有正确解析 php 文件，只是单纯的返回 php 源代码，参考[linux apache不解析php怎么办. 2020-08-10](https://www.php.cn/php-ask-457634.html)

```php
// info.php
<?php phpinfo(); ?>
```

```bash
# 查找 httpd 的配置文件
find / -name "httpd.conf"
## /etc/httpd/conf/httpd.conf

# 编辑配置文件
nano /etc/httpd/conf/httpd.conf
## 粘贴进下面的内容
AddType application/x-httpd-php .php

## 找到 DirectoryIndex index.html index.html.var，在其后面追加 index.php ，如下所示
DirectoryIndex index.html index.html.var index.php
```

## Zend Optimizer 3.3.9

- [How to Install Zend Optimizer in Linux? By Jithin on April 4th, 2016](https://www.interserver.net/tips/kb/install-zend-optimizer-linux/#:~:text=In%20this%20documentation%2C%20we%20can%20check%20how%20to,the%20accounts%20using%20PHP%20version%205.2%20or%20below.)
> Please note that Zend Optimizer is only available for the accounts using PHP version 5.2 or below.  

- [Linux下ZendOptimizer的安装与配置方法. 2007年04月12日](https://www.jb51.net/article/9389.htm)

```bash
wget http://downloads.zend.com/optimizer/3.3.9/ZendOptimizer-3.3.9-linux-glibc23-x86_64.tar.gz
tar -xzvf ZendOptimizer-3.3.9-linux-glibc23-x86_64.tar.gz
cp ZendOptimizer-3.3.9-linux-glibc23-x86_64/data/5_2_x_comp/ZendOptimizer.so /usr/local/php52/lib/ZendOptimizer.so

nano /usr/local/lib/php52/php.ini
# 粘贴进下面的内容,第三行的路径取决与上面复制的结果
Zend Optimizer options 
zend_optimizer.optimization_level=1023
zend_extension=/usr/local/php52/lib/ZendOptimizer.so
```

## phpvod
### unrar

因为上传到服务器的源代码压缩格式是 rar，tar 不支持这个格式的压缩文件，这里编译安装一个 rar 来解决这个问题，当然也可以重新打包格式传到服务器。

```bash
# 下载源码，参考 https://www.jianshu.com/p/5a5f17e4a911
wget http://www.rarlab.com/rar/rarlinux-x64-5.3.0.tar.gz --no-check-certificate
# 解压
tar -zxvf rarlinux-x64-5.3.0.tar.gz
cd rar
# 安装 rar
make
```


### 安装

```bash
# 解压源代码
unrar x phpvod2_utf-8.rar
# 复制源代码到默认的网站根目录下
cp -r ./phpvod/* /var/www/html/
# 目录权限修改
chmod 777 /var/www/html -R
```

没有正确配置 zend 的话在首页（index.php）会显示没有安装 Zend Optimizer，正确配置之后首页是空白的，进入安装页面（install.php）

#### 问题解决

1. 安装页面中 install.htm 显示有一个错误 - **您的PHP环境没有开启短标记，安装无法继续。**，并且许可协议的文字显示为 `$land_licence`；而在 install.php 中没有这个错误出现并且许可协议完全显示为协议文字。

参考[php: "short_open_tag = On" not working. 2012 ](https://stackoverflow.com/questions/12579448/php-short-open-tag-on-not-working)，需要注意的是，如果 install.php 没有这个错误的话，可以忽略，因为这里通过 install.php 进行安装，而不是 install.htm。是否开启了短标记可以从 phpinfo 页面中进行查询。

```bash
# 编辑自定义的配置文件
nano /usr/local/lib/php52/php.ini
```

```ini
# 找到对应内容并在后边添加内容
AddType text/html .shtml
AddOutputFilter INCLUDES .shtml

AddType text/html .shtml .html .htm
AddOutputFilter INCLUDES .shtml .html .htm
```

2. 网站根目录 ......777属性检测不通过

`chmod 777 /var/www/html -R`

### 效果预览

![phpvod首页截图](https://img-blog.csdnimg.cn/ad15ccd7eead464d852687c3bcf1a341.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16)

![phpvod管理页面截图](https://img-blog.csdnimg.cn/cb156d92a0804c48b51300c1d6ef5194.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16)

![phpvod后台管理的自定义播放器格式页面](https://img-blog.csdnimg.cn/345f198eb967409eb90af0ffd2e05831.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAS2Vhcm5leSBmb3JtIEFuIGlkZWE=,size_20,color_FFFFFF,t_70,g_se,x_16)

