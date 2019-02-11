---
title: Azure SQL 数据库基于 DTU 的资源限制 - 单一数据库 | Microsoft Docs
description: 本页介绍 Azure SQL 数据库中单一数据库的一些常见资源限制（基于 DTU）。
services: sql-database
ms.service: sql-database
ms.subservice: standalone-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: e73bb2f1908abd049ea6e2406c0d1db02381695f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2019
ms.locfileid: "55455535"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-based-purchasing-model"></a>使用基于 DTU 的购买模型的单一数据库的资源限制

本文提供了针对使用基于 DTU 的购买模型的 Azure SQL 数据库的单一数据库的详细资源限制。

有关弹性池的基于 DTU 的购买模型资源限制，请参阅[基于 DTU 的资源限制 - 弹性池](sql-database-vcore-resource-limits-elastic-pools.md)。 有关基于 vCore 的资源限制，请参阅[基于 vCore 的资源限制 - 单一数据库](sql-database-vcore-resource-limits-single-databases.md)和[基于 vCore 的资源限制 - 弹性池](sql-database-vcore-resource-limits-elastic-pools.md)。 有关不同购买模型的更多信息，请参阅[购买模型和服务层](sql-database-service-tiers.md)。

> [!IMPORTANT]
> 在某些情况下，可能需要收缩数据库来回收未使用的空间。 有关详细信息，请参阅[管理 Azure SQL 数据库中的文件空间](sql-database-file-space-management.md)。

## <a name="single-database-storage-sizes-and-compute-sizes"></a>单一数据库：存储大小和计算大小

下表显示了可用于每个服务层和计算大小的单一数据库的资源。 可通过 [Azure 门户](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server)、[Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-sql-database-servers-and-single-databases)、[PowerShell](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases)、[Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases) 或 [REST API](sql-database-single-databases-manage.md#rest-api-manage-sql-database-servers-and-single-databases) 为单一数据库设置服务层、计算大小和存储量。

### <a name="basic-service-tier"></a>基本服务层

| **计算大小** | **基本** |
| :--- | --: |
| 最大 DTU 数 | 5 |
| 包含的存储 (GB) | 2 |
| 最大存储选择 (GB) | 2 |
| 最大内存中 OLTP 存储 (GB) |不适用 |
| 最大并发工作线程数（请求数） | 30 |
| 最大并发会话数 | 300 |
|||

### <a name="standard-service-tier"></a>标准服务层

| **计算大小** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|
| 最大 DTU 数 | 10 | 20 | 50 | 100 |
| 包含的存储 (GB) | 250 | 250 | 250 | 250 |
| 最大存储选择 (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| 最大内存中 OLTP 存储 (GB) | 不适用 | 不适用 | 不适用 | 不适用 |
| 最大并发工作线程数（请求数）| 60 | 90 | 120 | 200 |
| 最大并发会话数 |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>标准服务层（续）

| **计算大小** | **S4** | **S6** | S7 | S9 | S12 |
| :--- |---:| ---:|---:|---:|---:|
| 最大 DTU 数 | 200 | 400 | 800 | 1600 | 3000 |
| 包含的存储 (GB) | 250 | 250 | 250 | 250 | 250 |
| 最大存储选择 (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| 最大内存中 OLTP 存储 (GB) | 不适用 | 不适用 | 不适用 | 不适用 |不适用 |
| 最大并发工作线程数（请求数）| 400 | 800 | 1600 | 3200 |6000 |
| 最大并发会话数 |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>高级服务层

| **计算大小** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** |
| :--- |---:|---:|---:|---:|---:|---:|
| 最大 DTU 数 | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| 包含的存储 (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| 最大存储选择 (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| 最大内存中 OLTP 存储 (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| 最大并发工作线程数（请求数）| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| 最大并发会话数 | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||

> [!IMPORTANT]
> 除以下区域外，其他所有区域的高级层目前均可提供超过 1 TB 的存储：中国东部、中国北部、德国中部、德国东北部、英国北部、英国南部、US DOD 中部、US DOD 东部、美国政府中部和美国中西部。  在这些区域，高级层中的最大存储限制为 1 TB。 请参阅 [P11-P15 当前限制](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb)。  

## <a name="single-database-change-storage-size"></a>单一数据库：更改存储大小

- 单一数据库的 DTU 价格附送了一定容量的存储，无需额外费用。 超出附送的量后，可花费额外的费用预配额外的存储，但不能超过存储上限，不超过 1 TB 时，以 250 GB 为增量进行预配，超出 1 TB 时，以 256 GB 为增量进行预配。 有关包括的存储量和大小上限，请参阅[单一数据库：存储大小和计算大小](#single-database-storage-sizes-and-compute-sizes)。
- 可通过 [Azure portal](sql-database-single-database-scale.md#dtu-based-purchasing-model-change-storage-size)、[Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples)、[PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase)、[Azure CLI](/cli/azure/sql/db#az-sql-db-update) 或 [REST API](https://docs.microsoft.com/rest/api/sql/databases/update) 为单一数据库增加大小上限，以预配额外存储。
- 单一数据库的额外存储价格等于额外存储量乘以服务层的额外存储单价。 有关额外存储价格的详细信息，请参阅 [SQL 数据库定价](https://azure.microsoft.com/pricing/details/sql-database/)。

## <a name="single-database-change-dtus"></a>单一数据库：更改 DTU

首先选择服务层、计算大小和存储量，然后使用 [Azure 门户](sql-database-single-database-scale.md#dtu-based-purchasing-model-change-compute-resources-dtus)、[Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples)、[PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase)、[Azure CLI](/cli/azure/sql/db#az-sql-db-update) 或 [REST API](https://docs.microsoft.com/rest/api/sql/databases/update)，根据实际体验动态扩展或缩减单一数据库。

下面的视频演示了如何动态更改服务层和计算大小以增加单一数据库的可用 DTU。

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

更改数据库的服务层和/或计算大小将以新的计算大小创建原始数据库的副本，并将连接切换到副本。 当我们切换到副本时，在此过程中不会丢失任何数据，但在短暂的瞬间，将禁用与数据库的连接，因此可能回滚某些处于进行状态的事务。 用于切换的时间长度因情况而异，但 99% 的情况下少于 30 秒。 如果在禁用连接的那一刻有大量的事务正在进行，则用于切换的时间长度可能会更长。

整个扩展过程的持续时间同时取决于更改前后数据库的大小和服务层。 例如，一个正在更改到标准服务层、从标准服务层更改或在标准服务层内更改的 250 GB 的数据库应在六小时内完成。 如果数据库与正在高级服务层内更改计算大小的大小相同，应在三小时内完成扩展。

> [!TIP]
> 要监视进行中的操作，请参阅：[使用 SQL REST API 管理操作](https://docs.microsoft.com/rest/api/sql/databaseoperations/listbydatabase
)、[使用 CLI 管理操作](/cli/azure/sql/db/op)、[使用 T-SQL 监视操作](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database)以及这两个 PowerShell 命令：[Get AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) 和 [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity)。

- 如果要升级到更高的服务层或计算大小，除非显式指定了更大的大小（最大），否则，最大数据库大小不会增大。
- 若要对数据库进行降级，数据库所用空间必须小于目标服务层和计算大小允许的最大大小。
- 从高级层降级至标准层时，如果同时满足 (1) 目标计算大小支持该数据库的最大大小，(2) 最大大小超出目标计算大小包括的存储量，那么将产生额外存储费用。 例如，如果将最大大小为 500 GB 的 P1 数据库缩小至 S3，那么将产生额外的存储费用，因为 S3 支持的最大大小为 500 GB，而它的附送存储量仅为 250 GB。 因此，额外存储量为 500 GB – 250 GB = 250 GB。 有关额外存储定价的信息，请参阅 [SQL 数据库定价](https://azure.microsoft.com/pricing/details/sql-database/)。 如果实际使用的空间量小于附送的存储量，只要将数据库最大大小减少到附送的量，就能避免此项额外费用。
- 在启用了[异地复制](sql-database-geo-replication-portal.md)的情况下升级数据库时，请先将其辅助数据库升级到所需的服务层和计算大小，然后再升级主数据库（用于实现最佳性能的常规指南）。 在升级到另一版本时，必须首先升级辅助数据库。
- 在启用了[异地复制](sql-database-geo-replication-portal.md)的情况下降级数据库时，请先将其主数据库降级到所需的服务层和计算大小，然后再降级辅助数据库（用于实现最佳性能的常规指南）。 在降级到另一版本时，必须首先降级主数据库。
- 各服务层的还原服务不同。 如果要降级到基本层，则备份保持期也将缩短。 请参阅 [Azure SQL 数据库备份](sql-database-automated-backups.md)。
- 所做的更改完成之前不会应用数据库的新属性。

## <a name="single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>单一数据库：当最大大小超过 1 TB 时，P11 和 P15 的限制

对于最大大小超过 1 TB 的 P11 和 P15 数据库，存在以下注意事项和限制：

- 如果在创建数据库时选择超过 1 TB 的最大大小（使用值 4 TB 或 4096 GB），如果在不受支持的区域中预配数据库，则 create 命令将会失败并出错。
- 对于位于一个受支持区域中的 P11 和 P15 数据库，可以以 256 GB 为增量，将其最大存储增至 1 TB 以上，最高可达 4 TB。 要了解所在区域是否支持更大的大小，请使用 [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) 功能，或在 Azure 门户中查看数据库大小。 升级现有 P11 或 P15 数据库只能由服务器级主体登录名或 dbmanager 数据库角色的成员执行。
- 如果在受支持的区域中执行升级操作，则配置会立即更新。 在升级过程中，数据库将保持联机。 但是，在实际的数据库文件已升级到新的最大大小之前，无法利用超过 1 TB 的所有存储量。 所需的时间长度取决于要升级的数据库的大小。
- 创建或更新 P11 或 P15 数据库时，只能在增量为 256 GB 的 1 TB 和 4 TB 最大大小之间选择。 创建 P11/P15 数据库时，系统已预先选择了默认的 1TB 存储选项。 对于位于某个受支持区域中的数据库，可将新的或现有单一数据库的存储上限增加到 4 TB。 对于所有其他区域，无法将最大大小增大到 1 TB 以上。 选择 4 TB 的随附存储时，价格不会更改。
- 如果将数据库的最大大小设置为 1 TB 以上，则即使使用的实际存储不到 1 TB，也无法将其更改为 1 TB。 因此，无法将最大大小超过 1 TB 的 P11 或 P15 降级为 1 TB P11、1 TB P15 或更低的计算大小，例如 P1-P6）。 此限制也适用于还原和复制方案，包括时间点、异地还原、长期备份保留以及数据库复制。 将数据库的最大大小配置为超过 1 TB 后，此数据库的所有还原操作都必须适合最大大小高于 1 TB 的 P11/P15。
- 对于“活动异地复制”方案：
  - 设置异地复制关系：如果主数据库是 P11 或 P15，则辅助数据库也必须为 P11 或 P15，更低的计算大小会被拒绝作为辅助数据库，因为它们不能支持超过 1 TB。
  - 升级异地复制关系中的主数据库：在主数据库上将最大大小更改为超过 1 TB 将触发辅助数据库上的相同更改。 这两个升级都必须成功才能使主数据库上的更改生效。 超过 1 TB 选项的区域限制适用。 如果辅助数据库位于不支持超过 1 TB 的区域，则不会升级主数据库。
- 不支持将导入/导出服务用于加载超过 1 TB 的 P11/P15 数据库。 使用 SqlPackage.exe 可[导入](sql-database-import.md)和[导出](sql-database-export.md)数据。

## <a name="next-steps"></a>后续步骤

- 有关常见问题的解答，请参阅 [SQL 数据库常见问题解答](sql-database-faq.md)。
- 有关服务器和订阅级别限制的信息，请参阅 [SQL 数据库服务器资源限制概述](sql-database-resource-limits-database-server.md)。
- 有关常规 Azure 限制的相关信息，请参阅 [Azure 订阅和服务限制、配额和约束](../azure-subscription-service-limits.md)。
- 有关 DTU 和 eDTU 的信息，请参阅 [DTU 和 eDTU](sql-database-service-tiers.md#dtu-based-purchasing-model)。
- 有关 tempdb 大小限制的信息，请参阅 [SQL 数据库 tempdb 限制](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database)。
