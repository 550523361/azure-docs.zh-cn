---
title: 使用 Azure 数据工厂向/从 Oracle 复制数据 | Microsoft Docs
description: 了解如何使用数据工厂将数据从支持的源存储复制到 Oracle 数据库，或从 Oracle 复制到支持的接收器存储。
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: jingwang
ms.openlocfilehash: 1e561a59ebe503e0088362087dbda4d7d89fee4c
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275680"
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>使用 Azure 数据工厂从/向 Oracle 复制数据
> [!div class="op_single_selector" title1="选择您正在使用的 Data Factory 服务的版本:"]
> * [版本 1](v1/data-factory-onprem-oracle-connector.md)
> * [当前版本](connector-oracle.md)

本文概述了如何在 Azure 数据工厂中使用复制活动从/向 Oracle 数据库复制数据。 本文基于总体概述复制活动的[复制活动概述](copy-activity-overview.md)一文。

## <a name="supported-capabilities"></a>支持的功能

可以将数据从 Oracle 数据库复制到任何支持的接收器数据存储。 还可以将数据从任何支持的源数据存储复制到 Oracle 数据库。 有关复制活动支持作为源或接收器的数据存储列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)表。

具体而言，此 Oracle 连接器支持以下版本的 Oracle 数据库。 它还支持基本或 OID 身份验证：

- Oracle 12c R1 (12.1)
- Oracle 11g R1, R2 (11.1, 11.2)
- Oracle 10g R1, R2 (10.1, 10.2)
- Oracle 9i R1, R2 (9.0.1, 9.2)
- Oracle 8i R3 (8.1.7)

> [!Note]
> 不支持 Oracle 代理服务器。

## <a name="prerequisites"></a>先决条件

若要从/向不可公开访问的 Oracle 数据库复制数据，需要设置自承载集成运行时。 有关集成运行时的详细信息，请参阅[自承载集成运行时](create-self-hosted-integration-runtime.md)。 集成运行时提供内置的 Oracle 驱动程序。 因此，在从/向 Oracle 复制数据时不需要手动安装驱动程序。

## <a name="get-started"></a>入门

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

对于特定于 Oracle 连接器的数据工厂实体，以下部分提供有关用于定义这些实体的属性的详细信息。

## <a name="linked-service-properties"></a>链接服务属性

Oracle 链接服务支持以下属性。

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | type 属性必须设置为 **Oracle**。 | 是 |
| connectionString | 指定连接到 Oracle 数据库实例所需的信息。 将此字段标记为 SecureString 以安全地将其存储在数据工厂中或[引用存储在 Azure Key Vault 中的机密](store-credentials-in-key-vault.md)。<br><br>**支持的连接类型**：可以使用 **Oracle SID** 或 **Oracle 服务名称**来标识数据库：<br>- 如果使用 SID：`Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>- 如果使用服务名称：`Host=<host>;Port=<port>;ServiceName=<servicename>;User Id=<username>;Password=<password>;` | 是 |
| connectVia | 用于连接到数据存储的[集成运行时](concepts-integration-runtime.md)。 如果可以公开访问数据存储，则可以使用自承载集成运行时或 Azure Integration Runtime 时。 如果未指定，则使用默认 Azure Integration Runtime。 |否 |

>[!TIP]
>如果遇到错误消息指出“ORA-01025: UPI 参数超出范围”，且 Oracle 版本为 8i，请将 `WireProtocolMode=1` 添加到连接字符串并重试。

若要在 Oracle 连接上启用加密，你有两种选择：

1.  若要使用三重 DES 加密 (3DES) 和高级加密标准 (AES)，在 Oracle 服务器端，转到“Oracle 高级安全性 (OAS)”并配置加密设置，请参阅[此处](https://docs.oracle.com/cd/E11882_01/network.112/e40393/asointro.htm#i1008759)的详细信息。 ADF Oracle 连接器会自动协商加密方法，以便在建立与 Oracle 的连接时使用在 OAS 中配置的加密方法。

2.  若要使用 SSL，请执行以下步骤：

    1.  获取 SSL 证书信息。 获取 SSL 证书的 DER 编码证书信息，并将输出 (----- Begin Certificate … End Certificate -----) 另存为文本文件。

        ```
        openssl x509 -inform DER -in [Full Path to the DER Certificate including the name of the DER Certificate] -text
        ```

        示例：从 DERcert.cer 提取证书信息；然后，将输出保存到 cert.txt

        ```
        openssl x509 -inform DER -in DERcert.cer -text
        Output:
        -----BEGIN CERTIFICATE-----
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXX
        -----END CERTIFICATE-----
        ```
    
    2.  生成密钥存储或信任存储。 以下命令使用或不使用 PKCS-12 格式的密码来创建信任存储文件。

        ```
        openssl pkcs12 -in [Path to the file created in the previous step] -out [Path and name of TrustStore] -passout pass:[Keystore PWD] -nokeys -export
        ```

        示例：使用密码创建名为 MyTrustStoreFile 的 PKCS12 信任存储文件

        ```
        openssl pkcs12 -in cert.txt -out MyTrustStoreFile -passout pass:ThePWD -nokeys -export  
        ```

    3.  将信任存储文件放于自承载 IR 计算机上，例如，C:\MyTrustStoreFile。
    4.  在 ADF 中，使用 `EncryptionMethod=1` 和相应的 `TrustStore`/`TrustStorePassword` 值配置 Oracle 连接字符串，例如 `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;EncryptionMethod=1;TrustStore=C:\\MyTrustStoreFile;TrustStorePassword=<trust_store_password>`。

**示例：**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>数据集属性

有关可用于定义数据集的各部分和属性的完整列表，请参阅[数据集](concepts-datasets-linked-services.md)一文。 本部分提供 Oracle 数据集支持的属性列表。

若要从/向 OData 复制数据，请将数据集的 type 属性设置为“OracleTable”。 支持以下属性。

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 数据集的 type 属性必须设置为 OracleTable。 | 是 |
| tableName |链接服务引用的 Oracle 数据库中表的名称。 | 是 |

**示例：**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>复制活动属性

有关可用于定义活动的各部分和属性的完整列表，请参阅[管道](concepts-pipelines-activities.md)一文。 本部分提供 Oracle 源和接收器支持的属性列表。

### <a name="oracle-as-a-source-type"></a>以 Oracle 作为源类型

要从 Oracle 复制数据，请将复制活动中的源类型设置为“OracleSource”。 复制活动的 **source** 节支持以下属性。

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 复制活动源的 type 属性必须设置为 OracleSource。 | 是 |
| oracleReaderQuery | 使用自定义 SQL 查询读取数据。 例如 `"SELECT * FROM MyTable"`。 | 否 |

如果未指定“oracleReaderQuery”，则将使用在数据集的“structure”节中定义的列，构建针对 Oracle 数据库运行的查询 (`select column1, column2 from mytable`)。 如果数据集定义没有“structure”，则将从表中选择所有列。

**示例：**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="oracle-as-a-sink-type"></a>以 Oracle 作为接收器类型

要向 Oracle 复制数据，请将复制活动中的接收器类型设置为“OracleSink”。 复制活动 **sink** 节支持以下属性。

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 复制活动接收器的 type 属性必须设置为 **OracleSink**。 | 是 |
| writeBatchSize | 缓冲区大小达到 writeBatchSize 时会数据插入 SQL 表。<br/>允许的值为 Integer（行数）。 |否（默认值为 10,000） |
| writeBatchTimeout | 超时之前等待批插入操作完成时的等待时间。<br/>允许的值为 Timespan。 示例为 00:30:00（30 分钟）。 | 否 |
| preCopyScript | 指定每次运行时，复制活动将数据写入到 Oracle 之前要执行的 SQL 查询。 可以使用此属性清除预加载的数据。 | 否 |

**示例：**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-oracle"></a>Oracle 的数据类型映射

从/向 Oracle 复制数据时，以下映射用于从 Oracle 数据类型映射到数据工厂临时数据类型。 若要了解复制活动如何将源架构和数据类型映射到接收器，请参阅[架构和数据类型映射](copy-activity-schema-and-type-mapping.md)。

| Oracle 数据类型 | 数据工厂临时数据类型 |
|:--- |:--- |
| BFILE |Byte[] |
| BLOB |Byte[]<br/>（仅支持 Oracle 10g 和更高版本） |
| CHAR |String |
| CLOB |String |
| DATE |DateTime |
| FLOAT |十进制、字符串（如果精度 > 28） |
| INTEGER |十进制、字符串（如果精度 > 28） |
| LONG |String |
| LONG RAW |Byte[] |
| NCHAR |String |
| NCLOB |String |
| NUMBER |十进制、字符串（如果精度 > 28） |
| NVARCHAR2 |String |
| RAW |Byte[] |
| ROWID |String |
| TIMESTAMP |DateTime |
| TIMESTAMP WITH LOCAL TIME ZONE |String |
| TIMESTAMP WITH TIME ZONE |String |
| UNSIGNED INTEGER |Number |
| VARCHAR2 |String |
| XML |String |

> [!NOTE]
> 不支持数据类型 INTERVAL YEAR TO MONTH 和 INTERVAL DAY TO SECOND。


## <a name="next-steps"></a>后续步骤
有关数据工厂中复制活动支持作为源和接收器的数据存储的列表，请参阅[支持的数据存储](copy-activity-overview.md##supported-data-stores-and-formats)。
