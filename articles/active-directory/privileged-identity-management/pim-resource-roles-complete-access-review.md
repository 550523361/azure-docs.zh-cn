---
title: 在 PIM 中完成 Azure 资源角色的访问评审 | Microsoft Docs
description: 了解如何在 Azure AD Privileged Identity Management (PIM) 中完成 Azure 资源角色的访问评审。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 1e2bdeeb8f2b59d69e761303176c36b26f47d4c8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165483"
---
# <a name="complete-an-access-review-for-azure-resource-roles-in-pim"></a>在 PIM 中完成 Azure 资源角色的访问评审
[访问评审开始](pim-resource-roles-start-access-review.md)后，特权角色管理员可以评审特权访问。 Azure 资源的 Privileged Identity Management (PIM) 会自动发送一封电子邮件来提示用户审阅其访问。 如果用户未收到电子邮件，可以向他们发送[如何执行访问评审](pim-resource-roles-perform-access-review.md)的相关说明。

访问评审期限结束，或者所有用户已完成其自评审后，请按照本文中的步骤管理评审并查看结果。

## <a name="manage-access-reviews"></a>管理访问评审
1. 转到 [Azure 门户](https://portal.azure.com/)。 然后，在仪表板中，选择“Azure 资源”应用程序。

2. 选择你的资源。

3. 选择仪表板的“访问审阅”部分。
![访问评审](media/azure-pim-resource-rbac/rbac-access-review-home-list.png)

4. 选择要管理的访问审阅。

在访问评审的详细信息边栏选项卡上，有大量用于管理该评审的选项。 选项如下：

![用于管理评审的选项](media/azure-pim-resource-rbac/rbac-access-review-menu.png)

### <a name="stop"></a>停止
所有访问审阅都有结束日期，但可以使用“停止”按钮提前结束。 若用户此时尚未完成审阅，在停止审阅后，用户无法完成审阅。 停止后，无法重新开始审阅。

### <a name="reset"></a>重置
可重置访问评审来删除对其所做的所有决策。 重置访问评审后，所有用户都将被重新标记为未审阅。 

### <a name="apply"></a>应用
完成访问评审后，使用“应用”按钮来实现评审结果。 如果在评审中拒绝了用户的访问，则此步骤会删除其角色分配。  

### <a name="delete"></a>删除
如果不再想了解审阅，可将其删除。 “删除”按钮可从 PIM 应用程序中删除审阅。

## <a name="results"></a>结果
在“结果”选项卡上查看和下载审阅结果列表。 
![“结果”选项卡](media/azure-pim-resource-rbac/rbac-access-review-results.png)

## <a name="reviewers"></a>审阅者
查看现有访问审阅的审阅者以及为其添加审阅者。 提醒审阅者完成其审阅。
![添加审阅者](media/azure-pim-resource-rbac/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>后续步骤

- [在 PIM 中启动 Azure 资源角色的访问评审](pim-resource-roles-start-access-review.md)
- [在 PIM 中对 Azure 资源角色执行访问评审](pim-resource-roles-perform-access-review.md)
