---
title: include 文件
description: include 文件
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 05/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a8be74558fb71ceb1c28c27004b27a5a5f477a8f
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "49650633"
---
Visual Studio 中的 Azure Functions 项目模板创建一个项目，该项目可发布到 Azure 中的函数应用。 函数应用可将函数分组为一个逻辑单元，以用于管理、部署和共享资源。

1. 在 Visual Studio 中，从“文件”菜单中选择“新建” > “项目”。

2. 在“新建项目”对话框中，选择“已安装”，展开“Visual C#” > “云”，选择“Azure Functions”，键入项目的“名称”，然后单击“确定”。 函数应用名称必须可以充当 C# 命名空间，因此请勿使用下划线、连字符或任何其他的非字母数字字符。

    ![“新建项目”对话框，用于在 Visual Studio 中创建函数](./media/functions-vstools-create/functions-vs-new-project.png)

3. 使用图像后的表中指定的设置。

    ![Visual Studio 中的“新建函数”对话框](./media/functions-vstools-create/functions-vs-new-function.png) 

    | 设置      | 建议的值  | Description                      |
    | ------------ |  ------- |----------------------------------------- |
    | **版本** | Azure Functions 2.x <br />(.NET Core) | 这会创建使用 Azure Functions 的版本 2.x 运行时（支持 .NET Core）的函数项目。 Azure Functions 1.x 支持 .NET Framework。 有关详细信息，请参阅[如何指向 Azure Functions 运行时版本](../articles/azure-functions/functions-versions.md)。   |
    | **模板** | HTTP 触发器 | 这会创建由 HTTP 请求触发的函数。 |
    | **存储帐户**  | 存储仿真器 | HTTP 触发器不使用存储帐户连接。 所有其他触发器类型需要有效的存储帐户连接字符串。 |
    | **访问权限** | 匿名 | 在未提供密钥的情况下，任何客户端都可以触发创建的函数。 通过此授权设置可以轻松测试新函数。 有关密钥和授权的详细信息，请参阅 [HTTP 和 Webhook 绑定](../articles/azure-functions/functions-bindings-http-webhook.md)中的[授权密钥](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys)。 |
4. 单击“确定”以创建函数项目和 HTTP 触发的函数。
