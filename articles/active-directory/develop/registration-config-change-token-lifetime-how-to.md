---
title: 更改自定义 Azure AD 应用的令牌生存期默认值
description: 如何为在 Azure AD 上开发的应用程序更新令牌生存期策略
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: ryanwi
ms.custom: aaddev, seoapril2019
ms.openlocfilehash: fc71ebe61fba8c1bdb6b7625b16a50d8995a581a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "80883347"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>如何为自定义开发的应用程序更改令牌生存期默认设置

本文介绍如何使用 Azure AD PowerShell 设置令牌生存期策略。 Azure AD Premium 允许应用程序开发人员和租户管理员配置为非机密客户端颁发的令牌的生存期。 根据租户范围或要访问的资源设置令牌生存期策略。

1. 若要设置令牌生存期策略，需要下载 [Azure AD PowerShell 模块](https://www.powershellgallery.com/packages/AzureADPreview)。
1. 运行 **Connect-AzureAD -Confirm** 命令。

    以下是一个策略示例，该策略设置最大期限单因素刷新令牌。 创建策略：```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>后续步骤

* 请参阅 [Azure AD 中的可配置令牌生存期](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes)，了解如何配置 Azure AD 颁发的令牌生存期，包括如何为组织中的所有应用、多租户应用或组织中的特定服务主体设置令牌生存期。 
* [Azure AD 令牌引用](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)
