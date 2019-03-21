---
title: 服务配额和限制 - Azure Batch | Microsoft Docs
description: 了解默认的 Azure Batch 配额、限制和约束，以及如何请求提高配额
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2018
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: f8941f84e85f330713cb4b263f2154acb80b520c
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2019
ms.locfileid: "57532387"
---
# <a name="batch-service-quotas-and-limits"></a>Batch 服务配额和限制

与其他 Azure 服务一样，与 Batch 服务关联的某些资源存在限制。 其中的许多限制是 Azure 在订阅或帐户级别应用的默认配额。 本文将描述这些默认值，以及如何请求提高配额。

设计和增加 Batch 工作负荷时，请记住这些配额。 例如，如果池没有达到指定的计算节点目标数量，那么可能是已达到 Batch 帐户的核心配额限制。

可以在单个批处理帐户中运行多个批处理工作负荷，或者在相同订阅的不同 Azure 区域的批处理帐户之间分散工作负荷。

如果打算在 Batch 中运行生产工作负荷，可能需要将一个或多个配额提高到默认值以上。 如果需要提高配额，可以免费提出在线[客户支持请求](#increase-a-quota)。

> [!NOTE]
> 配额是一种信用限制，不附带容量保证。 如果有大规模的容量需求，请联系 Azure 支持。
> 
> 

## <a name="resource-quotas"></a>资源配额
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]


### <a name="cores-quotas-in-user-subscription-mode"></a>用户订阅模式中的核心配额

如果创建了 Batch 帐户，并将池分配模式设置为“用户订阅”，则会以不同的方式应用配额。 在此模式下，会在创建池后直接在订阅中创建 Batch VM 和其他资源。 Azure Batch 核心配额不会应用到在此模式下创建的帐户。 对于此类帐户，将应用订阅中的区域计算核心数和其他资源的配额。 在 [Azure 订阅和服务的限制、配额和约束](../azure-subscription-service-limits.md)中详细了解这些配额。

## <a name="pool-size-limits"></a>池大小限制

| **资源** | **最大限制** |
| --- | --- |
| **[启用了节点间通信的池](batch-mpi.md)中的计算节点**  ||
| Batch 服务池分配模式 | 100 |
| Batch 订阅池分配模式 | 80 |
| **[使用自定义 VM 映像创建的池](batch-custom-images.md)中的计算节点**<sup>1</sup> ||
| 专用节点 | 2000 |
| 低优先级节点 | 1000 |

<sup>1</sup> 适用于未启用节点间通信的池。

## <a name="other-limits"></a>其他限制

| **资源** | **最大限制** |
| --- | --- |
| 每个计算节点的[并发任务](batch-parallel-node-tasks.md)数 | 4 x 节点核心数 |
| 每个 Batch 帐户的[应用程序](batch-application-packages.md)数 | 20 |
| 每个应用程序的应用程序包数 | 40 |
| 最长任务生存期 | 180 天<sup>1</sup> |

<sup>1</sup> 最长任务生存期（从添加到作业时算起到任务完成时结束）为 180 天。 已完成的任务保存 7 天；最长生存期内未完成的任务的数据不可访问。

## <a name="view-batch-quotas"></a>查看 Batch 配额

可在 [Azure 门户][portal]中查看批处理帐户配额。

1. 在门户中选择“Batch 帐户”，并选择所需的 Batch 帐户。
1. 在 Batch 帐户的菜单上选择“配额”。
1. 显示当前应用于 Batch 帐户的配额
   
    ![Batch 帐户配额][account_quotas]



## <a name="increase-a-quota"></a>提高配额

执行以下步骤，使用 [Azure 门户][portal]请求提高批处理帐户或订阅的配额。 可以提高哪种配额取决于批处理帐户的池分配模式。

### <a name="increase-a-batch-cores-quota"></a>提高批处理核心配额 

1. 在门户仪表板上选择“帮助 + 支持”磁贴，或单击门户右上角的问号 (**?**)。
1. 选择“新建支持请求” > “基本”。
1. 在“基本信息”中：
   
    a. “问题类型” > “配额”
   
    b. 选择订阅。
   
    c. “配额类型” > “Batch”
   
    d. “支持计划” > “配额支持 - 已包括”
   
    单击“下一步”。
1. 在“问题”中：
   
    a. 根据[业务影响情况][support_sev]选择“严重性”。
   
    b. 在“详细信息”中，指定想要更改的每个配额、Batch 帐户名和新限制。
   
    单击“下一步”。
1. 在“联系人信息”中：
   
    a. 选择“首选联系方法”。
   
    b. 输入并确认所需的联系人详细信息。
   
    单击“创建”提交支持请求。

提交支持请求后，Azure 支持人员将与你取得联系。 请注意，完成该请求最多需要 2 个工作日。

## <a name="related-quotas-for-vm-pools"></a>VM 池的相关配额

部署在 Azure 虚拟网络中的虚拟机配置中的 Batch 池可自动分配其他 Azure 网络资源。 在虚拟网络中，每 50 个池节点需要以下资源：

* 1 个[网络安全组](../virtual-network/security-overview.md#network-security-groups)
* 1 个[公共 IP 地址](../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* 1 个[负载均衡器](../load-balancer/load-balancer-overview.md)

在包含创建 Batch 池时提供的虚拟网络的订阅中分配这些资源。 这些资源受订阅的[资源配额](../azure-subscription-service-limits.md)限制。 如果计划在虚拟网络中部署大型池，请检查订阅的这些资源配额。 如果需要，请在 Azure 门户中选择“帮助和支持”，请求增大配额。


## <a name="related-topics"></a>相关主题
* [使用 Azure 门户创建 Azure Batch 帐户](batch-account-create-portal.md)
* [Azure Batch 功能概述](batch-api-basics.md)
* [Azure 订阅和服务限制、配额和约束](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: https://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
