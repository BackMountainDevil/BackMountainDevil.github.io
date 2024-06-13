# Linux tar 打包 gz bz xz zip 压缩
- date: 2024-06-13

打包的含义是将多个文件变成一个文件（这个文件称为归档文件），可以用 tar 来做这个事情，但是其不会压缩，可以结合使用其它压缩工具，如 gzip、bzip2 或 xz，来创建既打包又压缩的归档文件，这就是经常看到的压缩包了。

`scp -Cpr` 传输大量小文件时，发现比较慢，gz 压缩后传输快些，当然有些时间花在压缩上了，感觉先压缩打包再传输比传输时压缩要快些、可靠些

# 打包（不压缩）

下面的命令含义是将 test 文件夹打包，并将打包后的文件命名为 test.tar

```bash
tar -cvf test.tar test
```

参数说明：c代表创建（create），v代表详细输出（verbose），f 指定归档文件名，这里面 cf 是必须要写的，v 可选。该命令会覆盖操作，并且不会进行询问

解包则是

```bash
tar -xf test.tar
```

参数说明：x 表示提取文件

## 例子

```bash
$ tree test/   # 查看文件夹结构
test/
├── log
│   └── test.txt
├── main.py
├── README.md
├── src
│   └── test.py
└── temp
    └── temp.pyc

$ tar -cvf test.tar test   # 打包
test/
test/temp/
test/temp/temp.pyc
test/log/
test/log/test.txt
test/src/
test/src/test.py
test/main.py
test/README.md

$ ls -l test.tar
-rw-r--r-- 1 mifen mifen 10240  6月12日 16:40 test.tar
$ rm test.tar

$ tar -cf test.tar test   # 打包，不输出详细信息，可以看到不输出所有文件路径了
$ ls -l test.tar
-rw-r--r-- 1 mifen mifen 10240  6月12日 16:41 test.tar
```

# 打包压缩

下面的命令含义是将 test 文件夹打包，并将打包后的文件重命名

```bash
tar -czf test.tar.gz test
tar -cjf test.tar.bz2 test
tar -cJf test.tar.xz test
```

参数说明：z、j、J 分别代表 gzip、bzip2、xz 压缩算法

1. **gzip**：
   - 压缩和解压速度相对较快
   - 压缩率适中
   - 文件扩展名通常为`.gz`
2. **bzip2**：
   - 压缩率比gzip高，但压缩和解压速度较慢
   - 适合对压缩率有较高要求，而对处理速度要求不高的场景
   - 文件扩展名通常为`.bz2`
3. **xz**：
   - 提供了非常高的压缩率，但压缩和解压速度最慢
   - 适合在存储空间非常有限，且对压缩率有极高要求的场景
   - 文件扩展名通常为`.xz`

解压缩这三种压缩包，只需要将上面指令中的参数 c 替换为 x，然后把要打包的文件夹这个参数删除即可，如 `tar -xzf test.tar.gz`

还有一个不是和 tar 搭配的，而在 windows 上很常见的 zip： `zip -r test.zip test` ，参数 r 表示递归，其对应的解压命令是 `unzip test.zip`，默认解压到当前目录，指定目录可以用参数 d

## 例子

上面 test 文件夹没啥内容，用来做压缩的例子体现不出这个压缩效果的差异，不过和 zip 相比确实领先些。于是用一个包含 7w 文件、38个文件夹、大小为 32G 的文件夹做了压缩，压缩体积由大到小分别是 gz bz2 xz

```bash
$ zip -r test.zip test
  adding: test/ (stored 0%)
  adding: test/temp/ (stored 0%)
  adding: test/temp/temp.pyc (stored 0%)
  adding: test/log/ (stored 0%)
  adding: test/log/test.txt (stored 0%)
  adding: test/src/ (stored 0%)
  adding: test/src/test.py (stored 0%)
  adding: test/main.py (stored 0%)
  adding: test/README.md (stored 0%)
$ ls -l test.zip
-rw-r--r-- 1 mifen mifen 1394  6月12日 17:03 test.zip

$ tar -czf test.tar.gz test
$ tar -cjf test.tar.bz2 test
$ tar -cJf test.tar.xz test
$ ls -l test.tar.*
-rw-r--r-- 1 mifen mifen 284  6月12日 16:49 test.tar.bz2
-rw-r--r-- 1 mifen mifen 283  6月12日 16:49 test.tar.gz
-rw-r--r-- 1 mifen mifen 300  6月12日 16:49 test.tar.xz

$ du -sh col_1k_6 # 查看文件夹大小
32G     col_1k_6
$ tree col_1k_6 # 查看文件夹目录结构
...
38 directories, 78569 files
$ tar -czf col_1k_6.tar.gz col_1k_6
$ tar -cjf col_1k_6.tar.bz2 col_1k_6
$ tar -cJf col_1k_6.tar.xz col_1k_6
$ ls -lh col_1k_6.tar*
-rw-rw-r-- 1 mifen mifen 5.2G 6月  12 18:54 col_1k_6.tar.bz2
-rw-rw-r-- 1 mifen mifen 5.9G 6月  12 18:31 col_1k_6.tar.gz
-rw-rw-r-- 1 mifen mifen 1.6G 6月  12 21:21 col_1k_6.tar.xz
```

# refer

[Archiving and compression](https://wiki.archlinux.org/title/Archiving_and_compression)
