---
title: 对 Azure 资源的 RBAC 问题进行故障排除 | Microsoft Docs
description: 对 Azure 资源基于角色的访问控制 (RBAC) 问题进行故障排除。
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 7b27c811214def7f5646f886b955d035a50c0725
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342467"
---
# <a name="troubleshoot-rbac-for-azure-resources"></a>对 Azure 资源的 RBAC 问题进行故障排除

本文解答有关 Azure 资源的基于角色的访问控制 (RBAC) 的常见问题，以便你了解在 Azure 门户中使用角色时可能出现的情况，并可对访问问题进行故障排除。

## <a name="problems-with-rbac-role-assignments"></a>RBAC 角色分配出现问题

- 如果因为**添加角色分配**选项被禁用或者因为收到权限错误而无法添加角色分配，请检查你使用的角色在你尝试分配角色的作用域内是否具有 `Microsoft.Authorization/roleAssignments/*` 权限。 如果不具有此权限，请联系你的订阅管理员。
- 如果尝试创建资源时遇到权限错误，请检查使用的角色在所选范围内是否有权创建资源。 例如，你可能需要拥有“参与者”权限。 如果没有此权限，请联系订阅管理员。
- 如果尝试创建或更新支持票证时收到权限错误，请检查你使用的角色是否具有 `Microsoft.Support/*` 权限，例如[支持请求参与者](built-in-roles.md#support-request-contributor)。
- 如果尝试分配角色时收到“超出了角色分配数”错误，请尝试通过改为将角色分配给组来减少角色分配数。 Azure 对于每个订阅最多支持 **2000** 个角色分配。

## <a name="problems-with-custom-roles"></a>自定义角色出现问题

- 如果无法更新现有的自定义角色，请检查你是否拥有 `Microsoft.Authorization/roleDefinition/write` 权限。
- 如果无法更新现有的自定义角色，请检查租户中是否删除了一个或多个可分配范围。 自定义角色的 `AssignableScopes` 属性控制[谁可以创建、删除、更新或查看自定义角色](custom-roles.md#who-can-create-delete-update-or-view-a-custom-role)。
- 如果尝试创建新角色时遇到“超出了角色定义限制”错误，请删除任何未使用的自定义角色。 还可以尝试合并或重复使用任何现有的自定义角色。 Azure 在一个租户中最多支持 **2000** 个自定义角色。
- 如果无法删除某个自定义角色，请检查是否有一个或多个角色分配仍在使用自定义角色。

## <a name="recover-rbac-when-subscriptions-are-moved-across-tenants"></a>在租户之间移动订阅时恢复 RBAC

- 如果你需要了解将订阅转让给其他帐户的步骤，请参阅[将 Azure 订阅所有权转让给其他帐户](../billing/billing-subscription-transfer.md)。
- 如果将订阅转移到不同的租户，所有角色分配将从源租户中永久删除，而不会迁移到目标租户。 必须在目标租户中重新创建角色分配。
- 如果你是全局管理员并且失去了对某个订阅的访问权限，请使用“Azure 资源的访问权限管理”开关暂时[提升访问权限](elevate-access-global-admin.md)，以重新获取对该订阅的访问权限。

## <a name="rbac-changes-are-not-being-detected"></a>未检测到 RBAC 更改

Azure 资源管理器有时会缓存配置和数据以提高性能。 创建或删除角色分配时，更改最多可能需要 30 分钟才能生效。 如果使用的是 Azure 门户、Azure PowerShell 或 Azure CLI，则可以通过注销和登录来强制刷新角色分配更改。 如果使用 REST API 调用进行角色分配更改，则可以通过刷新访问令牌来强制刷新。

## <a name="web-app-features-that-require-write-access"></a>需要写访问权限的 Web 应用功能

如果为用户授予单个 Web 应用的只读访问权限，某些功能可能会被禁用，这可能不是你所期望的。 以下管理功能需要对 Web 应用具有**写**访问权限（参与者或所有者），并且在任何只读方案中不可用。

* 命令（例如启动、停止等。）
* 更改设置（如常规配置、缩放设置、备份设置和监视设置）
* 访问发布凭据和其他机密（如应用设置和连接字符串）
* 流式传输日志
* 诊断日志配置
* 控制台（命令提示符）
* 活动和最新部署（适用于本地 Git 连续部署）
* 估计费用
* Web 测试
* 虚拟网络（如果虚拟网络是以前具有写访问权限的用户所配置的，则仅对读者可见）。

如果无法访问以上任何磁贴，则需要让管理员提供对 Web 应用的“参与者”访问权限。

## <a name="web-app-resources-that-require-write-access"></a>需要写访问权限的 Web 应用资源

由于存在几个相互作用的不同资源，Web 应用程序是复杂的。 下面是包含几个网站的典型资源组：

![Web 应用程序资源组](./media/troubleshooting/website-resource-model.png)

因此，如果只授予某人对 Web 应用的访问权限，则 Azure 门户中的网站边栏选项卡上的很多功能将被禁用。

这些项需要对与网站对应的**应用服务计划**具有**写**访问权限：  

* 查看 Web 应用的定价层（“免费”或“标准”）  
* 规模配置（实例数、虚拟机大小、自动缩放设置）  
* 配额（存储、带宽、CPU）  

这些项需要对包含网站的整个**资源组**具有**写**访问权限：  

* SSL 证书和绑定（SSL 证书可以在同一资源组和地理位置中的站点之间共享）  
* 警报规则  
* 自动缩放设置  
* Application Insights 组件  
* Web 测试  

## <a name="virtual-machine-features-that-require-write-access"></a>需要写访问权限的虚拟机功能

与 Web 应用类似，虚拟机边栏选项卡上的某些功能需要对虚拟机或资源组中的其他资源具有写访问权限。

虚拟机与域名、虚拟网络、存储帐户和警报规则相关。

这些项需要对**虚拟机**具有**写**访问权限：

* 终结点  
* IP 地址  
* 磁盘  
* 扩展  

这些项需要对**虚拟机**和其所在的**资源组**（以及域名）具有**写**访问权限：  

* 可用性集  
* 负载均衡集  
* 警报规则  

如果无法访问以上任何磁贴，则需要让管理员提供对资源组的“参与者”访问权限。

## <a name="azure-functions-and-write-access"></a>Azure Functions 和写访问权限

[Azure Functions](../azure-functions/functions-overview.md) 的某些功能需要写入权限。 例如，如果给用户分配读者角色，他们将无法查看函数应用中的函数。 门户将显示 (无访问权限)。

![函数应用无访问权限](./media/troubleshooting/functionapps-noaccess.png)

读者可单击“平台功能”选项卡，然后单击“所有设置”查看与函数应用（类似于 Web 应用）相关的一些设置，但无法修改任何这些设置。

## <a name="next-steps"></a>后续步骤
* [使用 RBAC 和 Azure 门户管理对 Azure 资源的访问权限](role-assignments-portal.md)
* [查看 Azure 资源的 RBAC 更改的活动日志](change-history-report.md)

