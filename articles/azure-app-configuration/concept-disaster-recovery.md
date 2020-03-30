---
title: Azure 应用配置恢复和灾难恢复
description: 使用 Azure 应用配置实现恢复性和灾难恢复。
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 96ef09ac081aa328014217592a7fcd3ed6314c0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "77523758"
---
# <a name="resiliency-and-disaster-recovery"></a>复原能力和灾难恢复

目前，Azure 应用配置是一个区域性的服务。 每个配置存储在特定的 Azure 区域中创建。 区域范围的服务中断会影响该区域中的所有存储。 应用配置不提供自动故障转移到另一个区域的机制。 本文提供有关如何使用跨 Azure 区域的多个配置存储来提高应用程序异地复原能力的一般性指导。

## <a name="high-availability-architecture"></a>高可用性体系结构

若要实现跨区域冗余，需要在不同的区域中创建多个应用程序配置存储。 完成此设置后，应用程序至少会获得一个附加的配置存储，当主要存储不可访问时，应用程序可以回退到该附加存储。 下图演示了应用程序与其主要配置存储和辅助配置存储之间的拓扑：

![异地冗余的存储](./media/geo-redundant-app-configuration-stores.png)

应用程序会以并行方式从主要存储和辅助存储加载其配置。 这样做可以提高成功获取配置数据的可能性。 您有责任使两个存储中的数据保持同步。以下各节说明如何将地理恢复性构建到应用程序中。

## <a name="failover-between-configuration-stores"></a>配置存储之间的故障转移

从技术上讲，应用程序不会执行故障转移。 它将会尝试同时从两个应用配置存储检索相同的配置数据集。 按以下方式安排代码：让代码先从辅助存储加载数据，再从主要存储加载数据。 此方法可确保只要主要存储中的配置数据可用就会优先进行加载。 以下代码段显示了如何在 .NET Core 中实现这种安排：

#### <a name="net-core-2x"></a>[.NET Core 2.x](#tab/core2x)

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                  .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
        })
        .UseStartup<Startup>();
    
```

#### <a name="net-core-3x"></a>[.NET Core 3.x](#tab/core3x)

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
            webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionString_SecondaryStore"], optional: true)
                    .AddAzureAppConfiguration(settings["ConnectionString_PrimaryStore"], optional: true);
            })
            .UseStartup<Startup>());
```
---

请注意传入 `AddAzureAppConfiguration` 函数中的 `optional` 参数。 将此参数设置为 `true` 时，如果该函数无法加载配置数据，则此参数可防止应用程序无法继续。

## <a name="synchronization-between-configuration-stores"></a>配置存储之间的同步

所有异地冗余的配置存储必须包含相同的数据集，这一点非常重要。 可以按需使用应用配置中的“导出”功能将数据从主要存储复制到辅助存储****。 可通过 Azure 门户和 CLI 使用此功能。

在 Azure 门户中，可以按照以下步骤将更改推送到另一个配置存储。

1. 转到“导入/导出”选项卡****，然后依次选择“导出”**** > “应用配置”**** > “目标”**** > “选择资源”****。

1. 在打开的新边栏选项卡中，指定辅助存储的订阅、资源组和资源名称，然后选择 **"应用**"。

1. UI 将会更新，使你可以选择要导出到辅助存储的配置数据。 您可以将默认时间值保留为"**从"标签**和 **"到"标签**设置为相同的值。 选择“应用”。

1. 针对所有配置更改重复执行上述步骤。

若要自动执行此导出过程，请使用 Azure CLI。 以下命令演示如何将单个配置更改从主要存储导出到辅助存储：

```azurecli
    az appconfig kv export --destination appconfig --name {PrimaryStore} --label {Label} --dest-name {SecondaryStore} --dest-label {Label}
```

## <a name="next-steps"></a>后续步骤

本文已介绍如何增强应用程序，以便在应用程序配置运行时实现异地复原。 也可以在生成或部署时嵌入来自应用配置的配置数据。 有关详细信息，请参阅[与 CI/CD 管道集成](./integrate-ci-cd-pipeline.md)。
