---
title: include 文件
description: include 文件
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c0627dd0833e3b20468eb5f50fbeb9fd9d9ae2b3
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88864638"
---
**出站数据传输**：[出站数据传输](https://azure.microsoft.com/pricing/details/bandwidth/)（传出 Azure 数据中心的数据）会产生带宽使用费。

**事务**：会根据你对标准托管磁盘执行的事务数向你收费。 对于标准 SSD，每个小于或等于 256 KiB 吞吐量的 I/O 操作被视为单个 I/O 操作。 大于 256 KiB 吞吐量的 I/O 操作被视为大小为 256 KiB 的多个 I/O。 对于标准 HDD，每个 IO 操作会被视为单个事务，无论 I/O 大小如何。

有关托管磁盘定价的详细信息（包括事务成本），请参阅[托管磁盘定价](https://azure.microsoft.com/pricing/details/managed-disks)。

### <a name="ultra-disk-vm-reservation-fee"></a>超磁盘 VM 保留费用

Azure Vm 可以指示它们是否与超磁盘兼容。 与超级磁盘兼容的 VM 在计算 VM 实例与块存储缩放单元之间分配专用的带宽容量，以优化性能并降低延迟。 在 VM 上添加此功能会导致预留费用，不过，这笔费用是仅当在 VM 上启用了超级磁盘功能但未将超级磁盘附加到 VM 时才产生的。 如果将超级磁盘附加到与超级磁盘兼容的 VM，则不会收取此费用。 此费用根据 VM 上预配的每个 vCPU 计收。 

> [!Note]
> 对于 [受约束的核心 VM 大小](../articles/virtual-machines/constrained-vcpu.md)，预订费用将基于个 vcpu 的实际数量，而不是受约束的内核数。 对于 Standard_E32 8s_v3，预订费用将基于32核心。 

有关超高磁盘定价的详细信息，请参阅 [Azure 磁盘定价页](https://azure.microsoft.com/pricing/details/managed-disks/) 。

### <a name="azure-disk-reservation"></a>Azure 磁盘保留

磁盘保留是以折扣方式提前购买一年的磁盘存储的选项，降低了总成本。 购买磁盘保留时，请在目标区域中选择特定的磁盘 SKU，例如，10 P30 (1TiB) 美国东部2地区的高级 Ssd。 预订体验与保留虚拟机 (VM) 实例类似。 可以捆绑 VM 和磁盘保留以最大程度地节省费用。 目前，Azure 磁盘预订为高级 SSD Sku 提供一年承诺计划，从 P30 (1TiB) 到 P80 (32 TiB) 在所有生产区域中。 有关保留磁盘定价的详细信息，请参阅 [Azure 磁盘定价页](https://azure.microsoft.com/pricing/details/managed-disks/)。