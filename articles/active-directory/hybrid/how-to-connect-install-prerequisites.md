---
title: Azure AD Connect：先决条件和硬件 | Microsoft Docs
description: 本主题介绍 Azure AD Connect 的先决条件和硬件要求
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 91b88fda-bca6-49a8-898f-8d906a661f07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 06/25/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 11d5c1bb133f0aea241fbc55f96ab5f8818e5ed6
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86518107"
---
# <a name="prerequisites-for-azure-ad-connect"></a>Azure AD Connect 的先决条件
本主题介绍 Azure AD Connect 的先决条件和硬件要求。

## <a name="before-you-install-azure-ad-connect"></a>安装 Azure AD Connect 之前
在安装 Azure AD Connect 之前，需要准备好以下项目。

### <a name="azure-ad"></a>Azure AD
* Azure AD 租户。 通过 [Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)获得一个租户。 可以使用以下门户之一来管理 Azure AD Connect：
  * [Azure 门户](https://portal.azure.com)。
  * [Office 门户](https://portal.office.com)。  
* [添加并验证域](../active-directory-domains-add-azure-portal.md)，该域是计划在 Azure AD 中使用的。 例如，如果计划让用户使用 contoso.com，请确保此域已经过验证，并且不是直接使用 contoso.onmicrosoft.com 默认域。
* 默认情况下，一个 Azure AD 租户允许 5 万个对象。 在验证域后，该限制增加到 30 万个对象。 如果在 Azure AD 中需要更多的对象，则需要开具支持案例来请求增大此限制。 如果需要50万个个以上的对象，则需要使用 Office 365、Azure AD Premium 或企业移动性和安全性等许可证。

### <a name="prepare-your-on-premises-data"></a>准备本地数据
* 使用 [IdFix](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) 确定目录中的错误，如重复项和格式设置问题，然后同步到 Azure AD 和 Office 365。
* 查看[可以在 Azure AD 中启用的可选同步功能](how-to-connect-syncservice-features.md)并评估应启用哪些功能。

### <a name="on-premises-active-directory"></a>本地 Active Directory
* AD 架构版本与林功能级别必须是 Windows Server 2003 或更高版本。 只要符合架构和林级别的要求，域控制器就能运行任何版本。
* 若打算使用**密码写回**功能，必须在 Windows Server 2008 R2 或更高版本上安装域控制器。
* Azure AD 使用的域控制器必须可写。 **不支持**使用 RODC（只读域控制器），并且 Azure AD Connect 不会遵循任何写重定向。
* **不支持**通过“以点分隔的”（名称包含句点“.”）NetBios 名称使用本地林/域。
* 建议[启用 Active Directory 回收站](how-to-connect-sync-recycle-bin.md)。

### <a name="azure-ad-connect-server"></a>Azure AD Connect 服务器
Azure AD Connect 服务器包含关键的标识数据。 确保对此服务器的管理访问权限受到适当保护，请遵循[保护特权访问](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access)中所述的指导原则。 

必须将 Azure AD Connect 服务器视为第0层组件，如[Active Directory 管理层模型](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)中所述。  

若要详细了解如何保护 Active Directory 环境，请参阅[保护 Active Directory 的最佳实践](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/best-practices-for-securing-active-directory)。

#### <a name="installation-prerequisites"></a>安装的先决条件 

- Azure AD Connect 必须安装在已加入域的 Windows Server 2012 或更高版本上。 
- 不能在 Small Business Server 或 2019 版以前的 Windows Server Essentials（支持 Windows Server Essentials 2019）上安装 Azure AD Connect。 该服务器必须使用 Windows Server Standard 或更高版本。  
- 必须在 Azure AD Connect 服务器上安装完整的 GUI。 不支持在 Windows Server Core 上安装 Azure AD Connect。 
- 如果使用 Azure AD Connect 向导来管理 ADFS 配置，则 Azure AD Connect 服务器不得启用“PowerShell 转换”组策略。 如果使用 Azure AD Connect 向导来管理同步配置，则可以启用 PowerShell 脚本。 
- 如果正在部署 Active Directory 联合身份验证服务，则： 
    - 安装 AD FS 或 Web 应用程序代理的服务器必须是 Windows Server 2012 R2 或更高版本。 必须在这些服务器上启用 Windows 远程管理才能进行远程安装。 
    - 必须配置 TLS/SSL 证书。  请参阅[管理 ssl/TLS 协议和密码套件](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-protocols-in-ad-fs)，以便[在 AD FS 中 AD FS 和管理 ssl 证书](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap)。
    - 必须配置名称解析。 
- 如果全局管理员已启用 MFA，则 URL https://secure.aadcdn.microsoftonline-p.com **必须**位于 "受信任的站点" 列表中。 在显示 MFA 质询提示之前，系统会先提示将此 URL 添加到受信任的站点列表中（如果尚未添加）。 可以使用 Internet Explorer 将它添加到受信任站点。  

#### <a name="hardening-your-azure-ad-connect-server"></a>强化 Azure AD Connect 服务器 
Microsoft 建议强化 Azure AD Connect 服务器，以降低这一 IT 环境关键组件的安全攻击面。 遵循以下建议将有助于缓解组织面临的某些安全风险。

- 您必须将 Azure AD Connect 视为与域控制器和其他第0层资源相同：https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material 
- 应将 Azure AD Connect 服务器的管理访问权限仅限于域管理员或其他严格控制的安全组。
- 应[为具有特权访问权限的所有人员创建专用帐户](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access)。 管理员不应使用特权高的帐户浏览 Web、检查电子邮件和执行日常生产力任务。
- 应按照[保护特权访问](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/how-to-configure-protected-accounts)中的说明进行操作。 
- 应确保每台计算机都具有唯一的本地管理员密码。 [本地管理员密码解决方案（LAPS）](https://support.microsoft.com/help/3062591/microsoft-security-advisory-local-administrator-password-solution-laps)可在每个工作站上配置唯一的随机密码，并将其存储在受 ACL 保护 ACTIVE DIRECTORY （AD）中。 只有符合条件的授权用户才可以读取或请求重置这些本地管理员帐户密码。 你可以从[Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=46899#:~:text=The%20%22Local%20Administrator%20Password%20Solution,it%20or%20request%20its%20reset.)获取用于工作站和服务器的 LAPS。 有关使用 LAPS 和 Paw 运行环境的其他指南，请参阅[基于清洁源原则的操作标准](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material#operational-standards-based-on-clean-source-principle)。 
- 应为具有组织信息系统的特许访问权限的所有人员实现专用的[特权访问工作站（PAW）](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations) 。 
- 你应遵循这些[附加指导原则](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface)来减小 Active Directory 环境的攻击面。


### <a name="sql-server-used-by-azure-ad-connect"></a>Azure AD Connect 所使用的 SQL Server
* Azure AD Connect 要求使用 SQL Server 数据库来存储标识数据。 默认安装 SQL Server 2012 Express LocalDB（轻量版本的 SQL Server Express）。 SQL Server Express 有 10GB 的大小限制，允许管理大约 100,000 个对象。 如果需要管理更多的目录对象，则需要将安装向导指向不同的 SQL Server 安装。 SQL Server 安装的类型可能会影响 [Azure AD Connect 的性能](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-performance-factors#sql-database-factors)。
* 如果使用不同的 SQL Server 安装，则以下要求适用：
  * Azure AD Connect 支持从 2012（包含最新的 Service Pack）到 SQL Server 2019 的所有 Microsoft SQL Server 版本。 **不支持**将 Microsoft Azure SQL 数据库用作数据库。
  * 必须使用不区分大小写的 SQL 排序规则。 可通过名称中的 \_CI_ 识别这些排序规则。 **不支持**使用区分大小写的排序规则，该规则可通过其名称中的 \_CS_ 识别。
  * 每个 SQL 实例只能有一个同步引擎。 **不支持** 与 FIM/MIM Sync、DirSync 或 Azure AD Sync 共享 SQL 实例。

### <a name="accounts"></a>帐户
* 要集成的 Azure AD 租户的 Azure AD 全局管理员帐户。 该帐户必须是**学校或组织帐户**，而不能是 **Microsoft 帐户**。
* 如果使用[快速设置](reference-connect-accounts-permissions.md#express-settings-installation)或者从 DirSync 升级，则必须拥有本地 Active Directory 的企业管理员帐户。
* 如果你使用自定义设置安装路径，则会有更多选项。 有关详细信息，请参阅[自定义安装设置](reference-connect-accounts-permissions.md#custom-installation-settings)。

### <a name="connectivity"></a>连接
* Azure AD Connect 服务器需要 Intranet 和 Internet 的 DNS 解析。 DNS 服务器必须能够将名称解析成本地 Active Directory 以及 Azure AD 终结点。
* 如果 Intranet 有防火墙，且需要开放 Azure AD Connect 服务器与域控制器之间的端口，请参阅 [Azure AD Connect 端口](reference-connect-ports.md)，了解详细信息。
* 如果代理或防火墙限制了可访问的 URL，必须打开 [Office 365 URL 和 IP 地址范围](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) 中所述的 URL。
  * 如果在德国使用 Microsoft 云或 Microsoft Azure 政府版云，请参阅 [Azure AD Connect 同步服务实例注意事项](reference-connect-instances.md)以了解 URL。
* Azure AD Connect（1.1.614.0 版及更高版本）默认情况下使用 TLS 1.2 对同步引擎和 Azure AD 之间的通信进行加密。 如果 TLS 1.2 在基础操作系统上不可用，Azure AD Connect 会递增地回退到较旧的协议（TLS 1.1 和 TLS 1.0）。
* 在 1.1.614.0 版以前，Azure AD Connect 默认情况下使用 TLS 1.0 对同步引擎和 Azure AD 之间的通信进行加密。 若要更改为 TLS 1.2，请按照[为 Azure AD connect 启用 TLS 1.2](#enable-tls-12-for-azure-ad-connect) 中的步骤进行操作。
* 如果使用出站代理连接到 Internet，则必须在 **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** 文件中添加以下设置，才能将安装向导和 Azure AD Connect 同步连接到 Internet 和 Azure AD。 必须在文件底部输入此文本。 在此代码中，&lt;PROXYADDRESS&gt; 代表实际代理 IP 地址或主机名。

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* 如果代理服务器要求身份验证，则[服务帐户](reference-connect-accounts-permissions.md#adsync-service-account)必须位于域中，必须使用自定义的设置安装路径来指定[自定义服务帐户](how-to-connect-install-custom.md#install-required-components)。 还需要对 machine.config 进行不同的更改。在 machine.config 中进行此更改之后，安装向导和同步引擎响应来自代理服务器的身份验证请求。 在所有安装向导页中（“配置”页除外）都使用已登录用户的凭据。  在安装向导末尾的“配置”页上，上下文将切换到已创建的[服务帐户](reference-connect-accounts-permissions.md#adsync-service-account)。  machine.config 节应如下所示。

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* 当 Azure AD Connect 在目录同步过程中将 Web 请求发送到 Azure AD 时，Azure AD 可能需要最多 5 分钟才能响应。 代理服务器具有连接空闲超时配置很常见。 请确保配置设置为至少 6 分钟或更长时间。

有关[默认代理元素](https://msdn.microsoft.com/library/kd3cf2ex.aspx)的详细信息，请参阅 MSDN。  
有关遇到连接问题时的详细信息，请参阅[排查连接问题](tshoot-connect-connectivity.md)。

### <a name="other"></a>其他
* 可选：一个用于验证同步的测试用户帐户。

## <a name="component-prerequisites"></a>组件先决条件
### <a name="powershell-and-net-framework"></a>PowerShell 和 .NET Framework
Azure AD Connect 依赖于 Microsoft PowerShell 和 .NET Framework 4.5.1。 服务器上需要安装此版本或更高版本。 请根据 Windows Server 版本执行以下操作：

* Windows Server 2012R2
  * 已按默认安装 Microsoft PowerShell， 因此不需要执行任何操作。
  * .NET Framework 4.5.1 和更高版本通过 Windows 更新提供。 请确保已在控制面板中安装 Windows Server 的最新更新。
* Windows Server 2012
  * 可从 **Microsoft 下载中心**获取的 [Windows Management Framework 4.0](https://www.microsoft.com/downloads)中获得最新的 Microsoft PowerShell 版本。
  * .NET Framework 4.5.1 和更高版本可从 [Microsoft 下载中心](https://www.microsoft.com/downloads)获取。


### <a name="enable-tls-12-for-azure-ad-connect"></a>为 Azure AD connect 启用 TLS 1.2
在 1.1.614.0 版以前，Azure AD Connect 默认情况下使用 TLS 1.0 对同步引擎服务器和 Azure AD 之间的通信进行加密。 可以通过配置 .NET 应用程序在服务器上默认使用 TLS 1.2 来更改此项。 有关 TLS 1.2 的详细信息，请参阅 [Microsoft 安全通报 2960358](https://technet.microsoft.com/security/advisory/2960358)。

1.  请确保已为操作系统安装了 .NET 4.5.1 修补程序，请参阅 [Microsoft 安全通报 2960358](https://technet.microsoft.com/security/advisory/2960358)。 服务器上可能已经安装了此修补程序或更高版本的修补程序。
    ```
2. For all operating systems, set this registry key and restart the server.
    ```
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 "SchUseStrongCrypto"=dword:00000001
    ```
4. If you also want to enable TLS 1.2 between the sync engine server and a remote SQL Server, then make sure you have the required versions installed for [TLS 1.2 support for Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## Prerequisites for federation installation and configuration
### Windows Remote Management
When using Azure AD Connect to deploy Active Directory Federation Services or the Web Application Proxy, check these requirements:

* If the target server is domain joined, then ensure that Windows Remote Managed is enabled
  * In an elevated PowerShell command window, use command `Enable-PSRemoting –force`
* If the target server is a non-domain joined WAP machine, then there are a couple of additional requirements
  * On the target machine (WAP machine):
    * Ensure the winrm (Windows Remote Management / WS-Management) service is running via the Services snap-in
    * In an elevated PowerShell command window, use command `Enable-PSRemoting –force`
  * On the machine on which the wizard is running (if the target machine is non-domain joined or untrusted domain):
    * In an elevated PowerShell command window, use the command `Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
    * In Server Manager:
      * add DMZ WAP host to machine pool (server manager -> Manage -> Add Servers...use DNS tab)
      * Server Manager All Servers tab: right click WAP server and choose Manage As..., enter local (not domain) creds for the WAP machine
      * To validate remote PowerShell connectivity, in the Server Manager All Servers tab: right click WAP server and choose Windows PowerShell. A remote PowerShell session should open to ensure remote PowerShell sessions can be established.

### TLS/SSL Certificate Requirements
* It's strongly recommended to use the same TLS/SSL certificate across all nodes of your AD FS farm and all Web Application proxy servers.
* The certificate must be an X509 certificate.
* You can use a self-signed certificate on federation servers in a test lab environment. However, for a production environment, we recommend that you obtain the certificate from a public CA.
  * If using a certificate that is not publicly trusted, ensure that the certificate installed on each Web Application Proxy server is trusted on both the local server and on all federation servers
* The identity of the certificate must match the federation service name (for example, sts.contoso.com).
  * The identity is either a subject alternative name (SAN) extension of type dNSName or, if there are no SAN entries, the subject name specified as a common name.  
  * Multiple SAN entries can be present in the certificate, provided one of them matches the federation service name.
  * If you are planning to use Workplace Join, an additional SAN is required with the value **enterpriseregistration.** followed by the User Principal Name (UPN) suffix of your organization, for example, **enterpriseregistration.contoso.com**.
* Certificates based on CryptoAPI next generation (CNG) keys and key storage providers are not supported. This means you must use a certificate based on a CSP (cryptographic service provider) and not a KSP (key storage provider).
* Wild-card certificates are supported.

### Name resolution for federation servers
* Set up DNS records for the AD FS federation service name (for example sts.contoso.com) for both the intranet (your internal DNS server) and the extranet (public DNS through your domain registrar). For the intranet DNS record, ensure that you use A records and not CNAME records. This is required for windows authentication to work correctly from your domain joined machine.
* If you are deploying more than one AD FS server or Web Application Proxy server, then ensure that you have configured your load balancer and that the DNS records for the AD FS federation service name (for example sts.contoso.com) point to the load balancer.
* For windows integrated authentication to work for browser applications using Internet Explorer in your intranet, ensure that the AD FS federation service name (for example sts.contoso.com) is added to the intranet zone in IE. This can be controlled via group policy and deployed to all your domain joined computers.

## Azure AD Connect supporting components
The following is a list of components that Azure AD Connect installs on the server where Azure AD Connect is installed. This list is for a basic Express installation. If you choose to use a different SQL Server on the Install synchronization services page, then SQL Express LocalDB is not installed locally.

* Azure AD Connect Health
* Microsoft SQL Server 2012 Command Line Utilities
* Microsoft SQL Server 2012 Express LocalDB
* Microsoft SQL Server 2012 Native Client
* Microsoft Visual C++ 2013 Redistribution Package

## Hardware requirements for Azure AD Connect
The table below shows the minimum requirements for the Azure AD Connect sync computer.

| Number of objects in Active Directory | CPU | Memory | Hard drive size |
| --- | --- | --- | --- |
| Fewer than 10,000 |1.6 GHz |4 GB |70 GB |
| 10,000–50,000 |1.6 GHz |4 GB |70 GB |
| 50,000–100,000 |1.6 GHz |16 GB |100 GB |
| For 100,000 or more objects the full version of SQL Server is required | | | |
| 100,000–300,000 |1.6 GHz |32 GB |300 GB |
| 300,000–600,000 |1.6 GHz |32 GB |450 GB |
| More than 600,000 |1.6 GHz |32 GB |500 GB |

The minimum requirements for computers running AD FS or Web Application Proxy Servers is the following:

* CPU: Dual core 1.6 GHz or higher
* MEMORY: 2 GB or higher
* Azure VM: A2 configuration or higher

## Next steps
Learn more about [Integrating your on-premises identities with Azure Active Directory](whatis-hybrid-identity.md).
