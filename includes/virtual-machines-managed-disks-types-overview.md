---
title: include 文件
description: include 文件
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/03/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: dbca8798af83108ca5e8bdfe5918a25423fd934d
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86544946"
---
Azure 托管磁盘当前提供四种磁盘类型，每种类型都面向特定的客户方案。

## <a name="disk-comparison"></a>磁盘比较

下表提供了对托管磁盘的 ultra 磁盘、高级固态硬盘（SSD）、标准 SSD 和标准硬盘驱动器（HDD）的比较，有助于你决定要使用的内容。

| 详细信息 | 超级磁盘 | 高级 SSD | 标准 SSD | 标准 HDD |
| ------ | ---------- | ----------- | ------------ | ------------ |
|磁盘类型   |SSD   |SSD   |SSD   |HDD   |
|方案   |IO 密集型工作负荷，如[SAP HANA](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)、顶级数据库（例如 SQL、Oracle）和其他事务密集型工作负荷。   |生产和性能敏感型工作负荷   |Web 服务器、不常使用的企业应用程序和开发/测试   |备份、非关键、不常访问   |
|最大磁盘大小   |65,536 GB (GiB)    |32,767 GiB    |32,767 GiB   |32,767 GiB   |
|最大吞吐量   |2,000 MB/秒    |900 MB/秒   |750 MB/秒   |500 MB/秒   |
|最大 IOPS   |160,000    |20,000   |6,000   |2,000   |

## <a name="ultra-disk"></a>超级磁盘

Azure 超级磁盘为 Azure IaaS VM 提供高吞吐量、高 IOPS 和一贯低延迟的磁盘存储。 超磁盘的其他好处包括能够动态地更改磁盘的性能和工作负荷，而无需重新启动虚拟机（VM）。 超级磁盘适用于 SAP HANA、顶层数据库等数据密集型工作负荷，以及事务密集型工作负荷。 超级磁盘只可用作数据磁盘。 建议将高级 SSD 用作 OS 磁盘。

### <a name="performance"></a>性能

预配超级磁盘时，可以单独配置磁盘的容量和性能。 超磁盘具有多种固定大小，范围从 4 GiB 到 64 TiB，并提供灵活的性能配置模型，使你能够独立配置 IOPS 和吞吐量。

超磁盘的一些关键功能是：

- 磁盘容量：超小型磁盘容量范围为 4 GiB 至 64 TiB。
- 磁盘 IOPS： Ultra 磁盘支持 300 IOPS/GiB 的 IOPS 限制，每个磁盘最多可达 160 K IOPS。 若要实现预配的 IOPS，请确保所选磁盘 IOPS 小于 VM IOPS 限制。 每个磁盘保证的最小 IOPS 是 2 IOPS/GiB，整体基线至少为 100 IOPS。 例如，如果有4个 GiB 的超磁盘，则至少需要 100 IOPS，而不是 8 IOPS。
- 磁盘吞吐量：对于超磁盘，单个磁盘的吞吐量限制为每个预配 IOPS 256 KiB/s，每个磁盘最大可达 2000 MBps （其中 Mbits = 10 ^ 6 字节/秒）。 对于每个预配的 IOPS，每个磁盘的最小保证吞吐量为 4KiB/s，整体基线最小为 1 MBps。
- 超磁盘支持在运行时调整磁盘性能属性（IOPS 和吞吐量），而无需从虚拟机中分离该磁盘。 在对磁盘上发出磁盘性能调整大小操作后，更改实际上可能最多需要一个小时才能生效。 24小时内，在24小时内调整操作的性能限制为四个。 性能调整大小操作可能会因为缺乏性能带宽容量而失败。

### <a name="disk-size"></a>磁盘大小

|磁盘大小 (GiB)  |IOPS 上限  |吞吐量上限 (MBps)  |
|---------|---------|---------|
|4     |1,200         |300         |
|8     |2,400         |600         |
|16     |4,800         |1,200         |
|32     |9,600         |2,000         |
|64     |19,200         |2,000         |
|128     |38,400         |2,000         |
|256     |76,800         |2,000         |
|512     |80,000         |2,000         |
|1,024 - 65,536（此范围内的大小以 1 TiB 为增量递增）     |160,000         |2,000         |

### <a name="ga-scope-and-limitations"></a>GA 范围和限制

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](managed-disks-ultra-disks-GA-scope-and-limitations.md)]
