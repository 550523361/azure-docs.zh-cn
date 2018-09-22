---
title: 保护 Azure 安全中心的 Azure SQL 服务和数据 | Microsoft Docs
description: 本文档介绍了 Azure 安全中心提供的建议，这些建议有助于保护数据和 Azure SQL 服务，并遵守安全策略。
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 45f5dc840f015793912e314ab3d47e54a409708e
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "46126660"
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>在 Azure 安全中心保护 Azure SQL 服务和数据
Azure 安全中心可分析 Azure 资源的安全状态。 在安全中心识别潜在的安全漏洞时，它会创建一些建议，这些建议会指导完成配置所需控件的过程。  适用于 Azure 资源类型的建议：虚拟机 (VM)、网络、SQL 和数据，以及应用程序。

本文介绍适用于 Azure SQL 服务和数据的建议。 启用 Azure SQL 服务器和数据库审核、启用 SQL 数据库加密和启用 Azure 存储帐户加密的建议中心。  使用下表作为参考，以便了解可用的 SQL 服务和数据的建议，以及应用建议后每个建议的做法。
### <a name="monitor-data-security"></a>监视数据安全性

单击“预防”部分中的“数据安全性”时，“数据资源”会打开，其中显示了有关 SQL 和存储的建议。 此外还有针对数据库常规运行状况的[建议](security-center-sql-service-recommendations.md)。 有关存储加密的详细信息，请阅读 [Enable encryption for Azure storage account in Azure Security Center](security-center-enable-encryption-for-storage-account.md)（在 Azure 安全中心为 Azure 存储帐户启用加密）。

![数据资源](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

在“SQL 建议”下面，可以单击任一建议，获取有关进一步操作的更多详细信息来解决问题。 以下示例显示了“SQL 数据库的数据库审核与威胁检测”建议的补充说明。

![有关 SQL 建议的详细信息](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

“对 SQL 数据库启用审核与威胁检测”包含以下信息：

* SQL 数据库列表
* 所在的服务器
* 此设置是从服务器继承的，还是此数据库独有的
* 当前状态
* 问题的严重性

单击数据库处理建议时，会打开“审核和威胁检测”，如以下屏幕截图所示。

![审核和威胁检测](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

若要启用审核，请在“审核”选项下选择“启用”。

## <a name="available-sql-service-and-data-recommendations"></a>可用的 SQL 服务和数据的建议
| 建议 | Description |
| --- | --- |
| [在 SQL 服务器上启用审核与威胁检测](security-center-enable-auditing-on-sql-servers.md) |建议启用 Azure SQL 服务器审核和威胁检测（仅限 Azure SQL 服务；不包括虚拟机上运行的 SQL）。 |
| [在 SQL 数据库上启用审核与威胁检测](security-center-enable-auditing-on-sql-databases.md) |建议启用 Azure SQL 数据库审核和威胁检测（仅限 Azure SQL 服务；不包括虚拟机上运行的 SQL）。 |
| [启用 SQL 数据库的透明数据加密](security-center-enable-transparent-data-encryption.md) |建议启用 SQL 数据库加密（仅限 Azure SQL 服务）。 |

## <a name="see-also"></a>另请参阅
若要了解有关适用于其他 Azure 资源类型的建议的详细信息，请参阅以下内容：

* [保护 Azure 安全中心中的虚拟机](security-center-virtual-machine-recommendations.md)
* [保护 Azure 安全中心中的应用程序](security-center-application-recommendations.md)
* [保护 Azure 安全中心中的网络](security-center-network-recommendations.md)

若要了解有关安全中心的详细信息，请参阅以下文章：

* [在 Azure 安全中心中设置安全策略](security-center-policies.md) - 了解如何配置 Azure 订阅和资源组的安全策略。
* [管理和响应 Azure 安全中心的安全警报](security-center-managing-and-responding-alerts.md) -- 了解如何管理和响应安全警报。
* [Azure 安全中心常见问题](security-center-faq.md) - 查找有关使用服务的常见问题。
