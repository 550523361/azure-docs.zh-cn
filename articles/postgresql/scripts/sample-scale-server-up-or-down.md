---
title: Azure CLI 脚本 - 缩放 Azure Database for PostgreSQL
description: Azure CLI 脚本示例 - 在查询指标后用于 PostgreSQL 服务器的 Azure 数据库缩放为不同的性能级别。
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 04/05/2018
ms.openlocfilehash: 2602165a6fb5aea85383d4e435479f0bf0f7e48c
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582099"
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>使用 Azure CLI 监视和缩放单个 PostgreSQL 服务器
此示例 CLI 脚本在查询指标后用于 PostgreSQL 服务器的单个 Azure 数据库缩放为不同的性能级别。 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

如果选择在本地运行 CLI，本文要求使用 Azure CLI 2.0 或更高版本。 通过运行 `az --version` 来查看版本。 请参阅[安装 Azure CLI]( /cli/azure/install-azure-cli)，了解如何安装或升级 Azure CLI 的版本。

## <a name="sample-script"></a>示例脚本
在此示例脚本中，编辑突出显示的行，将管理员用户名和密码更新为你自己的。 将 `az monitor` 命令中使用的 SubscriptionID 替换为自己的订阅 ID。
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>清理部署
运行脚本示例后，请使用以下命令删除资源组以及与其关联的所有资源。 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>脚本说明
此脚本使用下表中列出的命令：

| **命令** | **说明** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | 创建用于存储所有资源的资源组。 |
| [az postgres server create](/cli/azure/postgres/server#az_postgres_server_create) | 创建托管数据库的 PostgreSQL 服务器。 |
| [az monitor metrics list](/cli/azure/monitor/metrics#az_monitor_metrics_list) | 列出资源的指标值。 |
| [az group delete](/cli/azure/group#az_group_delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤
- 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/azure)
- 尝试其他脚本：[Azure Database for PostgreSQL 的 Azure CLI 示例](../sample-scripts-azure-cli.md)
- 有关缩放的详细信息，请参阅[服务层](../concepts-service-tiers.md)和[计算单元和存储单元](../concepts-compute-unit-and-storage.md)
