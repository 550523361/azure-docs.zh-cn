---
title: 在 Azure 安全中心集成安全解决方案 | Microsoft Docs
description: 了解如何将 Azure 安全中心与合作伙伴集成，以增强 Azure 资源的总体安全性。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: memildin
ms.openlocfilehash: 23a00c766dbb38853c57c91e7f59ec364390c44b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245377"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>在 Azure 安全中心集成安全解决方案
本文档介绍如何管理已连接到 Azure 安全中心的安全解决方案，以及如何添加新的安全解决方案。

> [!NOTE]
> 2019 年 7 月 31 日，安全解决方案子集已停用。 有关详细信息和替代服务，请参阅[安全中心停用功能（2019 年 7 月）。](security-center-features-retirement-july2019.md#menu_solutions)

## <a name="integrated-azure-security-solutions"></a>集成式 Azure 安全解决方案
可以通过安全中心轻松地在 Azure 中启用集成式安全解决方案。 优势包括：

- **简化部署**：安全中心提供对集成式合作伙伴解决方案的简化预配。 对于反恶意软件和漏洞评估等解决方案，安全中心可以在虚拟机上预配代理。 对于防火墙设备，安全中心可以处理所需的大部分网络配置。
- **集成检测**：合作伙伴解决方案中的安全事件将自动收集、聚合，并作为安全中心警报和事件的一部分显示。 这些事件还与来自其他源的检测融合在一起，以提供高级威胁检测功能。
- **统一运行状况监视和管理**：利用集成运行状况事件，客户可以一目了然地监视所有合作伙伴解决方案。 可通过使用合作伙伴解决方案轻松地访问高级设置，进行基本管理。

目前，集成安全解决方案包括[Qualys](https://www.qualys.com/public-cloud/#azure)和[Rapid7](https://www.rapid7.com/products/insightvm/)和 Microsoft 应用程序网关 Web 应用程序防火墙的漏洞评估。

> [!NOTE]
> 安全中心不会在合作伙伴虚拟设备上安装 Microsoft 监视代理，因为大多数安全供应商禁止在其设备上运行外部代理。
>
>

## <a name="how-security-solutions-are-integrated"></a>安全中心如何集成
从安全中心部署的 Azure 安全解决方案是自动连接的。 您还可以连接其他安全数据源，包括本地或其他云中运行的计算机。

![合作伙伴解决方案集成](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>管理集成式 Azure 安全解决方案和其他数据源

1. 登录到 Azure[门户](https://azure.microsoft.com/features/azure-portal/)。

2. 在**Microsoft Azure 菜单上**，选择**安全中心**。 此时会打开“安全中心 - 概览”。****

3. 在“安全中心”菜单下，选择“安全解决方案”****。

   ![安全中心概述](./media/security-center-partner-integration/overview.png)

在**安全解决方案**中，您可以看到集成 Azure 安全解决方案的运行状况并运行基本管理任务。

### <a name="connected-solutions"></a>已连接解决方案

**"已连接的解决方案**"部分包括当前连接到安全中心的安全解决方案。 它还显示每个解决方案的运行状况。  

![已连接解决方案](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

合作伙伴解决方案的状态可能为：

* 健康（绿色） - 没有健康问题。
* 不健康（红色） - 有一个健康问题，需要立即关注。
* 运行状况问题（橙色）- 解决方案已停止报告其运行状况。
* 未报告（灰色） - 解决方案尚未报告任何内容，并且没有可用的健康数据。 如果解决方案最近连接且仍在部署，则其状态可能未报告。

> [!NOTE]
> 如果没有运行状况数据可用，则安全中心会显示上次收到事件的日期和时间以指示解决方案是否正在报告。 如果没有可用的运行状况数据，并且在过去 14 天内未收到警报，则安全中心指示解决方案不正常或不报告。
>
>

1. 选择 **"查看"** 以获取其他信息和选项，例如：

   - **解决方案控制台**。 打开此解决方案的管理体验。
   - **链接 VM**。 打开链接应用程序页面。 此处，可将资源连接到合作伙伴解决方案。
   - **删除解决方案**。
   - **配置**。

   ![合作伙伴解决方案详细信息](./media/security-center-partner-solutions/partner-solutions-detail.png)

### <a name="discovered-solutions"></a>已发现解决方案

安全中心会自动发现在 Azure 中运行但未连接到安全中心的安全解决方案，并在 **"发现的解决方案"** 部分中显示这些解决方案。 这些解决方案包括 Azure 解决方案，如[Azure AD 标识保护](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)以及合作伙伴解决方案。

> [!NOTE]
> 在订阅级别，安全中心标准层是已发现解决方案功能所必需的。 请参阅[定价](security-center-pricing.md)以了解有关定价层的更多情况。
>
>

选择解决方案下的**CONNECT**以与安全中心集成，并通知安全警报。

### <a name="add-data-sources"></a>添加数据源

****“添加数据源”部分包括其他可以连接的可用数据源。 如需从任何此类源添加数据的说明，请单击“添加”。****

![数据源](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)

## <a name="exporting-data-to-a-siem"></a>将数据导出到 SIEM

> [!NOTE]
> 有关将数据导出到 SIEM 的更简单方法（当前处于预览版）的详细信息，请参阅[导出安全警报和建议（预览）。](continuous-export.md) 新方法不使用活动日志作为中介，允许从安全中心直接导出到事件中心（然后导出到您的 SIEM），它还支持安全建议的出口。


您可以配置 SIEM 或其他监视工具以接收 Azure 安全中心事件。

Azure 安全中心的所有事件都发布到 Azure 监视器的 Azure[活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)。 Azure 监视器使用[整合管道](../azure-monitor/platform/stream-monitoring-data-event-hubs.md)将数据流式传输到事件中心，然后将其拉入监视工具。

以下各节介绍如何将数据配置为流式传输到事件中心。 步骤假设 Azure 订阅中已配置了 Azure 安全中心。

### <a name="high-level-overview"></a>综合概述

![高级概述](media/security-center-export-data-to-siem/overview.png)

### <a name="what-is-the-azure-security-data-exposed-to-siem"></a>公开给 SIEM 的 Azure 安全数据是什么？

在此版本中，我们公开[安全警报。](../security-center/security-center-managing-and-responding-alerts.md) 在即将发布的版本中，我们将通过安全建议丰富数据集。

### <a name="how-to-set-up-the-pipeline"></a>如何设置管道

#### <a name="create-an-event-hub"></a>创建事件中心

开始之前，[请创建事件中心命名空间](../event-hubs/event-hubs-create.md)- 所有监视数据的目标。

#### <a name="stream-the-azure-activity-log-to-event-hubs"></a>将 Azure 活动日志流式传输到事件中心

请参阅以下文章[流活动日志到事件中心](../azure-monitor/platform/activity-logs-stream-event-hubs.md)。

#### <a name="install-a-partner-siem-connector"></a>安装合作伙伴 SIEM 连接器 

通过 Azure Monitor 将监视数据路由到事件中心，可与合作伙伴 SIEM 和监视工具轻松集成。

有关[受支持的 SIEM](../azure-monitor/platform/stream-monitoring-data-event-hubs.md#partner-tools-with-azure-monitor-integration)的列表，请参阅以下文章。

### <a name="example-for-querying-data"></a>查询数据示例 

下面是一些可用于提取警报数据的 Splunk 查询：

| **查询说明** | **查询** |
|----|----|
| 所有警报| index=main Microsoft.Security/locations/alerts|
| 按名称汇总操作计数| index=main sourcetype="amal:security" \| table operationName \| stats count by operationName|
| 获取警报信息：时间、名称、状态、ID 和订阅 | index=main Microsoft.Security/locations/alerts \| table \_time, properties.eventName, State, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>后续步骤

本文介绍了如何在安全中心集成合作伙伴的解决方案。 若要详细了解安全中心，请参阅以下文章：

* [在安全中心进行安全运行状况监视](security-center-monitoring.md)。 了解如何监视 Azure 资源的运行状况。