---
title: 从 Windows VM 分离数据磁盘-Azure
description: 在 Azure 中从使用资源管理器部署模型的虚拟机分离磁盘。
author: cynthn
ms.service: virtual-machines-windows
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: article
ms.date: 01/08/2020
ms.author: cynthn
ms.openlocfilehash: c93bb5fd3e92c6a947fe997b58207b87b2717fd5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82082758"
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>如何从 Windows 虚拟机分离数据磁盘

不再需要附加到虚拟机的数据磁盘时，可以轻松地分离它。 这会从虚拟机中删除磁盘，但不会从存储中删除它。

> [!WARNING]
> 如果分离磁盘，它将不会自动删除。 如果订阅了高级存储，则将继续承担该磁盘的存储费用。 有关详细信息，请参阅[使用高级存储时的定价和计费方式](disks-types.md#billing)。

如果希望再次使用磁盘上的现有数据，可以将其重新附加到相同的虚拟机或另一个虚拟机。

 

## <a name="detach-a-data-disk-using-powershell"></a>使用 PowerShell 分离数据磁盘

可以使用 PowerShell 热** 删除数据磁盘，但在从 VM 中分离磁盘之前，请确保没有任何设备正在使用该磁盘。

在此示例中，我们从 **myResourceGroup** 资源组的 VM **myVM** 中删除名为 **myDisk** 的磁盘。 首先，使用 [Remove-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmdatadisk) cmdlet 删除磁盘。 然后，使用 [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) cmdlet 更新虚拟机的状态，完成数据磁盘删除过程。

```azurepowershell-interactive
$VirtualMachine = Get-AzVM `
   -ResourceGroupName "myResourceGroup" `
   -Name "myVM"
Remove-AzVMDataDisk `
   -VM $VirtualMachine `
   -Name "myDisk"
Update-AzVM `
   -ResourceGroupName "myResourceGroup" `
   -VM $VirtualMachine
```

磁盘保留在存储中，但不再附加到虚拟机。

## <a name="detach-a-data-disk-using-the-portal"></a>使用门户分离数据磁盘

可以*热*删除数据磁盘，但请确保在将磁盘从 VM 分离之前，没有任何活动正在使用该磁盘。

1. 在左侧菜单中，选择“虚拟机”****。
1. 选择包含要分离的数据磁盘的虚拟机。
1. 在“设置”下，选择“磁盘”********。
1. 在“磁盘”**** 窗格的顶部，选择“编辑”****。
1. 在 "**磁盘**" 窗格中，选择要分离的数据磁盘的最右侧，然后选择 "**分离**"。
1. 选择页面顶部的 "**保存**" 以保存所做的更改。

磁盘保留在存储中，但不再附加到虚拟机。

## <a name="next-steps"></a>后续步骤

如果想要重新使用数据磁盘，只需将它[附加到另一个 VM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
