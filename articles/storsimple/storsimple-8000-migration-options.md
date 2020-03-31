---
title: 来自 StorSimple 5000-7000 系列设备的数据迁移选项
description: 概述用于从 StorSimple 5000-7000 系列迁移数据的选项。
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 44a05ecb273bdf7582300c3b6a9110e2ada0994c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471816"
---
# <a name="options-to-migrate-data-from-storsimple-5000-7000-series"></a>用于从 StorSimple 5000-7000 系列迁移数据的选项 

> [!IMPORTANT]
> 2019 年 7 月 9 日，StorSimple 5000/7000 系列将到达支持 （EOS） 状态结束。 我们建议 StorSimple 5000/7000 系列客户迁移到本文档中所述的替代方案之一。

StorSimple 5000-7000 系列将在 2019 年 7 月[终止支持](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series)。 正在运行 StorSimple 5000-7000 系列的客户可以选择升级到其他 Azure 第一方混合服务。 本文介绍可用于迁移数据的 Azure 混合选项。 

## <a name="migration-options"></a>迁移选项

使用 StorSimple 5000-7000 系列的客户可以选择 Azure 或第三方选项。

### <a name="azure-options"></a>Azure 选项

#### <a name="upgrade-to-storsimple-8000-series"></a>升级到 StorSimple 8000 系列

升级到 StorSimple 8000 系列，从而继续使用 StorSimple 平台。  此升级路径要求客户将其 5000-7000 系列设备替换为 8000 系列。 使用迁移工具从 5000-7000 系列设备迁移数据。 成功完成迁移后，StorSimple 8000 系列设备将继续在 Azure Blob 存储中将数据分层。 

有关如何使用 StorSimple 8000 系列迁移数据的详细信息，请参阅[将数据从 StorSimple 5000-7000 系列迁移到 8000 系列设备](storsimple-8000-migrate-from-5000-7000.md)。

#### <a name="migrate-to-azure-file-sync"></a>迁移到 Azure 文件同步

此全新迁移选项可让客户在 Azure 文件中存储其组织的文件共享。 然后可以集中这些文件共享，以使用 Azure 文件同步 (AFS) 进行本地访问。 可在 Windows Server 主机上部署 AFS。 然后，可以通过主机复制或迁移工具执行实际的数据迁移。

有关如何将数据迁移到 Azure 文件同步的详细信息，请参阅[将数据从 StorSimple 5000-7000 系列迁移到 Azure 文件同步](storsimple-5000-7000-afs-migration.md)。

### <a name="third-party-options"></a>第三方选项

#### <a name="migrate-to-panzura-freedom-nas"></a>迁移到 Panzura Freedom NAS

StorSimple 5000-7000客户可以选择迁移到 Panzura Freedom NAS 以将他们的数据保存在 Azure 中。 Panzura Freedom 解决方案提供跨越数据中心、办公室、公有云和私有云的 NAS 解决方案。 该解决方案支持 NFS、SMB 和移动客户端的本地、混合和云内数据工作流。 

Panzura 支持此迁移，客户可以通过在 [Panzura 网站](https://panzura.com/storsimple-migration/)请求迁移支持来开始。

#### <a name="migrate-to-cohesity"></a>迁移到 Cohesity

Cohesity 使你可以将数据从当前 StorSimple 5000–7000 迁移到 Azure 上的 Cohesity 数据平台。 Cohesity 数据平台是软件定义的 Web 规模解决方案，将文件、备份、对象和 VM 合并到单个云本机解决方案中。 迁移到该数据平台之后，可以通过单一平台来管理、保护和预配数据和应用（从云到核心）。 使用 Cohesity 时，最少只需从三个节点开始。 

了解有关迁移到[Cohesity 数据平台](https://info.cohesity.com/migrate-from-storsimple-to-cohesity.html)的更多信息。

#### <a name="migrate-to-nasuni"></a>迁移到纳苏尼

Nasuni 使 StorSimple 5000-7000 客户能够轻松迁移并将数据保存在 Azure 中。  Nasuni 是一家领先的基于 Azure 的 NAS 存储解决方案，通过云经济和扩展功能为客户提供他们对前置解决方案的期望性能和安全性。  除了高性能文件存储外，Nasuni 和 Azure 还处理备份和 DR，同时允许您通过集中式文件存储管理在全球共享和协作数据。 

Nasuni 拥有让您轻松迁移的经验 – 立即开始：https://info.nasuni.com/nasuni-storsimple-migration

#### <a name="migrate-to-talon-fast"></a>迁移到塔隆 FAST

Talon 使 StorSimple 5000-7000 客户能够轻松地继续利用他们在 StorSimple 平台（由无限云资源支持的少量现场占用空间）中所珍视的优势，从而拥有更大的功能。  借助 Talon FAST 解决方案，客户可以在 Azure 中迁移和保留其数据，而现在，客户拥有更小的仅软件现场占用空间，并添加全局文件锁定、全局命名空间和多站点协作等优势。  Talon 是领先的 Azure 生态系统解决方案，它与全球客户合作，在不损害用户工作流或体验的情况下，将其本地文件服务器工作负载迁移到基于 Azure 的整合占用空间中。  

详细了解如何在 中https://www.talonstorage.com/alliances/microsoft-storsimple发展成为云整合企业。


## <a name="migration---frequently-asked-questions"></a>迁移 - 常见问题解答

### <a name="q-when-do-the-storsimple-5000-and-7000-series-devices-reach-end-of-service"></a>Q. StorSimple 5000 和 7000 系列设备何时退役？ 

A. StorSimple 5000-7000 系列将在 2019 年 7 月[退役](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series)。 “退役”意味着在 2019 年 7 月以后， Microsoft 不再能够为这些设备的硬件和软件提供支持。 我们强烈建议立即开始制定计划，以从这些设备迁移数据。

### <a name="q-what-happens-to-the-data-i-have-stored-in-azure"></a>Q. 在 Azure 中存储的数据会发生什么情况？  

A. 迁移到新服务后，可以继续使用 Azure 中的数据。 


### <a name="q-what-happens-to-the-data-i-have-stored-locally-on-my-storsimple-device"></a>Q. StorSimple 设备本地存储的数据会发生什么情况？ 

A. 可根据迁移文档中所述，将本地设备上的数据复制到新服务。

### <a name="q-what-happens-if-i-want-to-keep-my-storsimple-50007000-series-appliance"></a>Q. 如果保留 StorSimple 5000/7000 系列设备，会发生什么情况？ 

A. 尽管服务可以继续工作，但 Microsoft 不再能够提供硬件和软件支持。 为实现业务连续性，我们强烈建议迁移。

### <a name="q-what-options-are-available-to-migrate-data-from-storsimple-5000-7000-series-devices"></a>Q. 可以使用哪些选项从 StorSimple 5000-7000 系列设备迁移数据？ 

A. 根据具体的方案，StorSimple 5000-7000 系列用户可以使用以下迁移选项。 

 - **升级到 8000 系列**：若要继续在 StorSimple 平台上工作，请使用此选项。 
 - **迁移到 Azure 文件同步**：若要切换到 Azure 本机格式，请使用此选项。 可以使用 Azure 文件同步来集中管理文件共享。 

对于此处未列出的迁移选项，请咨询 Microsoft 支持人员。

### <a name="q-is-migration-to-other-storage-solutions-supported"></a>Q. 是否支持迁移到其他存储解决方案？

A. 是的。 支持迁移到使用数据主机复制的其他存储解决方案。

### <a name="q-is-migration-supported-by-microsoft"></a>Q. Microsoft 是否为迁移提供支持？ 

A. 从 5000 或 7000 系列迁移数据完全受到 Microsoft 的支持。 事实上，Microsoft 建议在开始迁移之前联系支持人员。 迁移当前是一项辅助的操作。 如果想要从 StorSimple 5000-7000 系列设备迁移数据，请[开具支持票证](storsimple-8000-contact-microsoft-support.md)。

### <a name="q-what-is-the-pricing-model-for-both-the-migration-options"></a>Q. 两个迁移选项的定价模型是什么？

A. 迁移成本根据选择的选项而异。 尽管迁移本身是免费的，但如果你决定升级到 StorSimple 8000 系列，则会产生硬件设备成本。 

同样，在使用 Azure 文件同步时，可能会产生服务的订阅费用。 对于每种情况，客户还需要持续支付存储费用。 请参阅以下内容进行估算： 
- [StorSimple 定价](https://azure.microsoft.com/pricing/details/storsimple/)  
- [AFS 定价]( https://azure.microsoft.com/pricing/details/storage/files/)

### <a name="q--how-long-does-it-take-to-complete-a-migration"></a>Q.  完成迁移需要多长时间？

A. 迁移数据所需的时间取决于数据量和选择的升级选项。 

### <a name="q-what-is-the-end-of-support-date-for-storsimple-8000-series"></a>Q. StorSimple 8000 系列的终止支持日期是何时？

A. StorSimple 8000 系列的终止支持日期在[此处](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series)发布。


## <a name="next-steps"></a>后续步骤
 - [将数据从 StorSimple 5000-7000 系列迁移到 8000 系列设备](storsimple-8000-migrate-from-5000-7000.md)。
 - [将数据从 StorSimple 5000-7000 系列迁移到 Azure 文件同步](storsimple-5000-7000-afs-migration.md)
