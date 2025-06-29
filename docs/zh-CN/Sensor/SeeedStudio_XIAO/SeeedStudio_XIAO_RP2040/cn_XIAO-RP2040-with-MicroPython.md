---
description: Seeed Studio XIAO RP2040 使用 MicroPython
title: MicroPython
keywords:
- xiao
image: https://files.seeedstudio.com/wiki/wiki-platform/S-tempor.png
slug: /cn/XIAO-RP2040-with-MicroPython
last_update:
  date: 05/15/2025
  author: shuxu hu
---

# **Seeed Studio XIAO RP2040 使用 MicroPython**

:::note
本文档由 AI 翻译。如您发现内容有误或有改进建议，欢迎通过页面下方的评论区，或在以下 Issue 页面中告诉我们：https://github.com/Seeed-Studio/wiki-documents/issues
:::

## **MicroPython 简介**

[MicroPython](https://github.com/micropython/micropython/wiki) 是一个支持部分原生代码编译功能的 Python 解释器。它提供了 Python 3.5 的一个子集功能，专为嵌入式处理器和受限系统实现。它与 CPython 不同，您可以在 [这里](https://github.com/micropython/micropython/wiki/Differences) 阅读更多关于它们之间差异的信息。

<div align="center"><img width={500} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/micropython/MicroPython-Logo.png" /></div>

## **快速入门**

首先，我们将 Seeed Studio XIAO RP2040 连接到电脑，并上传一段简单的 MicroPython 代码，以检查开发板是否正常工作。

### **硬件设置**

- [Seeed Studio XIAO RP2040](https://www.seeedstudio.com/XIAO-RP2040-v1-0-p-5026.html) x1
- Type-C 数据线 x1
- 电脑 x1

### **软件设置**

- **步骤 1**. 根据您的操作系统下载并安装最新版本的 [Thonny 编辑器](https://thonny.org/)

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/micropython/board_6.png" /></div>

- **步骤 2**. 启动 Thonny

- **步骤 3**. 点击 **"Tools-->Options"** 打开设置。

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/micropython/board_8.png" /></div>

- **步骤 4**. 选择 "Interpreter" 界面，并将设备设置为 **"MicroPython(Raspberry Pi Pico)"**，端口设置为 **"Try to detect port automatically"**

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/micropython/board_9.png" /></div>

### **连接 Seeed Studio XIAO RP2040 到电脑并点亮**

- **步骤 1**. 按住 "BOOT" 按钮，然后通过 Type-C 数据线将 Seeed Studio XIAO RP2040 连接到电脑。如果连接正常，电脑上会显示一个名为 "RPI-RP2" 的磁盘。

<div align="center"><img width={300} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/desk.png" /></div>

- **步骤 2**. 点击 **Install or update MicroPython**。

<div align="center"><img width={800} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/installandupdate.png" /></div>

它会自动搜索设备并在目标卷中显示。在下面的 MicroPython 版本选择中，我们保持默认设置。

<div align="center"><img width={800} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/chooseversion.png" /></div>

点击 Install 按钮，当安装状态显示 Done 时关闭此页面。一旦固件完成，界面上会显示以下信息。

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/desk3.png" /></div>

- **步骤 3**. 将以下代码复制到 Thonny。

```cpp
from machine import Pin, Timer
 
led = Pin(25, Pin.OUT)
Counter = 0
Fun_Num = 0
 
def fun(tim):
    global Counter
    Counter = Counter + 1
    print(Counter)
    led.value(Counter%2)
 
tim = Timer(-1)
tim.init(period=1000, mode=Timer.PERIODIC, callback=fun)
```

- **步骤 4**. 点击 "Run current script" 按钮上传代码。第一次运行时，Thonny 会询问您将代码文件保存在哪里。**This Computer** 和 **Raspberry Pi Pico** 都可以。

<div align="center"><img width={300} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/desk7.png" /></div>

如果运行正常，您会看到 LED 灯每秒亮灭一次。同时，Shell 中会显示递增的数字输出。

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/micropython/board_13.png" /></div>

连接完成，现在我们可以继续进行其他项目。

## **点亮 Seeed Studio XIAO RP2040 上的 RGB LED**

Seeed Studio XIAO RP2040 配备了一个 RGB LED，我们将通过 MicroPython 将其点亮。此过程需要一个第三方库，因此我们需要先添加一个额外的库。

- **步骤 1**. 下载 [ws2812.py](https://files.seeedstudio.com/wiki/XIAO-RP2040/img/micropython/ws2812.py) 库并用 Thonny 打开。

- **步骤 2**. 点击 "File-->Save as" 并保存库文件。

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/desk4.png" /></div>

选择 "Raspberry Pi Pico" 作为保存位置。

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/desk5.png" /></div>

确保保存的文件名为 "ws2812.py"，否则将无法正常工作。

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/desk6.png" /></div>

- **步骤 3**. 将以下代码复制到 Thonny。

```cpp
from ws2812 import WS2812
import utime
import machine
power = machine.Pin(11,machine.Pin.OUT)
power.value(1)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
YELLOW = (255, 150, 0)
GREEN = (0, 255, 0)
CYAN = (0, 255, 255)
BLUE = (0, 0, 255)
PURPLE = (180, 0, 255)
WHITE = (255, 255, 255)
COLORS = (BLACK, RED, YELLOW, GREEN, CYAN, BLUE, PURPLE, WHITE)

led = WS2812(12,1)#WS2812(pin_num,led_count)

while True:
    print("Beautiful color")
    for color in COLORS: 
        led.pixels_fill(color)
        led.pixels_show()
        utime.sleep(0.2)
```

- **步骤 4**. 点击 "Run current script" 按钮上传代码。第一次运行时，Thonny 会询问您将代码文件保存在哪里。**This Computer** 和 **Raspberry Pi Pico** 都可以。

<div align="center"><img width={300} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/desk7.png" /></div>

如果运行正常，您会看到 RGB LED 变换并闪烁不同的颜色。同时，Shell 中会显示文本 "Beautiful Color" 的输出。

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/micropython/board_14.png" /></div>

## **通过 IIC 接口连接 Seeed Studio XIAO RP2040 和显示屏**

在本项目中，我们将通过 IIC 接口连接 [Grove - OLED Display 0.96" (SSD1315)](https://www.seeedstudio.com/Grove-OLED-Display-0-96-SSD1315-p-4294.html)，以演示 Seeed Studio XIAO RP2040 的 IIC 功能。

**硬件连接**

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/micropython/board_15.png" /></div>

- **步骤 1**. 下载 [ssd1306.py](https://files.seeedstudio.com/wiki/XIAO-RP2040/img/micropython/ssd1306.py) 库，并用 Thonny 打开它。

- **步骤 2**. 点击 "File-->Save as"，并将库保存到 "Raspberry Pi Pico"。

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/desk4.png" /></div>

选择 "Raspberry Pi Pico" 作为保存位置。

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/desk5.png" /></div>

确保保存的文件名为 "ssd1306.py"，否则将无法正常工作。

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/desk8.png" /></div>

- **步骤 3**. 将以下代码复制到 Thonny。

```cpp
from ssd1306 import SSD1306_I2C
from machine import Pin, I2C
from time import sleep

i2c = I2C(1, scl=Pin(7), sda=Pin(6), freq=200000)#Grove - OLED Display 0.96" (SSD1315)
oled = SSD1306_I2C(128, 64, i2c)

while True:  
    oled.fill(0)#清屏
    oled.text("Hello,World!",0,0)
    oled.show()
    #sleep(0.5)
```

- **步骤 4**. 点击 "Run current script" 按钮上传代码。第一次运行时，Thonny 会询问您希望将代码文件保存在哪里。**This Computer** 和 **Raspberry Pi Pico** 都可以。

<div align="center"><img width={300} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/desk7.png" /></div>

如果一切正常，您将在屏幕上看到显示的文本 "Hello,World!"。

<div align="center"><img width={550} src="https://files.seeedstudio.com/wiki/XIAO-RP2040/img/micropython/board_16.png" /></div>

## MicroPython 设备控制台

我们的合作伙伴 **Neil** 为 XIAO 编写了一个基于 MicroPython 的命令行控制台程序。通过该程序，您可以轻松上传、下载和删除文件。我们感谢他对 XIAO 的贡献！

<div class="get_one_now_container" style={{textAlign: 'center'}}><a class="get_one_now_item" href="https://gitlab.cba.mit.edu/pub/upy/-/blob/main/upy.py"><strong><span><font color={'FFFFFF'} size={"4"}>📚 了解更多</font></span></strong></a></div>

## 其他资源

以下是一些额外的库和示例代码：

- **[ZIP]** [XIAO-RP2040-MicroPython-Grove.zip](https://files.seeedstudio.com/wiki/XIAO-RP2040/img/micropython/XIAO-RP2040-MicroPython-Grove.zip)

## 技术支持与产品讨论

感谢您选择我们的产品！我们为您提供多种支持渠道，以确保您使用我们的产品时体验顺畅。我们提供多种沟通方式，以满足不同的偏好和需求。

<div class="button_tech_support_container">
<a href="https://forum.seeedstudio.com/" class="button_forum"></a> 
<a href="https://www.seeedstudio.com/contacts" class="button_email"></a>
</div>

<div class="button_tech_support_container">
<a href="https://discord.gg/eWkprNDMU7" class="button_discord"></a> 
<a href="https://github.com/Seeed-Studio/wiki-documents/discussions/69" class="button_discussion"></a>
</div>