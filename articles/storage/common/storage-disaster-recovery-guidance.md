---
title: 灾难恢复和存储帐户故障转移（预览版）
titleSuffix: Azure Storage
description: Azure 存储支持异地冗余存储帐户故障转移（预览版）。 通过帐户故障转移，可以在主终结点不可用时为存储帐户启动故障转移过程。
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: f7a8f6d0d3ab3b456c41128da9b689f6b7eda0f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79365351"
---
# <a name="disaster-recovery-and-account-failover-preview"></a>灾难恢复和帐户故障转移（预览）

Microsoft 致力于确保 Azure 服务一直可用。 不过，可能会发生计划外服务中断。 如果应用程序需要恢复能力，Microsoft 建议使用异地冗余存储，以便将数据复制到第二个区域。 此外，客户还应制定用于处理区域服务中断的灾难恢复计划。 灾难恢复计划的一个重要组成部分是，准备在主终结点不可用时将故障转移到辅助终结点。

Azure 存储支持异地冗余存储帐户故障转移（预览版）。 通过帐户故障转移，可以在主终结点不可用时为存储帐户启动故障转移过程。 故障转移将辅助终结点更新为，存储帐户的主终结点。 在故障转移完成后，客户端便可以开始对新的主终结点执行写入操作。

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

本文介绍了帐户故障转移所涉及的概念和过程，以及如何让存储帐户做好恢复准备，且造成的客户影响最小。 若要了解如何在 Azure 门户或 PowerShell 中启动帐户故障转移，请参阅[启动帐户故障转移（预览版）](storage-initiate-account-failover.md)。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="choose-the-right-redundancy-option"></a>选择正确的冗余选项

Azure 存储将维护存储帐户的多个副本，以确保持续性和高可用性。 为帐户选择哪个冗余选项取决于所需的复原能力水平。 若要防止区域中断，请选择有权或无权从次要区域读取数据的异地冗余存储：  

**异地冗余存储 （GRS） 或地理区域冗余存储 （GZRS） （预览）** 在相距至少数百英里的两个地理区域中异步复制数据。 如果主要区域遭遇服务中断，次要区域便会成为数据的冗余源。 可以通过启动故障转移，将辅助终结点转换为主终结点。

**读取访问异地冗余存储 （RA-GRS） 或读取访问地理区域冗余存储 （RA-GZRS） （预览）** 提供异地冗余存储，此外还具有对辅助终结点的读取访问。 如果主终结点发生中断，配置了 RA-GRS 且旨在实现高可用性的应用程序可以继续从辅助终结点读取数据。 Microsoft 建议对应用程序使用 RA-GRS，以获取最大复原能力。

有关 Azure 存储中冗余的详细信息，请参阅 [Azure 存储冗余](storage-redundancy.md)。

> [!WARNING]
> 异地冗余存储有数据丢失风险。 数据是异步复制到次要区域，这意味着数据写入主要区域与数据写入次要区域之间存在延迟。 发生服务中断时，尚未复制到辅助终结点的对主终结点的写入操作将丢失。

## <a name="design-for-high-availability"></a>旨在实现高可用性

请务必从一开始就设计高可用性应用程序。 有关设计应用程序和计划灾难恢复方面的指导，请参阅以下 Azure 资源：

- [为 Azure 设计弹性应用程序](/azure/architecture/checklist/resiliency-per-service)：在 Azure 中构建高可用性应用程序的关键概念的概述。
- [可用性检查表](/azure/architecture/checklist/resiliency-per-service)：用于验证应用程序是否实现高可用性的最佳设计实践的清单。
- [使用 RA-GRS 设计高可用应用程序](storage-designing-ha-apps-with-ragrs.md)：用于构建应用程序以利用 RA-GRS 的设计指南。
- [教程：使用 Blob 存储构建一个高度可用的应用程序](../blobs/storage-create-geo-redundant-storage.md)：一个教程，演示如何构建一个高度可用的应用程序，该应用程序在模拟故障和恢复时在终结点之间自动切换。 

此外，还请注意下面这些可保持 Azure 存储数据高可用性的最佳做法：

- **磁盘：** 使用[Azure 备份](https://azure.microsoft.com/services/backup/)备份 Azure 虚拟机使用的 VM 磁盘。 还建议在发生区域灾难时使用 [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) 保护 VM。
- **阻止 blob：** 打开[软删除](../blobs/storage-blob-soft-delete.md)以防止对象级删除和覆盖，或使用[AzCopy、Azure](storage-use-azcopy.md) [PowerShell](storage-powershell-guide-full.md)或[Azure 数据移动库](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)将块 blob 复制到其他区域中的另一个存储帐户。
- **文件：** 使用[AzCopy](storage-use-azcopy.md)或[Azure PowerShell](storage-powershell-guide-full.md)将文件复制到其他区域中的另一个存储帐户。
- **表：** 使用 [AzCopy](storage-use-azcopy.md) 将表数据导出到其他区域中的另一个存储帐户内。

## <a name="track-outages"></a>跟踪服务中断

客户可以订阅 [Azure 服务运行状况仪表板](https://azure.microsoft.com/status/)，以跟踪 Azure 存储和其他 Azure 服务的运行状况和状态。

Microsoft 还建议将应用程序设计为，可以应对可能出现的写入故障。 应用程序应公开写入故障，以提醒你主要区域可能存在服务中断。

## <a name="understand-the-account-failover-process"></a>了解帐户故障转移过程

借助客户托管的帐户故障转移（预览版），可以在主要区域因任何原因而不可用时，将整个存储帐户故障转移到次要区域。 如果你强制故障转移到次要区域，客户端可以在故障转移完成后开始向辅助终结点写入数据。 故障转移通常需要大约一小时才能完成。

### <a name="how-an-account-failover-works"></a>帐户故障转移的工作原理

在正常情况下，客户端将数据写入主区域中的 Azure 存储帐户，并且数据会异步复制到辅助区域。 下图展示了主要区域可用时的场景：

![客户端将数据写入主要区域中的存储帐户](media/storage-disaster-recovery-guidance/primary-available.png)

如果主终结点因任何原因而不可用，客户端无法再向存储帐户写入数据。 下图展示了主终结点不可用、但尚未执行恢复时的场景：

![主终结点不可用，因此客户端无法写入数据](media/storage-disaster-recovery-guidance/primary-unavailable-before-failover.png)

客户启动帐户故障转移到辅助终结点。 故障转移过程更新 Azure 存储提供的 DNS 条目，这样辅助终结点就会成为存储帐户的新主终结点，如下图所示：

![客户启动帐户故障转移到辅助终结点](media/storage-disaster-recovery-guidance/failover-to-secondary.png)

在 DNS 条目已更新且请求定向到新的主终结点后，GRS 帐户和 RA-GRS 帐户便会恢复写入访问权限。 在故障转移完成后，用于 blob、表、队列和文件的现有存储服务终结点保持不变。

> [!IMPORTANT]
> 在故障转移完成后，存储帐户被配置为在新的主终结点中本地冗余。 若要继续复制到新的辅助终结点，请将帐户配置为重新使用异地冗余存储（RA-GRS 或 GRS）。
>
> 请注意，将 LRS 帐户转换为 RA-GRS 帐户或 GRS 帐户会产生费用。 如果在故障转移完成后将新的主要区域中的存储帐户更新为使用 RA-GRS 或 GRS，也会产生此费用。  

### <a name="anticipate-data-loss"></a>预测数据丢失

> [!CAUTION]
> 帐户故障转移通常涉及一些数据丢失。 请务必了解启动帐户故障转移的影响。  

由于数据是从主区域异步写入辅助区域的，因此在将写入主区域复制到辅助区域之前，始终存在延迟。 如果主区域不可用，则最近的写入可能尚未复制到辅助区域。

如果强制执行故障转移，主要区域中的所有数据就会在次要区域成为新的主要区域且存储帐户配置为本地冗余时丢失。 故障转移发生时，将保留已复制到辅助数据库的所有数据。 但是，写入主数据库的任何数据（未复制到辅助数据库）都将永久丢失。

“上次同步时间”**** 属性表示，最近一次保证已将主要区域中的数据写入次要区域的时间。 上次同步时间之前写入的所有数据都已复制到次要区域中，而在上次同步时间之后写入的数据则可能尚未写入次要区域并发生丢失。 在发生服务中断时，使用此属性可估计启动帐户故障转移可能会造成的数据丢失量。

最佳做法是，将应用程序设计为，可以使用上次同步时间来评估预期数据丢失。 例如，若要记录所有写入操作，可以比较上次写入操作时间与上次同步时间，以确定哪些写入操作尚未同步到次要区域。

### <a name="use-caution-when-failing-back-to-the-original-primary"></a>谨慎故障回复到原始主要区域

从主要区域故障转移到次要区域后，存储帐户被配置为在新的主要区域中本地冗余。 可以将帐户更新为使用 GRS 或 RA-GRS，从而再次为帐户配置异地冗余。 当帐户在故障转移后再次配置为异地冗余时，新的主区域立即开始将数据复制到新的辅助区域，这是原始故障转移之前的主区域。 但是，在将主数据库的现有数据完全复制到新辅助数据库之前，可能需要一些时间。

为存储帐户重新配置异地冗余后，可以启动另一个故障转移，从新的主要区域故障回复到新的次要区域。 在这种情况下，故障转移发生前的原始主要区域重新成为主要区域，并配置为本地冗余。 故障转移完成后的主要区域（即原始次要区域）中的所有数据都会丢失。 如果在故障恢复之前，存储帐户中的大多数数据尚未复制到新的辅助数据库，则可能会遭受重大数据丢失。

为了避免大量数据丢失，请在故障回复前检查“上次同步时间”**** 属性的值。 若要评估预期数据丢失，请比较上次同步时间与数据上次写入新的主要区域时间。 

## <a name="initiate-an-account-failover"></a>启动帐户故障转移

可以从 Azure 门户、PowerShell、Azure CLI 或 Azure 存储资源提供程序 API 启动帐户故障转移。 若要详细了解如何启动故障转移，请参阅[启动帐户故障转移（预览版）](storage-initiate-account-failover.md)。

## <a name="about-the-preview"></a>关于此预览版

使用 GRS 或 RA-GRS 部署的所有客户都可以预览帐户故障转移。 支持常规用途 v1、常规用途 v2 和 Blob 存储帐户类型。 帐户故障转移当前在所有公共区域都可用。 此时，帐户故障转移在主权/国家云中不可用。

此预览版仅用于非生产用途。 生产服务级别协议 (SLA) 当前不可用。

### <a name="additional-considerations"></a>其他注意事项

请参阅此部分中介绍的其他注意事项，以了解在预览期间强制执行故障转移会对应用程序和服务造成什么影响。

#### <a name="storage-account-containing-archived-blobs"></a>包含存档 Blob 的存储帐户

包含存档 Blob 的存储帐户支持帐户故障转移。 故障转移完成后，要将帐户转换回 GRS 或 RA-GRS，所有存档的 Blob 都需要先重新水化到联机层。

#### <a name="storage-resource-provider"></a>存储资源提供程序

故障转移完成后，客户端可以在新的主区域中再次读取和写入 Azure 存储数据。 但是，Azure 存储资源提供程序不会故障转移，因此资源管理操作仍必须在主区域中执行。 如果主区域不可用，您将无法对存储帐户执行管理操作。

由于 Azure 存储资源提供程序不会故障转移，[因此在](/dotnet/api/microsoft.azure.management.storage.models.trackedresource.location)故障转移完成后，位置属性将返回原始主位置。

#### <a name="azure-virtual-machines"></a>Azure 虚拟机

Azure 虚拟机 (VM) 不会在帐户故障转移过程中进行故障转移。 如果主要区域不可用且你故障转移到次要区域，那么就需要在故障转移完成后重新创建所有 VM。 此外，还有与帐户故障转移关联的潜在数据丢失。 Microsoft 建议遵循 Azure 中虚拟机特有的[以下高可用性](../../virtual-machines/windows/manage-availability.md)和[灾难恢复](../../virtual-machines/virtual-machines-disaster-recovery-guidance.md)指南。

#### <a name="azure-unmanaged-disks"></a>Azure 非托管磁盘

根据最佳做法，Microsoft 建议将非托管磁盘转换为托管磁盘。 不过，如果需要对包含附加到 Azure VM 的非托管磁盘的帐户进行故障转移，必须在启动故障转移前关闭 VM。

非托管磁盘在 Azure 存储中存储为页 blob。 如果 VM 在 Azure 中运行，任何附加到 VM 的非托管磁盘都会被租用。 如果 blob 上有租用，便无法继续帐户故障转移。 若要执行故障转移，请按照以下步骤操作：

1. 开始前，先记下所有非托管磁盘的名称、逻辑单元号 (LUN) 和附加到的 VM。 此操作可便于在故障转移完成后更轻松地重新附加磁盘。
2. 关闭 VM。
3. 删除 VM，但保留非托管磁盘的 VHD 文件。 记下 VM 删除时间。
4. 等到“上次同步时间”**** 已更新且晚于 VM 删除时间。 这一步很重要，因为如果在故障转移发生时辅助终结点尚未使用 VHD 文件完全更新，那么 VM 可能无法在新的主要区域中正常运行。
5. 启动帐户故障转移。
6. 等到帐户故障转移完成，且次要区域已成为新的主要区域。
7. 在新的主要区域中创建 VM，并重新附加 VHD。
8. 启动新 VM。

请注意，当 VM 关闭时，临时磁盘中存储的任何数据都会丢失。

### <a name="unsupported-features-and-services"></a>不支持的功能和服务

预览版本的帐户故障转移不支持以下功能和服务：

- Azure 文件同步不支持存储帐户故障转移。 不得对包含 Azure 文件共享且用作 Azure 文件同步中云终结点的存储帐户执行故障转移。 否则，将会导致同步停止，并且可能还会在有新分层文件的情况下导致意外数据丢失。
- 目前不支持 ADLS Gen2 存储帐户（启用了分层命名空间的帐户）。
- 无法对包含高级块 blob 的存储帐户执行故障转移。 支持高级块 blob 的存储帐户暂不支持异地冗余。
- 包含任何[启用 WORM 不变性策略的](../blobs/storage-blob-immutable-storage.md)容器的存储帐户不能失败。 已解锁/锁定基于时间的保留或法定保留策略可防止故障转移，以保持合规性。
- 故障转移完成后，如果最初启用，以下功能可能会停止工作：[事件订阅](../blobs/storage-blob-event-overview.md)、[更改源](../blobs/storage-blob-change-feed.md)、[生命周期策略](../blobs/storage-lifecycle-management-concepts.md)和[存储分析日志记录](storage-analytics-logging.md)。

## <a name="copying-data-as-an-alternative-to-failover"></a>除了故障转移外，还可以复制数据

如果存储帐户已配置 RA-GRS，你便有权读取访问辅助终结点中的数据。 如果不希望在主要区域发生服务中断时进行故障转移，可使用 [AzCopy](storage-use-azcopy.md)、[Azure PowerShell](storage-powershell-guide-full.md) 或 [Azure 数据移动库](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)等工具，将数据从次要区域中的存储帐户复制到未受影响区域中的另一个存储帐户。 然后，可以将应用程序指向此存储帐户，以进行读写操作。

> [!CAUTION]
> 帐户故障转移不应用作数据迁移策略的一部分。


## <a name="microsoft-managed-failover"></a>Microsoft 托管的故障转移

在由于重大灾难而导致区域丢失的极端情况下，Microsoft 可能会启动区域故障转移。 在此情况下，不需要采取任何操作。 在 Microsoft 托管的故障转移完成之前，你对存储帐户不拥有写入访问权限。 如果存储帐户已配置 RA-GRS，应用程序可以从次要区域读取数据。 

## <a name="see-also"></a>请参阅

- [启动帐户故障转移（预览版）](storage-initiate-account-failover.md)
- [使用 RA-GRS 设计高可用应用程序](storage-designing-ha-apps-with-ragrs.md)
- [教程：使用 Blob 存储构建高可用应用程序](../blobs/storage-create-geo-redundant-storage.md) 
