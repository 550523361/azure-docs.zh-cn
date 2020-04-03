---
title: 教程从 Azure 数据湖存储加载数据
description: 使用 PolyBase 外部表从 Azure 数据湖存储加载数据以进行 Synapse SQL。
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/04/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 7d599ce121b4c53662b91e5aab94130b0f3f4458
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2020
ms.locfileid: "80583943"
---
# <a name="load-data-from-azure-data-lake-storage-for-synapse-sql"></a>加载 Azure 数据湖存储中的数据，用于 Synapse SQL

本指南概述了如何使用 PolyBase 外部表从 Azure 数据湖存储加载数据。 尽管您可以对数据存储在数据湖存储中的数据运行临时查询，但我们建议导入数据以获得最佳性能。 

> [!NOTE]  
> 加载的替代是当前在公共预览中的[COPY 语句](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest)。  COPY 语句提供了最大的灵活性。 要提供有关 COPY 语句的反馈，请发送电子邮件至以下通讯组列表： sqldwcopypreview@service.microsoft.com。
>
> [!div class="checklist"]

> * 创建从数据湖存储加载所需的数据库对象。
> * 连接到数据湖存储目录。
> * 将数据加载到数据仓库中。

如果还没有 Azure 订阅，可以在开始前[创建一个免费帐户](https://azure.microsoft.com/free/)。

## <a name="before-you-begin"></a>在开始之前
开始本教程之前，请下载并安装最新版 [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS)。

若要运行本教程，需要：

* SQL 池。 请参阅[创建 SQL 池和查询数据](create-data-warehouse-portal.md)。
* 数据湖存储帐户。 请参阅[开始使用 Azure 数据存储](../../data-lake-store/data-lake-store-get-started-portal.md)。 对于此存储帐户，您需要配置或指定要加载的以下凭据之一：存储帐户密钥、Azure 目录应用程序用户或具有相应 RBAC 角色的 AAD 用户。 

##  <a name="create-a-credential"></a>创建凭据
使用 AAD 直通验证时，可以跳过此部分并继续"创建外部数据源"。 使用 AAD 传递时不需要创建或指定数据库作用域凭据，但请确保 AAD 用户具有存储帐户的相应 RBAC 角色（存储 Blob 数据读取器、参与者或所有者角色）。 更多细节[将在这里](https://techcommunity.microsoft.com/t5/Azure-SQL-Data-Warehouse/How-to-use-PolyBase-by-authenticating-via-AAD-pass-through/ba-p/862260)概述。 

要访问数据湖存储帐户，您需要创建数据库主密钥来加密凭据密钥。 然后创建数据库作用域凭据来存储机密。 使用服务主体（Azure 目录应用程序用户）进行身份验证时，数据库作用域凭据将存储在 AAD 中设置的服务主体凭据。 您还可以使用数据库作用域凭据存储 Gen2 的存储帐户密钥。

要使用服务主体连接到数据湖存储，必须**首先**创建 Azure 活动目录应用程序、创建访问密钥以及授予应用程序对数据湖存储帐户的访问权限。 有关说明，请参阅[使用活动目录对 Azure 数据湖存储进行身份验证](../../data-lake-store/data-lake-store-service-to-service-authenticate-using-active-directory.md)。

使用具有 CONTROL 级别权限的用户登录到 SQL 池，并针对数据库执行以下 SQL 语句：

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://msdn.microsoft.com/library/ms174382.aspx?f=255&MSPPError=-2147217396

CREATE MASTER KEY;


-- B (for service principal authentication): Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://msdn.microsoft.com/library/mt270260.aspx

CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    -- Always use the OAuth 2.0 authorization endpoint (v1)
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- B (for Gen2 storage key authentication): Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.

CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;

-- It should look something like this when authenticating using service principal:
CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```

## <a name="create-the-external-data-source"></a>创建外部数据源
使用此 [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql) 命令存储数据的位置。 如果使用 AAD 直通进行身份验证，则不需要"凭据"参数。 

```sql
-- C (for Gen1): Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen1 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<datalakestoregen1accountname>.azuredatalakestore.net',
    CREDENTIAL = ADLSCredential
);

-- C (for Gen2): Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen2 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStorage
WITH (
    TYPE = HADOOP,
    LOCATION='abfs[s]://<container>@<AzureDataLake account_name>.dfs.core.windows.net', -- Please note the abfss endpoint for when your account has secure transfer enabled
    CREDENTIAL = ADLSCredential
);
```

## <a name="configure-data-format"></a>配置数据格式
要从数据存储库导入数据，需要指定外部文件格式。 此对象定义文件在数据湖存储中写入的方式。
有关完整列表，请查看我们的 T-SQL 文档 [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)（创建外部文件格式）

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks the end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies the field terminator for data of type string in the text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store missing values as default for datatype.

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE
                    )
);
```

## <a name="create-the-external-tables"></a>创建外部表
指定数据源和文件格式后，可以开始创建外部表。 外部表是你与外部数据交互的方式。 位置参数可以指定文件或目录。 如果指定目录，将会加载该目录中的所有文件。

```sql
-- D: Create an External Table
-- LOCATION: Folder under the Data Lake Storage root folder.
-- DATA_SOURCE: Specifies which Data Source Object to use.
-- FILE_FORMAT: Specifies which File Format Object to use
-- REJECT_TYPE: Specifies how you want to deal with rejected rows. Either Value or percentage of the total
-- REJECT_VALUE: Sets the Reject value based on the reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStorage
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>外部表注意事项
创建外部表很容易，但有一些细微差别，需要进行讨论。

外部表经过强类型化。 这意味着所引入的每行数据必须满足表架构定义。
如果某个行不符合架构定义，则该行会被拒绝加载。

使用 REJECT_TYPE 和 REJECT_VALUE 可以定义在最终表中必须存在的数据行数或数据的百分比。 在加载过程中，如果达到拒绝值，则加载会失败。 行被拒绝的最常见原因是架构定义不匹配。 例如，当文件中的数据是字符串时，如果错误地为列指定了 int 的架构，则每一行都将无法加载。

Data Lake Storage Gen1 使用基于角色的访问控制 (RBAC) 控制对数据的访问。 也就是说，服务主体必须拥有对位置参数中定义的目录以及最终目录和文件的子项的读取权限。 这样一来，PolyBase 就可以进行身份验证，并加载该数据。 

## <a name="load-the-data"></a>加载数据
要从数据存储加载数据，请使用["创建表作为选择"（转算-SQL）](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)语句。 

CTAS 将创建新表，并在该表中填充 select 语句的结果。 CTAS 将新表定义为包含与 select 语句结果相同的列和数据类型。 如果选择了外部表中的所有列，则新表将是外部表中的列和数据类型的副本。

在此示例中，我们将从外部表 DimProduct_external 创建名为 DimProduct 的哈希分布表。

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a>优化列存储压缩
默认情况下，表定义为群集列存储索引。 加载完成后，某些数据行可能未压缩到列存储中。  发生这种情况的原因多种多样。 若要了解详细信息，请参阅[管理列存储索引](sql-data-warehouse-tables-index.md)。

若要在加载后优化查询性能和列存储压缩，请重新生成表，以强制列存储索引压缩所有行。

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

## <a name="optimize-statistics"></a>优化统计信息
最好是在加载之后马上创建单列统计信息。 对于统计信息，可以使用多个选项。 例如，如果针对每个列创建单列统计信息，则重新生成所有统计信息可能需要花费很长时间。 如果知道某些列不会在查询谓词中使用，可以不创建有关这些列的统计信息。

如果决定针对每个表的每个列创建单列统计信息，可以使用 [统计信息](sql-data-warehouse-tables-statistics.md)一文中的存储过程代码示例 `prc_sqldw_create_stats`。

以下示例是创建统计信息的不错起点。 它会针对维度表中的每个列以及事实表中的每个联接列创建单列统计信息。 以后，随时可以将单列或多列统计信息添加到其他事实表列。

## <a name="achievement-unlocked"></a>大功告成！
您已成功将数据加载到数据仓库中。 干得不错！

## <a name="next-steps"></a>后续步骤 
在本教程中，创建了外部表以定义 Data Lake Storage Gen1 中存储的数据的结构，然后使用了 PolyBase CREATE TABLE AS SELECT 语句将数据加载到数据仓库。 

完成了以下操作：
> [!div class="checklist"]
>
> * 创建从数据湖存储加载所需的数据库对象。
> * 连接到数据湖存储目录。
> * 将数据加载到数据仓库中。
>

加载数据是使用 Azure 同步分析开发数据仓库解决方案的第一步。 请查看我们的开发资源。

> [!div class="nextstepaction"]
> [了解如何为数据仓库开发表](sql-data-warehouse-tables-overview.md)
