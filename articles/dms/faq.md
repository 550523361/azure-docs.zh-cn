---
title: 有关使用 Azure 数据库迁移服务的常见问题解答 | Microsoft Docs
description: 有关使用 Azure 数据库迁移服务执行数据库迁移的常见问题。
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 12/19/2018
ms.openlocfilehash: b8001729c85c8447ab22bafffe24a32524a0662a
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/21/2018
ms.locfileid: "53714689"
---
# <a name="faq-about-using-the-azure-database-migration-service"></a>有关使用 Azure 数据库迁移服务的常见问题解答
本文列出了有关使用 Azure 数据库迁移服务的常见问题和相关解答。

### <a name="q-what-is-azure-database-migration-service"></a>问： 什么是 Azure 数据库迁移服务？
Azure 数据库迁移服务是一项完全托管的服务，旨在实现从多个数据库源到 Azure 数据平台的无缝迁移，并且最小化停机时间。 该服务目前已正式发布，目前正在进行的开发工作重点是：
- 可靠性和性能。
- 源目标对的迭代添加。
- 无摩擦迁移中的持续投资。

### <a name="q-what-source-target-pairs-does-the-azure-database-migration-service-currently-support"></a>问： Azure 数据库迁移服务目前支持哪些源-目标对？
该服务目前支持各种迁移方案。 有关每个可用迁移方案的状态的完整列表，请参阅文章 [Azure 数据库迁移服务支持的迁移方案的状态](https://docs.microsoft.com/azure/dms/resource-scenario-status)。 其他迁移方案处于有限预览状态，需要通过 DMS 预览站点提交提名。 有关有限预览中的方案的完整列表以及如何注册以参与其中一个产品/服务，请参阅 [DMS 预览站点](https://aka.ms/dms-preview/)。

### <a name="q-how-does-the-azure-database-migration-service-compare-to-other-microsoft-database-migration-tools-such-as-the-database-migration-assistant-dma-or-sql-server-migration-assistant-ssma"></a>问： Azure 数据库迁移服务与其他 Microsoft 数据库迁移工具（例如数据库迁移助手 (DMA) 或 SQL Server 迁移助手 (SSMA)）有何差别？
Azure 数据库迁移服务是将数据库大规模迁移到 Microsoft Azure 的首选方法。 有关 Azure 数据库迁移服务与其他 Microsoft 数据库迁移工具的差别，以及对各种方案使用不同服务的建议，请参阅博客文章 [Differentiating Microsoft’s Database Migration Tools and Services](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services/)（Microsoft 数据库迁移工具和服务的差别）。

### <a name="q-how-does-the-azure-database-migration-service-compare-to-the-azure-migrate-offering"></a>问： Azure 数据库迁移服务与 Azure 迁移产品有何差别？
Azure 迁移服务可以帮助将本地虚拟机迁移到 Azure IaaS。 该服务会评估是否适合迁移以及需要根据性能进行哪些大小调整，并提供在 Azure 中运行本地虚拟机的成本估算。 Azure Migrate 适合用于将基于本地 VM 的工作负荷直接迁移到 Azure IaaS VM。 但是，与 Azure 数据库迁移服务不同，Azure Migrate 不是适用于 Azure SQL 数据库、SQL Azure 或 Azure SQL 数据库托管实例等 Azure PaaS 关系数据库平台的专用数据库迁移服务产品。

### <a name="q-what-versions-of-sql-server-does-the-azure-database-migration-service-support-as-a-source"></a>问： Azure 数据库迁移服务支持将什么版本的 SQL Server 用作源？
从 SQL Server 迁移时，Azure 数据库迁移服务支持 SQL Server 2005 - SQL Server 2017 的所有版本。

### <a name="q-what-is-a-summary-of-the-steps-required-to-use-the-azure-database-migration-service-to-perform-a-database-migration"></a>问： 使用 Azure 数据库迁移服务执行数据库迁移所要执行的步骤摘要是什么？
在典型的简单数据库迁移过程中，需要：
1.  创建目标数据库。
2.  使用[数据库迁移助手](https://www.microsoft.com/en-us/download/details.aspx?id=53595)迁移数据库架构。
3.  创建 Azure 数据库迁移服务的实例。
4.  创建迁移项目，用于指定源数据库、目标数据库和要迁移的表。
5.  启动完整负载。
6.  选择后续验证。
7.  执行从生产环境到新的基于云的数据库的手动切换。 

### <a name="q-what-are-the-prerequisites-for-using-the-azure-database-migration-service"></a>问： 使用 Azure 数据库迁移服务的先决条件是什么？
若要确保 Azure 数据库迁移服务在执行数据库迁移时顺利运行，需要满足几个先决条件。 某些先决条件适用于该服务支持的所有方案（源/目标对），而其他先决条件则是特定方案所特有的。
在所有支持的迁移方案中通用的 Azure 数据库迁移服务先决条件包括需要：
- 使用 Azure 资源管理器部署模型创建 Azure 数据库迁移服务的 VNET，它将使用 [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) 或 [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) 为本地源服务器提供站点到站点连接。
- 确保 Azure 虚拟网络 (VNET) 网络安全组规则未阻止通信端口 443、53、9354、445、12000。 有关 Azure VNET NSG 流量筛选的更多详细信息，请参阅[使用网络安全组筛选网络流量](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)一文。
- 在源数据库的前面使用了防火墙设备时，可能需要添加防火墙规则以允许 Azure 数据库迁移服务访问要迁移的源数据库。
 
有关使用 Azure 数据库迁移服务完成特定迁移方案所要满足的所有先决条件列表，请参阅 docs.microsoft.com 上的 Azure 数据库迁移服务[文档](https://docs.microsoft.com/azure/dms/dms-overview)中的相关教程。

### <a name="q-how-do-i-find-the-ip-address-for-the-azure-database-migration-service-so-that-i-can-create-an-allow-list-for-the-firewall-rules-used-to-access-my-source-database-for-migration"></a>问： 如何查找 Azure 数据库迁移服务的 IP 地址，以便可以创建防火墙规则的允许列表用于访问要迁移的源数据库？
可能需要添加防火墙规则，以允许 Azure 数据库迁移服务访问要迁移的源数据库。 该服务的 IP 地址是动态的，但如果使用 Express Route，则企业网络会专门分配此地址。 标识相应 IP 地址的最简单方法是查看预配的 Azure 数据库迁移服务资源所在的同一个资源组，找到关联的网络接口。 通常，网络接口资源的名称以 NIC 前缀开头，后接唯一的字符和序号，例如 NIC-jj6tnztnmarpsskr82rbndyp。 选择此网络接口资源后，可以看到需要包含在 Azure 门户资源概述页上的允许列表中的 IP 地址。

可能还需要在允许列表中包含 SQL Server 侦听的端口源。 默认情况下为端口 1433，但源 SQL Server 可能也配置为侦听其他端口。 在这种情况下，也需要在允许列表中包含这些端口。 可以使用动态管理视图查询来确定 SQL Server 侦听的端口：

```sql
    SELECT DISTINCT 
        local_tcp_port 
    FROM sys.dm_exec_connections 
    WHERE local_tcp_port IS NOT NULL
```
也可以通过查询 SQL Server 错误日志来确定 SQL Server 侦听的端口：

```sql
    USE master
    GO
    xp_readerrorlog 0, 1, N'Server is listening on' 
    GO
```

### <a name="q-are-there-any-recommendations-for-optimizing-the-performance-of-the-azure-database-migration-service"></a>问： 在优化 Azure 数据库迁移服务的性能方面，你们是否提供任何建议？
可以采取几项措施来加快使用该服务迁移数据库的速度：
- 创建服务实例时使用多 CPU 常规用途定价层，使该服务可以利用多个 vCPU 来实现行化和加速数据传输。
- 在数据迁移操作期间，暂时将 Azure SQL 数据库目标实例纵向扩展到高级层 SKU，以尽量减少遇到使用较低级 SKU 时出现的 Azure SQL 数据库限制，从而避免对数据传输活动造成影响。

### <a name="q-how-do-i-set-up-an-azure-virtual-network"></a>问： 如何设置 Azure 虚拟网络？
有多篇 Microsoft 教程逐步讲解了如何完成设置 Azure VNET 的过程，另外，还可以参阅 [Azure 虚拟网络](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)一文中提到的官方文档。

### <a name="q-why-is-my-azure-database-migration-service-unavailable-or-stopped"></a>问： 为什么 Azure 数据库迁移服务不可用或已停止？
如果用户显式停止 Azure 数据库迁移服务 (DMS)，或者如果服务处于非活动状态已有 24 小时，则服务将处于已停止或自动暂停状态。 在上述每种情况下，服务将不可用并处于已停止状态。  若要恢复活动迁移，请重启该服务。

### <a name="q-where-can-i-leave-feedback-about-the-azure-database-migration-service"></a>问： 在哪里可以提供有关 Azure 数据库迁移服务的反馈？
我们希望收到读者的意见。 欢迎通过 [User Voice](https://feedback.azure.com/forums/906100-azure-database-migration-service) 发送有关 Azure 数据库迁移服务的任何反馈和看法。

## <a name="next-steps"></a>后续步骤
有关 Azure 数据库迁移服务和区域可用性的概述，请参阅[什么是 Azure 数据库迁移服务](dms-overview.md)一文。 
