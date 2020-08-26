---
title: 从快照创建 VM (Linux) - PowerShell 示例
description: Azure PowerShell 脚本示例 - 从快照创建 VM
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: d6bf74b9040ec3f677c23c84f8ece22befb35d56
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87010285"
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell-linux"></a>使用 PowerShell 从快照创建虚拟机 (Linux)

此脚本从 OS 磁盘的快照创建虚拟机。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>示例脚本

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]

## <a name="clean-up-deployment"></a>清理部署

运行以下命令来删除资源组、VM 和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令获取快照属性、从快照创建托管磁盘并创建 VM。 表中的每一项均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [Get-AzSnapshot](/powershell/module/az.compute/get-azsnapshot) | 使用快照名称获取快照。 |
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | 创建磁盘配置。 在磁盘创建过程中将使用此配置。 |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | 创建托管磁盘。 |
| [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig) | 创建 VM 配置。 此配置包括 VM 名称、操作系统和管理凭据等信息。 在创建 VM 期间将使用此配置。 |
| [Set-AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk) | 将托管磁盘作为 OS 磁盘附加到虚拟机 |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | 创建公共 IP 地址。 |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | 创建网络接口。 |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | 创建虚拟机。 |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组及其中包含的所有资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](/powershell/azure/)。

可以在 [Azure Linux VM 文档](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 PowerShell 脚本示例。
