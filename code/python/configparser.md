# Python用configparser对配置文件进行读写与大小写问题
- date: 2020-10-20
- lastmod: 2021-08-24

# 简介
想写一个登录注册的demo，但是以前的demo数据都写在程序里面，每一关掉程序新数据就没保存住。当然也可以写到文件里，然后再自己写一个读取、存储的办法，但是先看看有没有好轮子，查了一下真的有 - configparser

>Note The ConfigParser module has been renamed to configparser in Python 3. The 2to3 tool will automatically adapt imports when converting your sources to Python 3.

需要注意的是在 py2 中，该模块叫 ConfigParser，在 py3 中把字母全变成了小写。本文以 py3 为例

# configparser

## 数据格式

下面的 config.ini 展示了配置文件的数据格式，用中括号[]括起来的为一个 section 例如 Default、Color；每一个 section 有多个 option，例如 serveraliveinterval、compression 等。option就是我们用来保存自己数据的地方，类似于键值对 optionname = value 或者是 optionname : value	(也可以额外设置允许空值)

```ini
[Default]
serveraliveinterval = 45
compression = yes
compressionlevel = 9
forwardx11 = yes
values like this: 1000000
or this: 3.14159265359
[No Values]
key_without_value
empty string value here =
# 自定义颜色
[Color]
isset = true
version = 1.1.0
orange = 150,100,100
lightgreen = 0,220,0
```

## 数据类型

在configparser 保存的数据中，value 的值都保存为字符串类型，需要自己转换为自己需要的数据类型
> Config parsers do not guess datatypes of values in configuration files, always storing them internally as strings. This means that if you need other datatypes, you should convert on your own:

例如

```bash
>>> int(topsecret['Port'])
50022
>>> float(topsecret['CompressionLevel'])
9.0
```

## 属性和方法

从 init 中可以知道默认不允许值为空、键值对之间用 = 或 : 来表示等值、注释前缀是井号或者分号、默认键值对后面无注释。也就是说注释尽量不要一行里写了键值对又写注释。

<details>
<summary>ConfigParser 类的属性和方法</summary>

```python
ConfigParser -- responsible for parsing a list of
                    configuration files, and managing the parsed database.
 
    methods:
 
    __init__(defaults=None, dict_type=_default_dict, allow_no_value=False,
             delimiters=('=', ':'), comment_prefixes=('#', ';'),
             inline_comment_prefixes=None, strict=True,
             empty_lines_in_values=True, default_section='DEFAULT',
             interpolation=<unset>, converters=<unset>):
        Create the parser. When `defaults' is given, it is initialized into the
        dictionary or intrinsic defaults. The keys must be strings, the values
        must be appropriate for %()s string interpolation.
 
        When `dict_type' is given, it will be used to create the dictionary
        objects for the list of sections, for the options within a section, and
        for the default values.
 
        When `delimiters' is given, it will be used as the set of substrings
        that divide keys from values.
 
        When `comment_prefixes' is given, it will be used as the set of
        substrings that prefix comments in empty lines. Comments can be
        indented.
 
        When `inline_comment_prefixes' is given, it will be used as the set of
        substrings that prefix comments in non-empty lines.
 
        When `strict` is True, the parser won't allow for any section or option
        duplicates while reading from a single source (file, string or
        dictionary). Default is True.
 
        When `empty_lines_in_values' is False (default: True), each empty line
        marks the end of an option. Otherwise, internal empty lines of
        a multiline option are kept as part of the value.
 
        When `allow_no_value' is True (default: False), options without
        values are accepted; the value presented for these is None.
 
        When `default_section' is given, the name of the special section is
        named accordingly. By default it is called ``"DEFAULT"`` but this can
        be customized to point to any other valid section name. Its current
        value can be retrieved using the ``parser_instance.default_section``
        attribute and may be modified at runtime.
 
        When `interpolation` is given, it should be an Interpolation subclass
        instance. It will be used as the handler for option value
        pre-processing when using getters. RawConfigParser objects don't do
        any sort of interpolation, whereas ConfigParser uses an instance of
        BasicInterpolation. The library also provides a ``zc.buildbot``
        inspired ExtendedInterpolation implementation.
 
        When `converters` is given, it should be a dictionary where each key
        represents the name of a type converter and each value is a callable
        implementing the conversion from string to the desired datatype. Every
        converter gets its corresponding get*() method on the parser object and
        section proxies.
 
    sections()
        Return all the configuration section names, sans DEFAULT.
 
    has_section(section)
        Return whether the given section exists.
 
    has_option(section, option)
        Return whether the given option exists in the given section.
 
    options(section)
        Return list of configuration options for the named section.
 
    read(filenames, encoding=None)
        Read and parse the iterable of named configuration files, given by
        name.  A single filename is also allowed.  Non-existing files
        are ignored.  Return list of successfully read files.
 
    read_file(f, filename=None)
        Read and parse one configuration file, given as a file object.
        The filename defaults to f.name; it is only used in error
        messages (if f has no `name' attribute, the string `<???>' is used).
 
    read_string(string)
        Read configuration from a given string.
 
    read_dict(dictionary)
        Read configuration from a dictionary. Keys are section names,
        values are dictionaries with keys and values that should be present
        in the section. If the used dictionary type preserves order, sections
        and their keys will be added in order. Values are automatically
        converted to strings.
 
    get(section, option, raw=False, vars=None, fallback=_UNSET)
        Return a string value for the named option.  All % interpolations are
        expanded in the return values, based on the defaults passed into the
        constructor and the DEFAULT section.  Additional substitutions may be
        provided using the `vars' argument, which must be a dictionary whose
        contents override any pre-existing defaults. If `option' is a key in
        `vars', the value from `vars' is used.
 
    getint(section, options, raw=False, vars=None, fallback=_UNSET)
        Like get(), but convert value to an integer.
 
    getfloat(section, options, raw=False, vars=None, fallback=_UNSET)
        Like get(), but convert value to a float.
 
    getboolean(section, options, raw=False, vars=None, fallback=_UNSET)
        Like get(), but convert value to a boolean (currently case
        insensitively defined as 0, false, no, off for False, and 1, true,
        yes, on for True).  Returns False or True.
 
    items(section=_UNSET, raw=False, vars=None)
        If section is given, return a list of tuples with (name, value) for
        each option in the section. Otherwise, return a list of tuples with
        (section_name, section_proxy) for each section, including DEFAULTSECT.
 
    remove_section(section)
        Remove the given file section and all its options.
 
    remove_option(section, option)
        Remove the given option from the given section.
 
    set(section, option, value)
        Set the given option.
 
    write(fp, space_around_delimiters=True)
        Write the configuration state in .ini format. If
        `space_around_delimiters' is True (the default), delimiters
        between keys and values are surrounded by spaces.
```

</details>


# 常用方法

## 打开配置文件

```python
import configparser

file = 'config.ini'
# 创建配置文件对象
cfg = configparser.ConfigParser()
# 读取配置文件
cfg.read(file, encoding='utf-8')
```

这里只打开不做什么读取和改变

## 读取配置文件的所有 section

file处替换为对应的配置文件即可
```python
import configparser

file = 'config.ini'
cfg = configparser.ConfigParser()
cfg.read(file, encoding='utf-8')
#  获取所有section
sections = cfg.sections()
#  显示读取的section结果
print(sections)
```

## 判断有没有对应的 section!!!

当没有对应的section就直接操作时程序会非正常结束

```python
import configparser

file = 'config.ini'
cfg = configparser.ConfigParser()
cfg.read(file, encoding='utf-8')
if cfg.has_section("Default"):  #  有没有"Default" section
    print("存在Defaul section")
else:
	print("不存在Defaul section")	    
```

## 判断section下对应的Option

```python
import configparser

file = 'config.ini'
cfg = configparser.ConfigParser()
cfg.read(file, encoding='utf-8')
#  检测Default section下有没有"CompressionLevel" option
if cfg.has_option('Default', 'CompressionLevel'):  
    print("存在CompressionLevel option")
else:
	print("不存在CompressionLevel option")	    
```

## 添加section、option 与保存修改

最最重要的事情： 最后一定要写入文件进行保存！！！不然程序修改的结果不会修改到文件里
1. 添加section前要检测是否存在，否则存在重名的话就会报错程序非正常结束
2. 添加option前要确定section存在，否则同 1
3. 修改 option 时不存在该 option 就会创建该 option

```python
import configparser

file = 'config.ini'
cfg = configparser.ConfigParser()
cfg.read(file, encoding='utf-8')

if not cfg.has_section("Color"):  # 不存在Color section就创建
    cfg.add_section('Color')

#  设置sectin下的option的value，如果section不存在就会报错
cfg.set('Color', 'isset', 'true')
cfg.set('Color', 'version', '1.1.0')    
cfg.set('Color', 'orange', '150,100,100')

# 把所作的修改写入配置文件
with open(file, 'w', encoding='utf-8') as configfile:
    cfg.write(configfile)
```

## 删除option

```python
import configparser

file = 'config.ini'
cfg = configparser.ConfigParser()
cfg.read(file, encoding='utf-8')

cfg.remove_option('Default', 'CompressionLevel'

# 把所作的修改写入配置文件
with open(file, 'w', encoding='utf-8') as configfile:
    cfg.write(configfile)
```


## 删除section

删除 section 的时候会递归自动删除该 section 下面的所有 option，慎重使用

```python
import configparser

file = 'config.ini'
cfg = configparser.ConfigParser()
cfg.read(file, encoding='utf-8')

cfg.remove_section('Default')

# 把所作的修改写入配置文件
with open(file, 'w', encoding='utf-8') as configfile:
    cfg.write(configfile)
```
      


## 大小写问题

configparser 存储键值对的时候，会把键都转小写存储，值则按原样转字符存储，如果想要把键也按原样存储，则需要重载  optionxform 函数，根据这两篇文章的叙述，

```python
# /usr/lib/python3.9/configparser.py Line 874
    def optionxform(self, optionstr):
        return optionstr.lower()  # 转小写
```

> [python3 configparser 区分大小写.狐狸也糊涂丶.2020.10.14](https://www.jianshu.com/p/3e4f10bfac8d)：py2、3 重载

> [修改Python ConfigParser option 大小写的问题.xluren. 2014-10-20 ](https://blog.csdn.net/xluren/article/details/40298561)：py2 重载


`狐狸也糊涂丶`给出了 py3 的简单重载办法如下，只需要用两行替换原来的一行即可

```python
config= configparser.RawConfigParser()
config.optionxform = lambda option: option
```

# 案例

## 创建一个配置文件

下面的demo介绍了如何检测添加section和设置value

```python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-
import configparser

file = 'config.ini'

# 创建配置文件对象
cfg= configparser.RawConfigParser()
cfg.optionxform = lambda option: option
# 读取配置文件
cfg.read(file, encoding='utf-8')

if not cfg.has_section("Default"):  #  有没有"Default" section
    cfg.add_section("Default")      #  没有就创建

#  设置"Default" section下的option的value
#  如果这个section不存在就会报错，所以上面要检测和创建
cfg.set('Default', 'ServerAliveInterval', '45')
cfg.set('Default', 'Compression', 'yes')
cfg.set('Default', 'CompressionLevel', '9')
cfg.set('Default', 'ForwardX11', 'yes')

if not cfg.has_section("Color"):  # 不存在Color就创建
    cfg.add_section('Color')

#  设置sectin下的option的value，如果section不存在就会报错
cfg.set('Color', 'isset', 'true')
cfg.set('Color', 'version', '1.1.0')    
cfg.set('Color', 'orange', '150,100,100')
cfg.set('Color', 'lightgreen', '0,220,0')

if not cfg.has_section("User"):  
    cfg.add_section('User')

cfg.set('User', 'iscrypted', 'false')
cfg.set('User', 'Kearney', '191615342@qq.com')
cfg.set('User', 'Tony', 'backmountain@gmail.com')


# 把所作的修改写入配置文件，并不是完全覆盖文件
with open(file, 'w', encoding='utf-8') as configfile:
    cfg.write(configfile)
```

跑上面的程序就会创建一个config.ini的配置文件，然后添加section和option-value
文件内容如下所示

```ini
[Default]
serveraliveinterval = 45
compression = Yes
compressionlevel = 9
forwardx11 = Yes
ServerAliveInterval = 45
Compression = Yes
CompressionLevel = 9
ForwardX11 = Yes

[Color]
isset = true
version = 1.1.0
orange = 150,100,100
lightgreen = 0,220,0

[User]
iscrypted = false
kearney = 191615342@qq.com
tony = backmountain@gmail.com
Kearney = 191615342@qq.com
Tony = backmountain@gmail.com
```

# 相关参考
- [Configuration file parser - py2](https://docs.python.org/2/library/configparser.html#examples)
- [Configuration file parser - py3](https://docs.python.org/3/library/configparser.html)
- [python读取配置文件（ini、yaml、xml）-ini只读不写。。](https://blog.csdn.net/weixin_44409630/article/details/93074115)
- [python 编写配置文件 - open不规范，注释和上一篇参考冲突](https://blog.csdn.net/qq_43280079/article/details/103939564)
