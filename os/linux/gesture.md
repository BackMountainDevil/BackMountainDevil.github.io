# Linux 设置多指触控手势，以 Manjaro 为例
- date: 2020-09-5
- lastmod: 2022-12-22

在 Plasma(KDE) 上借助 gestures、xdotool 设置多手指触控手势，如双指缩放，三指换任务、桌面等。

## 前情提要（可跳过进入下一节 “怎么做”）

Plasma 桌面中有虚拟桌面和活动两者神奇的玩意（可在 “设置“-“工作区行为“ 中找到）；看起来效果都差不多，都是切换出了一个新桌面。但是切换活动有快捷键（Meta+Tab），切换虚拟桌面则没有，只有一个 Ctrl+F8 显示所有虚拟桌面，可以在任务栏加一个调度器来切换虚拟桌面。然而连遍历窗口都有个 Alt+Tab 。。。

便想着自定义快捷键（可在 “设置“-“快捷键“ 中找到），然而学了下里面的示例和 Konqueror手势，自己新建了一个发现不太行，不知道切换虚拟桌面的“动作“，尝试绘制手势的触发器的时候发现用触摸板无法绘制。。想起之前看到 Hello Github 推荐了一个 coder 借助 libinput 写了个快捷切换的，本想着去看一下怎么借鉴，后来想一下会不会有人写好了，搜索一番确实是有人写好了。

# 怎么做

- Linux 5.15.84-1-lts
- libinput-gestures 2.73-1
- xdotool 3.20211022.1-1

## 方法一：libinput-gestures

1. 安装 libinput-gestures, xdotool: `sudo pacman -S libinput-gestures xdotool`
2. 把自己加入 input 用户组 - `sudo gpasswd -a $USER input`
3. 设置自定义手势: `nano ~/.config/libinput-gestures.conf`
4. 启动&设置开机自启动: `libinput-gestures-setup start && libinput-gestures-setup autostart`

## 自定义触控手势

libinput-gestures-setup start|stop|restart|autostart|autostop|status

libinput-gestures 的用户配置文件位于 $HOME/.config/libinput-gestures.conf，我设置了四个自定义手势，分别是三指上下左右滑动：查看全部桌面、查看所有活动、切换到第二个桌面、切换到第一个桌面。参考 [陆道峰](https://segmentfault.com/a/1190000011327776)的文章添加了网页双指缩放的快捷手势（发现里面的 ctrl 不区分大小写唉）

```
gesture swipe up 3 xdotool key ctrl+F8
gesture swipe down 3 xdotool key ctrl+F10
gesture swipe left 3 xdotool key Ctrl+F2
gesture swipe right 3 xdotool key Ctrl+F1

gesture pinch in 2 xdotool key ctrl+minus
gesture pinch out 2 xdotool key ctrl+plus
```
## 方法二：gestures

我使用的是 Manjaro，可以方便的在 pamac 中安装 xdotool, gestures。Arch 系列需要从 AUR 中获取。不得不说，这一点上 manjaro 已经将 gestures 并入主仓库，而且将 libinput-gestures 作为依赖。总的来说， libinput-gestures 是一个很好的软件，可以自定义手势，而 gestures 呢就是给它套上一层包装，让不会命令行的人也能用（目前来说还需要搜索能力）。

安装完成之后需要将自己加入 input 用户组 - `sudo gpasswd -a $USER input`

之后就是启动 Gesture 来添加自定义触摸手势了。点击左上角的加号来添加新手势，类型（Type）可以选择 Swipe（滑动、刷） 或者 Pinch（捏），之后根据类型选择方向，最后选择指数（234，滑动模式仅支持34，不然会和系统冲突）。

选择好参数之后，在底部的输入框输出对于的快捷键指令，然后点击 Confirm 确定。比如你想给 F5 添加手势，对应的指令就是 `xdotool key F5`，想给遍历窗口加手势对于的指令是 `xdotool key Alt+Tab`，很好理解，最后加键位，多个按键用加号连接。

设置完之后想要修改就点击左上角加号旁边的铅笔，这时候就可以对已有的设置进行修改或删除。

# 参考

- [在 Mac 上使用多点触控手势](https://support.apple.com/zh-cn/HT204895)
- [Mac触控板常用的手势操作，让你告别Windows鼠标！. Topbook.Topbook](https://zhuanlan.zhihu.com/p/35557703)
- [Arch Linux触摸板手势设置ibinput-gestures. BigTaiYang大太阳.2020.04.24](https://www.jianshu.com/p/2b0caa18bfd8)
- [Linux配置触控板二指，三指，四指滑动捏合手势 JontyShaw 2020-04-10 2](https://blog.csdn.net/qq_37284020/article/details/105441741)：gestures libinput-gestures
  > 
  ```bash
  sudo pacman -S gestures
  sudo gpasswd -a $USER input
  libinput-gestures-setup autostart
  ```

- [kde5与archlinux环境下配置libinput-gestures多手势操作。陆道峰。发布于 2017-09-23 ](https://segmentfault.com/a/1190000011327776)：对标 Mac 的设置
  > 查询资料后发现touchegg和libinput-gestures都可以满足需求，但是在实际配置过程中，touchegg存在很多问题一直都没能解决，而libinput-gestures基本没有太大的问题。所以本文主要介绍如何配置libinput-gestures
- [系统设置/快捷键和手势](https://userbase.kde.org/System_Settings/Shortcuts_and_Gestures/zh-cn)

