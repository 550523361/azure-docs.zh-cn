---
title: Azure 诊断扩展概述
description: 使用 Azure 诊断在云服务、虚拟机和 Service Fabric 中进行调试、性能度量、监视和流量分析
ms.subservice: diagnostic-extension
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/14/2020
ms.openlocfilehash: 6cb514312db525ffd2ccf9f7b70968daaa94f322
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672372"
---
# <a name="azure-diagnostics-extension-overview"></a>Azure 诊断扩展概述
Azure 诊断扩展是[Azure 监视器中的代理](agents-overview.md)，用于从 Azure 计算资源（包括虚拟机）的来宾操作系统收集监视数据。 本文概述了 Azure 诊断扩展，包括它支持的特定功能以及安装和配置选项。 

> [!NOTE]
> Azure 诊断扩展是可用于从计算资源的来宾操作系统收集监视数据的代理之一。 有关不同代理的说明以及选择适合需求的代理的指导，请参阅[Azure 监视器代理概述](agents-overview.md)。

## <a name="comparison-to-log-analytics-agent"></a>与日志分析代理的比较
Azure 监视器中的日志分析代理还可用于从虚拟机的来宾操作系统收集监视数据。 根据您的要求，您可以选择使用任一或两者。 有关 Azure 监视器代理的详细比较，请参阅[Azure 监视器代理的概述](agents-overview.md)。 

需要考虑的主要区别是：

- Azure 诊断扩展只能与 Azure 虚拟机一起使用。 日志分析代理可用于 Azure 中的虚拟机、其他云和本地。
- Azure 诊断扩展将数据发送到 Azure 存储[、Azure 监视器指标](data-platform-metrics.md)（仅限 Windows）和事件中心。 日志分析代理将数据收集到 Azure[监视器日志](data-platform-logs.md)。
- [解决方案](../monitor-reference.md#insights-and-core-solutions)[、VM 的 Azure 监视器](../insights/vminsights-overview.md)和其他服务（如[Azure 安全中心](/azure/security-center/)）需要日志分析代理。

## <a name="costs"></a>成本
Azure 诊断扩展不收取任何费用，但可能会对引入的数据产生费用。 检查要收集数据的目标的[Azure 监视器定价](https://azure.microsoft.com/pricing/details/monitor/)。

## <a name="data-collected"></a>收集的数据
下表列出了 Windows 和 Linux 诊断扩展可以收集的数据。

### <a name="windows-diagnostics-extension-wad"></a>窗口诊断扩展 （WAD）

| 数据源 | 描述 |
| --- | --- |
| Windows 事件日志   | 来自 Windows 事件日志的事件。 |
| 性能计数器 | 测量操作系统和工作负载不同方面性能的数值。 |
| IIS 日志             | 在来宾操作系统上运行的 IIS 网站的使用情况信息。 |
| 应用程序日志     | 跟踪应用程序写入的消息。 |
| .NET EventSource 日志 |使用 .NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) 类的代码编写事件 |
| [基于清单的 ETW 日志](https://docs.microsoft.com/windows/desktop/etw/about-event-tracing) |任何进程生成的 Windows 事件的事件跟踪。 |
| 故障转储（日志）   | 有关应用程序崩溃时进程状态的信息。 |
| 基于文件的日志    | 应用程序或服务创建的日志。 |
| 代理诊断日志 | 有关 Azure 诊断本身的信息。 |


### <a name="linux-diagnostics-extension-lad"></a>Linux 诊断扩展 （LAD）

| 数据源 | 描述 |
| --- | --- |
| Syslog | 发送到 Linux 事件日志记录系统的事件。   |
| 性能计数器  | 测量操作系统和工作负载不同方面性能的数值。 |
| 日志文件 | 发送到基于文件的日志的条目。  |

## <a name="data-destinations"></a>数据目标
Windows 和 Linux 的 Azure 诊断扩展始终将数据收集到 Azure 存储帐户中。 请参阅[安装和配置 Windows Azure 诊断扩展 （WAD）](diagnostics-extension-windows-install.md)和使用[Linux 诊断扩展来监视](../../virtual-machines/extensions/diagnostics-linux.md)收集此数据的特定表和 blob 的列表的指标和日志。

配置一个或多个*数据接收器*以将数据发送到其他其他目标。 以下部分列出了可用于 Windows 和 Linux 诊断扩展的接收器。

### <a name="windows-diagnostics-extension-wad"></a>窗口诊断扩展 （WAD）

| 目标 | 描述 |
|:---|:---|
| Azure Monitor 指标 | 将性能数据收集到 Azure 监视器指标。 请参阅[将来宾 OS 指标发送到 Azure 监视器指标数据库](collect-custom-metrics-guestos-resource-manager-vm.md)。  |
| 事件中心 | 使用 Azure 事件中心在 Azure 外部发送数据。 请参阅[将 Azure 诊断数据流式传输到事件中心](diagnostics-extension-stream-event-hubs.md) |
| Azure 存储 Blob | 除了表之外，将数据写入 Azure 存储中的 Blob。 |
| Application Insights | 从 VM 中运行的应用程序收集数据到应用程序见解，以便与其他应用程序监视集成。 请参阅[将诊断数据发送到应用程序见解](diagnostics-extension-to-application-insights.md)。 |

还可以将 WAD 数据从存储收集到日志分析工作区中，以便使用 Azure 监视器日志进行分析，尽管日志分析代理通常用于此功能。 它可以将数据直接发送到日志分析工作区，并支持提供其他功能的解决方案和见解。  请参阅[从 Azure 存储收集 Azure 诊断日志](diagnostics-extension-logs.md)。 


### <a name="linux-diagnostics-extension-lad"></a>Linux 诊断扩展 （LAD）
LAD 将数据写入 Azure 存储中的表。 它支持下表中的接收器。

| 目标 | 描述 |
|:---|:---|
| 事件中心 | 使用 Azure 事件中心在 Azure 外部发送数据。 |
| Azure 存储 Blob | 除了表之外，将数据写入 Azure 存储中的 Blob。 |
| Azure Monitor 指标 | 除 LAD 外，还安装 Telegraf 代理。 请参阅[收集Linux VM的自定义指标与流入数据远程格拉夫代理](collect-custom-metrics-linux-telegraf.md)。


## <a name="installation-and-configuration"></a>安装和配置
诊断扩展在 Azure 中作为[虚拟机扩展](../../virtual-machines/extensions/overview.md)实现，因此它支持使用资源管理器模板、PowerShell 和 CLI 的相同安装选项。 有关安装和维护虚拟机扩展的一般详细信息，请参阅 Windows 和[虚拟机扩展和 Linux](../../virtual-machines/extensions/features-linux.md)的[虚拟机扩展和](../../virtual-machines/extensions/features-windows.md)功能。

您还可以在虚拟机菜单的 **"监视"** 部分下的 **"诊断设置**"下，在 Azure 门户中安装和配置 Windows 和 Linux 诊断扩展。

有关安装和配置 Windows 和 Linux 的诊断扩展的详细信息，请参阅以下文章。

- [安装和配置 Windows Azure 诊断扩展 （WAD）](diagnostics-extension-windows-install.md)
- [使用 Linux 诊断扩展监视指标和日志](../../virtual-machines/extensions/diagnostics-linux.md)

## <a name="other-documentation"></a>其他文档

###  <a name="azure-cloud-service-classic-web-and-worker-roles"></a>Azure 云服务（经典）Web 和辅助角色
- [云服务监视简介](../../cloud-services/cloud-services-how-to-monitor.md)
- [在 Azure 云服务中启用 Azure 诊断](../../cloud-services/cloud-services-dotnet-diagnostics.md)
- [Azure 云服务的应用程序见解](../app/cloudservices.md)<br>[使用 Azure 诊断跟踪云服务应用程序的流](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md) 

### <a name="azure-service-fabric"></a>Azure Service Fabric
- [监视和诊断本地机器开发设置中的服务](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

## <a name="next-steps"></a>后续步骤


* 了解如何[在 Azure 诊断中使用性能计数器](../../cloud-services/diagnostics-performance-counters.md)。
* 如果在 Azure 存储表中启动或查找数据时遇到诊断问题，请参阅["故障拍摄 Azure 诊断"](diagnostics-extension-troubleshooting.md)

