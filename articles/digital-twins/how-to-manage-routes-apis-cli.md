---
title: " (Api 和 CLI) 管理终结点和路由"
titleSuffix: Azure Digital Twins
description: 请参阅如何设置和管理 Azure 数字孪生数据的终结点和事件路由。
author: alexkarcher-msft
ms.author: alkarche
ms.date: 10/12/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 5d0956634289713f691feb1a9182233e6795e319
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92201727"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins-apis-and-cli"></a>在 Azure 数字孪生中管理终结点和路由 (Api 和 CLI) 

[!INCLUDE [digital-twins-route-selector.md](../../includes/digital-twins-route-selector.md)]

在 Azure 数字孪生中，可以将 [事件通知](how-to-interpret-event-data.md) 路由到下游服务或连接的计算资源。 这是通过首先设置可接收事件的 **终结点** 来完成的。 然后，可以创建  [**事件路由**](concepts-route-events.md) ，用于指定由 Azure 数字孪生生成的哪些事件将传递到哪些终结点。

可以通过 [EventRoutes api](how-to-use-apis-sdks.md)、 [.Net (c # ) SDK](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)或 [Azure 数字孪生 CLI](how-to-use-cli.md)来管理终结点和路由。 本文逐步讲解如何通过这些机制创建终结点和路由。

还可以通过 [Azure 门户](https://portal.azure.com)进行管理。 有关使用门户的本文版本，请参阅 [*操作方法：管理终结点和路由 (门户) *](how-to-manage-routes-portal.md)。

## <a name="prerequisites"></a>先决条件

* 你将需要一个 **Azure 帐户** (你可以在 [此处](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 免费设置一个帐户) 
* 你将需要 Azure 订阅中的 **Azure 数字孪生实例** 。 如果尚未安装实例，则可以使用 [*操作方法：设置实例和身份验证*](how-to-set-up-instance-portal.md)中的步骤创建一个实例。 将安装程序中的以下值用于本文后面的内容：
    - 实例名称
    - 资源组
    
## <a name="create-an-endpoint-for-azure-digital-twins"></a>为 Azure 数字孪生创建终结点

这些是可为实例创建的支持的终结点类型：
* [事件网格](../event-grid/overview.md)
* [事件中心](../event-hubs/event-hubs-about.md)
* [服务总线](../service-bus-messaging/service-bus-messaging-overview.md)

有关不同终结点类型的详细信息，请参阅在 [*Azure 消息服务之间选择*](../event-grid/compare-messaging-services.md)。

若要将终结点链接到 Azure 数字孪生，要用于终结点的事件网格主题、事件中心或服务总线必须已存在。 

### <a name="create-an-event-grid-endpoint"></a>创建事件网格端点

下面的示例演示如何使用 Azure CLI 创建事件网格类型终结点。 你可以使用 [Azure Cloud Shell](https://shell.azure.com)，或 [在本地安装 CLI](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest)。

首先，创建一个事件网格主题。 可以使用以下命令，或通过访问事件网格*自定义事件*快速入门中[的 *"创建自定义主题*" 部分](../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic)更详细地查看这些步骤。

```azurecli
az eventgrid topic create -g <your-resource-group-name> --name <your-topic-name> -l <region>
```

> [!TIP]
> 要输出可传递到 Azure CLI 命令中的 Azure 区域名称的列表，请运行以下命令：
> ```azurecli
> az account list-locations -o table
> ```

创建主题后，可以使用以下 [Azure 数字孪生 CLI 命令](how-to-use-cli.md)将其链接到 Azure 数字孪生：

```azurecli
az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
```

现在，可以在使用参数指定的名称下，使用事件网格主题作为 Azure 数字孪生内的终结点 `--endpoint-name` 。 通常使用该名称作为 **事件路由**的目标，将 [在本文稍后](#event-routes-with-apis-and-the-c-sdk) 使用 AZURE 数字孪生服务 API 进行创建。

### <a name="create-an-event-hubs-or-service-bus-endpoint"></a>创建事件中心或服务总线终结点

创建事件中心或服务总线终结点的过程类似于上面所示的事件网格进程。

首先，创建将用作终结点的资源。 下面是需要执行的操作：
* Service Bus： _服务总线命名空间_、 _服务总线主题_、 _授权规则_
* 事件中心： _事件中心命名空间_， _事件中心_， _授权规则_

然后，使用以下命令在 Azure 数字孪生中创建终结点： 

* 添加 Service Bus 主题终结点 (需要预先创建的服务总线资源) 
```azurecli 
az dt endpoint create servicebus --endpoint-name <Service-Bus-endpoint-name> --servicebus-resource-group <Service-Bus-resource-group-name> --servicebus-namespace <Service-Bus-namespace> --servicebus-topic <Service-Bus-topic-name> --servicebus-policy <Service-Bus-topic-policy> -n <your-Azure-Digital-Twins-instance-name>
```

* 添加事件中心终结点 (需要预先创建的事件中心资源) 
```azurecli
az dt endpoint create eventhub --endpoint-name <Event-Hub-endpoint-name> --eventhub-resource-group <Event-Hub-resource-group> --eventhub-namespace <Event-Hub-namespace> --eventhub <Event-Hub-name> --eventhub-policy <Event-Hub-policy> -n <your-Azure-Digital-Twins-instance-name>
```

### <a name="create-an-endpoint-with-dead-lettering"></a>创建具有死信的端点

当终结点无法在某个时间段内传递事件时，或者尝试将事件传递到一定次数后，它可以将未送达的事件发送到存储帐户。 此过程称为“死信处理”。

若要创建启用了死信的终结点，必须使用 [ARM api](https://docs.microsoft.com/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate) 来创建终结点。 

在设置死信位置之前，必须有一个包含容器的存储帐户。 创建终结点时，提供此容器的 URL。 死信作为带有 SAS 令牌的容器 URL 提供。 该令牌只需要 `write` 对存储帐户中目标容器的权限。 完整的格式 URL 将采用以下格式： `https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>`

若要了解有关 SAS 令牌的详细信息，请参阅： [使用共享访问签名授予对 Azure 存储资源的有限访问权限 (SAS) ](https://docs.microsoft.com/azure/storage/common/storage-sas-overview)

若要了解有关死信的详细信息，请参阅 [概念：事件路由](./concepts-route-events.md#dead-letter-events)

#### <a name="configuring-the-endpoint"></a>配置终结点

创建终结点时，请将添加 `deadLetterSecret` 到 `properties` 请求正文中的对象，该对象包含用于存储帐户的容器 URL 和 SAS 令牌。

```json
{
  "properties": {
    "endpointType": "EventGrid",
    "TopicEndpoint": "https://contosoGrid.westus2-1.eventgrid.azure.net/api/events",
    "accessKey1": "xxxxxxxxxxx",
    "accessKey2": "xxxxxxxxxxx",
    "deadLetterSecret":"https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>"
  }
}
```

有关详细信息，请参阅 Azure 数字孪生 REST API 文档： [终结点-DigitalTwinsEndpoint CreateOrUpdate](https://docs.microsoft.com/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate)。

### <a name="message-storage-schema"></a>消息存储架构

死信消息将按以下格式存储在您的存储帐户中：

`{container}/{endpointName}/{year}/{month}/{day}/{hour}/{eventId}.json`

死信消息将匹配要传递到原始终结点的原始事件的架构。

下面是一个用于克隆 [创建通知](./how-to-interpret-event-data.md#digital-twin-life-cycle-notifications)的死信消息示例：

```json
{
  "specversion": "1.0",
  "id": "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.DigitalTwins.Twin.Create",
  "source": "<yourInstance>.api.<yourregion>.da.azuredigitaltwins-test.net",
  "data": {
    "$dtId": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "$etag": "W/\"xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
    "TwinData": "some sample",
    "$metadata": {
      "$model": "dtmi:test:deadlettermodel;1",
      "room": {
        "lastUpdateTime": "2020-10-14T01:11:49.3576659Z"
      }
    }
  },
  "subject": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "time": "2020-10-14T01:11:49.3667224Z",
  "datacontenttype": "application/json",
  "traceparent": "00-889a9094ba22b9419dd9d8b3bfe1a301-f6564945cb20e94a-01"
}
```

## <a name="event-routes-with-apis-and-the-c-sdk"></a>与 Api 和 c # SDK (的事件路由) 

若要将数据从 Azure 数字孪生实际发送到终结点，需要定义 **事件路由**。 通过 Azure 数字孪生 **EventRoutes api** ，开发人员可以将事件流连接到整个系统和下游服务。 有关事件路由的详细信息，请参阅 [*概念：路由 Azure 数字孪生事件*](concepts-route-events.md)。

本节中的示例使用 [.net (c # ) SDK](https://www.nuget.org/packages/Azure.DigitalTwins.Core)。

**必备组件**：你需要先按本文前面所述创建终结点，然后才能继续创建路由。 终结点完成设置后，可以继续创建事件路由。

>[!NOTE]
>如果你最近部署了终结点，请验证它们是否已完成部署， **然后再** 尝试将它们用于新的事件路由。 如果由于终结点尚未准备好而导致路由部署失败，请等待几分钟，然后重试。
>
> 如果要编写此流脚本，可能需要在2-3 分钟的等待时间内生成终结点服务，然后再继续进行路由设置。

### <a name="create-an-event-route"></a>创建事件路由

使用 [数据平面 api](how-to-use-apis-sdks.md#overview-data-plane-apis)定义事件路由。 

路由定义可包含以下元素：
* 要使用的路由名称
* 要使用的终结点的名称
* 定义要发送到终结点的事件的筛选器 

如果没有路由名称，则不会在 Azure 数字孪生之外路由任何消息。 如果存在路由名称并且筛选器为，则 `true` 所有消息都将路由到该终结点。 如果存在路由名称并且添加了不同的筛选器，则将根据筛选器筛选消息。

一个路由应该允许选择多个通知和事件类型。 

`CreateEventRoute` 用于添加事件路由的 SDK 调用。 下面是其用法的示例：

```csharp
EventRoute er = new EventRoute("endpointName");
er.Filter = "true"; //Filter allows all messages
await client.CreateEventRoute("routeName", er);
```

> [!TIP]
> 所有 SDK 函数都提供同步和异步版本。

### <a name="event-route-sample-code"></a>事件路由示例代码

下面的代码示例演示如何创建、列出和删除事件路由：
```csharp
try
{
    Console.WriteLine("Create a route: testRoute1");
    EventRoute er = new EventRoute("< your - endpoint - name >");
    // Make a filter that passes everything
    er.Filter = "true";
    client.CreateEventRoute("< your - route - name >", er);
    Console.WriteLine("Create route succeeded. Now listing routes:");
    Pageable <EventRoute> result = client.GetEventRoutes();
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Route {r.Id} to endpoint {r.EndpointName} with filter {r.Filter} ");
    }
    Console.WriteLine("Deleting routes:");
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Deleting route {r.Id}:");
        client.DeleteEventRoute(r.Id);
    }
}
catch (RequestFailedException e)
{
    Console.WriteLine($"*** Error in event route processing ({e.ErrorCode}):\n${e.Message}");
}
```

### <a name="filter-events"></a>筛选事件

如果没有筛选，终结点就会收到来自 Azure 数字孪生的各种事件：
* [数字孪生](concepts-twins-graph.md)使用 Azure 数字孪生服务 API 触发的遥测数据
* 克隆属性更改通知，对 Azure 数字孪生实例中任何克隆的属性更改触发
* 生命周期事件，在创建或删除孪生或关系时触发
* 添加或删除在 Azure 数字孪生实例中配置的 [模型](concepts-models.md) 时触发的模型更改事件

可以通过将终结点的 **筛选器** 添加到事件路由来限制正在发送的事件。

若要添加筛选器，你可以将 PUT 请求用于 *https：//{YourHost}/EventRoutes/myNewRoute？ api 版本 = 2020-10-31* ，其中包含以下正文：

```json  
{
    "endpointName": "<endpoint-name>",
    "filter": "<filter-text>"
}
``` 

下面是受支持的路由筛选器。 使用 *筛选器文本架构* 列中的详细信息替换 `<filter-text>` 上述请求正文中的占位符。

[!INCLUDE [digital-twins-route-filters](../../includes/digital-twins-route-filters.md)]

## <a name="manage-endpoints-and-routes-with-cli"></a>用 CLI 管理终结点和路由

还可以使用 Azure 数字孪生 CLI 管理终结点和路由。 有关使用 CLI 以及哪些命令可用的详细信息，请参阅 [*操作方法：使用 Azure 数字孪生 CLI*](how-to-use-cli.md)。

[!INCLUDE [digital-twins-known-issue-cloud-shell](../../includes/digital-twins-known-issue-cloud-shell.md)]

[!INCLUDE [digital-twins-route-metrics](../../includes/digital-twins-route-metrics.md)]

## <a name="next-steps"></a>后续步骤

阅读可接收的不同类型的事件消息：
* [*操作说明：解释事件数据*](how-to-interpret-event-data.md)