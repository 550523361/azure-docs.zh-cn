---
title: 为 Azure 管理访问权限设置 RBAC 角色
titleSuffix: Azure Cognitive Search
description: Azure 门户中基于角色的管理控制（RBAC），用于控制和委派 Azure 认知搜索管理的管理任务。
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 9262d01e35bd03a9116a30b070b023f578f0b15a
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2019
ms.locfileid: "74112557"
---
# <a name="set-rbac-roles-for-administrative-access-to-azure-cognitive-search"></a>设置用于管理 Azure 认知搜索的 RBAC 角色

对于通过门户或 Resource Manager API 管理的所有服务，Azure 提供了[基于全局角色的授权模型](../role-based-access-control/role-assignments-portal.md)。 所有者、参与者和读者角色根据分配给每个角色的 Active Directory 用户、组和安全主体的服务管理，确定服务管理的级别。 

> [!Note]
> 不存在用于保护文档索引或文档子集且基于角色的访问控制。 如果要实现针对搜索结果的、基于标识的访问，可创建安全筛选器按标识来细化结果，由此去除请求者不应具有访问权限的那些文档。 有关详细信息，请参阅[安全筛选器](search-security-trimming-for-azure-search.md)和[使用 Active Directory 进行保护](search-security-trimming-for-azure-search-with-aad.md)。

## <a name="management-tasks-by-role"></a>按角色划分的管理任务

对于 Azure 认知搜索，角色与支持以下管理任务的权限级别关联：

| 角色 | 任务 |
| --- | --- |
| 所有者 |创建或删除服务或者服务上的任何对象，包括 API 密钥、索引、索引器、索引器数据源和索引器计划。<p>查看服务状态，包括计数和存储大小。<p>添加或删除角色成员身份（仅所有者才能管理角色成员身份）。<p>订阅管理员和服务所有者拥有所有者角色的自动成员身份。 |
| 参与者 |访问级别与所有者的访问级别相同，不包括 RBAC 角色管理。 例如，参与者可创建或删除对象，或查看和重新生成 [API 密钥](search-security-api-keys.md)，但不能修改角色成员身份。 |
| [搜索服务参与者内置角色](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor) | 等效于参与者角色。 |
| 读取器 |查看服务概要和指标。 此角色的成员无法查看索引、索引器、数据源或密钥信息。  |

角色不授予对服务终结点的访问权限。 搜索服务操作（例如索引管理、索引填充和搜索数据的查询）可通过 API 密钥而非角色进行控制。 有关详细信息，请参阅[管理 API 密钥](search-security-api-keys.md)。

## <a name="see-also"></a>另请参阅

+ [使用 PowerShell 管理](search-manage-powershell.md) 
+ [Azure 认知搜索中的性能和优化](search-performance-optimization.md)
+ [Azure 门户中基于角色的访问控制入门](../role-based-access-control/overview.md)。