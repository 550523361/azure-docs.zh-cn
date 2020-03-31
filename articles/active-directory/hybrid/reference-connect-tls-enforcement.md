---
title: Azure AD Connect：强制 Azure Active Directory Connect 执行 TLS 1.2 | Microsoft Docs
description: 本文列出 Azure AD Connect 和 Azure AD Sync 的所有版本
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ff5c75785622b43e66b808009c4674d4b2f2b50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78300842"
---
# <a name="tls-12-enforcement-for-azure-ad-connect"></a>强制 Azure AD Connect 执行 TLS 1.2

传输层安全性 (TLS) 协议版本 1.2 是一种旨在提供安全通信的加密协议。  TLS 协议主要目的在于提供隐私和数据完整性。  TLS 经历了多次迭代，版本 1.2 在 [RFC 5246](https://tools.ietf.org/html/rfc5246) 中进行定义。  Azure Active Directory Connect 1.2.65.0 及更高版本现在完全支持仅使用 TLS 1.2 与 Azure 进行通信。  本文档将提供有关如何强制 Azure AD Connect 服务器仅使用 TLS 1.2 的信息。

## <a name="update-the-registry"></a>更新注册表
为强制 Azure AD Connect 服务器仅使用 TLS 1.2，必须更新 Windows 服务器的注册表。  在 Azure AD Connect 服务器上设置以下注册表项。

>[!IMPORTANT]
>更新注册表后，必须重启 Windows 服务器才能使更改生效。


### <a name="enable-tls-12"></a>启用 TLS 1.2
- [HKEY_LOCAL_MACHINE_软件_WOW6432Node_微软\\。NETFramework_v4.0.30319*
  - "系统默认版本"=dword：0000001
  - "舒斯强加密"=dword：000001
- [HKEY_LOCAL_MACHINE_软件\微软\\。NETFramework_v4.0.30319*
  - "系统默认版本"=dword：0000001
  - "SchUseStrongCrypto"=dword:00000001
- [HKEY_LOCAL_MACHINE_SYSTEM_当前控制集\控制\安全提供程序\SCHANNEL_协议\TLS 1.2\服务器]
  - "已启用"=dword：0000001
- [HKEY_LOCAL_MACHINE_SYSTEM_当前控制集\控制\安全提供程序\SCHANNEL_协议\TLS 1.2\服务器]
  - "禁用状态"=dword：0000000 
- [HKEY_LOCAL_MACHINE_SYSTEM_当前控制集\控制\安全提供程序\SCHANNEL_协议\TLS 1.2\客户端]
  - "已启用"=dword：0000001
- [HKEY_LOCAL_MACHINE_SYSTEM_当前控制集\控制\安全提供程序\SCHANNEL_协议\TLS 1.2\客户端]
  - "禁用状态"=dword：0000000

### <a name="powershell-script-to-enable-tls-12"></a>用于启用 TLS 1.2 的 PowerShell 脚本
可使用以下 PowerShell 脚本在 Azure AD Connect 服务器上启用 TLS 1.2。

```powershell
    New-Item 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '1' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 0 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '1' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 0 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been enabled.'
```

### <a name="disable-tls-12"></a>禁用 TLS 1.2
- [HKEY_LOCAL_MACHINE_软件_WOW6432Node_微软\\。NETFramework_v4.0.30319*
  - "系统默认版本"=dword：0000000
  - "舒斯强加密"=dword：000000
- [HKEY_LOCAL_MACHINE_软件\微软\\。NETFramework_v4.0.30319*
  - "系统默认版本"=dword：0000000
  - "舒斯强加密"=dword：0000000
- [HKEY_LOCAL_MACHINE_SYSTEM_当前控制集\控制\安全提供程序\SCHANNEL_协议\TLS 1.2\服务器]
  - "已启用"=dword：0000000
- [HKEY_LOCAL_MACHINE_SYSTEM_当前控制集\控制\安全提供程序\SCHANNEL_协议\TLS 1.2\服务器]
  - "禁用状态"=dword：0000001
- [HKEY_LOCAL_MACHINE_SYSTEM_当前控制集\控制\安全提供程序\SCHANNEL_协议\TLS 1.2\客户端]
  - "已启用"=dword：0000000
- [HKEY_LOCAL_MACHINE_SYSTEM_当前控制集\控制\安全提供程序\SCHANNEL_协议\TLS 1.2\客户端]
  - "禁用状态"=dword：0000001 

### <a name="powershell-script-to-disable-tls-12"></a>用于禁用 TLS 1.2 的 PowerShell 脚本
可使用以下 PowerShell 脚本在 Azure AD Connect 服务器上禁用 TLS 1.2。\

```powershell
    New-Item 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="next-steps"></a>后续步骤
* [将本地标识与 Azure 活动目录集成](whatis-hybrid-identity.md)
