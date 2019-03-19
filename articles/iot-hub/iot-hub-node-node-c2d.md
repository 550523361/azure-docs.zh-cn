---
title: 使用 Azure IoT 中心发送云到设备消息 (Node) | Microsoft Docs
description: 如何使用 Azure IoT SDK for Node.js 将云到设备的消息从 Azure IoT 中心发送到设备。 修改模拟设备应用以接收云到设备消息，并修改后端应用以发送云到设备消息。
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: javascript
ms.topic: conceptual
ms.date: 06/16/2017
ms.openlocfilehash: 643fd4558c8edd65047eb9d9b780424076d9ea3b
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2019
ms.locfileid: "57530971"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a>使用 IoT 中心发送云到设备的消息 (Node)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>简介
Azure IoT 中心是一项完全托管的服务，有助于在数百万台设备和单个解决方案后端之间实现安全可靠的双向通信。 [IoT 中心入门]教程演示了如何创建 IoT 中心、在其中预配设备标识，以及编写用来发送“设备到云”消息的模拟设备应用。

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

本教程是在 [IoT 中心入门]的基础上编写的。 其中了说明了如何：

* 通过 IoT 中心，将云到设备的消息从解决方案后端发送到单个设备。
* 在设备上接收云到设备的消息。
* 通过解决方案后端，请求确认收到从 IoT 中心发送到设备的消息（反馈）。

可以在 [IoT 中心开发人员指南][IoT Hub developer guide - C2D]中找到有关云到设备消息的详细信息。

在本教程结束时，会运行两个 Node.js 控制台应用：

* **SimulatedDevice**，这是在 [IoT 中心入门]中创建的应用的修改版本，可连接到 IoT 中心并接收云到设备的消息。
* **SendCloudToDeviceMessage**，它将云到设备消息通过 IoT 中心发送到模拟设备应用，然后接收其传送确认。

> [!NOTE]
> IoT 中心通过 Azure IoT 设备 SDK 对许多设备平台和语言（包括 C、Java 和 Javascript）提供 SDK 支持。 有关如何将设备连接到本教程的代码以及通常如何连接到 Azure IoT 中心的分步说明，请参阅 [Azure IoT 开发人员中心]。
> 
> 

要完成本教程，需要以下各项：

* Node.js 版本 4.0.x 或更高版本。
* 有效的 Azure 帐户。 （如果没有帐户，只需花费几分钟就能创建一个[免费帐户][lnk-free-trial]。）

## <a name="receive-messages-in-the-simulated-device-app"></a>在模拟设备应用中接收消息
在本部分中，会修改在 [IoT 中心入门]中创建的模拟设备应用，以接收来自 IoT 中心的“云到设备”消息。

1. 使用文本编辑器打开 SimulatedDevice.js 文件。
2. 修改 **connectCallback** 函数以处理从 IoT 中心发送的消息。 在本示例中，设备始终调用 **complete** 函数，以通知 IoT 中心它已处理消息。 **connectCallback** 函数的新版本类似于以下片段：
   
    ```javascript
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var temperature = 20 + (Math.random() * 15);
            var humidity = 60 + (Math.random() * 20);            
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
            var message = new Message(data);
            message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
   
   > [!NOTE]
   > 如果使用 HTTPS（而不使用 MQTT 或 AMQP）作为传输，则 DeviceClient 实例将不会频繁（频率低于每 25 分钟一次）检查 IoT 中心发来的消息。 有关 MQTT、AMQP 和 HTTPS 支持之间的差异，以及 IoT 中心限制的详细信息，请参阅 [IoT 中心开发人员指南][IoT Hub developer guide - C2D]。
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a>发送云到设备的消息
在本部分中，创建一个 Node.js 控制台应用程序，它将云到设备的消息发送到模拟设备应用程序。 需要使用 [IoT 中心入门]教程中添加的设备的设备 ID。 还需要中心的 IoT 中心连接字符串（位于 [Azure 门户]）。

1. 创建名为 **sendcloudtodevicemessage**的空文件夹。 在命令提示符处，使用以下命令在 **sendcloudtodevicemessage** 文件夹中创建一个 package.json 文件。 接受所有默认值：
   
    ```shell
    npm init
    ```
2. 在命令提示符处，运行以下命令在 **sendcloudtodevicemessage** 文件夹中安装 **azure iothub** 包：
   
    ```shell
    npm install azure-iothub --save
    ```
3. 通过文本编辑器，在 **sendcloudtodevicemessage**文件夹中创建一个 **SendCloudToDeviceMessage.js** 文件。
4. 在 **SendCloudToDeviceMessage.js** 文件的开头添加以下 `require` 语句：
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. 将以下代码添加到 **SendCloudToDeviceMessage.js** 文件。 将“{IoT 中心连接字符串}”占位符值替换为在 [IoT 中心入门]教程中创建的中心的 IoT 中心连接字符串。 将“{设备 id}”占位符替换为在 [IoT 中心入门]教程中添加的设备的设备 ID：
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. 添加以下函数，以便在控制台中列显操作结果：
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. 添加以下函数，以便在控制台中列显送达反馈消息：
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. 添加以下代码，以便将消息发送到设备并在设备确认收到云到设备的消息时处理反馈消息：
   
    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```
9. 保存并关闭 **SendCloudToDeviceMessage.js** 文件。

## <a name="run-the-applications"></a>运行应用程序
现在，已准备就绪，可以运行应用程序了。

1. 在 **simulateddevice** 文件夹的命令提示符下，运行以下命令将遥测发送到 IoT 中心，并侦听云到设备消息：
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![运行模拟设备应用][img-simulated-device]
2. 在 **sendcloudtodevicemessage** 文件夹中的命令提示符下，运行以下命令发送云到设备的消息并等待确认反馈：
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![运行应用以发送云到设备的命令][img-send-command]
   
   > [!NOTE]
   > 为简单起见，本教程不实现任何重试策略。 在生产代码中，应按文章 [Transient Fault Handling](/azure/architecture/best-practices/transient-faults)（暂时性故障处理）中所述实施重试策略（例如指数性的回退）。
   > 
   > 

## <a name="next-steps"></a>后续步骤
在本教程中，已学习如何发送和接收云到设备的消息。 

若要查看使用 IoT 中心完成端到端解决方案的示例，请参阅 [Azure IoT 远程监视解决方案加速器]。

若要了解有关使用 IoT 中心开发解决方案的详细信息，请参阅 [IoT 中心开发人员指南]。

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[IoT 中心入门]: quickstart-send-telemetry-node.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IoT 中心开发人员指南]: iot-hub-devguide.md
[Azure IoT 开发人员中心]: https://azure.microsoft.com/develop/iot
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[Azure 门户]: https://portal.azure.com
[Azure IoT 远程监视解决方案加速器]: https://azure.microsoft.com/documentation/suites/iot-suite/
