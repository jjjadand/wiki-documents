---
description: Grove Starter Kit for Azure Sphere MT3620 Development Kit
title: Grove Starter Kit for Azure Sphere MT3620 Development Kit
keywords:
- Azure_Sphere_MT3620_Development_Kit
image: https://files.seeedstudio.com/wiki/wiki-platform/S-tempor.png
slug: /Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit
last_update:
  date: 1/13/2023
  author: jianjing Huang
---


![enter image description here](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit.jpg)

This product is a basic starter kit for [Azure Sphere MT3620 Development kit](https://www.seeedstudio.com/Azure-Sphere-MT3620-Development-Kit-US-Version-p-3052.html). The kit integrates SeeedStudio’s most popular ready-to-use modular – [Grove](https://www.seeedstudio.com/grove.html), aims at helping developers get started quickly and experience the Azure Sphere solutions.

Microsoft Azure Sphere is a solution for creating highly-secured, connected, MCU-powered devices. Azure Sphere brings together the best of Microsoft’s expertise in cloud, software and silicon—resulting in a unique approach to security that starts in the silicon and extends to the cloud. Together, Azure Sphere certified MCUs, the Azure Sphere OS, and the Azure Sphere Security Service provide you with the confidence and the power to reimagine your business and create the future.

:::caution
Please note that Azure Sphere MT3620 Development kit is required but not included in this Starter kit.  You can purchase it [here](https://www.seeedstudio.com/s/Azure%20Sphere%20MT3620.html).
:::

<p style={{textAlign: 'center'}}><a href="https://www.seeedstudio.com/Grove-Starter-Kit-for-Azure-Sphere-MT3620-Development-Kit-p-3150.html" target="_blank"><img src="https://files.seeedstudio.com/wiki/Seeed-WiKi/docs/images/get_one_now_small.png" width="210" height="41"  border="0" /></a></p>

## Features

- Azure Sphere: End-to-end security for IoT devices
- Microsoft Visual Studio development environment
- Online authentication & updates for device lifetime

## Hardware Overview

**MT3620 Grove Shield**

Since [Azure Sphere SDK](http://aka.ms/AzureSphereSDK) has not yet supported ADC and I2C on MT3620. This shield serves as an interface between MT3620 UART port and external I2C devices, like the I2C temperature sensor. The basic function of the shield is to help to connect to I2C external device. By using an I2C compatible ADC chip, developer can also read the analog data from analog port.

[MT3620 Grove Shield](https://www.seeedstudio.com/MT3620-Grove-Shield-p-3145.html) includes 2 chips, AD7992(Anlog to I2C) and SC18IM700(I2C to UART) to enable the ADC and I2C function from hardware side. So the analog sensors' signal go through AD7992 and then SC18IM700 to UART of develop board. The I2C sensors go through SC18IM700 to UART of develop board as well.

The [AD7992](https://files.seeedstudio.com/wiki/Azure_Sphere_MT3620_Development_Kit/datasheet/AD7992.pdf) is a 12-bit, low power, successive approximation ADC with an I2C-compatible interface. It transforms the anlog signal A0, A1 to I2C data.

![](https://files.seeedstudio.com/wiki/Azure_Sphere_MT3620_Development_Kit/img/ADC_2_I2C.png)

The [SC18IM700](https://files.seeedstudio.com/wiki/Azure_Sphere_MT3620_Development_Kit/datasheet/SC18IM700.pdf) is designed to serve as an interface between the standard UART port of a microcontroller or microprocessor and the serial I2C-bus; this allows the microcontroller or microprocessor to communicate directly with other I2C-bus devices. It transforms SDA/SCL signals to GPIO26_TXD0 and GPIO28_RXD0.

![](https://files.seeedstudio.com/wiki/Azure_Sphere_MT3620_Development_Kit/img/I2C_2_UART.png)

![](https://files.seeedstudio.com/wiki/Azure_Sphere_MT3620_Development_Kit/img/MT3620_Grove_Shield-2018-09-11.png)

<div style={{textAlign: 'center'}}>MT3620 Grove Shield Hardware Overview</div>

**Part List**

<!-- <style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-xldj{border-color:inherit;:left}
.tg .tg-0pky{border-color:inherit;:left;vertical-align:top}
</style> -->

<table class="tg">
  <tr>
    <th class="tg-xldj">Category</th>
    <th class="tg-xldj">Module</th>
    <th class="tg-0pky">Description</th>
  </tr>
  <tr>
    <td class="tg-xldj">Shield</td>
    <td class="tg-xldj">MT3620 Grove shield</td>
    <td class="tg-0pky">Offers 1 x UART, 2 x I2C, 1 x Analog, 4 x GPIO</td>
  </tr>
  <tr>
    <td class="tg-xldj" rowspan="2">Analog Input</td>
    <td class="tg-xldj">Grove - Rotary Angle Sensor</td>
    <td class="tg-0pky">Produce an analog value, which can be read by ADC.</td>
  </tr>
  <tr>
    <td class="tg-xldj">Grove - Light Sensor v1.2</td>
    <td class="tg-0pky">Measuring light levels</td>
  </tr>
  <tr>
    <td class="tg-0pky" rowspan="2">Digital Output</td>
    <td class="tg-0pky">Grove – Buzzer</td>
    <td class="tg-0pky">Driven by a digital signal and emit a tone</td>
  </tr>
  <tr>
    <td class="tg-0pky">Grove - Relay</td>
    <td class="tg-0pky">Driven by a digital signal and switch much higher voltages</td>
  </tr>
  <tr>
    <td class="tg-0pky">Digital I/O</td>
    <td class="tg-0pky">Grove - Blue LED Button</td>
    <td class="tg-0pky">Button act as an input , LED as an output device</td>
  </tr>
  <tr>
    <td class="tg-0pky">Sensor</td>
    <td class="tg-0pky">Grove - Temp&amp;Humi Sensor(SHT31)</td>
    <td class="tg-0pky">Collects temperature and can be read via I2C interface</td>
  </tr>
  <tr>
    <td class="tg-0pky">Display</td>
    <td class="tg-0pky">Grove - OLED Display 1.12'' V2</td>
    <td class="tg-0pky">OLED can display character and picture</td>
  </tr>
</table>

## Application Ideas

- Home/Building/Facilities
- Automation
- Security
- Equipment Management
- Utilities
- Public Safety

:::tip
To understand how Azure Sphere works in a real-world setting, consider [Contoso, Ltds cenario](https://learn.microsoft.com/en-us/azure-sphere/product-overview/what-is-azure-sphere).
:::

## Getting Started

### Prerequisites

**Install Azure Sphere**

If you have an Azure Sphere development kit that has not yet been used, complete [these steps](https://docs.microsoft.com/en-us/azure-sphere/install/overview) first to get up and running.

**MT3620 Grove Shield Library**

This is library for Azure Sphere MT3620 Grove Shield, the shield enhences Azure Sphere by adding I2C interface and Analog input.Here is the [tourial](https://github.com/Seeed-Studio/MT3620_Grove_Shield/blob/master/README.md) how to use the [MT3620 Grove Shield Library](https://github.com/Seeed-Studio/MT3620_Grove_Shield).

### Analog Input

Here is the example how to read analog signal through MT3620 Base Shield.  

**Hardware Connection**

| MT3620 development kits | MT3620 Grove Shield |  Grove - Rotary Angle Sensor |
|--------------|-------------|-----------------|
|![enter image description here](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/azure_s.jpg)|![enter image description here](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/mt3620groveshieldb_s.jpg)|![enter image description here](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/rotation.jpg)|
|[Get One Now](https://www.seeedstudio.com/Azure-Sphere-MT3620-Development-Kit-p-3052.html)|[Get One Now](https://www.seeedstudio.com/MT3620-Grove-Shield-p-3145.html)|[Get One Now](https://www.seeedstudio.com/Grove-Rotary-Angle-Sensor-p-770.html)|

![](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/Rotation_shield.jpg)

- Step 1. Connect Grove - Rotary Angle Sensor to analog port of MT3620 Grove Shield.
- Step 2. Plug the MT3620 base shield to Azure Sphere MT3620 Development board.
- Step 3. Plug the USB cable to Azure Sphere MT3620 Development board and PC.

**Software**

- Step 1. Download [MT3620 Grove Shield Library](https://github.com/Seeed-Studio/MT3620_Grove_Shield)
- Step 2. Open the **Samples/GroveRotaryAngleSensor** under MT3620 Grove Shield
- Step 3. Double click **GroveRotaryAngleSensor.sln**
- Step 4. Right-Click the project name of your application, select **Properties > C/C++ > General > Additional Include Directories**
- Step 5. Click the **down arraw >  Edit... > New Line** icons, modify the path of  **MT3620_Grove_Shield_Library**, click **Select Folder** > **OK** > **OK**
- Step 6. Go to the application project Right-Click **References** > **Add References**, select **Projets**, click the check box of **MT3620_Grove_Shield_Library**, then click **OK**
- Step 7. Right-Click the project name of your application, select **General** > **Target API Set**, click the check box and set as **1** if you use the **Visual Studio Community version** and please skip this step if you use the Enterprise version.
- Step 8. Click the **Remote GDB Debugger**, then you rotate the sensor and you will see the output as below.

```
Remote debugging from host 192.168.35.1
Application starting
Angle Value 0.85
Angle Value 0.85
Angle Value 0.94
Angle Value 1.00
Angle Value 0.41
Angle Value 0.31
Angle Value 0.00
Angle Value 0.11
Angle Value 0.39
```

### Digital Output

Here is the example how to write digital signal through MT3620 Base Shield.  

**Hardware Connection**

| MT3620 development kits | MT3620 Grove Shield |  Grove - Buzzer |
|--------------|-------------|-----------------|
|![enter image description here](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/azure_s.jpg)|![enter image description here](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/mt3620groveshieldb_s.jpg)|![enter image description here](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/buzzer.jpg)|
|[Get One Now](https://www.seeedstudio.com/Azure-Sphere-MT3620-Development-Kit-p-3052.html)|[Get One Now](https://www.seeedstudio.com/MT3620-Grove-Shield-p-3145.html)|[Get One Now](https://www.seeedstudio.com/Grove-Buzzer-p-768.html)|

![](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/buzzer_shield.jpg)

- Step 1. Connect Grove - Buzzer to GPIO port of MT3620 Grove Shield.
- Step 2. Plug the MT3620 base shield to Azure Sphere MT3620 Development board.
- Step 3. Plug the USB cable to Azure Sphere MT3620 Development board and PC.

**Software**

- Step 1. Download [MT3620 Grove Shield Library](https://github.com/Seeed-Studio/MT3620_Grove_Shield)
- Step 2. Open the **Samples/Grove_Buzzer** under MT3620 Grove Shield
- Step 3. Double click **Grove_Buzzer.sln**
- Step 4. Right-Click the project name of your application, select **Properties > C/C++ > General > Additional Include Directories**
- Step 5. Click the **down arraw >  Edit... > New Line** icons, modify the path of  **MT3620_Grove_Shield_Library**, click **Select Folder** > **OK** > **OK**
- Step 6. Go to the application project Right-Click **References** > **Add References**, select **Projets**, click the check box of **MT3620_Grove_Shield_Library**, then click **OK**
- Step 7. Right-Click the project name of your application, select **General** > **Target API Set**, click the check box and set as **1** if you use the **Visual Studio Community version** and please skip this step if you use the Enterprise version.
- Step 8. Click the **Remote GDB Debugger**, you will see the output as below.

```
Remote debugging from host 192.168.35.1
Application starting
Relay on
Relay off
Relay on
Relay off
```

### I2C

Here is the example how to read Grove - Temp&Humi Sensor(SHT31) I2C signal through MT3620 Base Shield.  

**Hardware Connection**

| MT3620 development kits | MT3620 Grove Shield |  Grove - Temp&Humi Sensor(SHT31) |
|--------------|-------------|-----------------|
|![enter image description here](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/azure_s.jpg)|![enter image description here](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/mt3620groveshieldb_s.jpg)|![enter image description here](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/SHT3_s.jpg)|
|[Get One Now](https://www.seeedstudio.com/Azure-Sphere-MT3620-Development-Kit-p-3052.html)|[Get One Now](https://www.seeedstudio.com/MT3620-Grove-Shield-p-3145.html)|[Get One Now](https://www.seeedstudio.com/Grove-Temperature-Humidity-Sensor-SHT3-p-2655.html)|

![](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/img/sht31_shield.jpg)

- Step 1. Connect Grove - Temp&Humi Sensor(SHT31) to I2C port of MT3620 Grove Shield.
- Step 2. Plug the MT3620 base shield to Azure Sphere MT3620 Development board.
- Step 3. Plug the USB cable to Azure Sphere MT3620 Development board and PC.

**Software**

- Step 1. Download [MT3620 Grove Shield Library](https://github.com/Seeed-Studio/MT3620_Grove_Shield)
- Step 2. Open the **Samples/Temp_and_Humidity_SHT31** under MT3620 Grove Shield
- Step 3. Double click **Temp_and_Humidity_SHT31.sln**
- Step 4. Right-Click the project name of your application, select **Properties > C/C++ > General > Additional Include Directories**
- Step 5. Click the **down arraw >  Edit... > New Line** icons, modify the path of  **MT3620_Grove_Shield_Library**, click **Select Folder** > **OK** > **OK**
- Step 6. Go to the application project Right-Click **References** > **Add References**, select **Projets**, click the check box of **MT3620_Grove_Shield_Library**, then click **OK**
- Step 7. Right-Click the project name of your application, select **General** > **Target API Set**, click the check box and set as **1** if you use the **Visual Studio Community version** and please skip this step if you use the Enterprise version.
- Step 8. Click the **Remote GDB Debugger**, you will see the output as below.

```
Remote debugging from host 192.168.35.1
Application starting
Hello world
Temperature: 19.2C
Humidity: 53.7%
Hello world
Temperature: 19.2C
Humidity: 53.7%
Hello world
Temperature: 19.1C
Humidity: 53.7%
Hello world
Temperature: 19.2C
Humidity: 53.8%
```

## Schematic Online Viewer

<div className="altium-ecad-viewer" data-project-src="https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/res/MT3620GroveShield%20v1.0.zip" style={{borderRadius: '0px 0px 4px 4px', height: 500, borderStyle: 'solid', borderWidth: 1, borderColor: 'rgb(241, 241, 241)', overflow: 'hidden', maxWidth: 1280, maxHeight: 700, boxSizing: 'border-box'}}>
</div>

## Resource

- **[Eagle&PDF]** [MT3620 Grove Shield v1.0 Sch and PCB](https://files.seeedstudio.com/wiki/Grove_Starter_Kit_for_Azure_Sphere_MT3620_Development_Kit/res/MT3620GroveShield%20v1.0.zip)
- **[Library]** [MT3620 Grove Shield Library](https://github.com/Seeed-Studio/MT3620_Grove_Shield)
- **[Product]** [Azure Sphere MT3620 Development Kit Product Brief](https://files.seeedstudio.com/wiki/Azure_Sphere_MT3620_Development_Kit/product_document/Azure%20Sphere%20MT3620%20Development%20Kit%20Product%20Brief-2018-09-10.pdf)
- **[Product]** [Welcome to Azure Sphere](https://docs.microsoft.com/en-us/azure-sphere/)
- **[DataSheet]]** [MediaTek MT3620 Product Brief](https://files.seeedstudio.com/wiki/Azure_Sphere_MT3620_Development_Kit/datasheet/MediaTek%20MT3620%20Product%20Brief.pdf)
- **[DataSheet]** [DS_FT4232H](https://files.seeedstudio.com/wiki/Azure_Sphere_MT3620_Development_Kit/datasheet/DS_FT4232H.pdf)
- **[Mechanical]** [Azure Sphere MT3620 Development Board-2D-Drawing](https://github.com/SeeedDocument/Azure_Sphere_MT3620_Development_Kit/tree/master/mechanical)
- **[FAQ Web]** [Azure Sphere Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azuresphere)
- **[FAQ Web]** [Azure Sphere Github issues](https://github.com/MicrosoftDocs/azure-sphere-issues/issues?utf8=%E2%9C%93&q=is%3Aissue)

## Tech Support & Product Discussion

Thank you for choosing our products! We are here to provide you with different support to ensure that your experience with our products is as smooth as possible. We offer several communication channels to cater to different preferences and needs.

<div class="button_tech_support_container">
<a href="https://forum.seeedstudio.com/" class="button_forum"></a> 
<a href="https://www.seeedstudio.com/contacts" class="button_email"></a>
</div>

<div class="button_tech_support_container">
<a href="https://discord.gg/eWkprNDMU7" class="button_discord"></a> 
<a href="https://github.com/Seeed-Studio/wiki-documents/discussions/69" class="button_discussion"></a>
</div>

