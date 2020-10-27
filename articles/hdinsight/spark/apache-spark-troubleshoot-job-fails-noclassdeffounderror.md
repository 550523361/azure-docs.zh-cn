---
title: NoClassDefFoundError-与 Azure HDInsight 中的 Apache Kafka 数据 Apache Spark
description: 从 Apache Kafka 群集读取数据的 Apache Spark 流式处理作业失败，并在 Azure HDInsight 中出现 NoClassDefFoundError
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 0185e1631b63bc2c6c6cc9e2f6d1e5efcb72858c
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2020
ms.locfileid: "92539046"
---
# <a name="apache-spark-streaming-job-that-reads-apache-kafka-data-fails-with-noclassdeffounderror-in-hdinsight"></a>读取 Apache Kafka 数据的 Apache Spark 流式处理作业失败，在 HDInsight 中显示 NoClassDefFoundError

本文介绍在 Azure HDInsight 群集中使用 Apache Spark 组件时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

Apache Spark 群集运行从 Apache Kafka 群集读取数据的 Spark 流式处理作业。 如果启用了 Kafka 流压缩，则 Spark 流式处理作业将会失败。 在这种情况下，由于出现错误，Spark 流式处理 Yarn 应用 application_1525986016285_0193 失败：

```
18/05/17 20:01:33 WARN YarnAllocator: Container marked as failed: container_e25_1525986016285_0193_01_000032 on host: wn87-Scaled.2ajnsmlgqdsutaqydyzfzii3le.cx.internal.cloudapp.net. Exit status: 50. Diagnostics: Exception from container-launch.
Container id: container_e25_1525986016285_0193_01_000032
Exit code: 50
Stack trace: ExitCodeException exitCode=50: 
 at org.apache.hadoop.util.Shell.runCommand(Shell.java:944)
```

## <a name="cause"></a>原因

此错误可能是由于指定的 `spark-streaming-kafka` jar 文件版本与正在运行的 Kafka 群集的版本不同而导致。

例如，如果运行的是 Kafka 群集版本 0.10.1，则以下命令将导致错误：

```
spark-submit \
--packages org.apache.spark:spark-streaming-kafka-0-8_2.11:2.2.0
--conf spark.executor.instances=16 \
...
~/Kafka_Spark_SQL.py <bootstrap server details>
```

## <a name="resolution"></a>解决方法

使用带 `–packages` 选项的 Spark-submit 命令，并确保 spark-streaming-kafka jar 文件的版本与正在运行的 kafka 群集的版本相同。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* 通过 [Azure 社区支持](https://azure.microsoft.com/support/community/)获取 Azure 专家的解答。

* 与 [@AzureSupport](https://twitter.com/azuresupport)（Microsoft Azure 官方帐户）联系，它可以将 Azure 社区与适当的资源（解答、支持人员和专家）相关联来改善客户体验。

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”，或打开“帮助 + 支持”中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](../../azure-portal/supportability/how-to-create-azure-support-request.md)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。