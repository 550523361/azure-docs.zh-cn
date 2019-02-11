---
title: 将 Hive 与 Apache Hadoop 配合使用以进行网站日志分析 - Azure HDInsight
description: 了解如何通过将 Apache Hive 与 HDInsight 配合使用来分析网站日志。 我们将使用日志文件作为 HDInsight 表的输入，并使用 HiveQL 来查询数据。
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/17/2016
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 30bfaad8fcc1a837a37689280149a6dbe20b7c1d
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2018
ms.locfileid: "53628145"
---
# <a name="use-apache-hive-with-windows-based-hdinsight-to-analyze-logs-from-websites"></a>将 Apache Hive 与基于 Windows 的 HDInsight 配合使用以分析来自网站的日志
了解如何通过将 HiveQL 与 HDInsight 配合使用来分析来自网站的日志。 网站日志分析可用于根据类似活动分类受众，按人口统计分类站点访问者，以及了解他们查看的内容和这些内容来自的网站等。

> [!IMPORTANT]  
> 本文档中的步骤仅适用于基于 Windows 的 HDInsight 群集。 Windows 上仅可使用低于 HDInsight 3.4 版本的 HDInsight。 Linux 是 HDInsight 3.4 或更高版本上使用的唯一操作系统。 有关详细信息，请参阅 [HDInsight 在 Windows 上停用](../hdinsight-component-versioning.md#hdinsight-windows-retirement)。

在此示例中，使用 HDInsight 群集来分析网站日志文件，以深入了解每天从外部网站访问网站的频率。 还将生成用户遇到的网站错误的摘要。 学习如何：

* 连接到 Azure Blob 存储，其中包含网站日志文件。
* 创建 HIVE 表以查询这些日志。
* 创建 HIVE 查询以分析数据。
* 使用 Microsoft Excel 连接到 HDInsight（通过使用开放的数据库连接 (ODBC)），以检索分析的数据。

![HDI.Samples.Website.Log.Analysis](./media/apache-hive-analyze-website-log/hdinsight-weblogs-sample.png)

## <a name="prerequisites"></a>先决条件
* 必须已在 Azure HDInsight 上预配了 Apache Hadoop 群集。 有关说明，请参阅[预配 HDInsight 群集](../hdinsight-hadoop-provision-linux-clusters.md)。
* 必须已安装 Microsoft Excel 2013 或 Excel 2010。
* 必须拥有 [Microsoft Hive ODBC 驱动程序](https://www.microsoft.com/download/details.aspx?id=40886)，才能将数据从 Hive 导入 Excel。

## <a name="to-run-the-sample"></a>运行示例
1. 从 [Azure 门户](https://portal.azure.com/)的启动板（如果在此处固定群集），单击要在其上运行示例的群集磁贴。
2. 在群集边栏选项卡中的“快速链接”下单击“群集仪表板”，并在“群集仪表板”边栏选项卡中单击“HDInsight 群集仪表板”。 或者，也可以通过使用以下 URL 直接打开仪表板：

         https://<clustername>.azurehdinsight.net

    出现提示时，通过使用设置群集时所用的管理员用户名和密码进行身份验证。
3. 在打开的网页中，单击“入门库”选项卡，并在“使用示例数据的解决方案”类别下方，单击“网站日志分析”示例。
4. 按照网页上提供的说明完成该示例。

## <a name="next-steps"></a>后续步骤
请尝试以下示例：[通过将 Hive 与 HDInsight 配合使用分析传感器数据](apache-hive-analyze-sensor-data.md)。

[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md
