---
title: 使用适用于 Cosmos DB 的 Azure Functions 触发器配置和读取日志
description: 了解如何在使用适用于 Cosmos DB 的 Azure Functions 触发器时向 Azure Functions 日志记录管道公开日志
author: ealsur
ms.service: cosmos-db
ms.topic: how-to
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: 69a8f44831f1af13158261bedb19a254c6a565a6
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2020
ms.locfileid: "92165292"
---
# <a name="how-to-configure-and-read-the-logs-when-using-azure-functions-trigger-for-cosmos-db"></a>如何在使用适用于 Cosmos DB 的 Azure Functions 触发器时配置和读取日志

本文介绍如何配置 Azure Functions 环境，以将 Cosmos DB 日志的 Azure Functions 触发器发送到配置的 [监视解决方案](../azure-functions/functions-monitoring.md)。

## <a name="included-logs"></a>包含的日志

适用于 Cosmos DB 的 Azure Functions 触发器在内部使用[更改源处理器库](./change-feed-processor.md)，该库会生成一组运行状况日志，而这些日志又可用于监视内部操作以[进行故障排除](./troubleshoot-changefeed-functions.md)。

运行状况日志描述了在负载均衡方案或初始化期间尝试执行操作时，适用于 Cosmos DB 的 Azure Functions 触发器的行为。

## <a name="enabling-logging"></a>启用日志记录

若要在使用适用于 Cosmos DB 的 Azure Functions 触发器时启用日志记录，请在 Azure Functions 项目或 Azure Functions 应用中找到 `host.json` 文件，并[配置所需的日志记录级别](../azure-functions/configure-monitoring.md#configure-log-levels)。 必须按以下示例中所示为 `Host.Triggers.CosmosDB` 启用跟踪：

```js
{
  "version": "2.0",
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "Host.Triggers.CosmosDB": "Trace"
    }
  }
}
```

使用更新的配置部署 Azure 函数后，将在跟踪中看到适用于 Cosmos DB 的 Azure Functions 触发器日志。 可以在类别  *下查看所配置的日志记录提供程序中的日志。* `Host.Triggers.CosmosDB`

## <a name="query-the-logs"></a>查询日志

在 [Azure Application Insights Analytics](../azure-monitor/app/analytics.md) 中运行以下查询，以查询适用于 Cosmos DB 的 Azure Functions 触发器生成的日志：

```sql
traces
| where customDimensions.Category == "Host.Triggers.CosmosDB"
```

## <a name="next-steps"></a>后续步骤

* 在 Azure Functions 应用程序中[启用监视](../azure-functions/functions-monitoring.md)。
* 了解使用适用于 Cosmos DB 的 Azure Functions 触发器时如何[诊断和排查常见问题](./troubleshoot-changefeed-functions.md)。
