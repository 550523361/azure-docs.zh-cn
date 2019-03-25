---
title: Azure Cosmos DB Gremlin API 简介
description: 了解如何在 Azure Cosmos DB 中通过使用 Apache TinkerPop 的 Gremlin 图形查询语言以较低的延迟存储、查询和遍历大量图形。
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 09/05/2018
ms.author: lbosq
ms.openlocfilehash: 36465c253996e4cecc665b2fd1d59c03adc78a2f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2019
ms.locfileid: "58110539"
---
# <a name="introduction-to-azure-cosmos-db-gremlin-api"></a>Azure Cosmos DB 简介：Gremlin API

[Azure Cosmos DB](introduction.md) 是 Microsoft 针对任务关键型应用程序提供的全局分布式多模型数据库服务。 它是一个多模型数据库，支持文档、键-值、图形和纵栏表数据模型。 Azure Cosmos DB Gremlin API 用于存储和操作图形数据。 Gremlin API 支持对图形数据进行建模，并提供了用于遍历图形数据的 API。

本文提供 Azure Cosmos DB Gremlin API 概述，并说明如何使用它存储具有数十亿顶点和边缘的大量图形。 可以使用毫秒级延迟的情况下查询图形，并轻松扩展图形结构和架构。 若要查询 Azure Cosmos DB，可以使用 [Apache TinkerPop](https://tinkerpop.apache.org) 图形遍历语言或 [Gremlin](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps)。

## <a name="what-is-a-graph-database"></a>什么是图形数据库
现实世界中的数据存在必然的联系。 传统数据建模以实体为重心。 对于许多应用程序，还需要进行建模，或是自然地为实体和关系建模。

[图形](http://mathworld.wolfram.com/Graph.html)是由[顶点](http://mathworld.wolfram.com/GraphVertex.html)和[边缘](http://mathworld.wolfram.com/GraphEdge.html)组成的结构。 顶点和边缘可以包含任意数目的属性。 

* **顶点** - 顶点表示人员、地点或事件等离散对象。 

* **边** - 边表示顶点之间的关系。 例如，一个人可能认识其他人、涉及到某个事件以及最近处于某个位置。 

* **属性** - 属性表示有关顶点和边的信息。 属性示例包括具有姓名和年龄的顶点。 边具有时间戳和/或权重。 更准确地讲，此模型称为[属性图形](https://tinkerpop.apache.org/docs/current/reference/#intro)。 Azure Cosmos DB 支持属性图形模型。

例如，以下示例图形显示了人员、移动设备、兴趣和操作系统之间的关系：

![显示人员、设备和兴趣的示例数据库](./media/graph-introduction/sample-graph.png)

使用图形数据库可以轻松高效地存储图形并为其建模，使图形可用于许多场合。 图形数据库通常是 NoSQL 数据库，因为这些用例通常还需要架构灵活性和快速迭代。

可以将图形数据库提供的高速遍历与深度优先搜索、广度优先搜索和 Dijkstra 算法等图形算法相结合，解决社交网络、内容管理、地理空间和推荐等各个领域存在的问题。

## <a name="features-of-azure-cosmos-db-graph-database"></a>Azure Cosmos DB 图形数据库的功能
 
Azure Cosmos DB 是一个完全托管的图形数据库，提供全局分发、存储和吞吐量弹性缩放、自动索引编制与查询、可优化的一致性级别，并支持 TinkerPop 标准。

![Azure Cosmos DB 图形体系结构](./media/graph-introduction/cosmosdb-graph-architecture.png)

与市场中的其他图形数据库相比，Azure Cosmos DB 提供以下独特功能：

* 可弹性缩放的吞吐量和存储

  现实世界中的图形需要扩展到超越单个服务器的容量。 使用 Azure Cosmos DB 可跨多个服务器无缝缩放图形。 还可以根据访问模式独立缩放图形的吞吐量。 Azure Cosmos DB 支持几乎可以扩展到无限存储大小和预配吞吐量的图形数据库。

* 多区域复制

  Azure Cosmos DB 以透明方式将图形数据复制到与你的帐户关联的所有区域。 通过复制可以开发需要访问数据全局访问权限的应用程序。 在一致性、可用性和性能以及对应保证的领域中，需要进行权衡。 Azure Cosmos DB 通过多宿主 API 提供透明区域故障转移。 可以在全球范围内灵活缩放吞吐量和存储。

* 使用熟悉的 Gremlin 语法的快速查询和遍历

  存储异类顶点和边缘，并通过熟悉的 Gremlin 语法查询这些文档。 Azure Cosmos DB 使用高并发、无锁、日志结构化索引技术为所有内容自动编制索引。 此功能可以实现丰富的实时查询和遍历，而无需指定架构提示、二级索引或视图。 在[通过使用 Gremlin 查询图形](gremlin-support.md)中了解详细信息。

* 完全托管

  通过 Azure Cosmos DB 无需管理数据库和计算机资源。 作为一种完全托管的 Microsoft Azure 服务，无需管理虚拟机、部署并配置软件、管理数据库和资源的增减，或处理复杂的数据层升级。 每个图形会自动备份，以防受到区域故障的影响。 你可以轻松添加 Azure Cosmos DB 帐户并根据需要预配容量，以便可以专注于应用程序而不是操作和管理数据库。

* 自动索引编制

  默认情况下，Azure Cosmos DB 自动为图形中的节点和边缘包含的所有属性编制索引，无需任何架构或创建二级索引。

* 与 Apache TinkerPop 兼容

  Azure Cosmos DB 原生支持开源 Apache TinkerPop 标准，可与其他支持 TinkerPop 的图形系统集成。 因此，可以轻松地从 Titan 或 Neo4j 等其他图形数据库迁移，或者将 Azure Cosmos DB 与 Apache Spark GraphX 等图形分析框架配合使用。

* 可优化的一致性级别

  从五个妥善定义的一致性级别中选择，实现一致性与性能之间的最佳平衡。 对于查询和读取操作，Azure Cosmos DB 提供五种不同的一致性级别：强、有限过时、会话、一致前缀和最终。 通过这些细化的妥善定义的一致性级别，可以在一致性、可用性与延迟之间实现合理的平衡。 有关详细信息，请参阅 [Azure Cosmos DB 中的可优化数据一致性级别](consistency-levels.md)。

Azure Cosmos DB 还可以在相同的容器/数据库中使用多个模型（例如文档和图形）。 可以使用文档容器将图形数据与文档一起存储。 可以使用 JSON 上的 SQL 查询和 Gremlin 查询来查询与图形相同的数据。

## <a name="get-started"></a>入门

可以使用 Azure 命令行接口 (CLI)、Azure PowerShell 或 Azure 门户来创建或访问 Azure Cosmos DB Gremlin API 帐户。 创建帐户后，可以使用 Gremlin API 服务终结点 `https://<youraccount>.gremlin.cosmosdb.azure.com`（提供适用于 Gremlin 的 WebSocket 前端）访问该帐户中的图形数据库。 可以将 TinkerPop 兼容的工具（例如 [Gremlin 控制台](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console)）配置为连接到此终结点，并在 Java、Node.js 或任何 Gremlin 客户端驱动程序中生成应用程序。

下表显示可以对 Azure Cosmos DB 使用的常用 Gremlin 驱动程序：

| 下载 | 文档 | 入门 | 支持的连接器版本 |
| --- | --- | --- | --- |
| [.NET](https://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-DotNet) | [GitHub 上的 Gremlin.NET](https://github.com/apache/tinkerpop/tree/master/gremlin-dotnet) | [使用 .NET 创建图形](create-graph-dotnet.md) | 3.4.0-RC2 |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) | [Gremlin JavaDoc](https://tinkerpop.apache.org/javadocs/current/full/) | [使用 Java 创建图形](create-graph-java.md) | 3.2.0+ |
| [Node.js](https://www.npmjs.com/package/gremlin) | [GitHub 上的 Gremlin-JavaScript](https://github.com/jbmusso/gremlin-javascript) | [使用 Node.js 创建图形](create-graph-nodejs.md) | 2.6.0|
| [Python](https://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-python) | [GitHub 上的 Gremlin-Python](https://github.com/apache/tinkerpop/tree/master/gremlin-python) | [使用 Python 创建图形](create-graph-python.md) | 3.2.7 |
| [PHP](https://packagist.org/packages/brightzone/gremlin-php) | [GitHub 上的 Gremlin-PHP](https://github.com/PommeVerte/gremlin-php) | [使用 PHP 创建图形](create-graph-php.md) | 3.1.0 |
| [Gremlin 控制台](https://tinkerpop.apache.org/downloads.html) | [TinkerPop 文档](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |  [使用 Gremlin 控制台创建图形](create-graph-gremlin-console.md) | 3.2.0 + |

## <a name="graph-database-design-considerations"></a>图形数据库设计注意事项

在图形设计过程中，将某个实体建模为其自身顶点（而不是建模为其他顶点实体的属性）的决策会对性能和成本产生影响。 此决策的主要驱动因素取决于数据的查询方式，以及模型本身的可伸缩性。

在规划如何对实体进行建模之前，请考虑以下问题：

* 对于大多数查询，哪些实体需要作为顶点进行检索？

* 在为了进行数据筛选而添加的图形中，需要包括哪些信息？

* 哪些实体仅是到其他实体的连接，以及需要检索哪些实体的值？

* 我的查询需要检索哪些信息，它们将产生的 RU 费用是多少？

例如，假设有以下图形设计：

![图形设计注意事项示例](./media/graph-introduction/graph-design-considerations-example.png)

* 根据查询，可以使用“区域->门店”关系来唯一地筛选“门店”顶点。 例如，如果查询采用以下格式 - “获取属于某个特定区域的所有门店”。 如果是这种情况，则值得考虑将“区域”实体从其自己的顶点折叠到“门店”顶点的属性。 

* 这样做的好处是，将检索每个“门店”顶点的成本从一次获取三个图形对象（区域、区域->门店、门店）降低到一次获取单个“门店”顶点。 这可以改进性能，并降低每个查询的成本。

* 因为“门店”顶点链接到两个不同的实体 -“员工”和“产品”。 这使得“门店”成为一个必要的顶点，因为它可以提供额外的遍历可能性。  



## <a name="scenarios-that-can-use-gremlin-api"></a>可以使用 Gremlin API 的场合
下面是可以使用 Azure Cosmos DB 图形支持的一些场合：

* 社交网络

  通过合并有关客户及其与其他人的互动的数据，可以开发个性化的体验、预测客户行为，或者将某些人员与其他具有类似兴趣的人员相连接。 使用 Azure Cosmos DB 可以管理社交网络以及跟踪客户的喜好与数据。

* 推荐引擎

  此场合通常用于零售行业。 通过合并有关产品、用户和用户互动（例如购买、浏览某件商品或者为商品评分）的信息，可以生成自定义的推荐内容。 Azure Cosmos DB 的低延迟、弹性缩放和原生图形支持是为这些互动建模的理想选择。

* 地理空间

  电信、物流和旅行规划行业中的许多应用程序需要在某个区域中查找兴趣点，或者查找两个地点之间最短/最佳的路线。 Azure Cosmos DB 天生就很适合解决这些问题。

* 物联网

  当 IoT 设备之间的网络和连接建模为图形时，可以更好地理解设备和资产的状态。 还可以了解网络一个部分的更改可能对另一个部分造成的影响。

## <a name="next-steps"></a>后续步骤
若要详细了解 Azure Cosmos DB 中的图形支持，请参阅：

* [Azure Cosmos DB 图形入门教程](create-graph-dotnet.md)。
* 了解如何[通过使用 Gremlin 在 Azure Cosmos DB 中查询图形](gremlin-support.md)。
