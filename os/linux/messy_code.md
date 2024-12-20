# 文件名称和内容乱码 unzip -O CP936
- date: 2024-12-20

之前用 Endeavouros 的时候，有时候下载的文件名是乱码，有时候打开的下载文件是乱码，如下图所示。

```bash
$ unzip Linux_install.zip 
Archive:  Linux_install.zip
  inflating: Linux_install/agent_live_linux_x86_64.sh  
  inflating: Linux_install/efs.x86_64.bin  
  inflating: Linux_install/╧╚░▓╫░agent╘┘░▓╫░efs.x86_64.bin.txt

$ more Linux_install/╧╚░▓╫░agent╘┘░▓╫░efs.x86_64.bin.txt 
��װagent���򣬸���ϵͳ�Ĳ�ͬ�����в�� ��ѡ��ִ�а�װ���� ��./  ��װ��.sh   ���� sh ./  ��װ��.sh

�ͻ��˰�װ��
�ɲο�����֧��https://help
```

如果使用 vs code 打开乱码文件，选择用 gb2312 编码打开就能正常显示了，说明这种文件内容显示异常是由于编码和解码不一致而引发的。

## 解压后文件名乱码

可以给 unzip 加上参数 -O CP936，不过这不能解决文件内容乱码

```bash
$ unzip -O CP936 Linux_install.zip 
Archive:  Linux_install.zip
  inflating: Linux_install/agent_live_linux_x86_64.sh  
  inflating: Linux_install/efs.x86_64.bin  
  inflating: Linux_install/先安装agent再安装efs.x86_64.bin.txt

$ more Linux_install/先安装agent再安装efs.x86_64.bin.txt
��װagent���򣬸���ϵͳ�Ĳ�ͬ�����в�� ��ѡ��ִ�а�װ���� ��./  ��װ��.sh   ���� sh ./  ��װ��.sh
```

错码解压后再转码测试不行

```bash
$ convmv -f GBK -t utf8 --notest -r .
Skipping, already UTF-8: ./╧╚░▓╫░agent╘┘░▓╫░efs.x86_64.bin.txt
Ready! I converted 0 files in 0 seconds.
$ ls
╧╚░▓╫░agent╘┘░▓╫░efs.x86_64.bin.txt  agent_live_linux_x86_64.sh  efs.x86_64.bin
```

## 文件内容乱码

需要将文件内容转码，下面就是将 gbk 格式的文件转为utf8，然后保存到utf8.txt 文件中

```bash
$ iconv -f GBK -t UTF-8  "╧╚░▓╫░agent╘┘░▓╫░efs.x86_64.bin.txt" -o utf8.txt
```

ps: nano 的 -I -L 参数实测都不是指定编码的，帮助文档说了分别是忽略配置文件、不在末尾添加新行

# 参考

[解决windows的zip压缩包在linux下解压后中文乱码问题 Zero_to_zero1234 2019-06-04](https://blog.csdn.net/suiyueruge1314/article/details/90766185)

[Linux与Windows 解压乱码 UTF8BOM读取问题  Hello~again ](https://www.cnblogs.com/louyihang-loves-baiyan/p/4864301.html)
> 最早的GBK编码，就是IBM定制的MBCS字符集，汉子编码正好在整个字符集中的936页，因此好多地方其实都是用CP936来代表GBK

[Linux命令：convmv（文件名转码的工具） ](https://tech.powereasy.net/cpzsk/wzfwqwlaq/content_24390)
> convmv 只是文件名编码的转换，文件内容不会发生变化。文件内容编码的转换，请使用 iconv 命令
