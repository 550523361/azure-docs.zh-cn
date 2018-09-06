---
title: 使用 Azure Cosmos DB 全局分发数据 | Microsoft Docs
description: 了解如何通过 Azure Cosmos DB（一种全球分布式多模型数据库服务），使用全球数据库进行全球范围的异地复制、故障转移和数据恢复。
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: sngun
ms.openlocfilehash: 19e47e0dba1a89ea32f42ef0bafc26f8c59b4ad7
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2018
ms.locfileid: "43288296"
---
# <a name="how-to-distribute-data-globally-with-azure-cosmos-db"></a>如何使用 Azure Cosmos DB 在全球范围内分发数据
Azure 无处不在 - 它的足迹遍布全球 50 多个地理区域，并且还在不断扩展。 借助其在全球范围的足迹，Azure 为其开发人员提供的特色功能之一是能够轻松生成、部署和管理全球分布式应用程序。 

[Azure Cosmos DB](../cosmos-db/introduction.md) 是 Microsoft 针对任务关键型应用程序提供的全球分布式多模型数据库服务。 Azure Cosmos DB 在全球范围内提供统包全球分发、[吞吐量和存储空间弹性缩放](../cosmos-db/partition-data.md)、99% 情况下低至个位数的毫秒级延迟、[五个妥善定义的一致性模型](consistency-levels.md)，以及得到保证的高可用性，所有这些均由[行业领先的综合 SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) 提供支持。 Azure Cosmos DB [自动为所有数据编制索引](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf)，无需客户管理架构或索引。 它是多模型服务并支持文档、键值、图和列系列数据模型。 Azure Cosmos DB 是一项精心打造的云端原生服务，在开发过程中自始至终以多租户和全球分发为目标。


![跨 3 个区域进行分区和分布的 Azure Cosmos DB 容器](./media/distribute-data-globally/global-apps.png)

**跨多个 Azure 区域进行分区和分布的一个 Azure Cosmos DB 容器**

我们在构建 Azure Cosmos DB 时就已经知道，全球分发功能不能事后添加。 不能将其“直接附加”到“单一站点”数据库系统之上。 全局分布式数据库提供的功能超越了“单一站点”数据库提供的传统地理灾难恢复 (GEO-DR) 的功能。 提供 GEO-DR 功能的单一站点数据库是全局分布式数据库的严格子集。 

借助 Azure Cosmos DB 的统包全球分发，开发人员无需通过对数据库日志采用 Lambda 模式（例如，[AWS DynamoDB 复制](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)）或跨多个区域执行“双写操作”来生成自己的复制基架。 不建议采用这些方法，因为无法确保此类方法的正确性并提供合理正确的 SLA。 

本文概要介绍 Azure Cosmos DB 的全局分发功能， 同时介绍 Azure Cosmos DB 用于提供综合 SLA 的独特方法。 

## <a id="EnableGlobalDistribution"></a>启用统包全球分发
Azure Cosmos DB 提供了以下功能，方便用户轻松编写全局分布式应用程序。 这些功能通过 Azure Cosmos DB 的基于资源提供程序的 [REST API](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/) 以及 Azure 门户提供。

### <a id="RegionalPresence"></a>无处不在、分布广泛 
Azure 通过上线[新区域](https://azure.microsoft.com/regions/)，不断扩大地理覆盖面。 Azure Cosmos DB 被归为 Azure 的基础服务，并且默认情况下在所有新的 Azure 区域中可用。 这样一来，只要 Azure 开辟了新的业务区域，就能够将该地理区域与 Azure Cosmos DB 数据库帐户关联。

### <a id="UnlimitedRegionsPerAccount"></a>将不限数量的区域与 Azure Cosmos DB 数据库帐户关联
Azure Cosmos DB 允许将任意数量的 Azure 区域与 Azure Cosmos DB 数据库帐户关联。 除设有地理围栏限制的地域（例如，中国、德国）外，可与 Azure Cosmos DB 数据库帐户关联的区域数量没有限制。 下图显示一个配置跨越 25 个 Azure 区域的数据库帐户。  

![跨越 25 个 Azure 区域的 Azure Cosmos DB 数据库帐户](./media/distribute-data-globally/spanning-regions.png)

**某个租户的跨越 25 个 Azure 区域的 Azure Cosmos DB 数据库帐户**


### <a id="PolicyBasedGeoFencing"></a>基于策略的地理围栏
Azure Cosmos DB 采用了支持基于策略的地理围栏。 地理围栏是保障数据监管和合规性限制的重要元素，可能会阻止特定区域与帐户的关联。 地理围栏的示例包括（但不限于）将全球分发的范围限制在主权云（例如，中国和德国）中的区域，或政府纳税边界（例如，澳大利亚）内的区域。 策略通过 Azure 订阅的元数据进行控制。

### <a id="DynamicallyAddRegions"></a>动态添加和删除区域
Azure Cosmos DB 允许在任何时间点从数据库帐户添加（关联）或删除（取消关联）区域（参见[前图](#UnlimitedRegionsPerAccount)）。 通过跨分区并行复制数据，Azure Cosmos DB 可确保在添加新区域后，用户在 30 分钟内即可从全球任意位置开始对其进行操作（假设数据小于或等于 100 TB）。 

### <a id="FailoverPriorities"></a>故障转移优先级
Azure Cosmos DB 支持 SLA 支持的*多个妥善定义的、直观且实际可行的一致性模型*。 Azure Cosmos DB 确保自动故障转移序列以指定的优先级顺序发生。 有关区域故障转移的详细信息，请参阅 [Azure Cosmos DB 中可实现业务连续性的自动区域故障转移](regional-failover.md)。


![通过 Azure Cosmos DB 配置故障转移优先级](./media/distribute-data-globally/failover-priorities.png)

**Azure Cosmos DB 的租户可对与数据库帐户关联的区域配置故障转移优先级顺序（右窗格）**

### <a id="ConsistencyLevels"></a>用于全球分布式数据库的多个妥善定义的一致性模型
Azure Cosmos DB 支持 SLA 支持的[多个妥善定义的、直观且实际可行的一致性模型](consistency-levels.md)。 可根据工作负荷/方案选择特定的一致性模型（从可用的选项列表选择）。 

### <a id="TunableConsistency"></a>可优化的全局复制数据库一致性
Azure Cosmos DB 允许基于每个请求在运行时以编程方式替代和放宽默认的一致性选择。 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>动态可配置的读取和写入区域
Azure Cosmos DB 允许将区域（与数据库关联）配置为“读取”、“写入”或“读/写”区域。 

### <a id="ElasticallyScaleThroughput"></a>跨 Azure 区域灵活缩放吞吐量
用户能够以编程方式预配吞吐量，灵活缩放 Azure Cosmos DB 容器。 吞吐量应用于 Azure Cosmos DB 容器分布于其中的所有区域。

### <a id="GeoLocalReadsAndWrites"></a>异地-本地读取和写入
全球分布式数据库的主要好处是用户能够在世界各地任何位置进行低延迟数据访问。 Azure Cosmos DB 在全球提供 99% 情况下的低延迟读写。 它确保所有读取都从最近（本地）区域提供。 为服务于读取请求，会使用特定于发出读取操作的区域的本地仲裁。 这同样适用于写入。 只有在大部分副本已持久地在本地提交写入后但没有针对远程副本（用于确认写入）限制写入确认时才会确认写入。 换句话说，如果读取和写入仲裁对于发出请求的区域来说始终为本地仲裁，便会执行 Azure Cosmos DB 的复制协议。

### <a id="ManualFailover"></a>手动故障转移
Azure Cosmos DB 允许触发数据库帐户的故障转移，以验证整个应用程序（超出数据库）的端到端可用性属性。 由于故障检测和前导选择的安全性和活跃度属性均得到了保证，Azure Cosmos DB 可确保租户启动的手动故障转移操作实现“零数据丢失”。

### <a id="AutomaticFailover"></a>自动故障转移
Azure Cosmos DB 支持在发生一个或多个区域性故障期间自动进行故障转移。 区域故障转移期间，Azure Cosmos DB 会保持其读取延迟率、运行时间可用性、一致性和吞吐量 SLA。 Azure Cosmos DB 对自动故障转移操作完成的持续时间设置了上限。 这是区域性故障期间潜在的数据丢失时间段。

### <a id="GranularFailover"></a>旨在实现不同的故障转移粒度
目前，自动和手动故障转移功能以数据库帐户的粒度进行公开。 请注意，在内部，Azure Cosmos DB 旨在以更精细的数据库、容器甚或（拥有一系列键的容器的）分区粒度提供自动故障转移。 

### <a id="MultiHomingAPIs"></a>Azure Cosmos DB 中的多宿主
Azure Cosmos DB 允许使用逻辑（与区域无关）或物理（特定于区域）终结点与数据库交互。 使用逻辑终结点可确保发生故障转移时，应用程序可以透明方式采用多个宿主。 后者（物理终结点）提供对应用程序的细粒度控制，以将读取和写入重定向到特定区域。

用户可以在这些文章中找到有关如何配置 [SQL API](../cosmos-db/tutorial-global-distribution-sql-api.md)、[表 API](../cosmos-db/tutorial-global-distribution-table.md) 和 [MongoDB API](../cosmos-db/tutorial-global-distribution-mongodb.md) 的读取首选项的信息。

### <a id="TransparentSchemaMigration"></a>透明且一致的数据库架构和索引迁移 
Azure Cosmos DB 完全与[架构无关](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf)。 数据库引擎的特殊设计允许 Azure Cosmos DB 在数据引入时自动且同步地索引所有数据，而无需要求用户提供任何架构或辅助索引。 这使用户能够快速地循环访问全局分布式应用程序，而无需担心数据库架构和索引迁移或者协调多阶段应用程序的架构更改推出。 Azure Cosmos DB 保证用户对索引策略进行的所有显式更改都不会导致性能或可用性降低。  

### <a id="ComprehensiveSLAs"></a>综合 SLA（不只是高可用性）
作为一种全局分布式数据库服务，无论与数据库关联的区域数量是多少，Azure Cosmos DB 都可为全球规模运行的数据库提供针对“可用性”、“延迟”、“吞吐量”和“一致性”的定义完善的 SLA。  

## <a id="LatencyGuarantees"></a>延迟保证
全球分布式数据库服务（如 Azure Cosmos DB）的主要好处在于，提供在全球任何位置以低延迟访问数据的权限。 Azure Cosmos DB 为各种数据库操作提供保证 99% 的情况下的低延迟保证。 Azure Cosmos DB 采用的复制协议可确保数据库操作（读取和写入均适用）始终在客户端的本地区域执行。 Azure Cosmos DB 的延迟 SLA 为各种大小的请求和响应的读取、（同步）索引写入和查询均提供 99% 的保证情况。 写入的延迟保证包括持久的本地区域内的大多数仲裁提交。

### <a id="LatencyAndConsistency"></a>延迟与一致性的关系 
为使全局分布式服务在全局分布式设置中提供较强的一致性，它需要同步复制写入或同步执行跨区域读取。 光速和广域网可靠性决定了较强的一致性会导致数据库操作的更高的延迟和降低的可用性。 因此，为了针对所有采用宽松一致性的单区域帐户和多区域帐户提供保证 99% 的情况下的低延迟和 99.99% 的可用性，以及针对所有多区域数据库帐户提供 99.999% 的可用性，该服务必须采用异步复制。 这反过来会要求服务必须提供[定义完善且宽松的一致性模型](consistency-levels.md) - 相比“强”而言较弱的（提供低延迟和可用性保证）且理想情况下强于“最终”一致性（附带直观的编程模型）。

Azure Cosmos DB 确保无需读取操作便可跨多个区域联系副本，以提供特定的一致性级别保证。 同样，它可确保跨所有区域复制数据（即跨各区域异步复制写入）时不会阻止写入操作。 对于多区域数据库帐户，提供了强一致性级别和多个宽松的一致性级别。 

### <a id="LatencyAndAvailability"></a>延迟与可用性的关系 
延迟和可用性是同一硬币的两面。 我们将讨论稳定状态操作的延迟和出现故障和网络分区时的可用性。 从应用程序角度来看，慢速运行的数据库操作与不可用的数据库没有区别。 

为了将高延迟与不可用区分开来，Azure Cosmos DB 对各种数据库操作的延迟提供绝对上限。 如果完成数据库操作所用的时间超过上限，Azure Cosmos DB 将返回超时错误。 Azure Cosmos DB 可用性 SLA 确保根据可用性 SLA 计算超时。 

### <a id="LatencyAndThroughput"></a>延迟与吞吐量的关系
Azure Cosmos DB 不会让用户在延迟和吞吐量之间做出选择。 它遵循 SLA，两者延迟均保证 99% 的情况并传递预配的吞吐量。 

## <a id="ConsistencyGuarantees"></a>一致性保证
虽然[强一致性模型](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)是数据可编程性的黄金标准，但该模型导致的延迟代价太高（稳定状态下）且会降低可用性（遇到故障时）。 

Azure Cosmos DB 为用户提供定义完善的编程模型，用于推断复制数据的一致性。 为了帮助你借助多主页功能轻松构建全局分布式应用程序，Azure Cosmos DB 公开的一致性模型旨在实现与区域无关，并且独立于提供读写的区域。 

Azure Cosmos DB 一致性 SLA 保证 100% 的读取请求满足指定的（数据库帐户或请求级别）一致性模型的一致性保证。 如果满足与一致性级别关联的所有一致性保证，则读取请求被视为已满足一致性 SLA。 下表捕获了与 Azure Cosmos DB 提供的特定一致性模型相对应的一致性保证。

<table>
    <tr>
        <td><strong>一致性模型</strong></td>
        <td><strong>一致性特征</strong></td>
        <td><strong>SLA</strong></td>
    </tr>
        <tr>
        <td>非常</td>
        <td>线性化</td>
        <td>100%</td>
    </tr>
    <tr>
        <td rowspan="3">有限过期性</td>
        <td>单调读取（区域内）</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>一致前缀</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>过期期限 &lt; K、T</td>
        <td>100%</td>
    </tr>
<tr>
        <td rowspan="3">会话</td>
        <td>读取自己的写入</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>单调读取</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>一致前缀</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>一致前缀</td>
        <td>一致前缀</td>
        <td>100%</td>
    </tr>
</table>

**与 Azure Cosmos DB 中给定的一致性模型关联的一致性保证**


### <a id="ConsistencyAndAvailability"></a>一致性与可用性的关系
[CAP 定理](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)的[不可能结果](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)证明遇到故障时，系统确实不可能保持可用并提供线性一致性。 数据库服务必须选择采用 CP 还是 AP，其中 CP 系统会放弃可用性以支持线性一致性，而 AP 系统会放弃[线性一致性](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)以支持可用性。 Azure Cosmos DB 绝不会违反所请求的一致性模型，由此可准确判断其为 CP 系统。 但在实践中，一致性并不是一个“全有或者全无”的命题；介于线性一致性和最终一致性之间的一致性范畴中还存在多个定义完善的一致性模型。 在 Azure Cosmos DB 中，标识数个适用于真实世界场景且可以直观使用的宽松一致性模型。 Azure Cosmos DB 提供[多个宽松但完善定义的一致性模型](consistency-levels.md)、99.99% 的一致性（对于所有单区域的数据库帐户）、99.999% 的读写可用性（对于所有多区域数据库帐户），从而进行一致性与可用性的权衡取舍。 

### <a id="ConsistencyAndAvailability"></a>一致性与延迟的关系
CAP 定理更全面的变体名为 [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)，该变体也用于稳定状态下的延迟和一致性的权衡取舍。 该定理认为在稳定状态下，数据库系统必须在一致性和延迟间做出选择。 通过多个宽松的一致性模型（由异步复制以及本地读取和写入仲裁提供支持），Azure Cosmos DB 可确保所有读取和写入操作分别在读取和写入区域本地进行。 这允许 Azure Cosmos DB 针对给定的一致性模型在区域内提供低延迟保证。  

### <a id="ConsistencyAndThroughput"></a>一致性与吞吐量的关系
由于特定一致性模型的实现依赖于所选的[仲裁类型](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)，因此吞吐量也会根据所选一致性模型而有所不同。 例如，在 Azure Cosmos DB 中，强一致读取的 RU 费用大约是最终一致读取的两倍。 在这种情况下，需要预配两倍的 RU 才能实现相同的吞吐量。


![一致性与吞吐量之间的关系](./media/distribute-data-globally/consistency-and-throughput.png)

**Azure Cosmos DB 中特定一致性模型的读取容量关系**

## <a id="ThroughputGuarantees"></a>吞吐量保证 
Azure Cosmos DB 允许根据需求，灵活地跨任意数量区域缩放吞吐量（以及存储）。 

![Azure Cosmos DB 分布容器和分区容器](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

单个 Azure Cosmos DB 容器 横向分区（跨区域内的三个资源分区），然后跨三个 Azure 区域全局分布

Azure Cosmos DB 容器在两个维度中进行分布：(i) 在区域内以及 (ii) 跨区域。 方法如下： 

* **本地分布**：在单个区域中，Azure Cosmos DB 根据资源分区横向扩展。 每个资源分区管理一组键，属于强一致且高度可用，由名为“副本集”的四个副本和这些副本中的状态机复制实现物理表示。 Azure Cosmos DB 是一种完全由资源管理的系统，其中资源分区负责传递系统资源分配给它的预算吞吐量。 Azure Cosmos DB 容器的缩放对用户是透明的。 Azure Cosmos DB 管理资源分区，并按照存储和吞吐量需求更改所需对它们进行拆分和合并。 
* **全球分布**：如果是多区域数据库，那么跨这些区域分布每个资源分区。 跨各个区域拥有同一组键的资源分区构成分区集（请参阅[前图](#ThroughputGuarantees)）。  通过跨多个与数据库相关联的区域使用状态机复制，对分区集内的资源分区进行协调。 根据配置的一致性级别，使用不同的拓扑（例如，星号、菊花链、树等）动态配置分区集内的资源分区。 

凭借高响应分区管理、负载均衡和严格的资源管理，Azure Cosmos DB 允许跨多个与 Azure Cosmos DB 容器或数据相关联的 Azure 区域灵活缩放吞吐量。 更改预配吞吐量是 Azure Cosmos DB 中的运行时操作。 类似于其他数据库操作，针对更改预配吞吐量的请求，Azure Cosmos DB 保证延迟的绝对上限。 例如，下图显示了一个根据需求灵活预配吞吐量（在两个区域之间，范围为 1M-10M 个请求/秒）的客户容器。

![Azure Cosmos DB 灵活预配的吞吐量](./media/distribute-data-globally/elastic-throughput.png)

**弹性预配吞吐量的客户容器（每秒 1M-10M 请求）**

### <a id="ThroughputAndConsistency"></a>吞吐量与一致性的关系 
这与[一致性与吞吐量的关系](#ConsistencyAndThroughput)中所述相同。

### <a id="ThroughputAndAvailability"></a>吞吐量与可用性的关系
预配吞吐量更改时，Azure Cosmos DB 继续保持其高可用性。 Azure Cosmos DB 以透明方式管理资源分区（执行拆分、合并和克隆操作），并确保应用程序灵活增加或减少吞吐量时，操作不会降低性能或可用性。 

## <a id="AvailabilityGuarantees"></a>可用性保证
Azure Cosmos DB 为所有单区域数据库帐户和具有松散一致性的所有多区域帐户提供 99.99% 的可用性 SLA，为所有多区域数据库帐户提供 99.999% 的可用性。 如前面所述，Azure Cosmos DB 的可用性保证包括针对每一个数据和控制平面操作的绝对延迟上限。 可用性保证是不变的，不会随区域数量或区域间的地理距离而更改。 可用性保证对手动和自动故障转移均适用。 Azure Cosmos DB 提供透明的多宿主 API，可确保应用程序能够针对逻辑终结点运行，并且在发生故障转移时能够以透明方式将请求路由到新区域。 在区域故障转移的情况下，不需要重新部署应用程序，且可用性 SLA 一直保留。

### <a id="AvailabilityAndConsistency"></a>可用性与一致性、延迟和吞吐量的关系
[一致性与可用性的关系](#ConsistencyAndAvailability)、[延迟与可用性的关系](#LatencyAndAvailability)和[吞吐量与可用性的关系](#ThroughputAndAvailability)几节中介绍了可用性与一致性、延迟和吞吐量的关系。 

## <a id="CustomerFacingSLAMetrics"></a>面向客户的 SLA 指标
Azure Cosmos DB 以透明方式公开吞吐量、延迟、一致性和可用性指标。 这些指标可通过 Azure 门户以编程方式进行访问（见下图）。 还可以使用 Azure Application Insights 对各种阈值设置警报。
 

![Azure Cosmos DB 的客户可见的 SLA 指标](./media/distribute-data-globally/customer-slas.png)

**一致性、延迟、吞吐量和可用性指标以透明方式向每个租户提供**

## <a id="Next Steps"></a>后续步骤
* 若要使用 Azure 门户实现 Azure Cosmos DB 帐户的全局复制，请参阅[如何使用 Azure 门户执行 Azure Cosmos DB 全局数据库复制](tutorial-global-distribution-sql-api.md)。
* 若要了解如何通过 Azure Cosmos DB 实现多主体系结构，请参阅[使用 Azure Cosmos DB 实现的多主数据库体系结构](multi-region-writers.md)。
* 若要深入了解 Azure Cosmos DB 中自动故障转移和手动故障转移的工作方式，请参阅 [Azure Cosmos DB 中的区域故障转移](regional-failover.md)。

## <a id="References"></a>参考
1. Eric Brewer。 [Towards Robust Distributed Systems](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)（迈向强大稳定的分布式系统）
2. Eric Brewer。 [CAP Twelve Years Later – How the rules have changed](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)（十二年之后的 CAP - 规则发生了怎样的改变）
3. Gilbert, Lynch。 - [Brewer&#39;s Conjecture and Feasibility of Consistent, Available, Partition Tolerant Web Services](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)（Brewer 的猜想以及一致、可用、分区容错的 Web 服务的可行性）
4. Daniel Abadi。 [Consistency Tradeoffs in Modern Distributed Database Systems Design](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)（现代分布式数据库系统设计中的一致性平衡方案）
5. Martin Kleppmann. [Please stop calling databases CP or AP](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)（请停止调用数据库 CP 或 AP）
6. Peter Bailis et al。[Probabilistic Bounded Staleness (PBS) for Practical Partial Quorums](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)（实用部分仲裁的概率有限过期性 (PBS)）
7. Naor 和 Wool。 [Load, Capacity and Availability in Quorum Systems](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)（仲裁系统中的负载、容量和可用性）
8. Herlihy 和 Wing。 [Lineralizability: A correctness condition for concurrent objects](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)（Lineralizability：并发对象的正确性条件）
9. [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
