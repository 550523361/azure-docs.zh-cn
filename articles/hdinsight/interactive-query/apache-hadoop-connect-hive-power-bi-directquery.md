---
title: 在 Azure HDInsight 中使用 Power BI 查看交互式查询 Hive 数据
description: 在 Azure HDInsight 中使用 Microsoft Power BI 直观显示交互式查询 Hive 数据
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/17/2019
ms.openlocfilehash: adcd4d9e81eecad9540a4ef1be5e675f940ffb8d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367986"
---
# <a name="visualize-interactive-query-apache-hive-data-with-microsoft-power-bi-using-direct-query-in-hdinsight"></a>使用 HDInsight 中的直接查询使用 Microsoft Power BI 可视化交互式查询 Apache Hive 数据

本文介绍如何将 Microsoft Power BI 连接到 Azure HDInsight 交互式查询群集并使用直接查询直观显示 Apache Hive 数据。 提供的示例将数据从`hivesampletable`Hive 表加载到 Power BI。 `hivesampletable` Hive 表包含一些移动电话使用情况数据。 然后在世界地图上绘制使用情况数据：

![HDInsight Power BI 地图报表](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

可以利用 [Apache Hive ODBC 驱动程序](../hadoop/apache-hadoop-connect-hive-power-bi.md)，通过 Power BI Desktop 中的通用 ODBC 连接器执行导入操作。 但是，鉴于 Hive 查询引擎的非交互性质，不建议对 BI 工作负荷执行此操作。 出于性能考量，[HDInsight 交互式查询连接器](./apache-hadoop-connect-hive-power-bi-directquery.md)和 [HDInsight Apache Spark 连接器](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect)是更好的选择。

## <a name="prerequisites"></a>先决条件
在开始阅读本文前，必须具备以下项：

* **HDInsight 集群**. 该群集可以是包含 Apache Hive 的 HDInsight 群集，也可以是新发布的交互式查询群集。 有关创建群集的信息，请参阅[创建群集](../hadoop/apache-hadoop-linux-tutorial-get-started.md)。
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**。 可从 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=45331)下载副本。

## <a name="load-data-from-hdinsight"></a>从 HDInsight 加载数据

`hivesampletable` Hive 表附带所有 HDInsight 群集。

1. 启动 Power BI Desktop。

2. 从菜单栏，导航到 **"** > **获取更多数据** > **..."。**

    ![HDInsight 电源 BI 获取更多数据](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)

3. 在“获取数据”窗口的搜索框中输入 hdinsight********。  

4. 从搜索结果中选择“HDInsight 交互式查询”，然后选择“连接”********。  如果看不到“HDInsight 交互式查询”，需要将 Power BI Desktop 更新到最新版本****。

5. 选择“继续”，关闭“连接到第三方服务”对话框。********

6. 在“HDInsight 交互式查询”窗口中输入以下信息，然后选择“确定”********：

    |properties | “值” |
    |---|---|
    |服务器 |输入群集名称，例如，*myiqcluster.azurehdinsight.net*。|
    |数据库 |就本文来说，请输入“default”。****|
    |数据连接模式 |就本文来说，请选择“DirectQuery”。****|

    ![HDInsight 交互式查询 Power BI DirectQuery 连接](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)

7. 输入 HTTP 凭据，然后选择“连接”****。 默认的用户名为 admin****。

8. 在左侧窗格的“导航器”窗口中，选择“hivesampletale”********。

9. 在主窗口中选择“加载”。****

    ![HDInsight 交互式查询 Power BI hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data-on-a-map"></a>在地图上直观显示数据

从上一过程继续进行。

1. 在“可视化”窗格中，选择“地图”（地球图标）****。 然后会在主窗口中显示常规地图。

    ![HDInsight Power BI 自定义报表](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)

2. 在“字段”窗格中，选择 **country** 和 **devicemake**。 一会儿就会在主窗口中显示包含数据点的世界地图。

3. 展开地图。

## <a name="next-steps"></a>后续步骤
本文介绍了如何使用 Microsoft Power BI 直观显示 HDInsight 中的数据。  有关数据可视化的详细信息，请参阅以下文章：

* [在 Azure HDInsight 中通过 Microsoft Power BI 使用 ODBC 直观显示 Apache Hive 数据](../hadoop/apache-hadoop-connect-hive-power-bi.md)。 
* [使用 Apache Zepelin 在 Azure HDInsight 中运行 Apache Hive 查询](../interactive-query/hdinsight-connect-hive-zeppelin.md)。
* [使用 Microsoft Hive ODBC 驱动程序将 Excel 连接到 HDInsight](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)。
* [使用 Power Query 将 Excel 连接到 Apache Hadoop](../hadoop/apache-hadoop-connect-excel-power-query.md)。
* [使用针对 Visual Studio 的 Data Lake 工具连接到 Azure HDInsight 并运行 Apache Hive 查询](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)。
* [使用 Azure HDInsight 工具进行可视化工作室代码](../hdinsight-for-vscode.md)。
* [将数据上传到 HDInsight](./../hdinsight-upload-data.md)。
