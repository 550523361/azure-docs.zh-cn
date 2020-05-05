---
title: 如何查询 Azure Cosmos DB 中的表数据？
description: 了解如何使用 OData 筛选器和 LINQ 查询来查询 Azure Cosmos DB 表 API 帐户中存储的数据
author: sakash279
ms.author: akshanka
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 05/21/2019
ms.reviewer: sngun
ms.openlocfilehash: 8f31ace0045dad2f038a1eded52a41ffb1932f99
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "76770489"
---
# <a name="tutorial-query-azure-cosmos-db-by-using-the-table-api"></a>教程：使用表 API 查询 Azure Cosmos DB

Azure Cosmos DB [表 API](table-introduction.md) 支持针对键/值（表）数据进行 OData 和 [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) 查询。  

本文涵盖以下任务： 

> [!div class="checklist"]
> * 使用表 API 查询数据

本文中的查询使用如下示例 `People` 表：

| PartitionKey | RowKey | 电子邮件 | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

有关如何使用表 API 执行查询的详细信息，请参阅[查询表和实体](https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities)。 

有关 Azure Cosmos DB 所提供的高级功能的详细信息，请参阅 [Azure Cosmos DB 表 API](table-introduction.md) 和[使用 .NET 通过表 API 进行开发](tutorial-develop-table-dotnet.md)。 

## <a name="prerequisites"></a>必备条件

若要使使这些查询生效，必须拥有 Azure Cosmos DB 帐户，且容器中必须包含实体数据。 没有这些内容？ 请学习[五分钟快速入门](create-table-dotnet.md)或者[开发人员教程](tutorial-develop-table-dotnet.md)，创建帐户并填充数据库。

## <a name="query-on-partitionkey-and-rowkey"></a>PartitionKey 和 RowKey 的查询
由于 PartitionKey 和 RowKey 属性构成实体的主键，因此可使用特殊语法确定实体，如下所示： 

**查询**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**结果**

| PartitionKey | RowKey | 电子邮件 | PhoneNumber |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0104 |

或者，可将这些属性指定为 `$filter` 选项的一部分，如以下部分中所示。 请注意，键属性名称和常量值区分大小写。 PartitionKey 和 RowKey 属性皆为 String 类型。 

## <a name="query-by-using-an-odata-filter"></a>使用 OData 筛选器查询
构造筛选器字符串时，请记住以下规则： 

* 使用由 OData 协议规范定义的逻辑运算符比较属性和值。 请注意，不能比较属性和动态值。 表达式的一侧必须是常数。 
* 必须使用 URL 编码空格分隔属性名称、运算符和常量值。 空格经 URL 编码后为 `%20`。 
* 筛选器字符串的所有部分都区分大小写。 
* 常量值的数据类型必须与属性的类型相同，这样筛选器才能返回有效的结果。 有关支持的属性类型的详细信息，请参阅 [Understanding the Table Service Data Model](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model)（了解表服务数据模型）。 

以下示例查询演示如何使用 OData `$filter` 按 PartitionKey 和 Email 属性进行筛选。

**查询**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

有关如何对不同数据类型构造筛选表达式的详细信息，请参阅[查询表和实体](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities)。

**结果**

| PartitionKey | RowKey | 电子邮件 | PhoneNumber |
| --- | --- | --- | --- |
| Smith |Ben | Ben@contoso.com| 425-555-0102 |

## <a name="query-by-using-linq"></a>使用 LINQ 查询 
还可使用 LINQ 进行查询，这会转换为相应的 OData 查询表达式。 以下示例演示如何使用 .NET SDK 生成查询：

```csharp
IQueryable<CustomerEntity> linqQuery = table.CreateQuery<CustomerEntity>()
            .Where(x => x.PartitionKey == "4")
            .Select(x => new CustomerEntity() { PartitionKey = x.PartitionKey, RowKey = x.RowKey, Email = x.Email });
```

## <a name="next-steps"></a>后续步骤

在本教程中，已完成以下内容：

> [!div class="checklist"]
> * 已了解如何使用表 API 进行查询

现在可继续学习下一教程，了解如何全局发布数据。

> [!div class="nextstepaction"]
> [全局分发数据](tutorial-global-distribution-table.md)
