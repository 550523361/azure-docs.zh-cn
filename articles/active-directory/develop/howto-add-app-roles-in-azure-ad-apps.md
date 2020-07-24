---
title: 添加应用角色并从令牌获取它们 | Azure
titleSuffix: Microsoft identity platform
description: 了解如何在注册到 Azure Active Directory 的应用程序中添加应用角色、如何向这些角色分配用户和组，以及如何在令牌的 `roles` 声明中接收它们。
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 07/15/2020
ms.author: kkrishna
ms.reviewer: kkrishna, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 0ec314e6b5abde60102dacfc81c9303cef16e887
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87058622"
---
# <a name="how-to-add-app-roles-in-your-application-and-receive-them-in-the-token"></a>如何：在应用程序中添加应用角色并在令牌中接收它们

基于角色的访问控制 (RBAC) 是一种常用的机制，用于在应用程序中强制进行授权。 使用 RBAC 时，管理员将权限授予角色而不是单个用户或组。 然后，管理员再将角色分配给不同的用户和组，以便控制用户对特定内容和功能的访问。

将 RBAC 与应用程序角色和角色声明配合使用，开发人员就可以安全地在应用中强制实施授权，相当轻松。

另一种方法是使用 Azure AD 组和组声明，如 [WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet) 中所示。 Azure AD 组和应用程序角色绝不会互相排斥，它们可以配合使用，进行更精细的访问控制。

## <a name="declare-roles-for-an-application"></a>为应用程序声明角色

这些应用程序角色在 [Azure 门户](https://portal.azure.com)的应用程序注册清单中定义。  当用户登录到应用程序时，Azure AD 会针对每个角色发出一个 `roles` 声明。这些角色包括单独授予用户的，以及通过组成员身份获得的。  可以通过门户的 UI 为用户或组分配角色，也可以使用 [Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/azuread-identity-access-management-concept-overview) 以编程方式进行分配。

### <a name="declare-app-roles-using-azure-portal"></a>使用 Azure 门户声明应用角色

1. 登录 [Azure 门户](https://portal.azure.com)。
1. 在门户工具栏中选择“目录 + 订阅”图标。
1. 在“收藏夹”或“所有目录”列表中，选择要在其中注册应用程序的 Active Directory 租户。 
1. 在 Azure 门户中，搜索并选择“Azure Active Directory”。
1. 在“Azure Active Directory”窗格中选择“应用注册”，查看一个包含所有应用程序的列表 。
1. 选择一个应用程序，以便在其中定义应用角色。 然后选择“清单”。
1. 编辑应用清单，方法是先查找 `appRoles` 设置，然后添加所有应用程序角色。

     > [!NOTE]
     > 此清单中的每个应用角色定义必须在清单上下文中有不同的有效 GUID（针对 `id` 属性）。
     >
     > 每个应用角色定义的 `value` 属性应该与应用程序的代码中使用的字符串完全匹配。 `value` 属性不能包含空格。 如果包含空格，则在保存清单时会收到错误。

1. 保存清单。

### <a name="examples"></a>示例

以下示例显示的 `appRoles` 可以分配给 `users`。

> [!NOTE]
>`id` 必须是唯一的 GUID。

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "User"
      ],
      "displayName": "Writer",
      "id": "d1c2ade8-98f8-45fd-aa4a-6d06b947c66f",
      "isEnabled": true,
      "description": "Writers Have the ability to create tasks.",
      "value": "Writer"
    }
  ],
"availableToOtherTenants": false,
```

> [!NOTE]
>`displayName` 不能包含空格。

可以针对 `users` 和/或 `applications` 来定义应用角色。 如果可用于 `applications` ，应用角色将作为 "**管理**" 部分下的 "应用程序权限" 显示 > **Api 权限 "> 添加 > 我的 Api 的权限 > 选择 api > 应用程序权限**。 以下示例显示一个以 `Application` 为目标的应用角色。

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "Application"
      ],
      "displayName": "ConsumerApps",
      "id": "47fbb575-859a-4941-89c9-0f7a6c30beac",
      "isEnabled": true,
      "description": "Consumer apps have access to the consumer data.",
      "value": "Consumer"
    }
  ],
"availableToOtherTenants": false,
```

定义的角色数会影响应用程序清单具有的限制。 它们已在[清单限制](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest#manifest-limits)页面上进行了详细讨论。

### <a name="assign-users-and-groups-to-roles"></a>将用户和组分配到角色

在应用程序中添加应用角色以后，即可将这些角色分配给用户和组。

1. 使用 **Azure Active Directory** 左侧导航菜单，在“Azure Active Directory”窗格中选择“企业应用程序” 。
1. 选择“所有应用程序”，查看所有应用程序的列表。

     如果看不到希望其显示在这里的应用程序，请使用“所有应用程序”列表顶部的各种筛选器来限制此列表，或者在列表中向下滚动，以便找到应用程序。

1. 选择一个应用程序，以便在其中为角色分配用户或安全组。
1. 在应用程序的左侧导航菜单中选择“用户和组”窗格。
1. 在“用户和组”列表顶部选择“添加用户”按钮，以便打开“添加分配”窗格。  
1. 在“添加分配”窗格中，选择“用户和组”选择器。

     将会显示用户和安全组的列表和一个文本框，后者用于搜索和查找特定用户或组。 此屏幕允许一次选择多个用户和组。

1. 选择好用户和组以后，按底部的“选择”按钮即可转到下一部分。
1. 在“添加分配”窗格中，选择“选择角色”选择器。 此前在应用清单中声明的所有角色都会显示。
1. 选择一个角色，然后按“选择”按钮。
1. 按底部的“分配”按钮即可完成将用户和组分配到应用的操作。
1. 确认已添加的用户和组显示在更新的“用户和组”列表中。

### <a name="receive-roles-in-tokens"></a>接收令牌中的角色

当分配给不同应用程序角色的用户登录到应用程序时，其令牌将在声明中具有分配给他们的角色 `roles` 。

## <a name="more-information"></a>详细信息

- [将使用应用角色和角色声明的授权添加到 ASP.NET Core Web 应用](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/5-WebApp-AuthZ/5-1-Roles)
- [在应用程序中通过 Microsoft 标识平台实现授权（视频）](https://www.youtube.com/watch?v=LRoc-na27l0)
- [Azure Active Directory 现在可以与组声明和应用程序角色配合使用](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-now-with-Group-Claims-and-Application/ba-p/243862)
- [Azure Active Directory 应用清单](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)
- [AAD 访问令牌](access-tokens.md)
- [AAD `id_tokens`](id-tokens.md)
