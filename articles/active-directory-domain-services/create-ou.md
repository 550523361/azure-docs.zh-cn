---
title: 在 Azure AD 域服务中创建组织单位 （OU） |微软文档
description: 了解如何在 Azure AD 域服务托管域中创建和管理自定义组织单元 （OU）。
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: iainfou
ms.openlocfilehash: 7abbdf03e85f425f65a45e6640b82529c2b9c84f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "77614069"
---
# <a name="create-an-organizational-unit-ou-in-an-azure-ad-domain-services-managed-domain"></a>在 Azure AD 域服务托管域中创建组织单位 （OU）

活动目录域服务 （AD DS） 中的组织单位 （O） 允许您以逻辑方式对对象（如用户帐户、服务帐户或计算机帐户）进行分组。 然后，您可以将管理员分配给特定 O，并应用组策略来强制实施目标配置设置。

Azure AD DS 托管域包括两个内置 O - *AADDC 计算机*和*AADDC 用户*。 *AADDC 计算机*OU 包含所有连接到托管域的计算机的计算机对象。 *AADDC 用户*OU 包括从 Azure AD 租户同步的用户和组。 创建和运行使用 Azure AD DS 的工作负载时，可能需要为应用程序创建服务帐户以进行自我身份验证。 要组织这些服务帐户，您通常在 Azure AD DS 托管域中创建自定义 OU，然后在该 OU 中创建服务帐户。

在混合环境中，在本地 AD DS 环境中创建的 O 不会同步到 Azure AD DS。 Azure AD DS 托管域使用平面 OU 结构。 所有用户帐户和组都存储在*AADDC 用户*容器中，尽管从不同的本地域或林同步，即使您已在那里配置了分层 OU 结构也是如此。

本文介绍如何在 Azure AD DS 托管域中创建 OU。

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>开始之前

要完成本文，您需要以下资源和特权：

* 一个有效的 Azure 订阅。
    * 如果你没有 Azure 订阅，请[创建一个帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。
* 与订阅关联的 Azure Active Directory 租户，可以与本地目录或仅限云的目录同步。
    * 如果需要，请[创建一个 Azure Active Directory 租户][create-azure-ad-tenant]或[将 Azure 订阅关联到你的帐户][associate-azure-ad-tenant]。
* 在 Azure AD 租户中启用并配置 Azure Active Directory 域服务托管域。
    * 如果需要，请完成创建[和配置 Azure 活动目录域服务实例][create-azure-ad-ds-instance]的教程。
* 加入到 Azure AD DS 托管域的 Windows 服务器管理 VM。
    * 如果需要，请完成教程以创建[管理 VM][tutorial-create-management-vm]。
* 属于 Azure AD 租户中“Azure AD DC 管理员”组的用户帐户。**

## <a name="custom-ou-considerations-and-limitations"></a>自定义 OU 注意事项和限制

在 Azure AD DS 托管域中创建自定义 O 时，您将获得用于用户管理和应用组策略的额外管理灵活性。 与本地 AD DS 环境相比，在 Azure AD DS 中创建和管理自定义 OU 结构时存在一些限制和注意事项：

* 要创建自定义 O，用户必须是*AAD DC 管理员*组的成员。
* 创建自定义 OU 的用户被授予对该 OU 的管理权限（完全控制），并且是资源所有者。
    * 默认情况下 *，AAD DC 管理员*组还可以完全控制自定义 OU。
* 为*AADDC 用户*创建默认 OU，其中包含 Azure AD 租户中的所有同步用户帐户。
    * 不能将用户或组从*AddC 用户*OU 移动到您创建的自定义 OU。 只能在 Azure AD DS 托管域中创建的用户帐户或资源才能移动到自定义 O 中。
* 在自定义 O 下创建的用户帐户、组、服务帐户和计算机对象在 Azure AD 租户中不可用。
    * 这些对象不会使用 Microsoft 图形 API 或 Azure AD UI 显示;否则，这些对象不会显示在 Azure AD UI 中。它们仅在 Azure AD DS 托管域中可用。

## <a name="create-a-custom-ou"></a>创建自定义 OU

要创建自定义 OU，请使用加入域的 VM 中的活动目录管理工具。 活动目录管理中心允许您在 Azure AD DS 托管域（包括 OEM）中查看、编辑和创建资源。

> [!NOTE]
> 要在 Azure AD DS 托管域中创建自定义 OU，必须登录到*属于 AAD DC 管理员*组成员的用户帐户。

1. 登录到管理 VM。 有关如何使用 Azure 门户进行连接的步骤，请参阅[连接到 Windows 服务器 VM][connect-windows-server-vm]。
1. 在"开始"屏幕中，选择 **"管理工具**"。 在本教程中显示了用于[创建管理 VM][tutorial-create-management-vm]的可用管理工具的列表。
1. 要创建和管理 O，请从管理工具列表中选择**活动目录管理中心**。
1. 在左侧窗格中，选择 Azure AD DS 托管域，如*aaddscontoso.com*。 将显示现有非统组织和资源的列表：

    ![在活动目录管理中心中选择 Azure AD DS 托管域](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

1. "**任务**"窗格显示在活动目录管理中心的右侧。 在域下，如*aaddscontoso.com，* 选择 **"新>组织单位**"。

    ![选择在活动目录管理中心创建新 OU 的选项](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

1. 在 **"创建组织单位"** 对话框中，为新 OU 指定**名称**，如*MyCustomOu*。 提供 OU 的简短说明，例如*服务帐户的自定义 OU。* 如果需要，还可以为 OU 设置 **"按管理"** 字段。 要创建自定义 OU，请选择 **"确定**"。

    ![从活动目录管理中心创建自定义 OU](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

1. 回到活动目录管理中心，自定义 OU 现已列出，可供使用：

    ![自定义 OU 可在活动目录管理中心使用](./media/active-directory-domain-services-admin-guide/create-ou-done.png)

## <a name="next-steps"></a>后续步骤

有关使用管理工具或创建和使用服务帐户的详细信息，请参阅以下文章：

* [Active Directory 管理中心：入门](https://technet.microsoft.com/library/dd560651.aspx)
* [服务帐户分步指南](https://technet.microsoft.com/library/dd548356.aspx)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[connect-windows-server-vm]: join-windows-vm.md#connect-to-the-windows-server-vm
