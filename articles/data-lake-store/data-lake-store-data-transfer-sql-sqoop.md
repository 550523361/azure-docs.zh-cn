---
title: 使用 Sqoop 在 Azure Data Lake Storage Gen1 和 Azure SQL 数据库之间复制数据 | Microsoft Docs
description: 使用 Sqoop 在 Azure SQL 数据库和 Azure Data Lake Storage Gen1 之间复制数据
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 3f914b2a-83cc-4950-b3f7-69c921851683
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 7d3283b03d15278d1f7fd42a72b154dab1a442b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "60878760"
---
# <a name="copy-data-between-azure-data-lake-storage-gen1-and-azure-sql-database-using-sqoop"></a>使用 Sqoop 在 Azure Data Lake Storage Gen1 和 Azure SQL 数据库之间复制数据
了解如何使用 Apache Sqoop 在 Azure SQL 数据库和 Azure Data Lake Storage Gen1 之间导入和导出数据。

## <a name="what-is-sqoop"></a>什么是 Sqoop？
大数据应用程序非常适合用于处理未结构化和半结构化数据，例如日志和文件。 但也可能需要处理关系数据库中存储的结构化数据。

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) 是一种用于在关系数据库和大数据存储库（例如 Data Lake Storage Gen1）之间传输数据的工具。 可使用该工具将数据从关系数据库管理系统 (RDBMS)（例如 Azure SQL 数据库）中导入到 Data Lake Storage Gen1。 之后，可使用大数据工作负荷转换和分析数据，然后将此数据返回导出到 RDBMS。 在本教程中，将 Azure SQL 数据库用作要向（从）其导入/导出的关系数据库。

## <a name="prerequisites"></a>必备组件
在开始阅读本文前，必须具有：

* **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。
* **Azure Data Lake Storage Gen1 帐户**。 有关如何创建帐户的说明，请参阅 [Azure Data Lake Storage Gen1 入门](data-lake-store-get-started-portal.md)
* 具有 Data Lake Storage Gen1 帐户访问权限的 Azure HDInsight 群集  。 请参阅[创建包含 Data Lake Storage Gen1 的 HDInsight 群集](data-lake-store-hdinsight-hadoop-use-portal.md)。 本文假定用户的群集是具有 Data Lake Storage Gen1 访问权限的 HDInsight Linux 群集。
* **Azure SQL 数据库**。 有关如何创建 Azure SQL 数据库的说明，请参阅[创建 Azure SQL 数据库](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>通过视频学得更快？
[观看此视频](https://mix.office.com/watch/1butcdjxmu114)，了解如何使用 DistCp 在 Azure 存储 Blob 和 Data Lake Storage Gen1 之间复制数据。

## <a name="create-sample-tables-in-the-azure-sql-database"></a>在 Azure SQL 数据库中创建示例表
1. 首先，在 Azure SQL 数据库中创建两个示例表。 使用 [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) 或 Visual Studio 连接到 Azure SQL 数据库，并运行以下查询。

    **创建表 1**

        CREATE TABLE [dbo].[Table1](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_1] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO

    **创建表 2**

        CREATE TABLE [dbo].[Table2](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_2] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO
2. 在**表 1** 中，添加一些示例数据。 **表 2** 留空。 从**表 1** 将数据导入到 Data Lake Storage Gen1。 然后，从 Data Lake Storage Gen1 将数据导出到**表 2**。 运行以下代码段。

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-storage-gen1"></a>使用具有 Data Lake Storage Gen1 帐户访问权限的 Azure HDInsight 群集中的 Sqoop
HDInsight 群集已经具有可用的 Sqoop 包。 如果已经配置 HDInsight 群集，将 Data Lake Storage Gen1 用作额外的存储，可使用 Sqoop（不作任何更改）在关系数据库（本例中是 Azure SQL 数据库）和 Data Lake Storage Gen1 帐户之间导入/导出数据。

1. 本教程假设已经创建 Linux 群集，因此，应使用 SSH 连接到该群集。 请参阅[连接到基于 Linux 的 HDInsight 群集](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)。
2. 验证是否可从此群集访问 Data Lake Storage Gen1 帐户。 从 SSH 提示符处运行以下命令：

        hdfs dfs -ls adl://<data_lake_storage_gen1_account>.azuredatalakestore.net/

    这样会提供 Data Lake Storage Gen1 帐户中文件/文件夹的列表。

### <a name="import-data-from-azure-sql-database-into-data-lake-storage-gen1"></a>从 Azure SQL 数据库将数据导入到 Data Lake Storage Gen1
1. 导航至 Sqoop 包可用的目录。 通常位于 `/usr/hdp/<version>/sqoop/bin`。
2. 将数据从**表 1** 导入到 Data Lake Storage Gen1 帐户。 使用以下语法：

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-storage-gen1-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    请注意 **sql-database-server-name** 占位符表示运行 Azure SQL 数据库的服务器的名称。 **sql-database-name** 占位符表示实际的数据库名称。

    例如，


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=twooley@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. 验证数据是否已经传输到 Data Lake Storage Gen1 帐户。 运行以下命令：

        hdfs dfs -ls adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    应会看到以下输出：


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    每个 part-m-* 文件对应源表（表 1）中的一行   。 可查看 part-m-* 文件的内容进行验证。


### <a name="export-data-from-data-lake-storage-gen1-into-azure-sql-database"></a>从 Data Lake Storage Gen1 将数据导出到 Azure SQL 数据库
1. 将数据从 Data Lake Storage Gen1 帐户导出到 Azure SQL 数据库中的空表（表 2  ）。 使用以下语法。

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-storage-gen1-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    例如，


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=twooley@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. 验证数据是否已经上传到 SQL 数据库表。 使用 [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) 或 Visual Studio 连接到 Azure SQL 数据库，并运行以下查询。

        SELECT * FROM TABLE2

    应会显示以下输出。

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a>使用 Sqoop 时的性能注意事项

若要了解为将数据复制到 Data Lake Storage Gen1 而对 Sqoop 作业进行的性能优化，请参阅 [Sqoop 性能文档](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)。

## <a name="see-also"></a>另请参阅
* [将数据从 Azure 存储 Blob 复制到 Data Lake Storage Gen1](data-lake-store-copy-data-azure-storage-blob.md)
* [保护 Data Lake Storage Gen1 中的数据](data-lake-store-secure-data.md)
* [配合使用 Azure Data Lake Analytics 和 Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [将 Azure HDInsight 与 Data Lake Storage Gen1 配合使用](data-lake-store-hdinsight-hadoop-use-portal.md)
