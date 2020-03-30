---
title: 使用 Azure 监视器日志监视容器
description: 使用 Azure 监视器日志监视在 Azure 服务结构群集上运行的容器。
author: srrengar
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 8d4231de13da3f8b2960bd4852136f803a97a546
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614428"
---
# <a name="monitor-containers-with-azure-monitor-logs"></a>使用 Azure 监视器日志监视容器
 
本文介绍设置 Azure 监视器日志容器监视解决方案以查看容器事件所需的步骤。 若要将群集设置为收集容器事件，请参阅此[分步教程](service-fabric-tutorial-monitoring-wincontainers.md)。 

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="set-up-the-container-monitoring-solution"></a>设置容器监视解决方案

> [!NOTE]
> 您需要为群集设置 Azure 监视器日志，并需要在节点上部署日志分析代理。 如果没有，请按照[设置 Azure 监视器日志](service-fabric-diagnostics-oms-setup.md)中的步骤操作，然后首先[将日志分析代理添加到群集](service-fabric-diagnostics-oms-agent.md)。

1. 使用 Azure 监视器日志和日志分析代理设置群集后，部署容器。 待容器部署完毕后，再执行下一步。

2. 在 Azure 市场中搜索“容器监视解决方案”，并单击“监视 + 管理”类别下显示的“容器监视解决方案”资源******。

    ![添加容器解决方案](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. 在已为群集创建的同一工作区内创建解决方案。 此更改自动触发代理开始收集容器上的 docker 数据。 约 15 分钟后，应看到解决方案显示了传入日志和统计信息，如下图所示。

    ![基本 Log Analytics 仪表板](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

代理支持收集多个特定于容器的日志，这些日志可以在 Azure 监视器日志中查询，或用于可视化性能指示器。 收集的日志类型：

* ContainerInventory：显示有关容器位置、名称和图像的信息
* ContainerImageInventory：有关已部署映像的信息，包括 ID 或大小
* ContainerLog：特定的错误日志、docker 日志（stdout 等）和其他条目
* ContainerServiceLog：已运行的 docker 守护程序命令
* Perf：性能计数器，包括容器 cpu、内存、网络流量、磁盘 i/o 和主机的自定义指标



## <a name="next-steps"></a>后续步骤
* 了解有关[Azure 监视器日志容器解决方案](../azure-monitor/insights/containers.md)的更多信息。
* 深入了解 Service Fabric 上容器业务流程 - [Service Fabric 和容器](service-fabric-containers-overview.md)
* 熟悉 Azure 监视器日志中提供的[日志搜索和查询](../log-analytics/log-analytics-log-searches.md)功能
* 配置 Azure 监视器日志以设置[自动警报](../log-analytics/log-analytics-alerts.md)规则，以帮助检测和诊断