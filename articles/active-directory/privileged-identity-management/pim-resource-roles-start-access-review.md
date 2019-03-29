---
title: 在 PIM 的 Azure Active Directory 中启动 Azure 资源角色的访问评审 |Microsoft Docs
description: 了解如何在 Azure AD Privileged Identity Management (PIM) 中启动 Azure 资源角色的访问评审。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46903967b375d882dc3c7a62cd0b7f8b6059f8b3
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2019
ms.locfileid: "58579489"
---
# <a name="start-an-access-review-for-azure-resource-roles-in-pim"></a>在 PIM 中启动 Azure 资源角色的访问评审
当用户有不再需要的特权访问时，角色分配将变为“过时”。 为了降低与这些过时角色分配相关联的风险，特权角色管理员应定期审阅角色。 本文档介绍用于启动访问评审在 Azure Active Directory (Azure AD) Privileged Identity Management (PIM) 的步骤。

从 PIM 应用程序主页转至：

* “访问审阅” > “添加”

![添加访问评审](media/azure-pim-resource-rbac/rbac-access-review-home.png)

选择“添加”按钮时，会显示“创建访问评审”边栏选项卡。 在此边栏选项卡上，使用某个名称在限制时间内配置评审、选择要评审的角色，并确定由谁执行评审。

![创建访问评审](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>配置审阅
要创建访问评审，请先为其命名，然后设置开始日期和结束日期。

![配置审阅 - 屏幕截图](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

设置足够长的审阅时间，以便用户完成审阅。 如果在结束日期之前完成，则始终可以提前停止该评审。

### <a name="choose-a-role-to-review"></a>选择要审阅的角色
每个审阅仅关注一个角色。 除非已从特定角色边栏选项卡开始访问评审，否则此时需要选择一个角色。

1. 转至“评审角色成员资格”
   
    ![审阅角色成员身份 - 屏幕截图](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. 从列表中选择一个角色。

### <a name="decide-who-will-perform-the-review"></a>确定谁将执行审阅
有三个选项用于执行审阅。 可以将评审分配给其他人完成、自行执行，也可以让每个用户评审自己的访问。

1. 选择以下选项之一：
   
   * **所选用户**：如果不知道谁需要访问，请使用此选项。 使用此选项，可以将审阅分配给资源所有者或组管理员完成。
   * **已分配(自我)**：使用此选项可让用户评审其自己的角色分配。
   
2. 转到“选择审阅者”。
   
    ![选择审阅者 - 屏幕快照](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>开始审阅
最后，可要求用户提供批准访问的原因。 添加评审的描述（如有需要）。 然后选择“开始”。

确保让用户知道他们有访问评审需要处理，并向他们显示[如何执行访问评审](pim-resource-roles-perform-access-review.md)。

## <a name="manage-the-access-review"></a>管理访问审阅
审阅者完成评审后，可在 PIM Azure 资源仪表板中跟踪进度。 在[评审完成](pim-resource-roles-complete-access-review.md)之前，目录中的任何访问权限都不会更改。

在审阅期限结束之前，都可以提醒用户完成其审阅，或者从访问审阅部分提前结束审阅。

## <a name="next-steps"></a>后续步骤

- [在 PIM 中完成 Azure 资源角色的访问评审](pim-resource-roles-complete-access-review.md)
- [在 PIM 中对 Azure 资源角色执行访问评审](pim-resource-roles-perform-access-review.md)
- [在 PIM 中启动 Azure AD 角色的访问评审](pim-how-to-start-security-review.md)
