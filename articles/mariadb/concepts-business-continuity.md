---
title: 业务连续性-Azure Database for MariaDB
description: 使用 Azure Database for MariaDB 服务时，了解业务连续性（时间点还原、数据中心中断、异地还原）。
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 76e749b26be5a5204b247d294f26da169f84094c
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74773011"
---
# <a name="understand-business-continuity-in-azure-database-for-mariadb"></a>了解 Azure Database for MariaDB 中的业务连续性

本文介绍 Azure Database for MariaDB 提供的用于业务连续性和灾难恢复的功能。 了解在发生破坏性事件后用于进行恢复的选项，破坏性事件可能导致数据丢失或者数据库和应用程序无法使用。 了解对一些情况的处理方式，包括用户或应用程序错误影响数据完整性、Azure 区域服务中断，或者应用程序需要维护。

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>可用来保证业务连续性的功能

Azure Database for MariaDB 提供了业务连续性功能，这包括自动备份和允许用户启动异地还原的功能。 每种功能在估计恢复时间 (ERT) 和可能丢失的数据方面都有不同的特性。 了解这些选项后，便可从中进行选择，可以针对不同方案将其搭配使用。 制定业务连续性计划时，需了解应用程序在破坏性事件发生后完全恢复前的最大可接受时间，即恢复时间目标 (RTO)。 此外，还需要了解从破坏性事件恢复时，应用程序可忍受丢失的最近数据更新（时间间隔）最大数量，即恢复点目标 (RPO)。

下表比较了各种可用功能的 ERT 和 RPO：

| **功能** | **基本** | **常规用途** | **内存优化** |
| :------------: | :-------: | :-----------------: | :------------------: |
| 从备份执行时间点还原 | 保留期内的任何还原点 | 保留期内的任何还原点 | 保留期内的任何还原点 |
| 从异地复制的备份执行异地还原 | 不支持 | ERT < 12 小时<br/>RPO < 1 小时 | ERT < 12 小时<br/>RPO < 1 小时 |

> [!IMPORTANT]
> 如果删除服务器，则该服务器中包含的所有数据库也会被删除且不可恢复。 无法还原已删除的服务器。

## <a name="recover-a-server-after-a-user-or-application-error"></a>在发生用户或应用程序错误之后恢复服务器

可以使用服务的备份在发生各种破坏性事件后对服务器进行恢复。 用户可能会不小心删除某些数据、无意中删除重要的表，甚至删除整个数据库。 应用程序可能因为自身缺陷，意外以错误数据覆盖正确数据，等等。

可以执行时间点还原来创建服务器在已知良好的时间点的副本。 此时间点必须在为服务器配置的备份保留期内。 在将数据还原到新服务器后，可以将原始服务器替换为新还原的服务器，或者将所需的数据从还原的服务器复制到原始服务器。

## <a name="recover-from-an-azure-regional-data-center-outage"></a>在 Azure 发生区域性数据中心中断后进行恢复

Azure 数据中心会罕见地发生中断。 发生中断时，可能仅导致业务中断持续几分钟，也可能持续数小时。

一个选项是等待数据中心中断结束时，服务器重新联机。 这适用于可以承受服务器脱机一段时间的应用程序，例如开发环境。 数据中心中断时，不知道中断会持续多久，因此该选项仅适用于暂时不需要服务器的情况。

另一个选项是使用 Azure Database for MariaDB 的异地还原功能，该功能使用异地冗余备份来还原服务器。 即使托管你的服务器的区域处于脱机状态，也可访问这些备份。 可以使用这些备份还原到任何其他区域并使服务器恢复联机。

> [!IMPORTANT]
> 只有当为服务器预配了异地冗余备份存储时，异地还原才是可行的。

## <a name="next-steps"></a>后续步骤

- 若要详细了解自动备份，请参阅 [Azure Database for MariaDB 中的备份](concepts-backup.md)。
- 若要使用 Azure 门户还原到某个时间点，请参阅 [使用 Azure 门户将数据库还原到某个时间点](howto-restore-server-portal.md)。

<!--
- To restore to a point in time using Azure CLI, see [restore database to a point in time using CLI](howto-restore-server-cli.md). 
-->
