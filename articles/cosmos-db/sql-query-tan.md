---
title: Azure Cosmos DB 查询语言中的 TAN
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 TAN。
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9d7187ba116067445e835769fc33aa70677ef80b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78301975"
---
# <a name="tan-azure-cosmos-db"></a>TAN (Azure Cosmos DB)
 返回指定表达式中指定角度（弧度）的正切值。  
  
## <a name="syntax"></a>语法
  
```sql
TAN (<numeric_expr>)  
```  
  
## <a name="arguments"></a>自变量
  
*numeric_expr*  
   为数值表达式。  
  
## <a name="return-types"></a>返回类型
  
  返回数值表达式。  
  
## <a name="examples"></a>示例
  
  以下示例可计算 PI()/2 的正切值。  
  
```sql
SELECT TAN(PI()/2) AS tan 
```  
  
 下面是结果集：  
  
```json
[{"tan": 16331239353195370 }]  
```  

## <a name="remarks"></a>备注

此系统功能不会利用索引。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure 宇宙 DB](sql-query-mathematical-functions.md)
- [系统功能 Azure 宇宙 DB](sql-query-system-functions.md)
- [Azure 宇宙 DB 简介](introduction.md)
