---
title: 在 Log Analytics 中分析数据使用情况| Microsoft Docs
description: 使用 Log Analytics 中的“使用情况和估计的成本”仪表板来估算发送到 Log Analytics 的数据，确定哪些原因可能导致增量无法预见。
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/11/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 1bb96821b61647f5dfad54c8b0cb6248eb0db4af
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2018
ms.locfileid: "42144173"
---
# <a name="analyze-data-usage-in-log-analytics"></a>在 Log Analytics 中分析数据使用情况

> [!NOTE]
> 本文介绍如何在 Log Analytics 中分析数据使用情况。  有关相关信息，请参阅以下文章。
> - [通过在 Log Analytics 中控制数据量和保留期管理成本](log-analytics-manage-cost-storage.md)介绍如何通过更改数据保留期来控制成本。
> - [监视使用情况和估算成本](../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md)介绍如何针对不同的定价模型查看多个 Azure 监视功能的使用情况和估算成本。 它还介绍如何更改定价模型。

Log Analytics 包括以下信息：收集的数据量、哪些源发送了数据、所发送数据的不同类型。  使用“Log Analytics 使用情况”仪表板查看和分析数据使用情况。 该仪表板显示每个解决方案收集的数据量，以及计算机所发送的数据量。

## <a name="understand-the-usage-dashboard"></a>了解“使用情况”仪表板
“Log Analytics 使用情况”仪表板会显示以下信息：

- 数据量
    - 一段时间的数据量（基于当前的时间范围）
    - 按解决方案统计的数据量
    - 未与计算机关联的数据
- 计算机
    - 发送数据的计算机
    - 过去 24 小时内无数据的计算机
- 产品/服务
    - 见解与分析节点
    - 自动化与控制节点
    - 安全节点  
- 性能
    - 收集数据和为数据建索引所花的时间  
- 查询列表

![使用情况和成本仪表板](./media/log-analytics-manage-cost-storage/usage-estimated-cost-dashboard-01.png)<br>
)

### <a name="to-work-with-usage-data"></a>处理使用情况数据
1. 登录到 [Azure 门户](https://portal.azure.com)。
2. 在 Azure 门户中，单击“所有服务”。 在资源列表中，键入“Log Analytics”。 开始键入时，会根据输入筛选该列表。 选择“Log Analytics”。<br><br> ![Azure 门户](./media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
3. 在 Log Analytics 工作区列表中选择一个工作区。
4. 在左窗格的列表中，选择“使用情况和预估成本”。
5. 在“使用情况和预估成本”仪表板中，可以通过选择“时间: 过去 24 小时”来修改时间范围，对时间间隔进行更改。<br><br> ![时间间隔](./media/log-analytics-usage/usage-time-filter-01.png)<br><br>
6. 查看“使用情况类别”边栏选项卡以显示感兴趣的区域。 选择一个边栏选项卡，并单击其中的项以在“[日志搜索](log-analytics-log-searches.md)”中查看更多详细信息。<br><br> ![示例数据使用量 kpi](media/log-analytics-usage/data-volume-kpi-01.png)<br><br>
7. 在“日志搜索”仪表板中，查看搜索返回的结果。<br><br> ![日志搜索用法示例](./media/log-analytics-usage/usage-log-search-01.png)

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>当数据收集量高于预期时创建警报
本部分介绍如何在以下情况下创建警报：
- 数据量超过指定的量。
- 预测数据量会超过指定的量。

Azure 警报支持使用搜索查询的[日志警报](../monitoring-and-diagnostics/monitor-alerts-unified-log.md)。 

如果在过去 24 小时内收集的数据超过 100 GB，则以下查询就会有结果：

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`

以下查询使用简单的公式来预测在一天中发送的数据何时会超过 100 GB： 

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`

若要针对其他数据量发出警报，请在查询中将 100 更改为要发出警报的 GB 数。

执行[创建新的日志警报](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md)中介绍的步骤，当数据收集量超出预期时，系统就会发出通知。

为第一个查询创建警报时，如果 24 小时内的数据超出 100 GB，则请进行如下设置：  

- **定义警报条件**将 Log Analytics 工作区指定为资源目标。
- **警报条件**指定下列项：
   - **信号名称**选择“自定义日志搜索”。
   - 将“搜索查询”设置为 `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`
   - **警报逻辑****基于***结果数*，**条件***大于***阈值** *0*
   - 将“时间段”设置为 1440 分钟，“警报频率”设置为每 60 分钟，因为使用情况数据一小时才更新一次。
- **定义警报详细信息**指定以下项：
   - 将“名称”设置为“24 小时内的数据量大于 100 GB”
   - 将“严重性”设置为“警告”

指定现有的操作组或创建一个新[操作组](../monitoring-and-diagnostics/monitoring-action-groups.md)，以便当日志警报匹配条件时，你会收到通知。

为第二个查询创建警报时，如果预测 24 小时内的数据会超出 100 GB，则请进行如下设置：

- **定义警报条件**将 Log Analytics 工作区指定为资源目标。
- **警报条件**指定下列项：
   - **信号名称**选择“自定义日志搜索”。
   - 将“搜索查询”设置为 `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`
   - **警报逻辑****基于***结果数*，**条件***大于***阈值** *0*
   - 将“时间段”设置为 180 分钟，“警报频率”设置为每 60 分钟，因为使用情况数据一小时才更新一次。
- **定义警报详细信息**指定以下项：
   - 将“名称”设置为“预期 24 小时内的数据量大于 100 GB”
   - 将“严重性”设置为“警告”

指定现有的操作组或创建一个新[操作组](../monitoring-and-diagnostics/monitoring-action-groups.md)，以便当日志警报匹配条件时，你会收到通知。

收到警报后，请执行以下部分介绍的步骤，排查使用量超出预期的原因。

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>排查使用量超出预期的原因
可以通过“使用情况”仪表板来确定使用量（以及由此带来的费用）超出预期的原因。

使用量较高是由下面的一个或两个原因引起的：
- 发送到 Log Analytics 的数据量超出预期
- 将数据发送到 Log Analytics 的节点数超出预期

### <a name="check-if-there-is-more-data-than-expected"></a>查看数据量是否超出预期 
使用情况页有两个关键部分，可以从中了解大多数数据的收集原因。

“一段时间的数据量”图表显示发送的数据总量以及发送最多数据的计算机。 从顶部的图表可以看出总体数据使用情况：正在增长、保持平稳或正在下降。 计算机列表显示发送最多数据的 10 台计算机。

“按解决方案统计的数据量”图表显示每个解决方案发送的数据量，以及发送最多数据的解决方案。 顶部的图表显示每个解决方案在一段时间发送的总数据量。 可以据此确定某个解决方案在一段时间发送的数据量是过多、大致持平还是过少。 解决方案列表显示发送最多数据的 10 个解决方案。 

这两个图表显示所有数据。 某些数据收费，某些数据免费。 如果只想专注于收费数据，请修改搜索页上的查询，使之包括 `IsBillable=true`。  

![数据量图表](./media/log-analytics-usage/log-analytics-usage-data-volume.png)

查看“一段时间的数据量”图表。 若要查看为特定计算机发送最多数据的解决方案和数据类型，请单击计算机的名称。 单击列表中第一台计算机的名称。

在以下屏幕截图中，“日志管理/性能”数据类型为计算机发送了最多的数据。<br><br> ![计算机的数据量](./media/log-analytics-usage/log-analytics-usage-data-volume-computer.png)<br><br>

接下来回到“使用情况”仪表板，查看“按解决方案统计的数据量”图表。 若要查看为解决方案发送最多数据的计算机，请单击列表中解决方案的名称。 单击列表中第一个解决方案的名称。 

在以下屏幕截图中，可以确认 mycon 计算机为“日志管理”解决方案发送了最多数据。<br><br> ![解决方案的数据量](./media/log-analytics-usage/log-analytics-usage-data-volume-solution.png)<br><br>

根据需要执行其他分析，确定某个解决方案或数据类型中的大型卷。 查询示例如下：

+ “安全”解决方案
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ “日志管理”解决方案
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ “性能”数据类型
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ “事件”数据类型
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ “Syslog”数据类型
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ AzureDiagnostics 数据类型
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

通过以下步骤减少所收集日志的量：

| 高数据量来源 | 如何减少数据量 |
| -------------------------- | ------------------------- |
| 安全性事件            | 选择[通用或最低安全性事件](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) <br> 更改安全审核策略，只收集所需事件。 具体而言，请查看是否需要收集以下对象的事件： <br> - [审核筛选平台](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [审核注册表](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [审核文件系统](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [审核内核对象](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [审核句柄操作](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - 审核可移动存储 |
| 性能计数器       | 更改[性能计数器配置](log-analytics-data-sources-performance-counters.md)如下： <br> - 降低收集频率 <br> - 减少性能计数器数 |
| 事件日志                 | 更改[事件日志配置](log-analytics-data-sources-windows-events.md)如下： <br> - 减少收集的事件日志数 <br> - 仅收集必需的事件级别。 例如，不收集“信息”级别事件 |
| Syslog                     | 更改 [syslog 配置](log-analytics-data-sources-syslog.md)如下： <br> - 减少收集的设施数 <br> - 仅收集必需的事件级别。 例如，不收集“信息”和“调试”级别事件 |
| AzureDiagnostics           | 更改资源日志集合，以便： <br> - 减少到 Log Analytics 的资源发送日志的数目 <br> - 仅收集必需的日志 |
| 不需解决方案的计算机中的解决方案数据 | 使用[解决方案目标](../operations-management-suite/operations-management-suite-solution-targeting.md)，只从必需的计算机组收集数据。 |

### <a name="check-if-there-are-more-nodes-than-expected"></a>查看节点数是否超出预期
如果你位于“按节点(OMS)”定价层，则根据所用节点和解决方案数收费。 可以在使用情况仪表板的产品/服务部分中查看使用了每项产品的多少个节点。<br><br> ![使用情况仪表板](./media/log-analytics-usage/log-analytics-usage-offerings.png)<br><br>

单击“全部查看...”，查看为所选产品/服务发送数据的计算机的完整列表。

使用[解决方案目标](../operations-management-suite/operations-management-suite-solution-targeting.md)，只从必需的计算机组收集数据。

## <a name="next-steps"></a>后续步骤
* 若要了解如何使用搜索语言，请参阅 [Log Analytics 中的日志搜索](log-analytics-log-searches.md)。 可以使用搜索查询，对使用情况数据执行其他分析。
* 执行[创建新的日志警报](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md)中介绍的步骤，当满足搜索条件时，系统就会通知你。
* 使用[解决方案目标](../operations-management-suite/operations-management-suite-solution-targeting.md)，只从必需的计算机组收集数据。
* 若要配置有效的安全事件收集策略，请参阅 [Azure 安全中心筛选策略](../security-center/security-center-enable-data-collection.md)。
* 更改[性能计数器配置](log-analytics-data-sources-performance-counters.md)。
* 若要修改事件收集设置，请参阅[事件日志配置](log-analytics-data-sources-windows-events.md)。
* 若要修改 syslog 收集设置，请参阅 [syslog 配置](log-analytics-data-sources-syslog.md)。
