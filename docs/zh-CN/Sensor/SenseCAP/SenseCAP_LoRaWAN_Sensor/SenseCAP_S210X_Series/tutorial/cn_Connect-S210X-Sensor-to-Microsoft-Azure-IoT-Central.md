---
description: 通过 Node-RED 将 S210X 传感器连接到 Microsoft Azure IoT Central
title: 通过 Node-RED 将 S210X 传感器连接到 Microsoft Azure IoT Central
keywords:
- SenseCAP LoRaWAN 传感器 & Microsoft Azure IoT Central
image: https://files.seeedstudio.com/wiki/wiki-platform/S-tempor.png
slug: /cn/Sensor/SenseCAP/SenseCAP_LoRaWAN_Sensor/SenseCAP_S210X_Series/tutorial/Connect-S210X-Sensor-to-Microsoft-Azure-IoT-Central
last_update:
  date: 05/15/2025
  author: Jessie
---
:::note
本文档由 AI 翻译。如您发现内容有误或有改进建议，欢迎通过页面下方的评论区，或在以下 Issue 页面中告诉我们：https://github.com/Seeed-Studio/wiki-documents/issues
:::

SenseCAP S210X 是一系列无线 LoRaWAN® 传感器。它在城市场景中可覆盖 2 公里的传输范围，在视距场景中可覆盖 10 公里，同时在传输过程中保持低功耗。配备可更换电池，支持长达 10 年的使用寿命，并采用工业级 IP66 外壳。支持 -40 ~ 85℃ 的工作温度，可部署在恶劣环境中。SenseCAP S210X 兼容 LoRaWAN® V1.0.3 协议，并可与 LoRaWAN® 网关协同工作。用户可以安装设备，通过二维码绑定并配置网络，然后可以通过支持 HTTP 和 MQTT 等流行物联网协议的 SenseCAP 门户查看数据。

<p style={{textAlign: 'center'}}><img src="https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/001.png" alt="pir" width={600} height="auto" /></p>

<p style={{textAlign: 'center'}}><a href="https://www.seeedstudio.com/catalogsearch/result/?q=S210x" target="_blank"><img src="https://files.seeedstudio.com/wiki/RS485_500cm%20ultrasonic_sensor/image%202.png" border="0" /></a></p>

在本教程中，我们将介绍如何通过 Node-RED 将 S210X 系列传感器连接到 Microsoft Azure IoT Central。

## SenseCAP & Node-RED

本章是系列教程的第一部分，将指导您安装和使用 Node-RED，并调用 SenseCAP API 以连接到 Node-RED。

本章旨在帮助用户更轻松地将 SenseCAP 平台上的数据连接到各种其他 PaaS 平台，以便进行更深入的数据处理。

**Node-RED**

Node-RED 是一种用于以新颖有趣的方式将硬件设备、API 和在线服务连接在一起的编程工具。它提供了一个基于浏览器的编辑器，使用户可以轻松地使用调色板中的各种节点来连接流程，并通过单击即可部署到其运行时。

<p style={{textAlign: 'center'}}><img src="https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/002.png" alt="pir" width={600} height="auto" /></p>

### 安装 Node.Js

要在本地安装 Node-RED，您需要一个受支持版本的 Node.js。

Node-RED 当前推荐使用 [Node 14.x LTS](https://nodejs.org/en/)。

### 使用 npm 安装 Node-RED

要安装 Node-RED，您可以使用随 Node.js 一起提供的 npm 命令：

```cpp
sudo npm install -g --unsafe-perm node-red
```

:::info 注意
如果您使用的是 Windows，请不要在命令前加 "sudo"。
:::

此命令将以全局模块的形式安装 Node-RED 及其依赖项。
安装为全局模块后，您可以使用以下命令在终端中启动 Node-RED：

```cpp
node-red
```

![IMG\_258](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/003.png)

然后，您可以通过浏览器访问 [http://localhost:1880](http://localhost:1880/) 打开 Node-RED 编辑器。

### 获取 SenseCAP API

在继续本节之前，请确保您已在 SenseCAP 控制台中绑定了您的 S210X 设备。

登录 [**SenseCAP 控制台**](https://sensecap.seeed.cc/portal/#/dashboard)。在仪表板顶部用户名右侧的下拉栏中，我们可以找到 **组织信息**，请选择它以获取 **组织 ID**。

![IMG\_259](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/004.png)

接下来，我们还需要获取 SenseCAP 的 API 密钥。请点击仪表板左侧的 **安全 -> 访问 API 密钥**。然后创建一个访问密钥。

![IMG\_260](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/005.png)

点击您创建的 **API ID**，您将获得 **访问 API 密钥**，请复制它和 **组织 ID**，我们将在后续步骤中使用它们。

![IMG\_261](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/006.png)

### Node-RED 配置

![IMG\_262](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/007.png)

* **步骤 1.** 添加一个新的 mqtt-broker 节点

拖出一个 **mqtt in** 节点，双击它进入配置页面，然后点击 **添加新的 mqtt-broker** 后的编辑按钮。

![IMG\_263](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/008.png)

mqtt-broker 的配置需要填写如下内容：

服务器：openstream.api.sensecap.seeed.cc

端口：1883

协议：MQTT V3.1.1

客户端 ID 格式：**org-"组织 ID" "随机 ID"**

**组织 ID：** 从您的 **组织信息** 中获取

**随机 ID：** 使用您自己随机生成的数字和小写字母。

示例：org-43243\*\*\*23-test

![IMG\_264](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/009.png)

然后我们在 **安全** 选项字段中填写用户名和密码：

用户名：**org-"组织 ID"**

**组织 ID：** 您的组织 ID。我们之前已经获取过。

密码：填写我们之前获取的 **访问 API 密钥**。

![IMG\_265](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/010.png)

添加 **主题**

主题：配置特定格式的主题以确定要接收的设备和数据类型。

主题格式：
**/device_sensor_data/"OrgID"/"DeviceEUI"/"Channel"/"Reserved"/"MeasurementID"**

| OrgID | 您可以在组织信息中找到 ID |
| :-: | :- |
| DeviceEUI | 您可以在设备基本属性或设备标签上找到 EUI |
| Channel | 设备上连接到传感器的物理接口，默认值：1 |
| Reserved | 保留字段 |
| MeasurementID | [measurement_list](https://sensecap-docs.seeed.cc/measurement_list.html) |

:::info 注意
"+" 表示此字段没有过滤条件，可以匹配所有内容。"/+/+/+/+" 表示监听所有 "DeviceEUI"、"Channel"、"Reserved"、"MeasurementID"。
:::

示例：/device\_sensor\_data/424988\*\*\*\*44/2CF7F\*\*\*0002/+/+/+

此主题表示接收当前设备的所有遥感数据。

![IMG\_266](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/011.png)

* **步骤 2.** 添加调试节点

拖出一个 **debug** 节点，连接到 **mqtt-in** 节点，然后点击 **Deploy**。

部署成功后，您会在 **mqtt in** 构建块下看到 "**Connected**"，数据报告间隔由我们连接的传感器决定。接收到数据后，右侧的调试窗口将显示原始数据。![IMG\_267](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/012.png)

## **SenseCAP & Node-RED & Azure IoT Central**

[**Microsoft Azure IoT Central**](https://azure.microsoft.com/en-us/services/iot-central) 是一个完全托管的全球 IoT SaaS（软件即服务）解决方案，使您能够轻松地大规模连接、监控和管理您的 IoT 资产。它具有高度的安全性，随着业务增长而扩展，确保您的投资可重复，并与现有业务应用程序集成。它还弥合了业务应用程序与 IoT 数据之间的差距。最后，它提供集中管理以重新配置和更新您的设备。

本章内容将继续使用前面介绍的 Node-RED，并通过 Node-RED 在 Microsoft Azure IoT Central 中管理 S210X 传感器套件。

### Microsoft Azure IoT Central 配置

* **步骤 1.** 登录 Azure IoT Central。

请访问 [**Azure IoT Central**](https://apps.azureiotcentral.com/home) 网站，从左侧导航菜单中点击 **Build**，然后点击 **Custom apps**。![IMG\_268](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/013.png)

* **步骤 2.** 填写 **Application name** 并选择 **Pricing plan**。当您填写应用名称时，应用 URL 将自动生成。

![IMG\_269](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/014.png)

注意：如果您是 Azure IoT Central 的新用户，我们建议选择 Free，因为这不会产生费用。

![IMG\_270](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/015.png)

* **步骤 3.** 点击 **Create** 创建新应用。现在您已成功设置 Azure IoT Central！

![IMG\_271](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/016.png)

* **步骤 4.** 创建设备模板

请通过点击左侧菜单栏中的 **Device templates** 创建一个新的设备模板。

![IMG\_272](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/017.png)

为您的设备模板命名并点击 **create**。

![IMG\_273](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/018.png)

![IMG\_274](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/019.png)

* **步骤 5.** 创建设备

点击左侧菜单栏中的 **Devices -> S2103**。![IMG\_275](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/020.png)

![IMG\_276](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/021.png)

创建设备后，您将在 **Device** 下看到刚刚创建的设备，请进入设备并点击左上角的 **Connect** 按钮。

请记录此信息，我们将在后续步骤中使用。

![IMG\_277](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/022.png)

### **Node-RED 配置**

* **步骤 1.** 安装 Azure IoT 插件

点击右上角菜单栏并选择 Settings。![IMG\_278](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/023.png)

在 **Paletts - Install** 中搜索并安装 "node-red-contrib-azure-iot-central"。![IMG\_279](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/024.png)

* **步骤 2.** 配置 Azure IoT Central 节点

从左侧功能栏拖出 **Azure IoT Central** 节点，双击进入配置页面，然后点击编辑按钮以编辑 **Azure IoT Central** 节点。

![IMG\_280](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/025.png)

需要填写以下配置：

Transport: MQTT

Authentication: SAS

Scope ID/Device ID/Primary Key: 我们之前已获取

![IMG\_281](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/026.png)

* **步骤 3.** 配置功能节点

向 Azure IoT Central 报告数据需要遵循特定的数据格式，因此需要添加一个功能构建块来处理数据格式。

从左侧功能栏拖出 **function** 节点，双击进入编辑页面，然后将代码复制到 **On Message**。

![IMG\_282](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/027.png)

**代码**：

```cpp
{
    var payload = msg.payload;
    var topic = msg.topic;
    var strs = topic.split("/");
    var length = strs.length
    if (length >= 2) {
        var measurementId = strs[length - 1]
        var body = {}
        var value = payload.value
        if (measurementId == 4097) {
            body.AirTemperature = value
        } else if (measurementId == 4098) {
            body.AirHumidity = value
        } else if (measurementId == 4100) {
            body.CO2 = value
        }
        msg.payload = body;
    }
    return msg;
}
```

如果您想查看数据的日志信息，可以在功能节点后添加一个调试节点。

![IMG\_283](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/028.png)

一旦 S210X 传感器开始启动并工作，并开始向 SenseCAP PaaS 服务器发送数据，我们就可以在 Azure IoT Central 上查看数据。

### **数据展示**
在 **Raw data** 列中可见的数据被放置在 **Unmodeled data** 中，因此我们需要根据上述代码解析数据。

添加所需的功能，然后点击 **保存** 和 **发布**

![IMG\_284](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/029.png)

![IMG\_285](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/030.png)

然后我们可以清楚地查看传感器上传的原始数据。 ![IMG\_286](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/031.png)

如果您希望丰富数据仪表板页面，还可以将其配置为在概览中显示。

点击左侧导航菜单中的 **概览**。

![IMG\_287](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/032.png)

展开 **以设备开始** 下拉菜单，并选择您想要可视化的遥测数据。

![IMG\_288](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/033.png)

点击 **添加磁贴**，您将看到磁贴已添加到 Azure IoT Central 仪表板。

![IMG\_289](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/034.png)

接下来，根据您的喜好定制传感器数据监控仪表板！

完成更改后，只需点击 **保存** 和 **发布**

![IMG\_290](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/035.png)

![IMG\_291](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/036.png)

现在您可以通过自定义仪表板查看传感器数据了！ ![IMG\_292](https://files.seeedstudio.com/wiki/SenseCAPS210X/Azure_IoT_Central/037.png)