---
title: PowerShell 示例-使用默认域的应用程序代理应用
description: PowerShell 示例，列出了所有使用默认域（msappproxy.net）的 Azure Active Directory （Azure AD）应用程序代理应用程序。
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
ms.openlocfilehash: 40a9f7ce86981d6f03b25237a7a73869fd5fd4ba
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "75483370"
---
# <a name="get-all-application-proxy-apps-using-default-domains-msappproxynet"></a>获取所有使用默认域 (.msappproxy.net) 的应用程序代理应用

此 PowerShell 脚本示例列出了所有使用默认域（msappproxy.net）的 Azure Active Directory （Azure AD）应用程序代理应用程序。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

此示例需要用于图形模块（AzureAD）的[AzureAD V2 powershell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0)或[graph 模块预览版本](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview)（AzureADPreview）的 AzureAD v2 powershell。

## <a name="sample-script"></a>示例脚本

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-default-domain-apps.ps1 "Get all Application Proxy apps using default domains (.msappproxy.net)")]

## <a name="script-explanation"></a>脚本说明

| Command | 说明 |
|---|---|
|[Get-AzureADServicePrincipal](https://docs.microsoft.com/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | 获取服务主体。 |
|[Get-AzureADApplication](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0) | 获取 Azure AD 应用程序。 |
|[Get-AzureADApplicationProxyApplication](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplicationproxyapplication?view=azureadps-2.0) | 检索为 Azure AD 中的应用程序代理配置的应用程序。 |

## <a name="next-steps"></a>后续步骤

有关 Azure AD PowerShell 模块的详细信息，请参阅[Azure AD powershell 模块概述](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0)。

有关应用程序代理的其他 PowerShell 示例，请参阅[Azure AD 应用程序代理的 Azure AD powershell 示例](../application-proxy-powershell-samples.md)。