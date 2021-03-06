---
title: 使用自定义策略设置使用 Microsoft 帐户的登录
titleSuffix: Azure AD B2C
description: 如何使用自定义策略以通过 OpenID Connect (OIDC) 协议将 Microsoft 帐户 (MSA) 作为标识提供者。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/12/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 282d60b1894ffa186a6b5b6b5630aefa9e961572
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2020
ms.locfileid: "96345127"
---
# <a name="set-up-sign-in-with-a-microsoft-account-using-custom-policies-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中使用自定义策略设置使用 Microsoft 帐户的登录

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

本文展示了如何在 Azure Active Directory B2C (Azure AD B2C) 中使用[自定义策略](custom-policy-overview.md)让来自 Microsoft 帐户的用户实现登录。

## <a name="prerequisites"></a>先决条件

- 完成 [Azure Active Directory B2C 中的自定义策略入门](custom-policy-get-started.md)中的步骤。
- 如果还没有 Microsoft 帐户，请在 [https://www.live.com/](https://www.live.com/) 上创建一个。

## <a name="register-an-application"></a>注册应用程序

若要让使用 Microsoft 帐户的用户登录，需要在 Azure AD 租户中注册应用程序。 Azure AD 租户与 Azure AD B2C 租户不同。

1. 登录 [Azure 门户](https://portal.azure.com)。
1. 请确保使用包含 Azure AD 租户的目录，方法是选择顶部菜单中的“目录 + 订阅”筛选器，然后选择包含 Azure AD 租户的目录。
1. 选择 Azure 门户左上角的“所有服务”，然后搜索并选择“应用注册” 。
1. 选择“新注册”。
1. 输入应用程序的 **名称**。 例如，MSAapp1。
1. 在“受支持的帐户类型”下，选择“任何组织目录（任何 Azure AD 目录 - 多租户）中的帐户和个人 Microsoft 帐户（例如，Skype、Xbox）”。
1. 在“重定向 URI（可选）”下，选择“Web”，然后在文本框中输入 `https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/oauth2/authresp`。 将 `<tenant-name>` 替换为 Azure AD B2C 租户的名称。
1. 选择“注册”
1. 记录应用程序概述页上显示的“应用程序（客户端）ID”。 在后面的部分中配置声明提供程序时，需要用到此项。
1. 选择“证书和密码”
1. 单击“新客户端密码”
1. 为密码输入“说明”，例如“MSA 应用程序客户端密码”，然后单击“添加”。
1. 记录“值”列中显示的应用程序密码。 在下一部分会使用此值。

## <a name="configuring-optional-claims"></a>配置可选声明

如果要从 Azure AD 获取 `family_name` 和 `given_name` 声明，可以在 Azure 门户 UI 或应用程序清单中为应用程序配置可选声明。 有关详细信息，请参阅[如何向 Azure AD 应用提供可选声明](../active-directory/develop/active-directory-optional-claims.md)。

1. 登录 [Azure 门户](https://portal.azure.com)。 搜索并选择“Azure Active Directory”。
1. 从“管理”部分中选择“应用注册”。 
1. 在列表中选择要为其配置可选声明的应用程序。
1. 从“管理”部分中选择“令牌配置（预览）”。 
1. 选择“添加可选声明”。
1. 选择要配置的令牌类型。
1. 选择要添加的可选声明。
1. 单击“添加”。

## <a name="create-a-policy-key"></a>创建策略密钥

现在，已在 Azure AD 租户中创建了应用程序，需要将该应用程序的客户端密码存储在 Azure AD B2C 租户中。

1. 登录 [Azure 门户](https://portal.azure.com/)。
1. 请确保使用的是包含 Azure AD B2C 租户的目录。 选择顶部菜单中的“目录 + 订阅”筛选器，然后选择包含租户的目录。
1. 选择 Azure 门户左上角的“所有服务”，然后搜索并选择“Azure AD B2C” 。
1. 在“概述”页上选择“标识体验框架”。
1. 选择“策略密钥”，然后选择“添加”。
1. 对于“选项”，请选择 `Manual`。
1. 输入策略密钥的 **名称**。 例如，`MSASecret`。 前缀 `B2C_1A_` 会自动添加到密钥名称。
1. 在“密码”中，输入在上一部分中记录的客户端密码。
1. 在“密钥用法”处选择 `Signature`。
1. 单击“创建”。

## <a name="add-a-claims-provider"></a>添加声明提供程序

要让用户使用 Microsoft 帐户登录，需将该帐户定义为 Azure AD B2C 可通过终结点与其进行通信的声明提供程序。 该终结点将提供一组声明，Azure AD B2C 使用这些声明来验证特定的用户是否已完成身份验证。

要将 Azure AD 定义为声明提供程序，可在策略的扩展文件中添加 **ClaimsProvider** 元素。

1. 保存 TrustFrameworkExtensions.xml 策略文件。
1. 找到 **ClaimsProviders** 元素。 如果该元素不存在，请在根元素下添加它。
1. 如下所示添加新的 **ClaimsProvider**：

    ```xml
    <ClaimsProvider>
      <Domain>live.com</Domain>
      <DisplayName>Microsoft Account</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="MSA-OIDC">
          <DisplayName>Microsoft Account</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <Item Key="ProviderName">https://login.live.com</Item>
            <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="scope">openid profile email</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
            <Item Key="client_id">Your Microsoft application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="oid" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
            <OutputClaim ClaimTypeReferenceId="email" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. 将 client_id 的值替换为前面记录的 Azure AD 应用程序的“应用程序（客户端）ID”。
1. 保存文件。

现在，已配置了策略，因此 Azure AD B2C 知道如何与 Azure AD 中的 Microsoft 帐户应用程序进行通信。

### <a name="upload-the-extension-file-for-verification"></a>上传扩展文件以进行验证

继续操作之前，请上传修改后的策略，以确认它目前不会有任何问题。

1. 在 Azure 门户中导航到 Azure AD B2C 租户，然后选择“Identity Experience Framework”。
1. 在“自定义策略”页上，选择“上传自定义策略” 。
1. 启用“覆盖策略(若存在)”，然后浏览到 *TrustFrameworkExtensions.xml* 文件并选中该文件。
1. 单击“上载” 。

如果门户中没有显示任何错误，请继续转到下一部分。

## <a name="register-the-claims-provider"></a>注册声明提供程序

此时，标识提供者已设置，但尚未出现在任何注册或登录屏幕中。 若要使其可用，需要创建现有模板用户旅程的副本，并对其进行修改，使其具有 Microsoft 帐户标识提供者。

1. 打开初学者包中的 *TrustFrameworkBase.xml* 文件。
1. 找到并复制包含 `Id="SignUpOrSignIn"` 的 **UserJourney** 元素的完整内容。
1. 打开 *TrustFrameworkExtensions.xml* 并找到 **UserJourneys** 元素。 如果该元素不存在，请添加一个。
1. 将复制的 **UserJourney** 元素的完整内容粘贴为 **UserJourneys** 元素的子级。
1. 重命名用户旅程的 ID。 例如，`SignUpSignInMSA`。

### <a name="display-the-button"></a>显示按钮

**ClaimsProviderSelection** 元素类似于注册或登录屏幕上的标识提供者按钮。 如果为 Microsoft 帐户添加 ClaimsProviderSelection 元素，则当用户进入页面时，会显示一个新按钮。

1. 在 *TrustFrameworkExtensions.xml* 文件中，在创建的用户旅程中找到包含 `Order="1"` 的 **OrchestrationStep** 元素。
1. 在 **ClaimsProviderSelects** 下，添加以下元素。 将 **TargetClaimsExchangeId** 设置为适当的值，例如 `MicrosoftAccountExchange`：

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="MicrosoftAccountExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>将按钮链接到操作

准备好按钮后，需将它链接到某个操作。 在本例中，Azure AD B2C 使用该操作来与 Microsoft 帐户通信以接收令牌。

1. 在用户旅程中找到包含 `Order="2"` 的 **OrchestrationStep**。
1. 添加以下 **ClaimsExchange** 元素，确保在 ID 和 **TargetClaimsExchangeId** 处使用相同的值：

    ```xml
    <ClaimsExchange Id="MicrosoftAccountExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    ```

    更新 TechnicalProfileReferenceId 的值，以与前面添加的声明提供程序的 TechnicalProfile 元素中的 `Id` 值相匹配。 例如，`MSA-OIDC`。

1. 保存 *TrustFrameworkExtensions.xml* 文件，并再次上传以进行验证。

## <a name="create-an-azure-ad-b2c-application"></a>创建 Azure AD B2C 应用程序

通过在 B2C 租户中注册的应用程序与 Azure AD B2C 进行通信。 本部分列出了可用于创建测试应用程序的可选步骤（如果尚未创建）。

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>更新和测试信赖方文件

更新用于启动创建的用户旅程的信赖方 (RP) 文件。

1. 在工作目录中创建 *SignUpOrSignIn.xml* 的副本并将其重命名。 例如，将其重命名为 *SignUpSignInMSA.xml*。
1. 打开新文件，并将 **TrustFrameworkPolicy** 的 **PolicyId** 属性的值更新为唯一的值。 例如，`SignUpSignInMSA`。
1. 将 **PublicPolicyUri** 的值更新为策略的 URI。 例如 `http://contoso.com/B2C_1A_signup_signin_msa`
1. 更新 DefaultUserJourney 中 ReferenceId 属性的值，以匹配之前创建的用户旅程的 ID (SignUpSignInMSA)。
1. 保存更改并上传文件，然后选择列表中的新策略。
1. 请确保在“选择应用程序”字段中选择了在上一部分中创建的 Azure AD B2C 应用程序（或通过完成先决条件，例如 webapp1 或 testapp1-template.json），然后单击“立即运行”进行测试。 
1. 选择“Microsoft 帐户”按钮并登录。

    如果登录操作成功，则会重定向到显示已解码令牌的 `jwt.ms`，类似于：

    ```json
    {
      "typ": "JWT",
      "alg": "RS256",
      "kid": "<key-ID>"
    }.{
      "exp": 1562365200,
      "nbf": 1562361600,
      "ver": "1.0",
      "iss": "https://<tenant-name>.b2clogin.com/10000000-0000-0000-0000-000000000000/v2.0/",
      "sub": "20000000-0000-0000-0000-000000000000",
      "aud": "30000000-0000-0000-0000-000000000000",
      "acr": "b2c_1a_signupsigninmsa",
      "nonce": "defaultNonce",
      "iat": 1562361600,
      "auth_time": 1562361600,
      "idp": "live.com",
      "name": "Azure User",
      "email": "azureuser@contoso.com",
      "tid": "6fc3b573-7b38-4c0c-b627-2e8684f6c575"
    }.[Signature]
    ```
