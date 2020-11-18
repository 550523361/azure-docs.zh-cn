---
title: 教程 - 将 IoT 即插即用示例 Node.js 组件设备代码连接到 Azure IoT 中心 | Microsoft Docs
description: 教程 - 生成和运行使用了多个组件且连接到 IoT 中心的 IoT 即插即用示例 Node.js 设备代码。 使用 Azure IoT 资源管理器工具查看由设备发送到中心的信息。
author: olivakar
ms.author: olkar
ms.date: 07/10/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.custom: devx-track-js
ms.openlocfilehash: 70fa10f5319de072ee8ea7dad4e73c58d4bdeede
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93421409"
---
# <a name="tutorial-connect-a-sample-iot-plug-and-play-multiple-component-device-application-to-iot-hub-nodejs"></a>教程：将示例 IoT 即插即用多组件设备应用程序连接到 IoT 中心 (Node.js)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

本教程介绍如何使用组件生成示例 IoT 即插即用设备应用程序，将其连接到 IoT 中心，并使用 Azure IoT 资源管理器工具来查看它发送到中心的信息。 该示例应用程序针对 Node.js 编写，包含在用于 Node.js 的 Azure IoT 中心设备 SDK 中。 解决方案构建者可以使用 Azure IoT 资源管理器工具来了解 IoT 即插即用设备的功能，而无需查看任何设备代码。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

若要完成本教程，需要在开发计算机上使用 Node.js。 可以从 [nodejs.org](https://nodejs.org) 下载适用于多个平台的最新推荐版本。

可以使用以下命令验证开发计算机上 Node.js 当前的版本：

```cmd/sh
node --version
```

## <a name="download-the-code"></a>下载代码

如果已完成[快速入门：将 Windows 上运行的示例 IoT 即插即用设备应用程序连接到 IoT 中心 (Node)](quickstart-connect-device-node.md)，则已克隆了存储库。

在所选目录中打开命令提示符。 执行以下命令，将[用于 Node.js 的 Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdk-node) GitHub 存储库克隆到此位置：

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-node
```

## <a name="install-required-libraries"></a>安装所需的库

使用设备 SDK 生成包含的示例代码。 你构建的应用程序会模拟一个即插即用设备，其中包含多个连接到 IoT 中心的组件。 应用程序将发送遥测数据和属性，并接收命令。

1. 在本地终端窗口中，前往克隆存储库的文件夹，导航到 /azure-iot-sdk-node/device/samples/pnp 文件夹。 然后运行以下命令以安装所需的库：

```cmd/sh
npm install
```

这会在文件夹中安装运行示例所需的相关 npm 文件。

## <a name="review-the-code"></a>查看代码

导航到 azure-iot-sdk-node\device\samples\pnp 文件夹。

azure-iot-sdk-node\device\samples\pnp 文件夹包含 IoT 即插即用温度控制器设备的示例代码。

pnpTemperatureController.js 文件中的代码实现 IoT 即插即用温度控制器设备。 此示例实现的模型使用[多个组件](concepts-components.md)。 [温度设备的数字孪生定义语言 (DTDL) 模型文件](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json)定义了设备实现的遥测、属性和命令。

在所选的代码编辑器中打开 pnpTemperatureController.js 文件。 示例代码演示如何执行以下操作：

- 为要实现的设备定义 DTMI `modelId`。 此 DTMI 是用户定义的，必须与[温度控制器 DTDL 模型](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json)的 DTMI 相匹配。

- 实现温度控制器 DTDL 模型中定义的组件。 实际温度控制器中的组件应实现这两个接口。 这两个接口已在中央存储库中发布。 在此示例中，这两个接口为：

  - 恒温调节器
  - Azure 开发的设备信息

- 定义组件名称。 此示例包含两个恒温器和一个设备信息组件。

- 定义命令名称。 这些是设备响应的命令。

- 定义 `serialNumber` 常量。 `serialNumber` 是任何指定的固定设备。

- 定义命令处理程序。

- 定义用于发送命令响应的函数。

- 定义用于记录命令请求的 helper 函数。

- 定义用于创建属性的 helper 函数。

- 为属性更新定义侦听器。

- 定义一个函数，用于从此设备发送遥测数据。 恒温器和默认组件都发送遥测数据。 此函数接收组件名称作为参数。

- 定义 `main` 函数：

  - 使用设备 SDK 创建设备客户端并连接到 IoT 中心。 设备提供 `modelId` 以便 IoT 中心可以将设备标识为 IoT 即插即用设备。

  - 使用 `onDeviceMethod` 函数开始侦听命令请求。 函数为来自服务的命令请求设置侦听器：

    - 设备 DTDL 定义 `reboot` 和 `getMaxMinReport` 命令。
    - `commandHandler` 函数定义设备对命令的响应方式。

  - 使用 `setInterval` 和 `sendTelemetry` 开始发送遥测数据。

  - 使用 `helperCreateReportedPropertiesPatch` 函数创建属性，并使用 `updateComponentReportedProperties` 来更新属性。

  - 使用 `desiredPropertyPatchListener` 侦听属性更新。

  - 禁用所有侦听器和任务，并在按 Q 或 q 时退出循环。

[!INCLUDE [iot-pnp-environment](../../includes/iot-pnp-environment.md)]

若要详细了解示例配置，请参阅[示例自述文件](https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/pnp/readme.md)。

看到代码后，请使用以下命令运行该示例：

```cmd\sh
node pnpTemperatureController.js
```

将显示以下输出，这表明设备已开始向中心发送遥测数据，现在可以接收命令和属性更新。

![设备确认消息](media/tutorial-multiple-components-node/multiple-component.png)

在执行后续步骤时，保持示例处于运行状态。

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>使用 Azure IoT 资源管理器验证代码

设备客户端示例启动后，使用 Azure IoT 资源管理器工具验证它是否正常工作。

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>后续步骤

本教程已介绍如何将 IoT 即插即用设备和组件连接到 IoT 中心。 若要详细了解 IoT 即插即用设备模型，请参阅：

> [!div class="nextstepaction"]
> [IoT 即插即用建模开发人员指南](concepts-developer-guide-device-csharp.md)
