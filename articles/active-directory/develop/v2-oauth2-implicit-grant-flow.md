---
title: 使用 Azure AD v2.0 隐式流保护单页应用程序 | Microsoft Docs
description: 使用单页面应用隐式流的 Azure AD v2.0 实现构建 Web 应用程序。
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 9e8d35ee606d66f5c4f6e11512247fc909ecfb9f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "46999845"
---
# <a name="v20-protocols---spas-using-the-implicit-flow"></a>v2.0 协议 - 使用隐式流的 SPA

使用 v2.0 终结点，可以让拥有 Microsoft 个人和工作/学校帐户的用户登录单一页面应用。 主要在浏览器上运行的单一页面和其他 JavaScript 应用程序在身份验证时面临一些有趣的挑战：

* 这些应用的安全特征与传统的基于服务器的 Web 应用程序大不相同。
* 许多授权服务器与标识提供者不支持 CORS 请求。
* 重定向离开应用程序的完整网页浏览器变得对用户经验特别有侵略性。

对于这些应用程序（AngularJS、Ember.js、React.js 等），Azure AD 支持 OAuth 2.0 隐式授权流。 有关隐式流的说明，请参阅 [OAuth 2.0 规范](http://tools.ietf.org/html/rfc6749#section-4.2)。 其主要优点是它可让应用程序从 Azure AD 获取令牌，无需要执行后端服务器凭据交换。 这可让应用登录用户、维护会话，并获取客户端 JavaScript 代码中所有其他 Web API 的令牌。 使用隐式流时有几个重要的安全注意事项 - 特别是关于[客户端](http://tools.ietf.org/html/rfc6749#section-10.3)和[用户模拟](http://tools.ietf.org/html/rfc6749#section-10.3)的注意事项。

如果要使用隐式流和 Azure AD 将身份验证添加到 JavaScript 应用，建议使用开放源代码 JavaScript 库 [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js)。 [此处](v2-overview.md#getting-started)有几篇 AngularJS 教程可以帮助入门。

但是，如果不想要使用单一页面应用中的库，而是自行发送协议消息，请遵循下面的常规步骤。

> [!NOTE]
> v2.0 终结点并不支持所有 Azure Active Directory 方案和功能。 若要确定是否应使用 v2.0 终结点，请阅读 [v2.0 限制](active-directory-v2-limitations.md)。

## <a name="protocol-diagram"></a>协议图

整个隐式登录流如下所示 - 下面将详细描述每个步骤。

![OpenId Connect Swimlanes](./media/v2-oauth2-implicit-grant-flow/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>发送登录请求

要一开始将用户登录到应用，可以发送 [OpenID Connect](v2-protocols-oidc.md) 授权请求，以及从 v2.0 终结点获取 `id_token`：

> [!IMPORTANT]
> 为了成功请求 ID 令牌，[注册门户](https://apps.dev.microsoft.com)中的应用注册必须已为 Web 客户端启用[隐式授权](v2-oauth2-implicit-grant-flow.md)。 如果未启用，将返回 `unsupported_response` 错误：“输入参数“response_type”提供的值不允许用于此客户端。 预期值为“code””

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> 单击下面的链接以执行此请求！ 登录之后，浏览器应重定向至地址栏中具有 `id_token` 的 `https://localhost/myapp/`。
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| 参数 |  | Description |
| --- | --- | --- |
| tenant |必填 |请求路径中的 `{tenant}` 值可用于控制哪些用户可以登录应用程序。 允许的值为 `common`、`organizations`、`consumers` 和租户标识符。 有关更多详细信息，请参阅[协议基础知识](active-directory-v2-protocols.md#endpoints)。 |
| client_id |必填 |注册门户 ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) 分配给应用的应用程序 ID。 |
| response_type |必填 |必须包含 OpenID Connect 登录的 `id_token`。 也可以包含 response_type `token`。 此处使用 `token`，让应用能够立即从授权终结点接收访问令牌，而无需向授权终结点发出第二次请求。 如果使用 `token` response_type，`scope` 参数必须包含范围，以指出要对哪个资源发出令牌。 |
| redirect_uri |建议 |应用程序的 redirect_uri，应用程序可在此发送及接收身份验证响应。 其必须完全符合在门户中注册的其中一个 redirect_uris，否则必须是编码的 url。 |
| 作用域 |必填 |范围的空格分隔列表。 针对 OpenID Connect，即必须包含范围 `openid`，其在同意 UI 中转换为“你将登录”权限。 （可选）可能需要包含 `email` 或 `profile` [范围](v2-permissions-and-consent.md)，以获取对其他用户数据的访问权限。 也可以在此请求中包含其他范围，以请求同意各种资源。 |
| response_mode |建议 |指定将生成的令牌送回到应用程序所应该使用的方法。 对于隐式流应该是 `fragment`。 |
| state |建议 |同样随令牌响应返回的请求中所包含的值。 它可以是你想要的任何内容的字符串。 随机生成的唯一值通常用于[防止跨站点请求伪造攻击](http://tools.ietf.org/html/rfc6749#section-10.12)。 该状态也用于在身份验证请求出现之前，于应用程序中编码用户的状态信息，例如之前所在的网页或视图。 |
| nonce |必填 |由应用程序生成且包含在请求中的值，以声明方式包含在生成的 id_token 中。 应用程序接着便可确认此值，以减少令牌重新执行攻击。 此值通常是随机的唯一字符串，可用以识别请求的来源。 |
| prompt |可选 |表示需要的用户交互类型。 此时唯一有效的值为“login”、“none”和“consent”。 `prompt=login` 将强制用户在该请求上输入其凭据，从而使单一登录无效。 `prompt=none` 完全相反，它将确保无论如何都不会向用户显示任何交互提示。 如果请求无法通过单一登录静默完成，该 v2.0 终结点将返回一个错误。 `prompt=consent` 在用户登录后将触发 OAuth 同意对话框，要求用户向应用授予权限。 |
| login_hint |可选 |如果事先知道其用户名称，可用于预先填充用户登录页面的用户名称/电子邮件地址字段。 通常，应用会在重新身份验证期间使用此参数，并且已经使用 `preferred_username` 声明从前次登录提取用户名。 |
| domain_hint |可选 |可以是 `consumers` 或 `organizations` 之一。 如果包含，它跳过用户在 v2.0 登录页面上经历的基于电子邮件的发现过程，导致稍微更加流畅的用户体验。 通常，应用会在重新身份验证期间使用此参数，方法是从 id_token 提取 `tid` 声明。 如果 `tid` 声明值为 `9188040d-6c67-4c5b-b112-36a304b66dad`（Microsoft 帐户使用者租户），则应使用 `domain_hint=consumers`。 否则使用 `domain_hint=organizations`。 |

此时，请求用户输入其凭据并完成身份验证。 v2.0 终结点也将确保用户已经同意 `scope` 查询参数中指示的权限。 如果用户未曾同意这些权限的任何一项，就请求用户同意请求的权限。 [此处提供了权限、许可与多租户应用](v2-permissions-and-consent.md)的详细信息。

用户经过身份验证并同意后，v2.0 终结点将使用 `response_mode` 参数中指定的方法，将响应返回到位于所指示的 `redirect_uri` 的应用。

#### <a name="successful-response"></a>成功的响应

使用 `response_mode=fragment` 和 `response_type=id_token+token` 的成功响应如下所示（包含换行符以方便阅读）：

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| 参数 | Description |
| --- | --- |
| access_token |如果 `response_type` 包含 `token`，则包含该参数。 应用请求的访问令牌，在本例中为 Microsoft Graph 的访问令牌。 访问令牌不得进行解码或检查，可被视为不透明的字符串。 |
| token_type |如果 `response_type` 包含 `token`，则包含该参数。 始终为 `Bearer`。 |
| expires_in |如果 `response_type` 包含 `token`，则包含该参数。 表示令牌有效的秒数（针对缓存目的）。 |
| 作用域 |如果 `response_type` 包含 `token`，则包含该参数。 表示 access_token 的有效范围。 |
| id_token      | 无符号 JSON Web 令牌 (JWT)。 应用可以解码此令牌的段，以请求已登录用户的相关信息。 应用可以缓存并显示值，但不应依赖于这些值获取任何授权或安全边界。 有关 id_tokens 的详细信息，请参阅 [`id_token reference`](id-tokens.md). <br> **注意：** 仅当已请求 `openid` 作用域时提供。 |
| state |如果请求中包含状态参数，响应中就应该出现相同的值。 应用程序应该验证请求和响应中的状态值是否完全相同。 |

#### <a name="error-response"></a>错误响应

错误响应可能也发送到 `redirect_uri`，让应用可以适当地处理：

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| 参数 | Description |
| --- | --- |
| error |用于分类发生的错误类型与响应错误的错误码字符串。 |
| error_description |帮助开发人员识别身份验证错误根本原因的特定错误消息。 |

## <a name="validate-the-idtoken"></a>验证 id_token

仅接收 id_token 不足以验证用户，必须身份验证 id_token 签名，并按照应用的要求验证令牌中的声明。 v2.0 终结点使用 [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)（JSON Web 令牌 (JWT)）和公钥加密对令牌进行签名并验证其是否有效。

可以选择验证客户端代码中的 `id_token`，但是常见的做法是将 `id_token` 发送到后端服务器，并在那里执行验证。 验证 id_token 的签名后，就有几项声明需要验证。 有关详细信息，请参阅 [`id_token` 参考](id-tokens.md)，其中包括[验证令牌](id-tokens.md#validating-idtokens)和[有关签名密钥滚动更新的重要信息](active-directory-signing-key-rollover.md)。 我们建议利用库来分析和验证令牌 - 对于大多数语言和平台至少有一个可用。
<!--TODO: Improve the information on this-->

可能还希望根据自己的方案验证其他声明。 一些常见的验证包括：

* 确保用户/组织已注册应用。
* 确保用户拥有正确的授权/权限
* 确保身份验证具有一定的强度，例如多重身份验证。

完全验证 id_token 后，即可开始与用户的会话，并使用 id_token 中的声明来获取应用中的用户相关信息。 此信息可用于显示、记录和个性化等。

## <a name="get-access-tokens"></a>获取访问令牌

将用户登录到单页应用后，便可获取访问令牌以调用受 Azure AD 保护的 Web API，例如 [Microsoft Graph](https://graph.microsoft.io)。 即使你已使用 `token` response_type 收到令牌，也仍可以使用此方法获取其他资源的令牌，而无需再次将用户重定向到登录页。

在正常的 OpenID Connect/OAuth 流中，可以通过对 v2.0 `/token` 终结点进行请求来实现此目的。 但是，v2.0 终结点不支持 CORS 请求，因此进行 AJAX 调用以获取和刷新令牌是不可能的。 相反，可以在隐藏的 iframe 中使用隐式流，以获取其他 Web API 的新令牌： 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [!TIP]
> 请尝试将以下请求复制并粘贴到浏览器选项卡中！ （不要忘记使用适用于用户的正确值替换 `domain_hint` 和 `login_hint`）

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| 参数 |  | Description |
| --- | --- | --- |
| tenant |必填 |请求路径中的 `{tenant}` 值可用于控制哪些用户可以登录应用程序。 允许的值为 `common`、`organizations`、`consumers` 和租户标识符。 有关更多详细信息，请参阅[协议基础知识](active-directory-v2-protocols.md#endpoints)。 |
| client_id |必填 |注册门户 ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) 分配给应用的应用程序 ID。 |
| response_type |必填 |必须包含 OpenID Connect 登录的 `id_token`。 还可以包含其他 response_type，例如 `code`。 |
| redirect_uri |建议 |应用程序的 redirect_uri，应用程序可在此发送及接收身份验证响应。 其必须完全符合在门户中注册的其中一个 redirect_uris，否则必须是编码的 url。 |
| 作用域 |必填 |范围的空格分隔列表。 若要获取令牌，请包含相应资源请求的所有[范围](v2-permissions-and-consent.md)。 |
| response_mode |建议 |指定将生成的令牌送回到应用程序所应该使用的方法。 可以是 `query`、`form_post` 或 `fragment` 之一。 |
| state |建议 |同样随令牌响应返回的请求中所包含的值。 它可以是你想要的任何内容的字符串。 随机生成的唯一值通常用于防止跨站点请求伪造攻击。 该状态也用于在身份验证请求出现之前，于应用程序中编码用户的状态信息，例如之前所在的网页或视图。 |
| nonce |必填 |由应用程序生成且包含在请求中的值，以声明方式包含在生成的 id_token 中。 应用程序接着便可确认此值，以减少令牌重新执行攻击。 此值通常是随机的唯一字符串，可用以识别请求的来源。 |
| prompt |必填 |若要刷新并获取隐藏的 iframe 中的令牌，应该使用 `prompt=none` 以确保 iframe 会立即返回，而不会在 v2.0 登录页面上挂起。 |
| login_hint |必填 |若要刷新并获取隐藏 iframe 中的令牌，必须在此提示中包含用户的用户名，以便区分用户在特定时间点可能具有的多个会话。 可以使用 `preferred_username` 声明从前次登录提取用户名。 |
| domain_hint |必填 |可以是 `consumers` 或 `organizations` 之一。 若要刷新并获取隐藏 iframe 中的令牌，必须在请求中包含 domain_hint。 应该从前次登录的 id_token 提取 `tid` 声明，以确定要使用哪个值。 如果 `tid` 声明值是 `9188040d-6c67-4c5b-b112-36a304b66dad`，应该使用 `domain_hint=consumers`。 否则使用 `domain_hint=organizations`。 |

借助 `prompt=none` 参数，此请求将立即成功或立即失败，并返回应用程序。 成功的响应会通过 `response_mode` 参数中指定的方法，发送到位于所指示的 `redirect_uri` 的应用。

#### <a name="successful-response"></a>成功的响应

使用 `response_mode=fragment` 的成功响应如下所示：

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| 参数 | Description |
| --- | --- |
| access_token |应用请求的令牌。 |
| token_type |始终为 `Bearer`。 |
| state |如果请求中包含状态参数，响应中就应该出现相同的值。 应用需验证请求和响应中的状态值是否相同。 |
| expires_in |访问令牌的有效期（以秒为单位）。 |
| 作用域 |访问令牌有效的范围。 |

#### <a name="error-response"></a>错误响应

错误响应还可能发送到 `redirect_uri`，因此应用可以适当地对其进行处理。 如果是 `prompt=none`，预期的错误为：

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| 参数 | Description |
| --- | --- |
| error |用于分类发生的错误类型与响应错误的错误码字符串。 |
| error_description |帮助开发人员识别身份验证错误根本原因的特定错误消息。 |

如果在 iframe 请求中收到此错误，用户必须再次以交互方式登录以检索新令牌。 可以选择对应用程序合理的任何方式处理这种情况。

## <a name="validating-access-tokens"></a>验证访问令牌

在收到 access_token 后，请确保对令牌的签名及以下声明进行验证。 还可以选择根据自己的方案对其他声明进行验证。 

* **受众**声明，以确保打算将令牌提供给你的应用
* **颁发者**声明：验证令牌是否由 v2.0 终结点颁发给应用
* **不早于**和**过期时间**声明，以验证令牌是否未过期

有关访问令牌中提供的声明的详细信息，请参阅[访问令牌参考](access-tokens.md)

## <a name="refreshing-tokens"></a>刷新令牌

隐式授权不提供刷新令牌。 `id_token` 和 `access_token` 很快就会过期，因此应用必须准备好定期刷新这些令牌。 若要刷新任一类型的令牌，可以通过使用 `prompt=none` 参数控制 Azure AD 的行为，来执行上述同一隐藏的 iframe 请求。 如果想要接收新的 `id_token`，请务必使用 `response_type=id_token` 和 `scope=openid`，以及 `nonce` 参数。

## <a name="send-a-sign-out-request"></a>发送注销请求

OpenIdConnect `end_session_endpoint` 允许应用向 v2.0 终结点发送请求，以结束用户会话并清除 v2.0 终结点设置的 Cookie。 要完全将用户从 Web 应用程序注销，应用应结束自己与用户的会话（通常通过清除令牌缓存或删除 Cookie 实现），然后将浏览器重定向到：

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| 参数 |  | Description |
| --- | --- | --- |
| tenant |必填 |请求路径中的 `{tenant}` 值可用于控制哪些用户可以登录应用程序。 允许的值为 `common`、`organizations`、`consumers` 和租户标识符。 有关更多详细信息，请参阅[协议基础知识](active-directory-v2-protocols.md#endpoints)。 |
| post_logout_redirect_uri | 建议 | 注销完成后用户应返回到的 URL。 此值必须与为应用程序注册的重定向 URI 之一匹配。 如果未包含，v2.0 终结点会向用户显示一条常规消息。 |
