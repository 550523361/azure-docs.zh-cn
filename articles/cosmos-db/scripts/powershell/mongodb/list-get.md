---
title: 用于列出和获取 Azure Cosmos DB 的 API for MongoDB 中操作的 PowerShell 脚本
description: Azure PowerShell 脚本 - Azure Cosmos DB 列出和获取操作 - MongoDB API
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 05/01/2020
ms.author: mjbrown
ms.openlocfilehash: d3b62a050789cb54791be3f33827e6e7a02810f5
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93074601"
---
# <a name="list-and-get-databases-and-graphs-for-azure-cosmos-db---mongodb-api"></a>列出和获取 Azure Cosmos DB 的数据库和图 - MongoDB API
[!INCLUDE[appliesto-mongodb-api](../../../includes/appliesto-mongodb-api.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>示例脚本

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-list-get.ps1 "List or get databases or collections for MongoDB API")]

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组以及与其关联的所有资源。

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
|**Azure Cosmos DB**| |
| [Get-AzCosmosDBAccount](/powershell/module/az.cosmosdb/get-azcosmosdbaccount) | 列出 Cosmos DB 帐户或获取指定的 Cosmos DB 帐户。 |
| [Get-AzCosmosDBMongoDBDatabase](/powershell/module/az.cosmosdb/get-azcosmosdbmongodbdatabase) | 列出帐户中的 MongoDB API 数据库，或在帐户中获取指定的 MogoDB API 数据库。 |
| [Get-AzCosmosDBMongoDBCollection](/powershell/module/az.cosmosdb/get-azcosmosdbmongodbcollection) | 列出 MongoDB API 集合，或在数据库中获取指定的 MongoDB API 集合 |
|**Azure 资源组**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](/powershell/)。