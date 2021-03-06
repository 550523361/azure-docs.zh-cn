---
title: 在 Azure 中扩展 Windows VM 的 OS 驱动器
description: 在资源管理器部署模型中，使用 Azure PowerShell 增加虚拟机的 OS 驱动器大小。
services: virtual-machines-windows
documentationcenter: ''
author: kirpasingh
manager: roshar
editor: ''
tags: azure-resource-manager
ms.assetid: d9edfd9f-482f-4c0b-956c-0d2c2c30026c
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/02/2020
ms.author: kirpas
ms.subservice: disks
ms.openlocfilehash: df27d7b25010fa68fc86ffe093318b2b0b7f4e96
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93393823"
---
# <a name="how-to-expand-the-os-drive-of-a-virtual-machine"></a>如何扩展虚拟机的 OS 驱动器

在资源组中通过从 [Azure 市场](https://azure.microsoft.com/marketplace/)部署映像来创建新的虚拟机 (VM) 时，默认的 OS 驱动器通常为 127 GB（默认情况下，某些映像的 OS 磁盘大小更小）。 尽管可以将数据磁盘添加到 VM（数量取决于所选择的 SKU），并且我们建议将应用程序和需要大量 CPU 的工作负荷安装在这些附加的磁盘上，但通常情况下，客户需要扩展 OS 驱动器以支持特定方案：

- 支持将组件安装在 OS 驱动器上的传统应用程序。
- 从本地迁移具有较大 OS 驱动器的物理电脑或 VM。

> [!IMPORTANT]
> 若要重设 Azure 虚拟机的 OS 磁盘或数据磁盘的大小，需要将虚拟机解除分配。
>
> 压缩现有磁盘不受支持，可能导致数据丢失。
> 
> 扩展磁盘后，需要[扩展 OS 中的卷](#expand-the-volume-within-the-os)才能使用更大的磁盘。

## <a name="resize-a-managed-disk-in-the-azure-portal"></a>重设 Azure 门户中的托管磁盘的大小

1. 在 [Azure 门户](https://portal.azure.com)中，请前往要在其中扩展磁盘的虚拟机。 选择 " **停止** " 以解除分配 VM。
2. VM 停止后，请在 " **设置** " 下的左侧菜单中选择 " **磁盘** "。

    :::image type="content" source="./media/expand-os-disk/select-disks.png" alt-text="屏幕截图，显示在菜单的 &quot;设置&quot; 部分中选择的 &quot;磁盘&quot; 选项。":::

 
3. 在 " **磁盘名称** " 下，选择要调整大小的磁盘。

    :::image type="content" source="./media/expand-os-disk/disk-name.png" alt-text="屏幕截图，显示选中了磁盘名称的 &quot;磁盘&quot; 窗格。":::

4. 在左侧菜单中的 " **设置** " 下，选择 " **配置** "。

    :::image type="content" source="./media/expand-os-disk/configuration.png" alt-text="屏幕截图，显示在菜单的 &quot;设置&quot; 部分中选择的配置选项。":::

5. 在 " **大小 (GiB")** 中，选择所需的磁盘大小。
   
   > [!WARNING]
   > 新大小应该大于现有磁盘大小。 对于 OS 磁盘来说，允许的最大值为 2,048 GB。  (可以将 VHD blob 扩展到该大小以上，但 OS 仅适用于前 2048 GB 空间。 ) 
   > 

    :::image type="content" source="./media/expand-os-disk/size.png" alt-text="显示所选磁盘大小的 &quot;配置&quot; 窗格的屏幕截图。":::

6. 选择“保存”。

    :::image type="content" source="./media/expand-os-disk/save.png" alt-text="屏幕截图，显示已选中 &quot;保存&quot; 按钮的配置窗格。":::


## <a name="resize-a-managed-disk-by-using-powershell"></a>使用 PowerShell 调整托管磁盘的大小

在管理模式下打开 PowerShell ISE 或 PowerShell 窗口，执行以下步骤：

1. 在资源管理模式下登录到 Microsoft Azure 帐户，并选择订阅：
   
    ```powershell
    Connect-AzAccount
    Select-AzSubscription –SubscriptionName 'my-subscription-name'
    ```

2. 设置资源组名称和 VM 名称：
   
    ```powershell
    $rgName = 'my-resource-group-name'
    $vmName = 'my-vm-name'
    ```

3. 获取对 VM 的引用：
   
    ```powershell
    $vm = Get-AzVM -ResourceGroupName $rgName -Name $vmName
    ```

4. 在重设磁盘大小之前停止 VM：
   
    ```powershell
    Stop-AzVM -ResourceGroupName $rgName -Name $vmName
    ```

5. 获取对托管 OS 磁盘的引用。 将托管 OS 磁盘的大小设置为所需值，并更新磁盘：
   
    ```powershell
    $disk= Get-AzDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
    $disk.DiskSizeGB = 1023
    Update-AzDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
    ```   
    > [!WARNING]
    > 新大小应该大于现有磁盘大小。 对于 OS 磁盘来说，允许的最大值为 2,048 GB。 （可以扩展 VHD Blob，使之超出该大小，但 OS 只能使用空间的头 2,048 GB。）
    > 
         
6. 更新 VM 可能需要几秒钟时间。 命令执行完后，请重启 VM：
   
    ```powershell
    Start-AzVM -ResourceGroupName $rgName -Name $vmName
    ```

这就是所有的操作！ 现在，请通过 RDP 访问 VM，打开“计算机管理”（或“磁盘管理”），然后使用刚刚分配的空间扩展驱动器。

## <a name="resize-an-unmanaged-disk-by-using-powershell"></a>使用 PowerShell 重设非托管磁盘的大小

在管理模式下打开 PowerShell ISE 或 PowerShell 窗口，执行以下步骤：

1. 在资源管理模式下登录到 Microsoft Azure 帐户，并选择订阅：
   
    ```powershell
    Connect-AzAccount
    Select-AzSubscription –SubscriptionName 'my-subscription-name'
    ```

2. 设置资源组名称和 VM 名称：
   
    ```powershell
    $rgName = 'my-resource-group-name'
    $vmName = 'my-vm-name'
    ```

3. 获取对 VM 的引用：
   
    ```powershell
    $vm = Get-AzVM -ResourceGroupName $rgName -Name $vmName
    ```

4. 在重设磁盘大小之前停止 VM：
   
    ```powershell
    Stop-AzVM -ResourceGroupName $rgName -Name $vmName
    ```

5. 将非托管 OS 磁盘的大小设置为所需值，并更新 VM：
   
    ```powershell
    $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
    Update-AzVM -ResourceGroupName $rgName -VM $vm
    ```
   
    > [!WARNING]
    > 新大小应该大于现有磁盘大小。 对于 OS 磁盘来说，允许的最大值为 2,048 GB。 （可以扩展 VHD Blob，使之超出该大小，但 OS 只能使用空间的头 2,048 GB。）
    > 
    > 
   
6. 更新 VM 可能需要几秒钟时间。 命令执行完后，请重启 VM：
   
    ```powershell
    Start-AzVM -ResourceGroupName $rgName -Name $vmName
    ```


## <a name="scripts-for-os-disk"></a>OS 磁盘的脚本

以下是同时适用于托管磁盘和非托管磁盘的引用的完整脚本：


**托管磁盘**

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzVM -ResourceGroupName $rgName -Name $vmName
Stop-AzVM -ResourceGroupName $rgName -Name $vmName
$disk= Get-AzDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
$disk.DiskSizeGB = 1023
Update-AzDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
Start-AzVM -ResourceGroupName $rgName -Name $vmName
```

**非托管磁盘**

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzVM -ResourceGroupName $rgName -Name $vmName
Stop-AzVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzVM -ResourceGroupName $rgName -VM $vm
Start-AzVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="resizing-data-disks"></a>调整数据磁盘的大小

虽然本文重介绍扩展 VM 的 OS 磁盘，但该脚本也可用于扩展附加到 VM 的数据磁盘。 例如，若要扩展附加到 VM 的第一个数据磁盘，请将 `StorageProfile` 的 `OSDisk` 对象替换为 `DataDisks` 数组，并使用数字索引获取对第一个附加数据磁盘的引用，如下所示：

**托管磁盘**

```powershell
$disk= Get-AzDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.DataDisks[0].Name
$disk.DiskSizeGB = 1023
```

**非托管磁盘**

```powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```

同样，可以使用索引（如上所示）或磁盘的“Name”属性引用附加到 VM 的其他数据磁盘：


**托管磁盘**

```powershell
(Get-AzDisk -ResourceGroupName $rgName -DiskName ($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'})).Name).DiskSizeGB = 1023
```

**非托管磁盘**

```powershell
($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'})).DiskSizeGB = 1023
```

## <a name="expand-the-volume-within-the-os"></a>扩展 OS 中的卷

扩展 VM 的磁盘后，需要转到 OS 并扩展卷，使之包含新的空间。 以下几种方法可用于扩展分区。 本部分介绍了如何使用 RDP 连接来连接 VM 以使用 DiskPart 展开分区。

1. 打开与 VM 的 RDP 连接。

2. 打开命令提示符并键入 diskpart

3. 在 DISKPART 提示符处，键入 `list volume`。 记下要扩展的卷。

4. 在 DISKPART 提示符处，键入 `select volume <volumenumber>`。 这将选择将扩展到同一磁盘上的连续可用空间的卷 volumenumber。

5. 在 DISKPART 提示符处，键入 `extend [size=<size>]`。 这将按大小（MB）扩展所选的卷。


## <a name="next-steps"></a>后续步骤

还可以使用 [Azure 门户](attach-managed-disk-portal.md)来附加磁盘。
