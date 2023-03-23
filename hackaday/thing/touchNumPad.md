# 触摸板当作数字小键盘的方法
- date: 2023-3-23

# libinput
## record

横移三个点，竖着三个点，可以明显看到 `ABS_MT_POSITION_X` 和 `ABS_MT_POSITION_Y` 的值的变化规律，并且还有和上一次坐标的差值。

<details>
<summary>测试输出</summary>

```bash
$ sudo libinput record > touchpad.yml
/dev/input/event0:      Lid Switch
/dev/input/event1:      Power Button
/dev/input/event2:      Power Button
/dev/input/event3:      AT Translated Set 2 keyboard
/dev/input/event4:      Video Bus
/dev/input/event5:      MSFT0001:00 06CB:CD3E Mouse
/dev/input/event6:      MSFT0001:00 06CB:CD3E Touchpad
/dev/input/event7:      Ideapad extra buttons
/dev/input/event8:      Integrated Camera: Integrated C
/dev/input/event9:      Integrated Camera: Integrated I
/dev/input/event10:     HD-Audio Generic HDMI/DP,pcm=3
/dev/input/event11:     HD-Audio Generic HDMI/DP,pcm=7
/dev/input/event12:     HD-Audio Generic Mic
/dev/input/event13:     HD-Audio Generic Headphone
Select the device event number: 6
Recording to 'stdout'.
^C$ cat touchpad.yml 
# libinput record
version: 1
ndevices: 1
libinput:
  version: "1.22.1"
  git: "unknown"
system:
  kernel: "6.2.5-arch1-1"
  dmi: "dmi:bvnLENOVO:bvrF0CN38WW:bd09/24/2022:br1.38:efr1.38:svnLENOVO:pn82DM:pvrLenovoXiaoXinPro-13ARE2020:rvnLENOVO:rnLNVNB161216:rvrSDK0L77769WIN:cvnLENOVO:ct10:cvrLenovoXiaoXinPro-13ARE2020:skuLENOVO_MT_82DM_BU_idea_FM_XiaoXinPro-13ARE2020:"
devices:
- node: /dev/input/event6
  evdev:
    # Name: MSFT0001:00 06CB:CD3E Touchpad
    # ID: bus 0x18 vendor 0x6cb product 0xcd3e version 0x100
    # Size in mm: 101x66
    # Supported Events:
    # Event type 0 (EV_SYN)
    # Event type 1 (EV_KEY)
    #   Event code 272 (BTN_LEFT)
    #   Event code 325 (BTN_TOOL_FINGER)
    #   Event code 328 (BTN_TOOL_QUINTTAP)
    #   Event code 330 (BTN_TOUCH)
    #   Event code 333 (BTN_TOOL_DOUBLETAP)
    #   Event code 334 (BTN_TOOL_TRIPLETAP)
    #   Event code 335 (BTN_TOOL_QUADTAP)
    # Event type 3 (EV_ABS)
    #   Event code 0 (ABS_X)
    #       Value        1148
    #       Min             0
    #       Max          1212
    #       Fuzz            0
    #       Flat            0
    #       Resolution     12
    #   Event code 1 (ABS_Y)
    #       Value         479
    #       Min             0
    #       Max           792
    #       Fuzz            0
    #       Flat            0
    #       Resolution     12
    #   Event code 47 (ABS_MT_SLOT)
    #       Value           0
    #       Min             0
    #       Max             4
    #       Fuzz            0
    #       Flat            0
    #       Resolution      0
    #   Event code 53 (ABS_MT_POSITION_X)
    #       Value           0
    #       Min             0
    #       Max          1212
    #       Fuzz            0
    #       Flat            0
    #       Resolution     12
    #   Event code 54 (ABS_MT_POSITION_Y)
    #       Value           0
    #       Min             0
    #       Max           792
    #       Fuzz            0
    #       Flat            0
    #       Resolution     12
    #   Event code 55 (ABS_MT_TOOL_TYPE)
    #       Value           0
    #       Min             0
    #       Max             2
    #       Fuzz            0
    #       Flat            0
    #       Resolution      0
    #   Event code 57 (ABS_MT_TRACKING_ID)
    #       Value           0
    #       Min             0
    #       Max         65535
    #       Fuzz            0
    #       Flat            0
    #       Resolution      0
    # Event type 4 (EV_MSC)
    #   Event code 5 (MSC_TIMESTAMP)
    # Properties:
    #    Property 0 (INPUT_PROP_POINTER)
    #    Property 2 (INPUT_PROP_BUTTONPAD)
    name: "MSFT0001:00 06CB:CD3E Touchpad"
    id: [24, 1739, 52542, 256]
....
 udev:
    properties:
    - ID_INPUT=1
    - ID_INPUT_HEIGHT_MM=66
    - ID_INPUT_TOUCHPAD=1
    - ID_INPUT_WIDTH_MM=101
    - LIBINPUT_DEVICE_GROUP=18/6cb/cd3e:i2c-MSFT0001:00
  quirks:
  events:
  # Current time is 14:51:15
  - evdev:
    - [  0,      0,   3,  57,    2194] # EV_ABS / ABS_MT_TRACKING_ID     2194
    - [  0,      0,   3,  53,     206] # EV_ABS / ABS_MT_POSITION_X       206 (-942)
    - [  0,      0,   3,  54,     311] # EV_ABS / ABS_MT_POSITION_Y       311 (-168)
    - [  0,      0,   1, 330,       1] # EV_KEY / BTN_TOUCH                 1
    - [  0,      0,   1, 325,       1] # EV_KEY / BTN_TOOL_FINGER           1
    - [  0,      0,   3,   0,     206] # EV_ABS / ABS_X                   206 (-942)
    - [  0,      0,   3,   1,     311] # EV_ABS / ABS_Y                   311 (-168)
    - [  0,      0,   4,   5,       0] # EV_MSC / MSC_TIMESTAMP             0
    - [  0,      0,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +0ms
  - evdev:
    - [  0,   7094,   4,   5,    7300] # EV_MSC / MSC_TIMESTAMP          7300
    - [  0,   7094,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  0,  14775,   4,   5,   14500] # EV_MSC / MSC_TIMESTAMP         14500
    - [  0,  14775,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  0,  22142,   4,   5,   21800] # EV_MSC / MSC_TIMESTAMP         21800
    - [  0,  22142,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  0,  29168,   4,   5,   29000] # EV_MSC / MSC_TIMESTAMP         29000
    - [  0,  29168,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  0,  36499,   4,   5,   36300] # EV_MSC / MSC_TIMESTAMP         36300
    - [  0,  36499,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  0,  43918,   4,   5,   43500] # EV_MSC / MSC_TIMESTAMP         43500
    - [  0,  43918,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  0,  51607,   4,   5,   50800] # EV_MSC / MSC_TIMESTAMP         50800
    - [  0,  51607,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  0,  58811,   4,   5,   58000] # EV_MSC / MSC_TIMESTAMP         58000
    - [  0,  58811,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  0,  65119,   3,  57,      -1] # EV_ABS / ABS_MT_TRACKING_ID       -1
    - [  0,  65119,   1, 330,       0] # EV_KEY / BTN_TOUCH                 0
    - [  0,  65119,   1, 325,       0] # EV_KEY / BTN_TOOL_FINGER           0
    - [  0,  65119,   4,   5,   65300] # EV_MSC / MSC_TIMESTAMP         65300
    - [  0,  65119,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
                                       # Touch device in neutral state
  - evdev:
    - [  1, 142438,   3,  57,    2195] # EV_ABS / ABS_MT_TRACKING_ID     2195
    - [  1, 142438,   3,  53,     599] # EV_ABS / ABS_MT_POSITION_X       599 (+393)
    - [  1, 142438,   3,  54,     327] # EV_ABS / ABS_MT_POSITION_Y       327 (+16)
    - [  1, 142438,   1, 330,       1] # EV_KEY / BTN_TOUCH                 1
    - [  1, 142438,   1, 325,       1] # EV_KEY / BTN_TOOL_FINGER           1
    - [  1, 142438,   3,   0,     599] # EV_ABS / ABS_X                   599 (+393)
    - [  1, 142438,   3,   1,     327] # EV_ABS / ABS_Y                   327 (+16)
    - [  1, 142438,   4,   5,       0] # EV_MSC / MSC_TIMESTAMP             0
    - [  1, 142438,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +1077ms
  - evdev:
    - [  1, 149478,   4,   5,    7300] # EV_MSC / MSC_TIMESTAMP          7300
    - [  1, 149478,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  1, 156836,   4,   5,   14500] # EV_MSC / MSC_TIMESTAMP         14500
    - [  1, 156836,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  1, 164436,   4,   5,   21800] # EV_MSC / MSC_TIMESTAMP         21800
    - [  1, 164436,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  1, 171865,   4,   5,   29000] # EV_MSC / MSC_TIMESTAMP         29000
    - [  1, 171865,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  1, 178965,   4,   5,   36300] # EV_MSC / MSC_TIMESTAMP         36300
    - [  1, 178965,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  1, 186307,   4,   5,   43500] # EV_MSC / MSC_TIMESTAMP         43500
    - [  1, 186307,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  1, 193674,   4,   5,   50800] # EV_MSC / MSC_TIMESTAMP         50800
    - [  1, 193674,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  1, 201318,   4,   5,   58000] # EV_MSC / MSC_TIMESTAMP         58000
    - [  1, 201318,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  1, 207905,   3,  57,      -1] # EV_ABS / ABS_MT_TRACKING_ID       -1
    - [  1, 207905,   1, 330,       0] # EV_KEY / BTN_TOUCH                 0
    - [  1, 207905,   1, 325,       0] # EV_KEY / BTN_TOOL_FINGER           0
    - [  1, 207905,   4,   5,   65300] # EV_MSC / MSC_TIMESTAMP         65300
    - [  1, 207905,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +6ms
                                       # Touch device in neutral state
  - evdev:
    - [  2, 122656,   3,  57,    2196] # EV_ABS / ABS_MT_TRACKING_ID     2196
    - [  2, 122656,   3,  53,    1014] # EV_ABS / ABS_MT_POSITION_X      1014 (+415)
    - [  2, 122656,   3,  54,     317] # EV_ABS / ABS_MT_POSITION_Y       317 (-10)
    - [  2, 122656,   1, 330,       1] # EV_KEY / BTN_TOUCH                 1
    - [  2, 122656,   1, 325,       1] # EV_KEY / BTN_TOOL_FINGER           1
    - [  2, 122656,   3,   0,    1014] # EV_ABS / ABS_X                  1014 (+415)
    - [  2, 122656,   3,   1,     317] # EV_ABS / ABS_Y                   317 (-10)
    - [  2, 122656,   4,   5,  964500] # EV_MSC / MSC_TIMESTAMP        964500
    - [  2, 122656,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +915ms
  - evdev:
    - [  2, 129714,   4,   5,  971700] # EV_MSC / MSC_TIMESTAMP        971700
    - [  2, 129714,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  2, 137054,   4,   5,  979000] # EV_MSC / MSC_TIMESTAMP        979000
    - [  2, 137054,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  2, 144485,   4,   5,  986200] # EV_MSC / MSC_TIMESTAMP        986200
    - [  2, 144485,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  2, 151897,   4,   5,  993500] # EV_MSC / MSC_TIMESTAMP        993500
    - [  2, 151897,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  2, 159283,   4,   5, 1000700] # EV_MSC / MSC_TIMESTAMP        1000700
    - [  2, 159283,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  2, 166564,   4,   5, 1008000] # EV_MSC / MSC_TIMESTAMP        1008000
    - [  2, 166564,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  2, 173922,   4,   5, 1015300] # EV_MSC / MSC_TIMESTAMP        1015300
    - [  2, 173922,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  2, 181271,   4,   5, 1022500] # EV_MSC / MSC_TIMESTAMP        1022500
    - [  2, 181271,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  2, 188657,   4,   5, 1029800] # EV_MSC / MSC_TIMESTAMP        1029800
    - [  2, 188657,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  2, 195103,   3,  57,      -1] # EV_ABS / ABS_MT_TRACKING_ID       -1
    - [  2, 195103,   1, 330,       0] # EV_KEY / BTN_TOUCH                 0
    - [  2, 195103,   1, 325,       0] # EV_KEY / BTN_TOOL_FINGER           0
    - [  2, 195103,   4,   5, 1037000] # EV_MSC / MSC_TIMESTAMP        1037000
    - [  2, 195103,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
                                       # Touch device in neutral state
  # Current time is 14:51:20
  - evdev:
    - [  3, 294484,   3,  57,    2197] # EV_ABS / ABS_MT_TRACKING_ID     2197
    - [  3, 294484,   3,  53,    1006] # EV_ABS / ABS_MT_POSITION_X      1006 (-8)
    - [  3, 294484,   3,  54,     514] # EV_ABS / ABS_MT_POSITION_Y       514 (+197)
    - [  3, 294484,   1, 330,       1] # EV_KEY / BTN_TOUCH                 1
    - [  3, 294484,   1, 325,       1] # EV_KEY / BTN_TOOL_FINGER           1
    - [  3, 294484,   3,   0,    1006] # EV_ABS / ABS_X                  1006 (-8)
    - [  3, 294484,   3,   1,     514] # EV_ABS / ABS_Y                   514 (+197)
    - [  3, 294484,   4,   5,       0] # EV_MSC / MSC_TIMESTAMP             0
    - [  3, 294484,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +1099ms
  - evdev:
    - [  3, 301729,   4,   5,    7200] # EV_MSC / MSC_TIMESTAMP          7200
    - [  3, 301729,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  3, 309028,   4,   5,   14500] # EV_MSC / MSC_TIMESTAMP         14500
    - [  3, 309028,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  3, 316439,   4,   5,   21800] # EV_MSC / MSC_TIMESTAMP         21800
    - [  3, 316439,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  3, 323776,   4,   5,   29000] # EV_MSC / MSC_TIMESTAMP         29000
    - [  3, 323776,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  3, 331103,   4,   5,   36300] # EV_MSC / MSC_TIMESTAMP         36300
    - [  3, 331103,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  3, 338700,   4,   5,   43500] # EV_MSC / MSC_TIMESTAMP         43500
    - [  3, 338700,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  3, 345836,   4,   5,   50800] # EV_MSC / MSC_TIMESTAMP         50800
    - [  3, 345836,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  3, 353162,   4,   5,   58000] # EV_MSC / MSC_TIMESTAMP         58000
    - [  3, 353162,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  3, 359653,   3,  57,      -1] # EV_ABS / ABS_MT_TRACKING_ID       -1
    - [  3, 359653,   1, 330,       0] # EV_KEY / BTN_TOUCH                 0
    - [  3, 359653,   1, 325,       0] # EV_KEY / BTN_TOOL_FINGER           0
    - [  3, 359653,   4,   5,   65300] # EV_MSC / MSC_TIMESTAMP         65300
    - [  3, 359653,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +6ms
                                       # Touch device in neutral state
  - evdev:
    - [  4, 282187,   3,  57,    2198] # EV_ABS / ABS_MT_TRACKING_ID     2198
    - [  4, 282187,   3,  53,     996] # EV_ABS / ABS_MT_POSITION_X       996 (-10)
    - [  4, 282187,   3,  54,     686] # EV_ABS / ABS_MT_POSITION_Y       686 (+172)
    - [  4, 282187,   1, 330,       1] # EV_KEY / BTN_TOUCH                 1
    - [  4, 282187,   1, 325,       1] # EV_KEY / BTN_TOOL_FINGER           1
    - [  4, 282187,   3,   0,     996] # EV_ABS / ABS_X                   996 (-10)
    - [  4, 282187,   3,   1,     686] # EV_ABS / ABS_Y                   686 (+172)
    - [  4, 282187,   4,   5,  971900] # EV_MSC / MSC_TIMESTAMP        971900
    - [  4, 282187,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +923ms
  - evdev:
    - [  4, 289268,   4,   5,  979100] # EV_MSC / MSC_TIMESTAMP        979100
    - [  4, 289268,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  4, 296648,   4,   5,  986400] # EV_MSC / MSC_TIMESTAMP        986400
    - [  4, 296648,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  4, 304019,   4,   5,  993600] # EV_MSC / MSC_TIMESTAMP        993600
    - [  4, 304019,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  4, 311470,   4,   5, 1000900] # EV_MSC / MSC_TIMESTAMP        1000900
    - [  4, 311470,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  4, 318702,   4,   5, 1008100] # EV_MSC / MSC_TIMESTAMP        1008100
    - [  4, 318702,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  4, 326099,   4,   5, 1015400] # EV_MSC / MSC_TIMESTAMP        1015400
    - [  4, 326099,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  4, 333468,   4,   5, 1022600] # EV_MSC / MSC_TIMESTAMP        1022600
    - [  4, 333468,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  4, 340826,   4,   5, 1029900] # EV_MSC / MSC_TIMESTAMP        1029900
    - [  4, 340826,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  4, 348193,   4,   5, 1037100] # EV_MSC / MSC_TIMESTAMP        1037100
    - [  4, 348193,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  4, 355566,   4,   5, 1044400] # EV_MSC / MSC_TIMESTAMP        1044400
    - [  4, 355566,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  4, 362926,   4,   5, 1051700] # EV_MSC / MSC_TIMESTAMP        1051700
    - [  4, 362926,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  4, 370249,   4,   5, 1058900] # EV_MSC / MSC_TIMESTAMP        1058900
    - [  4, 370249,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  4, 376803,   3,  57,      -1] # EV_ABS / ABS_MT_TRACKING_ID       -1
    - [  4, 376803,   1, 330,       0] # EV_KEY / BTN_TOUCH                 0
    - [  4, 376803,   1, 325,       0] # EV_KEY / BTN_TOOL_FINGER           0
    - [  4, 376803,   4,   5, 1066200] # EV_MSC / MSC_TIMESTAMP        1066200
    - [  4, 376803,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +6ms
                                       # Touch device in neutral state
  - evdev:
    - [  4, 952872,   3,  57,    2199] # EV_ABS / ABS_MT_TRACKING_ID     2199
    - [  4, 952872,   3,  53,     993] # EV_ABS / ABS_MT_POSITION_X       993 (-3)
    - [  4, 952872,   3,  54,     691] # EV_ABS / ABS_MT_POSITION_Y       691 (+5)
    - [  4, 952872,   1, 330,       1] # EV_KEY / BTN_TOUCH                 1
    - [  4, 952872,   1, 325,       1] # EV_KEY / BTN_TOOL_FINGER           1
    - [  4, 952872,   3,   0,     993] # EV_ABS / ABS_X                   993 (-3)
    - [  4, 952872,   3,   1,     691] # EV_ABS / ABS_Y                   691 (+5)
    - [  4, 952872,   4,   5, 1631700] # EV_MSC / MSC_TIMESTAMP        1631700
    - [  4, 952872,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +576ms
  - evdev:
    - [  4, 959964,   4,   5, 1638900] # EV_MSC / MSC_TIMESTAMP        1638900
    - [  4, 959964,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  4, 967334,   4,   5, 1646200] # EV_MSC / MSC_TIMESTAMP        1646200
    - [  4, 967334,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  4, 974682,   4,   5, 1653400] # EV_MSC / MSC_TIMESTAMP        1653400
    - [  4, 974682,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  4, 982049,   4,   5, 1660700] # EV_MSC / MSC_TIMESTAMP        1660700
    - [  4, 982049,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +8ms
  - evdev:
    - [  4, 989400,   4,   5, 1667900] # EV_MSC / MSC_TIMESTAMP        1667900
    - [  4, 989400,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  4, 996687,   4,   5, 1675200] # EV_MSC / MSC_TIMESTAMP        1675200
    - [  4, 996687,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
  - evdev:
    - [  5,   3207,   3,  57,      -1] # EV_ABS / ABS_MT_TRACKING_ID       -1
    - [  5,   3207,   1, 330,       0] # EV_KEY / BTN_TOUCH                 0
    - [  5,   3207,   1, 325,       0] # EV_KEY / BTN_TOOL_FINGER           0
    - [  5,   3207,   4,   5, 1682400] # EV_MSC / MSC_TIMESTAMP        1682400
    - [  5,   3207,   0,   0,       0] # ------------ SYN_REPORT (0) ---------- +7ms
                                       # Touch device in neutral state

```
</details>

## debug-event

`libinput debug-events` 根据 new bing 的回答得到是会有位置坐标，然而实际测试并没有得到坐标

<details>
<summary>测试输出</summary>

```bash
$ libinput debug-events
-event2   DEVICE_ADDED            Power Button                      seat0 default group1  cap:k
-event4   DEVICE_ADDED            Video Bus                         seat0 default group2  cap:k
-event1   DEVICE_ADDED            Power Button                      seat0 default group3  cap:k
-event0   DEVICE_ADDED            Lid Switch                        seat0 default group4  cap:S
-event8   DEVICE_ADDED            Integrated Camera: Integrated C   seat0 default group5  cap:k
-event9   DEVICE_ADDED            Integrated Camera: Integrated I   seat0 default group5  cap:k
-event7   DEVICE_ADDED            Ideapad extra buttons             seat0 default group6  cap:k
-event5   DEVICE_ADDED            MSFT0001:00 06CB:CD3E Mouse       seat0 default group7  cap:p left scroll-nat scroll-button
-event6   DEVICE_ADDED            MSFT0001:00 06CB:CD3E Touchpad    seat0 default group7  cap:pg  size 101x66mm tap(dl off) left scroll-nat scroll-2fg-edge click-buttonareas-clickfinger dwt-on dwtp-on
-event3   DEVICE_ADDED            AT Translated Set 2 keyboard      seat0 default group8  cap:k
-event6   GESTURE_HOLD_BEGIN      +0.040s       1
 event6   GESTURE_HOLD_END        +0.073s       1
 event6   GESTURE_HOLD_BEGIN      +1.455s       1
 event6   GESTURE_HOLD_END        +1.539s       1
 event6   GESTURE_HOLD_BEGIN      +2.974s       1
 event6   GESTURE_HOLD_END        +2.992s       1
-event3   KEYBOARD_KEY            +4.775s       *** (-1) pressed
 event3   KEYBOARD_KEY            +4.970s       *** (-1) pressed
^C
$ sudo libinput debug-events
-event2   DEVICE_ADDED            Power Button                      seat0 default group1  cap:k
-event4   DEVICE_ADDED            Video Bus                         seat0 default group2  cap:k
-event1   DEVICE_ADDED            Power Button                      seat0 default group3  cap:k
-event0   DEVICE_ADDED            Lid Switch                        seat0 default group4  cap:S
-event8   DEVICE_ADDED            Integrated Camera: Integrated C   seat0 default group5  cap:k
-event9   DEVICE_ADDED            Integrated Camera: Integrated I   seat0 default group5  cap:k
-event7   DEVICE_ADDED            Ideapad extra buttons             seat0 default group6  cap:k
-event5   DEVICE_ADDED            MSFT0001:00 06CB:CD3E Mouse       seat0 default group7  cap:p left scroll-nat scroll-button
-event6   DEVICE_ADDED            MSFT0001:00 06CB:CD3E Touchpad    seat0 default group7  cap:pg  size 101x66mm tap(dl off) left scroll-nat scroll-2fg-edge click-buttonareas-clickfinger dwt-on dwtp-on
-event3   DEVICE_ADDED            AT Translated Set 2 keyboard      seat0 default group8  cap:k
-event6   GESTURE_HOLD_BEGIN      +0.040s       1
 event6   GESTURE_HOLD_END        +0.072s       1
 event6   GESTURE_HOLD_BEGIN      +0.688s       1
 event6   GESTURE_HOLD_END        +0.706s       1
 event6   GESTURE_HOLD_BEGIN      +1.278s       1
 event6   GESTURE_HOLD_END        +1.333s       1
 event6   GESTURE_HOLD_BEGIN      +1.993s       1
 event6   GESTURE_HOLD_END        +2.048s       1
 event6   GESTURE_HOLD_BEGIN      +2.568s       1
 event6   GESTURE_HOLD_END        +2.571s       1
-event3   KEYBOARD_KEY            +3.476s       *** (-1) pressed
 event3   KEYBOARD_KEY            +3.740s       *** (-1) pressed
^C
$ sudo libinput debug-events --verbose
libinput version: 1.22.1
event2  - Power Button: is tagged by udev as: Keyboard
event2  - Power Button: device is a keyboard
event4  - Video Bus: is tagged by udev as: Keyboard
event4  - Video Bus: device is a keyboard
event1  - Power Button: is tagged by udev as: Keyboard
event1  - Power Button: device is a keyboard
event0  - Lid Switch: is tagged by udev as: Switch
event0  - Lid Switch: device is a switch device
event10 - HD-Audio Generic HDMI/DP,pcm=3: is tagged by udev as: Switch
event10 - not using input device '/dev/input/event10'
event11 - HD-Audio Generic HDMI/DP,pcm=7: is tagged by udev as: Switch
event11 - not using input device '/dev/input/event11'
event8  - Integrated Camera: Integrated C: is tagged by udev as: Keyboard
event8  - Integrated Camera: Integrated C: device is a keyboard
event9  - Integrated Camera: Integrated I: is tagged by udev as: Keyboard
event9  - Integrated Camera: Integrated I: device is a keyboard
event12 - HD-Audio Generic Mic: is tagged by udev as: Switch
event12 - not using input device '/dev/input/event12'
event13 - HD-Audio Generic Headphone: is tagged by udev as: Switch
event13 - not using input device '/dev/input/event13'
event7  - Ideapad extra buttons: is tagged by udev as: Keyboard
event7  - Ideapad extra buttons: device is a keyboard
event5  - MSFT0001:00 06CB:CD3E Mouse: is tagged by udev as: Mouse Pointingstick
event5  - MSFT0001:00 06CB:CD3E Mouse: device is a pointer
event6  - MSFT0001:00 06CB:CD3E Touchpad: is tagged by udev as: Touchpad
  ... event6  - thumb: enabled thumb detection (area)
event6  - MSFT0001:00 06CB:CD3E Touchpad: device is a touchpad
  ... event6  - lid: activated for MSFT0001:00 06CB:CD3E Touchpad<->Lid Switch
event3  - AT Translated Set 2 keyboard: is tagged by udev as: Keyboard
event3  - AT Translated Set 2 keyboard: device is a keyboard
  ... event0  - lid: keyboard paired with Lid Switch<->AT Translated Set 2 keyboard
  ... event6  - palm: dwt activated with MSFT0001:00 06CB:CD3E Touchpad<->AT Translated Set 2 keyboard
-event2   DEVICE_ADDED            Power Button                      seat0 default group1  cap:k
-event4   DEVICE_ADDED            Video Bus                         seat0 default group2  cap:k
-event1   DEVICE_ADDED            Power Button                      seat0 default group3  cap:k
-event0   DEVICE_ADDED            Lid Switch                        seat0 default group4  cap:S
-event8   DEVICE_ADDED            Integrated Camera: Integrated C   seat0 default group5  cap:k
-event9   DEVICE_ADDED            Integrated Camera: Integrated I   seat0 default group5  cap:k
-event7   DEVICE_ADDED            Ideapad extra buttons             seat0 default group6  cap:k
-event5   DEVICE_ADDED            MSFT0001:00 06CB:CD3E Mouse       seat0 default group7  cap:p left scroll-nat scroll-button
-event6   DEVICE_ADDED            MSFT0001:00 06CB:CD3E Touchpad    seat0 default group7  cap:pg  size 101x66mm tap(dl off) left scroll-nat scroll-2fg-edge click-buttonareas-clickfinger dwt-on dwtp-on
-event3   DEVICE_ADDED            AT Translated Set 2 keyboard      seat0 default group8  cap:k
   2: event6  - button state: touch 0 from BUTTON_STATE_NONE    event BUTTON_EVENT_IN_AREA     to BUTTON_STATE_AREA   
  ... event6  - gesture state GESTURE_STATE_NONE → GESTURE_EVENT_FINGER_DETECTED → GESTURE_STATE_UNKNOWN
   7: event6  - button state: touch 0 from BUTTON_STATE_AREA    event BUTTON_EVENT_UP          to BUTTON_STATE_NONE   
  ... event6  - gesture state GESTURE_STATE_UNKNOWN → GESTURE_EVENT_RESET → GESTURE_STATE_NONE
   8: event6  - button state: touch 0 from BUTTON_STATE_NONE    event BUTTON_EVENT_IN_AREA     to BUTTON_STATE_AREA   
  ... event6  - gesture state GESTURE_STATE_NONE → GESTURE_EVENT_FINGER_DETECTED → GESTURE_STATE_UNKNOWN
  12: event6  - button state: touch 0 from BUTTON_STATE_AREA    event BUTTON_EVENT_UP          to BUTTON_STATE_NONE   
  ... event6  - gesture state GESTURE_STATE_UNKNOWN → GESTURE_EVENT_RESET → GESTURE_STATE_NONE
  13: event6  - button state: touch 0 from BUTTON_STATE_NONE    event BUTTON_EVENT_IN_AREA     to BUTTON_STATE_AREA   
  ... event6  - gesture state GESTURE_STATE_NONE → GESTURE_EVENT_FINGER_DETECTED → GESTURE_STATE_UNKNOWN
  19: event6  - gesture state GESTURE_STATE_UNKNOWN → GESTURE_EVENT_HOLD_TIMEOUT → GESTURE_STATE_HOLD
-event6   GESTURE_HOLD_BEGIN      +3.704s       1
  21: event6  - button state: touch 0 from BUTTON_STATE_AREA    event BUTTON_EVENT_UP          to BUTTON_STATE_NONE   
  ... event6  - gesture state GESTURE_STATE_HOLD → GESTURE_EVENT_RESET → GESTURE_STATE_NONE
 event6   GESTURE_HOLD_END        +3.714s       1
  22: event6  - button state: touch 0 from BUTTON_STATE_NONE    event BUTTON_EVENT_IN_AREA     to BUTTON_STATE_AREA   
  ... event6  - gesture state GESTURE_STATE_NONE → GESTURE_EVENT_FINGER_DETECTED → GESTURE_STATE_UNKNOWN
  28: event6  - gesture state GESTURE_STATE_UNKNOWN → GESTURE_EVENT_HOLD_TIMEOUT → GESTURE_STATE_HOLD
 event6   GESTURE_HOLD_BEGIN      +5.355s       1
  29: event6  - button state: touch 0 from BUTTON_STATE_AREA    event BUTTON_EVENT_UP          to BUTTON_STATE_NONE   
  ... event6  - gesture state GESTURE_STATE_HOLD → GESTURE_EVENT_RESET → GESTURE_STATE_NONE
 event6   GESTURE_HOLD_END        +5.358s       1
  30: event6  - button state: touch 0 from BUTTON_STATE_NONE    event BUTTON_EVENT_IN_AREA     to BUTTON_STATE_AREA   
  ... event6  - gesture state GESTURE_STATE_NONE → GESTURE_EVENT_FINGER_DETECTED → GESTURE_STATE_UNKNOWN
  36: event6  - gesture state GESTURE_STATE_UNKNOWN → GESTURE_EVENT_HOLD_TIMEOUT → GESTURE_STATE_HOLD
 event6   GESTURE_HOLD_BEGIN      +6.224s       1
  41: event6  - button state: touch 0 from BUTTON_STATE_AREA    event BUTTON_EVENT_UP          to BUTTON_STATE_NONE   
  ... event6  - gesture state GESTURE_STATE_HOLD → GESTURE_EVENT_RESET → GESTURE_STATE_NONE
 event6   GESTURE_HOLD_END        +6.257s       1
-event3   KEYBOARD_KEY            +7.228s       *** (-1) pressed
 event3   KEYBOARD_KEY            +7.450s       *** (-1) pressed
^C
event2  - Power Button: device removed
event4  - Video Bus: device removed
event1  - Power Button: device removed
event0  - Lid Switch: device removed
event8  - Integrated Camera: Integrated C: device removed
event9  - Integrated Camera: Integrated I: device removed
event7  - Ideapad extra buttons: device removed
event5  - MSFT0001:00 06CB:CD3E Mouse: device removed
event6  - MSFT0001:00 06CB:CD3E Touchpad: device removed
event3  - AT Translated Set 2 keyboard: device removed
```
</details>

# evtest

先按三个不同的位置，然后按三次相同的位置，第一眼没有观察到坐标属性，后面发现是有的，比如 ABS_X

<details>
<summary>测试输出</summary>

```
$ evtest /dev/input/event6
Input driver version is 1.0.1
Input device ID: bus 0x18 vendor 0x6cb product 0xcd3e version 0x100
Input device name: "MSFT0001:00 06CB:CD3E Touchpad"
Supported events:
  Event type 0 (EV_SYN)
  Event type 1 (EV_KEY)
    Event code 272 (BTN_LEFT)
    Event code 325 (BTN_TOOL_FINGER)
    Event code 328 (BTN_TOOL_QUINTTAP)
    Event code 330 (BTN_TOUCH)
    Event code 333 (BTN_TOOL_DOUBLETAP)
    Event code 334 (BTN_TOOL_TRIPLETAP)
    Event code 335 (BTN_TOOL_QUADTAP)
  Event type 3 (EV_ABS)
    Event code 0 (ABS_X)
      Value    564
      Min        0
      Max     1212
      Resolution      12
    Event code 1 (ABS_Y)
      Value    334
      Min        0
      Max      792
      Resolution      12
    Event code 47 (ABS_MT_SLOT)
      Value      0
      Min        0
      Max        4
    Event code 53 (ABS_MT_POSITION_X)
      Value      0
      Min        0
      Max     1212
      Resolution      12
    Event code 54 (ABS_MT_POSITION_Y)
      Value      0
      Min        0
      Max      792
      Resolution      12
    Event code 55 (ABS_MT_TOOL_TYPE)
      Value      0
      Min        0
      Max        2
    Event code 57 (ABS_MT_TRACKING_ID)
      Value      0
      Min        0
      Max    65535
  Event type 4 (EV_MSC)
    Event code 5 (MSC_TIMESTAMP)
Properties:
  Property type 0 (INPUT_PROP_POINTER)
  Property type 2 (INPUT_PROP_BUTTONPAD)
Testing ... (interrupt to exit)
Event: time 1678690001.311168, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value 1982
Event: time 1678690001.311168, type 3 (EV_ABS), code 53 (ABS_MT_POSITION_X), value 200
Event: time 1678690001.311168, type 3 (EV_ABS), code 54 (ABS_MT_POSITION_Y), value 207
Event: time 1678690001.311168, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 1
Event: time 1678690001.311168, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 1
Event: time 1678690001.311168, type 3 (EV_ABS), code 0 (ABS_X), value 200
Event: time 1678690001.311168, type 3 (EV_ABS), code 1 (ABS_Y), value 207
Event: time 1678690001.311168, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 0
Event: time 1678690001.311168, -------------- SYN_REPORT ------------
Event: time 1678690001.317604, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value -1
Event: time 1678690001.317604, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 0
Event: time 1678690001.317604, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 0
Event: time 1678690001.317604, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 7300
Event: time 1678690001.317604, -------------- SYN_REPORT ------------
Event: time 1678690004.908470, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value 1983
Event: time 1678690004.908470, type 3 (EV_ABS), code 53 (ABS_MT_POSITION_X), value 674
Event: time 1678690004.908470, type 3 (EV_ABS), code 54 (ABS_MT_POSITION_Y), value 186
Event: time 1678690004.908470, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 1
Event: time 1678690004.908470, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 1
Event: time 1678690004.908470, type 3 (EV_ABS), code 0 (ABS_X), value 674
Event: time 1678690004.908470, type 3 (EV_ABS), code 1 (ABS_Y), value 186
Event: time 1678690004.908470, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 0
Event: time 1678690004.908470, -------------- SYN_REPORT ------------
Event: time 1678690004.915507, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 7300
Event: time 1678690004.915507, -------------- SYN_REPORT ------------
Event: time 1678690004.923088, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 14500
Event: time 1678690004.923088, -------------- SYN_REPORT ------------
Event: time 1678690004.930190, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 21800
Event: time 1678690004.930190, -------------- SYN_REPORT ------------
Event: time 1678690004.936770, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value -1
Event: time 1678690004.936770, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 0
Event: time 1678690004.936770, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 0
Event: time 1678690004.936770, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 29000
Event: time 1678690004.936770, -------------- SYN_REPORT ------------
Event: time 1678690007.060731, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value 1984
Event: time 1678690007.060731, type 3 (EV_ABS), code 53 (ABS_MT_POSITION_X), value 1123
Event: time 1678690007.060731, type 3 (EV_ABS), code 54 (ABS_MT_POSITION_Y), value 181
Event: time 1678690007.060731, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 1
Event: time 1678690007.060731, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 1
Event: time 1678690007.060731, type 3 (EV_ABS), code 0 (ABS_X), value 1123
Event: time 1678690007.060731, type 3 (EV_ABS), code 1 (ABS_Y), value 181
Event: time 1678690007.060731, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 0
Event: time 1678690007.060731, -------------- SYN_REPORT ------------
Event: time 1678690007.068014, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 7300
Event: time 1678690007.068014, -------------- SYN_REPORT ------------
Event: time 1678690007.075405, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 14500
Event: time 1678690007.075405, -------------- SYN_REPORT ------------
Event: time 1678690007.082724, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 21800
Event: time 1678690007.082724, -------------- SYN_REPORT ------------
Event: time 1678690007.089200, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value -1
Event: time 1678690007.089200, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 0
Event: time 1678690007.089200, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 0
Event: time 1678690007.089200, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 29000
Event: time 1678690007.089200, -------------- SYN_REPORT ------------
Event: time 1678690010.193594, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value 1985
Event: time 1678690010.193594, type 3 (EV_ABS), code 53 (ABS_MT_POSITION_X), value 579
Event: time 1678690010.193594, type 3 (EV_ABS), code 54 (ABS_MT_POSITION_Y), value 437
Event: time 1678690010.193594, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 1
Event: time 1678690010.193594, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 1
Event: time 1678690010.193594, type 3 (EV_ABS), code 0 (ABS_X), value 579
Event: time 1678690010.193594, type 3 (EV_ABS), code 1 (ABS_Y), value 437
Event: time 1678690010.193594, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 0
Event: time 1678690010.193594, -------------- SYN_REPORT ------------
Event: time 1678690010.201035, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 7200
Event: time 1678690010.201035, -------------- SYN_REPORT ------------
Event: time 1678690010.208094, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 14500
Event: time 1678690010.208094, -------------- SYN_REPORT ------------
Event: time 1678690010.215464, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 21700
Event: time 1678690010.215464, -------------- SYN_REPORT ------------
Event: time 1678690010.222820, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 29000
Event: time 1678690010.222820, -------------- SYN_REPORT ------------
Event: time 1678690010.230187, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 36200
Event: time 1678690010.230187, -------------- SYN_REPORT ------------
Event: time 1678690010.237686, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 43500
Event: time 1678690010.237686, -------------- SYN_REPORT ------------
Event: time 1678690010.244153, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value -1
Event: time 1678690010.244153, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 0
Event: time 1678690010.244153, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 0
Event: time 1678690010.244153, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 50700
Event: time 1678690010.244153, -------------- SYN_REPORT ------------
Event: time 1678690010.856944, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value 1986
Event: time 1678690010.856944, type 3 (EV_ABS), code 53 (ABS_MT_POSITION_X), value 575
Event: time 1678690010.856944, type 3 (EV_ABS), code 54 (ABS_MT_POSITION_Y), value 442
Event: time 1678690010.856944, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 1
Event: time 1678690010.856944, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 1
Event: time 1678690010.856944, type 3 (EV_ABS), code 0 (ABS_X), value 575
Event: time 1678690010.856944, type 3 (EV_ABS), code 1 (ABS_Y), value 442
Event: time 1678690010.856944, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 652700
Event: time 1678690010.856944, -------------- SYN_REPORT ------------
Event: time 1678690010.864117, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 660000
Event: time 1678690010.864117, -------------- SYN_REPORT ------------
Event: time 1678690010.871476, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 667200
Event: time 1678690010.871476, -------------- SYN_REPORT ------------
Event: time 1678690010.878856, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 674500
Event: time 1678690010.878856, -------------- SYN_REPORT ------------
Event: time 1678690010.886227, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 681700
Event: time 1678690010.886227, -------------- SYN_REPORT ------------
Event: time 1678690010.893591, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 689000
Event: time 1678690010.893591, -------------- SYN_REPORT ------------
Event: time 1678690010.900927, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 696200
Event: time 1678690010.900927, -------------- SYN_REPORT ------------
Event: time 1678690010.908398, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 703500
Event: time 1678690010.908398, -------------- SYN_REPORT ------------
Event: time 1678690010.914988, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value -1
Event: time 1678690010.914988, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 0
Event: time 1678690010.914988, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 0
Event: time 1678690010.914988, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 710700
Event: time 1678690010.914988, -------------- SYN_REPORT ------------
Event: time 1678690011.505473, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value 1987
Event: time 1678690011.505473, type 3 (EV_ABS), code 53 (ABS_MT_POSITION_X), value 577
Event: time 1678690011.505473, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 1
Event: time 1678690011.505473, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 1
Event: time 1678690011.505473, type 3 (EV_ABS), code 0 (ABS_X), value 577
Event: time 1678690011.505473, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 1290900
Event: time 1678690011.505473, -------------- SYN_REPORT ------------
Event: time 1678690011.512765, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 1298100
Event: time 1678690011.512765, -------------- SYN_REPORT ------------
Event: time 1678690011.520144, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 1305400
Event: time 1678690011.520144, -------------- SYN_REPORT ------------
Event: time 1678690011.527571, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 1312600
Event: time 1678690011.527571, -------------- SYN_REPORT ------------
Event: time 1678690011.534870, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 1319900
Event: time 1678690011.534870, -------------- SYN_REPORT ------------
Event: time 1678690011.542251, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 1327100
Event: time 1678690011.542251, -------------- SYN_REPORT ------------
Event: time 1678690011.549710, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 1334400
Event: time 1678690011.549710, -------------- SYN_REPORT ------------
Event: time 1678690011.556398, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value -1
Event: time 1678690011.556398, type 1 (EV_KEY), code 330 (BTN_TOUCH), value 0
Event: time 1678690011.556398, type 1 (EV_KEY), code 325 (BTN_TOOL_FINGER), value 0
Event: time 1678690011.556398, type 4 (EV_MSC), code 5 (MSC_TIMESTAMP), value 1341600
Event: time 1678690011.556398, -------------- SYN_REPORT ------------
```
</details>

# python-evdev

目前正在手搓的 [touchpad-as-numpad](https://gitee.com/anidea/touchpad-as-numpad)

<details>
<summary>测试代码和结果</summary>

触摸板事件测试：横着按三个点，再竖着按两个点

事件输出五元组：`InputEvent(1678764538, 58050, 3, 57, 2084)` ，根据 API文档 显示第一个参数是时间，如果这个参数相同则表示这几个输出是同一个事件的输出；第二个参数是时间戳的微秒部分；第三个参数是事件大类；第四个参数是事件大类中的小类状态码；第五个参数是事件的值。

根据我在课余时间的测试表明，同一台笔记本电脑的触摸板在重启之后，触摸板对应的设备文件发生了变化，上一次还是 `/dev/input/event6`，关机饮茶先，下一次跑这个代码发现没有事件输出，发现是文件接口变成了 `/dev/input/event7`

根据输出观测到，一次按下抬起涉及到多个事件，仅EV_ABS、EV_KEY、EV_SYN加起来就有十多个，也就是说每种事件在一次按下抬起事件中都多次出现，从输出中可以观察到每次排在最后的都是 EV_SYN，并且EV_KEY事件中的value发生了翻转。

```python
import evdev
devices = [evdev.InputDevice(path) for path in evdev.list_devices()]
for device in devices:
    print(device.path, device.name, device.phys)
device = evdev.InputDevice('/dev/input/event7') # Touch Pad 对应文件
for e in device.read_loop():
    print(e)
```

```bash
$ python tmp.py 
/dev/input/event13 HD-Audio Generic Headphone ALSA
/dev/input/event12 HD-Audio Generic Mic ALSA
/dev/input/event11 HD-Audio Generic HDMI/DP,pcm=7 ALSA
/dev/input/event10 HD-Audio Generic HDMI/DP,pcm=3 ALSA
/dev/input/event9 Integrated Camera: Integrated I usb-0000:03:00.4-2/button
/dev/input/event8 Integrated Camera: Integrated C usb-0000:03:00.4-2/button
/dev/input/event7 MSFT0001:00 06CB:CD3E Touchpad i2c-MSFT0001:00
/dev/input/event6 MSFT0001:00 06CB:CD3E Mouse i2c-MSFT0001:00
/dev/input/event5 Ideapad extra buttons ideapad/input0
/dev/input/event4 Video Bus LNXVIDEO/video/input0
/dev/input/event3 AT Translated Set 2 keyboard isa0060/serio0/input0
/dev/input/event2 Power Button LNXPWRBN/button/input0
/dev/input/event1 Power Button PNP0C0C/button/input0
/dev/input/event0 Lid Switch PNP0C0D/button/input0
InputEvent(1678764538, 58050, 3, 57, 2084)
InputEvent(1678764538, 58050, 3, 53, 183)
InputEvent(1678764538, 58050, 3, 54, 138)
InputEvent(1678764538, 58050, 1, 330, 1)
InputEvent(1678764538, 58050, 1, 325, 1)
InputEvent(1678764538, 58050, 3, 0, 183)
InputEvent(1678764538, 58050, 3, 1, 138)
InputEvent(1678764538, 58050, 4, 5, 0)
InputEvent(1678764538, 58050, 0, 0, 0)
InputEvent(1678764538, 65297, 4, 5, 7300)
InputEvent(1678764538, 65297, 0, 0, 0)
InputEvent(1678764538, 72606, 4, 5, 14500)
InputEvent(1678764538, 72606, 0, 0, 0)
InputEvent(1678764538, 79902, 4, 5, 21800)
InputEvent(1678764538, 79902, 0, 0, 0)
InputEvent(1678764538, 87316, 4, 5, 29100)
InputEvent(1678764538, 87316, 0, 0, 0)
InputEvent(1678764538, 94668, 4, 5, 36300)
InputEvent(1678764538, 94668, 0, 0, 0)
InputEvent(1678764538, 102010, 4, 5, 43600)
InputEvent(1678764538, 102010, 0, 0, 0)
InputEvent(1678764538, 109435, 4, 5, 50800)
InputEvent(1678764538, 109435, 0, 0, 0)
InputEvent(1678764538, 116716, 4, 5, 58100)
InputEvent(1678764538, 116716, 0, 0, 0)
InputEvent(1678764538, 124391, 4, 5, 65400)
InputEvent(1678764538, 124391, 0, 0, 0)
InputEvent(1678764538, 130543, 3, 57, -1)
InputEvent(1678764538, 130543, 1, 330, 0)
InputEvent(1678764538, 130543, 1, 325, 0)
InputEvent(1678764538, 130543, 4, 5, 72600)
InputEvent(1678764538, 130543, 0, 0, 0)
InputEvent(1678764541, 256349, 3, 57, 2085)
InputEvent(1678764541, 256349, 3, 53, 580)
InputEvent(1678764541, 256349, 3, 54, 194)
InputEvent(1678764541, 256349, 1, 330, 1)
InputEvent(1678764541, 256349, 1, 325, 1)
InputEvent(1678764541, 256349, 3, 0, 580)
InputEvent(1678764541, 256349, 3, 1, 194)
InputEvent(1678764541, 256349, 4, 5, 0)
InputEvent(1678764541, 256349, 0, 0, 0)
InputEvent(1678764541, 263569, 4, 5, 7200)
InputEvent(1678764541, 263569, 0, 0, 0)
InputEvent(1678764541, 270754, 4, 5, 14500)
InputEvent(1678764541, 270754, 0, 0, 0)
InputEvent(1678764541, 278050, 4, 5, 21700)
InputEvent(1678764541, 278050, 0, 0, 0)
InputEvent(1678764541, 284789, 3, 57, -1)
InputEvent(1678764541, 284789, 1, 330, 0)
InputEvent(1678764541, 284789, 1, 325, 0)
InputEvent(1678764541, 284789, 4, 5, 29000)
InputEvent(1678764541, 284789, 0, 0, 0)
InputEvent(1678764542, 435468, 3, 57, 2086)
InputEvent(1678764542, 435468, 3, 53, 1038)
InputEvent(1678764542, 435468, 3, 54, 155)
InputEvent(1678764542, 435468, 1, 330, 1)
InputEvent(1678764542, 435468, 1, 325, 1)
InputEvent(1678764542, 435468, 3, 0, 1038)
InputEvent(1678764542, 435468, 3, 1, 155)
InputEvent(1678764542, 435468, 4, 5, 0)
InputEvent(1678764542, 435468, 0, 0, 0)
InputEvent(1678764542, 442546, 4, 5, 7300)
InputEvent(1678764542, 442546, 0, 0, 0)
InputEvent(1678764542, 449913, 4, 5, 14500)
InputEvent(1678764542, 449913, 0, 0, 0)
InputEvent(1678764542, 457348, 4, 5, 21800)
InputEvent(1678764542, 457348, 0, 0, 0)
InputEvent(1678764542, 464818, 4, 5, 29100)
InputEvent(1678764542, 464818, 0, 0, 0)
InputEvent(1678764542, 472036, 4, 5, 36300)
InputEvent(1678764542, 472036, 0, 0, 0)
InputEvent(1678764542, 479370, 4, 5, 43600)
InputEvent(1678764542, 479370, 0, 0, 0)
InputEvent(1678764542, 487090, 4, 5, 50800)
InputEvent(1678764542, 487090, 0, 0, 0)
InputEvent(1678764542, 494261, 4, 5, 58100)
InputEvent(1678764542, 494261, 0, 0, 0)
InputEvent(1678764542, 501470, 4, 5, 65400)
InputEvent(1678764542, 501470, 0, 0, 0)
InputEvent(1678764542, 507930, 3, 57, -1)
InputEvent(1678764542, 507930, 1, 330, 0)
InputEvent(1678764542, 507930, 1, 325, 0)
InputEvent(1678764542, 507930, 4, 5, 72600)
InputEvent(1678764542, 507930, 0, 0, 0)
InputEvent(1678764543, 201962, 3, 57, 2087)
InputEvent(1678764543, 201962, 3, 53, 1043)
InputEvent(1678764543, 201962, 3, 54, 400)
InputEvent(1678764543, 201962, 1, 330, 1)
InputEvent(1678764543, 201962, 1, 325, 1)
InputEvent(1678764543, 201962, 3, 0, 1043)
InputEvent(1678764543, 201962, 3, 1, 400)
InputEvent(1678764543, 201962, 4, 5, 755300)
InputEvent(1678764543, 201962, 0, 0, 0)
InputEvent(1678764543, 209012, 4, 5, 762600)
InputEvent(1678764543, 209012, 0, 0, 0)
InputEvent(1678764543, 216364, 4, 5, 769800)
InputEvent(1678764543, 216364, 0, 0, 0)
InputEvent(1678764543, 223763, 4, 5, 777100)
InputEvent(1678764543, 223763, 0, 0, 0)
InputEvent(1678764543, 231299, 4, 5, 784300)
InputEvent(1678764543, 231299, 0, 0, 0)
InputEvent(1678764543, 238487, 4, 5, 791600)
InputEvent(1678764543, 238487, 0, 0, 0)
InputEvent(1678764543, 245866, 4, 5, 798900)
InputEvent(1678764543, 245866, 0, 0, 0)
InputEvent(1678764543, 253244, 4, 5, 806100)
InputEvent(1678764543, 253244, 0, 0, 0)
InputEvent(1678764543, 260502, 4, 5, 813400)
InputEvent(1678764543, 260502, 0, 0, 0)
InputEvent(1678764543, 267065, 3, 57, -1)
InputEvent(1678764543, 267065, 1, 330, 0)
InputEvent(1678764543, 267065, 1, 325, 0)
InputEvent(1678764543, 267065, 4, 5, 820700)
InputEvent(1678764543, 267065, 0, 0, 0)
InputEvent(1678764544, 64128, 3, 57, 2088)
InputEvent(1678764544, 64128, 3, 53, 1057)
InputEvent(1678764544, 64128, 3, 54, 680)
InputEvent(1678764544, 64128, 1, 330, 1)
InputEvent(1678764544, 64128, 1, 325, 1)
InputEvent(1678764544, 64128, 3, 0, 1057)
InputEvent(1678764544, 64128, 3, 1, 680)
InputEvent(1678764544, 64128, 4, 5, 1604900)
InputEvent(1678764544, 64128, 0, 0, 0)
InputEvent(1678764544, 71255, 4, 5, 1612200)
InputEvent(1678764544, 71255, 0, 0, 0)
InputEvent(1678764544, 78623, 4, 5, 1619400)
InputEvent(1678764544, 78623, 0, 0, 0)
InputEvent(1678764544, 85993, 4, 5, 1626700)
InputEvent(1678764544, 85993, 0, 0, 0)
InputEvent(1678764544, 93360, 4, 5, 1633900)
InputEvent(1678764544, 93360, 0, 0, 0)
InputEvent(1678764544, 101039, 4, 5, 1641200)
InputEvent(1678764544, 101039, 0, 0, 0)
InputEvent(1678764544, 107560, 3, 57, -1)
InputEvent(1678764544, 107560, 1, 330, 0)
InputEvent(1678764544, 107560, 1, 325, 0)
InputEvent(1678764544, 107560, 4, 5, 1648500)
InputEvent(1678764544, 107560, 0, 0, 0)
InputEvent(1678764544, 447374, 3, 57, 2089)
InputEvent(1678764544, 447374, 3, 54, 681)
InputEvent(1678764544, 447374, 1, 330, 1)
InputEvent(1678764544, 447374, 1, 325, 1)
InputEvent(1678764544, 447374, 3, 1, 681)
InputEvent(1678764544, 447374, 4, 5, 1982300)
InputEvent(1678764544, 447374, 0, 0, 0)
InputEvent(1678764544, 454504, 4, 5, 1989500)
InputEvent(1678764544, 454504, 0, 0, 0)
InputEvent(1678764544, 461847, 4, 5, 1996800)
InputEvent(1678764544, 461847, 0, 0, 0)
InputEvent(1678764544, 469274, 4, 5, 2004000)
InputEvent(1678764544, 469274, 0, 0, 0)
InputEvent(1678764544, 476617, 4, 5, 2011300)
InputEvent(1678764544, 476617, 0, 0, 0)
InputEvent(1678764544, 484094, 4, 5, 2018600)
InputEvent(1678764544, 484094, 0, 0, 0)
InputEvent(1678764544, 491327, 4, 5, 2025800)
InputEvent(1678764544, 491327, 0, 0, 0)
InputEvent(1678764544, 498676, 4, 5, 2033100)
InputEvent(1678764544, 498676, 0, 0, 0)
InputEvent(1678764544, 505982, 4, 5, 2040300)
InputEvent(1678764544, 505982, 0, 0, 0)
InputEvent(1678764544, 513510, 4, 5, 2047600)
InputEvent(1678764544, 513510, 0, 0, 0)
InputEvent(1678764544, 519895, 3, 57, -1)
InputEvent(1678764544, 519895, 1, 330, 0)
InputEvent(1678764544, 519895, 1, 325, 0)
InputEvent(1678764544, 519895, 4, 5, 2054900)
InputEvent(1678764544, 519895, 0, 0, 0)
```
</details>

# python-libinput

这个包已经在 2022.9 停止维护了，正在招守护者。文档的案例都存在点问题

# Refer

- [How can I get absolute touchpad coordinates? 2021](https://askubuntu.com/questions/1345561/how-can-i-get-absolute-touchpad-coordinates):python evdev,good example

- [Input event codes. kernel.org](https://www.kernel.org/doc/html/latest/input/event-codes.html)

- [python-evdev doc](https://python-evdev.readthedocs.io/en/latest/usage.html)

- [OzymandiasTheGreat /python-libinput](https://github.com/OzymandiasTheGreat/python-libinput)

- [Numpad asus](https://github.com/mohamed-badaoui/asus-touchpad-numpad-driver)

- [python-libinput doc](https://python-libinput.readthedocs.io/en/latest/usage.html)
