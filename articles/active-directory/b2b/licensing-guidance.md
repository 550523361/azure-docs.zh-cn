---
title: B2B 协作许可指南-Azure Active Directory |Microsoft Docs
description: Azure Active Directory B2B 协作无需付费的 Azure AD 许可证，但你仍然可以为 B2B 来宾用户获取付费功能
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28d02160163da4081ad8adbe233b27fee970a0df
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "74868841"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Azure Active Directory B2B 协作授权指南

借助 Azure Active Directory (Azure AD) 企业到企业 (B2B) 协作可以邀请外部用户（或“来宾用户”）使用你的付费 Azure AD 服务。 某些功能是免费的，但对于任何付费 Azure AD 功能，你可以为你的员工或租户中的非来宾用户最多为你拥有的每个 Azure AD 版本许可证邀请最多5个来宾用户。

> [!NOTE]
> 有关 Azure AD 定价和 B2B 协作功能的详细信息，请参阅[Azure Active Directory 定价](https://azure.microsoft.com/pricing/details/active-directory/)。

B2B 来宾用户许可按照 5:1 的比例自动计算和报告。 

此外，来宾用户可以使用免费的 Azure AD 功能，而不用满足附加的许可要求。 即使你没有任何付费的 Azure AD 许可证，来宾用户也有权访问免费的 Azure AD 功能。 

## <a name="examples-calculating-guest-user-licenses"></a>示例：计算来宾用户许可证数
确定有多少个来宾用户需要访问付费的 Azure AD 服务之后，请确保提供足够数量的 Azure AD 付费许可证，以按所需的 1:5 比例涵盖来宾用户。 下面是一些示例：

- 你要将100来宾用户邀请到 Azure AD 应用或服务，并提供访问管理和预配。 对于这些来宾用户中的50，你还需要进行 MFA 和条件访问，因此对于这些功能，你需要10个 Azure AD Premium P1 许可证。 如果你打算对来宾用户使用“标识保护”功能，需要按相同的 1:5 比例提供 Azure AD Premium P2 许可证才能涵盖这些来宾用户。
- 你想要邀请 60 个来宾用户，而这些用户都需要执行 MFA，因此，必须至少有 12 个 Azure AD Premium P1 许可证。 10 名员工有 Azure AD Premium P1 许可证，因此，根据 1:5 的许可比例，最多允许 50 个来宾用户。 需要额外购买两个 Premium P1 许可证才能涵盖 10 个附加的来宾用户。

## <a name="next-steps"></a>后续步骤

请参阅以下有关 Azure AD B2B 协作的资源：

* [Azure Active Directory 定价](https://azure.microsoft.com/pricing/details/active-directory/)
* [什么是 Azure AD B2B 协作？](what-is-b2b.md)
* [Azure Active Directory B2B 协作常见问题 (FAQ)](faq.md)
