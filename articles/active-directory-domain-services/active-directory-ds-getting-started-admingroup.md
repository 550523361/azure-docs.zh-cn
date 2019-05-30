---
title: Azure Active Directory 域服务：入门 | Microsoft Docs
description: 使用 Azure 门户启用 Azure Active Directory 域服务
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mstephen
ms.openlocfilehash: a4a515f3d669794d5bcdd8fa506a8d3b846db987
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66234942"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>使用 Azure 门户启用 Azure Active Directory 域服务

## <a name="task-3-configure-administrative-group"></a>任务 3：配置管理组

在此配置任务中，会在 Azure AD 目录中创建管理组。 此特殊管理组称为 *AAD DC 管理员*。 此组的成员在已加入托管域的计算机上拥有管理权限。 在已加入域的计算机上，此组将添加到管理员组。 此外，此组的成员可以使用远程桌面远程连接到已加入域的计算机。

> [!NOTE]
> 在使用 Azure Active Directory 域服务创建的托管域中没有“域管理员”或“企业管理员”权限。 在托管域上，服务将保留这些权限，不会将其提供给租户中的用户。 但是，可以使用在此配置任务中创建的特殊管理组执行某些特权操作。 这些操作包括将计算机加入域、将用户添加到已加入域的计算机上的管理组和配置组策略。
>

向导会自动在 Azure AD 目录中创建管理组。 此组名为“AAD DC 管理员”。 如果在 Azure AD 目录中拥有具有此名称的现有组，则向导会选择此组。 可以使用“管理员组”  向导页配置组成员资格。

1. 若要配置组成员资格，请单击“AAD DC 管理员”  。

    ![配置组成员资格](./media/getting-started/domain-services-blade-admingroup.png)

2. 单击“添加成员”  按钮以将用户从 Azure AD 目录添加到管理员组。

3. 完成后，单击“确定”  以前进到向导的“摘要”  页。

## <a name="configure-synchronization"></a>配置同步

Azure AD 域服务允许完全同步 Azure AD 中可用的所有用户和组，还允许选择具有作用域的同步以仅同步特定的组。 如果你选择完全同步，则以后无法  选择具有作用域的同步。 若要详细了解具有作用域的同步，请访问 [Azure AD 域服务具有作用域的同步文章](scoped-synchronization.md)。

### <a name="full-synchronization"></a>完全同步

1. 若要进行完全同步，只需要单击屏幕底部的“确定”，因为已选择了完全同步。
    ![完全同步](./media/active-directory-domain-services-admin-guide/create-sync-all.PNG)

### <a name="scoped-synchronization"></a>具有作用域的同步

1. 将同步按钮切换到“具有作用域的”，此时将出现“选择组”页面。 从此页面中，可以看到已选择将哪些组同步到你的托管域。
    ![具有作用域的同步](media/active-directory-domain-services-admin-guide/create-sync-scoped.PNG)
2. 单击顶部导航栏中的“选择组”  。 从此处，将在一侧弹出组选取器。 可以使用它选择要同步到 Azure AD 域服务的任何其他组。 完成后，单击“选择”  以关闭组选取器并将那些组添加到选定列表。
    ![具有作用域的同步选择组](media/active-directory-domain-services-admin-guide/create-sync-scoped-groupselect.PNG)
3. 单击“确定”  以移动到摘要页。

## <a name="deploy-your-managed-domain"></a>部署托管域

1. 在向导的“摘要”  页上，检查托管域的配置设置。 可以在必要时返回到向导的任何步骤进行更改。 完成后，单击“确定”  以创建新托管域。

    ![摘要](./media/getting-started/domain-services-blade-summary.png)

2. 你会看到一个通知，其中显示 Azure AD 域服务部署的进度。 单击该通知可查看部署的详细进度。

    ![通知 - 部署正在进行](./media/getting-started/domain-services-blade-deployment-in-progress.png)

## <a name="check-the-deployment-status-of-your-managed-domain"></a>检查托管域的部署状态

预配托管域的过程可能最多需要一小时。

1. 在部署正在进行时，可以在“搜索资源”  搜索框中搜索“域服务”。 从搜索结果中“选择 Azure AD 域服务”  。 “Azure AD 域服务”  边栏选项卡会列出正在预配的托管域。

    ![查找正在预配的托管域](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. 单击托管域的名称（例如，“contoso100.com”），以查看有关托管域的更多详细信息。

    ![域服务 - 预配状态](./media/getting-started/domain-services-provisioning-state.png)

3. “概述”  选项卡显示当前正在预配的托管域。 完全预配之前，无法配置托管域。 可能最多需要一小时才能完全预配托管域。

    ![预配状态期间的“域服务 - 概览”选项卡](./media/getting-started/domain-services-provisioning-state-details.png)

4. 托管域完全预配之后，“概览”  选项卡会将域状态显示为“正在运行”  。

    ![完全预配后的“域服务 - 概览”选项卡](./media/getting-started/domain-services-provisioned.png)
    >[!NOTE]
    >在预配过程中，Azure AD 域服务会在你的目录中创建名为“Domain Controller Services”和“AzureActiveDirectoryDomainControllerServices”的企业应用程序。 需要这些企业应用程序来为托管域提供服务。 必须在任何时候都不删除这些企业应用程序。
    >

5. 在“属性”  选项卡上，会看到两个 IP 地址，这些地址上的域控制器可用于虚拟网络。

    ![完全预配后的“域服务 - 属性”选项卡](./media/getting-started/domain-services-provisioned-properties.png)

## <a name="need-help"></a>需要帮助？

预配托管域的两个域控制器可能需要一个或两个小时。 如果部署失败或好几个小时停滞在“挂起”状态，请随意[联系产品团队以寻求帮助](contact-us.md)。

## <a name="next-step"></a>后续步骤

[任务 4：更新 Azure 虚拟网络的 DNS 设置](active-directory-ds-getting-started-dns.md)
