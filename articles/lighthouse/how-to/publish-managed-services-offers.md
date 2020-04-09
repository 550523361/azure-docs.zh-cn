---
title: 向 Azure 应用商店发布托管服务产品/服务
description: 了解如何发布将客户载入到 Azure 委派资源管理的托管服务产品。
ms.date: 04/08/2020
ms.topic: conceptual
ms.openlocfilehash: 4791b1d2ae233b0cc7aad33dd5b15b6ea94b2018
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984548"
---
# <a name="publish-a-managed-service-offer-to-azure-marketplace"></a>向 Azure 应用商店发布托管服务产品/服务

> [!IMPORTANT]
> 从 2020 年 4 月 14 日起，我们将开始将托管服务优惠的管理转移到合作伙伴中心。 迁移后，您将在合作伙伴中心创建和管理您的优惠。 按照[创建新托管服务产品/服务产品/服务/服务中的](../../marketplace/partner-center-portal/create-new-managed-service-offer.md)说明进行管理迁移的优惠。

在本文中，您将了解如何使用[云合作伙伴门户](https://cloudpartner.azure.com/)将公共或私有托管服务产品发布到 Azure[应用商店](https://azuremarketplace.microsoft.com)。 然后，购买产品/服务的客户将能够将订阅和资源组用于[Azure 委派的资源管理](../concepts/azure-delegated-resource-management.md)。

## <a name="publishing-requirements"></a>发布要求

您需要[在合作伙伴中心](../../marketplace/partner-center-portal/create-account.md)中拥有有效的帐户才能创建和发布产品/服务。 如果您还没有帐户，[注册过程](https://aka.ms/joinmarketplace)将引导您完成在合作伙伴中心创建帐户并注册商业市场计划的步骤。

根据[托管服务提供认证要求](https://docs.microsoft.com/legal/marketplace/certification-policies#7004-business-requirements)，您必须具有[银云或金云平台能力级别](https://docs.microsoft.com/partner-center/learn-about-competencies)或 Azure 专家[MSP](https://partner.microsoft.com/membership/azure-expert-msp)才能发布托管服务产品/服务。

你的 Microsoft 合作伙伴网络 (MPN) ID 将[自动关联](../../billing/billing-partner-admin-link-started.md)你发布的产品/服务，以跟踪你在客户参与中的影响。

> [!NOTE]
> 如果不想将产品/服务发布到 Azure 市场，可使用 Azure 资源管理器模板手动载入客户。 有关详细信息，请参阅[将客户载入到 Azure 委派资源管理](onboard-customer.md)。

与将其他任何类型的产品/服务发布到 Azure 市场相比，发布托管服务产品的过程很类似。 要了解常规发布过程，请参阅[Azure 应用商店和 AppSource 发布指南](../../marketplace/marketplace-publishers-guide.md)。 还应查看[商业市场认证策略](https://docs.microsoft.com/legal/marketplace/certification-policies)，特别是[托管服务](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services)部分。

客户添加产品/服务后，他们将能够委派一个或多个订阅或资源组，然后这些订阅或资源组将[注册用于 Azure 委派的资源管理](#the-customer-onboarding-process)。

> [!IMPORTANT]
> Each plan in a Managed service offer includes a **Manifest Details** section, where you define the Azure Active Directory (Azure AD) entities in your tenant that will have access to the delegated resource groups and/or subscriptions for customers who purchase that plan. 请务必注意，您包括的任何组（或用户或服务主体）对于购买计划的每个客户都将具有相同的权限。 要分配不同的组以与每个客户合作，您需要发布每个客户独有的单独[私人计划](../../marketplace/private-offers.md)。

## <a name="create-your-offer"></a>创建产品/服务

1. 登录到[云合作伙伴门户](https://cloudpartner.azure.com/)。
2. 从左侧导航菜单中，选择“新建产品/服务”，然后选择“托管服务”********。
3. 您将看到一个**编辑器**部分，包括要填写的四个部分：**产品/服务设置**、**计划**、**市场****和支持**。 请继续阅读，了解如何填写这些部分。

### <a name="enter-offer-settings"></a>输入产品/服务设置

在“产品/服务设置”部分中，提供以下信息****：

|字段  |说明  |
|---------|---------|
|**产品/服务 ID**     | （发布者配置文件内）产品/服务的唯一标识符。 此 ID 只能包含小写字母数字字符、短划线和下划线，且不得超过 50 个字符。 请记住，客户可能会在产品 URL 和计费报表等位置看到产品/服务 ID。 一旦发布产品/服务，即无法更改此值。        |
|**发布者 ID**     | 将与产品/服务关联的发布者 ID。 如果拥有多个发布者 ID，可选择要用于此产品/服务的 ID。       |
|**名称**     | 你的产品/服务将在 Azure 市场和 Azure 门户中向客户显示的名称（不超过 50 个字符）。 请使用客户能理解的可识别的品牌名称 - 如果正在通过自己的网站推广此产品/服务，请确保在此处使用完全相同的名称。        |

完成后，选择“保存”****。 现在，可继续转到“计划”部分****。

### <a name="create-plans"></a>创建计划

每个产品/服务都必须具有一个或多个计划（有时也称为 SKU）。 可添加多个计划，以不同的价格支持不同的功能集，也可为有限数量的特定客户自定义特定计划。 客户可在父级产品/服务中查看可供其使用的计划。

在“计划”部分中，选择“新建计划”****。 然后，输入计划 ID****。 此 ID 只能包含小写字母数字字符、短划线和下划线，且不得超过 50 个字符。 客户可能会在产品 URL 和计费报表等位置看到计划 ID。 一旦发布产品/服务，即无法更改此值。

#### <a name="plan-details"></a>计划详细信息

完成“计划详细信息”部分中的以下部分****：

|字段  |说明  |
|---------|---------|
|**Title**     | 要显示的计划的易记名称。 最大长度为 50 个字符。        |
|**摘要**     | 标题下显示的计划的简洁说明。 最大长度为 100 个字符。        |
|**说明**     | 更详细地介绍计划的说明文本。         |
|**计费模式**     | 此处显示两种计费模式，但必须为托管服务产品选择“自带许可”****。 这意味着你将直接向客户收取与此产品/服务相关的费用，而 Microsoft 不向你收取任何费用。   |
|**这是私人计划吗？**     | 指示该 SKU 是专用还是公共 SKU。 默认值为“否”（即公共 SKU）****。 如果保留此选择，则你的计划将不仅限于特定客户（或特定数量的客户）；发布公用计划后，无法再将其更改为专用计划。 要使此计划仅对特定客户可用，请选择“是”****。 执行此操作时，需要提供客户的订阅 ID 来确定客户身份。 可逐一输入（最多 10 个订阅），也可上传一个 .csv 文件（最多 20,000 个订阅）。 请确保在此处包含你自己的订阅，以便测试和验证该产品/服务。 有关详细信息，请参阅[专用 SKU 和计划](../../marketplace/cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md)。  |

> [!IMPORTANT]
> 计划一旦公开发布，您就无法将其更改为私有。 要控制哪些客户可以接受您的报价并委派资源，请使用私人计划。 使用公共计划时，您无法将可用性限制为特定客户，甚至限制特定数量的客户（尽管如果您选择这样做，您可以完全停止销售计划）。 只有在发布产品/服务时，在**角色定义**中包含"**授权"** 设置为["托管服务注册分配删除角色"时](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role)，才能在客户接受产品/服务后取消对[委派的访问权限](onboard-customer.md#remove-access-to-a-delegation)。 您也可以联系客户，要求他们[删除您的访问权限](view-manage-service-providers.md#add-or-remove-service-provider-offers)。

#### <a name="manifest-details"></a>清单详细信息

完成计划的“清单详细信息”部分****。 这会创建一个清单，其中包含用于管理客户资源的授权信息。 若要启用 Azure 委派资源管理，必须使用此信息。

> [!NOTE]
> 如上所述，“授权”条目中的用户和角色将应用于每个购买了计划的客户****。 如果想要限制特定客户的访问权限，需发布专用计划供其专用。

首先，请提供清单的版本****。 使用 n.n.n 格式（例如 1.2.5）**。

然后，输入租户 ID****。 这是与你组织的 Azure Active Directory 租户 ID 关联的 GUID（即你将在其中管理客户资源的租户）。 如果没有此信息，可将鼠标悬停在 Azure 门户右上方的帐户名称上，或者选择“切换目录”来找到它****。

最后，将一个或多个授权条目添加到计划中****。 授权定义了哪些实体可访问购买计划的客户的资源和订阅，并分配了授予特定访问级别的角色。

> [!TIP]
> 在大多数情况下，需要将权限分配给 Azure AD 用户组或服务主体，而不是分配给一系列单独的用户帐户。 这样，便可添加或删除单位用户的访问权限，而无需在访问要求更改时更新和重新发布计划。 有关其他建议，请参阅 [Azure Lighthouse 方案中的租户、角色和用户](../concepts/tenants-users-roles.md)。

对于每个授权，你需要提供以下信息****。 然后，可根据需要多次选择“新授权”，以添加更多用户和角色定义****。

- **Azure AD 对象 ID**：用户、用户组或应用程序的 Azure AD 标识符，这些权限将被授予客户资源的某些权限（如角色定义所述）。
- **Azure AD 对象显示名称**：一个友好名称，用于帮助客户了解此授权的目的。 在委派资源时，客户将看到此名称。
- **角色定义**：从列表中选择一个可用的 Azure AD 内置角色。 此角色将确定“Azure AD 对象 ID”字段中的用户对客户资源拥有的权限****。 有关这些角色的说明，请参阅[Azure 委派资源管理的](../concepts/tenants-users-roles.md#role-support-for-azure-delegated-resource-management)[内置角色](../../role-based-access-control/built-in-roles.md)和角色支持。
  > [!NOTE]
  > 由于适用的新内置角色已添加到 Azure 中，它们将在此处可用，尽管在出现之前可能会有一些延迟。
- **可分配角色**：仅在您在此授权**的角色定义**中选择"用户访问管理员"时，才需要此角色。 如果是这样，则必须在此处添加一个或多个可分配角色。 “Azure AD 对象 ID”字段中的用户能够将这些“可分配角色”分配给[托管标识](../../active-directory/managed-identities-azure-resources/overview.md)，这是[部署可修正的策略](deploy-policy-remediation.md)所必需的********。 请注意，通常与用户访问管理员角色关联的其他权限均不适用于此用户。 如果未在此处选择一个或多个角色，则你的提交将无法通过认证。 （如果未为此用户的角色定义选择"用户访问管理员"，则此字段不起作用。

> [!TIP]
> 为确保在需要时可以[删除对委派的访问](onboard-customer.md#remove-access-to-a-delegation)，请包括一个**授权，** 其中**的角色定义**设置为[托管服务注册分配删除角色](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role)。 如果未分配此角色，则只能由客户租户中的用户删除委派资源。

完成这些信息后，可以根据需要多次选择“新建计划”以创建其他计划****。 完成后，选择“保存”，然后继续填写“市场”部分********。

### <a name="provide-marketplace-text-and-images"></a>提供市场文本和图像

可在“市场”部分提供客户要在 Azure 市场和 Azure 门户看到的文本和图像****。

在“概述”部分中填写以下字段****：

|字段  |说明  |
|---------|---------|
|**Title**     |  套餐的标题，通常是较长的正式名称。 此标题将在市场中突出显示。 最大长度为 50 个字符。 大多数情况下，此名称应与你在“产品/服务设置”部分输入的名称相同********。       |
|**摘要**     | 产品/服务用途或功能的简要说明。 通常显示在标题下方。 最大长度为 100 个字符。        |
|**长摘要**     | 产品/服务用途或功能的详细摘要。 最大长度为 256 个字符。        |
|**说明**     | 有关产品/服务的详细信息。 此字段不得超过 3000 个字符，且支持简单的 HTML 格式。 必须在说明中的某个位置包含“托管服务”字样。       |
|**营销标识符**     | 唯一的 URL 友好标识符。 此标识符只能包含小写字母数字字符和破折号。 它将用于此优惠的应用商店 URL 中。 例如，如果发布者 ID 为 contoso，营销标识符为 sampleApp，则产品/服务在 Azure 市场中的 URL 为 https://azuremarketplace.microsoft.com/marketplace/apps/contoso-sampleApp******。        |
|**预览订阅 ID**     | 添加 1 到 100 个订阅标识符。 在产品/服务上线之前，与这些订阅关联的客户将可在 Azure 市场中查看此产品/服务。 建议在此处包含你自己的订阅，以便在向客户提供产品/服务之前，可预览其在 Azure 市场中的显示方式。  （在此预览期间，Microsoft 支持和工程团队也将能够查看你的产品/服务。）   |
|**有用链接**     | 与你的产品/服务相关的 URL，例如文档、发行说明、常见问题解答等等。        |
|**建议的类别（最多 5 个）**     | 适用于你的产品/服务的一个或多个类别（最多 5 个）。 这些类别可帮助客户在 Azure 市场和 Azure 门户中发现你的产品/服务。        |

在“营销项目”部分，可上传徽标和其他资产，使其与产品/服务一起显示****。 可选择性地上传屏幕截图或视频链接，帮助客户了解你的产品/服务。

需要四个徽标大小：**小 （40x40）**、**中等 （90x90）**、**大 （115x115）** 和**宽 （255x115）**。 请遵守徽标适用的下述准则：

- Azure 设计具有简单的调色板。 限制徽标上的主要和次要颜色数。
- 门户的主题颜色为白色和黑色。 请勿将这些颜色用作徽标的背景色。 使用可使徽标在门户中更为突出的颜色。 建议使用简单的主颜色。
- 如果使用透明背景，请确保徽标和文本不是白色、黑色或蓝色。
- 徽标的外观应平整，并且应避免渐变。 不要在徽标上使用渐变背景。
- 不要在徽标上放置文本，即使是公司或品牌名称也不可以。
- 确保徽标未被拉伸。

特大 (815x290) 徽标可选，但建议使用它****。 如果包含了特大徽标，请遵循以下准则：

- 特大徽标中不得包含任何文本，且务必在徽标右侧留出 415 像素的空白空间。 目的是有空间以编程方式嵌入发布者显示名称、计划标题和产品/服务详细摘要等文本元素。
- 特大徽标的背景不得为黑色、白色或透明。 请确保背景色较深，因为嵌入的文本将显示为白色。
- 一旦使用特大图标发布产品/服务，便不能将其删除（但如果需要，可使用其他版本进行更新）。

可在“潜在顾客管理”部分中，选择将存储潜在顾客的 CRM 系统****。 请注意，根据[托管服务认证策略](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services)，需要**潜在顾客目标**。

最后，在“法律”部分提供隐私策略 URL 和使用条款************。 还可在此处指定是否对此产品/服务使用[标准协定](../../marketplace/standard-contract.md)。

转到“支持”部分之前，请务必保存更改****。

### <a name="add-support-info"></a>添加支持信息

在“支持”部分中，提供工程联系人和客户支持联系人的姓名、电子邮件和电话号码****。 还需要提供支持 URL。 当 Microsoft 需要就业务和支持问题与你联系时，我们可能会使用此信息。

添加此信息后，请选择“保存”****。

## <a name="publish-your-offer"></a>发布产品/服务

完成所有部分后，下一步就是将产品/服务发布到 Azure 市场。 选择“发布”按钮启动产品/服务上线过程****。 有关此过程的详细信息，请参阅[发布 Azure 市场和 AppSource 产品/服务](../../marketplace/cloud-partner-portal/manage-offers/cpp-publish-offer.md)。

你可以随时[发布产品/服务](../../marketplace/cloud-partner-portal/manage-offers/cpp-update-offer.md)的更新版本。 例如，你可能想要向以前发布的产品/服务添加新的角色定义。 当你执行此操作时，已添加该产品/服务的客户会在 Azure 门户中的[服务提供程序****](view-manage-service-providers.md)页面上看到一个图标，他们可通过图标知道更新可用。 每个客户都可以[查看更改](view-manage-service-providers.md#update-service-provider-offers)并决定是否要更新到新版本。 

## <a name="the-customer-onboarding-process"></a>客户加入过程

客户添加你的产品/服务后，他们将能够[委托一个或多个特定订阅或资源组](view-manage-service-providers.md#delegate-resources)，然后将加入这些订阅或资源组以进行 Azure 委托资源管理。 如果客户已接受产品/服务但尚未委托任何资源，则他们会在 Azure 门户[“服务提供商”****](view-manage-service-providers.md)页中“提供商产品/服务”部分的顶部看到备注****。

> [!IMPORTANT]
> 委派必须由客户租户中的非来宾帐户完成，该帐户具有已加入的订阅[的所有者内置角色](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner)（或包含正在装机的资源组）。 若要查看所有可以委托订阅的用户，客户租户中的用户可以在 Azure 门户中选择订阅，打开“访问控制(IAM)”****，然后[查看具有“所有者”角色的所有用户](../../role-based-access-control/role-assignments-list-portal.md#list-owners-of-a-subscription)。

一旦客户委派订阅（或订阅中的一个或多个资源组 **），Microsoft.托管服务**资源提供程序将注册该订阅，租户中的用户将能够根据产品/服务中的授权访问委派的资源。

## <a name="next-steps"></a>后续步骤

- 了解[商业市场](../../marketplace/partner-center-portal/commercial-marketplace-overview.md)。
- 了解[跨租户管理体验](../concepts/cross-tenant-management-experience.md)。
- 在 Microsoft Azure 门户中转到“我的客户”，以[查看和管理客户](view-manage-customers.md)****。
