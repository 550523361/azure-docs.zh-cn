---
title: Azure Cosmos DB Gremlin 与 TinkerPop 功能的兼容性
description: 参考文档 Graph 引擎兼容性问题
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: sngun
ms.openlocfilehash: 581bc813ca27067b1f27ab9866a45df3084dbbcc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75644726"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Azure Cosmos DB Gremlin 兼容性
Azure Cosmos DB Graph 引擎紧密遵循 [Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps) 遍历步骤规范，但是存在差异。

## <a name="behavior-differences"></a>行为差异

* Azure Cosmos DB Graph 引擎运行***广度优先***遍历，而 TinkerPop Gremlin 则深度优先。 这种行为在像 Cosmos DB 这样的水平可缩放系统中可实现更好的性能。 

## <a name="unsupported-features"></a>不支持的功能

* ***[Gremlin 字节码](https://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** 是与编程语言无关的图遍历规范。 Cosmos DB Graph 尚不支持它。 请使用 `GremlinClient.SubmitAsync()` 并以文本字符串的形式传递遍历。

* ***`property(set, 'xyz', 1)`*** 设置基数今天不受支持。 请改用 `property(list, 'xyz', 1)`。 若要了解详细信息，请参阅 [TinkerPop 的顶点属性](http://tinkerpop.apache.org/docs/current/reference/#vertex-properties)。

* ***`atch()`*** 允许使用声明性模式匹配查询图形。 此功能不可用。

* 不支持顶点或边上的***对象作为属性***。 属性只能是基元类型或数组。

* 不支持***按数组属性排序*** `order().by(<array property>)`。 只支持按基元类型排序。

* 不支持***非基元 JSON 类型***。 使用 `string`、`number` 或 `true`/`false` 类型。 不支持 `null` 值。 

* 目前不支持 ***GraphSONv3*** 序列化程序。 在连接配置中使用 `GraphSONv2` Serializer、Reader 和 Writer 类。

* 目前不支持 **Lambda 表达式和函数**。 这包括 `.map{<expression>}`、`.by{<expression>}` 和 `.filter{<expression>}` 函数。 若要了解详细信息，并了解如何使用 Gremlin 步骤重写这些函数，请参阅[关于 Lambda 的说明](http://tinkerpop.apache.org/docs/current/reference/#a-note-on-lambdas)。

* 由于系统具有分布式特性，因此***事务***不受支持。  在 Gremlin 帐户上配置适当的一致性模型以“读取自己的写入”，并使用乐观并发解决冲突的写入。

## <a name="next-steps"></a>后续步骤
* 访问[Cosmos DB 用户语音](https://feedback.azure.com/forums/263030-azure-cosmos-db)页面，分享反馈，并帮助团队专注于对您重要的功能。
