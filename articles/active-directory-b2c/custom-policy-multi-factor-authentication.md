---
title: Azure Active Directory B2C 中的多重身份验证 | Microsoft Docs
description: 如何在由 Azure Active Directory B2C 保护的面向用户的应用程序中启用多重身份验证。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/15/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 8c150235dc6b867a69b539aba9c90d666122a210
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96170440"
---
# <a name="enable-multi-factor-authentication-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中启用多重身份验证

Azure Active Directory B2C (Azure AD B2C) 直接与 [Azure AD 多重身份验证](../active-directory/authentication/concept-mfa-howitworks.md) 集成，以便你可以在应用程序中添加另一个安全层来注册和登录体验。 无需编写一行代码便可启用多重身份验证。 如果已经创建了注册和登录用户流，则仍然可以启用多重身份验证。

此功能有助于应用程序处理以下情况：

- 不需要多重身份验证即可访问一个应用程序，但需要多重身份验证才能访问另一个应用程序。 例如，客户可以使用社交或本地帐户登录汽车保险应用程序，但是必须在访问在同一目录中注册的家庭保险应用程序之前验证电话号码。
- 通常不需要多重身份验证即可访问一个应用程序，但需要它才能访问其中的敏感部分。 例如，客户可以使用社交或本地帐户登录银行应用程序并查询帐户余额，但必须在尝试进行电子转账前验证电话号码。

## <a name="set-multi-factor-authentication"></a>设置多重身份验证

1. 登录到 [Azure 门户](https://portal.azure.com)
1. 使用顶部菜单中的“目录 + 订阅”筛选器来选择包含 Azure AD B2C 租户的目录。
1. 在左侧菜单中，选择“Azure AD B2C”。 或者，选择“所有服务”并搜索并选择“Azure AD B2C”。
1. 选择“用户流”。
1. 选择要为其启用 MFA 的用户流。 例如，*B2C_1_signinsignup*。
1. 选择“属性”。
1. 在 " **多重身份验证** " 部分中，选择所需的 **MFA 方法**，然后在 " **MFA 强制** " 下选择 " **始终打开**" 或 "建议的 **[条件](conditional-access-user-flow.md) (")**。 对于 "条件"，创建 [条件性访问策略](conditional-access-identity-protection-setup.md) 策略，并指定想要将策略应用到的应用。 
1. 选择“保存”。 现在已为此用户流启用 MFA。

可以使用“运行用户流”  来验证体验。 确认以下场景：

在多重身份验证步骤发生之前，在租户中创建了一个客户帐户。 在执行此步骤期间，会要求客户提供一个电话号码并对其进行验证。 如果验证成功，则会将电话号码附加到帐户供以后使用。 即使客户取消或退出登录，也可能会要求客户在下次登录时再次验证电话号码（启用了多重身份验证时）。