---
title: 在 Azure Active Directory B2C 中使用 OpenID Connect 进行 Web 登录 | Microsoft Docs
description: 通过使用 OpenID Connect 身份验证协议的 Azure Active Directory 实现构建 Web 应用程序。
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f844cc0b798c035e31b45ef00370f95b1ec06d43
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2019
ms.locfileid: "54846024"
---
# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C：使用 OpenID Connect 进行 Web 登录
OpenID Connect 是构建在 OAuth 2.0 基础之上的身份验证协议，可用于将用户安全登录到 Web 应用程序。 通过使用 OpenID Connect 的 Azure Active Directory B2C (Azure AD B2C) 实现，可以将 Web 应用程序中的注册、登录和其他标识管理体验转移到 Azure Active Directory (Azure AD) 中。 本指南演示如何使用与语言无关的方式执行此操作。 介绍在不使用我们的任何开放源代码库的情况下，如何发送和接收 HTTP 消息。

[OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) 扩展了 OAuth 2.0 *授权*协议，将其用作*身份验证*协议。 这样可以通过使用 OAuth 执行单一登录。 它引入了 ID 令牌的概念，这是一种安全令牌，可让客户端验证用户的标识，并获取有关用户的基本配置文件信息。

因为 OpenID Connect 扩展了 OAuth 2.0，因此，它还能使应用安全地获取访问令牌。 可以使用 access_token 访问由[授权服务器](active-directory-b2c-reference-protocols.md#the-basics)保护的资源。 如果要构建的 Web 应用程序托管在服务器中并通过浏览器访问，我们建议使用 OpenID Connect。 如果要使用 Azure AD B2C 向移动或桌面应用程序添加标识管理，则应使用 [OAuth 2.0](active-directory-b2c-reference-oauth-code.md)，而不是 OpenID Connect。

Azure AD B2C 扩展了标准 OpenID Connect 协议，使其功能远远超出了简单的身份验证和授权。 它引入了[用户流参数](active-directory-b2c-reference-policies.md)，通过此参数可以使用 OpenID Connect 向应用添加用户体验，例如注册、登录和配置文件管理。 在这里，我们将演示如何使用 OpenID Connect 和用户流在 Web 应用中实现每个体验。 我们还将展示如何获取用于访问 Web API 的访问令牌。

下一节中的示例 HTTP 请求将使用示例 B2C 目录、fabrikamb2c.onmicrosoft.com 以及示例应用程序、 https://aadb2cplayground.azurewebsites.net 和用户流。 可以随意使用这些值亲自尝试这些请求，或者可以用自己的值替换它们。
了解如何[获取自己的 B2C 租户、应用程序和用户流](#use-your-own-b2c-directory)。

## <a name="send-authentication-requests"></a>发送身份验证请求
当 Web 应用需要对用户进行身份验证并执行用户流时，它可以将用户定向到 `/authorize` 终结点。 这是流程的交互部分，在此部分中，用户会根据用户流执行一些操作。

在此请求中，客户端指示在 `scope` 参数中需要从用户获取的权限以及要在 `p` 参数中执行的用户流。 以下部分中提供了三个示例（带换行符以便阅读），每个示例使用不同的用户流。 要了解每个请求的工作原理，请尝试将请求粘贴到浏览器并运行它。

#### <a name="use-a-sign-in-user-flow"></a>使用登录用户流
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-user-flow"></a>使用注册用户流
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-user-flow"></a>使用编辑配置文件用户流
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| 参数 | 必需？ | 说明 |
| --- | --- | --- |
| client_id |必选 |Azure 门户分配给应用的[应用程序 ID](https://portal.azure.com/) 。 |
| response_type |必选 |响应类型，其中必须包括用于 OpenID Connect 的 ID 令牌。 如果 Web 应用还需要使用令牌来调用 Web API，则可以使用 `code+id_token`，正如我们在此处操作的一样。 |
| redirect_uri |建议 |应用的 `redirect_uri` 参数，应用可在此发送及接收身份验证响应。 它必须完全匹配在门户中注册的其中一个 `redirect_uri` 参数，但必须经过 URL 编码。 |
| 作用域 |必选 |范围的空格分隔列表。 一个范围值，该值向 Azure AD 指示正在请求的两个权限。 `openid` 作用域表示允许使用 ID 令牌的形式使用户登录并获取有关用户的数据（本文稍后对此进行详述）。 `offline_access` 作用域对 Web 应用是可选的。 它表示应用需要刷新令牌才能长期访问资源。 |
| response_mode |建议 |将生成的授权代码发送回应用所应该使用的方法。 这可以是 `query`、`form_post` 或 `fragment`。  建议使用 `form_post` 响应模式以获得最佳安全性。 |
| state |建议 |同时随令牌响应返回的请求中所包含的值。 它可以是用户想要的任何内容的字符串。 随机生成的唯一值通常用于防止跨站点请求伪造攻击。 该状态也用于在身份验证请求出现之前，在应用程序中编码用户的状态信息，例如用户之前所在的页面。 |
| nonce |必选 |包含在请求中的值（由应用生成），将以声明方式包含在生成的 ID 令牌中。 应用程序接着便可确认此值，以减少令牌重新执行攻击。 此值通常是随机的唯一字符串，可用以识别请求的来源。 |
| p |必选 |将执行的用户流。 它是在 B2C 租户中创建的用户流的名称。 用户流名称值应以 `b2c\_1\_` 开头。 了解有关策略和[可扩展用户流框架](active-directory-b2c-reference-policies.md)的详细信息。 |
| prompt |可选 |需要的用户交互类型。 此时唯一有效的值为 `login`，这会强制用户在该请求上输入其凭据。 单一登录不会生效。 |

此时，要求用户完成用户流的工作流。 这可能涉及用户输入其用户名和密码、用社交标识登录、注册目录，或任何其他若干步骤，具体取决于如何定义用户流。

用户完成用户流后，Azure AD 使用 `response_mode` 参数中指定的方法，将响应返回到位于所指示的 `redirect_uri` 参数的应用。 对于上述每种情况，响应均相同，与执行的用户流无关。

使用 `response_mode=fragment` 的成功的响应如下所示：

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| 参数 | 说明 |
| --- | --- |
| id_token |应用请求的 ID 令牌。 可以使用 ID 令牌验证用户的身份，并开始与用户的会话。 有关 ID 令牌及其内容的更多详细信息，请参阅 [Azure AD B2C 令牌参考](active-directory-b2c-reference-tokens.md)。 |
| 代码 |应用请求的授权代码（如果使用 `response_type=code+id_token`）。 应用可以使用授权代码请求目标资源的访问令牌。 授权代码的生存期非常短。 通常，它们在约 10 分钟后过期。 |
| state |如果请求中包含 `state` 参数，响应中应该出现相同的值。 应用需验证请求和响应中的 `state` 值是否相同。 |

错误响应也可能会被发送到 `redirect_uri` 参数，以便应用对它们进行恰当的处理：

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| 参数 | 说明 |
| --- | --- |
| error |用于分类发生的错误类型和响应错误的错误代码字符串。 |
| error_description |帮助开发人员识别身份验证错误根本原因的特定错误消息。 |
| state |请参阅本节第一个表中的完整说明。 如果请求中包含 `state` 参数，响应中应该出现相同的值。 应用需验证请求和响应中的 `state` 值是否相同。 |

## <a name="validate-the-id-token"></a>验证 ID 令牌
仅收到一个 ID 令牌并不表示可以对用户进行身份验证。 还必须根据应用的要求验证 ID 令牌的签名和令牌中的声明。 Azure AD B2C 使用 [JSON Web 令牌 (JWT)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) 和公钥加密对令牌进行签名并验证其是否有效。

有许多开放源代码库可用于验证 JWT，具体取决于首选语言。 我们建议使用这些库，而不是实施自己的验证逻辑。 此处的信息有助于弄清如何正确使用这些库。

Azure AD B2C 具有 OpenID Connect 元数据终结点，允许应用程序在运行时提取有关 Azure AD B2C 的信息。 此信息包括终结点、令牌内容和令牌签名密钥。 B2C 租户中的每个用户流都有一个 JSON 元数据文档。 例如，`fabrikamb2c.onmicrosoft.com` 中 `b2c_1_sign_in` 用户流的元数据文档位于：

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

此配置文档的一个属性为 `jwks_uri`，对于相同用户流，该属性的值为：

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`。

若要确定对 ID 令牌进行签名所使用的用户流（以及从何处获取元数据），可以使用两种方法。 第一种方法，用户流名称包含在 ID 令牌的 `acr` 声明中。 有关如何从 ID 令牌中解析声明的信息，请参阅 [Azure AD B2C 令牌参考](active-directory-b2c-reference-tokens.md)。 另一个方法是在发出请求时在 `state` 参数的值中对用户流进行编码，然后对其进行解码以确定使用的用户流。 任意一种方法均有效。

从 OpenID Connect 元数据终结点获取元数据文档后，可以使用 RSA 256 公钥（位于此终结点上）来验证 ID 令牌的签名。 在任何指定的时间点，此终结点上可能列出多个密钥，每个密钥使用 `kid` 声明进行标识。 ID 令牌的标头还包含 `kid` 声明，该声明指示哪个密钥用于对 ID 令牌进行签名。 有关详细信息，请参阅 [Azure AD B2C 令牌参考](active-directory-b2c-reference-tokens.md)（特别是[验证令牌](active-directory-b2c-reference-tokens.md#token-validation)部分）。
<!--TODO: Improve the information on this-->

验证 ID 令牌的签名后，还有几项声明需要验证。 例如：

* 需要验证 `nonce` 声明以防止令牌重放攻击。 其值应为在登录请求中指定的内容。
* 需要验证 `aud` 声明以确保已为应用发出 ID 令牌。 其值应为应用的应用程序 ID。
* 需要验证 `iat` 和 `exp` 声明以确保 ID 令牌未过期。

此外，还需要执行更多的一些验证。 有关详细信息，请参阅 [OpenID Connect 核心规范](https://openid.net/specs/openid-connect-core-1_0.html)。根据情况，可能还希望验证其他声明。 一些常见的验证包括：

* 确保用户/组织已注册应用。
* 确保用户拥有正确的授权/权限。
* 确保执行了一定强度的身份验证，例如 Azure 多重身份验证。

有关 ID 令牌中声明的详细信息，请参阅 [Azure AD B2C 令牌参考](active-directory-b2c-reference-tokens.md)。

验证 ID 令牌后，可以开始与用户的会话。 在应用中，可以使用 ID 令牌中的声明来获取用户的相关信息。 此信息的用途包括显示、记录和授权。

## <a name="get-a-token"></a>获取令牌
如果仅需要 Web 应用执行用户流，则可以跳过下面几节。 这些章节仅适用于需要对 Web API 进行验证的调用，以及受到 Azure AD B2C 保护的 Web 应用。

通过将 `POST` 请求发送到 `/token` 终结点，可以将获取的授权代码（通过 `response_type=code+id_token` 获取）兑换为所需资源的令牌。 当前可以为其请求令牌的唯一资源是应用的后端 Web API。 用于向自己请求令牌的约定是使用应用的客户端 ID 作为作用域：

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| 参数 | 必需？ | 说明 |
| --- | --- | --- |
| p |必选 |用于获取授权代码的用户流。 无法在此请求中使用不同的用户流。 请注意，将此参数添加到查询字符串中，而不是添加到 `POST` 正文中。 |
| client_id |必选 |Azure 门户分配给应用的[应用程序 ID](https://portal.azure.com/) 。 |
| grant_type |必选 |授予类型，该类型必须是授权代码流的 `authorization_code`。 |
| 作用域 |建议 |范围的空格分隔列表。 一个范围值，该值向 Azure AD 指示正在请求的两个权限。 `openid` 作用域表示允许使用 id_token 参数的形式使用户登录并获取有关用户的数据。 它可以用于为应用的后端 Web API 获取令牌，该令牌使用和客户端相同的应用程序 ID 表示。 `offline_access` 作用域表示应用需要刷新令牌才能获得访问资源的长生存期。 |
| 代码 |必选 |在流的第一个阶段获取的授权代码。 |
| redirect_uri |必选 |在其中收到授权代码的应用程序的 `redirect_uri` 参数。 |
| client_secret |必选 |在 [Azure 门户](https://portal.azure.com/)中生成的应用程序密码。 此应用程序密码是重要的安全项目。 应将其安全地存储在服务器上。 你还应定期更新此客户端密码。 |

成功的令牌响应如下所示：

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| 参数 | 说明 |
| --- | --- |
| not_before |epoch 时间中令牌被视为有效的时间。 |
| token_type |令牌类型值。 Azure AD 支持的唯一类型是 `Bearer`。 |
| access_token |请求的已签名的 JWT 令牌。 |
| 作用域 |令牌的有效范围。 可用于缓存令牌以供以后使用。 |
| expires_in |访问令牌有效的时间长度（以秒为单位）。 |
| refresh_token |OAuth 2.0 刷新令牌。 应用可以使用此令牌，在当前令牌过期之后获取其他令牌。 刷新令牌的生存期很长，而且可以用于延长保留资源访问权限的时间。 有关更多详细信息，请参阅 [B2C 令牌参考](active-directory-b2c-reference-tokens.md)。 请注意，必须在授权和令牌请求中都使用作用域 `offline_access`，才能接收刷新令牌。 |

错误响应如下所示：

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| 参数 | 说明 |
| --- | --- |
| error |用于分类发生的错误类型和响应错误的错误代码字符串。 |
| error_description |帮助开发人员识别身份验证错误根本原因的特定错误消息。 |

## <a name="use-the-token"></a>使用令牌
现在你已成功获取访问令牌，可通过在 `Authorization` 标头中加入令牌的方式，在后端 Web API 请求中使用该令牌：

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>刷新令牌
ID 令牌的生存期较短。 必须在它们过期后将其刷新才能继续访问资源。 为此，可以向 `/token` 终结点提交另一个 `POST` 请求。 此时，提供 `refresh_token` 参数而不是 `code` 参数：

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| 参数 | 必选 | 说明 |
| --- | --- | --- |
| p |必选 |用于获取原始刷新令牌的用户流。 无法在此请求中使用不同的用户流。 请注意，将此参数添加到查询字符串中，而不是添加到 POST 正文中。 |
| client_id |必选 |Azure 门户分配给应用的[应用程序 ID](https://portal.azure.com/) 。 |
| grant_type |必选 |授予类型，该类型必须是这一段授权代码流的刷新令牌。 |
| 作用域 |建议 |范围的空格分隔列表。 一个范围值，该值向 Azure AD 指示正在请求的两个权限。 `openid` 作用域表示允许使用 ID 令牌的形式使用户登录并获取有关用户的数据。 它可以用于为应用的后端 Web API 获取令牌，该令牌使用和客户端相同的应用程序 ID 表示。 `offline_access` 作用域表示应用需要刷新令牌才能获得访问资源的长生存期。 |
| redirect_uri |建议 |在其中收到授权代码的应用程序的 `redirect_uri` 参数。 |
| refresh_token |必选 |在流的第二个阶段获取的原始刷新令牌。 请注意，必须在授权和令牌请求中都使用作用域 `offline_access`，才能接收刷新令牌。 |
| client_secret |必选 |在 [Azure 门户](https://portal.azure.com/)中生成的应用程序密码。 此应用程序密码是重要的安全项目。 应将其安全地存储在服务器上。 你还应定期更新此客户端密码。 |

成功的令牌响应如下所示：

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| 参数 | 说明 |
| --- | --- |
| not_before |epoch 时间中令牌被视为有效的时间。 |
| token_type |令牌类型值。 Azure AD 支持的唯一类型是 `Bearer`。 |
| access_token |请求的已签名的 JWT 令牌。 |
| 作用域 |令牌的有效范围，可用于缓存令牌以供以后使用。 |
| expires_in |访问令牌有效的时间长度（以秒为单位）。 |
| refresh_token |OAuth 2.0 刷新令牌。 应用可以使用此令牌，在当前令牌过期之后获取其他令牌。  刷新令牌的生存期很长，而且可以用于延长保留资源访问权限的时间。 有关更多详细信息，请参阅 [B2C 令牌参考](active-directory-b2c-reference-tokens.md)。 |

错误响应如下所示：

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| 参数 | 说明 |
| --- | --- |
| error |用于分类发生的错误类型和响应错误的错误代码字符串。 |
| error_description |帮助开发人员识别身份验证错误根本原因的特定错误消息。 |

## <a name="send-a-sign-out-request"></a>发送注销请求
如果想要从应用程序中注销用户，只是清除应用程序的 cookie 或者结束与用户的会话是不够的。 还必须将用户重定向到 Azure AD 进行注销。如果没有这么做，那么用户可能可以在应用程序中重新进行身份验证，且无需再次输入其凭据。 这是因为他们具有和 Azure AD 的有效单一登录会话。

只需将用户重定向到前面章节“验证 ID 令牌”中所述的 OpenID Connect 元数据文档中列出的 `end_session` 终结点：

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| 参数 | 必需？ | 说明 |
| --- | --- | --- |
| p |必选 |想要用于从应用程序中注销用户的用户流。 |
| post_logout_redirect_uri |建议 |用户在成功注销后应重定向到的 URL。如果未包含此参数，Azure AD B2C 会向用户显示一条常规消息。 |

> [!NOTE]
> 尽管将用户定向到 `end_session` 终结点将清除用户的某些 Azure AD B2C 的单一登录状态，但是不会将用户从其社交标识提供者 (IDP) 会话中注销。 如果用户在后续登录中选择相同的 IDP，他们将重新进行身份验证，且无需输入其凭据。 如果用户想要注销 B2C 应用程序，并不表示他们想要注销其 Facebook 帐户。 但是，如果是本地帐户，则以正确的方式结束用户的会话。
> 
> 

## <a name="use-your-own-b2c-tenant"></a>使用自己的 B2C 租户
如果你想自行尝试这些请求，必须首先执行这三个步骤，然后用自己的值替换前面所述的示例值：

1. [创建一个 B2C 租户](active-directory-b2c-get-started.md)，并在请求中使用该租户的名称。
2. [创建应用程序](active-directory-b2c-app-registration.md)以获取应用程序 ID。 在应用中加入 Web 应用/Web API。 或者，可以选择创建应用程序密码。
3. [创建用户流](active-directory-b2c-reference-policies.md)以获取用户流名称。

