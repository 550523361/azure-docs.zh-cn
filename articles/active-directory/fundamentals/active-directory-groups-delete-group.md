---
title: 删除组 - Azure Active Directory | Microsoft Docs
description: 关于如何使用 Azure Active Directory 删除组的说明。
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 08/29/2018
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: cf309c10dc5924374d8c3a191f6fd11cceb2ac49
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85604278"
---
# <a name="delete-a-group-using-azure-active-directory"></a>使用 Azure Active Directory 删除组
可以因为各种原因删除 Azure Active Directory (Azure AD) 组，但通常是因为：

- 错误地将“组类型”设为了错误的选项  。

- 错误地创建错误或重复组。 

- 不再需要组。

## <a name="to-delete-a-group"></a>删除组
1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.com)。

2. 选择“Azure Active Directory”，然后选择“组”   。

3. 在“组 - 所有组”页中，搜索并选择要删除的组。  对于这些步骤，我们将使用“MDM 策略 - 东部”  。

    ![“组 - 所有组”页，其中突出显示了组名称](media/active-directory-groups-delete-group/group-all-groups-screen.png)

4. 在“MDM 策略 - 东部概览”页中，选择“删除”   。

    将从 Azure Active Directory 租户中删除组。

    ![MDM 策略 - “东部概览”页，其中突出显示了删除选项](media/active-directory-groups-delete-group/group-overview-blade.png)

## <a name="next-steps"></a>后续步骤

- 如果误删除某个组，可以重新创建。 有关详细信息，请参阅[如何创建基本组并添加成员](active-directory-groups-create-azure-portal.md)。

- 如果误删除 Office 365 组，可能能够将其还原。 有关详细信息，请参阅[还原已删除的 Office 365 组](../users-groups-roles/groups-restore-deleted.md)。
