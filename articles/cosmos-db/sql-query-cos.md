---
title: Azure Cosmos DB 查询语言中的 COS
description: 了解 Azure Cosmos DB 中的 Cosine (COS) SQL 系统函数如何在指定表达式中返回指定角度（以弧度为单位）的三角余弦
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 40d277ff38691e2cb74bd4d5d78a666c304acfcd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78304015"
---
# <a name="cos-azure-cosmos-db"></a>COS (Azure Cosmos DB)
 返回指定表达式中指定角度的三角余弦（弧度）。  
  
## <a name="syntax"></a>语法
  
```sql
COS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>自变量
  
*numeric_expr*  
   为数值表达式。  
  
## <a name="return-types"></a>返回类型
  
  返回数值表达式。  
  
## <a name="examples"></a>示例
  
  以下示例计算指定角度的 `COS`。  
  
```sql
SELECT COS(14.78) AS cos  
```  
  
 下面是结果集：  
  
```json
[{"cos": -0.59946542619465426}]  
```  

## <a name="remarks"></a>备注

此系统功能不会利用索引。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure 宇宙 DB](sql-query-mathematical-functions.md)
- [系统功能 Azure 宇宙 DB](sql-query-system-functions.md)
- [Azure 宇宙 DB 简介](introduction.md)
