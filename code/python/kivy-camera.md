# kivy（2.0.0） Camera 摄像头案例运行异常的解决办法
- Time: 2021-9-11

# 简介

人生最大的烦恼 - 造轮子，这个轮子其实有人写好了，但是写的不咋的，运行起来都让我修改了不少代码，折腾了一个多月才勉强跑起来，今天实在是闲着（主要是工作没着落。。），要不把这个轮子造的更优一点。

说起来也很简单，给搞研究的科学人员写一个上位机界面（原有的不稳定）。考虑了下可以用 flask 或者 django 写，这样比较熟悉；又想了一下要不要直接上 gui 得了，比如 pyqt、wxpython 啥的，之后看了下各自的开源协议，pyqt 是 GPL 感染的、wxpython 是 LPGL、kivy 新版采用 MIT。

考虑了一下还是采用 MIT 协议的 kivy。

### 平台信息

- OS: Linux xx 5.13.13-1-MANJARO x86_64 GNU/Linux
- Python: 3.9.6
- kivy: 2.0.0

# 具体过程
## 安装

根据 [官方文档](https://kivy.org/doc/stable/gettingstarted/installation.html) 可以采取比较简单的 pip 或者 conda 进行安装，试了一下 pip,一下子就安装上了

```bash
# 建立虚拟环境（这是一个好习惯）
python -m venv env
source env/bin/activate
# 安装完全版的 kivy
pip install kivy[full]
```

### 测试

然后运行 [最简单的窗口案例](https://kivy.org/doc/stable/guide/basic.html#quickstart)，没有啥问题说明安装还是很完美的（至少目前是这样的），实践表明版本号那行没有也行。

<details>
<summary>点击展开代码</summary>

```python
import kivy
from kivy.app import App
from kivy.uix.label import Label
class MyApp(App):
    def build(self):
        return Label(text='Hello world')
if __name__ == '__main__':
    MyApp().run()
```
</details>

## 摄像头案例

在 [Camera Example](https://kivy.org/doc/stable/examples/gen__camera__main__py.html) 中官方给出了案例代码，一键进行 cv 操作之后运行发现无法运行。

<details>
<summary>点击查看代码（与官网一致）</summary>

```python
'''
Camera Example
==============

This example demonstrates a simple use of the camera. It shows a window with
a buttoned labelled 'play' to turn the camera on and off. Note that
not finding a camera, perhaps because gstreamer is not installed, will
throw an exception during the kv language processing.

'''

# Uncomment these lines to see all the messages
# from kivy.logger import Logger
# import logging
# Logger.setLevel(logging.TRACE)

from kivy.app import App
from kivy.lang import Builder
from kivy.uix.boxlayout import BoxLayout
import time
Builder.load_string('''
<CameraClick>:
    orientation: 'vertical'
    Camera:
        id: camera
        resolution: (640, 480)
        play: False
    ToggleButton:
        text: 'Play'
        on_press: camera.play = not camera.play
        size_hint_y: None
        height: '48dp'
    Button:
        text: 'Capture'
        size_hint_y: None
        height: '48dp'
        on_press: root.capture()
''')


class CameraClick(BoxLayout):
    def capture(self):
        '''
        Function to capture the images and give them the names
        according to their captured time and date.
        '''
        camera = self.ids['camera']
        timestr = time.strftime("%Y%m%d_%H%M%S")
        camera.export_to_png("IMG_{}.png".format(timestr))
        print("Captured")


class TestCamera(App):

    def build(self):
        return CameraClick()


TestCamera().run()
```
</details>

### 错误

<details>
<summary>点击查看报错信息</summary>

主要是 picamera - ModuleNotFoundError: No module named 'picamera'
```bash
$ /home/kearney/Documents/code/python/uc2-gui/env/bin/python /home/kearney/Documents/code/python/uc2-gui/cam.py
[INFO   ] [Logger      ] Record log in /home/kearney/.kivy/logs/kivy_21-09-12_5.txt
[INFO   ] [Kivy        ] v2.0.0
[INFO   ] [Kivy        ] Installed at "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/__init__.py"
[INFO   ] [Python      ] v3.9.6 (default, Jun 30 2021, 10:22:16) 
[GCC 11.1.0]
[INFO   ] [Python      ] Interpreter at "/home/kearney/Documents/code/python/uc2-gui/env/bin/python"
[INFO   ] [Factory     ] 186 symbols loaded
[INFO   ] [ImageLoaderFFPy] Using ffpyplayer 4.3.2
[INFO   ] [Image       ] Providers: img_tex, img_dds, img_sdl2, img_pil, img_ffpyplayer 
[INFO   ] [Window      ] Provider: sdl2
[INFO   ] [GL          ] Using the "OpenGL" graphics system
[INFO   ] [GL          ] Backend used <sdl2>
[INFO   ] [GL          ] OpenGL version <b'4.6 (Compatibility Profile) Mesa 21.2.1'>
[INFO   ] [GL          ] OpenGL vendor <b'AMD'>
[INFO   ] [GL          ] OpenGL renderer <b'AMD RENOIR (DRM 3.41.0, 5.13.13-1-MANJARO, LLVM 12.0.1)'>
[INFO   ] [GL          ] OpenGL parsed version: 4, 6
[INFO   ] [GL          ] Shading version <b'4.60'>
[INFO   ] [GL          ] Texture max size <16384>
[INFO   ] [GL          ] Texture max units <32>
[INFO   ] [Window      ] auto add sdl2 input provider
[INFO   ] [Window      ] virtual keyboard not allowed, single mode, not docked
[CRITICAL] [Camera      ] Unable to find any valuable Camera provider. Please enable debug logging (e.g. add -d if running from the command line, or change the log level in the config) and re-run your app to identify potential causes
picamera - ModuleNotFoundError: No module named 'picamera'
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/__init__.py", line 58, in core_select_lib
    mod = __import__(name='{2}.{0}.{1}'.format(
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_picamera.py", line 18, in <module>
    from picamera import PiCamera

gi - ModuleNotFoundError: No module named 'gi'
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/__init__.py", line 58, in core_select_lib
    mod = __import__(name='{2}.{0}.{1}'.format(
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_gi.py", line 10, in <module>
    from gi.repository import Gst

opencv - ModuleNotFoundError: No module named 'cv2'
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/__init__.py", line 58, in core_select_lib
    mod = __import__(name='{2}.{0}.{1}'.format(
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_opencv.py", line 48, in <module>
    import cv2

 Traceback (most recent call last):
   File "/home/kearney/Documents/code/python/uc2-gui/cam.py", line 43, in <module>
     TestCamera().run()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/app.py", line 949, in run
     self._run_prepare()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/app.py", line 919, in _run_prepare
     root = self.build()
   File "/home/kearney/Documents/code/python/uc2-gui/cam.py", line 40, in build
     return CameraClick()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/boxlayout.py", line 145, in __init__
     super(BoxLayout, self).__init__(**kwargs)
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/layout.py", line 76, in __init__
     super(Layout, self).__init__(**kwargs)
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/widget.py", line 359, in __init__
     self.apply_class_lang_rules(
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/widget.py", line 463, in apply_class_lang_rules
     Builder.apply(
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/lang/builder.py", line 541, in apply
     self._apply_rule(
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/lang/builder.py", line 659, in _apply_rule
     child = cls(__no_builder=True)
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/camera.py", line 91, in __init__
     on_index()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/camera.py", line 101, in _on_index
     self._camera = CoreCamera(index=self.index, stopped=True)
 TypeError: 'NoneType' object is not callable
```
</details>

这就很离谱了，我现在用的又不是树莓派，干啥要调用 picamera。。。然后根据网站给出的
    > Note that not finding a camera, perhaps because gstreamer is not installed
然后查(sudo pacman -Q gstreamer)了一下我的系统里已经安装了 gstreamer 1.18.4-1

然后给句代码中的注释来看，去掉代码 13～15 行的注释，运行之后应该会有更详细的记录输出

<details>
<summary>点击查看更详细的报错信息</summary>

信息太长了，不太能懂这告诉了我啥。。
```bash
$ /home/kearney/Documents/code/python/uc2-gui/env/bin/python /home/kearney/Documents/code/python/uc2-gui/cam.py
[INFO   ] [Logger      ] Record log in /home/kearney/.kivy/logs/kivy_21-09-12_7.txt
[INFO   ] [Kivy        ] v2.0.0
[INFO   ] [Kivy        ] Installed at "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/__init__.py"
[INFO   ] [Python      ] v3.9.6 (default, Jun 30 2021, 10:22:16) 
[GCC 11.1.0]
[INFO   ] [Python      ] Interpreter at "/home/kearney/Documents/code/python/uc2-gui/env/bin/python"
[INFO   ] [Factory     ] 186 symbols loaded
[DEBUG  ] [Cache       ] register <kv.lang> with limit=None, timeout=None
[TRACE  ] [Lang        ] load file /home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/data/style.kv, using utf8 encoding
[TRACE  ] [Parser      ] parsing 1298 lines
[TRACE  ] [Parser      ] got directive <kivy 1.0>
[TRACE  ] [Builder     ] build rule for <Label>
[TRACE  ] [Builder     ] build rule for <-Button,-ToggleButton>
[TRACE  ] [Builder     ] build rule for <BubbleContent>
[TRACE  ] [Builder     ] build rule for <BubbleButton>
[TRACE  ] [Builder     ] build rule for <Slider>
[TRACE  ] [Builder     ] build rule for <ProgressBar>
[TRACE  ] [Builder     ] build rule for <SplitterStrip>
[TRACE  ] [Builder     ] build rule for <Scatter>
[TRACE  ] [Builder     ] build rule for <RelativeLayout>
[TRACE  ] [Builder     ] build rule for <Image,AsyncImage>
[TRACE  ] [Builder     ] build rule for <EffectWidget>
[TRACE  ] [Builder     ] build rule for <TabbedPanelContent>
[TRACE  ] [Builder     ] build rule for <TabbedPanelStrip>
[TRACE  ] [Builder     ] build rule for <StripLayout>
[TRACE  ] [Builder     ] build rule for <TabbedPanelHeader>
[TRACE  ] [Builder     ] build rule for <Selector>
[TRACE  ] [Builder     ] build rule for <TextInput>
[TRACE  ] [Builder     ] build rule for <TextInputCutCopyPaste>
[TRACE  ] [Builder     ] build rule for <CodeInput>
[TRACE  ] [Builder     ] build rule for <TreeViewNode>
[TRACE  ] [Builder     ] build rule for <TreeViewLabel>
[TRACE  ] [Builder     ] build rule for <StencilView>
[TRACE  ] [Builder     ] build rule for <FileChooserListLayout>
[TRACE  ] [Builder     ] build rule for <FileChooserListView>
[TRACE  ] [Builder     ] build template for [FileListEntry@FloatLayout+TreeViewNode]
[TRACE  ] [Builder     ] build rule for <FileChooserIconLayout>
[TRACE  ] [Builder     ] build rule for <FileChooserIconView>
[TRACE  ] [Builder     ] build template for [FileIconEntry@Widget]
[TRACE  ] [Builder     ] build rule for <FileChooserProgress>
[TRACE  ] [Builder     ] build rule for <Switch>
[TRACE  ] [Builder     ] build rule for <ModalView>
[TRACE  ] [Builder     ] build rule for <Popup>
[TRACE  ] [Builder     ] build rule for <SpinnerOption>
[TRACE  ] [Builder     ] build rule for <Spinner>
[TRACE  ] [Builder     ] build rule for <ActionBar>
[TRACE  ] [Builder     ] build rule for <ActionView>
[TRACE  ] [Builder     ] build rule for <ActionSeparator>
[TRACE  ] [Builder     ] build rule for <ActionButton,ActionToggleButton>
[TRACE  ] [Builder     ] build rule for <ActionLabel>
[TRACE  ] [Builder     ] build rule for <ActionGroup>
[TRACE  ] [Builder     ] build rule for <ActionCheck>
[TRACE  ] [Builder     ] build rule for <ActionPreviousImage@Image>
[TRACE  ] [Builder     ] build rule for <ActionPreviousButton@Button>
[TRACE  ] [Builder     ] build rule for <ActionPrevious>
[TRACE  ] [Builder     ] build rule for <ActionGroup>
[TRACE  ] [Builder     ] build rule for <ActionOverflow>
[TRACE  ] [Builder     ] build rule for <ActionDropDown>
[TRACE  ] [Builder     ] build template for [AccordionItemTitle@Label]
[TRACE  ] [Builder     ] build rule for <AccordionItem>
[TRACE  ] [Builder     ] build rule for <SettingSpacer>
[TRACE  ] [Builder     ] build rule for <SettingItem>
[TRACE  ] [Builder     ] build rule for <SettingBoolean>
[TRACE  ] [Builder     ] build rule for <SettingString>
[TRACE  ] [Builder     ] build rule for <SettingPath>
[TRACE  ] [Builder     ] build rule for <SettingOptions>
[TRACE  ] [Builder     ] build rule for <SettingTitle>
[TRACE  ] [Builder     ] build rule for <SettingSidebarLabel>
[TRACE  ] [Builder     ] build rule for <SettingsPanel>
[TRACE  ] [Builder     ] build rule for <Settings>
[TRACE  ] [Builder     ] build rule for <InterfaceWithSidebar>
[TRACE  ] [Builder     ] build rule for <InterfaceWithSpinner>
[TRACE  ] [Builder     ] build rule for <MenuSpinner>
[TRACE  ] [Builder     ] build rule for <MenuSidebar>
[TRACE  ] [Builder     ] build rule for <ContentPanel>
[TRACE  ] [Builder     ] build rule for <InterfaceWithTabbedPanel>
[TRACE  ] [Builder     ] build rule for <ScrollView>
[TRACE  ] [Builder     ] build rule for <VideoPlayerPreview>
[TRACE  ] [Builder     ] build rule for <VideoPlayerAnnotation>
[TRACE  ] [Builder     ] build rule for <VideoPlayer>
[TRACE  ] [Builder     ] build rule for <CheckBox>
[TRACE  ] [Builder     ] build rule for <ScreenManager>
[TRACE  ] [Builder     ] build rule for <ColorPicker_Input@TextInput>
[TRACE  ] [Builder     ] build rule for <ColorPicker_Label@Label>
[TRACE  ] [Builder     ] build rule for <ColorPicker_Selector@BoxLayout>
[TRACE  ] [Builder     ] build rule for <ColorWheel>
[TRACE  ] [Builder     ] build rule for <ColorPicker>
[DEBUG  ] [Cache       ] register <kv.image> with limit=None, timeout=60
[DEBUG  ] [Cache       ] register <kv.atlas> with limit=None, timeout=None
[INFO   ] [ImageLoaderFFPy] Using ffpyplayer 4.3.2
[INFO   ] [Image       ] Providers: img_tex, img_dds, img_sdl2, img_pil, img_ffpyplayer 
[DEBUG  ] [Cache       ] register <kv.texture> with limit=1000, timeout=60
[DEBUG  ] [Cache       ] register <kv.shader> with limit=1000, timeout=3600
[TRACE  ] [Parser      ] parsing 17 lines
[TRACE  ] [Builder     ] build rule for <CameraClick>
[DEBUG  ] [App         ] Loading kv </home/kearney/Documents/code/python/uc2-gui/testcamera.kv>
[DEBUG  ] [App         ] kv </home/kearney/Documents/code/python/uc2-gui/testcamera.kv> not found
[TRACE  ] [Parser      ] parsing 19 lines
[TRACE  ] [Builder     ] build rule for <-CoverBehavior>
[INFO   ] [Window      ] Provider: sdl2
[INFO   ] [GL          ] Using the "OpenGL" graphics system
[INFO   ] [GL          ] Backend used <sdl2>
[INFO   ] [GL          ] OpenGL version <b'4.6 (Compatibility Profile) Mesa 21.2.1'>
[INFO   ] [GL          ] OpenGL vendor <b'AMD'>
[INFO   ] [GL          ] OpenGL renderer <b'AMD RENOIR (DRM 3.41.0, 5.13.13-1-MANJARO, LLVM 12.0.1)'>
[INFO   ] [GL          ] OpenGL parsed version: 4, 6
[INFO   ] [GL          ] Shading version <b'4.60'>
[INFO   ] [GL          ] Texture max size <16384>
[INFO   ] [GL          ] Texture max units <32>
[TRACE  ] [Cache       ] Flushed category kv.texture from cache
[TRACE  ] [Cache       ] Flushed category kv.shader from cache
[DEBUG  ] [Shader      ] Fragment compiled successfully
[DEBUG  ] [Shader      ] Vertex compiled successfully
[DEBUG  ] [ImageSDL2   ] Load </home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/data/glsl/default.png>
[TRACE  ] [Image       ] '/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/data/glsl/default.png', populate to textures (1)
[INFO   ] [Window      ] auto add sdl2 input provider
[INFO   ] [Window      ] virtual keyboard not allowed, single mode, not docked
[TRACE  ] [Lang        ] Found 1 rules for <__main__.CameraClick object at 0x7f40097ee350>
[DEBUG  ] [Camera      ] Ignored <picamera> (import error)
[TRACE  ] 
Traceback (most recent call last):
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/__init__.py", line 58, in core_select_lib
    mod = __import__(name='{2}.{0}.{1}'.format(
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_picamera.py", line 18, in <module>
    from picamera import PiCamera
ModuleNotFoundError: No module named 'picamera'
[DEBUG  ] [Camera      ] Ignored <gi> (import error)
[TRACE  ] 
Traceback (most recent call last):
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/__init__.py", line 58, in core_select_lib
    mod = __import__(name='{2}.{0}.{1}'.format(
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_gi.py", line 10, in <module>
    from gi.repository import Gst
ModuleNotFoundError: No module named 'gi'
[DEBUG  ] [Camera      ] Ignored <opencv> (import error)
[TRACE  ] 
Traceback (most recent call last):
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_opencv.py", line 21, in <module>
    import opencv as cv
ModuleNotFoundError: No module named 'opencv'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/__init__.py", line 58, in core_select_lib
    mod = __import__(name='{2}.{0}.{1}'.format(
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_opencv.py", line 48, in <module>
    import cv2
ModuleNotFoundError: No module named 'cv2'
[CRITICAL] [Camera      ] Unable to find any valuable Camera provider. Please enable debug logging (e.g. add -d if running from the command line, or change the log level in the config) and re-run your app to identify potential causes
picamera - ModuleNotFoundError: No module named 'picamera'
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/__init__.py", line 58, in core_select_lib
    mod = __import__(name='{2}.{0}.{1}'.format(
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_picamera.py", line 18, in <module>
    from picamera import PiCamera

gi - ModuleNotFoundError: No module named 'gi'
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/__init__.py", line 58, in core_select_lib
    mod = __import__(name='{2}.{0}.{1}'.format(
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_gi.py", line 10, in <module>
    from gi.repository import Gst

opencv - ModuleNotFoundError: No module named 'cv2'
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/__init__.py", line 58, in core_select_lib
    mod = __import__(name='{2}.{0}.{1}'.format(
  File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_opencv.py", line 48, in <module>
    import cv2

 Traceback (most recent call last):
   File "/home/kearney/Documents/code/python/uc2-gui/cam.py", line 46, in <module>
     TestCamera().run()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/app.py", line 949, in run
     self._run_prepare()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/app.py", line 919, in _run_prepare
     root = self.build()
   File "/home/kearney/Documents/code/python/uc2-gui/cam.py", line 43, in build
     return CameraClick()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/boxlayout.py", line 145, in __init__
     super(BoxLayout, self).__init__(**kwargs)
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/layout.py", line 76, in __init__
     super(Layout, self).__init__(**kwargs)
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/widget.py", line 359, in __init__
     self.apply_class_lang_rules(
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/widget.py", line 463, in apply_class_lang_rules
     Builder.apply(
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/lang/builder.py", line 541, in apply
     self._apply_rule(
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/lang/builder.py", line 659, in _apply_rule
     child = cls(__no_builder=True)
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/camera.py", line 91, in __init__
     on_index()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/camera.py", line 101, in _on_index
     self._camera = CoreCamera(index=self.index, stopped=True)
 TypeError: 'NoneType' object is not callable
```
</details>

这就很离谱了，到其 Github repo 查了一下现在依旧有不少是无法使用摄像头的（如 ubuntu、win10、raspi），stackoverfolw 上的问题都是五年前的了，最近倒是有想自己把 opencv 的图结合到 kivy 中的。从其中一些交流来看，似乎需要安装 opencv-python 。。然后就一通操作给安上（pip install opencv-python）

这个时候先把代码 13～15 行注释掉，太长的信息反而导致混乱，此时的错误输出有所变化，不再是 picamera 的错误了

<details>
<summary>点击查看新的报错信息</summary>

```bash
$ /home/kearney/Documents/code/python/uc2-gui/env/bin/python /home/kearney/Documents/code/python/uc2-gui/cam.py
[INFO   ] [Logger      ] Record log in /home/kearney/.kivy/logs/kivy_21-09-12_8.txt
[INFO   ] [Kivy        ] v2.0.0
[INFO   ] [Kivy        ] Installed at "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/__init__.py"
[INFO   ] [Python      ] v3.9.6 (default, Jun 30 2021, 10:22:16) 
[GCC 11.1.0]
[INFO   ] [Python      ] Interpreter at "/home/kearney/Documents/code/python/uc2-gui/env/bin/python"
[INFO   ] [Factory     ] 186 symbols loaded
[INFO   ] [ImageLoaderFFPy] Using ffpyplayer 4.3.2
[INFO   ] [Image       ] Providers: img_tex, img_dds, img_sdl2, img_pil, img_ffpyplayer 
[INFO   ] [Window      ] Provider: sdl2
[INFO   ] [GL          ] Using the "OpenGL" graphics system
[INFO   ] [GL          ] Backend used <sdl2>
[INFO   ] [GL          ] OpenGL version <b'4.6 (Compatibility Profile) Mesa 21.2.1'>
[INFO   ] [GL          ] OpenGL vendor <b'AMD'>
[INFO   ] [GL          ] OpenGL renderer <b'AMD RENOIR (DRM 3.41.0, 5.13.13-1-MANJARO, LLVM 12.0.1)'>
[INFO   ] [GL          ] OpenGL parsed version: 4, 6
[INFO   ] [GL          ] Shading version <b'4.60'>
[INFO   ] [GL          ] Texture max size <16384>
[INFO   ] [GL          ] Texture max units <32>
[INFO   ] [Window      ] auto add sdl2 input provider
[INFO   ] [Window      ] virtual keyboard not allowed, single mode, not docked
[INFO   ] [Camera      ] Provider: opencv(['camera_picamera', 'camera_gi'] ignored)
[INFO   ] [Text        ] Provider: sdl2
[ WARN:0] global /tmp/pip-req-build-3umofm98/opencv/modules/videoio/src/cap_v4l.cpp (890) open VIDEOIO(V4L2:/dev/video0): can't open camera by index
 Traceback (most recent call last):
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/lang/builder.py", line 705, in _apply_rule
     setattr(widget_set, key, value)
   File "kivy/weakproxy.pyx", line 35, in kivy.weakproxy.WeakProxy.__setattr__
   File "kivy/properties.pyx", line 498, in kivy.properties.Property.__set__
   File "kivy/properties.pyx", line 840, in kivy.properties.ListProperty.set
   File "kivy/properties.pyx", line 545, in kivy.properties.Property.set
   File "kivy/properties.pyx", line 600, in kivy.properties.Property.dispatch
   File "kivy/_event.pyx", line 1248, in kivy._event.EventObservers.dispatch
   File "kivy/_event.pyx", line 1154, in kivy._event.EventObservers._dispatch
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/camera.py", line 103, in _on_index
     self._camera = CoreCamera(index=self.index,
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_opencv.py", line 70, in __init__
     super(CameraOpenCV, self).__init__(**kwargs)
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/__init__.py", line 70, in __init__
     self.init_camera()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_opencv.py", line 120, in init_camera
     self._resolution = (int(frame.shape[1]), int(frame.shape[0]))
 AttributeError: 'NoneType' object has no attribute 'shape'
 
 During handling of the above exception, another exception occurred:
 
 Traceback (most recent call last):
   File "/home/kearney/Documents/code/python/uc2-gui/cam.py", line 46, in <module>
     TestCamera().run()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/app.py", line 949, in run
     self._run_prepare()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/app.py", line 919, in _run_prepare
     root = self.build()
   File "/home/kearney/Documents/code/python/uc2-gui/cam.py", line 43, in build
     return CameraClick()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/boxlayout.py", line 145, in __init__
     super(BoxLayout, self).__init__(**kwargs)
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/layout.py", line 76, in __init__
     super(Layout, self).__init__(**kwargs)
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/widget.py", line 359, in __init__
     self.apply_class_lang_rules(
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/widget.py", line 463, in apply_class_lang_rules
     Builder.apply(
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/lang/builder.py", line 541, in apply
     self._apply_rule(
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/lang/builder.py", line 710, in _apply_rule
     raise BuilderException(rule.ctx, rule.line,
 kivy.lang.builder.BuilderException: Parser: File "<inline>", line 6:
 ...
       4:    Camera:
       5:        id: camera
 >>    6:        resolution: (640, 480)
       7:        play: False
       8:    ToggleButton:
 ...
 AttributeError: 'NoneType' object has no attribute 'shape'
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/lang/builder.py", line 705, in _apply_rule
     setattr(widget_set, key, value)
   File "kivy/weakproxy.pyx", line 35, in kivy.weakproxy.WeakProxy.__setattr__
   File "kivy/properties.pyx", line 498, in kivy.properties.Property.__set__
   File "kivy/properties.pyx", line 840, in kivy.properties.ListProperty.set
   File "kivy/properties.pyx", line 545, in kivy.properties.Property.set
   File "kivy/properties.pyx", line 600, in kivy.properties.Property.dispatch
   File "kivy/_event.pyx", line 1248, in kivy._event.EventObservers.dispatch
   File "kivy/_event.pyx", line 1154, in kivy._event.EventObservers._dispatch
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/uix/camera.py", line 103, in _on_index
     self._camera = CoreCamera(index=self.index,
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_opencv.py", line 70, in __init__
     super(CameraOpenCV, self).__init__(**kwargs)
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/__init__.py", line 70, in __init__
     self.init_camera()
   File "/home/kearney/Documents/code/python/uc2-gui/env/lib/python3.9/site-packages/kivy/core/camera/camera_opencv.py", line 120, in init_camera
     self._resolution = (int(frame.shape[1]), int(frame.shape[0]))
 ```
</details>

错误信息指出了 resolution: (640, 480) 这一行，之前调试 uc2 的经验告诉我，把这一行注销掉或者删掉会有好结果，删掉之后就能正常跑起来啦。

# 总结
## 解决办法

- 安装 openv-python
- 删除关于分辨率的代码

## 简洁代码

案例代码是 python 中嵌套 kv，实在是不太喜欢，改一下吧。

```python
from kivy.app import App
from kivy.uix.button import Button
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.camera import Camera
import time


class CameraGUI(BoxLayout):
    def __init__(self, **kwargs):
        super(CameraGUI, self).__init__(**kwargs)
        self.camera = Camera(play=False)
        self.add_widget(self.camera)
        self.orientation = "vertical"

        self.btnplay = Button(text="Play")
        self.btnplay.bind(on_release=self.Play)
        self.add_widget(self.btnplay)

        self.btncap = Button(text="Capture")
        self.btncap.bind(on_release=self.Capture)
        self.add_widget(self.btncap)

    def Capture(self, instance):
        """
        Function to capture the images and give them the names
        according to their captured time and date.
        """
        timestr = time.strftime("%Y%m%d_%H%M%S")
        self.camera.export_to_png("IMG_{}.png".format(timestr))
        print("The button <%s> is being pressed" % instance.text)

    def Play(self, instance):
        """
        start or stop the camera
        """
        print("The button <%s> is being pressed" % instance.text)
        self.camera.play = not self.camera.play


class Test(App):
    def build(self):
        return CameraGUI()


if __name__ == "__main__":
    Test().run()
```

# 参考

- [Python Kivy（App开发）调用摄像头的样例。海底捞淡水鱼 2020-08-05 ](https://blog.csdn.net/qq_37030400/article/details/107826596)：kivy 1.11 跑同样的案例代码，报错依旧
- [kivy doc examples](https://kivy.org/doc/stable/examples/)
- []()