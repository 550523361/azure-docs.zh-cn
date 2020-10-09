---
title: 显示控件参考
titleSuffix: Azure AD B2C
description: Azure AD B2C 显示控件参考。 使用显示控件自定义你的自定义策略中定义的用户旅程。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/10/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 131ecd010cba55f08199f713654792c0844a47e1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "85202290"
---
# <a name="display-controls"></a>显示控件

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**显示控件**是一个具有特殊功能的用户界面元素，可以与 Azure Active Directory B2C (Azure AD B2C) 后端服务进行交互。 它允许用户在页面上执行某些操作，这些操作在后端调用[验证技术配置文件](validation-technical-profile.md)。 显示控件显示在页面上，由[自断言技术配置文件](self-asserted-technical-profile.md)引用。

下图展示了一个自断言注册页面，其中包含两个用于验证主要电子邮件地址和辅助电子邮件地址的显示控件。

![呈现了显示控件的示例](media/display-controls/display-control-email.png)

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="prerequisites"></a>必备条件

 在[自断言技术配置文件](self-asserted-technical-profile.md)的[元数据](self-asserted-technical-profile.md#metadata)部分中，引用的 [ContentDefinition](contentdefinitions.md) 需要将 `DataUri` 设置为页面协定版本2.0.0 或更高版本。 例如：

```xml
<ContentDefinition Id="api.selfasserted">
  <LoadUri>~/tenant/default/selfAsserted.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:selfasserted:2.0.0</DataUri>
  ...
```

## <a name="defining-display-controls"></a>定义显示控件

**DisplayControl** 元素包含以下属性：

| Attribute | 必选 | 说明 |
| --------- | -------- | ----------- |
| ID | 是 | 用于显示控件的一个标识符。 可以对它进行[引用](#referencing-display-controls)。 |
| UserInterfaceControlType | 是 | 显示控件的类型。 当前支持的是 [VerificationControl](display-control-verification.md) |

**DisplayControl** 元素包含以下元素：

| 元素 | 出现次数 | 说明 |
| ------- | ----------- | ----------- |
| InputClaims | 0:1 | **InputClaims** 用于预填充要从用户那里收集的声明的值。 |
| DisplayClaims | 0:1 | **DisplayClaims** 用于表示要从用户那里收集的声明。 |
| OutputClaims | 0:1 | **OutputClaims** 用于表示要暂时为此 **DisplayControl** 保存的声明。 |
| 操作 | 0:1 | **Actions** 用于列出要针对在前端发生的用户操作调用的验证技术配置文件。 |

### <a name="input-claims"></a>输入声明

在显示控件中，可以使用 **InputClaims** 元素预填充要在页面上从用户那里收集的声明的值。 可在引用此显示控件的自断言技术配置文件中定义任何 **InputClaimsTransformations**。

以下示例使用已存在的地址预填充要验证的电子邮件地址。

```xml
<DisplayControl Id="emailControl" UserInterfaceControlType="VerificationControl">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="emailAddress" />
  </InputClaims>
  ...
```

### <a name="display-claims"></a>显示声明

每种类型的显示控件都需要一组不同的显示声明、[输出声明](#output-claims)，以及要执行的[操作](#display-control-actions)。

与在[自断言技术配置文件](self-asserted-technical-profile.md#display-claims)中定义的**显示声明**类似，显示声明表示在显示控件中要从用户那里收集的声明。 引用的 **ClaimType** 元素需要指定 Azure AD B2C 支持的某个用户输入类型的 **UserInputType** 元素，例如 `TextBox` 或 `DropdownSingleSelect`。 如果显示声明值是某个**操作**所必需的，请将 **Required** 属性设置为 `true` 来强制用户为该特定的显示声明提供一个值。

某些显示声明是某些类型的显示控件所必需的。 例如，**VerificationCode** 是 **VerificationControl** 类型的显示控件所必需的。 请使用 **ControlClaimType** 属性指定为该必需声明指定了哪个 DisplayClaim。 例如：

```xml
<DisplayClaim ClaimTypeReferenceId="otpCode" ControlClaimType="VerificationCode" Required="true" />
```

### <a name="output-claims"></a>输出声明

显示控件的**输出声明**不会发送到下一个业务流程步骤。 它们仅暂时保存以用于当前显示控件会话。 这些暂时声明可在同一显示控件的不同操作之间共享。

若要将输出声明传播到下一个业务流程步骤，请使用引用此显示控件的实际自断言技术配置文件的 **OutputClaims**。

### <a name="display-control-actions"></a>显示控件操作

显示控件的**操作**是用户在客户端（浏览器）执行特定操作时在 Azure AD B2C 后端发生的过程。 例如，当用户选择页面上的某个按钮时要执行的验证。

操作定义**验证技术配置文件**的列表。 它们用于验证显示控件的部分或全部显示声明。 验证技术配置文件将验证用户输入，并可能向用户返回错误。 可以在显示控件操作中使用 **ContinueOnError**、**ContinueOnSuccess** 和 **Preconditions**，使用方式类似于在自断言技术配置文件中的[验证技术配置文件](validation-technical-profile.md)中使用它们的方式。

下面的示例根据用户选择的 **mfaType** 声明通过电子邮件或短信来发送代码。

```xml
<Action Id="SendCode">
  <ValidationClaimsExchange>
    <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="AzureMfa-SendSms">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>mfaType</Value>
          <Value>email</Value>
          <Action>SkipThisValidationTechnicalProfile</Action>
        </Precondition>
      </Preconditions>
    </ValidationClaimsExchangeTechnicalProfile>
    <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="AadSspr-SendEmail">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>mfaType</Value>
          <Value>phone</Value>
          <Action>SkipThisValidationTechnicalProfile</Action>
        </Precondition>
      </Preconditions>
    </ValidationClaimsExchangeTechnicalProfile>
  </ValidationClaimsExchange>
</Action>
```

## <a name="referencing-display-controls"></a>引用显示控件

显示控件在[自断言技术配置文件](self-asserted-technical-profile.md)的[显示声明](self-asserted-technical-profile.md#display-claims)中引用。

例如：

```xml
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
  ...
  <DisplayClaims>
    <DisplayClaim DisplayControlReferenceId="emailVerificationControl" />
    <DisplayClaim DisplayControlReferenceId="PhoneVerificationControl" />
    <DisplayClaim ClaimTypeReferenceId="displayName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="givenName" Required="true" />
    <DisplayClaim ClaimTypeReferenceId="surName" Required="true" />
```
