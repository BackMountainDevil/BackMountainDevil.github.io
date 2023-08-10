# 用 Uno 当烧录器给 atmega328 烧录 bootloader
- date: 2023-8-10

## 引脚接线

把两个板子的 11(MOSI)、12(MISO)、13(SCK)、5V、GND 两两相连，还要把 Uno（烧录器）的 10 接到atmega328（待烧录的对象）的 RES(RESET) 上。这六根线必接，程序里还有三个 LED 状态指示，可以不接

接线，然后将示例中的 ArduinoISP 程序按照正常操作验证上传到 Uno 上，这一步通常没啥问题。其实这一步是把 ino 程序编译后缀为 hex 的固件，然后把固件刷上去（打开详细输出就能看到这些信息）

### error

```bash
avrdude error: cannot find USBtiny device (0x2341/0x49)
avrdude error: unable to open programmer arduinoisp on port usb
烧录引导程序出错。
```

菜单栏：文件 - 首选项，显示详细输出，把上传给勾选上

再尝试一次获取详细信息，工具-编程器 选 `Arduino as ISP`（默认是 AVRISP）,然后 工具-烧录引导程序

```bash
Arduino:1.8.19 (Linux), 开发板："Arduino Uno"

//bin/avrdude -C//etc/avrdude.conf -v -patmega328p -carduinoisp -e -Ulock:w:0x3F:m -Uefuse:w:0xFD:m -Uhfuse:w:0xDE:m -Ulfuse:w:0xFF:m 

avrdude: Version 7.2
         Copyright the AVRDUDE authors;
         see https://github.com/avrdudes/avrdude/blob/main/AUTHORS

         System wide configuration file is /etc/avrdude.conf
         User configuration file is /home/mifen/.avrduderc
         User configuration file does not exist or is not a regular file, skipping

         Using Port                    : usb
         Using Programmer              : arduinoisp
avrdude usbtiny_open() error: cannot find USBtiny device (0x2341/0x49)
avrdude main() error: unable to open programmer arduinoisp on port usb

avrdude done.  Thank you.

烧录引导程序出错。
```

上面的 Port 感觉就不对，按理说是类似这样的 `/dev/ttyUSB0`，通过编译上传的详细信息可以看到 avrdude 在上传时也是识别到了对应端口，怎么到这一步骤就不行了呢。然后查了一下网上的，要么更新程序，要么用别的软件来搞，有准备尝试配置文件的方式，最后发现是编程器选错了，要选 `Arduino as ISP` 而不是 `ArduinoISP`，成功烧录的详细信息如下

### 成功烧录详情

```bash
//bin/avrdude -C//etc/avrdude.conf -v -patmega328p -cstk500v1 -P/dev/ttyUSB0 -b19200 -e -Ulock:w:0x3F:m -Uefuse:w:0xFD:m -Uhfuse:w:0xDE:m -Ulfuse:w:0xFF:m 

avrdude: Version 7.2
         Copyright the AVRDUDE authors;
         see https://github.com/avrdudes/avrdude/blob/main/AUTHORS

         System wide configuration file is /etc/avrdude.conf
         User configuration file is /home/mifen/.avrduderc
         User configuration file does not exist or is not a regular file, skipping

         Using Port                    : /dev/ttyUSB0
         Using Programmer              : stk500v1
         Overriding Baud Rate          : 19200
         AVR Part                      : ATmega328P
         Chip Erase delay              : 9000 us
         PAGEL                         : PD7
         BS2                           : PC2
         RESET disposition             : possible i/o
         RETRY pulse                   : SCK
         Serial program mode           : yes
         Parallel program mode         : yes
         Timeout                       : 200
         StabDelay                     : 100
         CmdexeDelay                   : 25
         SyncLoops                     : 32
         PollIndex                     : 3
         PollValue                     : 0x53
         Memory Detail                 :

                                           Block Poll               Page                       Polled
           Memory Type Alias    Mode Delay Size  Indx Paged  Size   Size #Pages MinW  MaxW   ReadBack
           ----------- -------- ---- ----- ----- ---- ------ ------ ---- ------ ----- ----- ---------
           eeprom                 65    20     4    0 no       1024    4      0  3600  3600 0x00 0x00
           flash                  65    10   128    0 yes     32768  128    256  4500  4500 0x00 0x00
           lfuse                   0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           hfuse                   0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           efuse                   0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           lock                    0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           signature               0     0     0    0 no          3    1      0     0     0 0x00 0x00
           calibration             0     0     0    0 no          1    1      0     0     0 0x00 0x00

         Programmer Type : STK500
         Description     : Atmel STK500 version 1.x firmware
         Hardware Version: 2
         Firmware Version: 1.18
         Topcard         : Unknown
         Vtarget         : 0.0 V
         Varef           : 0.0 V
         Oscillator      : Off
         SCK period      : 0.1 us
avrdude: AVR device initialized and ready to accept instructions
avrdude: device signature = 0x1e950f (probably m328p)
avrdude: erasing chip

avrdude: processing -U lock:w:0x3F:m
avrdude: reading input file 0x3F for lock
         with 1 byte in 1 section within [0, 0]
avrdude: writing 1 byte lock ...
avrdude: 1 byte of lock written
avrdude: verifying lock memory against 0x3F
avrdude avr_verify() warning: ignoring mismatch in unused bits of lock
        (device 0xff != input 0x3f); to prevent this warning set
        unused bits to 1 when writing (double check with datasheet)
avrdude: 1 byte of lock verified

avrdude: processing -U efuse:w:0xFD:m
avrdude: reading input file 0xFD for efuse
         with 1 byte in 1 section within [0, 0]
avrdude: writing 1 byte efuse ...
avrdude: 1 byte of efuse written
avrdude: verifying efuse memory against 0xFD
avrdude: 1 byte of efuse verified

avrdude: processing -U hfuse:w:0xDE:m
avrdude: reading input file 0xDE for hfuse
         with 1 byte in 1 section within [0, 0]
avrdude: writing 1 byte hfuse ...
avrdude: 1 byte of hfuse written
avrdude: verifying hfuse memory against 0xDE
avrdude: 1 byte of hfuse verified

avrdude: processing -U lfuse:w:0xFF:m
avrdude: reading input file 0xFF for lfuse
         with 1 byte in 1 section within [0, 0]
avrdude: writing 1 byte lfuse ...
avrdude: 1 byte of lfuse written
avrdude: verifying lfuse memory against 0xFF
avrdude: 1 byte of lfuse verified

avrdude done.  Thank you.

//bin/avrdude -C//etc/avrdude.conf -v -patmega328p -cstk500v1 -P/dev/ttyUSB0 -b19200 -Uflash:w:/usr/share/arduino/hardware/archlinux-arduino/avr/bootloaders/optiboot/optiboot_atmega328.hex:i -Ulock:w:0x0F:m 

avrdude: Version 7.2
         Copyright the AVRDUDE authors;
         see https://github.com/avrdudes/avrdude/blob/main/AUTHORS

         System wide configuration file is /etc/avrdude.conf
         User configuration file is /home/mifen/.avrduderc
         User configuration file does not exist or is not a regular file, skipping

         Using Port                    : /dev/ttyUSB0
         Using Programmer              : stk500v1
         Overriding Baud Rate          : 19200
         AVR Part                      : ATmega328P
         Chip Erase delay              : 9000 us
         PAGEL                         : PD7
         BS2                           : PC2
         RESET disposition             : possible i/o
         RETRY pulse                   : SCK
         Serial program mode           : yes
         Parallel program mode         : yes
         Timeout                       : 200
         StabDelay                     : 100
         CmdexeDelay                   : 25
         SyncLoops                     : 32
         PollIndex                     : 3
         PollValue                     : 0x53
         Memory Detail                 :

                                           Block Poll               Page                       Polled
           Memory Type Alias    Mode Delay Size  Indx Paged  Size   Size #Pages MinW  MaxW   ReadBack
           ----------- -------- ---- ----- ----- ---- ------ ------ ---- ------ ----- ----- ---------
           eeprom                 65    20     4    0 no       1024    4      0  3600  3600 0x00 0x00
           flash                  65    10   128    0 yes     32768  128    256  4500  4500 0x00 0x00
           lfuse                   0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           hfuse                   0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           efuse                   0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           lock                    0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           signature               0     0     0    0 no          3    1      0     0     0 0x00 0x00
           calibration             0     0     0    0 no          1    1      0     0     0 0x00 0x00

         Programmer Type : STK500
         Description     : Atmel STK500 version 1.x firmware
         Hardware Version: 2
         Firmware Version: 1.18
         Topcard         : Unknown
         Vtarget         : 0.0 V
         Varef           : 0.0 V
         Oscillator      : Off
         SCK period      : 0.1 us
avrdude: AVR device initialized and ready to accept instructions
avrdude: device signature = 0x1e950f (probably m328p)
avrdude: Note: flash memory has been specified, an erase cycle will be performed.
         To disable this feature, specify the -D option.
avrdude: erasing chip

avrdude: processing -U flash:w:/usr/share/arduino/hardware/archlinux-arduino/avr/bootloaders/optiboot/optiboot_atmega328.hex:i
avrdude: reading input file /usr/share/arduino/hardware/archlinux-arduino/avr/bootloaders/optiboot/optiboot_atmega328.hex for flash
         with 502 bytes in 2 sections within [0x7e00, 0x7fff]
         using 4 pages and 10 pad bytes
avrdude: writing 502 bytes flash ...
Writing | ################################################## | 100% 0.08s
avrdude: 502 bytes of flash written
avrdude: verifying flash memory against /usr/share/arduino/hardware/archlinux-arduino/avr/bootloaders/optiboot/optiboot_atmega328.hex
Reading | ################################################## | 100% 0.00s
avrdude: 502 bytes of flash verified

avrdude: processing -U lock:w:0x0F:m
avrdude: reading input file 0x0F for lock
         with 1 byte in 1 section within [0, 0]
avrdude: writing 1 byte lock ...
avrdude: 1 byte of lock written
avrdude: verifying lock memory against 0x0F
avrdude avr_verify() warning: ignoring mismatch in unused bits of lock
        (device 0xcf != input 0x0f); to prevent this warning set
        unused bits to 1 when writing (double check with datasheet)
avrdude: 1 byte of lock verified

avrdude done.  Thank you.
```

用国行（开发板信息BN显示未知、无SN）烧正规军（可以获取开发板信息如序列号）可以（上面就是），用正规军烧国行则不行，会报错如下

```bash
Arduino:1.8.19 (Linux), 开发板："Arduino Uno"

//bin/avrdude -C//etc/avrdude.conf -v -patmega328p -cstk500v1 -P/dev/ttyACM0 -b19200 -e -Ulock:w:0x3F:m -Uefuse:w:0xFD:m -Uhfuse:w:0xDE:m -Ulfuse:w:0xFF:m 

avrdude: Version 7.2
         Copyright the AVRDUDE authors;
         see https://github.com/avrdudes/avrdude/blob/main/AUTHORS

         System wide configuration file is /etc/avrdude.conf
         User configuration file is /home/mifen/.avrduderc
         User configuration file does not exist or is not a regular file, skipping

         Using Port                    : /dev/ttyACM0
         Using Programmer              : stk500v1
         Overriding Baud Rate          : 19200
         AVR Part                      : ATmega328P
         Chip Erase delay              : 9000 us
         PAGEL                         : PD7
         BS2                           : PC2
         RESET disposition             : possible i/o
         RETRY pulse                   : SCK
         Serial program mode           : yes
         Parallel program mode         : yes
         Timeout                       : 200
         StabDelay                     : 100
         CmdexeDelay                   : 25
         SyncLoops                     : 32
         PollIndex                     : 3
         PollValue                     : 0x53
         Memory Detail                 :

                                           Block Poll               Page                       Polled
           Memory Type Alias    Mode Delay Size  Indx Paged  Size   Size #Pages MinW  MaxW   ReadBack
           ----------- -------- ---- ----- ----- ---- ------ ------ ---- ------ ----- ----- ---------
           eeprom                 65    20     4    0 no       1024    4      0  3600  3600 0x00 0x00
           flash                  65    10   128    0 yes     32768  128    256  4500  4500 0x00 0x00
           lfuse                   0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           hfuse                   0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           efuse                   0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           lock                    0     0     0    0 no          1    1      0  4500  4500 0x00 0x00
           signature               0     0     0    0 no          3    1      0     0     0 0x00 0x00
           calibration             0     0     0    0 no          1    1      0     0     0 0x00 0x00

         Programmer Type : STK500
         Description     : Atmel STK500 version 1.x firmware
         Hardware Version: 2
         Firmware Version: 1.18
         Topcard         : Unknown
         Vtarget         : 0.0 V
         Varef           : 0.0 V
         Oscillator      : Off
         SCK period      : 0.1 us
avrdude: AVR device initialized and ready to accept instructions
avrdude: device signature = 0x000000 (retrying)
avrdude: device signature = 0x000000 (retrying)
avrdude: device signature = 0x000000
avrdude main() error: Yikes!  Invalid device signature.
avrdude main() error: expected signature for ATmega328P is 1E 95 0F
        Double check connections and try again, or use -F to override
        this check.


avrdude done.  Thank you.

烧录引导程序出错。
```

根据上面的错误信息，我在终端加 -F 也不行，还是验证失败。奇怪的是，国行和正规军的差别在于串口芯片啊，Arduino as ISP 没走 ch340 国道啊，mcu 都是一样的咋就不行了？然后又对调一下（接线没动），发现出现了上面验证失败的错误，也就是说明接错线会发生这个错误。然而实际上刚才我有对调11、12看接线，没想到啊，看样子是刚才接线错了，重新拔掉接一遍就可以了。

# 指定引导程序

如果说要烧录指定的引导程序，如[ATmega-Soldering-Station](https://github.com/wagiminator/ATmega-Soldering-Station)下的引导文件，那该怎么办呢？结合仓库的介绍和上面的详细信息可以知道两行指令，需要修改端口号和文件路径，在终端中运行即可

```bash
avrdude -C//etc/avrdude.conf -v -patmega328p -cstk500v1 -P/dev/ttyACM0 -b19200 -e -Ulock:w:0x3F:m -Uefuse:w:0xFD:m -Uhfuse:w:0xDE:m -Ulfuse:w:0xFF:m 

avrdude -C//etc/avrdude.conf -v -patmega328p -cstk500v1 -P/dev/ttyACM0 -b19200 -Uflash:w:/path/to/filename.hex:i -Ulock:w:0x0F:m 
```

想刷回原版引导，用 arduino 运行烧录引导程序程序即可

# 参考

[Arduino 烧写bootloader cheng3100 2017.02.10](https://www.jianshu.com/p/2f274f8b3dab)

[[Arduino]烧写Arduino BootLoader的几种方法 李万鑫 2017-06-27](https://blog.csdn.net/sysjtlwx/article/details/73824903):和上面接线是一样的，只是看着不一样，因为 ISP 口有几个

[Arduino: [已修复] IDE 1.6.9 + ArduinoISP =“找不到 USBtiny 设备 (0x2341/0x49)”错误消息 =&gt; 使用“Arduino 作为 ISP”2016-07-12 HP-Sparks](https://bleepcoder.com/cn/arduino/165179524/fixed-ide-1-6-9-arduinoisp-could-not-find-usbtiny-device)

> 我找到了问题的根源：程序员选择的“ArduinoISP”与“Arduino as ISP”
