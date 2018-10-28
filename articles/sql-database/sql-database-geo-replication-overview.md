---
title: 故障转移组和活动异地复制 - Azure SQL 数据库 | Microsoft Docs
description: 在服务中断时使用自动故障转移组和活动异地复制，并启用自动故障转移。
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 10/19/2018
ms.openlocfilehash: 9332bcb5849deacde13d67e4a18ea19af8af2977
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471469"
---
# <a name="overview-active-geo-replication-and-auto-failover-groups"></a>概述：活动异地复制和自动故障转移组

活动异地复制是一项 Azure SQL 数据库功能，可用于在相同或不同的数据中心（区域）创建数据库的可读副本。

![异地复制](./media/sql-database-geo-replication-failover-portal/geo-replication.png )

活动异地复制旨在作为业务连续性解决方案，允许应用程序在发生数据中心规模的服务中断时执行快速灾难恢复。 如果启用了异地复制，则应用程序可以向其他 Azure 区域中的辅助数据库启动故障转移。 在相同或不同的区域中最多支持四个辅助数据库，并且辅助数据库也可以用于只读访问查询。 故障转移必须由应用程序或用户手动启动。 故障转移后，新的主数据库具有不同的连接终结点。

> [!NOTE]
> 活动异地复制现在可供所有区域的所有服务层中的所有数据库使用。
> 活动异地复制不可在托管实例中使用。

自动故障转移组是活动异地复制的扩展。 它旨在使用应用程序启动的故障转移或通过基于用户定义的条件委派 SQL 数据库服务完成故障转移，来同时管理多个异地复制数据库的故障转移。 后者允许你在发生下述情况后自动恢复次要区域中的多个相关数据库：灾难性故障或其他导致主要区域中 SQL 数据库服务完全或部分丧失可用性的计划外事件。 此外，你还可以使用可读辅助数据库卸载只读查询工作负荷。 由于自动故障转移组涉及多个数据库，因此这些数据库必须在主服务器上进行配置。 故障转移组中数据库的主服务器和辅助服务器必须位于同一订阅中。 自动故障转移组支持将组中所有的数据库复制到另一个区域中唯一的辅助服务器。

> [!NOTE]
> 如果需要多个辅助服务器，请使用活动异地复制。

如果使用活动异地复制，且主数据库因某种原因而出现故障，或者如果只是需要脱机，则可以启动到任何辅助数据库的故障转移。 当将故障转移激活到辅助数据库之一时，所有其他辅助数据库会自动链接到新的主数据库。 如果使用自动故障转移组管理数据库恢复，并且任何影响组中一个或多个数据库的故障会导致自动故障转移。 可以配置最能满足应用程序需求的自动故障转移策略，或者可以选择退出并使用手动激活。 此外，自动故障转移组提供在故障转移期间保持不变的读写和只读侦听器终结点。 无论使用手动故障转移激活还是自动故障转移激活，故障转移都会将组中所有的辅助数据库切换到主数据库。 数据库故障转移完成后，会自动更新 DNS 记录，以便将终结点重定向到新的区域。

可以使用活动异地复制管理服务器或弹性池中单个数据库或一组数据库的复制和故障转移。 可通过以下方式执行此操作：

- [Azure 门户](sql-database-geo-replication-portal.md)
- [PowerShell：单个数据库](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
- [PowerShell：弹性池](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
- [PowerShell：故障转移组](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- [TRANSACT-SQL：单个数据库或弹性池](/sql/t-sql/statements/alter-database-azure-sql-database)
- [REST API：单个数据库](https://docs.microsoft.com/rest/api/sql/replicationlinks)
- [REST API：故障转移组](https://docs.microsoft.com/rest/api/sql/failovergroups)。

故障转移后，请确保在新的主机上配置服务器和数据库的身份验证要求。 有关详细信息，请参阅[灾难恢复后的 Azure SQL 数据库安全性](sql-database-geo-replication-security-config.md)。 这同时适用于活动异地复制和自动故障转移组。

活动异地复制利用 SQL Server 的 [Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) 技术，使用快照隔离以异步方式将主数据库上已提交的事务复制到辅助数据库。 自动故障转移组提供基于活动异地复制的组语义，但使用相同的异步复制机制。 尽管在任意给定时间，辅助数据库可能略微滞后于主数据库，但系统可以保证辅助数据永不包含部分事务。 跨区域冗余使应用程序能够在自然灾害、灾难性人为失误或恶意行为导致整个或部分数据中心永久性数据丢失后得以快速恢复。 [业务连续性概述](sql-database-business-continuity.md)中提供了具体的 RPO 数据。

下图显示了配置的活动异地复制示例，其中主数据库位于“美国中北部”区域，辅助数据库位于“美国中南部”区域。

![异地复制关系](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

因为辅助数据库是可读的，并且可用于卸载只读工作负荷，如报表作业。 如果使用活动异地复制，则可在主数据库所在的区域中创建辅助数据库，但不会增加应用程序针对灾难性故障的复原能力。 如果使用自动故障转移组，则会始终在不同的区域中创建辅助数据库。

除了灾难恢复外，活动异地复制还可用于以下情况：

- **数据库迁移**：可以使用活动异地复制将数据库在联机情况下从一台服务器迁移到另一台服务器，只需要极少的停机时间。
- **应用程序升级**：可以在应用程序升级期间创建额外的辅助数据库作为故障回复副本。

若要真正实现业务连续性，只需添加数据中心之间的数据库冗余即可，这只是该解决方案的一部分功能。 在发生灾难性故障后，端对端地恢复应用程序（服务）需要恢复构成该服务的所有组件以及所有依赖服务。 这些组件的示例包括客户端软件（例如，使用自定义 JavaScript 的浏览器）、Web 前端、存储和 DNS。 所有组件必须能够弹性应对相同的故障，并在应用程序的恢复时间目标 (RTO) 值内变为可用，这一点非常关键。 因此，需要识别所有依赖服务，并了解它们提供的保证和功能。 然后，必须执行适当的步骤来确保对服务所依赖的服务执行故障转移期间，服务能够正常运行。 有关设计灾难恢复解决方案的详细信息，请参阅[设计使用活动异地复制的灾难恢复云解决方案](sql-database-designing-cloud-solutions-for-disaster-recovery.md)。

## <a name="active-geo-replication-capabilities"></a>活动异地复制功能

活动异地复制功能提供以下基本功能：

- **自动异步复制**

 只能通过添加到现有数据库来创建辅助数据库。 辅助数据库可在任何 Azure SQL 数据库服务器中创建。 创建完成之后，使用从主数据库复制的数据填充辅助数据库。 这个过程称为种子设定。 创建辅助数据库并设定其种子后，会自动以异步方式将主数据库的更新复制到辅助数据库。 异步复制是指先在主数据库上提交事务，然后将事务复制到辅助数据库。

- **可读取的辅助数据库**

  应用程序可使用与访问主数据库时所用的相同或不同的安全主体来访问辅助数据库以执行只读操作。 辅助数据库在快照隔离模式下运行，以确保对主数据库的更新的复制（日志重播）不会因辅助数据库上执行的查询操作而延迟。

  > [!NOTE]
  > 如果主数据库上有架构更新，则日志重播会在辅助数据库上延迟。 因为架构更新需要在辅助数据库上有架构锁。

- **多个可读的辅助数据库**

  两个或多个辅助数据库可提高主数据库和应用程序的冗余和保护级别。 如果存在多个辅助数据库，即使其中一个辅助数据库发生故障，应用程序仍会受到保护。 如果只有一个辅助数据库，一旦它发生故障，应用程序就会遭受更高的风险，直到创建了新的辅助数据库。

  > [!NOTE]
  > 如果使用活动异地复制生成全球分布的应用程序，并需要在四个以上的区域中提供数据只读访问，则可以创建辅助数据库的辅助数据库（此过程称为链接）。 采用这种方式几乎可以实现数据库复制的无限制缩放。 此外，链接减少了从主数据库进行复制的开销。 随之而来的弊端是增加了大多数辅助数据库上的复制延迟。

- **支持弹性池数据库**

  每个副本可单独参与弹性池或根本不在弹性池中。 每个副本的池选择是单独的，并且不会依赖任何其他副本的配置（无论是主数据库还是辅助数据库）。 每个弹性池包含在单个区域内，因此相同拓扑中的多个副本永远不会共享弹性池。

- **可配置的辅助数据库计算大小**

  主数据库和辅助数据库都需要有相同的服务层。 另外，强烈建议创建与主数据库具有相同计算大小（DTU 或 vCore）的辅助数据库。 如果辅助数据库的计算大小较低，则会面临复制延迟时间增大、辅助数据库可能不可用等风险，并因此导致在故障转移后丢失大量数据的风险。 因此，发布的 RPO = 5 秒无法保证。 另一个风险是，在故障转移后，在新的主数据库升级到较高的计算大小之前，应用程序的性能将由于新的主数据库的计算能力不足而受影响。 升级时间取决于数据库大小。 此外，当前这类升级要求主数据库和辅助数据库都处于联机状态，因此在中断缓解之前无法完成。 如果决定创建具有较低计算大小的辅助数据库，Azure 门户上的日志 IO 百分比图表提供了一种评估维持复制负荷所需的辅助数据库的最小计算大小的好办法。 例如，如果主数据库是 P6 (1000 DTU)，其日志 IO 百分比为 50%，则辅助数据库需要至少为 P4 (500 DTU)。 还可以使用 [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) 或 [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) 数据库视图检索日志 IO 数据。  有关 SQL 数据库计算大小的详细信息，请参阅[什么是 SQL 数据库服务层](sql-database-service-tiers.md)。

- **用户控制的故障转移和故障回复**

  应用程序或用户可随时会辅助数据库显式切换到主角色。 在实际服务中断期间，应使用“计划外”选项，这会立即将辅助数据库升级为主数据库。 当出现故障的主数据库恢复并再次可用时，系统会自动将恢复的主数据库标记为辅助数据库并使其与新的主数据库保持最新状态。 由于复制的异步特性，在未计划的故障转移期间，如果主数据库在将最新的更改复制到辅助数据库之前出现故障，则可能会丢失少量数据。 当具有多个辅助数据库的主数据库进行故障转移时，系统会自动重新配置复制关系，并将剩余辅助数据库链接到新升级的主数据库，无需任何用户的干预。 导致了故障转移的服务中断得到缓解后，可能需要将应用程序返回到主要区域。 为此，应使用“计划内”选项调用故障转移命令。

- **保持凭据和防火墙规则同步**

  建议对异地复制数据库使用[数据库防火墙规则](sql-database-firewall-configure.md)，以便这些规则可与数据库一起复制，从而确保所有辅助数据库具有与主数据库相同的防火墙规则。 此方法不再需要客户手动配置和维护承载主数据库和辅助数据库的服务器上的防火墙规则。 同样，将[包含的数据库用户](sql-database-manage-logins.md)用于数据访问可确保主数据库和辅助数据库始终具有相同的用户凭据，以便在故障转移期间，不会因登录名和密码不匹配而产生中断。 通过添加 [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)，客户可以管理主数据库和辅助数据库的用户访问权限，且不再需要同时管理数据库中的凭据。

## <a name="auto-failover-group-capabilities"></a>自动故障转移组功能

通过支持组级别复制和自动故障转移，自动故障转移组功能提供功能强大的活动异地复制的抽象。 此外，通过提供更多侦听器终结点，该功能消除了在故障转移后更改 SQL 连接字符串的必要性。

> [!NOTE]
> 托管实例不提供自动故障转移功能。
>  

- **故障转移组**

  可在不同区域的两个服务器（主服务器和辅助服务器）之间创建一个或多个故障转移组。 每组可包含一个或多个数据库，这些数据库是在所有或某些主数据库因主要区域中的服务中断而变得不可用时，作为单元恢复的。  

- **主服务器**

  在故障转移组中托管主数据库的服务器。

- **辅助服务器**

  在故障转移组中托管辅助数据库的服务器。 辅助服务器不能在与主服务器相同的区域。

- **将数据库添加到故障转移组**

  可将一台服务器内或一个弹性池内的多个数据库放入同一故障转移组。 如果将单个数据库添加到组中，则它会自动使用相同的版本和计算大小创建辅助数据库。 如果主数据库在弹性池中，将使用相同的名称在弹性池中自动创建辅助数据库。 如果在辅助服务器中添加已具有辅助数据库的数据库，则异地复制由组继承。

  > [!NOTE]
  > 在不属于故障转移组的服务器中添加已具有辅助数据库的数据库时，会在辅助服务器中创建新的辅助数据库。

- **故障转移组读写侦听器**

  指向当前主服务器 URL 的格式为 &lt;failover-group-name&gt;.database.windows.net 的 DNS CNAME 记录。 它允许读写 SQL 应用程序在故障转移发生后主服务器发生更改时，以透明方式重新连接到主数据库。

- **故障转移组只读侦听器**

  指向辅助服务器 URL 的格式为 &lt;failover-group-name&gt;.secondary.database.windows.net 的 DNS CNAME 记录。 它允许只读 SQL 应用程序以透明方式使用指定的负载均衡规则连接到辅助数据库。

- **自动故障转移策略**

  默认使用自动故障转移策略配置故障转移组。 检测到故障并且宽限期到期后，系统会触发故障转移。 系统必须确保，因影响范围太大，SQL 数据库服务的内置高可用性基础结构无法缓解服务中断。 如果要从应用程序控制故障转移工作流，可以关闭自动故障转移。

- **只读故障转移策略**

  默认禁用只读侦听器的故障转移功能。 这可确保在辅助数据库脱机时，主数据库的性能不会受到影响。 但是，这也意味辅助数据库恢复前，只读会话将无法连接。 如果无法容许只读会话停机，但能够以主数据库的潜在性能降级为代价将主数据库临时用于只读和读写流量，则可以为只读侦听器启用故障转移。 在这种情况下，如果辅助服务器不可用，则会将只读流量自动重定向到主服务器。  

- **手动故障转移**

  可随时手动启动故障转移，而不考虑自动故障转移配置。 如果未配置自动故障转移策略，则需要进行手动故障转移来恢复故障转移组中的数据库。 可通过完整数据同步启动强制或友好的故障转移。 后者可用于将活动服务器重新定位到主要区域。 故障转移完成后，会自动更新 DNS 记录，以确保连接到正确的服务器。

- **数据丢失宽限期**

  由于主数据库和辅助数据库是使用异步复制进行同步的，因此故障转移可能会导致数据丢失。 可以自定义自动故障转移策略，以便反映应用程序对数据丢失的容错。 通过配置 **GracePeriodWithDataLossHours**，可以控制系统启动可能导致数据丢失的故障转移之前的等待时间。

  > [!NOTE]
  > 系统检测到组中的数据库仍处于联机状态（例如服务中断仅影响服务控制平面）时，会立即使用完整数据同步激活故障转移（友好故障转移），而不考虑为 **GracePeriodWithDataLossHours** 设置的值。 此行为确保在恢复期间不会丢失数据。 宽限期仅在不可能进行友好故障转移时发挥作用。 如果在宽限期到期之前服务中断得到了缓解，则不会激活故障转移。

- **多个故障转移组**

  可为同一对服务器配置多个故障转移组以控制故障转移规模。 每个组均独立进行故障转移。 如果多租户应用程序使用弹性池，则可使用此功能来混合每个池的主数据库和辅助数据库。 采用这种方式可将服务中断的影响范围缩小到一半的租户中。

## <a name="best-practices-of-using-failover-groups-for-business-continuity"></a>使用故障转移组实现业务连续性的最佳做法

在设计具有业务连续性的服务时，应遵循以下准则：

- **使用一个或多个故障转移组来管理多个数据库的故障转移**

  可在不同区域的两个服务器（主服务器和辅助服务器）之间创建一个或多个故障转移组。 每组可包含一个或多个数据库，这些数据库是在所有或某些主数据库因主要区域中的服务中断而变得不可用时，作为单元恢复的。 故障转移组使用服务目标作为主数据库创建异地辅助数据库。 如果将现有的异地复制关系添加到故障转移组，请确保使用与主数据库相同的服务层和计算大小来配置异地辅助数据库。

- **使用读写侦听器处理 OLTP 工作负荷**

  执行 OLTP 操作时使用 &lt;failover-group-name&gt;.database.windows.net 作为服务器 URL，并将连接自动定向到主数据库。 此 URL 在故障转移后不会更改。 请注意，故障转移涉及更新 DNS 记录，以便仅在刷新客户端 DNS 缓存后，客户端连接才会重定向到新的主数据库。

- **使用只读侦听器处理只读工作负荷**

  如果你有一个在逻辑上隔离的只读工作负荷，且它允许存在一些过时数据，则可在应用程序中使用辅助数据库。 对于只读会话，使用 &lt;failover-group-name&gt;.secondary.database.windows.net 作为服务器 URL，并将连接自动定向到辅助数据库。 此外，还建议使用 **ApplicationIntent=ReadOnly** 在连接字符串中指示读取意向。

- **可应对性能下降的问题**

  SQL 故障转移决策与应用程序的其余部分或所用的其他服务无关。 应用程序可能是“mixed”与在一个区域和一些在另一些组件。 若要避免性能降低，确保 DR 区域中的冗余应用程序部署，请遵循本文中的准则。 请注意 DR 区域中的应用程序不必使用不同的连接字符串。  

- **可应对数据丢失的问题**

  如果检测到服务中断，则 SQL 会自动触发读写故障转移是否我们的知识的最佳零数据丢失。 否则，它会等待你指定的时期 **GracePeriodWithDataLossHours**。 如果指定了 **GracePeriodWithDataLossHours**，请为数据丢失做好准备。 一般情况下，在中断期间 Azure 倾向于可用性。 如果不能承受丢失数据，请确保设置 **GracePeriodWithDataLossHours** 到一个足够大的数字，如 24 小时。

> [!IMPORTANT]
> DTU 少于或等于 800、使用异地复制的数据库超过 250 个的弹性数据库池可能会遇到更长的计划故障转移和性能下降等问题。  这些问题更可能在写密集型工作负荷下发生，例如，异地复制终结点广泛分隔于各个地理位置，或者每个数据库使用多个辅助终结点。  当异地复制滞后随着时间推移增加时，这些问题的症状便会显现。  这种滞后可以使用 [sys.dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) 进行监视。  如果发生这些问题，缓解方法包括增加池 DTU 的数量或者减少同一池中异地复制数据库的数量。

## <a name="failover-groups-and-network-security"></a>故障转移组和网络安全

对于某些应用程序，安全规则要求只允许特定组件（如 VM、Web 服务等）通过网络访问数据层。此要求对业务连续性设计和故障转移组的使用提出了一些挑战。 在实施此类受限访问时，应考虑以下选项。

### <a name="using-failover-groups-and-virtual-network-rules"></a>使用故障转移组和虚拟网络规则

如果使用[虚拟网络服务终结点和规则](sql-database-vnet-service-endpoint-rule-overview.md)来限制对 SQL 数据库的访问，请注意每个虚拟网络服务终结点仅适用于一个 Azure 区域。 终结点不允许其他区域接受来自该子网的通信。 因此，只有部署在同一区域中的客户端应用程序才能连接到主数据库。 因为故障转移会导致 SQL 客户端会话重新路由到不同（次要）区域中的服务器，所以源自该区域之外的客户端的这些会话将失败。 因此，如果参与的服务器包含在虚拟网络规则中，则无法启用自动故障转移策略。 若要支持手动故障转移，请执行以下步骤：

1. 在次要区域中预配应用程序前端组件（Web 服务、虚拟机等）的冗余副本
2. 为主服务器和辅助服务器分别配置[虚拟网络规则](sql-database-vnet-service-endpoint-rule-overview.md)
3. 使用流量管理器配置启用[前端故障转移](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)
4. 检测到服务中断时启动手动故障转移

此选项针对需要在前端和数据层之间保持一致延迟的应用程序进行了优化，并支持在前端和/或数据层受到服务中断的影响时进行恢复。

> [!NOTE]
> 如果使用**只读侦听器**对只读工作负荷进行负载平衡，请确保在次要区域中的 VM 或其他资源中执行此工作负荷，以便它可以连接到辅助数据库。

### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>使用故障转移组和 SQL 数据库防火墙规则

如果业务连续性计划要求使用自动故障转移组进行故障转移，则可以使用传统防火墙规则限制对 SQL 数据库的访问。  若要支持自动故障转移，请执行以下步骤：

1. [创建公共 IP](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)
2. [创建公共负载均衡器](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-a-basic-load-balancer)并为其分配公共 IP。
3. 为前端组件[创建虚拟网络和虚拟机](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-back-end-servers)
4. [创建网络安全组](../virtual-network/security-overview.md)并配置入站连接。
5. 使用“Sql”[服务标记](../virtual-network/security-overview.md#service-tags)确保出站连接向 Azure SQL 数据库开放。
6. 创建 [SQL 数据库防火墙规则](sql-database-firewall-configure.md)，以允许来自步骤 1 中创建的公共 IP 地址的入站流量。

有关如何配置出站访问以及在防火墙规则中使用哪个 IP 的详细信息，请参阅[负载均衡器出站连接](../load-balancer/load-balancer-outbound-connections.md)。

上述配置将确保自动故障转移不会阻止来自前端组件的连接，并假定应用程序可以容忍前端与数据层之间的较长延迟。

> [!IMPORTANT]
> 若要保证区域服务中断的业务连续性，则必须确保前端组件和数据库的地理冗余。

## <a name="upgrading-or-downgrading-a-primary-database"></a>升级或降级主数据库

无需断开连接任何辅助数据库，即可将主数据库升级或降级到不同的计算大小（在相同的服务层中）。 升级时，建议先升级辅助数据库，再升级主数据库。 降级时，应反转顺序：先降级主数据库，再降级辅助数据库。 将数据库升级或降级到不同服务层时，将强制执行此建议操作。

> [!NOTE]
> 如果辅助数据库是作为故障转移组配置的一个部分创建的，则不建议对辅助数据库进行降级。 这是为了确保激活故障转移后，数据层有足够的容量来处理常规工作负荷。

## <a name="preventing-the-loss-of-critical-data"></a>防止丢失关键数据

由于广域网的延迟时间较长，连续复制使用了异步复制机制。 在发生故障时，异步复制会不可避免地丢失某些数据。 但是，某些应用程序可能要求不能有数据丢失。 为了保护这些关键更新，应用程序开发人员可以在提交事务后立即调用 [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) 系统过程。 调用 **sp_wait_for_database_copy_sync** 会阻止调用线程，直到将上次提交的事务传输到辅助数据库。 但是，它不会等待传输的事务提交到辅助数据库进行重播。 **sp_wait_for_database_copy_sync** 的作用域是一个具体的连续复制链路。 对主数据库具有连接权限的任何用户都可以调用此过程。

> [!NOTE]
> sp_wait_for_database_copy_sync 将在故障转移后防止数据丢失，但它不会保证读取访问的完全同步。 **sp_wait_for_database_copy_sync** 过程调用导致的延迟可能很明显，具体取决于调用时的事务日志大小。

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>以编程方式管理故障转移组和活动异地复制

如上所述，也可以使用 Azure PowerShell 和 REST API 以编程方式管理自动故障转移组和活动异地复制。 下表描述了可用的命令集。 活动异地复制包括一组用于管理的 Azure 资源管理器 API，其中包括 [Azure SQL 数据库 REST API](https://docs.microsoft.com/rest/api/sql/) 和 [Azure PowerShell cmdlet](https://docs.microsoft.com/powershell/azure/overview)。 这些 API 需要使用资源组，并支持基于角色的安全性 (RBAC)。 有关如何实现访问角色的详细信息，请参阅 [Azure 基于角色的访问控制](../role-based-access-control/overview.md)。

### <a name="manage-sql-database-failover-using-transact-sql"></a>使用 Transact-SQL 管理 SQL 数据库故障转移

| 命令 | Description |
| --- | --- |
| [ALTER DATABASE（Azure SQL 数据库）](/sql/t-sql/statements/alter-database-azure-sql-database) |使用 ADD SECONDARY ON SERVER 参数为现有数据库创建辅助数据库，并开始数据复制 |
| [ALTER DATABASE（Azure SQL 数据库）](/sql/t-sql/statements/alter-database-azure-sql-database) |使用 FAILOVER 或 FORCE_FAILOVER_ALLOW_DATA_LOSS 将辅助数据库切换为主数据库，启动故障转移 |
| [ALTER DATABASE（Azure SQL 数据库）](/sql/t-sql/statements/alter-database-azure-sql-database) |使用 REMOVE SECONDARY ON SERVER 终止 SQL 数据库和指定的辅助数据库之间的数据复制。 |
| [sys.geo_replication_links（Azure SQL 数据库）](/sql/relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database) |返回有关 Azure SQL 数据库逻辑服务器上每个数据库的所有现有复制链路的信息。 |
| [sys.dm_geo_replication_link_status（Azure SQL 数据库）](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database) |获取有关给定 SQL 数据库的复制链路的上次复制时间、上次复制滞后时间和其他信息。 |
| [sys.dm_operation_status（Azure SQL 数据库）](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) |显示所有数据库操作的状态，包括复制链路的状态。 |
| [sp_wait_for_database_copy_sync（Azure SQL 数据库）](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) |使应用程序等待，直到所有提交的事务已复制，并由活动辅助数据库确认。 |
|  | |

### <a name="manage-sql-database-failover-using-powershell"></a>使用 PowerShell 管理 SQL 数据库故障转移

| Cmdlet | Description |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |获取一个或多个数据库。 |
| [New-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |为现有数据库创建辅助数据库，并开始数据复制。 |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |将辅助数据库切换为主数据库，启动故障转移。 |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |终止 SQL 数据库和指定的辅助数据库之间的数据复制。 |
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |获取 Azure SQL 数据库和资源组或 SQL Server 之间的异地复制链路。 |
| [New-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |此命令会创建故障转移组，并将其同时注册到主服务器和辅助服务器|
| [Remove-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | 从服务器中移除故障转移组，并删除组中包含的所有辅助数据库 |
| [Get-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | 检索故障转移组配置 |
| [Set-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |修改故障转移组的配置 |
| [Switch-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | 触发故障转移组到辅助服务器的故障转移 |
|  | |

> [!IMPORTANT]
> 有关示例脚本，请参阅[配置单一数据库并使用活动异地复制对其进行故障转移](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)、[配置入池数据库并使用活动异地复制对其进行故障转移](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)和[针对单个数据库配置并故障转移一个故障转移组](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)。
>

### <a name="manage-sql-database-failover-using-the-rest-api"></a>使用 REST API 管理 SQL 数据库故障转移

| API | Description |
| --- | --- |
| [创建或更新数据库 (createMode=Restore)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |创建、更新或还原主数据库或辅助数据库。 |
| [获取创建或更新数据库状态](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) |返回创建操作过程中的状态。 |
| [将辅助数据库设为主数据库（计划的故障转移）](https://docs.microsoft.com/rest/api/sql/replicationlinks/replicationlinks_failover) |通过来自当前主要副本数据库的故障转移设置副本数据库。 |
| [将辅助数据库设为主数据库（未计划的故障转移）](https://docs.microsoft.com/rest/api/sql/replicationlinks/replicationlinks_failoverallowdataloss) |通过来自当前主要副本数据库的故障转移设置副本数据库。 此操作可能导致数据丢失。 |
| [获取复制链路](https://docs.microsoft.com/rest/api/sql/replicationlinks/replicationlinks_get) |获取异地复制合作关系中给定 SQL 数据库的特定复制链路。 它检索 sys.geo_replication_links 目录视图中可见的信息。 |
| [复制链接 - 按数据库列出](https://docs.microsoft.com/rest/api/sql/replicationlinks/replicationlinks_listbydatabase) | 获取异地复制合作关系中给定 SQL 数据库的所有复制链路。 它检索 sys.geo_replication_links 目录视图中可见的信息。 |
| [删除复制链接](https://docs.microsoft.com/rest/api/sql/replicationlinks/replicationlinks_delete) | 删除数据库复制链接。 无法在故障转移过程中完成。 |
| [创建或更新故障转移组](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_createorupdate) | 创建或更新故障转移组 |
| [删除故障转移组](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_delete) | 从服务器中删除故障转移组 |
| [故障转移（计划内）](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_failover) | 把故障从当前的主服务器转移到该服务器。 |
| [强制故障转移可能导致数据丢失](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_forcefailoverallowdataloss) |把故障从当前的主服务器转移到该服务器。 此操作可能导致数据丢失。 |
| [获取故障转移组](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_get) | 获取故障转移组。 |
| [服务器的故障转移组列表](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_listbyserver) | 列出服务器中的故障转移组。 |
| [更新故障转移组](https://docs.microsoft.com/rest/api/sql/failovergroups/failovergroups_update) | 更新故障转移组。 |
|  | |

## <a name="next-steps"></a>后续步骤

- 示例脚本请参阅：
  - [配置单一数据库并使用活动异地复制对其进行故障转移](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [配置入池数据库并使用活动异地复制对其进行故障转移](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
  - [针对单个数据库配置并故障转移一个故障转移组](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
- 有关业务连续性概述和应用场景，请参阅[业务连续性概述](sql-database-business-continuity.md)
- 若要了解 Azure SQL 数据库的自动备份，请参阅 [SQL 数据库自动备份](sql-database-automated-backups.md)。
- 若要了解如何使用自动备份进行恢复，请参阅[从服务启动的备份中还原数据库](sql-database-recovery-using-backups.md)。
- 若要了解新主服务器和数据库的身份验证要求，请参阅[灾难恢复后的 SQL 数据库安全性](sql-database-geo-replication-security-config.md)。
