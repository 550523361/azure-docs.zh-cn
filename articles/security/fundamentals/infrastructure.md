---
title: Azure 基础结构安全性 | Microsoft Docs
description: 本文介绍 Microsoft 如何保护我们的 Azure 数据中心。
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: a17d98d49d2c653e2498a663829d26e8a171fd74
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2019
ms.locfileid: "72433507"
---
# <a name="azure-infrastructure-security"></a>Azure 基础结构安全性
Microsoft Azure 在由 Microsoft 管理和运营的数据中心运行。 这些地理位置分散的数据中心符合有关安全性和可靠性的关键行业标准，例如 ISO/IEC 27001:2013 和 NIST SP 800-53。 它们由 Microsoft 运营人员进行管理和监控。 这些运营人员在提供全球最大的全天候持续运行的联机服务方面拥有多年经验。

本系列文章提供了有关 Microsoft 如何保护 Azure 基础结构的信息。 文章讨论了：

- [物理安全性](physical-security.md)
- [可用性](infrastructure-availability.md)
- [组件和边界](infrastructure-components.md)
- [网络体系结构](infrastructure-network.md)
- [生产网络](production-network.md)
- [SQL 数据库](infrastructure-sql.md)
- [操作](infrastructure-operations.md)
- [监视](infrastructure-monitoring.md)
- [完整性](infrastructure-integrity.md)
- [数据保护](protection-customer-data.md)

## <a name="shared-responsibility-model"></a>共担责任模型
请务必了解你与 Microsoft 之间的责任分工。 在本地，你拥有整个堆栈，但迁移到云后，某些责任将转移到 Microsoft。 下图说明了根据堆栈部署类型（软件即服务 [SaaS]、平台即服务 [PaaS]、基础结构即服务 [IaaS] 和本地）划分的职责范围。

![显示职责的图形](./media/infrastructure/responsibility-zones.png)

无论部署类型如何，你始终负责以下内容：

- 数据
- 终结点
- 帐户
- 访问管理

确保了解你与 Microsoft 在 SaaS、PaaS 和 IaaS 部署中的责任分工。 有关详细信息，请参阅[云计算的责任分担](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/225237/1/Shared%20Responsibilities%20for%20Cloud%20Computing%20(2017-04-03).pdf)。

## <a name="next-steps"></a>后续步骤
若要详细了解 Microsoft 如何帮助保护 Azure 基础结构，请参阅：

- [Azure 设施、场地和物理安全性](physical-security.md)
- [Azure 基础结构可用性](infrastructure-availability.md)
- [Azure 信息系统的组件和边界](infrastructure-components.md)
- [Azure 网络体系结构](infrastructure-network.md)
- [Azure 生产网络](production-network.md)
- [Azure SQL 数据库安全功能](infrastructure-sql.md)
- [Azure 生产操作和管理](infrastructure-operations.md)
- [Azure 基础结构监视](infrastructure-monitoring.md)
- [Azure 基础结构完整性](infrastructure-integrity.md)
- [Azure 客户数据保护](protection-customer-data.md)


