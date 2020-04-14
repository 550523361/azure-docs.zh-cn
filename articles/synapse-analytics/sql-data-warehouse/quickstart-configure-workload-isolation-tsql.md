---
title: 快速入门：配置工作负荷隔离 - T-SQL
description: 使用 T-SQL 配置工作负荷隔离。
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: d3d1b9af0b26fa775beb78b313937890cb9287b3
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633762"
---
# <a name="quickstart-configure-workload-isolation-using-t-sql"></a>快速入门：使用 T-SQL 配置工作负荷隔离

在本快速入门中，你将快速创建工作负荷组和分类器，以便保留资源以进行数据加载。 工作负荷组将向数据加载分配 20% 的系统资源。  工作负荷分类器将向数据加载工作负荷组分配请求。  它们对数据加载实现 20% 的隔离，这两项资源都必定满足 SLA 要求。

如果没有 Azure 订阅，请在开始之前创建一个[免费](https://azure.microsoft.com/free/)帐户。

> [!NOTE]
> 在 Azure Synapse Analytics 中创建一个 SQL Analytics 实例可能会产生一个新的计费服务。  有关详细信息，请参阅 [Azure Synapse Analytics 定价](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)。

## <a name="prerequisites"></a>先决条件

本快速入门假定你已在 Azure Synapse 中有 SQL Analytics 实例，并且具有 CONTROL DATABASE 权限。 如果需要创建一个 SQL 数据仓库，可使用[创建并连接 - 门户](create-data-warehouse-portal.md)创建名为“mySampleDataWarehouse”的数据仓库  。

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户

登录 [Azure 门户](https://portal.azure.com/)。

## <a name="create-login-for-dataloads"></a>为 DataLoads 创建登录名

使用 [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) 在 `master` 数据库中为“ELTLogin”创建 SQL Server 身份验证登录名。

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'ELTLogin')
BEGIN
CREATE LOGIN [ELTLogin] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user"></a>创建用户

在 mySampleDataWarehouse 中[创建用户](/sql/t-sql/statements/create-user-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)“ELTLogin”

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'ELTLogin')
BEGIN
CREATE USER [ELTLogin] FOR LOGIN [ELTLogin]
END
;
```

## <a name="create-a-workload-group"></a>创建工作负荷组

为隔离度为 20% 的 DataLoads 创建[工作负荷组](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)。

```sql
CREATE WORKLOAD GROUP DataLoads
WITH ( MIN_PERCENTAGE_RESOURCE = 20
      ,CAP_PERCENTAGE_RESOURCE = 100
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 5)
;
```

## <a name="create-a-workload-classifier"></a>创建工作负荷分类器

创建[工作负荷分类器](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)以将 ELTLogin 映射到 DataLoads 工作负荷组。

```sql
CREATE WORKLOAD CLASSIFIER [wgcELTLogin]
WITH (WORKLOAD_GROUP = 'DataLoads'
      ,MEMBERNAME = 'ELTLogin')
;
```

## <a name="view-existing-workload-groups-and-classifiers-and-run-time-values"></a>查看现有工作负荷组、分类器和运行时值

```sql
--Workload groups
SELECT * FROM
sys.workload_management_workload_groups

--Workload classifiers
SELECT * FROM
sys.workload_management_workload_classifiers

--Run-time values
SELECT * FROM
sys.dm_workload_management_workload_groups_stats
```

## <a name="clean-up-resources"></a>清理资源

```sql
DROP WORKLOAD CLASSIFIER [wgcELTLogin]
DROP WORKLOAD GROUP [DataLoads]
DROP USER [ELTLogin]
;
```

我们会针对数据仓库中存储的数据，按数据仓库单位收费。 这些计算和存储资源是分开计费的。

- 如果想要将数据保留在存储中，可以在不使用 SQL 池时暂停计算。 如果暂停计算资源，则你只需支付数据存储费用。 准备好处理数据时，可以恢复计算。
- 如果不想支付将来的费用，则可以删除数据仓库。

遵循以下步骤清理资源。

1. 登录到 [Azure 门户](https://portal.azure.com)，选择你的数据仓库。

    ![清理资源](./media/quickstart-configure-workload-isolation-tsql/clean-up-resources.png)

2. 若要暂停计算，请选择“暂停”  按钮。 暂停数据仓库后，可看到“启动”  按钮。  若要恢复计算，请选择“启动”  。

3. 若要删除数据仓库以免产生计算或存储费用，请选择“删除”  。

4. 要删除所创建的 SQL 服务器，请选择上图所示的“mynewserver-20180430.database.windows.net”，然后选择“删除”   。  请谨慎执行此删除操作，因为删除服务器的同时也会删除分配给该服务器的所有数据库。

5. 若要删除资源组，请选择“myResourceGroup”  ，然后选择“删除资源组”  。

## <a name="next-steps"></a>后续步骤

- 现已创建一个工作负荷组。 以 ELTLogin 身份运行一些查询，以查看其执行情况。 若要查看查询和分配的工作负荷组，请参阅 [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql/?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)。
- 有关 Synapse SQL 工作负载管理的详细信息，请参阅[工作负载管理](sql-data-warehouse-workload-management.md)和[工作负载隔离](sql-data-warehouse-workload-isolation.md)。
