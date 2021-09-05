触摸板有时候无法使用，重启之后就行

## lattop
https://wiki.archlinux.org/title/Laptop#Touchpad

Touchpad not detected at all

If a touchpad device is not detected and shown as a device at all, a possible 
solution might be using one or more of these kernel parameters.

i8042.noloop i8042.nomux i8042.nopnp i8042.reset

## libinput

https://wiki.archlinux.org/title/Libinput#Installation



## 正常使用

可通过功能键（F6）开启（桌面提示：触摸板启用）和关闭（桌面提示：触摸板关闭）

```bash
# 查看输入设备
$ sudo libinput list-devices
Device:           Power Button
Kernel:           /dev/input/event2
Group:            1
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Video Bus
Kernel:           /dev/input/event4
Group:            2
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Power Button
Kernel:           /dev/input/event1
Group:            3
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Lid Switch
Kernel:           /dev/input/event0
Group:            4
Seat:             seat0, default
Capabilities:     switch
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           USB OPTICAL MOUSE 
Kernel:           /dev/input/event13
Group:            5
Seat:             seat0, default
Capabilities:     pointer 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      disabled
Nat.scrolling:    disabled
Middle emulation: disabled
Calibration:      n/a
Scroll methods:   button
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   flat *adaptive
Rotation:         n/a

Device:           USB OPTICAL MOUSE  Keyboard
Kernel:           /dev/input/event14
Group:            5
Seat:             seat0, default
Capabilities:     keyboard pointer 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    disabled
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Integrated Camera: Integrated C
Kernel:           /dev/input/event15
Group:            6
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Integrated Camera: Integrated I
Kernel:           /dev/input/event16
Group:            6
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Ideapad extra buttons
Kernel:           /dev/input/event6
Group:            7
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           MSFT0001:00 06CB:CD3E Mouse
Kernel:           /dev/input/event7
Group:            8
Seat:             seat0, default
Capabilities:     pointer 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      disabled
Nat.scrolling:    disabled
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   *button
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   flat *adaptive
Rotation:         n/a

Device:           MSFT0001:00 06CB:CD3E Touchpad
Kernel:           /dev/input/event8
Group:            8
Seat:             seat0, default
Size:             101x66mm
Capabilities:     pointer gesture
Tap-to-click:     disabled
Tap-and-drag:     enabled
Tap drag lock:    disabled
Left-handed:      disabled
Nat.scrolling:    disabled
Middle emulation: disabled
Calibration:      n/a
Scroll methods:   *two-finger edge 
Click methods:    *button-areas clickfinger 
Disable-w-typing: enabled
Accel profiles:   flat *adaptive
Rotation:         n/a

Device:           AT Translated Set 2 keyboard
Kernel:           /dev/input/event3
Group:            9
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           MIIIW KB Air Mini Keyboard
Kernel:           /dev/input/event17
Group:            10
Seat:             seat0, default
Capabilities:     keyboard pointer 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    disabled
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

```


```bash
$ xinput list
⎡ Virtual core pointer                          id=2    [master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer                id=4    [slave  pointer  (2)]
⎜   ↳ USB OPTICAL MOUSE                         id=9    [slave  pointer  (2)]
⎜   ↳ USB OPTICAL MOUSE  Keyboard               id=10   [slave  pointer  (2)]
⎜   ↳ MSFT0001:00 06CB:CD3E Mouse               id=14   [slave  pointer  (2)]
⎜   ↳ MIIIW KB Air Mini Keyboard                id=18   [slave  pointer  (2)]
⎜   ↳ MSFT0001:00 06CB:CD3E Touchpad            id=15   [slave  pointer  (2)]
⎣ Virtual core keyboard                         id=3    [master keyboard (2)]
    ↳ Virtual core XTEST keyboard               id=5    [slave  keyboard (3)]
    ↳ Power Button                              id=6    [slave  keyboard (3)]
    ↳ Video Bus                                 id=7    [slave  keyboard (3)]
    ↳ Power Button                              id=8    [slave  keyboard (3)]
    ↳ Integrated Camera: Integrated C           id=11   [slave  keyboard (3)]
    ↳ Integrated Camera: Integrated I           id=12   [slave  keyboard (3)]
    ↳ Ideapad extra buttons                     id=13   [slave  keyboard (3)]
    ↳ AT Translated Set 2 keyboard              id=16   [slave  keyboard (3)]
    ↳ USB OPTICAL MOUSE  Keyboard               id=17   [slave  keyboard (3)]
    ↳ MIIIW KB Air Mini Keyboard                id=19   [slave  keyboard (3)]
```

```bash
$ cat /proc/bus/input/devices 
I: Bus=0019 Vendor=0000 Product=0005 Version=0000
N: Name="Lid Switch"
P: Phys=PNP0C0D/button/input0
S: Sysfs=/devices/LNXSYSTM:00/LNXSYBUS:00/PNP0C0D:00/input/input0
U: Uniq=
H: Handlers=event0 
B: PROP=0
B: EV=21
B: SW=1

I: Bus=0019 Vendor=0000 Product=0001 Version=0000
N: Name="Power Button"
P: Phys=PNP0C0C/button/input0
S: Sysfs=/devices/LNXSYSTM:00/LNXSYBUS:00/PNP0C0C:00/input/input1
U: Uniq=
H: Handlers=kbd event1 
B: PROP=0
B: EV=3
B: KEY=10000000000000 0

I: Bus=0019 Vendor=0000 Product=0001 Version=0000
N: Name="Power Button"
P: Phys=LNXPWRBN/button/input0
S: Sysfs=/devices/LNXSYSTM:00/LNXPWRBN:00/input/input2
U: Uniq=
H: Handlers=kbd event2 
B: PROP=0
B: EV=3
B: KEY=10000000000000 0

I: Bus=0011 Vendor=0001 Product=0001 Version=ab83
N: Name="AT Translated Set 2 keyboard"
P: Phys=isa0060/serio0/input0
S: Sysfs=/devices/platform/i8042/serio0/input/input3
U: Uniq=
H: Handlers=sysrq kbd leds event3 
B: PROP=0
B: EV=120013
B: KEY=402000000 3803078f800d001 feffffdfffefffff fffffffffffffffe
B: MSC=10
B: LED=7

I: Bus=0019 Vendor=0000 Product=0006 Version=0000
N: Name="Video Bus"
P: Phys=LNXVIDEO/video/input0
S: Sysfs=/devices/LNXSYSTM:00/LNXSYBUS:00/PNP0A08:00/device:05/LNXVIDEO:00/input/input4
U: Uniq=
H: Handlers=kbd event4 
B: PROP=0
B: EV=3
B: KEY=3e000b00000000 0 0 0

I: Bus=0019 Vendor=0000 Product=0000 Version=0000
N: Name="Ideapad extra buttons"
P: Phys=ideapad/input0
S: Sysfs=/devices/pci0000:00/0000:00:14.3/PNP0C09:00/VPC2004:00/input/input5
U: Uniq=
H: Handlers=kbd event5 rfkill 
B: PROP=0
B: EV=13
B: KEY=81000800100c03 4400000000300000 0 2
B: MSC=10

I: Bus=0010 Vendor=001f Product=0001 Version=0100
N: Name="PC Speaker"
P: Phys=isa0061/input0
S: Sysfs=/devices/platform/pcspkr/input/input6
U: Uniq=
H: Handlers=kbd event6 
B: PROP=0
B: EV=40001
B: SND=6

I: Bus=0003 Vendor=04f2 Product=b67c Version=9926
N: Name="Integrated Camera: Integrated C"
P: Phys=usb-0000:03:00.4-2/button
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.4/usb3/3-2/3-2:1.0/input/input7
U: Uniq=
H: Handlers=kbd event7 
B: PROP=0
B: EV=3
B: KEY=100000 0 0 0

I: Bus=0003 Vendor=04f2 Product=b67c Version=9926
N: Name="Integrated Camera: Integrated I"
P: Phys=usb-0000:03:00.4-2/button
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.4/usb3/3-2/3-2:1.2/input/input8
U: Uniq=
H: Handlers=kbd event8 
B: PROP=0
B: EV=3
B: KEY=100000 0 0 0

I: Bus=0000 Vendor=0000 Product=0000 Version=0000
N: Name="HD-Audio Generic HDMI/DP,pcm=3"
P: Phys=ALSA
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.1/sound/card0/input12
U: Uniq=
H: Handlers=event11 
B: PROP=0
B: EV=21
B: SW=140

I: Bus=0000 Vendor=0000 Product=0000 Version=0000
N: Name="HD-Audio Generic HDMI/DP,pcm=7"
P: Phys=ALSA
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.1/sound/card0/input13
U: Uniq=
H: Handlers=event12 
B: PROP=0
B: EV=21
B: SW=140

I: Bus=0000 Vendor=0000 Product=0000 Version=0000
N: Name="HD-Audio Generic Mic"
P: Phys=ALSA
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.6/sound/card1/input14
U: Uniq=
H: Handlers=event13 
B: PROP=0
B: EV=21
B: SW=10

I: Bus=0000 Vendor=0000 Product=0000 Version=0000
N: Name="HD-Audio Generic Headphone"
P: Phys=ALSA
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.6/sound/card1/input15
U: Uniq=
H: Handlers=event14 
B: PROP=0
B: EV=21
B: SW=4

I: Bus=0018 Vendor=06cb Product=cd3e Version=0100
N: Name="MSFT0001:00 06CB:CD3E Mouse"
P: Phys=i2c-MSFT0001:00
S: Sysfs=/devices/platform/AMDI0010:02/i2c-2/i2c-MSFT0001:00/0018:06CB:CD3E.0001/input/input16
U: Uniq=
H: Handlers=event9 mouse0 
B: PROP=0
B: EV=17
B: KEY=30000 0 0 0 0
B: REL=3
B: MSC=10

I: Bus=0018 Vendor=06cb Product=cd3e Version=0100
N: Name="MSFT0001:00 06CB:CD3E Touchpad"
P: Phys=i2c-MSFT0001:00
S: Sysfs=/devices/platform/AMDI0010:02/i2c-2/i2c-MSFT0001:00/0018:06CB:CD3E.0001/input/input17
U: Uniq=
H: Handlers=event10 mouse1 
B: PROP=5
B: EV=1b
B: KEY=e520 10000 0 0 0 0
B: ABS=2e0800000000003
B: MSC=20

I: Bus=0005 Vendor=2717 Product=0040 Version=0028
N: Name="MiMouse"
P: Phys=28:cd:c4:ba:bb:2c
S: Sysfs=/devices/virtual/misc/uhid/0005:2717:0040.0003/input/input19
U: Uniq=f7:f3:aa:56:9a:d0
H: Handlers=event15 mouse2 
B: PROP=0
B: EV=17
B: KEY=1f0000 0 0 0 0
B: REL=1943
B: MSC=10

I: Bus=0005 Vendor=2717 Product=0040 Version=0028
N: Name="MiMouse Consumer Control"
P: Phys=28:cd:c4:ba:bb:2c
S: Sysfs=/devices/virtual/misc/uhid/0005:2717:0040.0003/input/input20
U: Uniq=f7:f3:aa:56:9a:d0
H: Handlers=kbd event16 
B: PROP=0
B: EV=13
B: KEY=838c0000000 c000000000000 0
B: MSC=10
```


```bash
$ cat /var/log/Xorg.0.log | grep "input driver"
[     7.034] (II) Using input driver 'libinput' for 'Power Button'
[     7.134] (II) Using input driver 'libinput' for 'Video Bus'
[     7.374] (II) Using input driver 'libinput' for 'Power Button'
[     7.434] (II) No input driver specified, ignoring this device.
[     7.434] (II) No input driver specified, ignoring this device.
[     7.435] (II) No input driver specified, ignoring this device.
[     7.435] (II) Using input driver 'libinput' for 'Integrated Camera: Integrated C'
[     7.585] (II) Using input driver 'libinput' for 'Integrated Camera: Integrated I'
[     7.704] (II) No input driver specified, ignoring this device.
[     7.704] (II) No input driver specified, ignoring this device.
[     7.705] (II) Using input driver 'libinput' for 'Ideapad extra buttons'
[     7.794] (II) Using input driver 'libinput' for 'MSFT0001:00 06CB:CD3E Mouse'
[     7.934] (II) No input driver specified, ignoring this device.
[     7.935] (II) Using input driver 'libinput' for 'MSFT0001:00 06CB:CD3E Touchpad'
[     8.057] (II) No input driver specified, ignoring this device.
[     8.058] (II) Using input driver 'libinput' for 'AT Translated Set 2 keyboard'
[     8.095] (II) No input driver specified, ignoring this device.
[     8.212] (II) Using input driver 'libinput' for 'MSFT0001:00 06CB:CD3E Mouse'
[     8.482] (II) Using input driver 'libinput' for 'MSFT0001:00 06CB:CD3E Touchpad'
[     8.597] (II) No input driver specified, ignoring this device.
[     8.597] (II) No input driver specified, ignoring this device.
[    66.967] (II) No input driver specified, ignoring this device.
[    67.158] (II) Using input driver 'libinput' for 'MiMouse Consumer Control'
[    67.286] (II) Using input driver 'libinput' for 'MiMouse'
```

## 无法使用
偶尔发生，距离上次发生的时间过去了两个月  
功能键（F6）咋按都是关闭（桌面提示：触摸板关闭）
查看输入设备也没有 MSFT0001:00 06CB:CD3E Touchpad
可以正常使用蓝牙鼠标


```bash
# 查看输入设备
$ sudo libinput list-devices
Device:           Power Button
Kernel:           /dev/input/event2
Group:            1
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Video Bus
Kernel:           /dev/input/event4
Group:            2
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Power Button
Kernel:           /dev/input/event1
Group:            3
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Lid Switch
Kernel:           /dev/input/event0
Group:            4
Seat:             seat0, default
Capabilities:     switch
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Integrated Camera: Integrated C
Kernel:           /dev/input/event7
Group:            5
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Integrated Camera: Integrated I
Kernel:           /dev/input/event8
Group:            5
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           Ideapad extra buttons
Kernel:           /dev/input/event5
Group:            6
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           AT Translated Set 2 keyboard
Kernel:           /dev/input/event3
Group:            7
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a

Device:           MiMouse
Kernel:           /dev/input/event13
Group:            8
Seat:             seat0, default
Capabilities:     pointer 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      disabled
Nat.scrolling:    disabled
Middle emulation: disabled
Calibration:      n/a
Scroll methods:   button
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   flat *adaptive
Rotation:         n/a

Device:           MiMouse Consumer Control
Kernel:           /dev/input/event14
Group:            8
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Accel profiles:   n/a
Rotation:         n/a
```

```bash
# 可以对比看出没有识别到 触摸板
$ xinput list
⎡ Virtual core pointer                          id=2    [master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer                id=4    [slave  pointer  (2)]
⎜   ↳ MiMouse                                   id=14   [slave  pointer  (2)]
⎣ Virtual core keyboard                         id=3    [master keyboard (2)]
    ↳ Virtual core XTEST keyboard               id=5    [slave  keyboard (3)]
    ↳ Power Button                              id=6    [slave  keyboard (3)]
    ↳ Video Bus                                 id=7    [slave  keyboard (3)]
    ↳ Power Button                              id=8    [slave  keyboard (3)]
    ↳ Integrated Camera: Integrated C           id=9    [slave  keyboard (3)]
    ↳ Integrated Camera: Integrated I           id=10   [slave  keyboard (3)]
    ↳ Ideapad extra buttons                     id=11   [slave  keyboard (3)]
    ↳ AT Translated Set 2 keyboard              id=12   [slave  keyboard (3)]
    ↳ MiMouse Consumer Control                  id=13   [slave  keyboard (3)]

```


```bash
$ cat /proc/bus/input/devices 
I: Bus=0019 Vendor=0000 Product=0005 Version=0000
N: Name="Lid Switch"
P: Phys=PNP0C0D/button/input0
S: Sysfs=/devices/LNXSYSTM:00/LNXSYBUS:00/PNP0C0D:00/input/input0
U: Uniq=
H: Handlers=event0 
B: PROP=0
B: EV=21
B: SW=1

I: Bus=0019 Vendor=0000 Product=0001 Version=0000
N: Name="Power Button"
P: Phys=PNP0C0C/button/input0
S: Sysfs=/devices/LNXSYSTM:00/LNXSYBUS:00/PNP0C0C:00/input/input1
U: Uniq=
H: Handlers=kbd event1 
B: PROP=0
B: EV=3
B: KEY=10000000000000 0

I: Bus=0019 Vendor=0000 Product=0001 Version=0000
N: Name="Power Button"
P: Phys=LNXPWRBN/button/input0
S: Sysfs=/devices/LNXSYSTM:00/LNXPWRBN:00/input/input2
U: Uniq=
H: Handlers=kbd event2 
B: PROP=0
B: EV=3
B: KEY=10000000000000 0

I: Bus=0011 Vendor=0001 Product=0001 Version=ab83
N: Name="AT Translated Set 2 keyboard"
P: Phys=isa0060/serio0/input0
S: Sysfs=/devices/platform/i8042/serio0/input/input3
U: Uniq=
H: Handlers=sysrq kbd leds event3 
B: PROP=0
B: EV=120013
B: KEY=402000000 3803078f800d001 feffffdfffefffff fffffffffffffffe
B: MSC=10
B: LED=7

I: Bus=0019 Vendor=0000 Product=0006 Version=0000
N: Name="Video Bus"
P: Phys=LNXVIDEO/video/input0
S: Sysfs=/devices/LNXSYSTM:00/LNXSYBUS:00/PNP0A08:00/device:05/LNXVIDEO:00/input/input4
U: Uniq=
H: Handlers=kbd event4 
B: PROP=0
B: EV=3
B: KEY=3e000b00000000 0 0 0

I: Bus=0019 Vendor=0000 Product=0000 Version=0000
N: Name="Ideapad extra buttons"
P: Phys=ideapad/input0
S: Sysfs=/devices/pci0000:00/0000:00:14.3/PNP0C09:00/VPC2004:00/input/input5
U: Uniq=
H: Handlers=kbd event5 rfkill 
B: PROP=0
B: EV=13
B: KEY=81000800100c03 4400000000300000 0 2
B: MSC=10

I: Bus=0010 Vendor=001f Product=0001 Version=0100
N: Name="PC Speaker"
P: Phys=isa0061/input0
S: Sysfs=/devices/platform/pcspkr/input/input6
U: Uniq=
H: Handlers=kbd event6 
B: PROP=0
B: EV=40001
B: SND=6

I: Bus=0003 Vendor=04f2 Product=b67c Version=9926
N: Name="Integrated Camera: Integrated C"
P: Phys=usb-0000:03:00.4-2/button
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.4/usb3/3-2/3-2:1.0/input/input7
U: Uniq=
H: Handlers=kbd event7 
B: PROP=0
B: EV=3
B: KEY=100000 0 0 0

I: Bus=0003 Vendor=04f2 Product=b67c Version=9926
N: Name="Integrated Camera: Integrated I"
P: Phys=usb-0000:03:00.4-2/button
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.4/usb3/3-2/3-2:1.2/input/input8
U: Uniq=
H: Handlers=kbd event8 
B: PROP=0
B: EV=3
B: KEY=100000 0 0 0

I: Bus=0000 Vendor=0000 Product=0000 Version=0000
N: Name="HD-Audio Generic HDMI/DP,pcm=3"
P: Phys=ALSA
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.1/sound/card0/input9
U: Uniq=
H: Handlers=event9 
B: PROP=0
B: EV=21
B: SW=140

I: Bus=0000 Vendor=0000 Product=0000 Version=0000
N: Name="HD-Audio Generic HDMI/DP,pcm=7"
P: Phys=ALSA
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.1/sound/card0/input10
U: Uniq=
H: Handlers=event10 
B: PROP=0
B: EV=21
B: SW=140

I: Bus=0000 Vendor=0000 Product=0000 Version=0000
N: Name="HD-Audio Generic Mic"
P: Phys=ALSA
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.6/sound/card1/input11
U: Uniq=
H: Handlers=event11 
B: PROP=0
B: EV=21
B: SW=10

I: Bus=0000 Vendor=0000 Product=0000 Version=0000
N: Name="HD-Audio Generic Headphone"
P: Phys=ALSA
S: Sysfs=/devices/pci0000:00/0000:00:08.1/0000:03:00.6/sound/card1/input12
U: Uniq=
H: Handlers=event12 
B: PROP=0
B: EV=21
B: SW=4

I: Bus=0005 Vendor=2717 Product=0040 Version=0028
N: Name="MiMouse"
P: Phys=28:cd:c4:ba:bb:2c
S: Sysfs=/devices/virtual/misc/uhid/0005:2717:0040.000C/input/input35
U: Uniq=f7:f3:aa:56:9a:d0
H: Handlers=event13 mouse0 
B: PROP=0
B: EV=17
B: KEY=1f0000 0 0 0 0
B: REL=1943
B: MSC=10

I: Bus=0005 Vendor=2717 Product=0040 Version=0028
N: Name="MiMouse Consumer Control"
P: Phys=28:cd:c4:ba:bb:2c
S: Sysfs=/devices/virtual/misc/uhid/0005:2717:0040.000C/input/input36
U: Uniq=f7:f3:aa:56:9a:d0
H: Handlers=kbd event14 
B: PROP=0
B: EV=13
B: KEY=838c0000000 c000000000000 0
B: MSC=10

```

```bash

```

# end
