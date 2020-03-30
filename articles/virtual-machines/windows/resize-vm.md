---
title: 调整 Azure 中的 Windows VM 的大小
description: 更改用于 Azure 虚拟机的 VM 大小。
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 01/13/2020
ms.author: cynthn
ms.openlocfilehash: 6718804d4635edb2628b53017ab9d377928afad8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75941721"
---
# <a name="resize-a-windows-vm"></a>调整 Windows VM 大小

本文介绍如何将 VM 移动到不同的 VM[大小](sizes.md)。

完成创建虚拟机 (VM) 后，可以通过更改 VM 大小来扩展或缩减 VM。 在某些情况下，必须先解除分配 VM。 如果新大小在当前托管 VM 的硬件群集上不可用，则可能会出现这种情况。

如果虚拟机使用高级存储，请确保选择 **s** 版本的大小以获得高级存储支持。 例如，选择 Standard_E4**s**_v3，而不是 Standard_E4_v3。

## <a name="use-the-portal"></a>使用门户

1. 打开[Azure 门户](https://portal.azure.com)。
1. 打开虚拟机的页面。
1. 在左侧菜单中，选择 **"大小**"。
1. 从可用大小列表中选择新大小，然后选择 **"调整大小**"。


如果虚拟机当前正在运行，更改其大小将导致重新启动它。 停止虚拟机可能会显示其他大小。

## <a name="use-powershell-to-resize-a-vm-not-in-an-availability-set"></a>使用 PowerShell 调整不在可用性集中的 VM 的大小

设置一些变量。 将值替换为自己的信息。

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

列出托管 VM 的硬件群集上可用的 VM 大小。 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

如果列出了所需大小，运行以下命令即可重设虚拟机大小。 如果未列出所需大小，请转到步骤 3。
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

如果未列出所需大小，运行以下命令即可解除分配虚拟机、重设其大小，并重新启动虚拟机。 将**\<新的Vmsize>** 替换为所需的大小。
   
```powershell
Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
```

> [!WARNING]
> 解除分配 VM 会释放分配给该 VM 的所有动态 IP 地址。 OS 和数据磁盘不受影响。 
> 
> 

## <a name="use-powershell-to-resize-a-vm-in-an-availability-set"></a>使用 PowerShell 调整可用性集中的 VM 大小

如果可用性集中 VM 的新大小在当前托管 VM 的硬件群集上不可用，则需要解除分配可用性集中的所有 VM 来调整 VM 大小。 完成调整一个 VM 的大小后，可能还需要更新可用性集中其他 VM 的大小。 若要调整可用性集中 VM 的大小，请执行以下步骤。

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

列出托管 VM 的硬件群集上可用的 VM 大小。 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

如果列出了所需大小，运行以下命令即可调整 VM 大小。 如果未列出所需大小，请转到下一部分。
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName 
$vm.HardwareProfile.VmSize = "<newVmSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```
    
如果未列出所需大小，继续执行以下步骤即可解除分配可用性集中的所有 VM、重设 VM 大小，并重新启动它们。

停止可用性集中的所有 VM。
   
```powershell
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 
```

调整可用性集中 VM 的大小并重新启动。
   
```powershell
$newSize = "<newVmSize>"
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
  foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    $vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    $vm.HardwareProfile.VmSize = $newSize
    Update-AzVM -ResourceGroupName $resourceGroup -VM $vm
    Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    }
```

## <a name="next-steps"></a>后续步骤

对于其他可伸缩性，运行多个 VM 实例并横向扩展。有关详细信息，请参阅[在虚拟机缩放集中自动缩放 Windows 计算机](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)。

