---
title: 通过自定义策略（预览版）进行电话注册和登录
titleSuffix: Azure AD B2C
description: 通过 Azure Active Directory B2C 中的自定义策略，将文本消息中的一次性密码（OTP）发送到你的应用程序用户的手机。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: eadac0e973b361b1fdee63dcc9cfa848a0b2bacb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "78183952"
---
# <a name="set-up-phone-sign-up-and-sign-in-with-custom-policies-in-azure-ad-b2c-preview"></a>在 Azure AD B2C （预览版）中设置自定义策略的手机注册和登录

通过在 Azure Active Directory B2C （Azure AD B2C）中进行电话注册和登录，用户可以通过使用短信发送到其手机的一次性密码（OTP）来注册应用程序并登录到你的应用程序。 一次性密码可帮助最大程度地减少用户忘记密码或密码泄露的风险。

按照本文中的步骤使用自定义策略，使客户能够使用发送到其电话的一次性密码来注册和登录到你的应用程序。

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="pricing"></a>定价

一次性密码会通过使用短信发送给用户，并且你可能会对每个发送的消息付费。 有关定价信息，请参阅[Azure Active Directory B2C 定价](https://azure.microsoft.com/pricing/details/active-directory-b2c/)的**单独费用**部分。

## <a name="prerequisites"></a>先决条件

在设置 OTP 之前，需要准备好以下资源。

* [Azure AD B2C 租户](tutorial-create-tenant.md)
* 在租户中[注册的 Web 应用程序](tutorial-register-applications.md)
* 已上传到租户的[自定义策略](custom-policy-get-started.md)

## <a name="get-the-phone-sign-up--sign-in-starter-pack"></a>获取电话注册 & 登录初学者包

首先更新电话注册和登录自定义策略文件，以便与 Azure AD B2C 租户配合使用。

以下步骤假设已完成[先决条件](#prerequisites)，并已将[自定义策略初学者包][starter-pack]存储库克隆到本地计算机。

1. 在 starter pack 存储库的本地克隆中查找[电话注册和登录自定义策略文件][starter-pack-phone]，或直接下载它们。 XML 策略文件位于以下目录中：

    `active-directory-b2c-custom-policy-starterpack/scenarios/`**`phone-number-passwordless`**

1. 在每个文件中，将`yourtenant`字符串替换为 Azure AD B2C 租户的名称。 例如，如果 B2C 租户的名称为*contosob2c*，则的`yourtenant.onmicrosoft.com`所有实例都将变为`contosob2c.onmicrosoft.com`。

1. 完成[Azure Active Directory B2C 中的自定义策略入门](custom-policy-get-started.md)中的[将应用程序 id 添加到自定义策略](custom-policy-get-started.md#add-application-ids-to-the-custom-policy)部分中的步骤。 在这种情况下`/phone-number-passwordless/` **`Phone_Email_Base.xml`** ，请在完成必备组件、 *IdentityExperienceFramework*和*ProxyIdentityExperienceFramework*时，用注册的两个应用程序的**应用程序（客户端） id**进行更新。

## <a name="upload-the-policy-files"></a>上传策略文件

1. 登录到[Azure 门户](https://portal.azure.com)并导航到 Azure AD B2C 租户。
1. 在“策略”下，选择“Identity Experience Framework”。********
1. 选择 "**上载自定义策略**"。
1. 按以下顺序上传策略文件：
    1. *Phone_Email_Base .xml*
    1. *SignUpOrSignInWithPhone*
    1. *SignUpOrSignInWithPhoneOrEmail*
    1. *ProfileEditPhoneOnly*
    1. *ProfileEditPhoneEmail*
    1. *ChangePhoneNumber*
    1. *PasswordResetEmail*

上传每个文件时，Azure 会添加`B2C_1A_`前缀。

## <a name="test-the-custom-policy"></a>测试自定义策略

1. 在 "**自定义策略**" 下，选择**B2C_1A_SignUpOrSignInWithPhone**。
1. 在 "**选择应用程序**" 下，选择在完成先决条件时注册的*webapp1*应用程序。
1. 对于 "**选择回复 url**" `https://jwt.ms`，请选择。
1. 选择 "**立即运行**" 并使用电子邮件地址或电话号码进行注册。
1. 选择 "**立即运行**" 并使用同一帐户登录，以确认配置正确。

## <a name="get-user-account-by-phone-number"></a>按电话号码获取用户帐户

使用电话号码注册但未提供恢复电子邮件地址的用户在 Azure AD B2C 目录中记录为其电话号码作为登录名。 如果用户希望更改其电话号码，则支持人员或支持团队必须首先查找其帐户，然后更新他们的电话号码。

你可以使用[Microsoft Graph](manage-user-accounts-graph-api.md)按其电话号码（登录名）查找用户：

```http
GET https://graph.microsoft.com/v1.0/users?$filter=identities/any(c:c/issuerAssignedId eq '+{phone number}' and c/issuer eq '{tenant name}.onmicrosoft.com')
```

例如：

```http
GET https://graph.microsoft.com/v1.0/users?$filter=identities/any(c:c/issuerAssignedId eq '+450334567890' and c/issuer eq 'contosob2c.onmicrosoft.com')
```

## <a name="next-steps"></a>后续步骤

可在 GitHub 上找到电话注册和登录自定义策略初学者包（以及其他初学者包）：

[Azure-示例/active directory-active-directory-b2c-custom-policy-starterpack/方案/电话号码-无密码][starter-pack-phone]

初学者包策略文件使用多重身份验证技术配置文件和电话号码声明转换：

* [定义 Azure 多重身份验证技术配置文件](multi-factor-auth-technical-profile.md)
* [定义电话号码声明转换](phone-number-claims-transformations.md)

<!-- LINKS - External -->
[starter-pack]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
[starter-pack-phone]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/phone-number-passwordless
