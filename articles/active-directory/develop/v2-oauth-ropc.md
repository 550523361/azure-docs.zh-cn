---
title: 使用资源所有者密码凭据授权来登录 | Azure
titleSuffix: Microsoft identity platform
description: 使用资源所有者密码凭据 (ROPC) 授权可支持无浏览器身份验证流。
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 05/18/2020
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: bfc6b6fa6a2af8750c868aaacb289d39306ce06e
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2020
ms.locfileid: "83770970"
---
# <a name="microsoft-identity-platform-and-oauth-20-resource-owner-password-credentials"></a>Microsoft 标识平台和 OAuth 2.0 资源所有者密码凭据

Microsoft 标识平台支持 [OAuth 2.0 资源所有者密码凭据 (ROPC) 授权](https://tools.ietf.org/html/rfc6749#section-4.3)，后者允许应用程序通过直接处理用户的密码来登录用户。  本文介绍如何直接针对应用程序中的协议编程。  如果可能，建议改为使用受支持的 Microsoft 身份验证库 (MSAL) 来[获取令牌并调用受保护的 Web API](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)。  另请参阅[使用 MSAL 的示例应用](sample-v2-code.md)。

> [!WARNING]
> Microsoft 建议你不要使用 ROPC 流。 在大多数情况下，提供并建议使用更安全的替代项。 在应用程序中，此流需要非常高的信任度，并携带其他流中不存在的风险。 仅当不能使用其他更安全的流时，才应使用此流。

> [!IMPORTANT]
>
> * Microsoft 标识平台终结点仅支持将 ROPC 用于 Azure AD 租户而非个人帐户。 这意味着，必须使用特定于租户的终结点 (`https://login.microsoftonline.com/{TenantId_or_Name}`) 或 `organizations` 终结点。
> * 受邀加入 Azure AD 租户的个人帐户不能使用 ROPC。
> * 没有密码的帐户不能通过 ROPC 登录。 对于这种情况，建议改用适合应用的其他流。
> * 如果用户需使用[多重身份验证 (MFA)](../authentication/concept-mfa-howitworks.md) 来登录应用程序，则系统会改为阻止用户。
> * [混合联合身份验证](/azure/active-directory/hybrid/whatis-fed)方案（例如，用于对本地帐户进行身份验证的 Azure AD 和 ADFS）不支持 ROPC。 如果用户被整页重定向到本地标识提供程序，Azure AD 无法针对该标识提供程序测试用户名和密码。 不过，ROPC 支持[传递身份验证](/azure/active-directory/hybrid/how-to-connect-pta)。

## <a name="protocol-diagram"></a>协议图

下图显示了 ROPC 流。

![显示资源所有者密码凭据流的图](./media/v2-oauth2-ropc/v2-oauth-ropc.svg)

## <a name="authorization-request"></a>授权请求

ROPC 流是单一请求：它将客户端标识和用户的凭据发送到 IDP，然后接收返回的令牌。 在这样做之前，客户端必须请求用户的电子邮件地址 (UPN) 和密码。 在成功进行请求之后，客户端应立即以安全方式释放内存中的用户凭据， 而不得保存这些凭据。

> [!TIP]
> 尝试在 Postman 中执行此请求！
> [![尝试在 Postman 中执行此请求](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)


```HTTP
// Line breaks and spaces are for legibility only.  This is a public client, so no secret is required.

POST {tenant}/oauth2/v2.0/token
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=user.read%20openid%20profile%20offline_access
&username=MyUsername@myTenant.com
&password=SuperS3cret
&grant_type=password
```

| 参数 | 条件 | 说明 |
| --- | --- | --- |
| `tenant` | 必选 | 一个目录租户，用户需登录到其中。 这可采用 GUID 或友好名称格式。 此参数不能设置为 `common` 或 `consumers`，但可以设置为 `organizations`。 |
| `client_id` | 必选 | [Azure 门户 - 应用注册](https://go.microsoft.com/fwlink/?linkid=2083908)页分配给你的应用的应用程序（客户端）ID。 |
| `grant_type` | 必选 | 必须设置为 `password`。 |
| `username` | 必选 | 用户的电子邮件地址。 |
| `password` | 必选 | 用户的密码。 |
| `scope` | 建议 | 以空格分隔的[范围](v2-permissions-and-consent.md)或权限的列表，这是应用需要的。 在交互式流中，管理员或用户必须提前同意这些作用域。 |
| `client_secret`| 有时需要 | 如果你的应用是公用客户端，则不能包含 `client_secret` 或 `client_assertion`。  如果应用是机密客户端，则必须将其包含在内。 |
| `client_assertion` | 有时需要 | 使用证书生成的其他形式的 `client_secret`。  有关更多详细信息，请参阅[证书凭据](active-directory-certificate-credentials.md)。 |

### <a name="successful-authentication-response"></a>成功的身份验证响应

以下示例显示了成功的令牌响应：

```json
{
    "token_type": "Bearer",
    "scope": "User.Read profile openid email",
    "expires_in": 3599,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD..."
}
```

| 参数 | 格式 | 说明 |
| --------- | ------ | ----------- |
| `token_type` | 字符串 | 始终设置为 `Bearer`。 |
| `scope` | 空格分隔的字符串 | 如果返回了访问令牌，则此参数会列出该访问令牌的有效范围。 |
| `expires_in`| int | 包含的访问令牌的有效时间，以秒为单位。 |
| `access_token`| 不透明字符串 | 针对请求的[范围](v2-permissions-and-consent.md)颁发。 |
| `id_token` | JWT | 如果原始 `scope` 参数包含 `openid` 范围，则颁发。 |
| `refresh_token` | 不透明字符串 | 如果原始 `scope` 参数包含 `offline_access`，则颁发。 |

可以运行 [OAuth 代码流文档](v2-oauth2-auth-code-flow.md#refresh-the-access-token)中描述的同一个流，使用刷新令牌来获取新的访问令牌和刷新令牌。

### <a name="error-response"></a>错误响应

如果用户未提供正确的用户名或密码，或者客户端未收到请求的许可，则身份验证会失败。

| 错误 | 说明 | 客户端操作 |
|------ | ----------- | -------------|
| `invalid_grant` | 身份验证失败 | 凭据不正确，或者客户端没有所请求范围的许可。 如果没有授予作用域，则会返回 `consent_required` 错误。 如果发生这种情况，客户端应通过 Webview 或浏览器向用户发送交互式提示。 |
| `invalid_request` | 请求的构造方式不正确 | 授予类型在 `/common` 或 `/consumers` 身份验证上下文中不受支持。  请改用 `/organizations` 或租户 ID。 |

## <a name="learn-more"></a>了解详细信息

* 请通过[示例控制台应用程序](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2)自行试用 ROPC。
* 若要确定是否应使用 v2.0 终结点，请阅读 [Microsoft 标识平台限制](active-directory-v2-limitations.md)。
