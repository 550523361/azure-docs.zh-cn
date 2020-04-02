---
title: include 文件
description: include 文件
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/31/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: ad36547b0cf9457d7ff4fdd919a0bb1362539eb5
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2020
ms.locfileid: "80522721"
---
本文将对有关 Azure 托管磁盘和 Azure 高级 SSD 盘的一些常见问题进行解答。

## <a name="managed-disks"></a>托管磁盘

什么是 Azure 托管磁盘？****

托管磁盘是一种通过处理存储帐户管理来简化 Azure IaaS VM 的磁盘管理的功能。 有关详细信息，请参阅[托管磁盘概述](../articles/virtual-machines/windows/managed-disks-overview.md)。

如果从现有的 VHD（80 GB）创建标准托管磁盘，需要多少费用？****

从 80 GB VHD 创建的标准托管磁盘被视为下一个可用的标准磁盘大小（S10 磁盘）。 我们按 S10 磁盘定价收费。 有关详细信息，请参阅[定价页](https://azure.microsoft.com/pricing/details/storage)。

标准托管磁盘是否存在任何事务成本？****

是的。 我们针对每个事务进行收费。 有关详细信息，请参阅[定价页](https://azure.microsoft.com/pricing/details/storage)。

对于标准托管磁盘，是对磁盘上的数据实际大小收费还是对磁盘的预配容量收费？****

我们根据磁盘的预配容量收费。 有关详细信息，请参阅[定价页](https://azure.microsoft.com/pricing/details/storage)。

高级托管磁盘与非托管磁盘的定价有何不同？****

高级托管磁盘的定价与高级非托管磁盘的定价相同。

是否可以更改托管磁盘的存储帐户类型（标准或高级）？****

是的。 可以使用 Azure 门户、PowerShell 或 Azure CLI 更改托管磁盘的存储帐户类型。

是否可以使用 Azure 存储帐户中的 VHD 文件以不同的订阅创建托管磁盘？****

是的。

是否可以使用 Azure 存储帐户中的 VHD 文件在不同的区域中创建托管磁盘？****

不是。

客户使用托管磁盘是否存在任何规模限制？****

托管磁盘取消了与存储帐户相关的限制。 但是，订阅的最大限制为每个区域、每个磁盘类型 50,000 个托管磁盘。

可用性集中的 VM 是否可以同时包含托管和非托管磁盘？****

不是。 可用性集中的 VM 必须全部使用托管磁盘或全部使用非托管磁盘。 创建可用性集时，可以选择要使用的磁盘类型。

托管磁盘是否是 Azure 门户中的默认选项？****

是的。

是否可以创建一个空托管磁盘？****

是的。 可创建空磁盘。 可独立于 VM 创建托管磁盘，例如，不需要将磁盘附加到 VM。

什么是使用托管磁盘的可用性集的支持容错域计数？****

使用托管磁盘的可用性集的支持容错域计数为 2 或 3，具体取决于它所在的区域。

如何设置用于诊断的标准存储帐户？****

设置 VM 诊断的专用存储帐户。

托管磁盘支持哪类基于角色的访问控制？****

托管磁盘支持三个密钥默认角色：

* 所有者：可管理所有内容，包括访问权限
* 参与者：可管理除访问权限以外的所有内容
* 读者：可查看所有内容，但不能进行更改

是否可将托管磁盘复制或导出到专用存储帐户？****

可以为托管磁盘生成只读共享访问签名 (SAS) URI，使用它将内容复制到专用存储帐户或本地存储。 您可以使用 Azure 门户、Azure 电源外壳、Azure CLI 或[AzCopy 使用](../articles/storage/common/storage-use-azcopy.md)SAS URI

是否可以创建托管磁盘副本？****

客户可以生成托管磁盘的快照，并使用快照创建另一个托管磁盘。

是否仍支持非托管磁盘？****

是的，非托管磁盘和托管磁盘均受支持。 我们建议你对新的工作负荷使用托管磁盘，并将当前的工作负荷迁移到托管磁盘。

是否可以在同一 VM 上归置非托管和托管磁盘？****

不是。

**如果创建 128 GB 磁盘，然后将该大小增加到 130 GiB，是否会针对下一磁盘大小 (256 GiB) 进行收费？**

是的。

是否可以创建本地冗余存储、异地冗余存储和区域冗余存储托管磁盘？****

Azure 托管磁盘当前仅支持本地冗余存储托管磁盘。

是否可以收缩或缩小托管磁盘？****

不是。 目前，不支持此功能。

是否可以在磁盘上中断租用？****

不是。 目前不支持此功能，因为租用的作用是防止磁盘在使用时被意外删除。

当使用专用（未使用系统准备工具创建或未通用化）操作系统磁盘预配 VM 时，是否可以更改计算机名称属性？****

不是。 无法更新计算机名称属性。 新 VM 从创建操作系统磁盘时所用的父 VM 继承该属性。 

在哪里可找到用于使用托管磁盘创建 VM 的示例 Azure 资源管理器模板？****
* [使用托管磁盘的模板列表](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

**基于 blob 创建磁盘时，与该源 blob 之间是否会一直保持任何现有关系？**

否，创建新磁盘时，它是该 blob 在该时间的完整独立副本，两者之间没有联系。 如果你愿意，可以在创建磁盘后删除源 blob，这对新创建的磁盘没有任何影响。

**创建托管或非托管磁盘后，是否可以对其进行重命名？**

对于托管磁盘，无法对其进行重命名。 但是，可以对非托管磁盘进行重命名，只要它当前未附加到 VHD 或 VM。

**可否在 Azure 磁盘上使用 GPT 分区？**

第 1 代映像只能在数据磁盘而不是 OS 磁盘上使用 GPT 分区。 操作系统磁盘必须使用 MBR 分区样式。

[第 2 代映像](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2)可以在 OS 磁盘和数据磁盘上使用 GPT 分区。

**哪些磁盘类型支持快照？**

高级 SSD、标准 SSD 和标准 HDD 支持快照。 对于这三种磁盘类型，所有磁盘大小（包括大小高达 32 TiB 的磁盘）都支持快照。 超级磁盘不支持快照。

**什么是 Azure 磁盘预留？**
磁盘预留是提前购买一年磁盘存储的选项，从而降低了您的总成本。 有关 Azure 磁盘预留的详细信息，请参阅我们有关主题的文章：[了解预订折扣如何应用于 Azure 磁盘](../articles/cost-management-billing/reservations/understand-disk-reservations.md)。

**Azure 磁盘预留提供哪些选项？**    
Azure 磁盘预留提供了在指定的 SKU 中购买高级 SSD 的选项，从 P30 （1 TiB） 到 P80 （32 TiB），期限为一年。 购买磁盘预留所需的最小磁盘数量没有限制。 此外，您可以选择使用单笔预付款或每月付款付款。 高级 SSD 托管磁盘没有应用额外的事务成本。    

预订以磁盘形式进行，而不是以容量形式进行。 换句话说，当您保留 P80 （32 TiB） 磁盘时，您会得到一个 P80 磁盘，因此无法将该特定预留划分为两个较小的 P70 （16 TiB） 磁盘。 当然，您可以保留您喜欢的磁盘数量或数量之少，包括两个单独的 P70 （16 TiB） 磁盘。

**如何应用 Azure 磁盘预留？**    
磁盘预留遵循类似于预留虚拟机 （VM） 实例的模型。 区别在于磁盘预留不能应用于不同的 SKU，而 VM 实例可以。 有关 VM 实例的详细信息[，请参阅使用 Azure 预留 VM 实例节省成本](../articles/virtual-machines/linux/prepay-reserved-vm-instances.md)。     

**我能否使用通过 Azure 磁盘预留跨多个区域购买的数据存储？**    
Azure 磁盘预留是为特定区域和 SKU（如美国东部 2 中的 P30）购买的，因此不能在这些构造之外使用。 您始终可以购买其他 Azure 磁盘预留，以满足其他区域或 SKU 中的磁盘存储需求。    

**Azure 磁盘保留到期时会发生什么情况？**    
您将在到期前 30 天收到电子邮件通知，并在到期日期再次收到通知。 一旦预订到期，部署的磁盘将继续运行，并将用最新的即[用即付费率](https://azure.microsoft.com/pricing/details/managed-disks/)计费。

### <a name="azure-shared-disks"></a>Azure 共享磁盘

**对于非托管磁盘或页面 Blob，是否支持共享磁盘功能？**

否，它仅支持高级 SSD 托管磁盘。

**哪些区域支持共享磁盘？**

目前只有美国中西部。

**共享磁盘是否可以用作 OS 磁盘？**

否，共享磁盘仅支持数据磁盘。

**哪些磁盘大小支持共享磁盘？**

只有 P15 或更高支持的高级 SSD 支持共享磁盘。

**如果我有现有的高级 SSD，是否可以启用共享磁盘？**

使用 API 版本 2019-07-01 或更高版本创建的所有托管磁盘都可以启用共享磁盘。 为此，您需要从磁盘附加到的所有 VM 中卸载磁盘。 接下来，`maxShares`编辑磁盘上的属性。

**如果我不想再在共享模式下使用磁盘，如何禁用它？**

从磁盘附加到的所有 VM 中卸载磁盘。 然后，将磁盘上的 maxShare 属性编辑为 1。

**您可以调整共享磁盘的大小吗？**

是的。

**是否可以在同时启用共享磁盘的磁盘上启用写入加速器？**

不是。

**是否可以为已启用共享磁盘的磁盘启用主机缓存？**

唯一受支持的主机缓存选项是"无"。

## <a name="ultra-disks"></a>超级磁盘

**我应该将超磁盘吞吐量设置为什么？**
如果您不确定要将磁盘吞吐量设置为什么，我们建议您首先假定 IO 大小为 16 KiB，并在监视应用程序时从该大小调整性能。 公式为：MBps 中的吞吐量 = IOPS 的 + 16 / 1000。

**我把我的磁盘配置到 40000 IOPS，但我只看到 12800 IOPS，为什么我看不到磁盘的性能？**
除了磁盘限制之外，还有一个在 VM 级别施加的 IO 限制。 请确保正在使用的 VM 大小可以支持磁盘上配置的级别。 有关 VM 施加的 IO 限制的详细信息，请参阅[Azure 中 Windows 虚拟机的大小](../articles/virtual-machines/windows/sizes.md)。

**我可以使用超磁盘的缓存级别吗？**
不，超磁盘不支持其他磁盘类型支持的不同缓存方法。 将磁盘缓存设置为"无"。

**我可以将超磁盘附加到现有 VM 吗？**
也许，您的 VM 必须位于支持 Ultra 磁盘的区域和可用性区域对中。 有关详细信息[，请参阅开始使用超磁盘](../articles/virtual-machines/windows/disks-enable-ultra-ssd.md)。

**我是否可以将超磁盘用作 VM 的 OS 磁盘？**
不，超级磁盘仅作为数据磁盘支持，仅作为 4K 本机磁盘受支持。

**我可以将现有磁盘转换为超磁盘吗？**
否，但您可以将数据从现有磁盘迁移到超磁盘。 要将现有磁盘迁移到超级磁盘，请将两个磁盘连接到同一 VM，并将磁盘的数据从一个磁盘复制到另一个磁盘，或者利用第三方解决方案进行数据迁移。

**我可以为超磁盘创建快照吗？**
否，快照尚不可用。

**Azure 备份是否可用于超磁盘？**
否，Azure 备份支持尚不可用。

**我可以将超磁盘连接到在可用性集中运行的 VM 吗？**
否，这不受支持。

**是否可以使用超磁盘为 VM 启用 Azure 站点恢复？**
否，超级磁盘尚不支持 Azure 站点恢复。

## <a name="uploading-to-a-managed-disk"></a>上传到托管磁盘

**是否可将数据上传到现有的托管磁盘？**

不可以。只能在创建 **ReadyToUpload** 状态的新空磁盘期间使用上传。

**如何上传到托管磁盘？**

创建一个托管磁盘，并将 [creationData](https://docs.microsoft.com/rest/api/compute/disks/createorupdate#creationdata) 的 [createOption](https://docs.microsoft.com/rest/api/compute/disks/createorupdate#diskcreateoption) 属性设置为“Upload”，然后可以将数据上传到该磁盘。

**是否可将处于上传状态的磁盘附加到 VM？**

不是。

**是否可以创建处于上传状态的托管磁盘的快照？**

不是。

## <a name="standard-ssd-disks"></a>标准 SSD 盘

Azure 标准 SSD 盘是什么？****
标准 SSD 盘是受固态介质支持的标准磁盘，经过优化而作为在较低 IOPS 级别需要一致性能的工作负载的高性价比存储。

<a id="standard-ssds-azure-regions"></a>**当前支持标准 SSD 盘的区域有哪些？**
所有 Azure 区域现在都支持标准 SSD 盘。

**使用标准 SSD 时是否可以使用 Azure 备份？**
是的，Azure 备份现已可用。

如何创建标准 SSD 盘？****
可以使用 Azure 资源管理器模板、SDK、PowerShell 或 CLI 创建标准 SSD 盘。 以下为创建标准 SSD 盘时资源管理器模板中所需的参数：

* Microsoft.Compute 的 apiVersion** 必须设置为 `2018-04-01`（或更高）
* 将 managedDisk.storageAccountType 指定为 `StandardSSD_LRS`**

以下示例显示了使用标准 SSD 盘的 VM 的 properties.storageProfile.osDisk 部分**：

```json
"osDisk": {
    "osType": "Windows",
    "name": "myOsDisk",
    "caching": "ReadWrite",
    "createOption": "FromImage",
    "managedDisk": {
        "storageAccountType": "StandardSSD_LRS"
    }
}
```

有关如何使用模板创建标准 SSD 盘的完整模板示例，请参阅[使用标准 SSD 数据磁盘从 Windows 映像创建 VM](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/)。

是否可以将现有磁盘转换为标准 SSD？****
可以。 请参阅[将 Azure 托管磁盘存储从标准转换为高级，反之亦然](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage)，以了解有关转换托管磁盘的常规指南。 此外，使用以下值将磁盘类型更新为标准 SSD。
-AccountType StandardSSD_LRS

**使用标准 SSD 盘而不使用 HDD 的好处是什么？**
与 HDD 磁盘相比，标准 SSD 盘可以提供更好的延迟、一致性、可用性和可靠性。 因此，应用程序工作负荷可以更平稳地在标准 SSD 上运行。 注意，高级 SSD 盘是适用于大多数 IO 密集型生产工作负荷的建议解决方案。

是否可将标准 SSD 用作非托管磁盘？****
不可以，标准 SSD 盘仅可用作托管磁盘。

标准 SSD 磁盘是否支持“单实例 VM SLA”？****
不是，标准 SSD 没有单实例 VM SLA。 将高级 SSD 磁盘用于单实例 VM SLA。

## <a name="migrate-to-managed-disks"></a>迁移到托管磁盘

**迁移是否对托管磁盘性能产生任何影响？**

迁移涉及将磁盘从一个存储位置移动到另一个存储位置。 这是通过在后台复制数据来安排的，可能需要花费数小时才能完成，通常少于 24 小时，具体取决于磁盘中的数据量。 在此期间，由于一些读取可能被重定向到原始位置，所以应用程序可能会经历比平常更高的读取延迟，并且可能需要花费更长时间才能完成。 在此期间，对写入延迟没有影响。  

迁移到托管磁盘之前/之后，需要在现有的 Azure 备份服务配置中进行哪些更改？****

不需要进行任何更改。

在迁移之前通过 Azure 备份服务创建的 VM 备份是否可继续工作？****

是的，备份可以顺利工作。

迁移到托管磁盘之前/之后，需要在现有的 Azure 磁盘加密配置中进行哪些更改？****

不需要进行任何更改。

**是否支持将现有虚拟机规模集从非托管磁盘自动迁移到托管磁盘？**

不是。 可以使用包含非托管磁盘的旧规模集中的映像创建包含托管磁盘的新规模集。

是否可以通过迁移到托管磁盘之前创建的页 Blob 快照创建托管磁盘？****

不是。 可将页 Blob 快照导出为页 Blob，然后从导出的页 Blob 创建托管磁盘。

是否可将 Azure Site Recovery 保护的本地计算机故障转移到包含托管磁盘的 VM？****

是的，可以选择故障转移到包含托管磁盘的 VM。

迁移是否影响 Azure Site Recovery 通过 Azure 到 Azure 复制保护的 Azure VM？****

不是。 对于包含托管磁盘的 VM，提供 Azure Site Recovery Azure 到 Azure 保护。

是否可以迁移位于存储帐户中现在或以前已加密的 VM 的非托管磁盘迁移到托管磁盘？****

是

## <a name="managed-disks-and-storage-service-encryption"></a>托管磁盘和存储服务加密

创建托管磁盘时，是否会默认启用 Azure 存储服务加密？****

是的。

**默认情况下，托管磁盘上的启动卷是否已加密？**

是的。 默认情况下，所有托管磁盘均已加密，包括 OS 磁盘。

**加密密钥由谁管理？**

Microsoft 管理加密密钥。

是否可以为托管磁盘禁用存储服务加密？****

不是。

存储服务加密是否仅适用于特定区域？****

不是。 它适用于托管磁盘可用的所有区域。 托管磁盘适用于所有公共区域和德国。 这也适用于中国，但是，仅适用于 Microsoft 托管密钥，不适用于客户托管密钥。

如何确定我的托管磁盘是否已加密？****

你可以从 Azure 门户、Azure CLI 和 PowerShell 确定托管磁盘的创建时间。 如果时间是在 2017 年 6 月 9 日之后，则磁盘已加密。

如何对 2017 年 6 月 10 日之前创建的现有磁盘加密？****

自 2017 年 6 月 10 日起，写入到现有托管磁盘的新数据会自动加密。 我们还打算对现有数据进行加密，且在后台以异步方式加密。 如果必须立即对现有数据进行加密，请创建磁盘的副本。 将对新磁盘进行加密。

* [使用 Azure CLI 复制托管磁盘](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [使用 PowerShell 复制托管磁盘](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

是否已加密托管快照和映像？****

是的。 2017 年 6 月 9 日之后创建的所有托管快照和映像均会自动加密。 

是否可以将 VM 的位于存储帐户且现在或以前已加密的非托管磁盘转换为托管磁盘？****

是

是否同时会加密从托管磁盘或快照导出的 VHD？****

不是。 但如果将 VHD 从加密托管磁盘或快照导出到加密存储帐户，则会对其进行加密。 

## <a name="premium-disks-managed-and-unmanaged"></a>高级磁盘：托管和非托管

如果 VM 使用支持高级 SSD 盘的大小系列（比如 DSv2），是否可以同时附加高级和标准数据磁盘？**** 

是的。

是否可以同时将高级和标准数据磁盘附加到不支持高级 SSD 盘的大小系列，例如 D、Dv2、G 或 F 系列？****

不是。 只可以将标准数据磁盘附加到不使用支持高级 SSD 盘的大小系列的 VM。

如果从现有的 VHD (80 GB) 创建高级数据磁盘，需要多少费用？****

从 80 GB VHD 创建的高级数据磁盘被视为下一个可用的高级磁盘大小（P10 磁盘）。 我们按 P10 磁盘定价收费。

使用高级 SSD 盘时是否存在事务成本？****

每个磁盘大小都有固定成本，其根据 IOPS 和吞吐量的特定限制进行预配。 其他成本包括出站带宽和快照容量（如果适用）。 有关详细信息，请参阅[定价页](https://azure.microsoft.com/pricing/details/storage)。

可从磁盘缓存获取的 IOPS 和吞吐量限制是多少？****

DS 系列的缓存和本地 SSD 合并限制是每个核心 4,000 IOPS，以及每个核心每秒 33 MiB。 GS 系列提供每个核心 5,000 IOPS，每个核心每秒 50 MiB。

托管磁盘 VM 是否支持本地 SSD？****

本地 SSD 是托管磁盘 VM 随附的临时存储。 临时存储不需要额外的成本。 建议不要使用此本地 SSD 来存储应用程序数据，因为这些数据不会永久保存在 Azure Blob 存储中。

在高级磁盘上使用 TRIM 是否有任何影响？****

在高级或标准磁盘的 Azure 磁盘上使用 TRIM 没有负面影响。

## <a name="new-disk-sizes-managed-and-unmanaged"></a>新磁盘大小：托管和非托管

**哪些区域支持针对适用的优质 SSD 磁盘大小的爆破功能？**

Azure 公共云中的所有区域当前都支持爆破功能，对主权云的支持即将推出。 

**哪些区域支持 4/8/16 GiB 托管磁盘大小（P1/P2/P3、E1/E2/E3）？**

Azure 公共云中的所有区域当前都支持这些新磁盘大小，并且将很快支持主权云。 

**非托管磁盘或页 blob 是否支持 P1/P2/P3 磁盘大小？**

否，它只在高级 SSD 托管磁盘上受支持。 

**非托管磁盘或页 blob 是否支持 E1/E2/E3 磁盘大小？**

否，任何大小的标准 SSD 托管磁盘都不能与非托管磁盘或页 blob 配合使用。

**操作系统和数据磁盘支持的最大托管磁盘大小是多少？**

Azure 支持的操作系统磁盘的分区类型是主启动记录 (MBR)。 MBR 格式支持的磁盘最大大小为 2 TiB。 Azure 支持的操作系统磁盘的最大大小为 2 TiB。 Azure 支持的托管数据磁盘最大大小为 32 TiB。

**操作系统和数据磁盘支持的最大非托管磁盘大小是多少？**

Azure 支持的操作系统磁盘的分区类型是主启动记录 (MBR)。 MBR 格式支持的磁盘最大大小为 2 TiB。 Azure 支持的操作系统非托管磁盘的最大大小为 2 TiB。 Azure 支持的非托管数据磁盘最大大小为 4 TiB。

支持的最大页 blob 大小是多少？****

Azure 支持的最大页 blob 大小是 8 TiB (8,191 GiB)。 附加到 VM 作为数据或操作系统磁盘时，最大页 blob 大小为 4 TiB (4,095 GiB)。

**是否需要使用新版本的 Azure 工具来创建、附加、上传大于 1 TiB 的磁盘并重设其大小？**

无需升级现有 Azure 工具即可创建、附加大于 1 TiB 的磁盘或重设其大小。 若要直接从本地将 VHD 文件作为页 blob 或非托管磁盘上传到到 Azure，需要使用下方列出的最新工具集。 我们仅支持最大大小为 8 TiB 的 VHD 上传。

|Azure 工具      | 支持的版本                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | 版本号 4.1.0：2017 年 6 月版本或更高版本|
|Azure CLI v1     | 版本号 0.10.13：2017 年 5 月版本或更高版本|
|Azure CLI v2     | 版本号 2.0.12：2017 年 7 月版本或更高版本|
|AzCopy              | 版本号 6.1.0：2017 年 6 月版本或更高版本|

非托管磁盘或页 blob 是否支持 P4 和 P6 磁盘大小？****

非托管磁盘和页 blob 不支持 P4 (32 GiB) 和 P6 (64 GiB) 磁盘大小作为默认磁盘层。 需要显式[设置 Blob 层](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier)，将其设为 P4 和 P6，以便存储映射到这些层的磁盘。 如果使用少于 32 GiB 或介于 32 GiB 到 64 GiB 之间的磁盘大小或内容长度部署非托管磁盘或页 blob 而不设置 Blob 层，则将继续停留在 P10（500 IOPS 和 100 MiB/秒）和映射的定价层。

**如果在支持较小磁盘（约 2017 年 6 月 15 日）之前创建了小于 64 GiB 的高级托管磁盘，将如何计费？**

根据 P10 定价层继续对小于 64 GiB 的现有高级小磁盘计费。

**如何将小于 64 GiB 的高级小磁盘的磁盘层级从 P10 切换到 P4 或 P6？**

你可以拍摄小磁盘的快照，然后创建磁盘以自动根据预配大小将定价层切换到 P4 或 P6。

**是否可以将托管磁盘的大小从小于 4 TiB 调整到最新引入的磁盘大小（最大大小为 32 TiB）？**

是的。

**Azure 备份和 Azure Site Recovery 服务支持的最大磁盘大小是多少？**

Azure 备份支持的最大磁盘大小是 32 TiB（加密磁盘为 4 TiB）。 Azure 站点恢复支持的最大磁盘大小为 8 TiB。 Azure 站点恢复中尚未提供对最多 32 个 TiB 的较大磁盘的支持。

**标准 SSD 和标准 HDD 大型磁盘大小 (>4 TiB) 用于实现优化磁盘 IOPS 和带宽建议的 VM 大小是多少？**

为了实现标准 SSD 和标准硬盘大型磁盘（>4 TiB）超过 500 IOPS 和 60 MiB/s 的磁盘吞吐量， 我们建议您从以下 VM 大小之一部署新的 VM 以优化性能：B 系列、DSv2 系列、Dsv3 系列、ESv3 系列、Fs 系列、Fsv2 系列、M 系列、GS 系列、NCv2 系列、NCv3 系列或 Ls 系列 VM。 将大型磁盘附加到现有 VM 或者不使用上述建议大小的 VM 可能会遇到性能下降的问题。

**如何升级在大型磁盘预览期部署的磁盘 (> 4 TiB)，以便在正式版中获得更高的 IOPS 和带宽？**

可以停止然后启动该磁盘附加到 VM，或者分离然后重新附加磁盘。 在正式版中，高级 SSD 和标准 SSD 大型磁盘的性能目标已提高。

**哪些区域支持大于 8 TiB、16 TiB 和 32 TiB 的托管磁盘大小？**

Azure 全球、 Microsoft Azure 政府和 Azure 中国世纪互联涵盖的所有区域都支持 8 TiB、16 TiB 和 32 TiB 磁盘 SKU。

**是否支持在所有磁盘大小上启用主机缓存？**

我们支持在小于 4 TiB 的磁盘大小上启用只读和读/写主机缓存。 对于超过 4 TiB 的磁盘大小，我们并不支持设置“无”以外的缓存选项。 建议利用较小磁盘大小的缓存，以便通过缓存到 VM 的数据观察到明显的性能提升。

## <a name="what-if-my-question-isnt-answered-here"></a>如果未在此处找到相关问题怎么办？

如果未在此处找到相关问题，请联系我们获取帮助。 你可以在本文末尾的评论中发布问题。 若要与 Azure 存储团队和其他社区成员就本文进行沟通，请使用 MSDN [Azure 存储论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)。

若要提出功能请求，请将请求和想法提交到 [Azure 存储反馈论坛](https://feedback.azure.com/forums/217298-storage)。
