---
title: 使用 Hive ODBC 驱动程序将 Excel 连接到 Apache Hadoop - Azure HDInsight
description: 了解如何设置和使用针对 Excel 的 Microsoft Hive ODBC 驱动程序来从 Microsoft Excel 查询 HDInsight 群集中的数据。
keywords: hadoop excel,hive excel,hive odbc
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 963e34ae9327b7b124d47f4223de8d6ab2082fbf
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2018
ms.locfileid: "53437004"
---
# <a name="connect-excel-to-apache-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>使用 Microsoft Hive ODBC 驱动程序将 Excel 连接到 Azure HDInsight 中的 Apache Hadoop

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoft 的大数据解决方案将 Microsoft 商业智能 (BI) 组件与已由 Azure HDInsight 部署的 Apache Hadoop 群集进行了集成。 此集成的一个例子是，能够使用 Microsoft Hive 开放式数据库连接 (ODBC) 驱动程序将 Excel 连接到 HDInsight 中的 Hadoop 群集的 Hive 数据仓库。

还可以使用用于 Excel 的 Microsoft Power Query 外接程序从 Excel 连接与 HDInsight 群集和其他数据源（包括其他非 HDInsight Hadoop 群集）关联的数据。 有关安装和使用 Power Query 的信息，请参阅[利用 Power Query 将 Excel 连接到 HDInsight][hdinsight-power-query]。



**先决条件**：

在开始阅读本文前，必须具有以下项目：

* **一个 HDInsight 群集**。 要创建此群集，请参阅 [Azure HDInsight 入门](apache-hadoop-linux-tutorial-get-started.md)。
* 装有 Office 2013 Professional Plus、Office 365 Pro Plus、Excel 2013 Standalone 或 Office 2010 Professional Plus 的**工作站**。

## <a name="install-microsoft-hive-odbc-driver"></a>安装 Microsoft Hive ODBC 驱动程序
从[下载中心][hive-odbc-driver-download]下载并安装 Microsoft Hive ODBC 驱动程序。

此驱动程序可以安装在 32 位或 64 位版本的 Windows 7、Windows 8、Windows 10、Windows Server 2008 R2 和 Windows Server 2012 上。 此驱动程序允许连接到 Azure HDInsight。 应安装与你在其中使用 ODBC 驱动程序的应用程序版本匹配的版本。 在本教程中，将通过 Office Excel 使用此驱动程序。

## <a name="create-apache-hive-odbc-data-source"></a>创建 Apache Hive ODBC 数据源
下列步骤演示如何创建 Hive ODBC 数据源。

1. 在 Windows 8 或 Windows 10 中，按 Windows 键打开“开始”屏幕，并键入“数据源”。
2. 单击“设置 ODBC 数据源(32 位)”或“设置 ODBC 数据源(64 位)”，具体取决于 Office 版本。 如果使用的是 Windows 7，请从“管理工具”选择“ODBC 数据源(32 位)”或“ODBC 数据源(64 位)”。 这会显示“ODBC 数据源管理器”对话框。
   
    ![ODBC 数据源管理器](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "使用ODBC 数据源管理器配置 DSN")

3. 从“用户 DNS”单击“添加”打开“新建数据源”向导。
4. 选择“Microsoft Hive ODBC 驱动程序”，并单击“完成”。 应当会看到“Microsoft Hive ODBC 驱动程序 DNS 设置”对话框。
5. 键入或选择以下值：
   
   | 属性 | Description |
   | --- | --- |
   |  数据源名称 |为数据源提供名称 |
   |  主机 |输入 &lt;HDInsightClusterName>.azurehdinsight.net。 例如，myHDICluster.azurehdinsight.net |
   |  端口 |使用 <strong>443</strong>。 （此端口已从 563 更改为 443。） |
   |  数据库 |使用“默认”。 |
   |  机制 |选择“Azure HDInsight 服务” |
   |  用户名 |输入 HDInsight 群集 HTTP 用户的用户名。 默认的用户名为 <strong>admin</strong>。 |
   |  密码 |输入 HDInsight 群集用户的密码。 |
   
    </table>
   
    在单击“高级选项”时，需要注意某些重要参数：
   
   | 参数 | Description |
   | --- | --- |
   |  使用本机查询 |选择此项时，ODBC 驱动程序不会尝试将 TSQL 转换为 HiveQL。 仅 100% 确定提交的是纯 HiveQL 语句时，才应使用此项。 连接 SQL Server 或 Azure SQL 数据库时，应将此项保留为未选中状态。 |
   |  每块提取的行数 |提取大量记录时，可能需要调整此参数以确保最佳性能。 |
   |  默认字符串列长度、二进制列长度、十进制列小数位数 |数据类型长度和精度可能会影响返回数据的方式。 由于精度损失和/或截断，可能会返回不正确的信息。 |

    ![高级选项](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "高级 DSN 配置选项")

1. 单击“测试”以测试数据源。 正确配置数据源时，会显示“测试成功完成!”。
2. 单击“确定”关闭“测试”对话框。 此时新的数据源将在“ODBC 数据源管理器”中列出。
3. 单击“确定”退出向导。

## <a name="import-data-into-excel-from-hdinsight"></a>将数据从 HDInsight 导入 Excel
下列步骤介绍如何使用在前面部分中创建的 ODBC 数据源将数据从 Hive 表导入到 Excel 工作簿。

1. 在 Excel 中打开新工作簿或现有工作簿。
2. 在“数据”选项卡中，依次单击“获取数据”、“从其他源”、“从 ODBC”以启动“数据连接向导”。
   
    ![打开数据连接向导](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "打开数据连接向导")
4. 选择在上一部分中创建的数据源名称，然后单击“确定”。
5. 输入 Hadoop 用户名（默认名称为 admin）和密码，然后单击“连接”。
6. 在导航器中，依次展开“HIVE”、“默认”，依次单击“hivesampletable”、“加载”。 需要几秒钟时间才能将数据导入到 Excel 中。

    ![HDInsight Hive ODBC 导航器](./media/apache-hadoop-connect-excel-hive-odbc-driver/hdinsight.hive.odbc.navigator.png "打开数据连接向导")


## <a name="next-steps"></a>后续步骤
在本文中，已了解了如何使用 Microsoft Hive ODBC 驱动程序将来自 HDInsight 服务的数据检索到 Excel 中。 同样地，也可以将来自 HDInsight 服务的数据检索到 SQL 数据库中。 也可以将数据上传到 HDInsight 服务中。 若要了解更多信息，请参阅以下文章：

* [在 Azure HDInsight 中使用 Microsoft Power BI 直观显示 Apache Hive 数据](apache-hadoop-connect-hive-power-bi.md)。
* [在 Azure HDInsight 中使用 Power BI 直观显示交互式查询 Hive 数据](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)。
* [在 Azure HDInsight 中使用 Apache Zeppelin 运行 Apache Hive 查询](./../hdinsight-connect-hive-zeppelin.md)。
* [使用 Power Query 将 Excel 连接到 Apache Hadoop](apache-hadoop-connect-excel-power-query.md)。
* [使用针对 Visual Studio 的 Data Lake 工具连接到 Azure HDInsight 并运行 Apache Hive 查询](apache-hadoop-visual-studio-tools-get-started.md)。
* [使用用于 Visual Studio Code 的 Azure HDInsight 工具](../hdinsight-for-vscode.md)。
* [将数据上传到 HDInsight](./../hdinsight-upload-data.md)。

[hdinsight-use-sqoop]:hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]:hdinsight-use-hive.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-power-query]: ../hdinsight-connect-excel-power-query.md
[hive-odbc-driver-download]: https://go.microsoft.com/fwlink/?LinkID=286698


