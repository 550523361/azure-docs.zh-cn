---
title: Microsoft Threat Modeling Tool 版本4/9/2019
titleSuffix: Azure
description: 阐述 Threat Modeling Tool 的发行说明
author: jegeib
ms.author: jegeib
ms.service: security
ms.subservice: security-develop
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: 59d385ba7de5bf7bceae4dc8ddadbca813046094
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "78269717"
---
# <a name="threat-modeling-tool-update-release-71604081---492019"></a>Threat Modeling Tool 更新版本 7.1.60408.1-4/9/2019

Microsoft Threat Modeling Tool （TMT）的版本7.1.60408.1 于 9 2019 年4月发布，其中包含以下更改：

- Azure Key Vault 和 Azure 流量管理器的新模具
- TMT 版本号现在显示在主屏幕上
- 支持链接已更新
- Bug 修复

## <a name="feature-changes"></a>功能更改

### <a name="new-stencils-for-azure-key-vault-and-azure-traffic-manager"></a>Azure Key Vault 和 Azure 流量管理器的新模具

![Azure Key Vault 模具](./media/threat-modeling-tool-releases-71604081/tmt_keyvault_trafficmanager.PNG)

已将 Azure Key Vault 和 Azure 流量管理器的新模具和威胁添加到了 Azure 模具集。 打开基于 Azure 模具集的模型时，系统将提示用户更新与模型关联的模板。 还可以使用 "文件" 菜单中的 "应用模板" 命令手动启动基于 Azure 模具集的模型，并重新应用最新的 Azure 云 tb7 文件。

### <a name="tmt-version-number-is-now-shown-on-the-home-screen"></a>TMT 版本号现在显示在主屏幕上

Threat Modeling Tool 的客户端版本现在显示在应用程序的主屏幕上，以方便访问。

![Azure Key Vault 模具](./media/threat-modeling-tool-releases-71604081/tmt_version.PNG)

### <a name="support-links-have-been-updated"></a>支持链接已更新

已更新工具中的所有支持链接，以将用户定向[tmtextsupport@microsoft.com](mailto:tmtextsupport@microsoft.com)到，而不是 MSDN 论坛。

## <a name="system-requirements"></a>系统要求

- 支持的操作系统
  - [Microsoft Windows 10 周年更新](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97)或更高版本
- 所需的 .NET 版本
  - [.Net 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262)或更高版本
- 其他要求
  - 需要建立 Internet 连接才能接收工具和模板的更新。

## <a name="documentation-and-feedback"></a>文档和反馈

- Threat Modeling Tool 的文档位于 [docs.microsoft.com](threat-modeling-tool.md)，其中包含[有关如何使用该工具](threat-modeling-tool-getting-started.md)的信息。

## <a name="next-steps"></a>后续步骤

下载最新版本的 [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool)。
