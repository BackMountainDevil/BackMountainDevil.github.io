# linux 下的按键精灵 xdotool
- date: 2022-08-23
- lastmod: 2022-09-09

# 背景

学校开展啥在线入学教育，要看视频、答题，有的还需要多次手动翻页。针对手动翻页尝试在浏览器F12就会退出到平台的登陆入口而非学校的统一认证登陆。。看来js不太好搞，大佬们说搞一个按键精灵就好了。

# 案例

鼠标自动点击“下一页”、“翻页” ： watch -n 3 xdotool click 1

运行该代码之后将光标移动到下一页按钮出现的地方即可，每间隔3秒会自动点击一次。缺点也有，就是不同的学习课程的位置不是固定在右下角，有的在正中下方，移动鼠标到对应位置就好。自动答题暂时是没有的

## 自动点击某位置的按钮

1. 首先用命令获取该位置的坐标: xdotool getmouselocation
2. 然后用移动指令: xdotool mousemove x y
3. 鼠标左键点击指令: xdotool click 1

我有5个竖排的按钮需要点击，点击之后还会有一个弹窗，弹窗的按钮也需要点击，分别获取6个按钮的坐标，然后编写即可

```bash
# 点击一个按钮的测试脚本，文件名tmp.sh
# usage：watch -n 3 bash tmp.sh
xdotool mousemove 800 679
xdotool click 1
xdotool mousemove 973 976
xdotool click 1
xdotool click 1
```

watch 的间隔时间可查(man watch)是最小0.1s，而且按钮之间应该加一个点击延迟(sleep 0.01),最后我没有使用watch,因为0.1s这个间隔还是有点慢，于是最终在脚本里写了一个死循环，如下所示

  <details>
  <summary>temp.sh</summary>

  ```bash
  # 点击多个按钮带弹窗的测试脚本，文件名temp.sh
  # usage：bash temp.sh
  #!/bin/bash
  while [ 1 ]
  do
    xdotool mousemove 800 679 # 移动到按钮一的位置
    xdotool click 1 # 左键点击按钮
    xdotool mousemove 973 976 # 点击弹窗的确认按钮
    xdotool click 1
    sleep 0.001 # 延迟1ms
    xdotool click 1 # 点击弹窗2的确认按钮
    sleep 0.01

    xdotool mousemove 800 823
    xdotool click 1
    xdotool mousemove 973 976
    xdotool click 1
    sleep 0.001
    xdotool click 1
    sleep 0.01

    xdotool mousemove 800 973
    xdotool click 1
    xdotool mousemove 973 976
    xdotool click 1
    sleep 0.001
    xdotool click 1
    sleep 0.01

    xdotool mousemove 800 1115
    xdotool click 1
    xdotool mousemove 973 976
    xdotool click 1
    sleep 0.001
    xdotool click 1
    sleep 0.01

    xdotool mousemove 800 1249
    xdotool click 1
    xdotool mousemove 973 976
    xdotool click 1
    sleep 0.001
    xdotool click 1
    sleep 0.01
  done
  ```
  </details>

## 退出点击进程

如果间隔比较长，能够把鼠标移动回终端并点击关闭就退出了，或者点击后Ctrl+C；如果时间间隔压根不够把鼠标挪动到终端，那么这个时候就要从桌面切换到tty了（Ctrl+Alt+F2），通过 ps查找进程号，用kill -9 关闭进程。比如使用 watch -n 0.1 bash tmp.sh 时就要切换到tty,然后使用 ps aux | grep watch 查找进程号,假设显示的进程号是911,则关闭进程指令为 kill -9 911；再比如死循环的 bash temp.sh，然后使用 ps aux | grep bash 查找进程号,假设显示的进程号是1988,则关闭进程指令为 kill -9 1988，最后使用 Ctrl+Alt+F1 切回桌面，如果点击进程还在跑，多半是进程号搞错了。

# 参考

- [ 推荐软件--linux下的按键精灵--图形界面 2010-10-14 bailiangcn](http://blog.chinaunix.net/uid-20147410-id-86066.html):xdotoolgui
  <details>
  <summary>回答摘录</summary>

  ```
  若干基本用法：
  xdotool mousemove x y
  将鼠标移动到在屏幕上特定的X和Y坐标位置
  xdotool click 1
  点击鼠标左键，1表示左键，2表示中键，3表示右键。
  xdotool key ctrl+l
  同时按下ctrl和l键

  更多命令详解请输入：man xdotool
    这个工具没有内置延时和循环功能。不过linux下提倡的就是一个软件做一件事，这个功能只要稍微组合一下就能实现了。
  举个例子：
  如果要鼠标每隔10秒点击左键一次
  我们可以用终端下的watch命令组合实现
  watch -n 10 xdotool click 1
  ```
  </details>
- [Linux下的按键精灵xdotool IT老吴 2020-03-03](https://blog.csdn.net/weixin_36572983/article/details/104636220)
