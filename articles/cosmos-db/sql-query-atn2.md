---
title: Azure Cosmos DB 查询语言中的 ATN2
description: 了解 Azure Cosmos DB 中的 ATN2 SQL 系统函数如何返回 y/x 的反正切的主体值（以弧度表示）。
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 696e14e75998ead04c99fab2b84fc4c742d5f54a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302655"
---
# <a name="atn2-azure-cosmos-db"></a>ATN2 (Azure Cosmos DB)
 返回 y/x 的反正切函数的主值（以弧度表示）。  
  
## <a name="syntax"></a>语法
  
```sql
ATN2(<numeric_expr>, <numeric_expr>)  
```  
  
## <a name="arguments"></a>自变量
  
*numeric_expr*  
   为数值表达式。  
  
## <a name="return-types"></a>返回类型
  
  返回数值表达式。  
  
## <a name="examples"></a>示例
  
  以下示例可计算指定的 x 分量和 y 分量的 ATN2 值。  
  
```sql
SELECT ATN2(35.175643, 129.44) AS atn2  
```  
  
 下面是结果集：  
  
```json
[{"atn2": 1.3054517947300646}]  
```  

## <a name="remarks"></a>备注

此系统功能不会利用索引。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure 宇宙 DB](sql-query-mathematical-functions.md)
- [系统功能 Azure 宇宙 DB](sql-query-system-functions.md)
- [Azure 宇宙 DB 简介](introduction.md)
