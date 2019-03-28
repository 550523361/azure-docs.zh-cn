---
title: Azure Active Directory 条件访问中的访问控制是什么？ | Microsoft Docs
description: 了解 Azure Active Directory 条件访问中的访问控制的工作原理。
services: active-directory
keywords: 对应用的条件性访问, 使用 Azure AD 进行条件性访问, 保护对公司资源的访问, 条件性访问策略
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: ceaf472f53c48b17701b14fdf4107045c2e43fdc
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521969"
---
# <a name="what-are-access-controls-in-azure-active-directory-conditional-access"></a>Azure Active Directory 条件访问中的访问控制是什么？

使用 [Azure Active Directory (Azure AD) 条件访问](../active-directory-conditional-access-azure-portal.md)，可以控制授权用户访问云应用程序的方式。 在条件访问策略中，定义触发策略的诱因（“出现这种情况时”）的响应（“执行此操作”）。

![控制](./media/controls/10.png)

在条件访问的上下文中，

- “出现这种情况时”称为条件

- “则执行此操作”称为访问控制

条件语句与控制的组合表示一种条件性访问策略。

![控制](./media/controls/61.png)

每个控制要么限制人员或系统必须满足哪些要求才能登录，要么限制用户在登录后可以执行哪些操作。

访问控制分为两种类型：

- **授权控制** - 旨在限制访问

- **会话控制** - 旨在限制可以在会话中执行的操作

本主题介绍了 Azure AD 条件访问中的各种控制。 

## <a name="grant-controls"></a>授权控件

使用授权控制，可以完全阻止访问，也可以选择所需的控制，限制为只有满足其他要求才能访问。 如果有多个控制，可以要求：

- 满足所有选定控制 (AND)
- 满足一个选定控制 (OR)

![控制](./media/controls/17.png)

### <a name="multi-factor-authentication"></a>多重身份验证

此控制可用于要求必须通过多重身份验证，才能访问指定的云应用程序。 此控制支持以下多重身份验证提供程序：

- Azure 多重身份验证

- 本地多重身份验证提供程序，与 Active Directory 联合身份验证服务 (AD FS) 结合使用。

使用多重身份验证有助于保护资源，使其免遭可能已有权访问有效用户的主要凭据的未授权用户访问。

### <a name="compliant-device"></a>兼容设备

可以配置基于设备的条件访问策略。 基于设备的条件性访问策略旨在仅从[受管理设备](require-managed-devices.md)授予对所选云应用的访问权限。 要求将设备标记为合规是限制对受管理设备的访问可以选择的一个选项。 可以通过 Intune（适用于任何设备 OS）或通过适用于 Windows 10 设备的第三方 MDM 系统将设备标记为合规。 不支持除 Windows 10 以外的设备 OS 类型的第三方 MDM 系统。 

你的设备需要先注册到 Azure AD，然后才能将其标记为合规。 若要注册设备，你有三种选择： 

- [Azure AD 注册设备](../devices/overview.md#azure-ad-registered-devices)
- [已加入 Azure AD 的设备](../devices/overview.md#azure-ad-joined-devices)  
- [已加入混合 Azure AD 的设备](../devices/overview.md#hybrid-azure-ad-joined-devices)

有关详细信息，请参阅[如何通过条件访问要求使用受管理设备进行云应用访问](require-managed-devices.md)。

### <a name="hybrid-azure-ad-joined-device"></a>混合 Azure AD 加入设备

若要配置基于设备的条件访问策略，还可以要求必须为混合 Azure AD 加入设备。 此要求是指已加入本地 Active Directory 的 Windows 台式机、笔记本电脑和企业平板电脑。 如果选择了此选项，则条件访问策略会授权访问通过加入到本地 Active Directory 和 Azure Active Directory 的设备所做的尝试。  

有关详细信息，请参阅[设置 Azure Active Directory 基于设备的条件访问策略](require-managed-devices.md)。

### <a name="approved-client-app"></a>核准客户端应用程序

由于员工使用移动设备执行个人和工作任务，因此可能需要能够保护设备访问的公司数据，即使这些设备不受你管理，也不例外。
可以使用 [Intune 应用程序保护策略](https://docs.microsoft.com/intune/app-protection-policy)，帮助保护公司数据，而不受任何移动设备管理 (MDM) 解决方案的影响。

利用核准客户端应用程序，可以要求客户端应用程序必须支持 [Intune 应用程序保护策略](https://docs.microsoft.com/intune/app-protection-policy)，才能访问云应用程序。 例如，可以限制为只有 Outlook 应用程序，才能访问 Exchange Online。 要求必须为核准客户端应用程序的条件访问策略亦称为[基于应用程序的条件访问策略](app-based-conditional-access.md)。 有关支持的核准客户端应用程序列表，请参阅[核准客户端应用程序要求](technical-reference.md#approved-client-app-requirement)。

### <a name="terms-of-use"></a>使用条款

在向某个资源授予访问权限之前，可以要求租户中的用户同意相关使用条款。 作为管理员，可以通过上传 PDF 文档配置和自定义使用条款。 如果用户属于此控制范围，则仅在同意使用条款的情况下才授予某个应用程序的访问权限。

### <a name="custom-controls-preview"></a>自定义控件(预览版)

可以在条件访问中创建将用户重定向至兼容服务的自定义控件，以满足 Azure Active Directory 之外的其他要求。 这允许用户使用某个外部的多重身份验证和验证提供程序，强制实施条件访问规则或建立自己的自定义服务。 若要满足此控件要求，用户浏览器将重定向至外部服务，执行任何需要的身份验证或验证活动，然后重定向回 Azure Active Directory。 如果用户已成功完成身份验证或验证，该用户将继续留在条件访问流中。 

## <a name="custom-controls"></a>自定义控件

自定义控件是 Azure Active Directory Premium P1 版的一项功能。 使用自定义控件时，用户将被重定向至兼容服务，以满足 Azure Active Directory 之外的其他要求。 若要满足此控件要求，用户浏览器将重定向至外部服务，执行任何需要的身份验证或验证活动，然后重定向回 Azure Active Directory。 Azure Active Directory 将验证响应，如果用户已成功完成身份验证或验证，该用户将继续留在条件访问流中。

通过这些控件可以将某些外部或自定义服务用作条件访问控制，并在一般情况下扩展条件访问的功能。

提供商当前提供的兼容服务包括：

- [Duo Security](https://duo.com/docs/azure-ca)
- [Entrust Datacard](https://www.entrustdatacard.com/products/authentication/intellitrust)
- [Ping 标识](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_AzureADIntegration.html)
- RSA
- [SecureAuth](https://docs.secureauth.com/pages/viewpage.action?pageId=47238992#)
- [Silverfort](https://www.silverfort.io/company/using-silverfort-mfa-with-azure-active-directory/)
- [Symantec VIP](https://help.symantec.com/home/VIP_Integrate_with_Azure_AD)
- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

有关这些服务的详细信息，请直接与提供商联系。

### <a name="creating-custom-controls"></a>创建自定义控件

若要创建自定义控件，应首先联系想使用的控件的提供商。 每个非 Microsoft 提供商在注册、订阅或以其他方式加入服务以及指示想要与条件访问集成方面都有自己的进程和要求。 此时，提供商将提供采用 JSON 格式的数据块。 使用此数据可使提供商和条件访问一起服务于租户，创建新控件，并确定条件访问如何判断用户是否通过提供商成功执行了验证。

复制 JSON 数据，然后将其粘贴到相关文本框中。 不要对 JSON 做任何更改，除非用户明确理解所做的更改。 做出任何更改可能中断提供商和 Microsoft 之间的联系，并且有可能将你和你的用户锁定在帐户之外。

创建自定义控件的选项位于“条件访问”页的“管理”部分中。

![控制](./media/controls/82.png)

单击“新建自定义控件”，打开包含控件 JSON 数据文本框的边栏选项卡。  

![控制](./media/controls/81.png)

### <a name="deleting-custom-controls"></a>删除自定义控件

若要删除自定义控件，必须先确定它未在任何条件访问策略中使用。 完成后：

1. 转到“自定义控件”列表

2. 单击...  

3. 选择“删除” 。

### <a name="editing-custom-controls"></a>编辑自定义控件

若要编辑自定义控件，必须删除当前控件，然后使用更新的信息创建新控件。

## <a name="session-controls"></a>会话控制

可通过会话控件在云应用中获取有限体验。 会话控制由云应用强制实施，取决于由 Azure AD 提供给应用的有关会话的其他信息。

![控制](./media/controls/31.png)

### <a name="use-app-enforced-restrictions"></a>使用应用强制实施的限制

可以使用此控制要求 Azure AD 将设备信息传递给所选云应用。 设备信息使云应用能够知道连接是从兼容设备还是已加入域设备发起的。 此控制仅支持将 SharePoint Online 和 Exchange Online 作为选定的云应用。 选择后，云应用会使用设备信息为用户提供有限或完整的体验，具体取决于设备状态。

若要了解更多信息，请参阅以下文章：

- [使用 SharePoint Online 启用受限访问](https://aka.ms/spolimitedaccessdocs)

- [使用 Exchange Online 启用受限访问](https://aka.ms/owalimitedaccess)

## <a name="next-steps"></a>后续步骤

- 若要了解如何配置条件访问策略，请参阅[通过 Azure Active Directory 条件访问要求特定应用必须使用 MFA](app-based-mfa.md)。

- 如果已准备好配置环境的条件访问策略，请参阅 [Azure Active Directory 中条件访问的最佳做法](best-practices.md)。
