---
title: 创建基于事件的工作流或操作 - Azure 逻辑应用 | Microsoft Docs
description: 使用 Webhook 和 Azure 逻辑应用自动执行基于事件的工作流或操作
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.topic: article
tags: connectors
ms.date: 07/21/2016
ms.openlocfilehash: 7b1886321ca4afd4b4710bd9fddf16d2d5eb224b
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126581"
---
# <a name="create-event-based-workflows-or-actions-by-using-webhooks-and-azure-logic-apps"></a>使用 Webhook 和 Azure 逻辑应用创建基于事件的工作流或操作

借助 Webhook 操作和触发器，可启动、暂停和恢复流以执行以下任务：

* 收到项目时从 [Azure 事件中心](https://github.com/logicappsio/EventHubAPI)触发
* 在继续工作流之前等待批准

深入了解[如何创建支持 webhook 的自定义 API](../logic-apps/logic-apps-create-api-app.md)。

## <a name="use-the-webhook-trigger"></a>使用 Webhook 触发器

[触发器](connectors-overview.md)是一个会启动逻辑应用工作流的事件。 Webhook 触发器是基于事件的，不依赖针对新项的轮询。 同[请求触发器](connectors-native-reqres.md)一样，会在事件发生时触发逻辑应用。 Webhook 触发器会向一个服务注册回调 URL，并根据需要使用该 URL 触发逻辑应用。

以下示例演示如何在逻辑应用设计器中设置 HTTP 触发器。 这些步骤假设之前已部署或正在访问遵循[逻辑应用中的 webhook 订阅和取消订阅模式](../logic-apps/logic-apps-create-api-app.md#webhook-triggers)的 API。 每当使用新 Webhook 保存逻辑应用或将逻辑应用从禁用切换到启用时，都进行订阅调用。 删除和保存逻辑应用 webhook 触发器或将其从启用切换为禁用时，进行取消订阅调用。

**添加 Webhook 触发器**

1. 作为逻辑应用中的第一步，添加 **HTTP Webhook** 触发器。
2. 为 webhook 订阅和取消订阅调用填写参数。

   此步骤遵循与 [HTTP 操作](connectors-native-http.md)格式相同的模式。

     ![HTTP 触发器](./media/connectors-native-webhook/using-trigger.png)

3. 至少添加一个操作。
4. 单击“保存”以发布逻辑应用。 此步骤将使用触发此逻辑应用所需的回调 URL 调用订阅终结点。
5. 每当服务向回调 URL 发送 `HTTP POST` 请求时，逻辑应用都会触发，并包含传递到请求中的任何数据。

## <a name="use-the-webhook-action"></a>使用 Webhook 操作

[操作](connectors-overview.md)是指在逻辑应用中定义的工作流所执行的操作。 webhook 操作会将*回调 URL* 注册到服务，并在恢复前等待该 URL 被调用。 [“发送批准电子邮件”](connectors-create-api-office365-outlook.md)是遵循此模式的连接器示例。 可通过 Webhook 操作将此模式扩展到任何服务中。 

以下示例演示如何在逻辑应用设计器中设置 webhook 操作。 这些步骤假设之前已部署或正在访问遵循[逻辑应用中使用的 webhook 订阅和取消订阅模式](../logic-apps/logic-apps-create-api-app.md#webhook-actions)的 API。 逻辑应用执行 webhook 操作时，进行订阅调用。 在等待响应时或在逻辑应用超时之前取消运行时，进行取消订阅调用。

**添加 webhook 操作**

1. 选择“新步骤” > “添加操作”。

2. 在搜索框中，键入“webhook”，查找 **HTTP Webhook** 操作。

    ![选择查询操作](./media/connectors-native-webhook/using-action-1.png)

3. 为 Webhook 订阅和取消订阅调用填写参数

   此步骤遵循与 [HTTP 操作](connectors-native-http.md)格式相同的模式。

     ![完成查询操作](./media/connectors-native-webhook/using-action-2.png)
   
   在运行时，逻辑应用在进行到该步骤后调用订阅终结点。

4. 单击“保存”以发布逻辑应用。

## <a name="technical-details"></a>技术详细信息

以下是有关 webhook 支持的触发器和操作的详细信息。

## <a name="webhook-triggers"></a>Webhook 触发器

| 操作 | Description |
| --- | --- |
| HTTP Webhook |将回调 URL 订阅到可调用该 URL 的服务以按需触发逻辑应用。 |

### <a name="trigger-details"></a>触发器详细信息

#### <a name="http-webhook"></a>HTTP Webhook

将回调 URL 订阅到可调用该 URL 的服务以按需触发逻辑应用。
\* 表示必填字段。

| 显示名称 | 属性名称 | Description |
| --- | --- | --- |
| 订阅方法* |方法 |要用于订阅请求的 HTTP 方法 |
| 订阅 URI* |uri |要用于订阅请求的 HTTP URI |
| 取消订阅方法* |方法 |要用于取消订阅请求的 HTTP 方法 |
| 取消订阅 URI* |uri |要用于取消订阅请求的 HTTP URI |
| 订阅正文 |body |订阅的 HTTP 请求正文 |
| 订阅标头 |headers |订阅的 HTTP 请求标头 |
| 订阅身份验证 |authentication |要用于订阅的 HTTP 身份验证。 有关详细信息，[请参阅 HTTP 连接器](connectors-native-http.md#authentication) |
| 取消订阅正文 |body |取消订阅的 HTTP 请求正文 |
| 取消订阅标头 |headers |取消订阅的 HTTP 请求标头 |
| 取消订阅身份验证 |authentication |要用于取消订阅的 HTTP 身份验证。 有关详细信息，[请参阅 HTTP 连接器](connectors-native-http.md#authentication) |

**输出详细信息**

Webhook 请求

| 属性名称 | 数据类型 | Description |
| --- | --- | --- |
| 标头 |对象 |Webhook 请求标头 |
| Body |对象 |Webhook 请求对象 |
| 状态代码 |int |Webhook 请求状态代码 |

## <a name="webhook-actions"></a>Webhook 操作

| 操作 | Description |
| --- | --- |
| HTTP Webhook |将回调 URL 订阅到可调用该 URL 的服务以按需恢复工作流步骤。 |

### <a name="action-details"></a>操作详细信息

#### <a name="http-webhook"></a>HTTP Webhook

将回调 URL 订阅到可调用该 URL 的服务以按需恢复工作流步骤。
\* 表示必填字段。

| 显示名称 | 属性名称 | Description |
| --- | --- | --- |
| 订阅方法* |方法 |要用于订阅请求的 HTTP 方法 |
| 订阅 URI* |uri |要用于订阅请求的 HTTP URI |
| 取消订阅方法* |方法 |要用于取消订阅请求的 HTTP 方法 |
| 取消订阅 URI* |uri |要用于取消订阅请求的 HTTP URI |
| 订阅正文 |body |订阅的 HTTP 请求正文 |
| 订阅标头 |headers |订阅的 HTTP 请求标头 |
| 订阅身份验证 |authentication |要用于订阅的 HTTP 身份验证。 有关详细信息，[请参阅 HTTP 连接器](connectors-native-http.md#authentication) |
| 取消订阅正文 |body |取消订阅的 HTTP 请求正文 |
| 取消订阅标头 |headers |取消订阅的 HTTP 请求标头 |
| 取消订阅身份验证 |authentication |要用于取消订阅的 HTTP 身份验证。 有关详细信息，[请参阅 HTTP 连接器](connectors-native-http.md#authentication) |

**输出详细信息**

Webhook 请求

| 属性名称 | 数据类型 | Description |
| --- | --- | --- |
| 标头 |对象 |Webhook 请求标头 |
| Body |对象 |Webhook 请求对象 |
| 状态代码 |int |Webhook 请求状态代码 |

## <a name="next-steps"></a>后续步骤

* [创建逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [查找其他连接器](apis-list.md)