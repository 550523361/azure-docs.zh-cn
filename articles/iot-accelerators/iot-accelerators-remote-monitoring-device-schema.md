---
title: 远程监视解决方案中的设备架构 - Azure | Microsoft Docs
description: 本文介绍用于在远程监视解决方案中定义模拟设备的 JSON 架构。
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 12/18/2018
ms.topic: conceptual
ms.openlocfilehash: 54e21e0b5f522b226ec1abb71a1c5ab5b245df05
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2018
ms.locfileid: "53635521"
---
# <a name="understand-the-device-model-schema"></a>了解设备模型架构

可以使用远程监视解决方案中的模拟设备来测试该解决方案的行为。 远程监视解决方案包括用于运行模拟设备的设备模拟服务。 部署远程监视解决方案时，会自动预配一系列模拟设备。 我们可以自定义现有的模拟设备，或创建自己的模拟设备。

本文介绍指定模拟设备的功能和行为的设备模型架构。 设备模型存储在 JSON 文件中。

> [!NOTE]
> 此设备模型架构仅适用于在设备模拟服务中托管的模拟设备。 如果想要创建物理设备，请参阅[将设备连接到远程监视解决方案加速器](iot-accelerators-connecting-devices.md)。

以下文章与本文相关：

* [实现设备模型行为](iot-accelerators-remote-monitoring-device-behavior.md)介绍了可用于实现模拟设备行为的 JavaScript 文件。
* [创建新模拟设备](iot-accelerators-remote-monitoring-create-simulated-device.md)将所有相关内容统合在一起，介绍如何在解决方案中部署新的模拟设备类型。

在本文中，学习如何：

>[!div class="checklist"]
> * 使用 JSON 文件定义模拟设备模型
> * 指定模拟设备的属性
> * 指定模拟设备发送的遥测数据
> * 指定设备响应的云到设备方法

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>后续步骤

本文介绍了如何创建自己的自定义模拟设备模型。 本文的内容包括：

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * 使用 JSON 文件定义模拟设备模型
> * 指定模拟设备的属性
> * 指定模拟设备发送的遥测数据
> * 指定设备响应的云到设备方法

了解 JSON 架构后，建议接下来继续学习如何[实现模拟设备的行为](iot-accelerators-remote-monitoring-device-behavior.md)。

有关可供开发人员参考的远程监视解决方案详细信息，请参阅：

* [开发人员参考指南](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [开发人员故障排除指南](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)
