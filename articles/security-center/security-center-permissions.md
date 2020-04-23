---
title: Azure 安全中心的权限 | Microsoft Docs
description: 本文介绍 Azure 安全中心如何使用基于角色的访问控制将权限分配给用户，并辨别每个角色允许的操作。
services: security-center
cloud: na
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ''
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: 6e61571400930d4a781d6d67647bd662a7f2d350
ms.sourcegitcommit: 354a302d67a499c36c11cca99cce79a257fe44b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2020
ms.locfileid: "82106213"
---
# <a name="permissions-in-azure-security-center"></a>Azure 安全中心的权限

Azure 安全中心使用[基于角色的访问控制 (RBAC)](../role-based-access-control/role-assignments-portal.md) 提供可在 Azure 中分配给用户、组和服务的[内置角色](../role-based-access-control/built-in-roles.md)。

安全中心会评估资源的配置以识别安全问题和漏洞。 如果分配有资源所属的订阅或资源组的“所有者”、“参与者”或“读取者”角色，则仅可在安全中心看到与资源相关的信息。

除这些角色外，还有两个特定的安全中心角色：

* **安全读取者**：属于此角色的用户对安全中心具有查看权限。 该用户可查看建议、警报、安全策略和安全状态，但不能更改。
* **安全管理员**：属于此角色的用户具有与安全读取器相同的权限，还可以更新安全策略并消除警报和建议。

> [!NOTE]
> 安全角色、安全管理员和安全管理员只能访问安全中心。 安全角色无权访问存储、Web 和移动或物联网等其他 Azure 服务区域。
>
>

## <a name="roles-and-allowed-actions"></a>角色和允许的操作

下表显示安全中心的角色和允许的操作。

| 角色 | 编辑安全策略 | 应用资源的安全建议</br> （包括 with "Quick Fix！"） | 关闭警报和建议 | 查看警报和建议 |
|:--- |:---:|:---:|:---:|:---:|
| 订阅所有者 | ✔ | ✔ | ✔ | ✔ |
| 订阅参与者 | -- | ✔ | ✔ | ✔ |
| 资源组所有者 | -- | ✔ | -- | ✔ |
| 资源组参与者 | -- | ✔ | -- | ✔ |
| 读取器 | -- | -- | -- | ✔ |
| 安全管理员 | ✔ | -- | ✔ | ✔ |
| 安全读取者 | -- | -- | -- | ✔ |

> [!NOTE]
> 对于需要完成任务的用户，建议尽可能为其分配权限最小的角色。 例如，将“读者”角色分配到只需查看有关资源的安全运行状况而不执行操作（例如应用建议或编辑策略）的用户。
>
>

## <a name="next-steps"></a>后续步骤
本文介绍安全中心如何使用 RBAC 将权限分配给用户，并辨别每个角色允许的操作。 现在，已熟悉监视订阅安全状态所需的角色分配，请编辑安全策略，并应用建议，了解如何：

- [在安全中心设置安全策略](tutorial-security-policy.md)
- [管理安全中心的安全建议](security-center-recommendations.md)
- [监视 Azure 资源的安全运行状况](security-center-monitoring.md)
- [管理和响应安全中心的安全警报](security-center-managing-and-responding-alerts.md)
- [监视合作伙伴安全解决方案](security-center-partner-solutions.md)
