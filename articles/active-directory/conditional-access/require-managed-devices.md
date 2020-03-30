---
title: 条件访问需要托管设备 - Azure 活动目录
description: 了解如何配置 Azure Active Directory (Azure AD) 基于设备的条件访问策略来要求使用受管理设备进行云应用访问。
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 11/22/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a3c71534febc3cdb6429d3092225ebc73f6cbe7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481477"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>如何：需要托管设备才能通过条件访问进行云应用访问

在移动优先、云优先的世界中，使用 Azure Active Directory (Azure AD) 可以实现从任意位置以单一登录方式登录到应用和服务。 经授权的用户可以从一系列设备（包括移动设备和个人设备）访问云应用。 但是，许多环境中至少有几个应用会要求仅通过满足你的安全性和符合性标准的设备进行访问。 这些设备也称为受管理设备。 

本文介绍了如何配置条件访问策略来要求使用受管理设备访问环境中的特定云应用。 

## <a name="prerequisites"></a>先决条件

需要托管设备进行云应用访问将**Azure AD 条件访问**和**Azure AD 设备管理**联系在一起。 如果尚不熟悉其中的某项功能，应该先阅读以下主题:

- **[Azure 活动目录中的条件访问](../active-directory-conditional-access-azure-portal.md)**- 本文为您提供了条件访问和相关术语的概念概述。
- **[Azure Active Directory 中的设备管理简介](../devices/overview.md)** - 此文章概述了可以用来将设备置于组织控制下的各种选项。 
- 有关 Windows **10 创建者更新（版本 1703）** 或更高版本的 Chrome 支持，请安装[Windows 10 帐户扩展名](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji)。 当条件访问策略需要特定于设备的详细信息时，此扩展是必需的。

>[!NOTE] 
> 我们建议使用基于 Azure AD 设备的条件访问策略，在初始设备身份验证后获得最佳实施。 这包括如果设备不符合合规性和设备代码流，则结束会话。


## <a name="scenario-description"></a>方案描述

掌控安全性与工作效率之间的平衡是一个难题。 扩大用来访问云资源的受支持设备的范围有助于提高用户的工作效率。 另一方面，你可能不希望具有未知保护级别的设备访问你的环境中的某些资源。 对于受影响的资源，你应当要求用户只能使用受管理设备访问它们。 

使用 Azure AD 条件访问，可以通过进行授权的以下单个策略来解决此要求：

- 授予对所选云应用的访问权限
- 为所选用户和组授予权限
- 要求使用受管理设备

## <a name="managed-devices"></a>受管理设备  

简而言之，受管理设备是指处于*某种*组织控制之下的设备。 在 Azure AD 中，受管理设备的先决条件是它已向 Azure AD 注册。 注册设备时会以设备对象的形式为设备创建标识。 Azure 使用此对象来跟踪设备的状态信息。 作为 Azure AD 管理员，你可以使用此对象切换（启用/禁用）设备状态。
  
![基于设备的条件](./media/require-managed-devices/32.png)

若要向 Azure AD 注册设备，你有三种选择： 

- **Azure AD 注册设备**：向 Azure AD 注册个人设备
- **加入 Azure AD 的设备** - 向 Azure AD 注册未加入本地 AD 的组织 Windows 10 设备。 
- **加入混合 Azure AD 的设备** - 向 Azure AD 注册已加入本地 AD 的 Windows 10 或受支持的低级别设备。

这三个选项在本文中讨论了[什么是设备标识？](../devices/overview.md)

若要成为受管理设备，注册设备必须是**加入混合 Azure AD 的设备**或者是**已标记为合规的设备**。  

![基于设备的条件](./media/require-managed-devices/47.png)
 
## <a name="require-hybrid-azure-ad-joined-devices"></a>要求使用加入混合 Azure AD 的设备

在条件访问策略中，可以选择 **"需要混合 Azure AD 联接设备**"以声明只能使用托管设备访问选定的云应用。 

![基于设备的条件](./media/require-managed-devices/10.png)

此设置仅适用于已加入本地 AD 的 Windows 10 或低级别设备（例如 Windows 7 或 Windows 8）。 你只能使用混合 Azure AD 加入功能向 Azure AD 注册这些设备，这是一种注册 Windows 10 设备的[自动化过程](../devices/hybrid-azuread-join-plan.md)。 

![基于设备的条件](./media/require-managed-devices/45.png)

怎样使加入混合 Azure AD 的设备成为受管理设备？  对于已加入本地 AD 的设备，假定使用管理解决方案（如**配置管理器**或**组策略 （GP））** 来强制执行对这些设备的控制。这些解决方案用于管理这些设备。 由于 Azure AD 无法确定是否已向设备应用这些方法中的任何一种，因此，在要求使用受管理设备的情况下，要求使用加入混合 Azure AD 的设备是一种相对较弱的机制。 如果加入本地域的设备同时也是加入混合 Azure AD 的设备，则由管理员判断应用于此类设备的方法是否强大到足以使其成为受管理设备。

## <a name="require-device-to-be-marked-as-compliant"></a>要求将设备标记为合规

“要求将设备标记为合规”选项是一种用于请求受管理设备的最强大的形式**。

![基于设备的条件](./media/require-managed-devices/11.png)

此选项要求向 Azure AD 注册设备，此外还要求通过以下方式将该设备标记为合规：
         
- Intune
- 第三方移动设备管理 (MDM) 系统，该系统通过 Azure AD 集成管理 Windows 10 设备。 不支持除 Windows 10 以外的设备 OS 类型的第三方 MDM 系统。
 
![基于设备的条件](./media/require-managed-devices/46.png)

对于标记为合规的设备，你可以假设： 

- 员工用来访问公司数据的移动设备是受管理设备
- 员工使用的移动应用是受管理应用
- 通过帮助控制员工访问和共享公司信息的方式，为公司信息提供保护
- 该设备及其应用符合公司安全要求

### <a name="known-behavior"></a>已知行为

在 Windows 7、iOS、Android、macOS 和某些第三方 Web 浏览器上，Azure AD 使用客户端证书来标识设备，该证书是在向 Azure AD 注册设备时预配的。 用户首次通过浏览器登录时，系统会提示用户选择此证书。 最终用户必须选择此证书才能继续使用浏览器。

## <a name="next-steps"></a>后续步骤

在环境中配置基于设备的条件访问策略之前，应了解[Azure 活动目录中的条件访问最佳做法](best-practices.md)。
