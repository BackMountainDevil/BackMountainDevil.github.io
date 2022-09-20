# docker 的入门笔记
- date: 2022-04-13
- lastmod: 2022-09-19

# 前言

1. 虚拟机和容器的区别？

简单的说容器技术比虚拟机技术更快、占用更小、更方便移植维护部署。用 docker 也是可以运行虚拟机来玩的，比如用 docker 跑 Mac。

2. 镜像和容器的区别？

> 镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的 类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。  
> 容器的实质是进程  
> https://yeasy.gitbook.io/docker_practice/basic_concept/container

3. 数据存放在哪里？

镜像默认存放在 /var/lib/docker，但是查看文件就知道不是按照镜像作为文件夹存放的，才是是因为 docker 采取文件分层存储。

4. 删除镜像还是删除容器？

类 和 实例，一般删除容器即可，如果这个镜像不再需要了那就删除呗。

# 安装与基础配置

```bash
sudo pacman install docker  # 
sudo usermod -a -G docker $USER # 将当前用户加入 docker 用户组，此操作的风险 wiki 中已经明确标红指示出来了。然后注销重新登录，但是我测试得重启才行
systemctl enable docker # 设置开启自启动 docker 服务，禁止就 enable 换成 disable
```

如果不想开启启动 docker 服务，比如我一般不这么设置，需要时再用下面这句 `systemctl start docker` 启动 docker 服务

## web 管理 portainer

docker 有很多第三方 web 管理界面，这里选择的是 portainer。安装的时候我首先到 pamac 查找了一下发现都是 AUR 的，想着哪里不太对劲就stw发现有人是通过 docker 去安装的。

```bash
docker pull portainer/portainer
docker run -d -p 9000:9000 -v "/var/run/docker.sock:/var/run/docker.sock" --restart=always --name portainer portainer/portainer
```

浏览器打开 http://localhost:9000 后创建管理员账号密码 然后选择 Docker(默认选择是k8s)，这样就可以通过 web 管理 docker 了

- [Docker内运行ROS(melodic版本)以及使用Rviz 冷色调的夏天 2022-02-07](https://blog.csdn.net/qq_40695642/article/details/117607446):通过docker 安装 portainer 测试成功
- [Docker可视化工具——Portainer全解 网久软件 2021-08-25](https://zhuanlan.zhihu.com/p/403285855)：安装指令稍微不一样，但都是通过docker来安装

# 常见操作

不清楚的话就先加上 --help 查看参数含义

 指令                                                   | 含义                 
:----------------------------------------------------|:------------------
 docker image ls                                      | 列出镜像
 docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签] | 获取镜像
 docker image rm [选项] <镜像1> [<镜像2> ...]               | 删除本地镜像
 docker run [OPTIONS] IMAGE [COMMAND] [ARG...]        | 新建容器并启动
 docker container start <容器名称>                     | 启动已终止容器
 docker container stop  <容器名称>                     | 终止一个运行中的容器
 docker container rm  <容器名称>                       | 删除容器
 docker container ls -a                               | 查看所有已经创建的包括终止状态的容器
 docker container prune                               | 清理所有处于终止状态的容器     

## run 参数

docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

-d 让 Docker 在后台运行而不是直接把执行命令的结果输出在当前宿主机下，输出结果可以用 docker logs 查看。使用 -d 参数启动后会返回一个唯一的 id，也可以通过 docker container ls 命令来查看容器信息，要获取容器的输出信息，可以通过 docker container logs [cCONTAINER ID or NAME] 命令

-t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上， -i 则让容器的标准输入保持打开，退出操作为 exit 指令或者按键组合 Ctrl+D 

-v 将文件/目录映射（挂载）到容器中，类似与共享文件。如  -v ${PWD}:/root/code 就是把用户当前所在目录（$PWD）挂载到容器的 /root/code 目录下，对 /root/code 的操作会同步修改到当前用户目录下的文件

--rm 指定容器停止后自动删除容器(不支持以docker run -d启动的容器)

--restart=always 设置在docker启动时，自动启动该容器，比如宕机重启docker,让容器也自动启动

# 示例

```bash
$ docker run -it --rm archlinux bash -c "echo hello world"  # 创建 archlinux 镜像并启动容器运行 bash，结果是会输出 hello world

$ docker run -it archlinux bash # 启动一个 bash 终端，允许用户进行交互

$ docker image ls
REPOSITORY                  TAG       IMAGE ID       CREATED        SIZE
archlinux                   latest    503d83557caa   4 days ago     386MB
iyuucn/iyuuplus             latest    6e59662bbf01   6 months ago   57.6MB
dinghao188/rcore-tutorial   latest    e342c22ef6be   7 months ago   1.17GB

$ docker container ls -a 
CONTAINER ID   IMAGE                    COMMAND            CREATED          STATUS                           PORTS     NAMES
cde8dfbea7c1   archlinux                "bash"             44 minutes ago   Exited (1) 44 minutes ago                  zealous_matsumoto
7c5bbc99b8d2   archlinux                "bash"             44 minutes ago   Exited (0) 44 minutes ago                  recursing_kirch
66432e3b1b49   iyuucn/iyuuplus:latest   "/entrypoint.sh"   2 hours ago      Exited (137) About an hour ago             IYUUPlus

$ docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
cde8dfbea7c160fbb5f2879a42c5d4f9b1e293210fbb001da5e364a35925d9ec
7c5bbc99b8d221d93c4d375e01885c4f691be07a2ea531bd0547f35acc785d0a
66432e3b1b497d542dd2de309425f7eebde9e548b4af7fac306dfe97b35c4b89

Total reclaimed space: 156B

$ docker container ls -a 
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

$ docker image ls
REPOSITORY                  TAG       IMAGE ID       CREATED        SIZE
archlinux                   latest    503d83557caa   4 days ago     386MB
iyuucn/iyuuplus             latest    6e59662bbf01   6 months ago   57.6MB
dinghao188/rcore-tutorial   latest    e342c22ef6be   7 months ago   1.17GB
```

# Q&A

1. sock: connect: permission denied.

用户不在 docker 用户组中或者未使用 sudo，我已经把自己加入到 docker 组中了，已经注销又重新登录然后重启 docker 服务，还是会这样，重试两次之后我就重启了，重启大法解决问题了
```bash
$ docker run -it --rm archlinux bash -c "echo hello world"
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
$ groups $USER
wheel lp sys network power docker kearney
```

# 参考

- [docker](https://www.docker.com/)
- [Docker -- 从入门到实践](https://yeasy.gitbook.io/docker_practice/)
- [Docker - Arch Wiki](https://wiki.archlinux.org/title/Docker)
- [docker run 参数详解. 雪东~. 2019-08-16](https://blog.csdn.net/weixin_39998006/article/details/99680522)