---
title: 排查已加入混合 Azure Active Directory 的下层设备问题 | Microsoft Docs
description: 排查已加入混合 Azure Active Directory 的下层设备问题。
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 2c50ba1abfe3681a39b39bf52f127efd9d518aef
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "43041862"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>排查已加入混合 Azure Active Directory 的下层设备问题 

本文仅适用于以下设备： 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

对于 Windows 10 或 Windows Server 2016，请参阅[排查已加入混合 Azure Active Directory 的 Windows 10 和 Windows Server 2016 设备问题](troubleshoot-hybrid-join-windows-current.md)。

本文假设你已[配置已加入混合 Azure Active Directory 的设备](hybrid-azuread-join-plan.md)，以支持以下方案：

- 基于设备的条件访问

- [企业设置漫游](../active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello for Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification) 





本文提供有关如何解决潜在问题的故障排除指导。  

**应了解的内容：** 

- 每个用户的最大设备数以设备为中心。 例如：如果 *jdoe* 和 *jharnett* 登录到某个设备，会在“用户信息”选项卡中为其中每个用户单独创建一个注册 (DeviceID)。  

- 设备的初始注册/加入配置为在登录或锁定/解锁时执行尝试。 可能会有 5 分钟的延迟，由任务计划程序任务触发。 

- 由于操作系统的重新安装或手动重新注册，在用户信息选项卡上可能会出现同一设备的多个条目。 

- 对于 Windows 7 SP1 或 Windows Server 2008 R2 SP1，请确保安装 [KB4284842](https://support.microsoft.com/help/4284842)。 此更新可防止将来因客户更改密码后无法访问受保护密钥而导致身份验证失败。

## <a name="step-1-retrieve-the-registration-status"></a>步骤 1：检索注册状态 

**验证注册状态：**  

1. 使用已执行混合 Azure AD 加入的用户帐户登录。

2. 以管理员身份打开命令提示符 

3. 键入 `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

此命令会显示一个对话框，其中提供了有关加入状态的更多详细信息。

![适用于 Windows 的工作区加入](./media/troubleshoot-hybrid-join-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>步骤 2：评估混合 Azure AD 加入状态 

如果混合 Azure AD 加入不成功，对话框中会提供有关发生的问题的详细信息。

**最常见的问题包括：**

- AD FS 或 Azure AD 配置不当

    ![适用于 Windows 的工作区加入](./media/troubleshoot-hybrid-join-windows-legacy/02.png)

- 登录身份不是域用户

    ![适用于 Windows 的工作区加入](./media/troubleshoot-hybrid-join-windows-legacy/03.png)
    
    以下几种不同原因可能会导致此问题：
    
    - 已登录的用户不是域用户（例如，本地用户）。 低级别设备上的混合 Azure AD 联接仅支持域用户。
    
    - Autoworkplace.exe 无法以无提示方式通过 Azure AD 或 AD FS 进行身份验证。 这可能是由于 Azure AD URL 的出站网络连接问题导致的。 另外，还可能是由于为用户启用/配置了多重身份验证 (MFA)，但没有在联合服务器上配置 WIAORMUTLIAUTHN。 另一种可能性是主领域发现 (HRD) 页面正在等待用户交互，从而阻止了 **autoworkplace.exe** 以无提示方式请求令牌。
    
    - 你的组织使用 Azure AD 无缝单一登录，设备的 IE intranet 设置中不存在 `https://autologon.microsoftazuread-sso.com` 或 `https://aadg.windows.net.nsatc.net`，未对 Intranet 区域启用“允许通过脚本更新状态栏”。

- 已达到配额

    ![适用于 Windows 的工作区加入](./media/troubleshoot-hybrid-join-windows-legacy/04.png)

- 服务未响应 

    ![适用于 Windows 的工作区加入](./media/troubleshoot-hybrid-join-windows-legacy/05.png)

此外，也可以在“应用程序和服务日志\Microsoft-Workplace Join”下面的事件日志中找到状态信息
  
**混合 Azure AD 加入失败的最常见原因是：** 

- 计算机既没有连接到组织的内部网络，也没有连接到与本地 AD 域控制器建立连接的 VPN。

- 使用本地计算机帐户登录到了计算机。 

- 服务配置问题： 

  - 联合服务器已配置为支持 **WIAORMULTIAUTHN**。 

  - 在 Azure AD 中，计算机的林内没有指向已验证域名的“服务连接点”对象 

  - 用户已达到设备限制。 

## <a name="next-steps"></a>后续步骤

如有问题，请参阅[设备管理常见问题解答](faq.md)  
