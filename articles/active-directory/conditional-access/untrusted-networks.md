---
title: 要求来自不受信任网络的访问进行 MFA - Azure Active Directory
description: 了解如何在 Azure Active Directory (Azure AD) 中针对来自不受信任网络的访问尝试配置条件访问策略。
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7986ca441f7d274670d8fa0238e7dcfa01497b6f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85253165"
---
# <a name="how-to-require-mfa-for-access-from-untrusted-networks-with-conditional-access"></a>如何：使用条件访问要求来自不受信任网络的访问进行 MFA   

Azure Active Directory (Azure AD) 允许从任何位置以单一登录方式登录到设备、应用和服务。 用户不但可以从组织的网络访问云应用，而且可以从任何不受信任的 Internet 位置访问云应用。 对于来自不受信任网络的访问，常见的最佳做法是要求其进行多重身份验证 (MFA)。

本文提供了在配置条件访问策略以要求来自不受信任网络的访问进行 MFA 时需要的信息。 

## <a name="prerequisites"></a>先决条件

本文假定你熟悉以下内容： 

- Azure AD 条件访问的[基本概念](overview.md) 
- 在 Azure 门户中配置条件访问策略的[最佳做法](best-practices.md)

## <a name="scenario-description"></a>方案描述

为掌控安全性与工作效率之间的平衡，对于你来说，对于来自你的组织网络的登录，只需要要求其提供密码可能就足够了。 但是，对于来自不受信任网络位置的访问，登录不是由合法用户执行的这一风险会增大。 要解决此顾虑，可以阻止来自不受信任网络的访问。 另外，还可以要求进行多重身份验证 (MFA) 来获得额外的保证，以确保访问尝试是由该帐户的合法所有者执行的。 

使用 Azure AD 条件访问，可以通过进行授权的以下单个策略来解决此要求： 

- 授予对所选云应用的访问权限
- 为所选用户和组授予权限  
- 要求进行多重身份验证 
- 当访问来自： 
   - 不受信任的位置

## <a name="implementation"></a>实现

此场景的挑战在于将“来自不受信任网络位置的访问”  转换为条件访问条件。 在条件访问策略中，可以配置[位置条件](location-condition.md)来应对与网络位置相关的场景。 使用位置条件，你可以选择已命名位置，这些位置是 IP 地址范围、国家和地区的逻辑分组。  

通常，你的组织拥有一个或多个地址范围，例如 199.30.16.0 - 199.30.16.15。
可以通过以下方式配置命名位置：

- 指定此范围 (199.30.16.0/28) 
- 分配一个描述性名称，例如**公司网络** 

可以选择以下选项，而不是尝试定义不受信任的所有位置：

- 包括任何位置 

   ![条件访问](./media/untrusted-networks/02.png)

- 排除所有受信任的位置 

   ![条件访问](./media/untrusted-networks/01.png)

## <a name="policy-deployment"></a>策略部署

使用本文中概述的方法，你现在可以针对不受信任位置配置条件访问策略。 若要确保你的策略按预期工作，建议的最佳做法是在将其推广到生产环境之前对其进行测试。 理想情况下，使用一个测试租户来验证新策略是否按预期方式工作。 有关详细信息，请参阅[如何部署新策略](best-practices.md#how-should-you-deploy-a-new-policy)。 

## <a name="next-steps"></a>后续步骤

若要了解有关条件访问的详细信息，请参阅[什么是 Azure Active Directory 中的条件访问？](../active-directory-conditional-access-azure-portal.md)
