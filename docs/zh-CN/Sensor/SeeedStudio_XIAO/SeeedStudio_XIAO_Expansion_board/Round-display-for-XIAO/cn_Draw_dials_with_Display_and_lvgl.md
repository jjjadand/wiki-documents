---
description: 使用 LVGL 和 TFT 在 Seeed Studio 圆形显示屏上为 XIAO 系列绘制表盘
title: 为所有 XIAO 系列使用 LVGL 和 TFT
keywords:
- XIAO
- 圆形显示屏
- lvgl
- 绘制表盘
image: https://files.seeedstudio.com/wiki/seeed_logo/logo_2023.png
slug: /cn/using_lvgl_and_tft_on_round_display
last_update:
  date: 05/15/2025
  author: Spencer
---

# 为所有 XIAO 系列使用 LVGL 和 TFT 在 Seeed Studio 圆形显示屏上

:::note
本文档由 AI 翻译。如您发现内容有误或有改进建议，欢迎通过页面下方的评论区，或在以下 Issue 页面中告诉我们：https://github.com/Seeed-Studio/wiki-documents/issues
:::

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/000.jpg" style={{width:600, height:'auto'}}/></div>

<div class="get_one_now_container" style={{textAlign: 'center'}}>
    <a class="get_one_now_item" href="https://www.seeedstudio.com/Seeed-Studio-Round-Display-for-XIAO-p-5638.html">
            <strong><span><font color={'FFFFFF'} size={"4"}> 立即购买 🖱️</font></span></strong>
    </a>
</div>

<br></br>

感谢您购买 Seeed Studio 圆形显示屏产品。在本教程部分中，我们将重点介绍如何使用 `TFT_eSPI` 库和 `LVGL` 库在圆形显示屏上绘制各种丰富有趣的表盘图案，并从简单到深入地介绍这两个复杂但实用的库的一些常用功能。通过本教程的内容，希望您能够使用该产品绘制理想的表盘图案。祝您学习愉快！

## 入门指南

在开始学习之前，我们希望您准备好以下内容。

### 硬件准备

为了演示，本教程将使用 **XIAO ESP32S3** 作为主控。

<table align="center">
	<tr>
	    <th>Seeed Studio 圆形显示屏适配 XIAO</th>
	    <th>Seeed Studio XIAO ESP32S3</th>
	</tr>
    <tr>
        <td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/rounddisplay.jpg" style={{width:210, height:'auto'}}/></div></td>
        <td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/img/xiaoesp32s3.jpg" style={{width:210, height:'auto'}}/></div></td>
    </tr>
	<tr>
	    <td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="https://www.seeedstudio.com/Seeed-Studio-Round-Display-for-XIAO-p-5638.html"> 
            <strong><span><font color={'FFFFFF'} size={"4"}> 立即购买 🖱️</font></span></strong>
    		</a>
		</div></td>
	    <td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="https://www.seeedstudio.com/XIAO-ESP32S3-p-5627.html">
            <strong><span><font color={'FFFFFF'} size={"4"}> 立即购买 🖱️</font></span></strong>
    		</a>
		</div></td>
	</tr>
</table>

如果您想使用其他 XIAO 产品，本教程中的方法也同样适用。

<table align="center">
	<tr>
		<th>Seeed Studio XIAO SAMD21</th>
		<th>Seeed Studio XIAO RP2040</th>
		<th>Seeed Studio XIAO nRF52840 (Sense)</th>
		<th>Seeed Studio XIAO ESP32C3</th>
	</tr>
	<tr>
		<td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/Seeeduino-XIAO/img/Seeeduino-XIAO-preview-1.jpg" style={{width:400, height:'auto'}}/></div></td>
		<td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/102010428_Preview-07.jpg" style={{width:500, height:'auto'}}/></div></td>
	    <td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/xiaoblesense.jpg" style={{width:500, height:'auto'}}/></div></td>
		<td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/xiaoesp32c3.jpg" style={{width:450, height:'auto'}}/></div></td>
	</tr>
    <tr>
	    <td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="https://www.seeedstudio.com/Seeeduino-XIAO-Arduino-Microcontroller-SAMD21-Cortex-M0+-p-4426.html">
            <strong><span><font color={'FFFFFF'} size={"4"}> 立即购买 🖱️</font></span></strong>
    		</a>
		</div></td>
		<td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="https://www.seeedstudio.com/XIAO-RP2040-v1-0-p-5026.html">
            <strong><span><font color={'FFFFFF'} size={"4"}> 立即购买 🖱️</font></span></strong>
    		</a>
		</div></td>
		<td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="https://www.seeedstudio.com/Seeed-XIAO-BLE-Sense-nRF52840-p-5253.html">
            <strong><span><font color={'FFFFFF'} size={"4"}> 立即购买 🖱️</font></span></strong>
    		</a>
		</div></td>
		<td><div class="get_one_now_container" style={{textAlign: 'center'}}>
    		<a class="get_one_now_item" href="https://www.seeedstudio.com/seeed-xiao-esp32c3-p-5431.html">
            <strong><span><font color={'FFFFFF'} size={"4"}> 立即购买 🖱️</font></span></strong>
    		</a>
		</div></td>
	</tr>
</table>

安装 XIAO 与圆形显示屏时，请让 XIAO 的 Type-C 接口朝向圆形显示屏的外侧，然后将每个引脚与双排 7 针插头对齐连接。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/50.jpg" style={{width:500, height:'auto'}}/></div>

### 软件准备

此部分已在 [基础 Wiki](https://wiki.seeedstudio.com/get_start_round_display#software-preparation) 中详细描述，您可以直接跳转阅读。

## 使用 TFT_eSPI 库绘制简单表盘

TFT_eSPI 是一个功能丰富的 Arduino IDE 兼容图形和字体库，适用于 32 位处理器。XIAO 圆形显示屏提供的 TFT 库基于该库，并经过兼容 XIAO 和 XIAO 圆形显示屏的调整，支持整个 XIAO 系列的使用。

### TFT 库的常用接口

#### 1. 创建 TFT 对象

```c
TFT_eSPI tft = TFT_eSPI()
TFT_eSPI tft = TFT_eSPI(240,240)        // 在创建对象时设置屏幕尺寸
```

#### 2. 初始化

```c
void init(uint8_t tc = TAB_COLOUR)
void begin(uint8_t tc = TAB_COLOUR)
```

`tft.begin()` 和 `tft.init()` 是两个功能相同的函数。

#### 3. 清屏

```c
void fillScreen(uint32_t color) // 用指定颜色填充屏幕
```

#### 4. 设置屏幕方向

```c
void setRotation(uint8_t r);      // 设置显示图像的旋转方向，r 的可选参数为 0, 1, 2, 3
uint8_t getRotation(void)         // 读取当前的旋转角度
```

0, 1, 2, 3 分别表示 0°、90°、180°、270°，而 4 表示镜像。

#### 5. 颜色转换

```c
uint16_t color565(uint8_t red, uint8_t green, uint8_t blue)    // 将 8 位红、绿、蓝转换为 16 位
uint16_t color8to16(uint8_t color332)                          // 将 8 位颜色转换为 16 位
uint8_t  color16to8(uint16_t color565)                         // 将 16 位颜色转换为 8 位
uint32_t color16to24(uint16_t color565)                        // 将 16 位颜色转换为 24 位
uint32_t color24to16(uint32_t color888)                        // 将 24 位颜色转换为 16 位
```

#### 6. 颜色反转

```c
void invertDisplay(bool i)      // i = true 时反转所有显示颜色，i = false 时正常显示
```

#### 7. 文本相关设置

```c
/* 光标 */
void setCursor(int16_t x, int16_t y)                     // 设置 tft.print() 的光标位置
void setCursor(int16_t x, int16_t y, uint8_t font)       // 设置 tft.print() 的光标位置和字体大小
int16_t getCursorX(void)                                 // 读取当前光标的 x 坐标（随 tft.print() 移动）
int16_t getCursorY(void)                                 // 读取当前光标的 y 坐标

/* 字体颜色 */
void setTextColor(uint16_t color)                        // 仅设置字符颜色
void setTextColor(uint16_t fgcolor, uint16_t bgcolor, bool bgfill = false)   // 设置字符的前景色和背景色

/* 字体大小 */
void setTextSize(uint8_t size)                           // 设置字符大小倍数（这会增加像素大小）
void setTextWrap(bool wrapX, bool wrapY = false)         // 打开/关闭文本在 TFT 宽度和/或高度的换行功能

/* 文本参考位置 */
void setTextDatum(uint8_t datum)                         // 设置文本参考位置（默认是左上角）
uint8_t getTextDatum(void)                               // 获取文本参考位置

/* 设置背景填充，可用于清除指定区域的显示 */
void setTextPadding(uint16_t x_width)                    // 设置文本填充（背景边距/重写）宽度，以像素为单位
uint16_t getTextPadding(void)                            // 获取文本填充宽度
```

从上述函数可以看出，如果您想打印显示的文本，可以简单地使用 `tft.print()` 函数。

#### 8. 简单形状绘制

您可以使用以下函数绘制一些简单的形状，包括像素点、直线段、矩形、圆等。

```c
drawPixel(int32_t x, int32_t y, uint32_t color)    // 绘制单个像素点

drawLine(int32_t xs, int32_t ys, int32_t xe, int32_t ye, uint32_t color)  // 绘制一条线

drawRect(int32_t x, int32_t y, int32_t w, int32_t h, uint32_t color)   // 绘制一个矩形
fillRect(int32_t x, int32_t y, int32_t w, int32_t h, uint32_t color)  // 绘制一个填充颜色的矩形

drawCircle(int32_t x, int32_t y, int32_t r, uint32_t color)  // 绘制一个圆
fillCircle(int32_t x, int32_t y, int32_t r, uint32_t color)  // 绘制一个填充颜色的圆

drawEllipse(int16_t x, int16_t y, int32_t rx, int32_t ry, uint16_t color)  // 绘制一个椭圆
fillEllipse(int16_t x, int16_t y, int32_t rx, int32_t ry, uint16_t color)  // 绘制一个填充颜色的椭圆

drawTriangle(int32_t x1,int32_t y1, int32_t x2,int32_t y2, int32_t x3,int32_t y3, uint32_t color)  // 绘制一个三角形
fillTriangle(int32_t x1,int32_t y1, int32_t x2,int32_t y2, int32_t x3,int32_t y3, uint32_t color)  // 绘制一个填充颜色的三角形
```

#### 9. 复杂形状绘制

TFT 库还为我们提供了绘制复杂形状的方法，例如圆角矩形、圆弧、平滑形状等。

```c
/** 
    绘制一个与背景颜色（bg_color）混合的像素点，并返回混合后的颜色
    如果未指定 bg_color，则背景颜色将从 TFT 或精灵中读取
**/
drawPixel(int32_t x, int32_t y, uint32_t color, uint8_t alpha, uint32_t bg_color)

/** 
    在 ax, ay 位置绘制一个小的抗锯齿填充圆，半径为 r（使用 drawWideLine）
    如果未包含 bg_color，则背景颜色将从 TFT 或精灵中读取
**/
drawSpot(float ax, float ay, float r, uint32_t fg_color, uint32_t bg_color)


drawFastVLine(int32_t x, int32_t y, int32_t h, uint32_t color)  // 绘制垂直直线
drawFastHLine(int32_t x, int32_t y, int32_t w, uint32_t color)  // 绘制水平直线
drawWideLine(float ax, float ay, float bx, float by, float wd, uint32_t fg_color, uint32_t bg_color)  // 绘制一条粗实线
drawWedgeLine(float ax, float ay, float bx, float by, float aw, float bw, uint32_t fg_color, uint32_t bg_color);  // 绘制一条渐变线。aw 和 bw 分别表示渐变线的起始和结束宽度。


/**
    与 "drawSmoothArc" 类似，但弧的两端未进行抗锯齿处理，这便于动态改变弧段长度并确保弧段连接处的干净效果。
    默认情况下弧的侧面是抗锯齿的。如果 smoothArc 为 false，则侧面不会进行抗锯齿处理。
**/
drawArc(int32_t x, int32_t y, int32_t r, int32_t ir, uint32_t startAngle, uint32_t endAngle, uint32_t fg_color, uint32_t bg_color, bool smoothArc);

/**
    绘制一个抗锯齿（平滑）的弧，起始角度和结束角度之间的弧段。弧的两端是抗锯齿的。
    默认情况下弧是以方形端点绘制的，除非包含 "roundEnds" 参数并设置为 true。
    角度 = 0 表示 6 点钟位置，90 表示 9 点钟位置等。角度必须在 0-360 范围内，否则会被裁剪到这些限制。
    起始角度可以大于结束角度。弧总是从起始角度顺时针绘制。
**/
drawSmoothArc(int32_t x, int32_t y, int32_t r, int32_t ir, uint32_t startAngle, uint32_t endAngle, uint32_t fg_color, uint32_t bg_color, bool roundEnds);

/**
    在 x, y 位置绘制一个抗锯齿填充圆，半径为 r。
    注意：线的厚度为 3 像素，以减少抗锯齿窄线的可见“编织”效果，这意味着内抗锯齿区域始终在 r-1，外部区域在 r+1。
**/
drawSmoothCircle(int32_t x, int32_t y, int32_t r, uint32_t fg_color, uint32_t bg_color)

/**
    在 x, y 位置绘制一个抗锯齿填充圆，半径为 r。
    如果未包含 bg_color，则背景颜色将从 TFT 或精灵中读取。
**/
fillSmoothCircle(int32_t x, int32_t y, int32_t r, uint32_t color, uint32_t bg_color)


/**
    绘制一个圆角矩形，线的厚度为 r-ir+1，边界框由 x, y 和 w, h 定义。
    外角半径为 r，内角半径为 ir。
    边框的内外部分均为抗锯齿处理。
**/
drawSmoothRoundRect(int32_t x, int32_t y, int32_t r, int32_t ir, int32_t w, int32_t h, uint32_t fg_color, uint32_t bg_color, uint8_t quadrants)

/**
    绘制一个填充的圆角矩形，角半径为 r，边界框由 x, y 和 w, h 定义。
**/
fillSmoothRoundRect(int32_t x, int32_t y, int32_t w, int32_t h, int32_t radius, uint32_t color, uint32_t bg_color)
```

#### 10. 变量与文本

除了简单地显示特定字符串，有时我们需要在屏幕上显示一些变量，例如时间和传感器值等。根据变量的类型，可以选择使用以下不同的函数。

```c
drawChar(int32_t x, int32_t y, uint16_t c, uint32_t color, uint32_t bg, uint8_t size)
drawNumber(long intNumber, int32_t x, int32_t y, uint8_t font) // 使用指定字体绘制整数。如果未设置字体（最后一个参数，默认字体）
drawFloat(float floatNumber, uint8_t decimal, int32_t x, int32_t y, uint8_t font), // 使用指定字体绘制浮点数。如果未设置字体（最后一个参数，默认字体）
drawString(const char *string, int32_t x, int32_t y, uint8_t font),  // 使用指定字体绘制字符串。如果未设置字体（最后一个参数，默认字体）
```

#### 11. 图像处理

要使用 `pushImage()` 函数通过 TFT 库在屏幕上显示图像，可以按照以下步骤操作：

- 将图像文件转换为 Arduino 可识别的 C/C++ 数组格式。可以使用在线工具如 **Image2CPP** 将位图图像转换为数组格式。

- 在 Arduino 程序中包含生成的图像数组文件。

- 初始化 TFT 库和屏幕，设置屏幕分辨率和颜色模式。

- 使用 `tft.pushImage(x, y, width, height, data)` 函数将图像数据推送到屏幕，其中 x 和 y 是图像左上角的坐标，width 和 height 是图像的宽度和高度，data 是图像数组。

```c
// 图像数据数组
const unsigned char image_data[] PROGMEM = {
  // 在此处插入图像转换后的 C/C++ 数组数据
};

tft.pushImage(x, y, image_width, image_height, image_data);
```

#### 12. TFT_eSprite 类

`TFT_eSprite` 和 `TFT_eSPI` 都是用于 TFT LCD 显示屏的 Arduino 库，但它们具有不同的功能和设计目的。

TFT_eSPI 是一个功能强大的库，具有许多高级功能和配置选项，可以实现各种显示效果。它支持多种显示驱动芯片和控制器，并可用于多个 Arduino 硬件平台。它使用 SPI 接口和高度优化的代码来实现快速刷新率和较小的代码占用。TFT_eSPI 库可用于包括游戏、图形和 GUI 在内的各种应用。

TFT_eSprite 是对 TFT_eSPI 库的补充，主要用于在显示屏上绘制小型精灵，例如游戏角色、图标、文本等。TFT_eSprite 可以实现更快的绘图速度，因为它在内存中缓存图像并进行部分刷新。这使得在更新小型精灵时可以实现更快的刷新率，并节省处理器时间和内存空间。

因此，TFT_eSPI 是一个功能强大的通用库，适用于多种应用，而 TFT_eSprite 是一个专注于绘制小型精灵的库，可以提供更高的绘图效率。

使用 TFT_eSPI 库的一般格式如下：

```c
#include <TFT_eSPI.h>

TFT_eSPI tft = TFT_eSPI();

void setup() {
  tft.init();
  tft.setRotation(1);
}

void loop() {
    // 编写绘制图形的代码
}
```

使用 TFT_eSprite 库的一般格式如下：

```c
#include <TFT_eSPI.h>

TFT_eSPI tft = TFT_eSPI();
TFT_eSprite spr = TFT_eSprite(&tft);

void setup() {
  tft.init();
  tft.setRotation(1);
}

void loop() {
    spr.createSprite(128, 128);  // 创建一个 128*128 大小的精灵

    // 编写绘制图形的代码

    spr.pushSprite(0, 0);  // 放置绘制的精灵
    spr.deleteSprite();
}
```

:::note
在上述示例代码中，当程序退出 `loop()` 函数时，会调用 `spr.deleteSprite();` 来删除 TFT_eSprite 对象并释放内存空间。这样，在下一次循环中创建 TFT_eSprite 对象时，可以使用之前释放的内存空间，从而避免浪费内存资源。
:::

有关 TFT 库函数及其使用的更多信息，建议阅读库中的 **TFT_eSPI.h** 和 **TFT_eSPI.cpp** 文件。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/1.png" style={{width:400, height:'auto'}}/></div>

### 关于 TFT 库的自定义选项

有时我们需要使用一些自定义字体库或未启用的功能以节省空间。这时需要修改 **Setup66_Seeed_XIAO_RoundDisplay.h** 文件的内容。

- **Windows** 系统中该文件的默认路径为： 

`C:\Users\{UserName}\Documents\Arduino\libraries\TFT_eSPI\User_Setups\Setup66_Seeed_XIAO_RoundDisplay.h`

- **MacOS** 系统中该文件的默认路径为： 

`\Users\{UserName}\Documents\Arduino\libraries\TFT_eSPI\User_Setups\Setup66_Seeed_XIAO_RoundDisplay.h`

请根据实际使用情况开启或关闭一些不必要的功能。
例如，如果需要使用自定义字体，则应取消注释 `#define SMOOTH_FONT`，否则运行时可能会出现错误。

当然，自定义字体的头文件应保存在与 ino 文件相同的目录中，这是避免错误的必要步骤。

### 基于 TFT 的表盘示例程序

我们为圆形显示屏编写了一个表盘程序，可以直接使用。由于某些 XIAO 型号的内存空间限制，该程序仅实现了基本的指针移动功能，未设计其他功能。用户可以通过该程序学习 TFT 库的使用和指针移动功能，并根据实际情况完成更复杂的表盘开发。

<div class="github_container" style={{textAlign: 'center'}}>
    <a class="github_item" href="https://github.com/limengdu/Seeed-Studio-XIAO-Round-Display-lvgl8.3.5/tree/main/tft_espi-base-dial">
    <strong><span><font color={'FFFFFF'} size={"4"}> 下载代码</font></span></strong> <svg aria-hidden="true" focusable="false" role="img" className="mr-2" viewBox="-3 10 9 1" width={16} height={16} fill="currentColor" style={{textAlign: 'center', display: 'inline-block', userSelect: 'none', verticalAlign: 'text-bottom', overflow: 'visible'}}><path d="M8 0c4.42 0 8 3.58 8 8a8.013 8.013 0 0 1-5.45 7.59c-.4.08-.55-.17-.55-.38 0-.27.01-1.13.01-2.2 0-.75-.25-1.23-.54-1.48 1.78-.2 3.65-.88 3.65-3.95 0-.88-.31-1.59-.82-2.15.08-.2.36-1.02-.08-2.12 0 0-.67-.22-2.2.82-.64-.18-1.32-.27-2-.27-.68 0-1.36.09-2 .27-1.53-1.03-2.2-.82-2.2-.82-.44 1.1-.16 1.92-.08 2.12-.51.56-.82 1.28-.82 2.15 0 3.06 1.86 3.75 3.64 3.95-.23.2-.44.55-.51 1.07-.46.21-1.61.55-2.33-.66-.15-.24-.6-.83-1.23-.82-.67.01-.27.38.01.53.34.19.73.9.82 1.13.16.45.68 1.31 2.69.94 0 .67.01 1.3.01 1.49 0 .21-.15.45-.55.38A7.995 7.995 0 0 1 0 8c0-4.42 3.58-8 8-8Z" /></svg>
    </a>
</div>

## 使用 LVGL 库绘制简单表盘

LVGL 库是一款通用嵌入式图形库，提供了丰富的图形用户界面控件，例如按钮、标签、列表等，可用于构建各种用户界面。与 TFT 库不同，LVGL 库提供了一个抽象的、面向对象的图形接口，使用和维护起来更加方便，但可能会带来一些性能和可靠性上的权衡。

在构建复杂用户界面时，LVGL 库是一个非常有用的工具，可以减少编写和维护代码的工作量。而 TFT 库则更适合一些需要高性能图形处理的应用，例如实时图像处理、视频渲染等。

### LVGL 库的常用接口

LVGL 库的 API 非常丰富和复杂，我们希望每位使用 LVGL 的用户都能花时间阅读官方 LVGL 介绍文档。

- [快速入门](https://docs.lvgl.io/latest/en/html/get-started/index.html)
- [显示接口](https://docs.lvgl.io/latest/en/html/porting/display.html)
- [输入设备接口](https://docs.lvgl.io/latest/en/html/porting/indev.html)
- [时钟接口](https://docs.lvgl.io/latest/en/html/porting/tick.html)
- [操作系统和中断](https://docs.lvgl.io/latest/en/html/porting/os.html)

### 使用 SquareLine Studio 绘制复杂的 UI 界面

除了阅读丰富的 LVGL 官方文档并编写自己的 LVGL 图形程序外，我们还可以使用官方的 LVGL SquareLine Studio 工具来提高开发效率。

接下来，我们将简要介绍如何在圆形屏幕上使用该软件、配置方法，以帮助您快速上手使用该软件设计一些界面。

:::caution
我们建议您使用 **v1.5.1** 版本的 SquareLine Studio。这也是撰写本 Wiki 时的最新软件版本。

本教程使用的环境：

1. **[SeeedStudio_TFT_eSPI 库](https://github.com/Seeed-Projects/SeeedStudio_TFT_eSPI)**，由 Bodmer 修改，由 Seeed Studio 提供，版本：**2.5.43**

2. **[LVGL 库](https://github.com/lvgl/lvgl/releases/tag/v8.3.11)**，由 kisvegabor、embeddedt、pete-pjb 提供，版本：**8.3.11**

**LVGL** 库可以通过库版本管理器直接搜索下载 **8.3.11** 版本。**SeeedStudio_TFT_eSPI** 库需要从 Github 下载并添加到 Arduino IDE 环境中。

:::

#### 第一步：下载 SquareLine Studio

您可以点击 [这里](https://squareline.io/) 访问 SquareLine Studio 的官方网站，然后点击 **TRY IT FOR FREE** 将软件下载到您的电脑。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/3.png" style={{width:1000, height:'auto'}}/></div>

如果您的电脑是首次使用该软件，那么您将获得免费的 30 天试用期。如果不是，免费版本也可以绘制最多 5 个页面，使用 50 个组件。

:::caution
如果您是首次使用，请不要注册登录没有余额的账户，否则可能会浪费完整的 30 天试用期！
:::

#### 第二步：配置屏幕接口信息

接下来，我们可以打开软件并从创建一个空白显示页面开始。

由于我们使用的是 Arduino 编程，那么我们创建的项目也需要是 Arduino 文件。

我们的圆形屏幕分辨率为 **240*240**，仅支持 **16bit** 色深。除此之外，项目的名称和主题需要您自行定义，这里我将使用暗色风格作为主题。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/4.png" style={{width:1000, height:'auto'}}/></div>

如果您像我一样操作过快，忘记设置表盘形状并创建了项目怎么办？没问题，在主界面的左上角，您还可以找到项目设置选项卡，对刚刚的设置进行修改。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/5.png" style={{width:1000, height:'auto'}}/></div>

:::caution
请注意，在正式开始绘制之前完成您的项目设置，并确保其与我们的屏幕规格匹配，否则您绘制的内容可能无法正确显示在屏幕上。

并非所有内容都可以在项目创建后修改，例如项目名称。请不要在项目名称中使用除英文以外的语言或特殊字符，并请不要使用 **"-"** 符号，请将 **"-"** 替换为 **"_"**。否则，导出的程序在编译时可能会出现错误！
:::

#### 第三步：了解软件的功能布局

根据我的使用习惯，我将软件的主界面大致分为以下几个部分。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/6.png" style={{width:1000, height:'auto'}}/></div>

- **层次结构和动画面板**：此区域允许您设置不同的表盘页面、显示层和动画。

- **组件**：在这里，您可以选择要添加到显示页面上的组件。这些组件已集成到软件中，可以直接使用。这里没有的组件需要您稍后在自己的编程软件中添加。

- **工作区**：在工作区中，您可以通过拖放来更改某些组件的位置。更方便的是，最终显示将与工作区中显示的内容一致，因此所见即所得。

- **资源 & 控制台**：资源显示您已添加的图像剪辑，这些添加的图像剪辑可以用于支持插入图像的组件。控制台将显示您在设置过程中发生的一些错误信息（如果有）。

- **设置区域**：这里的主要用途是配置组件的属性。

我们将首先对软件界面有一个总体了解，然后带您实际理解每个部分的使用方法。

#### 第四步：使用软件实现您的想法

假设我现在想要绘制一个音乐界面。当然，我非常喜欢听音乐，所以我想以绘制一个音乐显示界面为例。

我希望这个音乐显示屏幕包含以下组件：

- 我最喜欢的音乐的专辑封面作为背景。
- 一个播放进度条。
- 一个音量控制条。
- 一个播放和暂停按钮。

在整理好我们的需求后，我们需要像建造建筑一样，从底层开始设计显示组件。

第一步是创建一个音乐背景图片。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/7.png" style={{width:1000, height:'auto'}}/></div>

在小部件中选择 **Panel**，您可以点击它，它会自动放置在表盘的中心，或者您可以将其拖动到您想要放置的位置。

我们将显示的图像放置在 Panel 中，而不是直接放在表盘背景上，因为我不希望整个表盘都是这张图片，并且 Panel 可以自由调整大小。

此时，您可以看到在设置区域中已经有一系列属性可以设置。每个组件的设置通常是类似的，只有一两个选项可能会有所不同。

> **名称**: 您应该为小部件命名。名称不能以数字、下划线或特殊字符开头。导出代码后，您可以通过该名称找到小部件。

> **布局**: 您可以使用布局自动排列小部件的子元素。如果启用了布局，则子元素的 X 和 Y 值无法手动调整，除非在子元素上启用了 `IGNORE_LAYOUT` 或 `FLOATING` 标志。

>    **主要标志**
>    - **隐藏** - 使对象隐藏。（就像它根本不存在一样。）
>    - **可点击** - 使对象可以通过输入设备点击
>    - **点击聚焦** - 点击时为对象添加聚焦状态
>    - **可选中** - 点击对象时切换选中状态
>    - **可捕捉** - 如果父对象启用了滚动捕捉，则可以捕捉到此对象
>    - **按压锁定** - 即使按压滑离对象，也保持对象被按下
>    - **事件冒泡** - 将事件传播到父对象
>    - **手势冒泡** - 将手势事件传播到父对象
>    - **高级点击测试** - 允许执行更精确的点击测试。例如，考虑圆角
>    - **忽略布局** - 使对象可以通过布局进行定位
>    - **浮动** - 当父对象滚动时，对象不会滚动，并忽略布局

>   **滚动标志**
>   - **可滚动** - 使对象可滚动
>   - **滚动弹性** - 允许内部滚动，但速度较慢
>   - **滚动惯性** - 当“甩动”时，使对象滚动更远
>   - **单项滚动** - 仅允许滚动一个可捕捉的子元素
>   - **滚动链** - 允许将滚动传播到父对象
>   - **滚动聚焦** - 当对象被聚焦时，自动滚动以使其可见

>**滚动设置**
>   - **滚动方向** - 根据配置显示滚动条
>   - **滚动条模式** - 根据配置模式显示滚动条。以下模式可用：
>       - **关闭** - 从不显示滚动条
>       - **开启** - 始终显示滚动条
>       - **活动** - 当对象正在滚动时显示滚动条
>       - **自动** - 当内容足够大以至于需要滚动时显示滚动条

> **状态**: 对象可以处于以下状态的组合中：
> - **可点击** - 切换或选中状态
> - **禁用** - 禁用状态
> - **可聚焦** - 通过键盘、编码器聚焦或通过触摸板/鼠标点击聚焦
> - **按下** - 正在被按下

> **样式设置**: 样式可用于为小部件或其部分添加效果。您可以添加自定义背景颜色、边框、阴影等。在样式设置中，您可以添加或修改这些参数的值。
>
> **状态**: 您可以为每种状态创建自定义样式。
>
> **样式属性**: 样式属性是为样式设置的参数。
> - **弧形**: 弧形样式可用于具有弧形组件的小部件。
>    - **线条颜色** - 线条的颜色
>    - **弧宽度** - 弧的宽度
>    - **弧圆角** - 弧线的两端为圆角
>    - **弧图像** - 弧线的背景图像
> - **背景**: 背景样式是小部件的背景。您可以创建渐变或使背景的角变圆。
>   - **颜色和透明度** - 设置对象的背景颜色和透明度。
>   - **渐变颜色** - 设置背景的渐变颜色。
>   - **主渐变停止点** - 设置渐变背景颜色的起始点。
>   - **渐变停止点** - 设置背景渐变颜色的结束点。
>   - **背景半径** - 用于使背景角变圆的半径
>   - **渐变方向** - 渐变的方向。可以是水平或垂直。
>   - **裁剪角** - 启用后，裁剪圆角溢出的内容。
> - **背景图像**: 您可以将图像设置为背景图像。
>   - **背景图像** - 您用作背景的图像
>   - **背景图像透明度** - 背景图像的透明度
>   - **重新着色** - 使用重新着色功能，您可以在背景图像上使用一种颜色。通过更改 alpha 参数设置颜色深度。
>   - **背景图像平铺** - 如果启用，背景图像将被平铺
> - **混合**: 使用混合样式，您可以将当前小部件部分的像素颜色与后续对象的颜色混合。
>   - **混合模式** - 从四个选项中选择。
>       - **正常** - 默认状态
>       - **加法** - 像素相加
>       - **减法** - 像素相减
>       - **乘法** - 像素相乘
>   - **混合透明度** - 在此设置小部件部分的透明度
> - **边框**: 使用边框，您可以在选定对象的内线周围绘制边框。
>   - **边框颜色** - 边框的颜色
>   - **边框宽度** - 边框的宽度
>   - **边框方向** - 边框的方向
> - **线条**: 线条样式可用于具有线条组件的小部件。
>   - **颜色** - 线条的颜色
>   - **宽度** - 线条的宽度
>   - **线条圆角** - 线条的两端将为圆角
> - **轮廓**: 轮廓样式类似于边框样式，但这里您是在选定的小部件部分周围绘制边框。
>   - **轮廓颜色** - 轮廓的颜色
>   - **轮廓宽度** - 轮廓的宽度
>   - **轮廓间距** - 与小部件边缘的距离（以像素为单位）
> - **内边距**: 内边距样式为小部件部分添加内边距。这意味着在层级中位于其下方的部分将根据内边距的像素值移动。
>   - **内边距** - 内边距的范围
> - **阴影**: 使用阴影样式，您可以为选定的小部件部分绘制阴影或发光效果。
>   - **阴影颜色** - 阴影的颜色
>   - **阴影宽度** - 阴影的宽度
>   - **阴影扩展** - 阴影的深度
>   - **阴影 OX** - 在 X 轴上移动阴影
>   - **阴影 OY** - 在 Y 轴上移动阴影
> - **文本**: 文本样式定义小部件上文本的参数。
>   - **文本颜色** - 文本的颜色
>   - **字母间距** - 字母之间的间距
>   - **行间距** - 行之间的间距
>   - **文本对齐** - 文本对齐的方向
>   - **文本装饰** - 您可以为文本添加上划线或下划线
>       - **无** - 正常文本
>       - **下划线** - 带下划线的文本
>       - **删除线** - 带删除线的文本
>       - **文本字体** - 文本的字体
>
> **事件属性**: 添加事件后，您可以为小部件创建不同的交互，例如按下按钮时更改屏幕、播放动画等。
> - **添加事件**: 在检查器面板的底部，您可以找到“添加事件”按钮。首先，您应该为事件命名，然后选择触发器以启动它。
>   - **事件名称** - 事件的名称
>   - **事件触发器** - 事件启动的交互
>       - **按下** - 对象已被按下
>       - **点击** - 对象被按下短时间后释放。如果滚动，则不会调用
>       - **长按** - 对象已被按下较长时间
>       - **长按重复** - 在 `long_press_time` 后每隔 `long_press_repeat_time` 毫秒调用一次。如果滚动，则不会调用
>       - **聚焦** - 对象被聚焦
>       - **失焦** - 对象失去聚焦
>       - **值更改** - 对象的值已更改
>       - **准备就绪** - 进程已完成
>       - **取消** - 进程已取消
>       - **屏幕加载** - 屏幕已加载，当所有动画完成时调用
>       - **屏幕卸载** - 屏幕已卸载，当所有动画完成时调用
>       - **屏幕加载开始** - 屏幕加载开始，当屏幕更改延迟到期时触发
>       - **屏幕卸载开始** - 屏幕卸载开始，当调用 lv_scr_load/lv_scr_load_anim 时立即触发
>           - **选中** - 小部件被选中
>           - **未选中** - 小部件未被选中
>           - **手势** - 手指触摸滑动方向
> - **添加事件**
>   - **动作**: 动作是事件的元素，当触发器发生时启动。
>       - **调用函数**: 使用调用函数动作，您可以添加事件可以引用的函数名称。在导出过程中，此函数将被创建到 ui__events.c 或 ui_events.py 文件中。
>       - **更改屏幕**: 您可以使用此动作在屏幕之间切换。
>           - **目标屏幕** - 您想要切换到的屏幕
>           - **淡入模式** - 切换屏幕时的动画
>           - **速度** - 切换屏幕的速度
>           - **延迟** - 切换屏幕的延迟
>       - **增加弧值**: 您可以修改弧形小部件的值。
>       - **增加条形值**: 您可以修改条形小部件的值。
>       - **增加滑块值**: 您可以修改滑块小部件的值。
>       - **修改标志**: 您可以修改小部件的标志状态。
>       - **播放动画**: 您可以播放在动画面板中创建的动画。
>           - **动画** - 选定的动画
>           - **目标** - 您希望在其上使用动画的目标小部件
>           - **延迟** - 动画的延迟时间
>       - **设置透明度**: 选定小部件的透明度。
>       - **设置标志**: 设置小部件的标志状态值。
>       - **设置属性**: 更改小部件的属性值。
>       - **从弧形设置文本值**: 使用此动作在标签小部件上显示弧形小部件的值。
>       - **从滑块设置文本值**: 使用此动作在标签小部件上显示滑块小部件的值。
>       - **选中时设置文本值**: 根据目标对象的选中或未选中状态更改标签小部件的文本。

##### 面板使用

总结一下，如果我需要在表盘的上半部分显示专辑图片，那么我需要调整面板的坐标和大小，并设置背景图片。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/8.png" style={{width:1000, height:'auto'}}/></div>

:::note
为了尽可能减少主板上的内存占用，请尽量将图片插入到表盘的分辨率范围内进行压缩，不要使用大图片！
:::

然后我们可以在 **Bg Image opa** 中设置图片的透明度。我不希望它完全不透明，因为完全不透明的图片会影响后续文字的显示。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/9.png" style={{width:1000, height:'auto'}}/></div>

这里需要注意的是，所有组件默认都有边框线，因此为了不影响美观，我们需要移除边框线。移除的方法是将边框颜色的透明度设置为 0。

因此，**如果你想移除任何颜色或任何线段，可以通过将透明度设置为 0 来实现**。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/12.png" style={{width:600, height:'auto'}}/></div>

##### 标签使用

接着，我们在中间添加文字（使用 **Label** 小部件），显示艺术家和歌曲名称。在样式中，我们可以更改字体大小、颜色等内容。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/10.png" style={{width:1000, height:'auto'}}/></div>

##### Imgbutton 使用

在文字下方，我们添加一些播放组件（使用 **Imgbutton** 小部件），例如播放/暂停按钮和切换上一曲/下一曲按钮。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/11.png" style={{width:1000, height:'auto'}}/></div>

Imgbutton 是一种特殊的按钮，它与普通按钮最大的区别在于 Imgbutton 可以分别配置按钮按下前、按下时和释放后的三种状态的样式。这个过程非常类似于我们按下按钮切换播放状态的场景。如果你认为上下切换按钮不需要如此复杂的功能，也可以直接使用普通按钮。

接下来我们将按下和释放按钮的图片设置为播放样式，而仅在禁用状态下为暂停样式。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/13.png" style={{width:400, height:'auto'}}/></div>

然后我们添加一个事件，这个事件的功能是，当用户按下按钮时，状态切换为禁用，从而实现图片切换的效果。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/14.png" style={{width:500, height:'auto'}}/></div>

如果你想验证效果，可以点击工作区右上角的播放按钮，然后点击播放按钮查看切换效果。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/15.png" style={{width:600, height:'auto'}}/></div>

##### 弧形使用

接着我们添加最后一个组件，即音量条和播放进度条。我们将通过使用 Arc 来实现。

对于 Arc，我们主要需要调整的是环形的颜色和大小。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/16.png" style={{width:1000, height:'auto'}}/></div>

- MAIN：指的是整个 Arc 背后矩形背景的样式设计。MAIN 样式中配置的 **Arc** 并不代表弧形区域的样式。

    <div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/17.png" style={{width:1000, height:'auto'}}/></div>

- INDICATOR：指的是起始指示区域弧形的样式设置。一般使用 INDICATOR 下的 **Arc** 设置。

    <div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/18.png" style={{width:1000, height:'auto'}}/></div>

- KNOB：指的是图片上这个圆圈的配置。如果不需要这个圆圈，可以将其透明度设置为 0。这个圆圈的大小需要在 INDICATOR 中的 Arc 内设置。

    <div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/19.png" style={{width:1000, height:'auto'}}/></div>

这是我想要实现的效果。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/20.png" style={{width:1000, height:'auto'}}/></div>

如果你已经检查它是可点击的，那么你可以点击运行按钮并拖动弧形，这样就可以实现更改音量条的效果。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/21.png" style={{width:1000, height:'auto'}}/></div>

##### 屏幕切换

音乐界面设计得差不多了，我们不妨再添加一个新的主界面。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/22.png" style={{width:400, height:'auto'}}/></div>

然后设计一个事件，用于将主屏幕切换到音乐屏幕。例如，我在这里设计为在主界面下向右滑动手指切换到音乐播放界面。

由于主界面是主角，这个事件应该添加到主界面的 Screen 下。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/23.png" style={{width:500, height:'auto'}}/></div>

如果你想要一个滑动后缓慢切换的动画效果，那么 Speed 可以保持为 500；如果你想立即切换，那么 Speed 应设置为 0。

##### 指针动画

回到主界面的设计，我们希望添加表盘指针旋转的动画效果。

首先，你需要绘制自己的秒针、分针和时针。然后以 **Image** 的样式将其添加到主表盘中。

指针位置的调整需要耐心，因为我们需要确保指针围绕图片上的固定点旋转。

图像放置时需要设置 Transform。在下方的 Image 标签中，Pivot 设置旋转点的坐标。一般的设置方法是先调整 Transform，确保指针的固定点位于表盘中心，然后稍微调整 Pivot 的坐标。当所有角度都能使指针的固定点不移动时，这些参数就是最合适的。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/24.png" style={{width:1000, height:'auto'}}/></div>

一旦确定了所有指针的位置，就可以开始添加新的动画效果。不同指针的动画效果设置可以参考下图。

<table align="center">
	<tr>
	    <th>秒针</th>
	    <th>分针</th>
        <th>时针</th>
	</tr>
	<tr>
	    <td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/25.png" style={{width:300, height:'auto'}}/></div></td>
	    <td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/26.png" style={{width:300, height:'auto'}}/></div></td>
        <td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/27.png" style={{width:300, height:'auto'}}/></div></td>
	</tr>
</table>

最后，我们只需设置指针移动的动画在主屏加载时播放即可。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/28.png" style={{width:400, height:'auto'}}/></div>

:::tip
以上教程基本涵盖了软件使用场景的 80%，其他许多组件都非常相似。最后，这里有一些使用软件时的提示和建议。

1. 需要将所有具有触摸功能的组件放在顶部，否则会干扰组件触摸功能的实现。

    默认情况下，软件最后放置的组件位于顶部。这意味着在 Hierarchy 标签中，相邻排列的组件往往位于顶部。在刚设计的音乐界面中，顶层是用于调整音量大小的 Arc，这会导致一个问题，因为 Arc 是一个占据整个表盘的矩形，会影响播放按钮的触摸，因此通常需要将播放按钮的层级调整到 Arc 层的顶部，以免影响触摸。

    <div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/29.png" style={{width:400, height:'auto'}}/></div>

2. 尽可能关闭不需要的功能以节省更多内存。

    每个组件默认会勾选一些 Flags，但并非所有都需要。虽然默认选项不会有问题，但关闭不需要的 Flags 会让你的表盘 UI 运行更流畅。

    比如音乐界面中的背景专辑，不需要点击也没有动画，关闭 Flags 中的选项即可。但关闭时也需要考虑实际情况，例如在滑动切换表盘页面的场景中，关闭某些 Flags 的功能会导致滑动失效，因此仍需根据效果运行情况酌情关闭。

    <div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/30.png" style={{width:1000, height:'auto'}}/></div>

3. 确保为所有组件、动画、事件等赋予唯一名称。

    软件只能帮你节省绘制一些图案和动画的时间，但更复杂的效果可能需要你后续通过编程来实现。因此能够快速在代码中找到组件的位置非常重要。命名组件、事件和动画是关键！
:::

#### 第五步：程序的导出与应用

当你的表盘 UI 绘制完成后，可以考虑将 UI 导出为程序并通过 Arduino 上传到 XIAO。点击软件左上角，选择 **Export** -> **Create Template Project**。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/31.png" style={{width:400, height:'auto'}}/></div>

然后选择保存路径，代码会自动导出。导出的项目模板中会包含以下文件：

- **libraries**: 此文件夹目录提供了项目中需要使用的所有库。对于本教程，目录中的 *lvgl* 和 *TFT_eSPI* 库是 **不需要的**，但 *ui* 和 *lv_conf.h* 是 **有用的**。
- **ui**: 内部是 Arduino 的项目程序，即 .ino 文件。
- README.md

接下来，我们需要先将所需的库和配置文件放入 Arduino 的库文件夹中。然后修改 .ino 文件以确保功能正常运行。

请将 SquareLine Studio 导出的 **libraries** 文件夹目录中的 **ui** 文件夹和 **lv_conf.h** 文件复制到 Arduino 库的根目录中。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/101.png" style={{width:1000, height:'auto'}}/></div>

然后，我们可以直接打开 ui 文件夹下的 .ino 文件。接着，我们需要对以下文件进行修改以确保程序能够顺利编译。

- **ui.ino**:

<table align="center">
	<tr>
		<th>描述</th>
	    <th>截图</th>
	    <th>代码片段</th>
	</tr>
	<tr>
		<th>定义要使用的 TFT 库并导入圆形屏幕库</th>
		<td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/102.png" style={{width:600, height:'auto'}}/></div></td>
		<td><a href="https://github.com/limengdu/Seeed-Studio-XIAO-Round-Display-lvgl8.3.5/blob/f286996e967549de94891a63b58327e488bd46a3/ui/ui.ino#L1" target="_blank"><b>查看示例代码</b></a></td>
	</tr>
    <tr>
		<th>注释掉 tft 类的重复定义</th>
	    <td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/103.png" style={{width:600, height:'auto'}}/></div></td>
		<td><a href="https://github.com/limengdu/Seeed-Studio-XIAO-Round-Display-lvgl8.3.5/blob/f286996e967549de94891a63b58327e488bd46a3/ui/ui.ino#L20" target="_blank"><b>查看示例代码</b></a></td>
	</tr>
	<tr>
		<th>重写触摸功能</th>
	    <td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/104.png" style={{width:600, height:'auto'}}/></div></td>
		<td><a href="https://github.com/limengdu/Seeed-Studio-XIAO-Round-Display-lvgl8.3.5/blob/f286996e967549de94891a63b58327e488bd46a3/ui/ui.ino#L46" target="_blank"><b>查看示例代码</b></a></td>
	</tr>
	<tr>
		<th>添加初始化屏幕功能和初始化触摸功能</th>
	    <td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/105.png" style={{width:600, height:'auto'}}/></div></td>
		<td><a href="https://github.com/limengdu/Seeed-Studio-XIAO-Round-Display-lvgl8.3.5/blob/f286996e967549de94891a63b58327e488bd46a3/ui/ui.ino#L86" target="_blank"><b>查看示例代码</b></a></td>
	</tr>
	<tr>
		<th>屏幕旋转</th>
	    <td><div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/106.png" style={{width:600, height:'auto'}}/></div></td>
		<td><a href="https://github.com/limengdu/Seeed-Studio-XIAO-Round-Display-lvgl8.3.5/blob/f286996e967549de94891a63b58327e488bd46a3/ui/ui.ino#L94" target="_blank"><b>查看示例代码</b></a></td>
	</tr>
</table>

然后，您可以选择使用哪个 XIAO 来编译和上传。

### 关于 LVGL 库的自定义选项

如果在编译后遇到某些组件未定义的错误消息，那么很可能是您没有将 Arduino 库根目录中的 **lv_conf.h** 文件替换为 SquareLine Studio 导出的 **lv_conf.h** 文件。

为了节省主板内存，默认的 lv_conf.h 文件禁用了部分 lvgl 功能。但如果您在表盘设计中使用了这些功能，则需要手动启用它们。

- **Windows** 系统中 `lv_conf.h` 的默认路径为：

`C:\Users\{UserName}\Documents\Arduino\libraries`

- **MacOS** 系统中 `lv_conf.h` 的默认路径为：

`\Users\{UserName}\Documents\Arduino\libraries`

举个简单的例子，在上述示例中，我们使用了 `MONTSERRAT 8` 字体，但默认情况下该字体是关闭的，因此在编译过程中可能会出现错误。

此时，我们只需要将 lv_conf.h 文件中该字体后面的 0 改为 1，即表示启用该字体。

<div style={{textAlign:'center'}}><img src="https://files.seeedstudio.com/wiki/round_display_for_xiao/38.png" style={{width:600, height:'auto'}}/></div>

如果遇到类似错误，可以再次检查是否启用了相关的自定义功能。

### 基于 LVGL 的表盘程序

我们为圆形屏幕创建了两种表盘样式供用户参考。由于复杂的 UI 逻辑，这将需要 XIAO 主板具备一定的性能和内存。如果您的 XIAO 在编译以下表盘程序时出现内存不足错误，则可能需要升级您的 XIAO 或减少表盘的功能。

- 表盘样式 I：

<div class="github_container" style={{textAlign: 'center'}}>
    <a class="github_item" href="https://github.com/limengdu/Seeed-Studio-XIAO-Round-Display-lvgl8.3.5/tree/main/ui">
    <strong><span><font color={'FFFFFF'} size={"4"}> 下载代码</font></span></strong> <svg aria-hidden="true" focusable="false" role="img" className="mr-2" viewBox="-3 10 9 1" width={16} height={16} fill="currentColor" style={{textAlign: 'center', display: 'inline-block', userSelect: 'none', verticalAlign: 'text-bottom', overflow: 'visible'}}><path d="M8 0c4.42 0 8 3.58 8 8a8.013 8.013 0 0 1-5.45 7.59c-.4.08-.55-.17-.55-.38 0-.27.01-1.13.01-2.2 0-.75-.25-1.23-.54-1.48 1.78-.2 3.65-.88 3.65-3.95 0-.88-.31-1.59-.82-2.15.08-.2.36-1.02-.08-2.12 0 0-.67-.22-2.2.82-.64-.18-1.32-.27-2-.27-.68 0-1.36.09-2 .27-1.53-1.03-2.2-.82-2.2-.82-.44 1.1-.16 1.92-.08 2.12-.51.56-.82 1.28-.82 2.15 0 3.06 1.86 3.75 3.64 3.95-.23.2-.44.55-.51 1.07-.46.21-1.61.55-2.33-.66-.15-.24-.6-.83-1.23-.82-.67.01-.27.38.01.53.34.19.73.9.82 1.13.16.45.68 1.31 2.69.94 0 .67.01 1.3.01 1.49 0 .21-.15.45-.55.38A7.995 7.995 0 0 1 0 8c0-4.42 3.58-8 8-8Z" /></svg>
    </a>
</div>

<!-- - 表盘样式 II:

<div class="github_container" style={{textAlign: 'center'}}>
    <a class="github_item" href="https://github.com/limengdu/Seeed-Studio-XIAO-Round-Display-lvgl8.3.5/tree/main/tft_espi-base-dial">
    <strong><span><font color={'FFFFFF'} size={"4"}> 下载代码</font></span></strong> <svg aria-hidden="true" focusable="false" role="img" className="mr-2" viewBox="-3 10 9 1" width={16} height={16} fill="currentColor" style={{textAlign: 'center', display: 'inline-block', userSelect: 'none', verticalAlign: 'text-bottom', overflow: 'visible'}}><path d="M8 0c4.42 0 8 3.58 8 8a8.013 8.013 0 0 1-5.45 7.59c-.4.08-.55-.17-.55-.38 0-.27.01-1.13.01-2.2 0-.75-.25-1.23-.54-1.48 1.78-.2 3.65-.88 3.65-3.95 0-.88-.31-1.59-.82-2.15.08-.2.36-1.02-.08-2.12 0 0-.67-.22-2.2.82-.64-.18-1.32-.27-2-.27-.68 0-1.36.09-2 .27-1.53-1.03-2.2-.82-2.2-.82-.44 1.1-.16 1.92-.08 2.12-.51.56-.82 1.28-.82 2.15 0 3.06 1.86 3.75 3.64 3.95-.23.2-.44.55-.51 1.07-.46.21-1.61.55-2.33-.66-.15-.24-.6-.83-1.23-.82-.67.01-.27.38.01.53.34.19.73.9.82 1.13.16.45.68 1.31 2.69.94 0 .67.01 1.3.01 1.49 0 .21-.15.45-.55.38A7.995 7.995 0 0 1 0 8c0-4.42 3.58-8 8-8Z" /></svg>
    </a>
</div> -->

## 技术支持与产品讨论

感谢您选择我们的产品！我们为您提供多种支持，以确保您使用我们的产品时体验顺畅。我们提供多种沟通渠道，以满足不同的偏好和需求。

<div class="button_tech_support_container">
<a href="https://forum.seeedstudio.com/" class="button_forum"></a> 
<a href="https://www.seeedstudio.com/contacts" class="button_email"></a>
</div>

<div class="button_tech_support_container">
<a href="https://discord.gg/eWkprNDMU7" class="button_discord"></a> 
<a href="https://github.com/Seeed-Studio/wiki-documents/discussions/69" class="button_discussion"></a>
</div>