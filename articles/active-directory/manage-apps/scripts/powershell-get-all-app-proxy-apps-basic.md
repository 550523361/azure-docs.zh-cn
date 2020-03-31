---
title: PowerShell 示例 - 列出应用程序代理应用的基本信息
description: 列出 Azure 活动目录 （Azure AD） 应用程序代理应用程序以及应用程序 ID （AppId）、名称（显示名称）和对象 ID （ObjId） 的 PowerShell 示例。
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 009aabb8b742a752a29d775efbd11b153588c09b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75482213"
---
# <a name="get-all-application-proxy-apps-and-list-basic-information"></a>获取所有应用程序代理应用并列出基本信息

此 PowerShell 脚本示例列出了有关所有 Azure 活动目录 （Azure AD） 应用程序代理应用程序的信息，包括应用程序 ID （AppId）、名称（显示名称）和对象 ID （ObjId）。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

此示例需要用于图形模块 （AzureAD）[的 AzureAD V2 电源外壳](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0)，或[用于图形模块预览版本的 AzureAD V2 电源外壳](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview)（AzureAD 预览版）。
## <a name="sample-script"></a>示例脚本

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-appproxy-apps-basic.ps1 "Get all Application Proxy apps")]

## <a name="script-explanation"></a>脚本说明

| 命令 | 说明 |
|---|---|
|[Get-AzureADServicePrincipal](https://docs.microsoft.com/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | 获取服务主体。 |

## <a name="next-steps"></a>后续步骤

有关 Azure AD 电源外壳模块的详细信息，请参阅[Azure AD PowerShell 模块概述](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0)。

有关应用程序代理的其他 PowerShell 示例，请参阅[Azure AD 应用程序代理的 Azure AD PowerShell 示例](../application-proxy-powershell-samples.md)。