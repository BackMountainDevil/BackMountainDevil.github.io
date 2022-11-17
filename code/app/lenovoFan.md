# 联想小新风扇控制
- date: 2022-10-18
- lastmod: 2022-11-17

在 windows 上有官方驱动可以通过 Fn+Q 调节电脑模式（性能、平衡、省电），这个设置也可以在 BIOS 里面设置，但是在 linux 下这种快捷键由于没有驱动就实现不了这样的操作。

- [How to control fan speed - ThinkWiki](https://www.thinkwiki.org/wiki/How_to_control_fan_speed)

- [ExtremeCooling4Linux](https://gitlab.com/OdinTdh/extremecooling4linux/):ec_write(self.EXTREME_COOLING_REGISTER, self.ACTIVATE_EXTREME_COOLING)

## extremecooling4linux

https://gitlab.com/OdinTdh/extremecooling4linux/-/issues/15

### code analyse

Register address: `EXTREME_COOLING_REGISTER = 0xBD`

Register status:

    ACTIVATE_EXTREME_COOLING = 0x40

    DEACTIVATE_EXTREME_COOLING = 0x00

### install & test

install from [extremecooling4linux 0.3.1-1 AUR](https://aur.archlinux.org/packages/extremecooling4linux)

device: Lenovo XiaoXinPro-13ARE 2020. BIOS version:F0CN38WW(lastest)

three mode: battery saving, Extreme Performance, Intelligent Cooling

set three mode in BIOS. boot in os and run `pkexec ec4Linux.py` to check register status. However, all test show the same status of register. if the Register address is ok, then the status of register should be different when changing diff mode.

#### battery saving

```bash
$ pkexec ec4Linux.py 
INFO:extremecooling4linux:running ExtremeCooling4Linux in a normal Python process
INFO:extremecooling4linux:/usr/share/locale/
INFO:extremecooling4linux:running ExtremeCooling4Linux as gui program
INFO:extremecooling4linux:/root/.config/extremecooling4linux
INFO:extremecooling4linux:/root
INFO:extremecooling4linux:starting program
INFO:extremecooling4linux:Root user detected
INFO:extremecooling4linux:acces to :0x62
INFO:extremecooling4linux:acces to :0x66
INFO:extremecooling4linux:trying to detect extreme cooling support
INFO:extremecooling4linux:system_manufacturer LENOVO
INFO:extremecooling4linux:product_name 82DM
INFO:extremecooling4linux:product_version Lenovo XiaoXinPro-13ARE 2020
INFO:extremecooling4linux:chassis_type 10
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:is_extreme_cooling_activate value register 0x0 
INFO:extremecooling4linux:Extreme cooling could be supported, current value in extreme cooling register is 0x0 
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:is_extreme_cooling_activate value register 0x0
```

####  intelligence cooling

```bash
$ pkexec ec4Linux.py
INFO:extremecooling4linux:running ExtremeCooling4Linux in a normal Python process
INFO:extremecooling4linux:/usr/share/locale/
INFO:extremecooling4linux:running ExtremeCooling4Linux as gui program
INFO:extremecooling4linux:/root/.config/extremecooling4linux
INFO:extremecooling4linux:/root
INFO:extremecooling4linux:starting program
INFO:extremecooling4linux:Root user detected
INFO:extremecooling4linux:acces to :0x62
INFO:extremecooling4linux:acces to :0x66
INFO:extremecooling4linux:trying to detect extreme cooling support
INFO:extremecooling4linux:system_manufacturer LENOVO
INFO:extremecooling4linux:product_name 82DM
INFO:extremecooling4linux:product_version Lenovo XiaoXinPro-13ARE 2020
INFO:extremecooling4linux:chassis_type 10
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:is_extreme_cooling_activate value register 0x0 
INFO:extremecooling4linux:Extreme cooling could be supported, current value in extreme cooling register is 0x0 
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:is_extreme_cooling_activate value register 0x0
```

#### extre cooling

```bash
$ pkexec ec4Linux.py
INFO:extremecooling4linux:running ExtremeCooling4Linux in a normal Python process
INFO:extremecooling4linux:/usr/share/locale/
INFO:extremecooling4linux:running ExtremeCooling4Linux as gui program
INFO:extremecooling4linux:/root/.config/extremecooling4linux
INFO:extremecooling4linux:/root
INFO:extremecooling4linux:starting program
INFO:extremecooling4linux:Root user detected
INFO:extremecooling4linux:acces to :0x62
INFO:extremecooling4linux:acces to :0x66
INFO:extremecooling4linux:trying to detect extreme cooling support
INFO:extremecooling4linux:system_manufacturer LENOVO
INFO:extremecooling4linux:product_name 82DM
INFO:extremecooling4linux:product_version Lenovo XiaoXinPro-13ARE 2020
INFO:extremecooling4linux:chassis_type 10
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:is_extreme_cooling_activate value register 0x0 
INFO:extremecooling4linux:Extreme cooling could be supported, current value in extreme cooling register is 0x0 
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:is_extreme_cooling_activate value register 0x0

$ sudo ec4Linux
INFO:extremecooling4linux:running ExtremeCooling4Linux in a normal Python process
INFO:extremecooling4linux:/usr/share/locale/
INFO:extremecooling4linux:running ExtremeCooling4Linux as gui program
INFO:extremecooling4linux:/root/.config/extremecooling4linux
INFO:extremecooling4linux:/root
INFO:extremecooling4linux:starting program
INFO:extremecooling4linux:Root user detected
INFO:extremecooling4linux:acces to :0x62
INFO:extremecooling4linux:acces to :0x66
INFO:extremecooling4linux:trying to detect extreme cooling support
INFO:extremecooling4linux:system_manufacturer LENOVO
INFO:extremecooling4linux:product_name 82DM
INFO:extremecooling4linux:product_version Lenovo XiaoXinPro-13ARE 2020
INFO:extremecooling4linux:chassis_type 10
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:is_extreme_cooling_activate value register 0x0 
INFO:extremecooling4linux:Extreme cooling could be supported, current value in extreme cooling register is 0x0 
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:ec_wait
INFO:extremecooling4linux:ec_wait correct
INFO:extremecooling4linux:is_extreme_cooling_activate value register 0x0
```

### dive

[Not working with Lenovo Y910](https://gitlab.com/OdinTdh/extremecooling4linux/-/issues/10):使用 [RW Everything](http://rweverything.com/download/) 查找寄存器和对应的状态

[Identify registers](https://gitlab.com/OdinTdh/extremecooling4linux/-/issues/7): 

[lenovo forum](https://forums.lenovo.com/):搜不到 XiaoXinPro-13ARE

[NoteBook FanControl](https://github.com/hirschmann/nbfc)


## nbfc

[nbfc-linux 0.1.7-3](https://aur.archlinux.org/packages/nbfc-linux)

```bash
$ sudo nbfc config -r
HP Compaq Presario CQ40 Turion X2 RM-74
Acer Aspire VN7-793G V17 Nitro BE
Acer Aspire VN7-792G V17 Nitro BE
Acer Aspire VN7-591G V15 Nitro BE
Acer Aspire VN7-791G V17 Nitro BE
Acer Aspire VN7-593G V15 Nitro BE
Acer Aspire VN7-572G V15 Nitro BE
HP Compaq 6735s Turion X2 RM-72
Acer Aspire VN7-572G V15 Nitro
HP Spectre x360 Convertible 15-df1015ng
HP 245 G7 Notebook PC
HP ENVY x360 Convertible 15-cn0xxx
HP Spectre x360 Convertible 13-ae0xx
HP Spectre x360 Convertible 15-ch0xx
HP ZBook Fury 15 G7

$ sudo nbfc config --set auto
Error: Try `nbfc config -r` for recommended configs
```