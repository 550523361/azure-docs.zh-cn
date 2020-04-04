---
title: 升级到最新一代
description: 将 Azure Synapse 分析 SQL 池升级到最新一代的 Azure 硬件和存储体系结构。
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/19/2019
ms.author: martinle
ms.reviewer: jrasnick
ms.custom: seo-lt-2019
ms.openlocfilehash: 3299aa8ed85cff5c29d043d30aac08db45ffe5d4
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632275"
---
# <a name="optimize-performance-by-upgrading-azure-synapse-analytics-sql-pool"></a>通过升级 Azure 突触分析 SQL 池来优化性能

将 SQL 池升级到最新一代的 Azure 硬件和存储体系结构。

## <a name="why-upgrade"></a>为什么升级？

现在，您可以无缝升级到 Azure 门户中[支持区域](gen2-migration-schedule.md#automated-schedule-and-region-availability-table)的 SQL 池计算优化第 2 代层。 如果你的区域不支持自助升级，则可以升级到受支持的区域，或者等待你的区域支持自助升级。 请立即升级来利用最新一代 Azure 硬件和增强的存储体系结构，包括更快的性能、更高的可伸缩性以及无限的列式存储。

> [!VIDEO https://www.youtube.com/embed/9B2F0gLoyss]

> [!IMPORTANT]
> 此升级适用于[受支持区域](gen2-migration-schedule.md#automated-schedule-and-region-availability-table)中的计算优化第 1 代 SQL 池。

## <a name="before-you-begin"></a>在开始之前

1. 检查你的[区域](gen2-migration-schedule.md#automated-schedule-and-region-availability-table)是否支持从第 1 代到第 2 代迁移。 请注意自动迁移日期。 为了避免与自动化流程发生冲突，请在自动化流程开始日期之前计划手动迁移。
2. 如果你所在的区域尚不受支持，请继续检查你的区域是否已添加到受支持的区域中，或者[使用还原来升级](#upgrade-from-an-azure-geographical-region-using-restore-through-the-azure-portal)到受支持的区域。
3. 如果你的区域受支持，请[通过 Azure 门户进行升级](#upgrade-in-a-supported-region-using-the-azure-portal)
4. 使用以下映射，根据计算优化第 1 代层上的当前性能级别选择 SQL 池**的建议性能级别**：

   | “计算优化第 1 代”层级 | “计算优化第 2 代”层级 |
   | :-------------------------: | :-------------------------: |
   |            DW100            |           DW100c            |
   |            DW200            |           DW200c            |
   |            DW300            |           DW300c            |
   |            DW400            |           DW400c            |
   |            DW500            |           DW500c            |
   |            DW600            |           DW500c            |
   |           DW1000            |           DW1000c           |
   |           DW1200            |           DW1000c           |
   |           DW1500            |           DW1500c           |
   |           DW2000            |           DW2000c           |
   |           DW3000            |           DW3000c           |
   |           DW6000            |           DW6000c           |

> [!NOTE]
> 建议的性能级别不是直接转换。 例如，我们建议从 DW600 转换到 DW500c。

## <a name="upgrade-in-a-supported-region-using-the-azure-portal"></a>使用 Azure 门户在受支持的区域中进行升级

- 通过 Azure 门户从第 1 代迁移到第 2 代是永久性的。 无法通过任何流程返回到第 1 代。
- SQL 池必须运行才能迁移到第 2 代

### <a name="before-you-begin"></a>在开始之前

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

- 登录 [Azure 门户](https://portal.azure.com/)。
- 确保 SQL 池正在运行 - 必须迁移到第 2 代

### <a name="powershell-upgrade-commands"></a>电源外壳升级命令

1. 如果要升级的计算优化第 1 层 SQL 池已暂停，[请继续 SQL 池](pause-and-resume-compute-portal.md)。

2. 做好停机几分钟的准备。

3. 识别任何对计算优化第 1 代性能级别的代码引用，并将其修改为等效的计算优化第 2 代性能级别。 下面是升级前需要更新代码引用的两个示例：

   原始 Gen1 PowerShell 命令：

   ```powershell
   Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
   ```

   修改为：

   ```powershell
   Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300c"
   ```

   > [!NOTE]
   > -RequestedServiceObjectiveName“DW300”更改为 - RequestedServiceObjectiveName“DW300c****”
   >

   原始 Gen1 T-SQL 命令：

   ```SQL
   ALTER DATABASE mySampleDataWarehouse MODIFY (SERVICE_OBJECTIVE = 'DW300') ;
   ```

   修改为：

   ```sql
   ALTER DATABASE mySampleDataWarehouse MODIFY (SERVICE_OBJECTIVE = 'DW300c') ;
   ```

   > [!NOTE]
   > SERVICE_OBJECTIVE = “DW300”更改为 SERVICE_OBJECTIVE = “DW300c”****

## <a name="start-the-upgrade"></a>开始升级

1. 转到 Azure 门户中的计算优化第 1 代 SQL 池。 如果要升级的计算优化第 1 层 SQL 池已暂停，[请继续 SQL 池](pause-and-resume-compute-portal.md)。
2. 在"任务"选项卡下选择 **"升级到 Gen2"** 卡：Upgrade_1 ![](./media/upgrade-to-latest-generation/upgrade-to-gen2-1.png)

   > [!NOTE]
   > 如果在“任务”选项卡下未看到“升级到第 2 代”**** 卡，则你的订阅类型在当前区域中受到限制。
   > [提交支持票证](sql-data-warehouse-get-started-create-support-ticket.md)以便将你的订阅列入允许列表。

3. 在升级之前，请确保工作负荷已完成运行并处于静止状态。 在 SQL 池作为计算优化 Gen2 层 SQL 池重新联机之前，您将遇到几分钟的停机时间。 选择“升级”****：

   ![Upgrade_2](./media/upgrade-to-latest-generation/upgrade-to-gen2-2.png)

4. 通过在 Azure 门户中检查状态来**监视升级**：

   ![Upgrade3](./media/upgrade-to-latest-generation/upgrade-to-gen2-3.png)

   升级过程的第一个步骤将经历缩放操作（“升级 - 脱机”），其中，所有会话都将终止并且连接将会断开。

   升级过程的第二个步骤是数据迁移（“升级 - 联机”）。 数据迁移是一个联机缓慢执行的后台进程。 此进程使用本地 SSD 缓存，缓慢地将列式数据从旧的存储体系结构移动到新的存储体系结构。 在此期间，您的 SQL 池将联机以进行查询和加载。 你的数据将可供查询，无论它是否已迁移。 数据迁移以可变速率进行，具体取决于数据大小、性能级别和列存储段的数目。

5. **可选建议：** 缩放操作完成后，可以加快数据迁移后台进程。 可以通过以更大的 SLO 和资源类对要查询的所有主要列存储表运行 [Alter Index rebuild](sql-data-warehouse-tables-index.md) 来强制数据移动。 与缓慢执行的后台进程（可能需要数小时才能完成，具体取决于表的数量和大小）相对比，此操作是**脱机**的。 但是，一旦完成，数据迁移将更快，因为新的增强的存储体系结构具有高质量的行组。

> [!NOTE]
> Alter Index rebuild 是一项脱机操作，在重新生成完成之前，这些表将不可用。

以下查询会生成加快数据迁移时所需的 Alter Index Rebuild 命令：

```sql
SELECT 'ALTER INDEX [' + idx.NAME + '] ON ['
       + Schema_name(tbl.schema_id) + '].['
       + Object_name(idx.object_id) + '] REBUILD ' + ( CASE
                                                         WHEN (
                                                     (SELECT Count(*)
                                                      FROM   sys.partitions
                                                             part2
                                                      WHERE  part2.index_id
                                                             = idx.index_id
                                                             AND
                                                     idx.object_id =
                                                     part2.object_id)
                                                     > 1 ) THEN
              ' PARTITION = '
              + Cast(part.partition_number AS NVARCHAR(256))
              ELSE ''
                                                       END ) + '; SELECT ''[' +
              idx.NAME + '] ON [' + Schema_name(tbl.schema_id) + '].[' +
              Object_name(idx.object_id) + '] ' + (
              CASE
                WHEN ( (SELECT Count(*)
                        FROM   sys.partitions
                               part2
                        WHERE
                     part2.index_id =
                     idx.index_id
                     AND idx.object_id
                         = part2.object_id) > 1 ) THEN
              ' PARTITION = '
              + Cast(part.partition_number AS NVARCHAR(256))
              + ' completed'';'
              ELSE ' completed'';'
                                                    END )
FROM   sys.indexes idx
       INNER JOIN sys.tables tbl
               ON idx.object_id = tbl.object_id
       LEFT OUTER JOIN sys.partitions part
                    ON idx.index_id = part.index_id
                       AND idx.object_id = part.object_id
WHERE  idx.type_desc = 'CLUSTERED COLUMNSTORE';
```

## <a name="upgrade-from-an-azure-geographical-region-using-restore-through-the-azure-portal"></a>通过 Azure 门户使用还原从 Azure 地理区域进行升级

## <a name="create-a-user-defined-restore-point-using-the-azure-portal"></a>使用 Azure 门户创建用户定义的还原点

1. 登录 [Azure 门户](https://portal.azure.com/)。

2. 导航到要为其创建还原点的 SQL 池。

3. 在“概述”部分的顶部，选择“+ 新建还原点”****。

    ![新建还原点](./media/upgrade-to-latest-generation/creating_restore_point_0.png)

4. 为还原点指定一个名称。

    ![还原点的名称](./media/upgrade-to-latest-generation/creating_restore_point_1.png)

## <a name="restore-an-active-or-paused-database-using-the-azure-portal"></a>使用 Azure 门户还原活动或暂停的数据库

1. 登录 [Azure 门户](https://portal.azure.com/)。
2. 导航到要还原的 SQL 池。
3. 在“概述”部分的顶部，选择“还原”****。

    ![ 还原概述](./media/upgrade-to-latest-generation/restoring_0.png)

4. 选择**自动还原点**或**用户定义的还原点**。 对于用户定义的还原点，**请选择用户定义的还原点**或**创建新的用户定义的还原点**。 对于服务器，选择 **"新建"** 并选择 Gen2 支持的地理区域中的服务器。

    ![自动还原点](./media/upgrade-to-latest-generation/restoring_1.png)

## <a name="restore-from-an-azure-geographical-region-using-powershell"></a>使用 PowerShell 从 Azure 地理区域还原

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

若要恢复数据库，请使用 [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) cmdlet。

> [!NOTE]
> 可以执行到第 2 代的异地还原！ 若要执行此操作，请将一个第 2 代 ServiceObjectiveName（例如 DW1000**c**）指定为可选参数。

1. 打开 Windows PowerShell。
2. 连接到 Azure 帐户，并列出与帐户关联的所有订阅。
3. 选择包含要还原的数据库的订阅。
4. 获取要恢复的数据库。
5. 为数据库创建恢复请求，指定一个第 2 代 ServiceObjectiveName。
6. 验证异地还原的数据库的状态。

```Powershell
Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>" -RequestedServiceObjectiveName "DW300c"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> 若要在完成还原后配置数据库，请参阅 [在恢复后配置数据库](../../sql-database/sql-database-disaster-recovery.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#configure-your-database-after-recovery)。

如果源数据库启用了 TDE，则已恢复的数据库将启用 TDE。

如果 SQL 池遇到任何问题，请创建[支持请求](sql-data-warehouse-get-started-create-support-ticket.md)，并将"Gen2 升级"作为可能的原因。

## <a name="next-steps"></a>后续步骤

升级后的 SQL 池处于联机状态。 若要利用增强的体系结构，请参阅[用于工作负荷管理的资源类](resource-classes-for-workload-management.md)。
