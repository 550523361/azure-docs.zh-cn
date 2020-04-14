---
title: 创建独立的 Azure 自动化帐户
description: 本文逐步讲解如何在 Azure 自动化中创建、测试和使用示例安全主体身份验证。
services: automation
ms.subservice: process-automation
ms.date: 01/15/2019
ms.topic: conceptual
ms.openlocfilehash: f06480767b697dca8fe41e484c02aefc58f040bf
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261323"
---
# <a name="create-a-standalone-azure-automation-account"></a>创建独立的 Azure 自动化帐户

本文介绍如何在 Azure 门户中创建 Azure 自动化帐户。 可以使用门户自动化帐户来评估和了解自动化，无需使用其他管理解决方案，也无需与 Azure Monitor 日志集成。 以后随时可以添加这些管理解决方案或者与 Azure Monitor 日志集成，以进行 Runbook 作业的高级监视。

使用自动化帐户，可以对在 Azure 资源管理器部署或经典部署中管理资源的 Runbook 进行身份验证。 一个自动化帐户可以跨所有区域和订阅管理给定租户的资源。

在 Azure 门户中创建自动化帐户时，会自动创建以下帐户：

* **以帐户身份运行**。 此帐户执行以下任务：
  * 在 Azure Active Directory (Azure AD) 中创建服务主体。
  * 创建证书。
  * 向参与者分配基于角色的访问控制 (RBAC)，以便使用 Runbook 管理 Azure 资源管理器资源。

使用系统创建的这些帐户，可以快速开始构建和部署 Runbook 来支持自动化需求。

## <a name="permissions-required-to-create-an-automation-account"></a>创建自动化帐户所需的权限

若要创建或更新自动化帐户，并完成本文所述的任务，必须具有以下特权和权限：

* 要创建自动化帐户，必须将 Azure AD 用户帐户添加到具有与 Microsoft 所有者角色等值的权限的角色 **。自动化**资源。 有关详细信息，请参阅[Azure 自动化 中的基于角色的访问控制](automation-role-based-access-control.md)。
* 在 Azure 门户中，在**Azure 活动目录** > **管理** > **用户设置**下 ，如果**应用注册**设置为 **"是**"，则 Azure AD 租户中的非管理员用户可以[注册活动目录应用程序](../active-directory/develop/howto-create-service-principal-portal.md#check-azure-subscription-permissions)。 如果“应用注册”**** 设置为“否”****，则执行此操作的用户必须是 Azure AD 中的全局管理员。

如果在被添加到订阅的全局管理员/共同管理员角色之前不是订阅的 Active Directory 实例的成员，则将作为来宾添加到 Active Directory。 在这种情况下，您可以在 **"添加自动化帐户"** 页上看到此消息："您没有创建权限。

如果已将某用户添加到全局管理员/共同管理员角色，则可先将其从订阅的 Active Directory 实例中删除，然后将其重新添加到 Active Directory 中的完整用户角色。

若要验证用户角色，请执行以下操作：

1. 在 Azure 门户中，转到“Azure Active Directory”**** 窗格。
1. 选择**用户和组**。
1. 选择**所有用户**。
1. 选择特定的用户后，选择“配置文件”****。 用户配置文件下的“用户类型”**** 属性值不应为“来宾”****。

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>在 Azure 门户中创建新的自动化帐户

若要在 Azure 门户中创建 Azure 自动化帐户，请完成以下步骤：

1. 使用帐户登录到 Azure 门户，该帐户应当是订阅管理员角色的成员并且是订阅的共同管理员。
1. 选择“+ 创建资源”。****
1. 搜索“自动化”****。 在搜索结果中，选择“自动化”****。

   ![在 Azure 市场中搜索并选择“自动化和控制”](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)

1. 在下一个屏幕上选择 **"创建**"。

   ![添加自动化帐户](media/automation-create-standalone-account/automation-create-automationacct-properties.png)

   > [!NOTE]
   > 如果“添加自动化帐户”边栏选项卡中显示以下消息，则表示你的帐户不是订阅管理员角色成员和订阅的共同管理员****。
   >
   > ![添加自动化帐户警报](media/automation-create-standalone-account/create-account-without-perms.png)

1. 在“添加自动化帐户”窗格的“名称”框中，输入新自动化帐户的名称。******** 此名称选定后即不可更改。 *自动化帐户名称对于每个区域和资源组是唯一的。已删除的自动化帐户的名称可能不会立即可用。*
1. 如果有多个订阅，请在“订阅”框中为新帐户指定一个订阅****。
1. 对于“资源组”，请输入或选择新的或现有的资源组。****
1. 对于“位置”，请选择一个 Azure 数据中心位置。****
1. 对于“创建 Azure 运行方式帐户”选项，请确保选择“是”，并选择“创建”************。

   > [!NOTE]
   > 如果对“创建 Azure 运行方式帐户”选择“否”以选择不创建运行方式帐户，则“添加自动化帐户”窗格中会显示一条消息。************ 尽管该帐户是在 Azure 门户中创建的，但它在经典部署模型订阅或 Azure 资源管理器订阅目录服务中没有对应的身份验证标识。 因此，自动化帐户无法访问订阅中的资源。 这会导致引用此帐户的任何 Runbook 无法进行身份验证，也无法针对这些部署模型中的资源执行任务。
   >
   > ![添加自动化帐户警报](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)
   >
   > 未创建服务主体时不会分配参与者角色。
   >

1. 若要跟踪自动化帐户的创建进度，请在菜单中选择“通知”****。

### <a name="resources-included"></a>包含的资源

成功创建自动化帐户后，系统会自动创建几个资源。 创建后，如果不想保留这些 runbook，可以放心地将其删除。 运行方式帐户可用于对 runbook 中的帐户进行身份验证，除非创建其他运行方式帐户或不需要它们，否则应将其保留。 下表汇总了运行方式帐户的资源。

| 资源 | 说明 |
| --- | --- |
| AzureAutomationTutorial Runbook |一个示例图形 Runbook，演示如何使用运行方式帐户进行身份验证。 该 Runbook 获取所有资源管理器资源。 |
| AzureAutomationTutorialScript Runbook |一个示例 PowerShell Runbook，演示如何使用运行方式帐户进行身份验证。 该 Runbook 获取所有资源管理器资源。 |
| AzureAutomationTutorialPython2 Runbook |一个示例 Python Runbook，演示如何使用运行方式帐户进行身份验证。 该 Runbook 列出订阅中的所有资源组。 |
| AzureRunAsCertificate |在创建自动化帐户时自动创建的，或针对现有帐户使用 PowerShell 脚本创建的证书资产。 使用该证书可向 Azure 进行身份验证，以便通过 Runbook 管理 Azure 资源管理器资源。 此证书有一年的有效期。 |
| AzureRunAsConnection |在创建自动化帐户时自动创建的，或针对现有帐户使用 PowerShell 脚本创建的连接资产。 |

## <a name="create-a-classic-run-as-account"></a>创建经典运行身份帐户

默认情况下，创建 Azure 自动化帐户时，将不再创建经典运行方式帐户。 如果仍需要经典运行方式帐户，请执行以下步骤。

1. 在“自动化帐户”页的“帐户设置”下，选择“运行方式帐户”************。
2. 选择“Azure 经典运行方式帐户”****。
3. 单击“创建”****，以继续创建经典运行方式帐户。

## <a name="next-steps"></a>后续步骤

* 要了解有关图形创作的更多信息，请参阅[Azure 自动化 中的图形创作](automation-graphical-authoring-intro.md)。
* 要开始使用 PowerShell 运行簿，请参阅[我的第一个 PowerShell 运行簿](automation-first-runbook-textual-powershell.md)。
* 若要开始使用 PowerShell 工作流 Runbook，请参阅 [我的第一个 PowerShell 工作流 Runbook](automation-first-runbook-textual.md)。
* 要开始使用 Python2 运行簿，请参阅[我的第一个 Python2 运行簿](automation-first-runbook-textual-python2.md)。

