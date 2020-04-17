---
title: 在 Azure 资源管理器中为 Windows VM 设置密钥保管库
description: 如何设置与 Azure 资源管理器虚拟机搭配使用的密钥保管库。
services: virtual-machines-windows
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 01/24/2017
ms.author: mimckitt
ms.openlocfilehash: 098d4df3938f591cb72b3bfd59b7a5549469785c
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2020
ms.locfileid: "81451731"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>在 Azure 资源管理器中为虚拟机设置密钥保管库

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

在 Azure 资源管理器堆栈中，密码/证书被建模为密钥保管库资源提供程序所提供的资源。 若要了解有关密钥保管库的详细信息，请参阅[什么是 Azure 密钥保管库？](../../key-vault/general/overview.md)

> [!NOTE]
> 1. 为了让密钥保管库能与 Azure 资源管理器虚拟机搭配使用，必须将密钥保管库上的 **EnabledForDeployment** 属性设置为 true。 可以在各种客户端中执行此操作。
> 2. 需要在与虚拟机相同的订阅和位置中创建密钥保管库。
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>使用 PowerShell 设置密钥保管库
若要使用 PowerShell 创建密钥保管库，请参阅[使用 PowerShell 在 Azure 密钥保管库中设置和检索机密](../../key-vault/secrets/quick-create-powershell.md)。

对于新的密钥保管库，可以使用此 PowerShell cmdlet：

    New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

对于现有的密钥保管库，可以使用此 PowerShell cmdlet：

    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="use-cli-to-set-up-key-vault"></a>使用 CLI 设置密钥保管库
若要使用命令行接口 (CLI) 创建密钥保管库，请参阅[使用 CLI 管理密钥保管库](../../key-vault/general/manage-with-cli2.md#create-a-key-vault)。

使用 CLI 时，必须先创建密钥保管库，然后分配部署策略。 可以使用以下命令来执行此操作：

    az keyvault create --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --location "EastAsia"
    
然后，要启用密钥保管库以用于模板部署，请运行以下命令：

    az keyvault update --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --enabled-for-deployment "true"

## <a name="use-templates-to-set-up-key-vault"></a>使用模板设置密钥保管库
使用模板时，必须将密钥保管库资源的 `enabledForDeployment` 属性设置为 `true`。

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

有关使用模板创建密钥保管库时可以配置的其他选项，请参阅[创建密钥保管库](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)。
