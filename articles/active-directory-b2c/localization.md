---
title: Localization - Azure Active Directory B2C
description: 在 Azure Active Directory B2C 中指定自定义策略的 Localization 元素。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e73eae4d66f4ff94a48dfa27e258f8ba8ef87633
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79126752"
---
# <a name="localization"></a>本地化

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

使用 **Localization** 元素可在用户旅程的策略中支持多个区域设置或语言。 使用策略中的本地化支持可以：

- 在策略中设置支持的语言的显式列表和选择默认语言。
- 提供特定于语言的字符串和集合。

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
  <LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedResources Id="api.localaccountsignup.es">
  ...
```

**Localization** 元素包含以下属性：

| 特性 | 必选 | 描述 |
| --------- | -------- | ----------- |
| 已启用 | 否 | 可能的值：`true` 或 `false`。 |

**Localization** 元素包含以下 XML 元素

| 元素 | 出现次数 | 描述 |
| ------- | ----------- | ----------- |
| SupportedLanguages | 1:n | 支持的语言列表。 |
| LocalizedResources | 0:n | 本地化资源列表。 |

## <a name="supportedlanguages"></a>SupportedLanguages

**SupportedLanguages** 元素包含以下属性：

| 特性 | 必选 | 描述 |
| --------- | -------- | ----------- |
| DefaultLanguage | 是 | 用作本地化资源默认值的语言。 |
| MergeBehavior | 否 | 与父策略中具有相同标识符的任何 ClaimType 合并在一起的值的枚举值。 覆盖基本策略中指定的声明时，请使用此属性。 可能的值：`Append`、`Prepend` 或 `ReplaceAll`。 `Append` 值指定应将现有数据集合追加到父策略中指定的集合的末尾。 `Prepend` 值指定应将现有数据集合添加到父策略中指定的集合的前面。 `ReplaceAll` 值指定应忽略父策略中定义的数据集合，改用当前策略中定义的数据。 |

### <a name="supportedlanguages"></a>SupportedLanguages

**SupportedLanguages** 元素包含以下元素：

| 元素 | 出现次数 | 描述 |
| ------- | ----------- | ----------- |
| SupportedLanguage | 1:n | 显示符合 RFC 5646“用于标识语言的标记”中所述语言标记的内容。 |

## <a name="localizedresources"></a>LocalizedResources

**LocalizedResources** 元素包含以下属性：

| 特性 | 必选 | 描述 |
| --------- | -------- | ----------- |
| ID | 是 | 用于唯一标识本地化资源的标识符。 |

**LocalizedResources** 元素包含以下元素：

| 元素 | 出现次数 | 描述 |
| ------- | ----------- | ----------- |
| LocalizedCollections | 0:n | 在各种区域性中定义整个集合。 一个集合可以包含不同的项数，以及适用于各种区域性的不同字符串。 集合的示例包括声明类型中显示的枚举。 例如，在下拉列表中向用户显示国家/地区列表。 |
| LocalizedStrings | 0:n | 在各种区域性中定义所有字符串，但集合中出现的字符串除外。 |

### <a name="localizedcollections"></a>LocalizedCollections

**LocalizedCollections** 元素包含以下元素：

| 元素 | 出现次数 | 描述 |
| ------- | ----------- | ----------- |
| LocalizedCollection | 1:n | 支持的语言列表。 |

#### <a name="localizedcollection"></a>LocalizedCollection

**LocalizedCollection** 元素包含以下属性：

| 特性 | 必选 | 描述 |
| --------- | -------- | ----------- |
| ElementType | 是 | 引用策略文件中的 ClaimType 元素或用户界面元素。 |
| ElementId | 是 | 一个字符串，包含当 **ElementType** 设置为 ClaimType 时使用的 ClaimsSchema 节中已定义的声明类型的引用。 |
| TargetCollection | 是 | 目标集合。 |

**LocalizedCollection** 元素包含以下元素：

| 元素 | 出现次数 | 描述 |
| ------- | ----------- | ----------- |
| Item | 0:n | 定义可让用户在用户界面中为声明选择的可用选项，例如下拉列表中的值。 |

**Item** 元素包含以下属性：

| 特性 | 必选 | 描述 |
| --------- | -------- | ----------- |
| Text | 是 | 应在用户界面中向用户显示的此选项的用户友好字符串。 |
| “值” | 是 | 与此选项关联的字符串声明值。 |
| SelectByDefault | 否 | 指示默认情况下是否应在 UI 中选择此选项。 可能的值：True 或 False。 |

以下示例演示了 **LocalizedCollections** 元素的用法。 其中包含两个 **LocalizedCollection** 元素，一个元素适用于英语区域设置，另一个元素适用于西班牙语区域设置。 这两个元素都设置了声明 `Gender` 的 **Restriction** 集合，以及适用于英语和西班牙语的项列表。

```XML
<LocalizedResources Id="api.selfasserted.en">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Female" Value="F" />
      <Item Text="Male" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>

<LocalizedResources Id="api.selfasserted.es">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Femenino" Value="F" />
      <Item Text="Masculino" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>
```

### <a name="localizedstrings"></a>LocalizedStrings

**LocalizedStrings** 元素包含以下元素：

| 元素 | 出现次数 | 描述 |
| ------- | ----------- | ----------- |
| LocalizedString | 1:n | 一个本地化字符串。 |

**LocalizedString** 元素包含以下属性：

| 特性 | 必选 | 描述 |
| --------- | -------- | ----------- |
| ElementType | 是 | 对策略中声明类型元素或用户界面元素的引用。 可能的值： `ClaimType` `UxElement`、 `ErrorMessage` `Predicate`、 `GetLocalizedStringsTransformationClaimType`、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 、 `ClaimType` 值用于本地化 StringId 中指定的某个声明属性。 `UxElement` 值用于本地化 StringId 中指定的某个用户界面元素。 `ErrorMessage` 值用于本地化 StringId 中指定的某个系统错误消息。 `Predicate` 值用于本地化 StringId 中指定的某个 [Predicate](predicates.md) 错误消息。 `InputValidation` 值用于本地化 StringId 中指定的某个 [PredicateValidation](predicates.md) 组错误消息。 该`GetLocalizedStringsTransformationClaimType`值用于将本地化字符串复制到声明中。 有关详细信息，请参阅[获取本地化字符串转换声明转换](string-transformations.md#getlocalizedstringstransformation)  | 
| ElementId | 是 | 如果**ElementType**设置为`ClaimType` `Predicate`，或`InputValidation`， 此元素包含对声明架构部分中已定义的声明类型的引用。 |
| StringId | 是 | 如果 **ElementType** 设置为 `ClaimType`，此元素包含对声明类型的属性的引用。 可能的值：`DisplayName`、`AdminHelpText` 或 `PatternHelpText`。 `DisplayName` 值用于设置声明显示名称。 `AdminHelpText` 值用于设置声明用户的帮助文本名称。 `PatternHelpText` 值用于设置声明模式帮助文本。 如果 **ElementType** 设置为 `UxElement`，此元素包含对用户界面元素的属性的引用。 如果 **ElementType** 设置为 `ErrorMessage`，此元素指定错误消息的标识符。 有关 `UxElement` 标识符的完整列表，请参阅[本地化字符串 ID](localization-string-ids.md)。|


以下示例演示了一个本地化的注册页。 前三个 **LocalizedString** 值设置声明属性。 第三个值更改继续按钮的值。 最后一个值更改错误消息。

```XML
<LocalizedResources Id="api.selfasserted.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

以下示例演示了 ID 为 `IsLengthBetween8And64` 的 **Predicate** 的本地化 **UserHelpText**。 另外还演示了 ID 为 `CharacterClasses` 的 **PredicateGroup**，以及 ID 为 `StrongPassword` 的 **PredicateValidation** 的本地化 **UserHelpText**。

```XML
<PredicateValidation Id="StrongPassword">
  <PredicateGroups>
    ...
    <PredicateGroup Id="CharacterClasses">
    ...
    </PredicateGroup>
  </PredicateGroups>
</PredicateValidation>

...

<Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
  ...
</Predicate>
...


<LocalizedString ElementType="InputValidation" ElementId="StrongPassword" StringId="CharacterClasses">The password must have at least 3 of the following:</LocalizedString>

<LocalizedString ElementType="Predicate" ElementId="IsLengthBetween8And64" StringId="HelpText">The password must be between 8 and 64 characters.</LocalizedString>
```

## <a name="set-up-localization"></a>设置本地化

本文将介绍如何在用户旅程的策略中支持多个区域设置或语言。 本地化需要执行三个步骤：设置受支持语言的显式列表，提供特定于语言的字符串和集合，以及编辑页面的 ContentDefinition。

### <a name="set-up-the-explicit-list-of-supported-languages"></a>设置受支持语言的显式列表

在 **BuildingBlocks** 元素下，添加包含受支持语言列表的 **Localization** 元素。 以下示例演示如何为英语（默认）和西班牙语定义本地化支持：

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
</Localization>
```

## <a name="next-steps"></a>后续步骤

有关本地化示例，请参阅以下文章：

- [使用 Azure 活动目录 B2C 中的自定义策略进行语言自定义](custom-policy-localization.md)
- [使用 Azure 活动目录 B2C 中的用户流进行语言自定义](user-flow-language-customization.md)
