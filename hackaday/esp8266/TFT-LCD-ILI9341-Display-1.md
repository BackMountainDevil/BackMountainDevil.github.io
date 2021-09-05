---
title: "TFT LCD ILI9341 Display_1"
date: 2020-08-27T16:33:31+08:00
lastmod: 2020-08-27T19:33:31+08:00
keywords: ['MCU']
description: ""
tags: ['MCU']
categories: []
author: "筱氚"
---
[TOC]



# 简介

由于项目需求，需要做个屏幕控制，支持按钮控制和触摸控制

一番选择思考之后选择了比较容易的Arduino Uno和2.4‘’ TFT LCD with Touch（ILI9341）

## Needed Libraries

```c++
Adafruit_GFX
Adafruit_TFTLCD
```

# 坐标

这个坐标我整理出来发现有两种。。第二种暂时没有验证，先不放上来了。

先说第一种显示坐标系，由于屏幕大小位240 * 320

所以GFX库采用的坐标系就以像素为单位，大小完全就明了了。

# Adafruit_GFX

```c++
class Adafruit_GFX : public Print {

public:
  Adafruit_GFX(int16_t w, int16_t h); // Constructor

  virtual void drawPixel(int16_t x, int16_t y, uint16_t color) = 0;
  virtual void startWrite(void);
  virtual void writePixel(int16_t x, int16_t y, uint16_t color);
  virtual void writeFillRect(int16_t x, int16_t y, int16_t w, int16_t h,
                             uint16_t color);
  virtual void writeFastVLine(int16_t x, int16_t y, int16_t h, uint16_t color);
  virtual void writeFastHLine(int16_t x, int16_t y, int16_t w, uint16_t color);
  virtual void writeLine(int16_t x0, int16_t y0, int16_t x1, int16_t y1,
                         uint16_t color);
  virtual void endWrite(void);

  virtual void setRotation(uint8_t r);
  virtual void invertDisplay(bool i);

  virtual void drawFastVLine(int16_t x, int16_t y, int16_t h, uint16_t color);
  virtual void drawFastHLine(int16_t x, int16_t y, int16_t w, uint16_t color);
  virtual void fillRect(int16_t x, int16_t y, int16_t w, int16_t h,
                        uint16_t color);
  virtual void fillScreen(uint16_t color);
  // Optional and probably not necessary to change
  virtual void drawLine(int16_t x0, int16_t y0, int16_t x1, int16_t y1,
                        uint16_t color);
  virtual void drawRect(int16_t x, int16_t y, int16_t w, int16_t h,
                        uint16_t color);

  // These exist only with Adafruit_GFX (no subclass overrides)
  void drawCircle(int16_t x0, int16_t y0, int16_t r, uint16_t color);
  void drawCircleHelper(int16_t x0, int16_t y0, int16_t r, uint8_t cornername,
                        uint16_t color);
  void fillCircle(int16_t x0, int16_t y0, int16_t r, uint16_t color);
  void fillCircleHelper(int16_t x0, int16_t y0, int16_t r, uint8_t cornername,
                        int16_t delta, uint16_t color);
  void drawTriangle(int16_t x0, int16_t y0, int16_t x1, int16_t y1, int16_t x2,
                    int16_t y2, uint16_t color);
  void fillTriangle(int16_t x0, int16_t y0, int16_t x1, int16_t y1, int16_t x2,
                    int16_t y2, uint16_t color);
  void drawRoundRect(int16_t x0, int16_t y0, int16_t w, int16_t h,
                     int16_t radius, uint16_t color);
  void fillRoundRect(int16_t x0, int16_t y0, int16_t w, int16_t h,
                     int16_t radius, uint16_t color);
  void drawBitmap(int16_t x, int16_t y, const uint8_t bitmap[], int16_t w,
                  int16_t h, uint16_t color);
  void drawBitmap(int16_t x, int16_t y, const uint8_t bitmap[], int16_t w,
                  int16_t h, uint16_t color, uint16_t bg);
  void drawBitmap(int16_t x, int16_t y, uint8_t *bitmap, int16_t w, int16_t h,
                  uint16_t color);
  void drawBitmap(int16_t x, int16_t y, uint8_t *bitmap, int16_t w, int16_t h,
                  uint16_t color, uint16_t bg);
  void drawXBitmap(int16_t x, int16_t y, const uint8_t bitmap[], int16_t w,
                   int16_t h, uint16_t color);
  void drawGrayscaleBitmap(int16_t x, int16_t y, const uint8_t bitmap[],
                           int16_t w, int16_t h);
  void drawGrayscaleBitmap(int16_t x, int16_t y, uint8_t *bitmap, int16_t w,
                           int16_t h);
  void drawGrayscaleBitmap(int16_t x, int16_t y, const uint8_t bitmap[],
                           const uint8_t mask[], int16_t w, int16_t h);
  void drawGrayscaleBitmap(int16_t x, int16_t y, uint8_t *bitmap, uint8_t *mask,
                           int16_t w, int16_t h);
  void drawRGBBitmap(int16_t x, int16_t y, const uint16_t bitmap[], int16_t w,
                     int16_t h);
  void drawRGBBitmap(int16_t x, int16_t y, uint16_t *bitmap, int16_t w,
                     int16_t h);
  void drawRGBBitmap(int16_t x, int16_t y, const uint16_t bitmap[],
                     const uint8_t mask[], int16_t w, int16_t h);
  void drawRGBBitmap(int16_t x, int16_t y, uint16_t *bitmap, uint8_t *mask,
                     int16_t w, int16_t h);
  void drawChar(int16_t x, int16_t y, unsigned char c, uint16_t color,
                uint16_t bg, uint8_t size);
  void drawChar(int16_t x, int16_t y, unsigned char c, uint16_t color,
                uint16_t bg, uint8_t size_x, uint8_t size_y);
  void getTextBounds(const char *string, int16_t x, int16_t y, int16_t *x1,
                     int16_t *y1, uint16_t *w, uint16_t *h);
  void getTextBounds(const __FlashStringHelper *s, int16_t x, int16_t y,
                     int16_t *x1, int16_t *y1, uint16_t *w, uint16_t *h);
  void getTextBounds(const String &str, int16_t x, int16_t y, int16_t *x1,
                     int16_t *y1, uint16_t *w, uint16_t *h);
  void setTextSize(uint8_t s);
  void setTextSize(uint8_t sx, uint8_t sy);
  void setFont(const GFXfont *f = NULL);


  void setCursor(int16_t x, int16_t y) {
    cursor_x = x;
    cursor_y = y;
  }

  void setTextColor(uint16_t c) { textcolor = textbgcolor = c; }


  void setTextColor(uint16_t c, uint16_t bg) {
    textcolor = c;
    textbgcolor = bg;
  }


  void setTextWrap(bool w) { wrap = w; }


  void cp437(bool x = true) { _cp437 = x; }

  using Print::write;
#if ARDUINO >= 100
  virtual size_t write(uint8_t);
#else
  virtual void write(uint8_t);
#endif

 
  int16_t width(void) const { return _width; };

 
  int16_t height(void) const { return _height; }

 
  uint8_t getRotation(void) const { return rotation; }

 
  int16_t getCursorX(void) const { return cursor_x; }

  
  int16_t getCursorY(void) const { return cursor_y; };

protected:
  void charBounds(unsigned char c, int16_t *x, int16_t *y, int16_t *minx,
                  int16_t *miny, int16_t *maxx, int16_t *maxy);
  int16_t WIDTH;        ///< This is the 'raw' display width - never changes
  int16_t HEIGHT;       ///< This is the 'raw' display height - never changes
  int16_t _width;       ///< Display width as modified by current rotation
  int16_t _height;      ///< Display height as modified by current rotation
  int16_t cursor_x;     ///< x location to start print()ing text
  int16_t cursor_y;     ///< y location to start print()ing text
  uint16_t textcolor;   ///< 16-bit background color for print()
  uint16_t textbgcolor; ///< 16-bit text color for print()
  uint8_t textsize_x;   ///< Desired magnification in X-axis of text to print()
  uint8_t textsize_y;   ///< Desired magnification in Y-axis of text to print()
  uint8_t rotation;     ///< Display rotation (0 thru 3)
  bool wrap;            ///< If set, 'wrap' text at right edge of display
  bool _cp437;          ///< If set, use correct CP437 charset (default is off)
  GFXfont *gfxFont;     ///< Pointer to special font
};

/// A simple drawn button UI element
class Adafruit_GFX_Button {

public:
  Adafruit_GFX_Button(void);
  // "Classic" initButton() uses center & size
  void initButton(Adafruit_GFX *gfx, int16_t x, int16_t y, uint16_t w,
                  uint16_t h, uint16_t outline, uint16_t fill,
                  uint16_t textcolor, char *label, uint8_t textsize);
  void initButton(Adafruit_GFX *gfx, int16_t x, int16_t y, uint16_t w,
                  uint16_t h, uint16_t outline, uint16_t fill,
                  uint16_t textcolor, char *label, uint8_t textsize_x,
                  uint8_t textsize_y);
  // New/alt initButton() uses upper-left corner & size
  void initButtonUL(Adafruit_GFX *gfx, int16_t x1, int16_t y1, uint16_t w,
                    uint16_t h, uint16_t outline, uint16_t fill,
                    uint16_t textcolor, char *label, uint8_t textsize);
  void initButtonUL(Adafruit_GFX *gfx, int16_t x1, int16_t y1, uint16_t w,
                    uint16_t h, uint16_t outline, uint16_t fill,
                    uint16_t textcolor, char *label, uint8_t textsize_x,
                    uint8_t textsize_y);
  void drawButton(bool inverted = false);
  bool contains(int16_t x, int16_t y);


  void press(bool p) {
    laststate = currstate;
    currstate = p;
  }

  bool justPressed();
  bool justReleased();


  bool isPressed(void) { return currstate; };

private:
  Adafruit_GFX *_gfx;
  int16_t _x1, _y1; // Coordinates of top-left corner
  uint16_t _w, _h;
  uint8_t _textsize_x;
  uint8_t _textsize_y;
  uint16_t _outlinecolor, _fillcolor, _textcolor;
  char _label[10];

  bool currstate, laststate;
};


class GFXcanvas1 : public Adafruit_GFX {
public:
  GFXcanvas1(uint16_t w, uint16_t h);
  ~GFXcanvas1(void);
  void drawPixel(int16_t x, int16_t y, uint16_t color);
  void fillScreen(uint16_t color);
  bool getPixel(int16_t x, int16_t y) const;
 
  uint8_t *getBuffer(void) const { return buffer; }

protected:
  bool getRawPixel(int16_t x, int16_t y) const;

private:
  uint8_t *buffer;

#ifdef __AVR__

  static const uint8_t PROGMEM GFXsetBit[], GFXclrBit[];
#endif
};


class GFXcanvas8 : public Adafruit_GFX {
public:
  GFXcanvas8(uint16_t w, uint16_t h);
  ~GFXcanvas8(void);
  void drawPixel(int16_t x, int16_t y, uint16_t color);
  void fillScreen(uint16_t color);
  void writeFastHLine(int16_t x, int16_t y, int16_t w, uint16_t color);
  uint8_t getPixel(int16_t x, int16_t y) const;
 
  uint8_t *getBuffer(void) const { return buffer; }

protected:
  uint8_t getRawPixel(int16_t x, int16_t y) const;

private:
  uint8_t *buffer;
};


class GFXcanvas16 : public Adafruit_GFX {
public:
  GFXcanvas16(uint16_t w, uint16_t h);
  ~GFXcanvas16(void);
  void drawPixel(int16_t x, int16_t y, uint16_t color);
  void fillScreen(uint16_t color);
  void byteSwap(void);
  uint16_t getPixel(int16_t x, int16_t y) const;

  uint16_t *getBuffer(void) const { return buffer; }

protected:
  uint16_t getRawPixel(int16_t x, int16_t y) const;

private:
  uint16_t *buffer;
};
```



# Adafruit_TFTLCD

```cpp
class Adafruit_TFTLCD : public Adafruit_GFX {

public:
  Adafruit_TFTLCD(uint8_t cs, uint8_t cd, uint8_t wr, uint8_t rd, uint8_t rst);
  Adafruit_TFTLCD(void);

  void begin(uint16_t id = 0x9325);
  void drawPixel(int16_t x, int16_t y, uint16_t color);
  void drawFastHLine(int16_t x0, int16_t y0, int16_t w, uint16_t color);
  void drawFastVLine(int16_t x0, int16_t y0, int16_t h, uint16_t color);
  void fillRect(int16_t x, int16_t y, int16_t w, int16_t h, uint16_t c);
  void fillScreen(uint16_t color);
  void reset(void);
  void setRegisters8(uint8_t *ptr, uint8_t n);
  void setRegisters16(uint16_t *ptr, uint8_t n);
  void setRotation(uint8_t x);

  void setAddrWindow(int x1, int y1, int x2, int y2);
  void pushColors(uint16_t *data, uint8_t len, boolean first);

  uint16_t color565(uint8_t r, uint8_t g, uint8_t b),
      readPixel(int16_t x, int16_t y), readID(void);
  uint32_t readReg(uint8_t r);

private:
  void init(),
```

# 光说不做假把式

## 创建对象

```cpp
Adafruit_TFTLCD tft(LCD_CS, LCD_CD, LCD_WR, LCD_RD, LCD_RESET);

void setup(void) {
  tft.reset();
  tft.begin(0x9341);
}
```

首先创建了一个Adafruit_TFTLCD对象，名为tft，管脚定义这里省去了。
begin方法中的0x9341表示改TFT LCD的驱动为ILI9341，其它的这里不做介绍

## 屏幕

```c++
void fillScreen(uint16_t color);

uint16_t width();  //屏幕的宽度
uint16_t height();  //屏幕的高度
```

全屏填充颜色color，再次之前显示的内容会被挡住

### 案例

```c++
  tft.fillScreen(BLACK);
  delay(1000);
  tft.fillScreen(RED);
  delay(1000);
  tft.fillScreen(BLUE);
   delay(1000);
```



## 点

```cpp
void drawPixel(int16_t x, int16_t y, uint16_t color);
```

在点（x，y）上画一个颜色为color的像素点。

### 案例

```cpp
  tft.drawPixel(1,1,RED);
  tft.drawPixel(10,10,RED);
  tft.drawPixel(20,20,RED);
  tft.drawPixel(40,40,RED);
  tft.drawPixel(60,60,RED);
```

## 线

```cpp
  void drawLine(int16_t x0, int16_t y0, int16_t x1, int16_t y1, uint16_t color);  
  void drawFastHLine(int16_t x0, int16_t y0, int16_t w, uint16_t color);
  void drawFastVLine(int16_t x0, int16_t y0, int16_t h, uint16_t color);
  
```

最简单的是两点确定一条直线，当然也可以确定一个点、方向、长度，后两个是画水平线或者铅锤线。

### 案例

```c++
 tft.drawFastHLine(10,10,170,RED);
 tft.drawFastVLine(10,10,170,RED);
 tft.drawLine(10,10,100,180,RED);
```



## 矩形&&圆角矩形

```c++
void drawRect(int16_t x, int16_t y, int16_t w, int16_t h, uint16_t c);
void fillRect(int16_t x, int16_t y, int16_t w, int16_t h, uint16_t c);

void drawRoundRect(int16_t x, int16_t y, int16_t w, int16_t h, int16_t radius, uint16_t color);
void fillRoundRect(int16_t x, int16_t y, int16_t w, int16_t h, int16_t radius, uint16_t color);
```

drawRect的矩形直画出边框，内部不填充，如果需要内部填充颜色则需要使用fillRect

圆角矩形则是多了一个设置圆角半径的参数raduis，是否填充与上面一样。



### 案例



```c++
   tft.drawRect(10,10,150,100,RED);
   tft.fillRect(10,120,150,100,RED);
```



```c++
 tft.drawRoundRect(10,10,150,100,10,RED);
 tft.fillRoundRect(10,120,150,100,10,RED);
```

## 圆形

```c++
void drawCircle(uint16_t x0, uint16_t y0, uint16_t r, uint16_t color);
void fillCircle(uint16_t x0, uint16_t y0, uint16_t r, uint16_t color);
```

这个就比较简单，圆心坐标（x0,y0）,半径r，颜色color

### 案例

```c++
 tft.drawCircle(100,100,50,WHITE);
 tft.fillCircle(100,260,50,BLUE);
```



## 三角形

```c++
void drawTriangle(uint16_t x0, uint16_t y0, uint16_t x1, uint16_t y1, uint16_t x2, uint16_t y2, uint16_t color);
void fillTriangle(uint16_t x0, uint16_t y0, uint16_t x1, uint16_t y1, uint16_t x2, uint16_t y2, uint16_t color);
```

三角形需要确定三个顶点以及颜色

### 案例

```c++
 tft.drawTriangle(10,10,100,15,180,100,GREEN);
 tft.fillTriangle(10,110,100,115,180,200,GREEN);
```



## 字符&&英文文本

```c++
void drawChar(uint16_t x, uint16_t y, char c, uint16_t color, uint16_t bg, uint8_t size);

void setCursor(uint16_t x0, uint16_t y0);  //字体左上角顶点坐标
void setTextColor(uint16_t color); //字体前景色
void setTextColor(uint16_t color, uint16_t backgroundcolor);//字体前景色与背景色
void setTextSize(uint8_t size);  //字体大小放法因子
void setTextWrap(boolean w);	//是否自动换行，默认为true，滚动显示设置为false
```

drawChar只能显示单个字符，需要确定左上角顶点坐标(x,y)，字符c，颜色color，前景色color，背景色bg，大小size，大小为1时表示5 * 8像素，为2就表示10 * 16

自定义字体不支持背景色，可在此之前绘制填充颜色的形状，如圆角矩形

### 案例

```c++
tft.fillScreen(GREEN);
 tft.drawChar(150,10,'A',RED,WHITE,5);

 tft.setCursor(10,50);
 tft.print("AB 3.14");    //默认前景色white、无背景色、大小为1

 tft.setCursor(10,80);
 tft.setTextSize( 4);
 tft.print("AB 3.14");
 
 tft.setCursor(10,115);
 tft.setTextColor(RED); //背景色不做设置
 tft.setTextSize( 4);
 tft.print("AB你好3.141516");

  tft.setCursor(10,180);
  tft.setTextColor(RED, WHITE);
 tft.setTextSize( 4);
 tft.setTextWrap(false);
 tft.print("AB你好3.141516");
```

可以看到默认的字体不支持中文，需要使用中文的改日再补上



## 旋转

```c++
void setRotation(uint8_t rotation);
```

旋转参数可以是0、1、2或3，分别对应0,90,180或270度。

对于属于Arduino屏蔽的显示，旋转值0将显示设置为竖屏(高)模式，旋转值2也是纵向模式，。旋转1是横屏模式，，而旋转3也是横屏模式。

### 案例

```c++
 //tft.setRotation(1); //注释和未注释的情况下做对比

 tft.fillScreen(GREEN);
 tft.drawChar(150,10,'A',RED,WHITE,5);

 tft.setCursor(10,50);
 tft.print("AB 3.14");    //默认前景色white、无背景色、大小为1

 tft.setCursor(10,80);
 tft.setTextSize( 4);
 tft.print("AB 3.14");
 
 tft.setCursor(10,115);
 tft.setTextColor(RED); //背景色不做设置
 tft.setTextSize( 4);
 tft.print("AB你好3.141516");

  tft.setCursor(10,180);
  tft.setTextColor(RED, WHITE);
 tft.setTextSize( 4);
 tft.setTextWrap(false);
 tft.print("AB你好3.141516");
```

# Conclusion

截至目前案例程序TestDraw_1.ino已上传github，[地址传送门](https://github.com/BackMountainDevil/UNO-TFTLCD-ILI9341-Demo)

还有部分内容慢慢更新，比如显示中文、取模做点阵位图、显示图像、触摸功能之类的、下期[传送门](http://kearney.club/2020/08/27/TFT-LCD-ILI9341-Display-2/)

# References

- https://github.com/adafruit/TFTLCD-Library
- https://github.com/adafruit/Adafruit-GFX-Library
- https://github.com/BackMountainDevil/UNO-TFTLCD-ILI9341-Demo
-   https://zhuanlan.zhihu.com/p/157915188
-   https://blog.csdn.net/weixin_43031092/article/details/108159783
-   https://blog.csdn.net/weixin_43031092/article/details/10800748