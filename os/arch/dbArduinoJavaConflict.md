# Dbeaver 启动提示 Java 版本过低，和 arduino 的 java 依赖发生冲突，java 版本管理
- date: 2022-11-10
- lastmod: 2022-11-10

DBeaver ce  22.2.3-1 要求 jre>=11，安装时默认安装了 jre17-openjdk 17.0.5-u1-1。但是启动时却提示 `Version 1.8.0_352 of the JVM is not suituable for this product. Version 11 or greater is required.`，使用 pamac 再检查 dbeaver 的依赖 jre17 已经安装了，结果 `java -version` 的运行结果返回的是 `openjdk version "1.8.0_352"`，然后使用 pamac 搜索 java、jdk、jre 发现是 arduino 依赖于 jre8-openjdk(8.352.u08-1)。

因此得到了上述问题的原因：首先安装了 arduino 使得 java 默认为 1.8，后面再安装 dbeaver 时安装的 jre17 虽然也安装成功了，但是没有被设置成最优先级别的 java，在论坛里搜索没有啥相似的问题，要么是这个问题不是很普遍，要么就是这个问题简单到不需要提问。最后我把目光投向了 [Arch wiki](https://wiki.archlinux.org/title/Java#Switching_between_JVM)

```bash
$ archlinux-java status # 查看有啥
Available Java environments:
  java-17-openjdk
  java-8-openjdk/jre (default)

$ sudo archlinux-java set java-17-openjdk   # 设置默认 java为 17

$ archlinux-java status
Available Java environments:
  java-17-openjdk (default)
  java-8-openjdk/jre
```

此后 Dbeaver 可以成功启动运行，arduino 也能启动，功能上不知道有啥bug，需要用 arduino 的时候发现 bug 了再切换回来，或者用 arduino 之前切换回 j8（主要是包依赖关系里 arduino 依赖的 jre==8，而不是 jre>=8）