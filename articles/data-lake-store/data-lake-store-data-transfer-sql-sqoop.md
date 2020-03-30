---
title: 在数据存储库第 1 代和 Azure SQL 之间复制数据 - Sqoop |微软文档
description: 使用 Sqoop 在 Azure SQL 数据库和 Azure Data Lake Storage Gen1 之间复制数据
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: twooley
ms.openlocfilehash: cf3893706afcb4c4cc5b90dd3d2431ecedc71d0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "73839061"
---
# <a name="copy-data-between-data-lake-storage-gen1-and-azure-sql-database-using-sqoop"></a>使用 Sqoop 在数据存储库第 1 代和 Azure SQL 数据库之间复制数据

了解如何使用 Apache Sqoop 在 Azure SQL 数据库和 Azure Data Lake Storage Gen1 之间导入和导出数据。

## <a name="what-is-sqoop"></a>什么是 Sqoop？

大数据应用程序非常适合用于处理未结构化和半结构化数据，例如日志和文件。 但是，您可能还需要处理存储在关系数据库中的结构化数据。

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) 是一种用于在关系数据库和大数据存储库（例如 Data Lake Storage Gen1）之间传输数据的工具。 可使用该工具将数据从关系数据库管理系统 (RDBMS)（例如 Azure SQL 数据库）中导入到 Data Lake Storage Gen1。 然后，您可以使用大数据工作负载转换和分析数据，然后将数据导出回 RDBMS。 在本文中，您将 Azure SQL 数据库用作关系数据库，以便从中导入/导出。

## <a name="prerequisites"></a>先决条件

在开始之前，必须满足以下条件：

* **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。
* **Azure Data Lake Storage Gen1 帐户**。 有关如何创建帐户的说明，请参阅[使用 Azure 数据存储第 1 代](data-lake-store-get-started-portal.md)
* 具有 Data Lake Storage Gen1 帐户访问权限的 Azure HDInsight 群集****。 请参阅[创建包含 Data Lake Storage Gen1 的 HDInsight 群集](data-lake-store-hdinsight-hadoop-use-portal.md)。 本文假定用户的群集是具有 Data Lake Storage Gen1 访问权限的 HDInsight Linux 群集。
* **Azure SQL 数据库**。 有关如何创建 Azure SQL 数据库的说明，请参阅[创建 Azure SQL 数据库](../sql-database/sql-database-get-started.md)

## <a name="create-sample-tables-in-the-azure-sql-database"></a>在 Azure SQL 数据库中创建示例表

1. 首先，在 Azure SQL 数据库中创建两个示例表。 使用[SQL 服务器管理工作室](../sql-database/sql-database-connect-query-ssms.md)或可视化工作室连接到数据库，然后运行以下查询。

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

1. 运行以下命令以将一些示例数据添加到**表 1**。 **表 2 **留空。 稍后，您将将数据从**表 1**导入到数据湖存储 Gen1 中。 然后，您将将数据从数据存储第 1 代导出到**表 2**中。

       INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-storage-gen1"></a>使用具有 Data Lake Storage Gen1 帐户访问权限的 Azure HDInsight 群集中的 Sqoop

HDInsight 群集已经具有可用的 Sqoop 包。 如果将 HDInsight 群集配置为使用 Data Lake 存储 Gen1 作为附加存储，则可以使用 Sqoop（无需任何配置更改）在关系数据库（如 Azure SQL 数据库）和数据存储库第 1 代帐户之间导入/导出数据。

1. 在本文中，我们假设您创建了一个 Linux 群集，因此您应该使用 SSH 连接到群集。 请参阅[连接到基于 Linux 的 HDInsight 群集](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)。

1. 验证是否可从此群集访问 Data Lake Storage Gen1 帐户。 从 SSH 提示符处运行以下命令：

       hdfs dfs -ls adl://<data_lake_storage_gen1_account>.azuredatalakestore.net/

   此命令提供数据湖存储 Gen1 帐户中的文件/文件夹的列表。

### <a name="import-data-from-azure-sql-database-into-data-lake-storage-gen1"></a>从 Azure SQL 数据库将数据导入到 Data Lake Storage Gen1

1. 导航至 Sqoop 包可用的目录。 通常，此位置为`/usr/hdp/<version>/sqoop/bin`。

1. 将数据从**表 1** 导入到 Data Lake Storage Gen1 帐户。 使用以下语法：

       sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-storage-gen1-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

   **sql-数据库-服务器名称**占位符表示运行 Azure SQL 数据库的服务器的名称。 **sql-database-name** 占位符表示实际的数据库名称。

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

   每个 part-m-* 文件对应源表（表 1）中的一行********。 可查看 part-m-* 文件的内容进行验证。

### <a name="export-data-from-data-lake-storage-gen1-into-azure-sql-database"></a>从 Data Lake Storage Gen1 将数据导出到 Azure SQL 数据库

1. 将数据从 Data Lake Storage Gen1 帐户导出到 Azure SQL 数据库中的空表（表 2****）。 使用以下语法。

       sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-storage-gen1-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

   例如，

       sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=twooley@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. 验证数据是否已经上传到 SQL 数据库表。 使用 [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) 或 Visual Studio 连接到 Azure SQL 数据库，并运行以下查询。

       SELECT * FROM TABLE2

   此命令应具有以下输出。

        ID  FName    LName
       -------------------
       1    Neal     Kell
       2    Lila     Fulton
       3    Erna     Myers
       4    Annette  Simpson

## <a name="performance-considerations-while-using-sqoop"></a>使用 Sqoop 时的性能注意事项

有关性能调整您的 Sqoop 作业将数据复制到数据存储第 1 代，请参阅[Sqoop 性能博客文章](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)。

## <a name="next-steps"></a>后续步骤

* [将数据从 Azure 存储 Blob 复制到 Data Lake Storage Gen1](data-lake-store-copy-data-azure-storage-blob.md)
* [保护 Data Lake Storage Gen1 中的数据](data-lake-store-secure-data.md)
* [将 Azure Data Lake Analytics 与 Data Lake Storage Gen1 配合使用](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [将 Azure HDInsight 与 Data Lake Storage Gen1 配合使用](data-lake-store-hdinsight-hadoop-use-portal.md)
