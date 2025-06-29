---
description: Seeed Studio XIAO ESP32C6 使用 CircuitPython
title: XIAO ESP32C6 使用 CircuitPython
keywords:
- xiao
image: https://files.seeedstudio.com/wiki/esp32c6_circuitpython/title.png
slug: /cn/xiao_esp32c6_with_circuitpython
last_update:
  date: 05/15/2025
  author: Evelyn Chen
---

# **Seeed Studio XIAO ESP32C6 使用 CircuitPython**

:::note
本文档由 AI 翻译。如您发现内容有误或有改进建议，欢迎通过页面下方的评论区，或在以下 Issue 页面中告诉我们：https://github.com/Seeed-Studio/wiki-documents/issues
:::

<div align="center"><img width={800} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/title.png" /></div>

本篇 Wiki 介绍如何在 Seeed Studio XIAO ESP32C6 开发板上安装并运行 Adafruit Industries 官方的 CircuitPython！  
CircuitPython 是一种编程语言，旨在简化在低成本微控制器板上进行实验和学习编程的过程。它使入门变得前所未有的简单，无需预先下载桌面软件。设置好开发板后，只需打开任意文本编辑器，即可开始编辑代码。更多信息，请参考 [这里](https://learn.adafruit.com/welcome-to-circuitpython/what-is-circuitpython)。

## 安装 CircuitPython

### 方法 1：使用命令行工具 esptool

#### 安装 Esptool
如果尚未安装 esptool.py，可以通过 pip 在您的电脑上安装：
``` linux
pip install esptool
```

#### 下载 ESP32C6 的 CircuitPython 固件
您需要从 [circirtpython.org](https://circuitpython.org/board/seeed_xiao_esp32c6/) 下载固件二进制文件。  
下载正确的 bin 文件后，导航到该文件所在的文件夹，并在该文件夹中打开命令行终端。  
截至最终草稿，最新的 bin 文件版本为：
```
adafruit-circuitpython-seeed_xiao_esp32c6-en_GB-9.1.1.bin
```

#### 将 XIAO ESP32C6 连接到您的电脑
您需要按住 XIAO ESP32C6 开发板上的 BOOT 按钮，同时将其通过 Type-C USB 数据线连接到电脑，以进入“bootloader”模式。

#### 检查端口
查看电脑上所有的串口设备。

* Linux

在 Linux 上，可以使用 *dmesg* 命令查看连接的设备：
```Linux
dmesg | grep tty
```
或者，您可以使用 *ls* 列出串口设备：
```
ls /dev/ttyS* /dev/ttyUSB*
```

* Windows

在 Windows 上，可以通过设备管理器检查串口。在“端口 (COM 和 LPT)”部分查看可用的串口。  
也可以在命令提示符中使用 mode 命令列出串口：
```
mode
```

* macOS

在 macOS 上，可以使用 *ls* 命令列出可用的串口：
```
ls /dev/cu*
```
这将显示所有串口设备。
<div align="center"><img width={600} src="https://files.seeedstudio.com/wiki/esp32c3_circuitpython/1.png" /></div>

:::提示
如果端口被占用，可以使用以下命令查找并终止占用该端口的进程（适用于 macOS）：
识别使用该端口的进程：
```
lsof | grep port
```
此命令列出打开的文件并搜索使用指定端口的任何进程。  
找到输出中的进程 ID (PID)，然后终止该进程：
```
kill -9 <PID>
```
将 *PID* 替换为实际的进程 ID。

#### 擦除 Flash
```linux
esptool.py --chip esp32c6 --port /dev/cu.usbmodem11301 erase_flash
```
将 '/dev/cu.usbmodem11301' 替换为您系统中的正确端口名称（例如，Windows 上为 `COM3`，Linux 上为 `/dev/ttyUSB0`）。

#### 写入 Flash
将固件烧录到 XIAO ESP32C6：
```linux
esptool.py --chip esp32c6 --port /dev/cu.usbmodem11301 --baud 460800 write_flash -z 0x0 adafruit-circuitpython-seeed_xiao_esp32c6-en_GB-9.1.1.bin
```
同样，将 '/dev/cu.usbmodem11301' 替换为正确的端口名称，将 'adafruit-circuitpython-seeed_xiao_esp32c6-en_GB-9.1.1.bin' 替换为固件文件的路径。  
硬件复位通过 RTS 引脚完成...

### 方法 2：Web Serial esptool
WebSerial ESPTool 是为编程基于 Espressif ESP 系列微控制器板的串口 ROM 引导加载程序设计的 Web 选项。它允许您擦除微控制器的内容，并在不同的偏移量处编程最多 4 个文件。请参考 [Web Serial ESPtool](https://learn.adafruit.com/circuitpython-with-esp32-quick-start/web-serial-esptool)。

然后，您可以使用首选工具开始为 ESP32C6 编写脚本！

## CircuitPython 推荐编辑器

通常，当 CircuitPython 安装完成后，或者您将已安装 CircuitPython 的开发板插入电脑时，开发板会作为名为 CIRCUITPY 的 USB 驱动器显示在电脑上。  
然而，不支持原生 USB 的 ESP32 / ESP32-C3 / ESP32-C6 微控制器无法呈现 CIRCUITPY 驱动器。  
在这些开发板上，有其他方式传输和编辑文件。您可以使用 [Thonny](https://thonny.org/)，它通过发送隐藏命令到 REPL 来读取和写入文件。或者，您可以使用 CircuitPython 8 引入的 [CircuitPython Web Workflow](https://code.circuitpython.org/)，它提供基于浏览器的 WiFi 访问 CircuitPython 文件系统的功能。请参考 [使用代码编辑器入门 Web Workflow](https://learn.adafruit.com/getting-started-with-web-workflow-using-the-code-editor/overview)。

### 1. Thonny
安装并打开 Thonny，然后按照以下说明配置 Thonny：
```
pip install thonny
#安装完成后打开 thonny
thonny
```
进入 Run-->Configure Interpreter，确保 Thonny 选项中的 Interpreter 选项卡如下图所示，选择 "CircuitPython (generic)" 和端口：
<div align="center"><img width={600} src="https://files.seeedstudio.com/wiki/esp32c3_circuitpython/2.png" /></div>

点击对话框中的 "OK"，您将看到 Thonny 窗口底部的 Micropython shell，如下图所示。  
然后，您可以使用 **R**ead-**E**valuate-**P**rint-**L**oop（REPL）进行串口连接，这允许您输入单独的代码行并立即在 shell 中运行。  
这对于调试特定程序并找出问题非常有用。它是交互式的，因此非常适合测试新想法。请参考 [REPL](https://learn.adafruit.com/welcome-to-circuitpython/the-repl) 获取更多信息。

通过输入 *help()* 与 REPL 交互，这将告诉您从哪里开始探索 REPL。要在 REPL 中运行代码，请在 REPL 提示符旁输入代码。  
输入 *help("modules")* 列出内置模块，这将显示 CircuitPython 中所有核心模块的列表，包括 "*board*"。
<div align="center"><img width={600} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/3.png" /></div>

然后在 REPL 中输入 "*import board*" 并按回车。接下来，在 REPL 中输入 "*dir(board)*"，即可获取开发板上所有引脚的列表。
```
# 使用以下命令检查引脚。有关 REPL 的详细信息，请参阅 Welcome to CircuitPython!
# 进入 Thonny 的 shell。
>>> import board
>>> dir(board)
['__class__', '__name__', 'A0', 'A1', 'A2', 'A4', 'A5', 'A6', 'D0', 'D1', 'D10', 'D2', 'D3', 'D4', 'D5', 'D6', 'D7', 'D8', 'D9', 'I2C', 'LP_I2C_SCL', 'LP_I2C_SDA', 'LP_UART_RXD', 'LP_UART_TXD', 'MISO', 'MOSI', 'MTCK', 'MTDI', 'MTDO', 'MTMS', 'RX', 'SCK', 'SCL', 'SDA', 'SPI', 'TX', 'UART', '__dict__', 'board_id']
```

### 2. CircuitPython Web Workflow
<div align="center"><img width={600} src="https://files.seeedstudio.com/wiki/esp32c3_circuitpython/5.png" /></div>
[The CircuitPython Code Editor](https://code.circuitpython.org/) 提供了一个更全面、更丰富的体验，用于编辑运行最新版本 CircuitPython 的基于 ESP32 的设备上的文件。
该编辑器允许您通过 Web Bluetooth、USB 和 WiFi 上的 Web Workflow 来编辑文件。

## 引脚/接口信息
<div align="center"><img width={600} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/5.png" /></div>

* 更多信息请参考 [硬件概览](https://wiki.seeedstudio.com/xiao_esp32c6_getting_started/#hardware-overview)
* [Seeed Studio XIAO ESP32C6 原理图](https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32C6/XIAO-ESP32-C6_v1.0_SCH_PDF_24028.pdf)

## 在 XIAO ESP32C6 上使用 CircuitPython 入门

### 网络-WLAN

对于没有原生 USB 的开发板（如 ESP32-C6 或 ESP32），您需要通过 REPL 来连接 Wi-Fi。当在 CircuitPython 文件系统的根目录中添加名为 *settings.toml* 的文件时，Wi-Fi 功能将被启用。

#### 方法 1：通过 REPL 创建 *settings.toml* 文件

通过 REPL 创建 *settings.toml* 文件：
```r
f = open('settings.toml', 'w')
f.write('CIRCUITPY_WIFI_SSID = "wifissid"\n')
f.write('CIRCUITPY_WIFI_PASSWORD = "wifipassword"\n')
f.write('CIRCUITPY_WEB_API_PASSWORD = "webpassword"\n')
f.close()
```
* 将 *wifissid* 替换为您本地 WiFi 网络的名称
* 将 *wifipassword* 替换为您本地 WiFi 的密码
* 另一个密码 *webpassword* 用于通过网络浏览器访问开发板。设置为您想要的任何值

连接成功后，您可以按下 **Reset** 按钮以启动固件，然后多次按回车键以进入 REPL 提示符。然后重新连接设备到 Thonny，XIAO ESP32C6 的 IP 地址将显示出来。

#### 方法 2：通过 Thonny 文件编辑 *settings.toml* 文件
打开 Thonny-->View-->Files，并将 WiFi 网络、密码和 Web 密码写入 *settings.toml* 文件。记得保存并按下 Reset 按钮以启动固件，然后重新打开 Thonny。

<div align="center"><img width={300} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/6.png" /></div>
<div align="center"><img width={600} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/7.png" /></div>
:::note
请注意，ESP32 不支持 5 GHz 网络，因此如果您有两个网络，请使用 2.4 GHz 的 SSID。
:::
<div align="center"><img width={600} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/8.png" /></div>

### 延时和计时
*time* 模块：
```python
import time
dir(time)
time.sleep(1)           # 休眠 1 秒
time.localtime()        # 获取本地时间
```

### 引脚和 GPIO
可以使用 "*board*" 和 "*microcontroller*" 模块通过以下代码控制 GPIO，并将一个 LED 连接到 D5：
<div align="center"><img width={600} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/16.png" /></div>

```python 
# 使用 board 模块
import board
import digitalio
import time

led = digitalio.DigitalInOut(board.D5)
led.direction = digitalio.Direction.OUTPUT

while True:
    led.value = True  # 打开 LED
    time.sleep(1)
    led.value = False  # 关闭 LED
    time.sleep(1)
    
# 使用 microcontroller 模块
import microcontroller
import digitalio
import time

led = digitalio.DigitalInOut(microcontroller.pin.GPIO23)
led.direction = digitalio.Direction.OUTPUT

while True:
    led.value = True  # 打开 LED
    time.sleep(1)
    led.value = False  # 关闭 LED
    time.sleep(1)
```
<div align="center"><img width={600} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/9.png" /></div>

### UART（串行总线）
使用 *busio* 模块：
```python
import board
import busio

# 初始化 UART
uart = busio.UART(board.LP_UART_TXD, board.LP_UART_RXD, baudrate=9600)

# 发送数据
uart.write(b"Hello UART\n")

# 接收数据
while True:
    if uart.in_waiting > 0:
        data = uart.read()
        print("Received:", data)

```
XIAO ESP32C6 有一个硬件 UART，相关引脚如下：
| UART       | 引脚   | GPIO  |
|------------|-------|-------|
| LP_UART_TXD | A5    | GPIO5 |
| LP_UART_RXD | A4    | GPIO4 |

### PWM（脉宽调制）

使用 *pwmio* 模块：
```python
import board
import pwmio
from digitalio import DigitalInOut
import time

# 初始化 PWM
pwm = pwmio.PWMOut(board.D5, frequency=5000, duty_cycle=0)

# 一个渐变亮度的 LED
while True:
    for duty_cycle in range(0, 65535, 1000):
        pwm.duty_cycle = duty_cycle
        time.sleep(0.1)
```

### ADC（模拟到数字转换）
使用 *analogio* 模块：
```python
import board
import analogio
import time

# 初始化 ADC
adc = analogio.AnalogIn(board.A0)

while True:
    value = adc.value
    print("ADC Value:", value)
    time.sleep(1)

```
### SPI
```python
import board
import busio
import digitalio

# 初始化 SPI
spi = busio.SPI(board.SCK, board.MOSI, board.MISO)
# 调用 try_lock（稍后调用 unlock）以确保您是 SPI 总线的唯一用户
spi.try_lock()

# 选择一个片选引脚
cs = digitalio.DigitalInOut(board.D3)
cs.direction = digitalio.Direction.OUTPUT
cs.value = True

# 在通信前确保片选处于活动状态（低电平）
cs.value = False

# 发送和接收数据
data_out = bytearray([0x01, 0x02, 0x03])
data_in = bytearray(3)

try:
    # 写入和读取数据
    spi.write_readinto(data_out, data_in)
    print("Received:", data_in)
finally:
    # 确保通信后片选处于非活动状态（高电平）
    cs.value = True
```
XIAO ESP32C6 有一个硬件 SPI。相关引脚如下：

| SPI  | 引脚 |
|------|-----|
| SCK  | D8  |
| MOSI | D10 |
| MISO | D9  |

### I2C
```python
import board
import busio

# 初始化 I2C
i2c = busio.I2C(board.SCL, board.SDA, frequency=400000)
```
### XIAO 扩展板底座
*先决条件*：

<table align="center">
  <tbody><tr>
      <th>XIAO ESP32C6<br /> 带焊接针脚</th>
      <th>XIAO 扩展板底座</th>
      <th>Grove 光传感器</th>
    </tr>
    <tr>
      <td><div align="center"><img src="https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32C6/img/xiaoc6.jpg" style={{width:210, height:'auto'}}/></div></td>
      <td><div align="center"><img src="https://files.seeedstudio.com/wiki/esp32c3_circuitpython/15.png" style={{width:210, height:'auto'}}/></div></td>
      <td><div align="center"><img src="https://files.seeedstudio.com/wiki/esp32c3_circuitpython/16.png" style={{width:210, height:'auto'}}/></div></td>
    </tr>
    <tr>
        <td align="center"><div class="get_one_now_container" style={{textAlign: 'center'}}>
            <a class="get_one_now_item" href="https://www.seeedstudio.com/Seeed-Studio-XIAO-ESP32C6-p-5884.html">
            <strong><span><font color={'FFFFFF'} size={"4"}> 立即购买 🖱️</font></span></strong>
            </a>
        </div></td>
        <td align="center"><div class="get_one_now_container" style={{textAlign: 'center'}}>
            <a class="get_one_now_item" href="https://www.seeedstudio.com/Seeeduino-XIAO-Expansion-board-p-4746.html">
            <strong><span><font color={'FFFFFF'} size={"4"}> 立即购买 🖱️</font></span></strong>
            </a>
        </div></td>
        <td align="center"><div class="get_one_now_container" style={{textAlign: 'center'}}>
            <a class="get_one_now_item" href="https://www.seeedstudio.com/Grove-Light-Sensor-v1-2-LS06-S-phototransistor.html">
            <strong><span><font color={'FFFFFF'} size={"4"}> 立即购买 🖱️</font></span></strong>
            </a>
        </div></td>
    </tr>
  </tbody></table>

#### 读取光传感器数据
<div align="center"><img width={300} src="https://files.seeedstudio.com/wiki/esp32c3_circuitpython/9.png" /></div>

```python
import time
import board
import analogio

# 初始化 A0 上的模拟输入
analog_in = analogio.AnalogIn(board.A0)

def get_voltage(pin):
    return (pin.value * 3.3) / 65536

while True:
    # 读取原始模拟值
    raw_value = analog_in.value
    # 将原始值转换为电压
    voltage = get_voltage(analog_in)
    
    # 将原始值和电压打印到串行控制台
    print("[Light] 原始值: {:5d} 电压: {:.2f}V".format(raw_value, voltage))
    
    # 在再次读取之前延迟一段时间
    time.sleep(1)
```
<div align="center"><img width={600} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/11.png" /></div>

#### 点亮 OLED 屏幕
**下载并解压库包**：
* 前往 [库](https://circuitpython.org/libraries) 页面，下载适用于 CircuitPython 的库包。根据您的 CircuitPython 版本下载相应的库包。

**将库复制到 CIRCUITPY**：
* 解压库包 ZIP 文件。您会发现一个名为 lib 的文件夹，其中包含多个 *.mpy* 文件。
* 打开 Thonny-->View-->Files，然后将所需的 .mpy 文件和 lib 文件夹复制到 CircuitPython 设备的 lib 文件夹中。
您需要从库包中手动安装以下必要的库：
  * adafruit_ssd1306
  * adafruit_bus_device
  * adafruit_register
  * adafruit_framebuf.mpy
  
**将 font5x8.bin 复制到 CIRCUITPY**：
* 从 [这里](https://github.com/adafruit/Adafruit_CircuitPython_framebuf/blob/main/examples/font5x8.bin) 下载 font5x8.bin 文件。
* 打开 Thonny-->View-->Files，然后将 font5x8.bin 文件复制到 CircuitPython 设备中。

<div align="center"><img width={300} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/12.png" /></div>
<div align="center"><img width={600} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/13.png" /></div>
<div align="center"><img width={300} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/14.png" /></div>

**创建您的 CircuitPython 代码**：
* 创建一个 code.py 文件（或 main.py）。此文件应包含您的 CircuitPython 代码。
```python 
import board
import busio
import displayio
import adafruit_ssd1306
import terminalio

# 初始化 I2C
i2c = busio.I2C(board.SCL, board.SDA)

# 定义显示参数
oled_width = 128
oled_height = 64

# 初始化 OLED 显示屏
oled = adafruit_ssd1306.SSD1306_I2C(oled_width, oled_height, i2c)

# 用颜色 0 填充显示屏
oled.fill(0)
# 将第一个像素设置为白色
oled.pixel(0, 0, 1)
oled.show()
```
<div align="center"><img width={500} src="https://files.seeedstudio.com/wiki/esp32c6_circuitpython/15.png" /></div>

## “卸载” CircuitPython
我们的许多开发板可以支持多种编程语言。例如，Circuit Playground Express 可以与 MakeCode、Code.org CS Discoveries、CircuitPython 和 Arduino 一起使用。您可能希望切换回 Arduino 或 MakeCode。实际上无需卸载任何内容。CircuitPython 就是加载到开发板中的“另一个程序”。因此，您只需加载另一个程序（如 Arduino 或 MakeCode），它将覆盖 CircuitPython。

### 备份您的代码
在替换 CircuitPython 之前，请不要忘记备份 CIRCUITPY 驱动器上的代码。这包括您的 *code.py* 文件以及其他文件、lib 文件夹等。如果您移除 CircuitPython，可能会丢失这些文件，因此备份非常重要！只需像使用任何 USB 驱动器一样，将文件拖到笔记本电脑或台式电脑上的文件夹中即可。

### 切换到 Arduino
如果您想使用 Arduino，只需使用 Arduino IDE 加载一个 Arduino 程序即可。以下是上传一个简单的“Blink” Arduino 程序的示例，但您不必使用此特定程序。
首先，将开发板插入电脑，并双击复位按钮，直到板载 LED 开始闪烁。

感谢您阅读本文！欢迎在评论中分享您的想法。

## 资源
* [适用于 XIAO ESP32C6 的 CircuitPython 固件二进制文件](https://circuitpython.org/board/seeed_xiao_esp32c6/)
* [CircuitPython 的库包](https://circuitpython.org/libraries)

## 技术支持与产品讨论

感谢您选择我们的产品！我们致力于为您提供各种支持，以确保您使用我们的产品时能够获得流畅的体验。我们提供多个沟通渠道，以满足不同的偏好和需求。

<div class="button_tech_support_container">
<a href="https://forum.seeedstudio.com/" class="button_forum"></a> 
<a href="https://www.seeedstudio.com/contacts" class="button_email"></a>
</div>

<div class="button_tech_support_container">
<a href="https://discord.gg/eWkprNDMU7" class="button_discord"></a> 
<a href="https://github.com/Seeed-Studio/wiki-documents/discussions/69" class="button_discussion"></a>
</div>