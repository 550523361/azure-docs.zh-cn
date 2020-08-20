---
title: 只读副本 - Azure Database for PostgreSQL（单一服务器）
description: 本文介绍 Azure Database for PostgreSQL（单一服务器）中的只读副本功能。
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/10/2020
ms.openlocfilehash: 608740ea52cf82485bae073d9679107ac52baa28
ms.sourcegitcommit: cd0a1ae644b95dbd3aac4be295eb4ef811be9aaa
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2020
ms.locfileid: "88611120"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL（单一服务器）中的只读副本

使用只读副本功能可将数据从 Azure Database for PostgreSQL 服务器复制到只读服务器。 可将主服务器中的数据复制到最多 5 个副本。 副本是使用 PostgreSQL 引擎的本机复制技术以异步方式更新的。

副本是新的服务器，可以像管理普通的 Azure Database for PostgreSQL 服务器一样对其进行管理。 每个只读副本按照预配计算资源的 vCore 数量以及每月 GB 存储量计费。

了解如何[创建和管理副本](howto-read-replicas-portal.md)。

## <a name="when-to-use-a-read-replica"></a>何时使用只读副本
只读副本功能可帮助改善读取密集型工作负荷的性能与规模。 读取工作负载可以与副本服务器隔离，而写入工作负载可以定向到主服务器。

常见方案是让 BI 和分析工作负载将只读副本用作报告的数据源。

由于副本是只读的，它们不能直接缓解主服务器上的写入容量负担。 此功能并非面向写入密集型工作负荷。

只读副本功能使用 PostgreSQL 本机异步复制。 该功能不适用于同步复制方案。 主服务器与副本之间存在明显的延迟。 副本上的数据最终将与主服务器上的数据保持一致。 对于能够适应这种延迟的工作负荷，可以使用此功能。

## <a name="cross-region-replication"></a>跨区域复制
可以在与主服务器不同的区域中创建只读副本。 跨区域复制对于灾难恢复规划或使数据更接近用户等方案非常有用。

>[!NOTE]
> 基本层服务器仅支持相同区域复制。

可以在任何 [Azure Database for PostgreSQL 区域](https://azure.microsoft.com/global-infrastructure/services/?products=postgresql)中设置主服务器。 主服务器可以在其配对区域或通用副本区域中拥有副本。 下图显示了哪些副本区域可用，具体取决于你的主区域。

[ ![只读副本区域](media/concepts-read-replica/read-replica-regions.png)](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>通用副本区域
无论主服务器位于何处，始终可以在以下任何区域中创建读取副本。 下面是通用副本区域：

澳大利亚东部、澳大利亚东南部、美国中部、东亚、美国东部、美国东部2、日本东部、日本西部、韩国中部、韩国南部、美国中北部、北欧、美国中南部、东南亚、英国南部、英国西部、西欧、美国西部、美国西部2、美国中部。

### <a name="paired-regions"></a>配对区域
除通用副本区域外，还可以在主服务器的 Azure 配对区域中创建只读副本。 如果不了解区域的配对情况，可以通过 [Azure 配对区域文章](../best-practices-availability-paired-regions.md)了解详细信息。

如果你使用跨区域副本进行灾难恢复规划，建议你在配对区域而不是其他某个区域中创建副本。 配对区域可避免同时更新，并优先考虑物理隔离和数据驻留。  

需要考虑以下限制： 

* 区域可用性： Azure Database for PostgreSQL 在法国中部、阿拉伯联合酋长国北部和德国中部提供。 但是，它们的配对区域不可用。
    
* 单向对：某些 Azure 区域仅在一个方向上配对。 这些区域包括印度西部、巴西南部。 
   这意味着印度西部的主服务器可以在印度南部创建副本。 但是，印度南部的主服务器无法在印度西部创建副本。 这是因为印度西部的次要区域是印度南部，但印度南部的次要区域不是印度西部。


## <a name="create-a-replica"></a>创建副本
启动“创建副本”工作流时，将创建空白的 Azure Database for PostgreSQL 服务器。 新服务器中填充了主服务器上的数据。 创建时间取决于主服务器上的数据量，以及自上次每周完整备份以来所经历的时间。 具体所需时间从几分钟到几小时不等。

每个副本都启用了存储[自动增长](concepts-pricing-tiers.md#storage-auto-grow)。 自动增长功能允许副本与复制到它的数据保持同步，并防止由于存储空间不足错误而导致的复制中断。

只读副本功能使用 PostgreSQL 的物理复制，而不使用逻辑复制。 使用复制槽位的流复制是默认的操作模式。 必要时，使用日志传送来跟上进度。

了解如何[在 Azure 门户中创建只读副本](howto-read-replicas-portal.md)。

## <a name="connect-to-a-replica"></a>连接到副本
创建副本时，该副本不会继承主服务器的防火墙规则或 VNet 服务终结点。 必须单独为副本设置这些规则。

副本从主服务器继承其管理员帐户。 主服务器上的所有用户帐户将复制到只读副本。 只能使用主服务器上可用的用户帐户连接到只读副本。

可以使用主机名和有效的用户帐户连接到副本，就像在常规的 Azure Database for PostgreSQL 服务器上连接一样。 对于名称为 **my replica**、管理员用户名为 **myadmin** 的服务器，可以使用 psql 连接到副本：

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

在提示符下，输入用户帐户的密码。

## <a name="monitor-replication"></a>监视复制
Azure Database for PostgreSQL 提供了两个用于监视复制的指标。 这两个指标是**副本的最大滞后时间**和**副本滞后时间**。 若要了解如何查看这些指标，请参阅[只读副本操作指南文章](howto-read-replicas-portal.md)的“监视副本”部分。

“副本的最大滞后时间”指标显示主服务器与滞后时间最长的副本之间的滞后时间（以字节为单位）。 此指标仅适用于主服务器。

“副本滞后时间”指标显示的是自上次重放事务以来所经历的时间。 如果主服务器上未发生任何事务，则该指标会反映此滞后时间。 此指标仅适用于副本服务器。 “副本滞后时间”是从 `pg_stat_wal_receiver` 视图计算得出的：

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp());
```

请设置警报，以便在副本滞后时间达到工作负荷不可接受的值时收到通知。 

若要获取更多见解，请直接查询主服务器，以获取所有副本上的复制滞后情况（以字节为单位）。

在 PostgreSQL 版本 10 中：

```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

在 PostgreSQL 9.6 和更低版本中：

```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> 如果主服务器或只读副本重启，“副本滞后时间”指标中会反映重启以及跟上进度所花费的时间。

## <a name="stop-replication"></a>停止复制
可以停止主服务器与副本之间的复制。 停止操作会导致副本重启，并删除其复制设置。 在主服务器与只读副本之间停止复制后，副本将成为独立服务器。 独立服务器中的数据是启动“停止复制”命令时副本上可用的数据。 独立服务器与主服务器不同步。

> [!IMPORTANT]
> 独立服务器不能再次成为副本。
> 在只读副本上停止复制之前，请确保副本包含所需的全部数据。

停止复制后，副本会丢失指向其以前的主服务器和其他副本的所有链接。

了解如何[停止复制到副本](howto-read-replicas-portal.md)。

## <a name="failover"></a>故障转移
在主服务器与副本服务器之间无法自动进行故障转移。 

由于复制是异步的，因此在主体和副本之间存在延迟。 延迟程度受许多因素影响，例如，在主服务器上运行的工作负荷有多大，以及数据中心之间的延迟有多严重。 大多数情况下，副本验证在几秒钟到几分钟之间。 可以使用“副本延迟”指标来跟踪实际的副本延迟，该指标适用于每个副本。 该指标显示的是自上次重播事务以来所经历的时间。 建议观察一段时间的副本延迟，以便确定平均延迟。 可以针对副本延迟设置警报，这样，当它超出预期范围时，你就可以采取行动。

> [!Tip]
> 如果故障转移到副本，则取消副本与主体之间的链接时遇到的延迟会指示丢失了多少数据。

一旦决定要故障转移到某个副本， 

1. 请停止将数据复制到副本<br/>
   此步骤是使副本服务器能够接受写入所必需的。 在此过程中，副本服务器会重启并取消与主体的链接。 启动停止复制的操作后，后端进程通常需要大约 2 分钟才能完成。 请参阅本文的[停止复制](#stop-replication)部分，了解此操作的潜在影响。
    
2. 将应用程序指向（以前的）副本<br/>
   每个服务器都有唯一的连接字符串。 更新应用程序，使之指向（以前的）副本而不是主体。
    
如果应用程序成功处理了读取和写入操作，则表明故障转移已完成。 应用程序经历的停机时间取决于何时检测到问题并完成上面的步骤 1 和 2。


## <a name="considerations"></a>注意事项

本部分汇总了有关只读副本功能的注意事项。

### <a name="prerequisites"></a>先决条件
只读副本和[逻辑解码](concepts-logical.md)都依赖 Postgres 预写日志 (WAL) 来获取信息。 这两个功能需要来自 Postgres 的不同级别的日志记录。 逻辑解码需要的日志记录的级别比只读副本需要的更高。

若要配置正确的日志记录级别，请使用 Azure 复制支持参数。 Azure 复制支持有三个设置选项：

* **关闭** - 在 WAL 中包含最少的信息。 大多数 Azure Database for PostgreSQL 服务器上都不提供此设置。  
* **副本** - 比“关闭”详细。 这是运行[只读副本](concepts-read-replicas.md)所需的最低日志记录级别。 此设置是大多数服务器上的默认设置。
* **逻辑** - 比“副本”详细。 这是运行逻辑解码所需的最低日志记录级别。 使用此设置时，只读副本也可以运行。

更改此参数后，需要重启服务器。 在内部，此参数设置 Postgres 参数 `wal_level`、`max_replication_slots` 和 `max_wal_senders`。

### <a name="new-replicas"></a>新副本
只读副本创建为新的 Azure Database for PostgreSQL 服务器。 无法将现有的服务器设为副本。 无法创建另一个只读副本的副本。

### <a name="replica-configuration"></a>副本配置
使用与主服务器相同的计算和存储设置创建副本。 创建副本后，可以更改多个设置，包括存储和备份保留期。

创建副本时或之后，防火墙规则、虚拟网络规则和参数设置不会从主服务器继承到副本服务器。

### <a name="scaling"></a>扩展
缩放 Vcore 或常规用途与内存优化：
* PostgreSQL 要求 `max_connections` 辅助服务器上的设置 [大于或等于主服务器上的设置](https://www.postgresql.org/docs/current/hot-standby.html)，否则，辅助服务器将无法启动。
* 在 Azure Database for PostgreSQL 中，每台服务器允许的最大连接数已固定到计算 sku，因为连接占用了内存。 你可以了解 [max_connections 和计算 sku 之间的映射](concepts-limits.md)的详细信息。
* **向上缩放**：首先向上扩展副本的计算，然后向上扩展主副本。 此顺序可防止错误违反 `max_connections` 要求。
* **缩小**：首先缩小主副本的计算，然后缩小副本。 如果尝试缩放低于主副本的副本，则会出现错误，因为这违反了 `max_connections` 要求。

缩放存储：
* 所有副本都启用了存储自动增长，以防止从存储空间的副本复制问题。 无法禁用此设置。
* 你还可以手动扩展存储，就像在任何其他服务器上一样


### <a name="basic-tier"></a>基本层
基本层服务器仅支持相同区域复制。

### <a name="max_prepared_transactions"></a>max_prepared_transactions
[PostgreSQL 要求](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAX-PREPARED-TRANSACTIONS)只读副本上的 `max_prepared_transactions` 参数值大于或等于主服务器上的值，否则副本不会启动。 如果要更改主服务器上的 `max_prepared_transactions`，请先在副本上进行相应更改。

### <a name="stopped-replicas"></a>停止的副本
如果停止主服务器与只读副本之间的复制，副本会重启以应用更改。 已停止的副本将成为可接受读取和写入的独立服务器。 独立服务器不能再次成为副本。

### <a name="deleted-master-and-standalone-servers"></a>删除的主服务器和独立服务器
删除主服务器后，其所有只读副本将成为独立服务器。 副本将会重启以反映此项更改。

## <a name="next-steps"></a>后续步骤
* 了解如何[在 Azure 门户中创建和管理只读副本](howto-read-replicas-portal.md)。
* 了解如何[通过 Azure CLI 和 REST API 创建和管理只读副本](howto-read-replicas-cli.md)。
