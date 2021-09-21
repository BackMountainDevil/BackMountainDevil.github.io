# 树莓派 4B 安装 opencv-python 的正解
- date: 2021-09-21
- lastmod: 2021-07-21

在自己的笔记本上写了个程序，需要借助 opcv-python（以下简称opencv） 来调用摄像头（别的轮子暂时没想到），ras pi 上有 picamera,可是那玩意在我电脑上没法用，[NanoCamera](https://github.com/thehapyone/NanoCamera)(看了源码基于 opencv) 还没有测试过，不知道效果咋样，最后程序要在树莓派 4B 上跑，加班加点熬出了程序，最后过去移植的时候，整了两个多小时的 opencv，硬是没装上。

## 悲惨世界

排除网速问题，用了 TUNA 镜像，最后愣是没装上，晚上干脆一不做二不休，直接重装系统，之前[装的](./raspberrypi.md)都是 armhf,这回试一下 arm64,一样整 TUNA 镜像，于是熟悉的“红色警戒“发生了。

<details>
<summary>pip install opencv-python 过程记录</summary>

```bash
# 版本核查
pi@raspberrypi:~ $ python -V
Python 2.7.16
pi@raspberrypi:~ $ python3 -V
Python 3.7.3
pi@raspberrypi:~ $ pip -V
pip 18.1 from /usr/lib/python2.7/dist-packages/pip (python 2.7)
pi@raspberrypi:~ $ pip3 -V
pip 18.1 from /usr/lib/python3/dist-packages/pip (python 3.7)


pi@raspberrypi:~ $ uname -a
Linux raspberrypi 5.10.17-v8+ #1414 SMP PREEMPT Fri Apr 30 13:23:25 BST 2021 aarch64 GNU/Linux


pi@raspberrypi:~/Desktop $ mkdir uc2-tkinter
pi@raspberrypi:~/Desktop $ cd uc2-tkinter/
# 虚拟环境
pi@raspberrypi:~/Desktop/uc2-tkinter $ python3 -m venv env
pi@raspberrypi:~/Desktop/uc2-tkinter $ source env/bin/activate
(env) pi@raspberrypi:~/Desktop/uc2-tkinter $ pip list
Package       Version
------------- -------
pip           18.1   
pkg-resources 0.0.0  
setuptools    40.8.0 
(env) pi@raspberrypi:~/Desktop/uc2-tkinter $ pip -V
pip 18.1 from /home/pi/Desktop/uc2-tkinter/env/lib/python3.7/site-packages/pip (python 3.7)

(env) pi@raspberrypi:~/Desktop/uc2-tkinter $ pip install opencv-python
Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple, https://www.piwheels.org/simple
Collecting opencv-python
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/01/9b/be08992293fb21faf35ab98e06924d7407fcfca89d89c5de65442631556a/opencv-python-4.5.3.56.tar.gz (89.2MB)
    100% |████████████████████████████████| 89.2MB 6.1kB/s 
  Installing build dependencies ... error
  Complete output from command /home/pi/Desktop/uc2-tkinter/env/bin/python3 -m pip install --ignore-installed --no-user --prefix /tmp/pip-build-env-61h13h0q --no-warn-script-location --no-binary :none: --only-binary :none: -i https://pypi.tuna.tsinghua.edu.cn/simple --extra-index-url https://www.piwheels.org/simple --trusted-host https://pypi.tuna.tsinghua.edu.cn -- setuptools wheel scikit-build cmake pip "numpy==1.13.3; python_version=='3.6' and platform_machine != 'aarch64' and platform_machine != 'arm64'" "numpy==1.19.3; python_version>='3.6' and sys_platform == 'linux' and platform_machine == 'aarch64'" "numpy==1.21.0; python_version>='3.6' and sys_platform == 'darwin' and platform_machine == 'arm64'" "numpy==1.14.5; python_version=='3.7' and platform_machine != 'aarch64' and platform_machine != 'arm64'" "numpy==1.17.3; python_version=='3.8' and platform_machine != 'aarch64' and platform_machine != 'arm64'" "numpy==1.19.3; python_version>='3.9' and platform_machine != 'aarch64' and platform_machine != 'arm64'":
  Ignoring numpy: markers 'python_version == "3.6" and platform_machine != "aarch64" and platform_machine != "arm64"' don't match your environment
  Ignoring numpy: markers 'python_version >= "3.6" and sys_platform == "darwin" and platform_machine == "arm64"' don't match your environment
  Ignoring numpy: markers 'python_version == "3.7" and platform_machine != "aarch64" and platform_machine != "arm64"' don't match your environment
  Ignoring numpy: markers 'python_version == "3.8" and platform_machine != "aarch64" and platform_machine != "arm64"' don't match your environment
  Ignoring numpy: markers 'python_version >= "3.9" and platform_machine != "aarch64" and platform_machine != "arm64"' don't match your environment
  Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple, https://www.piwheels.org/simple, https://www.piwheels.org/simple
  Collecting setuptools
    Downloading https://pypi.tuna.tsinghua.edu.cn/packages/c4/c1/aed7dfedb18ea73d7713bf6ca034ab001a6425be49ffa7e79bbd5999f677/setuptools-58.0.4-py3-none-any.whl (816kB)
  Collecting wheel
    Downloading https://pypi.tuna.tsinghua.edu.cn/packages/04/80/cad93b40262f5d09f6de82adbee452fd43cdff60830b56a74c5930f7e277/wheel-0.37.0-py2.py3-none-any.whl
  Collecting scikit-build
    Downloading https://pypi.tuna.tsinghua.edu.cn/packages/04/19/f694dbab665bc2aacaf614452b1577d740e5ce8518d1b10fced2522759bf/scikit_build-0.12.0-py2.py3-none-any.whl (73kB)
  Collecting cmake
    Downloading https://pypi.tuna.tsinghua.edu.cn/packages/0f/8f/57a0ee34cbfc2dad9b5cc7a2b2f9f7b42bfce9a8521cc9ae67e42d444296/cmake-3.21.2.tar.gz
    Installing build dependencies: started
    Installing build dependencies: finished with status 'done'
  Collecting pip
    Downloading https://pypi.tuna.tsinghua.edu.cn/packages/ca/31/b88ef447d595963c01060998cb329251648acf4a067721b0452c45527eb8/pip-21.2.4-py3-none-any.whl (1.6MB)
  Collecting numpy==1.19.3
    Downloading https://pypi.tuna.tsinghua.edu.cn/packages/cb/c0/7b3d69e6ee68bc54c97ba51f8c3c3e43ff1dbc7bd97347cc19a1f944e60a/numpy-1.19.3.zip (7.3MB)
    Installing build dependencies: started
    Installing build dependencies: finished with status 'done'
  Collecting distro (from scikit-build)
    Using cached https://pypi.tuna.tsinghua.edu.cn/packages/b3/8d/a0a5c389d76f90c766e956515d34c3408a1e18f60fbaa08221d1f6b87490/distro-1.6.0-py2.py3-none-any.whl
  Collecting packaging (from scikit-build)
    Using cached https://pypi.tuna.tsinghua.edu.cn/packages/3c/77/e2362b676dc5008d81be423070dd9577fa03be5da2ba1105811900fda546/packaging-21.0-py3-none-any.whl
  Collecting pyparsing>=2.0.2 (from packaging->scikit-build)
    Using cached https://pypi.tuna.tsinghua.edu.cn/packages/8a/bb/488841f56197b13700afd5658fc279a2025a39e22449b7cf29864669b15d/pyparsing-2.4.7-py2.py3-none-any.whl
  Building wheels for collected packages: cmake, numpy
    Running setup.py bdist_wheel for cmake: started
    Running setup.py bdist_wheel for cmake: finished with status 'error'
    Complete output from command /home/pi/Desktop/uc2-tkinter/env/bin/python3 -u -c "import setuptools, tokenize;__file__='/tmp/pip-install-9o10lm8p/cmake/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" bdist_wheel -d /tmp/pip-wheel-usevob4s --python-tag cp37:
    Traceback (most recent call last):
      File "/tmp/pip-build-env-8rlvivs8/lib/python3.7/site-packages/skbuild/setuptools_wrap.py", line 564, in setup
        cmkr = cmaker.CMaker(cmake_executable)
      File "/tmp/pip-build-env-8rlvivs8/lib/python3.7/site-packages/skbuild/cmaker.py", line 95, in __init__
        self.cmake_version = get_cmake_version(self.cmake_executable)
      File "/tmp/pip-build-env-8rlvivs8/lib/python3.7/site-packages/skbuild/cmaker.py", line 82, in get_cmake_version
        "Problem with the CMake installation, aborting build. CMake executable is %s" % cmake_executable)
  
    Problem with the CMake installation, aborting build. CMake executable is cmake
  
    ----------------------------------------
    Failed building wheel for cmake
    Running setup.py clean for cmake
    Running setup.py bdist_wheel for numpy: started
    Running setup.py bdist_wheel for numpy: still running...
    Running setup.py bdist_wheel for numpy: still running...
    Running setup.py bdist_wheel for numpy: still running...
    Running setup.py bdist_wheel for numpy: still running...
    Running setup.py bdist_wheel for numpy: still running...
    Running setup.py bdist_wheel for numpy: still running...
    Running setup.py bdist_wheel for numpy: still running...
    Running setup.py bdist_wheel for numpy: finished with status 'done'
    Stored in directory: /home/pi/.cache/pip/wheels/44/11/95/6da6b3cee2d59e4dbc398180696257459e3b8ad5114d48b1f6
  Successfully built numpy
  Failed to build cmake
  Installing collected packages: setuptools, wheel, distro, pyparsing, packaging, scikit-build, cmake, pip, numpy
    Running setup.py install for cmake: started
      Running setup.py install for cmake: finished with status 'error'
      Complete output from command /home/pi/Desktop/uc2-tkinter/env/bin/python3 -u -c "import setuptools, tokenize;__file__='/tmp/pip-install-9o10lm8p/cmake/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-record-rhcvpxf7/install-record.txt --single-version-externally-managed --prefix /tmp/pip-build-env-61h13h0q --compile --install-headers /home/pi/Desktop/uc2-tkinter/env/include/site/python3.7/cmake:
      Traceback (most recent call last):
        File "/tmp/pip-build-env-8rlvivs8/lib/python3.7/site-packages/skbuild/setuptools_wrap.py", line 564, in setup
          cmkr = cmaker.CMaker(cmake_executable)
        File "/tmp/pip-build-env-8rlvivs8/lib/python3.7/site-packages/skbuild/cmaker.py", line 95, in __init__
          self.cmake_version = get_cmake_version(self.cmake_executable)
        File "/tmp/pip-build-env-8rlvivs8/lib/python3.7/site-packages/skbuild/cmaker.py", line 82, in get_cmake_version
          "Problem with the CMake installation, aborting build. CMake executable is %s" % cmake_executable)
  
      Problem with the CMake installation, aborting build. CMake executable is cmake
  
      ----------------------------------------
  Command "/home/pi/Desktop/uc2-tkinter/env/bin/python3 -u -c "import setuptools, tokenize;__file__='/tmp/pip-install-9o10lm8p/cmake/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-record-rhcvpxf7/install-record.txt --single-version-externally-managed --prefix /tmp/pip-build-env-61h13h0q --compile --install-headers /home/pi/Desktop/uc2-tkinter/env/include/site/python3.7/cmake" failed with error code 1 in /tmp/pip-install-9o10lm8p/cmake/
  
  ----------------------------------------
Command "/home/pi/Desktop/uc2-tkinter/env/bin/python3 -m pip install --ignore-installed --no-user --prefix /tmp/pip-build-env-61h13h0q --no-warn-script-location --no-binary :none: --only-binary :none: -i https://pypi.tuna.tsinghua.edu.cn/simple --extra-index-url https://www.piwheels.org/simple --trusted-host https://pypi.tuna.tsinghua.edu.cn -- setuptools wheel scikit-build cmake pip "numpy==1.13.3; python_version=='3.6' and platform_machine != 'aarch64' and platform_machine != 'arm64'" "numpy==1.19.3; python_version>='3.6' and sys_platform == 'linux' and platform_machine == 'aarch64'" "numpy==1.21.0; python_version>='3.6' and sys_platform == 'darwin' and platform_machine == 'arm64'" "numpy==1.14.5; python_version=='3.7' and platform_machine != 'aarch64' and platform_machine != 'arm64'" "numpy==1.17.3; python_version=='3.8' and platform_machine != 'aarch64' and platform_machine != 'arm64'" "numpy==1.19.3; python_version>='3.9' and platform_machine != 'aarch64' and platform_machine != 'arm64'"" failed with error code 1 in None
```
</details>

面向互联网学习的经验告诉我，搜索一下，什么同济啊、hu啊、cn啊，说啥 pip 镜像加速、自行编译源码啥的，一同操作下来，真想打电话过去问一下，你当时真的安装成功了吗？

## 最终办法

[How to Install OpenCV on Raspberry Pi 3.  Jul 5, 2019](https://linuxize.com/post/how-to-install-opencv-on-raspberry-pi/#:~:text=The%20OpenCV%20Python%20module%20is%20available%20from%20the,commands%3A%20sudo%20apt%20update%20sudo%20apt%20install%20python3-opencv)中给出了一种办法和测试办法

```bash
# 给 python3 安装 opencv-python，py2 则是 python-opencv
sudo apt install python3-opencv 
# 测试
python3 -c "import cv2; print(cv2.__version__)"
```

正常来说上面的结果会输出版本号，但是我怎么会不知道 apt 呢？其实我已经试验过 apt 了，还安装上了，结果我们都蒙蔽了，安装了却没法使用（ModuleNotFoundError: No module named 'cv2'），最后是怎么发现这个 bug 的呢？想起之前整 flask 的时候用过[虚拟环境](/code/python/pyvenv.md)，出现过我更新的数据没有及时的反馈到程序上，需要退出再进入虚拟环境来使得最新数据同步。

于是退出了(deactivate)刚生成的虚拟环境，试一下测试发现输出了版本好，激活虚拟环境之后反而不行了，我人傻了，最后只好临时不使用虚拟环境先拿去做实验了。

Python 的 venv 模块为什么把系统的 opencv 隔离开了呢？

刚开始我都觉得这是 venv 的一个 bug，准备打开 issue 进行反馈，忽然想到这不就是虚拟环境要干的事情吗，把不同的环境隔离开来。但是这就导致了虚拟环境中无法使用通过系统包管理器进行安装的包，而 pip 大法不给力。

如何将这个包引入到虚拟环境呢？
