# 干啥
在 Plasma(KDE) 上借助 gestures、xdotool 设置多手指触控手势，如双指缩放，三指换任务、桌面等。

## 前情提要（可跳过进入下一节 “怎么做”）
Plasma 桌面中有虚拟桌面和活动两者神奇的玩意（可在 “设置“-“工作区行为“ 中找到）；看起来效果都差不多，都是切换出了一个新桌面。但是切换活动有快捷键（Meta+Tab），切换虚拟桌面则没有，只有一个 Ctrl+F8 显示所有虚拟桌面，可以在任务栏加一个调度器来切换虚拟桌面。然而连遍历窗口都有个 Alt+Tab 。。。

便想着自定义快捷键（可在 “设置“-“快捷键“ 中找到），然而学了下里面的示例和 Konqueror手势，自己新建了一个发现不太行，不知道切换虚拟桌面的“动作“，尝试绘制手势的触发器的时候发现用触摸板无法绘制。。想起之前看到 Hello Github 推荐了一个 coder 借助 libinput 写了个快捷切换的，本想着去看一下怎么借鉴，后来想一下会不会有人写好了，搜索一番确实是有人写好了。

# 怎么做

- Linux xx 5.13.12-1-MANJARO
- gestures 0.2.5-1
- libinput-gestures 2.67-1
- xdotool 3.20210804.2-1

我使用的是 Manjaro，可以方便的在 pamac 中安装 xdotool, gestures。Arch 系列需要从 AUR 中获取。不得不说，这一点上 manjaro 已经将 gestures 并入主仓库，而且将 libinput-gestures 作为依赖， AUR 的并未将其作为依赖（这种方式更符合 KISS 原则）。

安装完成之后需要将自己加入 input 用户组 - `sudo gpasswd -a $USER input`

之后就是启动 Gesture 来添加自定义触摸手势了。点击左上角的加号来添加新手势，类型（Type）可以选择 Swipe（滑动、刷） 或者 Pinch（捏），之后根据类型选择方向，最后选择指数（234，滑动模式仅支持34，不然会和系统冲突）。

选择好参数之后，在底部的输入框输出对于的快捷键指令，然后点击 Confirm 确定。比如你想给 F5 添加手势，对应的指令就是 `xdotool key F5`，想给遍历窗口加手势对于的指令是 `xdotool key Alt+Tab`，很好理解，最后加键位，多个按键用加号连接。

设置完之后想要修改就点击左上角加号旁边的铅笔，这时候就可以对已有的设置进行修改或删除。

# 参考
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
- []()
- []()
- []()