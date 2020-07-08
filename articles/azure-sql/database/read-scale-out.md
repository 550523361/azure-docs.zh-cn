---
title: 副本上的读取查询
description: 使用 Azure SQL 数据库，可以使用只读副本的容量（称为读取横向扩展）对只读工作负荷进行负载均衡。
services: sql-database
ms.service: sql-db-mi
ms.subservice: high-availability
titleSuffix: Azure SQL Database & SQL Managed Instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
ms.date: 06/03/2019
ms.openlocfilehash: a043eb5eed8f5554e42a113a3d7963be94da8a49
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2020
ms.locfileid: "85985561"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads"></a>使用只读副本对只读的查询工作负荷进行负载均衡
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

作为[高可用性体系结构](high-availability-sla.md#premium-and-business-critical-service-tier-availability)的一部分，"高级" 和 "业务关键" 服务层中的每个数据库都是使用主副本和多个辅助副本自动预配的。 为次要副本预配的计算大小与主要副本相同。 使用*读取横向扩展*功能，你可以使用一个只读副本的容量（而不是共享读写副本）对 SQL 数据库只读工作负荷进行负载均衡。 这样一来，只读工作负荷将与主要读写工作负荷隔离，并且不会影响其性能。 该功能适用于包含逻辑隔离的只读工作负荷（例如分析）的应用程序。 在“高级”和“业务关键”服务层级中，应用程序可以使用此额外的容量获得性能优势，而无需额外付费。

如果至少创建了一个辅助副本，则还可以在超大规模服务层中使用读取横向扩展功能。 如果只读工作负荷需要的资源比一个辅助副本上可用的资源多，则可以使用多个辅助副本。 "基本"、"标准" 和 "常规用途" 服务层的高可用性体系结构不包含任何副本。 读取横向扩展功能在这些服务层中不可用。

下图演示了使用“业务关键”数据库的应用程序。

![只读副本](./media/read-scale-out/business-critical-service-tier-read-scale-out.png)

默认情况下，在新的 Premium、业务关键和超大规模数据库上启用 "读取横向扩展" 功能。 对于超大规模，默认情况下，为新数据库创建一个次要副本。 如果在 SQL 连接字符串中配置了 `ApplicationIntent=ReadOnly`，则网关会将应用程序重定向到该数据库的只读副本。 有关如何使用属性的信息 `ApplicationIntent` ，请参阅[指定应用程序意向](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent)。

如果你希望确保应用程序始终连接到主要副本，而不管 SQL 连接字符串中的 `ApplicationIntent` 设置如何，则必须在创建数据库或更改其配置时显式禁用读取扩展。 例如，如果你将数据库从标准层或常规用途层升级到 Premium、业务关键或超大规模层，并且想要确保所有连接继续转到主副本，请禁用读取横向扩展。有关如何禁用它的详细信息，请参阅[启用和禁用读取横向扩展](#enable-and-disable-read-scale-out)。

> [!NOTE]
> 只读副本不支持查询数据存储、扩展事件和 SQL Profiler 功能。

## <a name="data-consistency"></a>数据一致性

副本的优势之一是，它始终处于事务一致性状态，但在不同的时间点，不同的副本之间可能会有一些较小的延迟。 读取横向扩展支持会话级一致性。 这意味着，如果只读会话在由于副本不可用而导致的连接错误后重新连接，则可能会将其重定向到与读写副本不是100% 的副本。 同样，如果应用程序使用读写会话写入数据，并立即使用只读会话读取该数据，则可能不会在副本上立即看到最新的更新。 延迟是由异步事务日志重做操作导致的。

> [!NOTE]
> 区域内的复制延迟较低，这种情况极少出现。

## <a name="connect-to-a-read-only-replica"></a>连接到只读副本

为数据库启用读取横向扩展时， `ApplicationIntent` 客户端提供的连接字符串中的选项会指示连接是路由到写入副本还是只读副本。 具体而言，如果 `ApplicationIntent` 值为 `ReadWrite`（默认值），则连接会定向到数据库的读写副本。 这与现有行为相同。 如果 `ApplicationIntent` 值为 `ReadOnly`，则连接将路由到只读副本。

例如，以下连接字符串将客户端连接到只读副本（请将尖括号中的项替换为环境的正确值，并删除尖括号）：

```sql
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

以下连接字符串之一将客户端连接到读写副本（请将尖括号中的项替换为环境的正确值，并删除尖括号）：

```sql
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="verify-that-a-connection-is-to-a-read-only-replica"></a>验证连接到只读副本

可通过运行以下查询来验证是否连接到只读副本。 当你连接到只读副本时，它将返回 READ_ONLY。

```sql
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```

> [!NOTE]
> 在任意给定时间，只读会话仅可访问其中一个 AlwaysON 副本。

## <a name="monitor-and-troubleshoot-a-read-only-replica"></a>监视只读副本并对其进行故障排除

连接到只读副本后，可以使用 `sys.dm_db_resource_stats` DMV 访问性能指标。 若要访问查询计划统计信息，请使用 `sys.dm_exec_query_stats`、`sys.dm_exec_query_plan` 和 `sys.dm_exec_sql_text` DMV。

> [!NOTE]
> 逻辑 master 数据库中的 DMV `sys.resource_stats` 返回主要副本的 CPU 使用率和存储数据。

## <a name="enable-and-disable-read-scale-out"></a>启用和禁用读取横向扩展

默认情况下，"高级"、"业务关键" 和 "超大规模" 服务层上已启用 "读取"。 无法在基本、标准或常规用途服务层中启用读取横向扩展。 在配置了零副本的超大规模数据库上，将自动禁用读取横向扩展。

您可以使用以下方法在高级服务层或业务关键服务层中的单一数据库和弹性池数据库上禁用并重新启用读取横向扩展。

> [!NOTE]
> 为了向后兼容，提供了禁用读取横向扩展功能的功能。

### <a name="azure-portal"></a>Azure 门户

你可以管理 "**配置**数据库" 边栏选项卡上的 "读取" 扩展设置。

### <a name="powershell"></a>PowerShell

> [!IMPORTANT]
> PowerShell Azure 资源管理器模块仍受支持，但所有将来的开发都适用于 Az .Sql 模块。 如果至少12月2020，Azure 资源管理器模块将继续收到 bug 修复。  Az 模块和 Azure 资源管理器模块中的命令参数完全相同。 有关其兼容性的详细信息，请参阅[新 Azure PowerShell Az Module 简介](/powershell/azure/new-azureps-module-az)。

管理 Azure PowerShell 中的读取横向扩展需要2016年12月 Azure PowerShell 版本或更高版本。 有关最新的 PowerShell 版本，请参阅 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)。

可以通过调用[AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) cmdlet 并传入参数的所需值（或），在 Azure PowerShell 中禁用或重新启用读取横向扩展 `Enabled` `Disabled` `-ReadScale` 。

若要对现有数据库禁用读取扩展（请将尖括号中的项替换为环境的正确值，并删除尖括号）：

```powershell
Set-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Disabled
```

若要对新数据库禁用读取扩展（请将尖括号中的项替换为环境的正确值，并删除尖括号）：

```powershell
New-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Disabled -Edition Premium
```

若要对现有数据库重新启用读取扩展（请将尖括号中的项替换为环境的正确值，并删除尖括号）：

```powershell
Set-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Enabled
```

### <a name="rest-api"></a>REST API

若要创建禁用了读取横向扩展的数据库，或要更改现有数据库的设置，请使用以下方法，并将 `readScale` 属性设置为 `Enabled` 或 `Disabled` ，如以下示例请求所示。

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body: {
   "properties": {
      "readScale":"Disabled"
   }
}
```

有关详细信息，请参阅[数据库-创建或更新](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)。

## <a name="using-tempdb-on-a-read-only-replica"></a>对只读副本使用 TempDB

TempDB 数据库不会复制到只读副本。 每个副本具有自身的 TempDB 数据库版本，该版本是创建该副本时创建的。 它可确保在执行查询的过程中更新和修改 TempDB。 如果只读工作负荷依赖于使用 TempDB 对象，则应创建这些对象作为查询脚本的一部分。

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>使用异地复制的数据库的读取横向扩展

如果使用 "读取横向扩展" 对异地复制的数据库（例如，作为故障转移组的成员）的只读工作负荷进行负载平衡，请确保在主要和异地复制的辅助数据库上启用 "读取" 扩展。 此配置可确保应用程序在故障转移后连接到新的主数据库时，相同的负载均衡体验能够继续。 

如果要连接到启用了读取规模的异地复制辅助数据库，则与 `ApplicationIntent=ReadOnly` 的会话将路由到其中一个副本，就像我们在主数据库上路由连接一样。  而未设为 `ApplicationIntent=ReadOnly` 的会话将路由到异地复制辅助数据库的主要副本，该副本也为只读。 因为异地复制的辅助数据库具有与主数据库不同的终结点，所以，在这种情况下，不需要设置它 `ApplicationIntent=ReadOnly` 。 为确保后向兼容性，`sys.geo_replication_links` DMV 显示`secondary_allow_connections=2`（允许的任何客户端连接）。

> [!NOTE]
> 不支持在辅助数据库的本地副本之间执行轮循机制或任何其他负载均衡路由。

## <a name="next-steps"></a>后续步骤

有关 SQL 数据库超大规模产品/服务的信息，请参阅[超大规模服务层](service-tier-hyperscale.md)。
