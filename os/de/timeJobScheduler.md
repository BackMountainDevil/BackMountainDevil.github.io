# 定时任务 crontab systemd
- date: 2024-5-22

按照预定的时间自动运行命令

## cron 

根据 [Cron - wiki](https://wiki.archlinux.org/title/Cron) 描述，安装 cronie 包，然后将该服务启动（`systemctl enable cronie --now`），然后编辑任务文件，通常一行代表一个定时任务，格式一般为“分 时 日 月 周几 命令”，除了用五个字段表示触发时间，还有一些特殊时间触发的关键词


```bash
crontab -e

10 4 * * * bash /home/mifen/Downloads/curl.sh
* 7 * * * cd /home/mifen/Documents/code && conda activate 310 && python main.py &> crontab.log
```

第一行的 `10 4 * * * bash /home/mifen/Downloads/curl.sh` 是指每天4点10分的时候，运行对应的脚本；
第二行的 `* 7 * * * cd /home/mifen/Documents/code && conda activate 310 && python main.py &> crontab.log` 是指每天7点的时候，进入对应的目录、激活 conda 环境、运行代码，将运行结果保存的文件中。(此行表述有误，参见下文)

### 第二行错误分析

然而第二个任务时间到了之后，并没有对应的日志文件（crontab.log）以及代码产生的日志。查看状态（`systemctl status cronie`）发现 cronie 在运行，运行日志显示识别到了对应的指令，同时日志也显示了可能存在问题的地方 - `conda: 未找到命令`，除此之外，日志时间是将近 8 点。

```bash
$ systemctl status cronie
     Loaded: loaded (/usr/lib/systemd/system/cronie.service; enabled; preset: disabled)
     Active: active (running) since Mon 2024-05-06 14:41:16 CST; 19h ago

5月 07 07:59:00 hp CROND[494348]: (mifen) CMD (cd /home/mifen/Documents/code && conda activate 310 && python main.py &> crontab.log)
5月 07 07:59:00 hp CROND[494346]: (mifen) CMDOUT (/bin/sh: 行 1: conda: 未找到命令)
```

然而我自己打开终端，运行指令，是可以正常运行的，有生成 crontab.log，同时，代码日志也记录到了文件，如下所示

```bash
$ cd /home/mifen/Documents/code
$ conda activate 310
$ python main.py &> crontab.log
```

那么这个问题，我想其它人也会遇到，检索一下：

- [crontab 使用 conda env 运行 python 脚本 宋小雅](https://zhuanlan.zhihu.com/p/337608389)：修改 SHELL、PATH 环境变量
   ```
   SHELL=/bin/bash
   PATH=/usr/bin:/bin:/home/public/anaconda3/bin
   # m h  dom mon dow   command
   29 16 * * 1-5 source activate env-name; python /path/to/task.py; source deactivate
   ```

- [crontab 调用python定时任务不执行，原因彻底分析 大Py 于 2020-05-30](https://blog.csdn.net/A_pinkpig/article/details/106425681)：默认用户是root，默认路径是/root/。办法是将 python 替换为对应的解释器路径

最后学到：可以用分号分隔指令，使用 python 解释器的绝对路径。修改后的指令如下：

```
* 7 * * * cd /home/mifen/Documents/code; /home/mifen/.conda/envs/310/bin/python main.py &> crontab.log
```

根据日志显示，第二天 7:00 ~ 7:59 这段时间内，代码重复运行多次，我想要的是代码只执行一次就够了。那么根据运行日志倒推，如果我想要实现每天 7 点运行一次代码，那么时间段表示应该加上指定运行的分钟，修改后的指令如下：

```
0 7 * * * cd /home/mifen/Documents/code; /home/mifen/.conda/envs/310/bin/python main.py &> crontab.log
```

根据运行日志显示，确实只在七点运行了一遍代码
