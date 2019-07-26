---
title: Azure Active Directory 标识保护演练手册 | Microsoft Docs
description: 了解如何使用 Azure AD 标识保护来限制攻击者利用已泄露标识或设备的能力，以及保护之前怀疑或已知受到威胁的标识或设备。
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 273a6aca2050676650b955ec078b47b2ffcfe319
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333930"
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory 标识保护操作手册

此操作手册有助于：

* 通过模拟风险事件和漏洞在“标识保护”环境中填充数据
* 设置基于风险的条件性访问策略并测试这些策略的影响

## <a name="simulating-risk-events"></a>模拟风险事件

本部分向你提供模拟以下风险事件类型的步骤：

* 从匿名 IP 地址登录（简单）
* 从不熟悉的位置登录（中等）
* 不可能前往异常位置（困难）

不能以安全方式模拟其他风险事件。

### <a name="sign-ins-from-anonymous-ip-addresses"></a>从匿名 IP 地址登录

有关此风险事件的详细信息，请参阅[从匿名 IP 地址登录](../reports-monitoring/concept-risk-events.md#sign-ins-from-anonymous-ip-addresses)。 

完成以下过程需要使用：

- [Tor 浏览器](https://www.torproject.org/projects/torbrowser.html.en)，用于模拟匿名 IP 地址。 如果组织限制使用 Tor 浏览器，则可能需要使用虚拟机。
- 尚未注册进行多重身份验证的测试帐户。

**若要模拟从匿名 IP 登录，请执行以下步骤**：

1. 使用 [Tor 浏览器](https://www.torproject.org/projects/torbrowser.html.en)，导航到 [https://myapps.microsoft.com](https://myapps.microsoft.com)。   
2. 输入要在**从匿名 IP 地址登录**报告中显示的帐户的凭据。

登录会在 10 - 15 分钟内显示在“标识保护”仪表板上。 

### <a name="sign-ins-from-unfamiliar-locations"></a>从不熟悉的位置登录

有关此风险事件的详细信息，请参阅[从不熟悉的位置登录](../reports-monitoring/concept-risk-events.md#sign-in-from-unfamiliar-locations)。 

若要模拟不熟悉的位置，必须从测试帐户之前未登录过的位置和设备登录。

下面的过程使用新创建的以下项目：

- VPN 连接，用于模拟新位置。
- 虚拟机，用于模拟新设备。

完成以下过程需要使用具有以下特征的用户帐户：

- 至少 30 天的登录历史记录。
- 已启用多重身份验证。

**若要模拟从不熟悉的位置登录，请执行以下步骤**：

1. 使用测试帐户登录时，可以不通过 MFA 质询，造成 MFA 质询失败。
2. 使用新 VPN 导航到 [https://myapps.microsoft.com](https://myapps.microsoft.com)，然后输入测试帐户的凭据。

登录会在 10 - 15 分钟内显示在“标识保护”仪表板上。

### <a name="impossible-travel-to-atypical-location"></a>不可能前往异常位置

有关此风险事件的详细信息，请参阅[不可能前往异常位置](../reports-monitoring/concept-risk-events.md#impossible-travel-to-atypical-locations)。 

模拟不可能前往条件很难，因为该算法使用机器学习来清除误报，如来自熟悉设备的不可能前往，或从目录中其他用户使用的 VPN 登录。 另外，此算法要求用户 14 天 10 次的登录历史记录，才能开始生成风险事件。 考虑到机器学习模型的复杂性以及上述规则的存在，以下步骤有可能不会导致风险事件。 如果有多个 Azure AD 帐户，则可能需要复制这些步骤来发布该风险事件。

**若要模拟不可能前往异常位置，请执行以下步骤**：

1. 使用标准浏览器，导航到 [https://myapps.microsoft.com](https://myapps.microsoft.com)。  
2. 输入你想要为其生成不可能前往风险事件的帐户的凭据。
3. 更改用户代理。 可以在 Internet Explorer 中通过开发人员工具更改用户代理，或者在 Firefox 或 Chrome 中使用用户代理切换器加载项更改用户代理。
4. 更改 IP 地址。 可以在不同的数据中心中通过使用 VPN、Tor 加载项或在 Azure 中启动新计算机更改 IP 地址。
5. 像往常一样使用相同凭据并在上次登录后的数分钟内登录到 [https://myapps.microsoft.com](https://myapps.microsoft.com)。

登录会在 2-4 小时内显示在“标识保护”仪表板中。

## <a name="simulating-vulnerabilities"></a>模拟漏洞
漏洞是 Azure AD 环境中可能由危险分子利用的弱点。 Azure AD 标识保护中当前显示了 3 种类型的漏洞，这些漏洞会利用 Azure AD 的其他功能。 设置这些功能后，这些漏洞会自动显示在“标识保护”仪表板上。

* Azure AD [多重身份验证](../authentication/multi-factor-authentication.md)
* Azure AD [Cloud Discovery](https://docs.microsoft.com/cloud-app-security/)。
* Azure AD [Privileged Identity Management](../privileged-identity-management/pim-configure.md)。 

## <a name="testing-security-policies"></a>测试安全策略

此部分提供了用于测试用户风险和登录风险安全策略的步骤。

### <a name="user-risk-security-policy"></a>用户风险安全策略

有关详细信息，请参阅[如何配置用户风险策略](howto-user-risk-policy.md)。

![用户风险](./media/playbook/02.png "演练手册")

**若要测试用户风险安全策略，请执行以下步骤**：

1. 使用租户的全局管理员凭据登录到 [https://portal.azure.com](https://portal.azure.com)。
2. 导航到**标识保护**。 
3. 在“Azure AD 标识保护”页中，单击“用户风险策略”。
4. 在“分配”部分，选择所需的用户（和组）以及用户风险级别。

    ![用户风险](./media/playbook/03.png "演练手册")

5. 在“控制”部分，选择所需的访问控制（例如，需要更改密码）。
5. 对于“强制实施策略”，请选择“关闭”。
6. 可以通过特定的方式来提升测试帐户的用户风险，例如，可以多次模拟某个风险事件。
7. 等待几分钟，验证用户的用户级别是否为“中等”。 如果否，请为用户模拟更多的风险事件。
8. 对于“强制实施策略”，请选择“打开”。
9. 你现在可以通过使用具有提升的风险级别的用户登录, 来测试基于用户风险的条件性访问。

### <a name="sign-in-risk-security-policy"></a>登录风险安全策略

有关详细信息，请参阅[如何配置登录风险策略](howto-sign-in-risk-policy.md)。

![登录风险](./media/playbook/01.png "演练手册")

**若要测试登录风险策略，请执行以下步骤：**

1. 使用租户的全局管理员凭据登录到 [https://portal.azure.com](https://portal.azure.com)。
2. 导航到 **Azure AD 标识保护**。
3. 在“Azure AD 标识保护”主页中，单击“登录风险策略”。 
4. 在“分配”部分，选择所需的用户（和组）以及登录风险级别。

    ![登录风险](./media/playbook/04.png "演练手册")

5. 在“控制”部分，选择所需的访问控制（例如，“需要多重身份验证”）。 
6. 对于“强制实施策略”，请选择“打开”。
7. 单击“保存” 。
8. 你现在可以通过使用有风险的会话 (例如, 通过使用 Tor 浏览器) 登录来测试登录基于风险的条件性访问。 

## <a name="see-also"></a>请参阅

- [Azure Active Directory 标识保护](../active-directory-identityprotection.md)
