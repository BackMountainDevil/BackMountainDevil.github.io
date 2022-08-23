# linux 下的按键精灵 xdotool
- date: 2022-08-23
- lastmod: 2022-08-23

# 背景

学校开展啥在线入学教育，要看视频、答题，有的还需要多次手动翻页。针对手动翻页尝试在浏览器F12就会退出到平台的登陆入口而非学校的统一认证登陆。。看来js不太好搞，大佬们说搞一个按键精灵就好了。

# 案例

鼠标自动点击“下一页”、“翻页” ： watch -n 3 xdotool click 1

运行该代码之后将光标移动到下一页按钮出现的地方即可，每间隔3秒会自动点击一次。缺点也有，就是不同的学习课程的位置不是固定在右下角，有的在正中下方，移动鼠标到对应位置就好。自动答题暂时是没有的

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
