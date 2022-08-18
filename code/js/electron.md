# electron 快速启动
- date: 2022-08-18
- lastmod: 2022-08-18

## 依赖准备
[nodejs](https://nodejs.org/en/)是基于 Chrome 的 V8 JavaScript 引擎构建的 JavaScript 运行时，说白了就是给运行 js 用的工具，类比 gcc 运行 C/C++。而npm 是包管理工具，类似 apt、yum、pip。建议安装 lts 长期支持版本。

```bash
$ node -v
v14.17.4  # manjaro 的 lts 和官网 14.17.6 不一致。。
$ npm -v
7.23.0  # Latest LTS Version: 14.17.6 (includes npm 6.14.15)..
# Manjaro 打包感人，下 lts 的 node 居然不搭配 lts 标配版本的 npm
# 打开 pamac 搜索一下 npm,官方仓库里有 npm6 的lts,下载即可
$ npm -v
6.14.15
```

镜像

```bash
$ npm config get registry # 查看软件源配置，默认源网速“似乎“感人
https://registry.npmjs.org/
# 镜像源 tx、tb自行选择
$ npm config set registry http://mirrors.cloud.tencent.com/npm/ # 设置 tx 镜像源
$ npm config set registry https://registry.npm.taobao.org # 设置 tb 镜像源
$ npm config set registry https://registry.npmjs.org/ # 重置为默认源，自行取舍
```

## electron

从包管理器中可以直接安装，然后用它来启动一些 electron 应用

```bash
git clone https://github.com/electron/electron-quick-start
cd electron-quick-start
electron ./
```

### VS C

配置一下 F5 一键运行项目

```json
// /.vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
      {
          "type": "node",
          "request": "launch",
          "name": "Run Electron",
          "runtimeExecutable": "electron",
          "program": "${workspaceFolder}/main.js",
          "protocol": "inspector" //添加默认的协议是legacy，这个协议导致不进入断点
      }
  ]
}
```
