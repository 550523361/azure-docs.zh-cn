---
title: Azure Cosmos DB 查询语言中的 REPLACE
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 REPLACE。
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 758ac13530752df481d27e7e253f025f5c8d6430
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302196"
---
# <a name="replace-azure-cosmos-db"></a>REPLACE (Azure Cosmos DB)
 用另一个字符串值替换出现的所有指定字符串值。  
  
## <a name="syntax"></a>语法
  
```sql
REPLACE(<str_expr1>, <str_expr2>, <str_expr3>)  
```  
  
## <a name="arguments"></a>自变量
  
*str_expr1*  
   是要搜索的字符串表达式。  
  
*str_expr2*  
   要找到的字符串表达式。  
  
*str_expr3*  
   是用于替换 *str_expr1* 中的 *str_expr2* 匹配项的字符串表达式。  
  
## <a name="return-types"></a>返回类型
  
  返回字符串表达式。  
  
## <a name="examples"></a>示例
  
  以下示例演示如何在查询中使用 `REPLACE`。  
  
```sql
SELECT REPLACE("This is a Test", "Test", "desk") AS replace
```  
  
 下面是结果集：  
  
```json
[{"replace": "This is a desk"}]  
```  

## <a name="remarks"></a>备注

此系统功能不会利用索引。

## <a name="next-steps"></a>后续步骤

- [字符串函数 Azure Cosmos DB](sql-query-string-functions.md)
- [系统功能 Azure 宇宙 DB](sql-query-system-functions.md)
- [Azure 宇宙 DB 简介](introduction.md)
