---
title: 使用 Azure Monitor 日志监视 Azure HDInsight 群集
description: 了解如何使用 Azure Monitor 日志监视 HDInsight 群集中运行的作业。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 4007d18291e91ae2a72d7a99dac2cb3a8bc2877a
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82143852"
---
# <a name="use-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>使用 Azure Monitor 日志监视 HDInsight 群集

了解如何启用 Azure Monitor 日志来监视 HDInsight 中的 Hadoop 群集操作。 以及如何添加 HDInsight 监视解决方案。

[Azure Monitor 日志](../log-analytics/log-analytics-overview.md)是监视云和本地环境的 Azure Monitor 服务。 监视是为了保持其可用性和性能。 它收集云、本地环境和其他监视工具中的资源生成的数据。 数据用于跨多个源提供分析。

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

如果还没有 Azure 订阅，可以在开始前[创建一个免费帐户](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>先决条件

* Log Analytics 工作区。 可以将此工作区视为唯一的 Azure Monitor 日志环境，其中包含自己的数据存储库、数据源和解决方案。 有关说明，请参阅[创建 Log Analytics 工作区](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace)。

* 一个 Azure HDInsight 群集。 目前，可以将 Azure Monitor 日志与以下 HDInsight 群集类型一起使用：

  * Hadoop
  * HBase
  * 交互式查询
  * Kafka
  * Spark
  * Storm

  有关如何创建 HDInsight 群集的说明，请参阅 [Azure HDInsight 入门](hadoop/apache-hadoop-linux-tutorial-get-started.md)。  

* Azure PowerShell Az 模块。  请参阅[新 Azure PowerShell Az Module 简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az)。 确保具有最新版本。 如有必要， `Update-Module -Name Az`请运行。

> [!NOTE]  
> 建议将 HDInsight 群集和 Log Analytics 工作区放置在同一区域中，以实现更好的性能。 Azure Monitor 日志在所有 Azure 区域中均不可用。

## <a name="enable-azure-monitor-logs-by-using-the-portal"></a>使用门户启用 Azure Monitor 日志

在本部分中，将配置现有的 HDInsight Hadoop 群集，以使用 Azure Log Analytics 工作区监视作业、调试日志等。

1. 在 [Azure 门户](https://portal.azure.com/)中，选择群集。 群集会在新的门户页中打开。

1. 从左侧的 "**监视**" 下，选择**Azure Monitor**。

1. 在主视图中，在 " **Azure Monitor 集成**" 下，选择 "**启用**"。

1. 在“选择工作区”下拉列表中，选择现有的 Log Analytics 工作区****。

1. 选择“保存”。   需要几分钟来保存设置。

    ![为 HDInsight 群集启用监视](./media/hdinsight-hadoop-oms-log-analytics-tutorial/azure-portal-monitoring.png "为 HDInsight 群集启用监视")

## <a name="enable-azure-monitor-logs-by-using-azure-powershell"></a>使用 Azure PowerShell 启用 Azure Monitor 日志

可以使用 Azure PowerShell Az module [AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/enable-azhdinsightmonitoring) cmdlet 启用 Azure Monitor 日志。

```powershell
# Enter user information
$resourceGroup = "<your-resource-group>"
$cluster = "<your-cluster>"
$LAW = "<your-Log-Analytics-workspace>"
# End of user input

# obtain workspace id for defined Log Analytics workspace
$WorkspaceId = (Get-AzOperationalInsightsWorkspace `
                    -ResourceGroupName $resourceGroup `
                    -Name $LAW).CustomerId

# obtain primary key for defined Log Analytics workspace
$PrimaryKey = (Get-AzOperationalInsightsWorkspace `
                    -ResourceGroupName $resourceGroup `
                    -Name $LAW | Get-AzOperationalInsightsWorkspaceSharedKeys).PrimarySharedKey

# Enables monitoring and relevant logs will be sent to the specified workspace.
Enable-AzHDInsightMonitoring `
    -ResourceGroupName $resourceGroup `
    -Name $cluster `
    -WorkspaceId $WorkspaceId `
    -PrimaryKey $PrimaryKey

# Gets the status of monitoring installation on the cluster.
Get-AzHDInsightMonitoring `
    -ResourceGroupName $resourceGroup `
    -Name $cluster
```

若要禁用，请使用[AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/disable-azhdinsightmonitoring) cmdlet：

```powershell
Disable-AzHDInsightMonitoring -Name "<your-cluster>"
```

## <a name="install-hdinsight-cluster-management-solutions"></a>安装 HDInsight 群集管理解决方案

HDInsight 提供特定于群集的管理解决方案，可为 Azure Monitor 日志添加这些解决方案。 [管理解决方案](../log-analytics/log-analytics-add-solutions.md)可将功能添加到 Azure Monitor 日志，同时提供额外的数据和分析工具。 这些解决方案从 HDInsight 群集收集重要的性能指标。 和提供用于搜索指标的工具。 这些解决方案还为 HDInsight 支持的大多数群集类型提供可视化和仪表板。 使用解决方案收集指标后，即可利用这些指标创建自定义监视规则和警报。

可用的 HDInsight 解决方案：

* HDInsight Hadoop 监视
* HDInsight HBase 监视
* HDInsight 交互式查询监视
* HDInsight Kafka 监视
* HDInsight Spark 监视
* HDInsight Storm 监视

有关管理解决方案的说明，请参阅[Azure 中的管理解决方案](../azure-monitor/insights/solutions.md#install-a-monitoring-solution)。 若要进行试验，请安装 HDInsight Hadoop 监视解决方案。 完成后，将看到 "**摘要**" 下面列出了**HDInsightHadoop**磁贴。 选择“HDInsightHadoop”磁贴****。 HDInsightHadoop 解决方案如下所示：

![HDInsight 监视解决方案视图](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

由于群集是全新的群集，因此报告不会显示任何活动。

## <a name="configuring-performance-counters"></a>配置性能计数器

Azure monitor 支持收集和分析群集中节点的性能指标。 有关详细信息，请参阅[中的 Linux 性能数据源 Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-performance-counters#linux-performance-counters)。

## <a name="cluster-auditing"></a>群集审核

HDInsight 通过导入以下类型的日志来支持 Azure Monitor 日志的群集审核：

* `log_gateway_audit_CL`-此表提供了可显示成功和失败的登录尝试的群集网关节点审核日志。
* `log_auth_CL`-此表提供了成功和失败的登录尝试的 SSH 日志。
* `log_ambari_audit_CL`-此表提供 Ambari 中的审核日志。
* `log_ranger_audti_CL`-此表提供了 Apache Ranger on ESP 群集的审核日志。

## <a name="next-steps"></a>后续步骤

* [查询 Azure Monitor 日志以监视 HDInsight 群集](hdinsight-hadoop-oms-log-analytics-use-queries.md)
* [如何通过 Apache Ambari 和 Azure Monitor 日志监视群集可用性](./hdinsight-cluster-availability.md)
