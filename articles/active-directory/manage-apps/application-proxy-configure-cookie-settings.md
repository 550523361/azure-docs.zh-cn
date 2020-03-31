---
title: 应用程序代理 cookie 设置 - Azure Active Directory | Microsoft 文档
description: Azure Active Directory (Azure AD) 具有访问和会话 cookie，可通过应用程序代理访问本地应用程序。 本文会介绍如何使用和配置 cookie 设置。
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcedb24a0efdbabaaef150fc3d5aff07d210ce23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481358"
---
# <a name="cookie-settings-for-accessing-on-premises-applications-in-azure-active-directory"></a>用于在 Azure Active Directory 中访问本地应用程序的 Cookie 设置

Azure Active Directory (Azure AD) 具有访问和会话 cookie，可通过应用程序代理访问本地应用程序。 了解如何使用应用程序代理 cookie 设置。 

## <a name="what-are-the-cookie-settings"></a>Cookie 设置有哪些？

[应用程序代理](application-proxy.md)使用以下访问和会话 cookie 设置。

| Cookie 设置 | 默认 | 描述 | 建议 |
| -------------- | ------- | ----------- | --------------- |
| 使用仅限 HTTP 的 Cookie | **否** | **是**允许应用程序代理在 HTTP 响应标头中包含 HTTPOnly 标志。 此标志提升安全性，例如，它会阻止客户端脚本 (CSS) 复制或修改 cookie。<br></br><br></br>在我们支持仅 HTTP 设置之前，应用程序代理通过安全的 TLS 通道加密和传输 Cookie，以防止修改。 | 选择“是”****，可提升安全性。<br></br><br></br>为那些需要访问会话 cookie 的客户端或用户代理使用“否”****。 例如，为通过应用程序代理连接远程桌面网关服务器的 RDP 或 MTSC 客户端使用“否”****。|
| 使用安全 Cookie | **否** | **是**允许应用程序代理在 HTTP 响应标头中包含 Secure 标志。 Secure Cookie 通过经由 TLS 保护的通道（如 HTTPS）来传输 cookie，可增强安全性。 由于以明文形式传输 cookie，这样可防止 cookie 被未经授权的参与方观察到。 | 选择“是”****，可提升安全性。|
| 使用永久性 Cookie | **否** | **是**允许应用程序代理设置其访问 cookie，使其在 Web 浏览器关闭时不会过期。 其有效性一直持续到访问令牌过期为止，或持续到用户手动删除持久性 cookie 为止。 | 使用“否”**** 是由于使用户一直处于通过验证的状态会产生安全风险。<br></br><br></br>建议仅为不能在进程间共享 cookie 的较旧应用程序使用“是”****。 更好的做法是更新应用程序，在进程之间共享 cookie，而不是使用持久性 cookie。 例如，可能需要持久性 cookie 来允许用户从 SharePoint 站点的资源管理器视图中打开 Office 文档。 在没有持久性 cookie 的情况下，如果不在浏览器、资源管理器进程和 Office 进程之间共享访问 cookie，则此操作可能失败。 |

## <a name="samesite-cookies"></a>同一网站饼干
从 Chrome 80 版本开始，最终在浏览器中利用铬，不指定[SameSite](https://web.dev/samesite-cookies-explained)属性的 Cookie 将被视为"同**一网站_Lax**"。 SameSite 属性声明如何将 Cookie 限制为同一站点上下文。 当设置为 Lax 时，Cookie 只能发送到同一站点请求或顶级导航。 但是，应用程序代理要求在第三方上下文中保留这些 Cookie，以便在用户会话期间保持正确登录。 因此，我们正在更新应用程序代理访问和会话 Cookie，以避免此更改的不利影响。 更新包括：

* 将**同一网站**属性设置为 **"无**"。 这允许在第三方上下文中正确发送应用程序代理访问和会话 Cookie。
* 将 **"使用安全 Cookie"** 设置设置为默认使用 **"是**"。 Chrome 还要求 Cookie 指定安全标志，否则将被拒绝。 此更改将应用于通过应用程序代理发布的所有现有应用程序。 请注意，应用程序代理访问 Cookie 始终设置为"安全"，并且仅通过 HTTPS 传输。 此更改仅适用于会话 Cookie。

对应用程序代理 Cookie 的这些更改将在 Chrome 80 发布日期之前的几周内推出。

此外，如果您的后端应用程序具有需要在第三方上下文中可用的 Cookie，则必须通过将应用程序更改为对这些 Cookie 使用 SameSite_None 来明确选择加入。 应用程序代理将 Set-Cookie 标头转换为其 URLS，并将尊重后端应用程序设置的这些 Cookie 的设置。



## <a name="set-the-cookie-settings---azure-portal"></a>设置 cookie 设置 - Azure 门户
使用 Azure 门户设置 cookie 设置：

1. 登录到 Azure[门户](https://portal.azure.com)。 
2. 导航到**Azure 活动目录** > **企业应用程序** > **所有应用程序**。
3. 选择要为其启用 cookie 设置的应用程序。
4. 单击**应用程序代理**。
5. 在“其他设置”**** 下，将 cookie 设置设置为“是”**** 或“否”****。
6. 单击“保存”**** 应用所做的更改。 

## <a name="view-current-cookie-settings---powershell"></a>查看当前 Cookie 设置 - 电源外壳

要查看应用程序的当前 Cookie 设置，请使用此 PowerShell 命令：  

```powershell
Get-AzureADApplicationProxyApplication -ObjectId <ObjectId> | fl * 
```

## <a name="set-cookie-settings---powershell"></a>设置 Cookie 设置 - 电源外壳

在以下 PowerShell 命令```<ObjectId>```中，是应用程序的 ObjectId。 

**仅 Http-只 Cookie** 

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $false 
```

**安全饼干**

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $false 
```

**持久饼干**

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $false 
```
