---
title: 在自定义策略中定义 OAuth1 技术配置文件
titleSuffix: Azure AD B2C
description: 在 Azure Active Directory B2C 中的自定义策略中定义 OAuth 1.0 技术配置文件。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 6b54cff85da02415bbc9dfa9ead037ced48cb58f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91259418"
---
# <a name="define-an-oauth1-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>在 Azure Active Directory B2C 自定义策略中定义 OAuth1 技术配置文件

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) 提供对 [OAuth 1.0 协议](https://tools.ietf.org/html/rfc5849) 标识提供者的支持。 本文介绍了与支持此标准化协议的声明提供程序进行交互的技术配置文件的详细信息。 使用 OAuth1 技术配置文件，可以与基于 OAuth1 的标识提供者（如 Twitter）联合。 与标识提供者联合允许用户用其现有的社交或企业标识登录。

## <a name="protocol"></a>协议

“Protocol”元素的“Name”属性必须设置为 `OAuth1`。 例如，**Twitter-OAUTH1** 技术配置文件的协议为 `OAuth1`。

```xml
<TechnicalProfile Id="Twitter-OAUTH1">
  <DisplayName>Twitter</DisplayName>
  <Protocol Name="OAuth1" />
  ...
```

## <a name="input-claims"></a>输入声明

**InputClaims** 和 **InputClaimsTransformations** 元素为空或不存在。

## <a name="output-claims"></a>输出声明

**OutputClaims** 元素包含 OAuth1 标识提供者返回的声明列表。 可能需要将策略中定义的声明名称映射到标识提供者中定义的名称。 如果设置了 **DefaultValue** 属性，则还可以包含标识提供者不会返回的声明。

**OutputClaimsTransformations** 元素可能包含用于修改输出声明或生成新输出声明的 **OutputClaimsTransformation** 元素集合。

以下示例演示 Twitter 标识提供者返回的声明：

- 映射到**issuerUserId**声明的**user_id**声明。
- 映射到 **displayName** 声明的 **screen_name** 声明。
- 没有名称映射的 **email** 声明。

技术配置文件还会返回标识提供者不返回的声明：

- **identityProvider** 声明，其中包含标识提供者的名称。
- **authenticationSource** 声明，其默认值为 `socialIdpAuthentication`。

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="user_id" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
  <OutputClaim ClaimTypeReferenceId="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>元数据

| 属性 | 必须 | 说明 |
| --------- | -------- | ----------- |
| client_id | 是 | 标识提供者的应用程序标识符。 |
| ProviderName | 否 | 标识提供者的名称。 |
| request_token_endpoint | 是 | 符合 RFC 5849 规范的请求令牌终结点的 URL。 |
| authorization_endpoint | 是 | 符合 RFC 5849 规范的授权终结点的 URL。 |
| access_token_endpoint | 是 | 符合 RFC 5849 规范的令牌终结点的 URL。 |
| ClaimsEndpoint | 否 | 用户信息终结点的 URL。 |
| ClaimsResponseFormat | 否 | 声明响应格式。|

## <a name="cryptographic-keys"></a>加密密钥

**CryptographicKeys** 元素包含以下属性：

| Attribute | 必须 | 说明 |
| --------- | -------- | ----------- |
| client_secret | 是 | 标识提供者应用程序的客户端机密。   |

## <a name="redirect-uri"></a>重定向 URI

配置标识提供者的重定向 URI 时，请输入 `https://{tenant-name}.b2clogin.com/{tenant-name}.onmicrosoft.com/{policy-id}/oauth1/authresp`。 请确保将替换为 `{tenant-name}` 你的租户名称 (例如，contosob2c) ，并 `{policy-id}` 将替换为你的策略的标识符 (例如，b2c_1a_policy) 。 重定向 URI 需要采用全小写形式。 添加使用标识提供者登录名的所有策略的重定向 URL。

示例：

- [使用自定义策略添加 Twitter 作为 OAuth1 标识提供者](identity-provider-twitter-custom.md)
