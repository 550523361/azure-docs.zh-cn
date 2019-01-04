---
title: 将 Azure SQL 数据库备份存储 10 年之久 | Microsoft Docs
description: 了解 Azure SQL 数据库如何支持将完整数据库备份存储长达 10 年。
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 10/24/2018
ms.openlocfilehash: b1ef03b97f9fe95286d427effc40e69ae07b6b3c
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2018
ms.locfileid: "53601485"
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>将 Azure SQL 数据库备份存储 10 年之久

出于法规要求、符合性或其他商业目的，许多应用程序要求保留 Azure SQL 数据库的[自动备份](sql-database-automated-backups.md)功能提供的过去 7-35 天的数据库备份。 通过使用长期保留 (LTR) 功能，可以将指定的 SQL 数据库完整备份存储在 [RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) blob 存储中长达 10 年。 然后，可以将任何备份还原为新数据库。

> [!NOTE]
> 可以在 Azure SQL 数据库逻辑服务器中托管的数据库上启用 LTR。 它尚不可用于托管实例中托管的数据库。 可以使用 SQL 代理作业来安排[仅复制数据库备份](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server)作为超过 35 天的 LTR 的替代方案。
> 

## <a name="how-sql-database-long-term-retention-works"></a>SQL 数据库长期保留的工作原理

长期备份保留 (LTR) 利用[自动创建](sql-database-automated-backups.md)的完整数据库备份来启用时间点还原 (PITR)。 如果配置了 LTR 策略，则会将这些备份复制到不同的存储 blob。
可以为每个 SQL 数据库配置 LTR 策略并指定需要以何频率将备份复制到长期存储 blob。 若要实现该灵活性，可以使用以下四个参数的组合定义策略：每周备份保留 (W)、每月备份保留 (M)、每年备份保留 (Y) 和年中的周 (WeekOfYear)。 如果指定 W，则每周会将一个备份复制到长期存储。 如果指定 M，则会在每月的第一周将一个备份复制到长期存储。 如果指定 Y，则会在 WeekOfYear 指定的周将一个备份复制到长期存储。 每个备份将在长期存储中保留由这些参数指定的期限。 

示例：

-  W=0、M=0、Y=5、WeekOfYear=3

   每年的第 3 个完整备份将保留 5 年。
- W=0、M=3、Y=0

   每月的第一个完整备份将保留 3 个月。

- W=12、M=0、Y=0

   每个每周完整备份将保留 12 周。

- W=6、M=12、Y=10、WeekOfYear=16

   每个每周完整备份将保留 6 周。 每月的第一个完整备份例外，该备份将保留 12 个月。 每年的第 16 周创建的完整备份例外，该备份将保留 10 年。 

下表说明了以下策略的长期备份的节奏和到期：

W=12 周（84 天）、M=12 个月（365 天）、Y=10 年（3650 天）、WeekOfYear=15（4 月 15 日后的周）

   ![ltr 示例](./media/sql-database-long-term-retention/ltr-example.png)


 
如果你打算修改以上策略并设置 W=0（无每周备份），则备份副本的节奏将更改，如上表中突出显示的日期所示。 保留这些备份所需的存储量将相应减少。 

> [!NOTE]
1. LTR 副本是由 Azure 存储服务创建的，因此，复制过程对现有数据库的性能没有影响。
2. 将策略应用到将来的备份。 例如 如果配置策略时指定的 WeekOfYear 在过去，则将在明年创建第一个 LTR 备份。 
3. 若要从 LTR 存储还原数据库，可以根据时间戳选择一个特定备份。   数据库可以还原到原始数据库所在的订阅中的任何现有服务器。 
> 

## <a name="geo-replication-and-long-term-backup-retention"></a>异地复制和长期备份保留

如果使用活动异地复制或故障转移组作为业务连续性解决方案，则应为最终故障转移做准备，并在异地辅助数据库上配置相同的 LTR 策略。 这样做不会增加 LTR 存储成本，因为辅助数据库不会生成备份。 仅当辅助数据库成为主数据库时才会创建备份。 通过此方法，当触发故障转移并且主数据库变为辅助数据库时，可保证 LTR 备份生成不会中断。 

> [!NOTE]
当原始主数据库从导致其故障转移的中断中恢复时，它将成为新的辅助数据库。 因此，在该数据库再次变成主数据库前，备份生成将不会继续，现有 LTR 策略也不会生效。 
> 

## <a name="configure-long-term-backup-retention"></a>配置长期备份保留

若要了解如何使用 Azure 门户或 PowerShell 配置长期保留，请参阅[配置长期备份保留](sql-database-long-term-backup-retention-configure.md)。

## <a name="next-steps"></a>后续步骤

数据库备份可保护数据免遭意外损坏或删除，因此数据库备份是任何业务连续性和灾难恢复策略不可或缺的组成部分。 若要了解其他 SQL 数据库业务连续性解决方案，请参阅[业务连续性概述](sql-database-business-continuity.md)。
