---
title: 了解如何向 Azure AD 应用程序提供可选声明 | Microsoft Docs
description: 有关如何将自定义或附加的声明添加到 Azure Active Directory 颁发的 SAML 2.0 令牌和 JSON Web 令牌 (JWT) 的指南。
documentationcenter: na
author: CelesteDG
services: active-directory
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/08/2018
ms.author: celested
ms.reviewer: paulgarn, hirsin
ms.custom: aaddev
ms.openlocfilehash: 0983c2235fba0cacbda53208e5dcad5b2878619c
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "51345481"
---
# <a name="how-to-provide-optional-claims-to-your-azure-ad-app-public-preview"></a>如何：向 Azure AD 应用（公共预览版）提供可选声明

应用程序开发人员可以使用此功能来指定要在发送到其应用程序的令牌中添加哪些声明。 使用可选声明可以：
- 选择要包含在应用程序令牌中的附加声明。
- 更改 Azure AD 在令牌中返回的某些声明的行为。
- 添加和访问应用程序的自定义声明。 

> [!NOTE]
> 此功能目前以公共预览版提供。 应准备好还原或删除所做的任何更改。 在公共预览版推出期间，可在任何 Azure AD 订阅中使用此功能。 但是，在正式版推出后，某些功能可能需要使用 Azure AD Premium 订阅。

有关标准声明的列表及其在令牌中的使用方式，请参阅 [Azure AD 颁发的令牌基础知识](v1-id-and-access-tokens.md)。 

[v2.0 Azure AD 终结点](active-directory-appmodel-v2-overview.md)的目标之一是缩小令牌大小，以确保客户端获得最佳性能。 因此，以前包含在访问令牌和 ID 令牌中的多个声明不再在 v2.0 令牌中提供，必须根据应用程序专门请求这些声明。

**表 1：适用性**

| 帐户类型 | V1.0 终结点 | V2.0 终结点  |
|--------------|---------------|----------------|
| Microsoft 个人帐户  | 不适用 - 改用 RPS 票证 | 即将提供支持 |
| Azure AD 帐户          | 支持                          | 带注意事项的支持 |

> [!IMPORTANT]
> 同时支持个人帐户和 Azure AD（已通过[应用注册门户](https://apps.dev.microsoft.com)注册）的应用无法使用可选声明。 但是，使用 v2.0 终结点仅为 Azure AD 注册的应用可以获取它们在清单中请求的可选声明。 在 Azure 门户中，可以使用现有**应用注册**体验中的应用程序清单编辑器来编辑可选声明。 但是，在新的**应用注册(预览版)** 体验中，使用应用程序清单编辑器时还不能使用此功能。

## <a name="standard-optional-claims-set"></a>标准的可选声明集

下面列出了默认可对应用程序使用的可选声明集。 若要为应用程序添加自定义可选声明，请参阅下面的[目录扩展](active-directory-optional-claims.md#Configuring-custom-claims-via-directory-extensions)。 请注意，在向**访问令牌**添加声明时，这将适用于应用程序 (Web API) 请求的访问令牌，而不是应用程序发出的访问令牌。 这可确保无论哪个客户端访问你的 API，正确的数据都存在于用于对你的 API 进行身份验证的访问令牌中。

> [!NOTE]
> 其中的大多数声明可包含在 v1.0 和 v2.0 令牌的 JWT 中，但不可包含在 SAML 令牌中，“令牌类型”列中指明的声明除外。 此外，尽管可选声明目前仅支持 AAD 用户，但 MSA 支持即将推出。 当 MSA 在 v2.0 终结点上提供可选声明支持时，如果某个声明适用于 AAD 或 MSA 用户，“用户类型”列中会予以注明。 

**表 2：标准的可选声明集**

| 名称                        | Description   | 令牌类型 | 用户类型 | 说明  |
|-----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | 用户上次进行身份验证的时间。 请参阅 OpenID Connect 规范。| JWT        |           |  |
| `tenant_region_scope`      | 资源租户的区域 | JWT        |           | |
| `home_oid`                 | 对于来宾用户，表示该用户在用户主租户中的对象 ID。| JWT        |           | |
| `sid`                      | 会话 ID，用于基于会话的用户注销。 | JWT        |           |         |
| `platf`                    | 设备平台    | JWT        |           | 限制为可以验证设备类型的托管设备。|
| `verified_primary_email`   | 源自用户的 PrimaryAuthoritativeEmail      | JWT        |           |         |
| `verified_secondary_email` | 源自用户的 SecondaryAuthoritativeEmail   | JWT        |           |        |
| `enfpolids`                | 强制实施的策略 ID。 针对当前用户评估的策略 ID 列表。 | JWT |  |  |
| `vnet`                     | VNET 说明符信息。 | JWT        |           |      |
| `fwd`                      | IP 地址。| JWT    |   | 添加请求方客户端（如果位于 VNET 中）的原始 IPv4 地址 |
| `ctry`                     | 用户所在的国家/地区 | JWT |           | Azure AD 返回 `ctry` 可选声明（如果存在）且声明的值是标准的双字母国家/地区代码，例如 FR、JP、SZ 等。 |
| `tenant_ctry`              | 资源租户所在的国家/地区 | JWT | | |
| `xms_pdl`          | 首选数据位置   | JWT | | 对于多地区租户，这是显示用户所在地理区域的 3 字母代码。 有关更多详细信息，请参阅[有关首选数据位置的 Azure AD Connect 文档](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation)。 <br> 例如：`APC` 表示“亚太”。 |
| `xms_pl`                   | 用户首选语言  | JWT ||用户的首选语言（如果已设置）。 在来宾访问方案中，源自其主租户。 已格式化 LL-CC（“en-us”）。 |
| `xms_tpl`                  | 租户首选语言| JWT | | 资源租户的首选语言（如果已设置）。 已格式化 LL（“en”）。 |
| `ztdid`                    | 零接触部署 ID | JWT | | 用于 [Windows AutoPilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) 的设备标识 |
|`email`                     | 此用户的可寻址电子邮件（如果此用户有）。  | JWT、SAML | | 如果用户是租户中的来宾，则默认包含此值。  对于托管用户（租户内部的用户），必须通过此可选声明进行请求，或者仅在 v2.0 上使用 OpenID 范围进行请求。  对于托管用户，必须在 [Office 管理门户](https://portal.office.com/adminportal/home#/users)中设置电子邮件地址。|  
| `acct`             | 租户中的用户帐户状态。 | JWT、SAML | | 如果用户是租户的成员，则该值为 `0`。 如果他们是来宾，则该值为 `1`。 |
| `upn`                      | UserPrincipalName 声明。 | JWT、SAML  |           | 尽管会自动包含此声明，但可以将它指定为可选声明，以附加额外的属性，在来宾用例中修改此声明的行为。 <br> 附加属性： <br> `include_externally_authenticated_upn` <br> `include_externally_authenticated_upn_without_hash` |

### <a name="v20-optional-claims"></a>v2.0 可选声明

这些声明始终包含在 v1.0 令牌中，但除非提出请求，否则不会包含在 v2.0 令牌中。 这些声明仅适用于 JWT（ID 令牌和访问令牌）。 

**表 3：仅限 V2.0 的可选声明**

| JWT 声明     | 名称                            | Description                                | 说明 |
|---------------|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|
| `ipaddr`      | IP 地址                      | 客户端从中登录的 IP 地址。   |       |
| `onprem_sid`  | 本地安全标识符 |                                             |       |
| `pwd_exp`     | 密码过期时间        | 密码过期的日期时间。 |       |
| `pwd_url`     | 更改密码 URL             | 用户更改密码时可以访问的 URL。   |       |
| `in_corp`     | 企业网络内部        | 表示客户端是否从企业网络登录。 如果不是，则不包含此声明。   |       |
| `nickname`    | 别名                        | 用户的附加名称，不同于名字或姓氏。 |       |                                                                                                                |       |
| `family_name` | 姓氏                       | 按照 Azure AD 用户对象中的定义，指定用户的姓。 <br>"family_name":"Miller" |       |
| `given_name`  | 名字                      | 和对 Azure AD 用户对象的设置一样，指定用户的名。<br>"given_name": "Frank"                   |       |

### <a name="additional-properties-of-optional-claims"></a>可选声明的附加属性

可以配置某些可选声明来更改声明的返回方式。 这些附加属性主要用于帮助迁移具有不同数据预期的本地应用程序（例如，`include_externally_authenticated_upn_without_hash` 可帮助迁移无法处理 UPN 中的井号标记 (`#`) 的客户端）

**表 4：用于配置标准可选声明的值**

| 属性名称                                     | 附加属性名称                                                                                                             | Description |
|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-------------|
| `upn`                                                 |                                                                                                                                      |  可用于 SAML 和 JWT 响应。        |
| | `include_externally_authenticated_upn`              | 包含资源租户中存储的来宾 UPN。 例如： `foo_hometenant.com#EXT#@resourcetenant.com`                            |             
| | `include_externally_authenticated_upn_without_hash` | 同上，不过，井号标记 (`#`) 已替换为下划线 (`_`)，例如 `foo_hometenant.com_EXT_@resourcetenant.com` |             

> [!Note]
>如果指定 upn 可选声明但不使用附加属性，则不会更改任何行为 – 若要查看令牌中颁发的新声明，必须至少添加一个附加属性。 

#### <a name="additional-properties-example"></a>附加属性示例

```json
 "optionalClaims": 
   {
       "idToken": [ 
             { 
                "name": "upn", 
            "essential": false,
                "additionalProperties": [ "include_externally_authenticated_upn"]  
              }
        ]
}
```

此 OptionalClaims 对象会导致返回到客户端的 ID 令牌包含另一个 UPN 及其他主租户和资源租户信息。 仅当用户是租户中的来宾（使用不同的 IDP 进行身份验证）时，这才会更改令牌中的 `upn` 声明。 

## <a name="configuring-optional-claims"></a>配置可选声明

可以通过修改应用程序清单来配置应用程序的可选声明 （请参阅下面的示例）。 有关详细信息，请参阅[了解 Azure AD 应用程序清单](reference-app-manifest.md)一文。

**示例架构：**

```json
"optionalClaims":  
   {
       "idToken": [
             { 
                   "name": "auth_time", 
                   "essential": false
              }
        ],
 "accessToken": [ 
             {
                    "name": "ipaddr", 
                    "essential": false
              }
        ],
"saml2Token": [ 
              { 
                    "name": "upn", 
                    "essential": true
               },
               { 
                    "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                    "source": "user", 
                    "essential": true
               }
       ]
   }
```

### <a name="optionalclaims-type"></a>OptionalClaims 类型

声明应用程序请求的可选声明。 应用程序可以配置为要在它能够从安全令牌服务收到的每种令牌（共三种，即 ID 令牌、访问令牌和 SAML 2 令牌）中返回可选声明。 应用程序可以配置为要在每种令牌类型中返回一组不同的可选声明。 Application 实体的 OptionalClaims 属性是一个 OptionalClaims 对象。

**表 5：OptionalClaims 类型属性**

| 名称        | 类型                       | Description                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | 集合 (OptionalClaim) | 在 JWT ID 令牌中返回的可选声明。 |
| `accessToken` | 集合 (OptionalClaim) | 在 JWT 访问令牌中返回的可选声明。 |
| `saml2Token`  | 集合 (OptionalClaim) | 在 SAML 令牌中返回的可选声明。   |

### <a name="optionalclaim-type"></a>OptionalClaim 类型

包含与应用程序或服务主体关联的可选声明。 [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) 类型的 IdToken、accessToken 和 saml2Token 属性是一个 OptionalClaim 集合。
如果特定的声明支持这样做，则还可以使用 AdditionalProperties 字段修改 OptionalClaim 的行为。

**表 6：OptionalClaim 类型属性**

| 名称                 | 类型                    | Description                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | 可选声明的名称。                                                                                                                                                                                                                                                                           |
| `source`               | Edm.String              | 声明的源（目录对象）。 扩展属性提供预定义声明和用户定义的声明。 如果源值为 null，则声明是预定义的可选声明。 如果源值为 user，则 name 属性中的值是来自用户对象的扩展属性。 |
| `essential`            | Edm.Boolean             | 如果值为 true，则必须使用客户端指定的声明，以确保为最终用户请求的特定任务提供顺利的授权体验。 默认值为 false。                                                                                                             |
| `additionalProperties` | 集合 (Edm.String) | 声明的附加属性。 如果此集合中存在某个属性，该属性将修改 name 属性中指定的可选声明的行为。                                                                                                                                               |
## <a name="configuring-custom-claims-via-directory-extensions"></a>通过目录扩展配置自定义声明

除了标准的可选声明集以外，还可将令牌配置为包含目录架构扩展（有关详细信息，请参阅[目录架构扩展](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)一文）。 使用此功能可以附加应用可以使用的附加用户信息 – 例如，用户设置的附加标识符或重要配置选项。 

> [!Note]
> 目录架构扩展功能只能在 AAD 中使用，因此，如果应用程序清单请求自定义扩展，而 MSA 用户登录到你的应用，则不会返回这些扩展。 

### <a name="values-for-configuring-additional-optional-claims"></a>用于配置附加可选声明的值

对于扩展属性，请在应用程序清单中使用扩展的完整名称（格式：`extension_<appid>_<attributename>`）。 `<appid>` 必须与请求该声明的应用程序的 ID 相匹配。 

在 JWT 中，将使用以下命名格式发出这些声明：`extn.<attributename>`。

在 SAML 令牌中，将使用以下 URI 格式发出这些声明：`http://schemas.microsoft.com/identity/claims/extn.<attributename>`。

## <a name="optional-claims-example"></a>可选声明示例

本部分通过一个场景来演练如何对应用程序使用可选声明功能。
可以使用多个选项来更新应用程序标识配置中的属性，以启用和配置可选声明：
-   可以修改应用程序清单。 以下示例将使用此方法来执行配置。 请先阅读[了解 Azure AD 应用程序清单文档](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)中的清单简介。
-   也可以编写使用[图形 API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) 的应用程序来更新应用程序。 图形 API 参考指南中的[实体和复杂类型参考](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type)可帮助你配置可选声明。

**示例：** 在以下示例中，我们将修改某个应用程序的清单，以便将声明添加到用于该应用程序的访问、ID 和 SAML 令牌。

1. 登录到 [Azure 门户](https://portal.azure.com)。
1. 通过身份验证后，在页面右上角选择 Azure AD 租户。
1. 选择左侧的“应用注册”。
1. 在列表中找到要为其配置可选声明的应用程序并单击它。
1. 在应用程序页面中，单击“清单”打开内联清单编辑器。 
1. 可使用此编辑器直接编辑清单。 该清单遵循 [Application 实体](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity)的架构，保存后会自动设置格式。 新元素将添加到 `OptionalClaims` 属性。

      ```json
      "optionalClaims": 
      {
            "idToken": [ 
                  { 
                        "name": "upn", 
                        "essential": false, 
                        "additionalProperties": [ "include_externally_authenticated_upn"]  
                  }
            ],
      "accessToken": [ 
                  {
                        "name": "auth_time", 
                        "essential": false
                  }
            ],
      "saml2Token": [ 
                  { 
                        "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                        "source": "user", 
                        "essential": true
                  }
            ]
      }
      ```
      在本例中，已将不同的可选声明添加到应用程序可以接收的每种令牌。 ID 令牌现在会包含联合用户的完整格式 UPN (`<upn>_<homedomain>#EXT#@<resourcedomain>`)。 其他客户端请求此应用程序的访问令牌现在将包含 auth_time 声明。 SAML 令牌现在会包含 skypeId 目录架构扩展（在本例中，此应用的应用 ID 为 ab603c56068041afb2f6832e2a17e237）。 SAML 令牌会将 Skype ID 公开为 `extension_skypeId`。

1. 更新完清单后，请单击“保存”以保存清单

## <a name="next-steps"></a>后续步骤

详细了解 Azure AD 提供的标准声明。

- [ID 令牌](id-tokens.md)
- [访问令牌](access-tokens.md)
