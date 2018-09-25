---
title: 为 Azure Stack 用户启用 Azure CLI | Microsoft Docs
description: 了解如何使用跨平台命令行接口 (CLI) 管理和部署 Azure Stack 上的资源
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: mabrigg
ms.openlocfilehash: 09c551ea7196ae20a60a5dd34c1cda889ff5df46
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2018
ms.locfileid: "47091036"
---
# <a name="enable-azure-cli-for-azure-stack-users"></a>为 Azure Stack 用户启用 Azure CLI

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

可以向 Azure Stack 的用户提供 CA 根证书，使他们能够在其开发计算机上使用 Azure CLI。 用户需使用该证书通过 CLI 管理资源。

* 如果用户通过 Azure Stack 开发工具包外部的工作站使用 CLI，**Azure Stack CA 根证书**是必需的。  

* **虚拟机别名终结点**提供在部署 VM 时以单个参数形式引用映像发布者、产品/服务、SKU 和版本的别名，如“UbuntuLTS”或“Win2012Datacenter”。  

以下部分介绍如何获取这些值。

## <a name="export-the-azure-stack-ca-root-certificate"></a>导出 Azure Stack CA 根证书

可以在开发工具包以及在开发工具包环境内运行的租户虚拟机上找到 Azure Stack CA 根证书。 若要以 PEM 格式导出 Azure Stack 根证书，请登录到开发工具包或租户虚拟机并运行以下脚本：

```powershell
$label = "AzureStackSelfSignedRootCert"
Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
$root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
if (-not $root)
{
    Log-Error "Certificate with subject CN=$label not found"
    return
}

Write-Host "Exporting certificate"
Export-Certificate -Type CERT -FilePath root.cer -Cert $root

Write-Host "Converting certificate to PEM format"
certutil -encode root.cer root.pem
```

## <a name="set-up-the-virtual-machine-aliases-endpoint"></a>设置虚拟机别名终结点

Azure Stack 操作员应设置可公开访问的终结点来托管虚拟机别名文件。 虚拟机别名文件是一个 JSON 文件，提供映像的公用名称。 随后以 Azure CLI 参数的形式部署 VM 时，指定该名称。  

向别名文件添加条目之前，请确保[从 Azure 市场下载映像](azure-stack-download-azure-marketplace-item.md)，或者已[发布自己的自定义映像](azure-stack-add-vm-image.md)。 如果发布自定义映像，请记下发布过程中指定的发布者、产品/服务、SKU 和版本信息。 如果映像来自市场，可以使用 ```Get-AzureVMImage``` cmdlet 查看信息。  

可以使用包含许多常见映像别名的[示例别名文件](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json)。 可以使用该文件作为起点。 将该文件托管在 CLI 客户端可以访问它的空间。 一种方法是将该文件托管在 blob 存储帐户中并与用户共享 URL：

1. 从 GitHub 下载[示例文件](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json)。
2. 在 Azure Stack 中创建新的存储帐户。 完成该操作后，创建新的 Blob 容器。 将访问策略设置为“公开”。  
3. 将 JSON 文件上传到新容器。 完成该操作后，可以通过选择 Blob 名称，然后从 Blob 属性中选择 URL 来查看 Blob 的 URL。

## <a name="next-steps"></a>后续步骤

- [使用 Azure CLI 部署模板](azure-stack-deploy-template-command-line.md)

- [使用 PowerShell 连接](azure-stack-connect-powershell.md)

- [管理用户权限](azure-stack-manage-permissions.md)
