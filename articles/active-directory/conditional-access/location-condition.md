---
title: Azure Active Directory 条件访问中的位置条件
description: 了解如何使用位置条件基于用户的网络位置来控制对云应用的访问。
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.workload: identity
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 915675af1e646f2cb77e36c0018ed372ff9496fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263226"
---
# <a name="what-is-the-location-condition-in-azure-active-directory-conditional-access"></a>Azure Active Directory 条件访问中的位置条件是什么？ 

使用[Azure 活动目录 （Azure AD） 条件访问](../active-directory-conditional-access-azure-portal.md)，可以控制授权用户可以如何访问云应用。 使用条件访问策略的位置条件可将访问控制设置绑定到用户的网络位置。

本文提供配置位置条件所需的信息。

## <a name="locations"></a>位置

Azure AD 允许从公共 Internet 上的任何位置单一登录到设备、应用和服务。 使用位置条件，可以基于用户的网络位置来控制对云应用的访问。 位置条件的常见用例如下：

- 要求用户在企业网络外部访问服务时执行多重身份验证。
- 阻止特定国家或地区的用户访问服务。

位置是网络位置的标签，表示命名位置或多重身份验证信任的 IP。

## <a name="named-locations"></a>命名位置

使用命名位置可以创建 IP 地址范围或者国家和地区的逻辑分组。

您可以在"条件访问 **"页的"管理**"部分中访问命名位置。

![条件访问中的命名位置](./media/location-condition/02.png)

命名位置包括以下组成部分：

![创建新命名位置](./media/location-condition/42.png)

- **名称** - 命名位置的显示名称。
- **IP 范围** - 采用 CIDR 格式的一个或多个 IPv4 地址范围。 不支持指定 IPv6 地址范围。

   > [!NOTE]
   > IPv6 地址范围目前无法包含在命名位置中。 这意味着无法从“条件访问”策略中排除 IPv6 范围。

- **标记为可信位置** - 可为命名位置设置标志，以指示它是可信的位置。 通常，可信位置是由 IT 部门控制的网络区域。 除了条件访问之外，Azure 标识保护和 Azure AD 安全报告还使用受信任的命名位置来减少[误报](../reports-monitoring/concept-risk-events.md#impossible-travel-to-atypical-locations-1)。
- **国家/地区** - 使用此选项可以选择一个或多个国家或地区，以定义命名位置。
- **包含未知区域** - 某些 IP 地址未映射到特定的国家或地区。 使用此选项可以选择这些 IP 地址是否应包含在命名位置中。 如果使用命名位置的策略需要应用到未知位置，则使用此设置。

可配置的已命名位置数受限于 Azure AD 中相关对象的大小。 可以根据以下限制来配置位置：

- 一个命名位置最多可以有 1200 个 IP 范围。
- 最多可有 90 个命名位置，其中每个都分配有一个 IP 范围。

条件访问策略适用于 IPv4 和 IPv6 流量。 目前，命名位置不允许配置 IPv6 范围。 此限制导致以下情况：

- 条件访问策略的目标不能是特定的 IPv6 范围
- 条件访问策略不能排除特定的 IPv6 范围

如果配置的策略适用于“任何位置”，则适用于 IPv4 和 IPv6 流量。 为指定国家和地区配置的命名位置仅支持 IPv4 地址。 只有在“包含未知区域”选项已选中的情况下，才会包括 IPv6 流量。

## <a name="trusted-ips"></a>受信任的 IP

还可以在[多重身份验证服务设置](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx)中配置 IP 地址范围，用于表示组织的本地 Intranet。 使用此功能最多可以配置 50 个 IP 地址范围。 IP 地址范围采用 CIDR 格式。 有关详细信息，请参阅受信任的[IP](../authentication/howto-mfa-mfasettings.md#trusted-ips)。  

如果配置了受信任的 IP，则它们在位置条件的位置列表中显示为**MFA 受信任的 IPS。**

### <a name="skipping-multi-factor-authentication"></a>跳过多重身份验证

在多重身份验证服务设置页中，可以通过选择“跳过多重身份验证以适用于我的 Intranet 上的联合用户发出的请求”，来标识企业 Intranet 用户。**** 此设置指示 AD FS 颁发的内部企业网络声明应受信任，并且应该用于将用户标识为位于企业网络中。 有关详细信息，请参阅[使用条件访问启用受信任的 IP 功能](../authentication/howto-mfa-mfasettings.md#enable-the-trusted-ips-feature-by-using-conditional-access)。

选中此选项之后，“MFA 受信任的 IP”（包括命名位置）将应用到已选择此选项的所有策略。****

对于会话生存期较长的移动和桌面应用程序，将定期重新评估条件访问。 默认设置是一小时评估一次。 如果只在初始身份验证时才颁发内部企业网络声明，则 Azure AD 可能没有受信任的 IP 范围列表。 在这种情况下，更难以确定用户是否仍在企业网络中：

1. 检查用户的 IP 地址是否在某个受信任的 IP 范围内。
2. 检查用户 IP 地址的前三个八位字节是否匹配初始身份验证 IP 地址的前三个八位字节。 当内部企业网络声明最初是初次颁发且用户位置已经过验证时，IP 地址将与初始身份验证进行比较。

如果这两个步骤均失败，则将用户视为不再位于受信任的 IP 中。

## <a name="location-condition-configuration"></a>位置条件配置

配置位置条件时，可以选择区分：

- 任何位置
- 所有受信任的位置
- 选定的位置

![位置条件配置](./media/location-condition/01.png)

### <a name="any-location"></a>任何位置

默认情况下，选择“任何位置”会将策略应用到所有 IP 地址，即 Internet 上的所有地址。**** 此设置不限于已配置为命名位置的 IP 地址。 选择“任何位置”时，仍可从策略中排除特定位置。**** 例如，可将策略应用到除可信位置以外的所有位置，以将作用域设置为除企业网络以外的所有位置。

### <a name="all-trusted-locations"></a>所有受信任的位置

此选项将应用到：

- 已标记为可信位置的所有位置
- **MFA 受信任的 IP**（如果已配置）

### <a name="selected-locations"></a>选定的位置

使用此选项可以选择一个或多个命名位置。 对于要应用此设置的策略，用户需要从任一选定位置建立连接。 单击“选择”时，将会打开显示命名网络列表的命名网络选择控件。**** 此列表还显示网络位置是否已标记为可信。 名为“MFA 受信任的 IP”的命名位置用于包含可在多重身份验证服务设置页中配置的 IP 设置。****

## <a name="what-you-should-know"></a>要点

### <a name="when-is-a-location-evaluated"></a>何时评估位置？

条件访问策略的评估时机：

- 当用户最初登录到 Web 应用、移动应用或桌面应用程序时。
- 当使用新式身份验证的移动应用或桌面应用程序使用刷新令牌来获取新的访问令牌时。 默认情况下此检查一小时进行一次。

对于使用新式身份验证的移动应用和桌面应用程序，此检查意味着，在更改网络位置的一小时内会检测到位置更改。 对于不使用新式身份验证的移动应用和桌面应用程序，此策略将应用于每个令牌请求。 请求的频率可能会因应用程序而异。 同样，对于 Web 应用程序，此策略在初始登录时应用，并适合用于 Web 应用程序的会话生存期。 由于不同应用程序的会话生存期不同，因此策略评估间隔的时间也会有所不同。 每次应用程序请求新的登录令牌时，就会应用一次此策略。

默认情况下，Azure AD 每小时颁发一个令牌。 在移出企业网络后的一小时内，将使用新式身份验证对应用程序实施该策略。

### <a name="user-ip-address"></a>用户 IP 地址

在策略评估中使用的 IP 地址是用户的公共 IP 地址。 对于专用网络中的设备，此 IP 地址不是 Intranet 中用户设备的客户端 IP，而是专用网络连接到公共 Internet 时使用的地址。

> [!WARNING]
> 如果设备只有一个 IPv6 地址，则不支持配置位置条件。

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>批量上传和下载命名位置

创建或更新命名位置时，若要进行批量更新，可以上传或下载含 IP 范围的 CSV 文件。 上传过程会将列表中的 IP 范围替换为该文件中的 IP 范围。 该文件的每行包含一个采用 CIDR 格式的 IP 地址范围。

### <a name="cloud-proxies-and-vpns"></a>云代理和 VPN

使用云托管代理或 VPN 解决方案时，Azure AD 在评估策略时使用的 IP 地址是该代理的 IP 地址。 不会使用包含用户公共 IP 地址的 X-Forwarded-For (XFF) 标头，因为没有任何机制验证该地址是否来自受信任的源，该标头可能提供了一种用于伪造 IP 地址的方法。

当云代理准备就绪时，可以使用一个策略来要求设备加入域，或使用来自 AD FS 的内部企业网络声明。

### <a name="api-support-and-powershell"></a>API 支持和 PowerShell

API 和 PowerShell 尚不支持命名位置或条件访问策略。

## <a name="next-steps"></a>后续步骤

- 若要了解如何配置条件访问策略，请参阅[通过 Azure Active Directory 条件访问要求特定应用必须使用 MFA](app-based-mfa.md)。
- 如果已准备好为环境配置条件访问策略，请参阅[Azure 活动目录中的条件访问最佳做法](best-practices.md)。
