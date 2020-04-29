---
title: Azure 安全中心中 IaaS 的高级数据安全性 |Microsoft Docs
description: " 了解如何在 Azure 安全中心为 IaaS 启用高级数据安全性。 "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2019
ms.author: memildin
ms.openlocfilehash: a2970ea3f5ad360deaedd7efc82154cd3bc50337
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "79282726"
---
# <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview"></a>Azure 虚拟机上 SQL server 的高级数据安全性（预览版）
Azure 虚拟机上的 SQL Server 高级数据安全是适用于高级 SQL 安全功能的统一包。 此预览功能包含用于识别和缓解潜在的数据库漏洞并检测可能指示数据库威胁的异常活动的功能。 

此适用于 Azure Vm 的安全产品/服务基于[AZURE Sql 数据库高级数据安全包](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)中使用的相同基础技术。


## <a name="overview"></a>概述

高级数据安全性提供一组高级 SQL 安全功能，其中包括漏洞评估和高级威胁防护。

* [漏洞评估](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment)是一项易于配置的服务，可以发现、跟踪并帮助修正潜在的数据库漏洞。 它提供安全状态的可见性，并包括用于解决安全问题并增强数据库 fortifications 的步骤。
* [高级威胁防护](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)可检测异常活动，这些活动表明访问或利用 SQL server 的异常和潜在有害尝试。 它会持续监视你的数据库中是否存在可疑活动，并针对异常数据库访问模式提供面向操作的安全警报。 这些警报提供可疑活动的详细信息和建议的操作来调查和缓解威胁。

## <a name="get-started-with-advanced-data-security-for-sql-on-azure-vms"></a>Azure Vm 上的 SQL 高级数据安全入门

以下步骤使你开始了解 Azure Vm 公共预览版中的 SQL 高级数据安全。

### <a name="set-up-advanced-data-security-for-sql-on-azure-vms"></a>在 Azure Vm 上设置 SQL 高级数据安全性

在订阅/工作区级别为虚拟机上的 SQL Server 启用高级数据安全：
 
1. 从安全中心边栏打开**定价 & 设置**"页。

1. 选择要为 Azure Vm 上的 SQL 启用高级数据安全的订阅或工作区。

1. 将**虚拟机上的 SQL server**的选项切换为 "已启用"。 

    （单击屏幕快照展开）

    [![Windows 管理中心中显示的安全中心建议和警报](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-small.png)](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-large.png#lightbox)

    将在连接到所选工作区的所有 SQL Server 或所选订阅的默认工作区中启用 SQL Server 高级数据安全。

    >[!NOTE]
    > 在首次重新启动 SQL Server 后，解决方案将处于完全活动状态。 

若要创建新的工作区，请按照[创建 Log Analytics 工作区](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)中的说明进行操作。

若要将 SQL Server 的主机连接到工作区，请按照[将 Windows 计算机连接到 Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows)中的说明进行操作。


## <a name="set-up-email-notification-for-security-alerts"></a>设置安全警报的电子邮件通知 

可以设置在生成安全中心警报时接收电子邮件通知的收件人列表。 电子邮件包含指向 Azure 安全中心中的警报的直接链接，其中包含所有相关的详细信息。 

1. 请参阅**安全中心** > **定价 & 设置**，并单击相关订阅

    ![订阅设置](./media/security-center-advanced-iaas-data/subscription-settings.png)

1. 从 "设置" 菜单中单击 "**电子邮件通知**"。 
1. 在 "**电子邮件地址**" 文本框中，输入接收通知的电子邮件地址。 您可以输入多个电子邮件地址，方法是使用逗号（，）分隔电子邮件地址。  例如admin1@mycompany.comadmin2@mycompany.com，admin3@mycompany.com

    ![电子邮件设置](./media/security-center-advanced-iaas-data/email-settings.png)

1. 在**电子邮件通知**设置中，设置以下选项：
  
    * **发送电子邮件通知中的高严重性警报**：不发送所有警报的电子邮件，只发送高严重性警报。
    * **同时向订阅所有者发送电子邮件通知：也**将通知发送给订阅所有者。

1. 在**电子邮件通知**屏幕的顶部，单击 "**保存**"。

  > [!NOTE]
  > 在关闭该窗口之前，请务必单击 "**保存**"，否则将不会保存新的**电子邮件通知**设置。

## <a name="explore-vulnerability-assessment-reports"></a>探索漏洞评估报告

漏洞评估仪表板概述了你的所有数据库中的评估结果。 您可以根据 SQL Server 版本来查看数据库的分布情况，还可以查看失败与传递数据库的摘要，以及根据风险分布的失败检查的总体摘要。

你可以直接从安全中心查看漏洞评估结果。

1. 在安全中心边栏下的 "资源安全" 下，选择 "**数据 & 存储**"。

1. 选择**应修正 vm 中的 SQL 数据库的建议漏洞（预览）**。 有关详细信息，请参阅[安全中心建议](security-center-recommendations.md)。 

    [![* * Vm 中的 SQL 数据库上的漏洞应修正（预览版） * * 建议](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png)](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png#lightbox)

    此建议的详细视图随即出现。

    [![应修正 Vm 中 SQL 数据库的 * * 漏洞的详细视图（预览版） * * 建议](media/security-center-advanced-iaas-data/all-servers-view.png)](media/security-center-advanced-iaas-data/all-servers-view.png#lightbox)

1. 若要深入了解更多详细信息：

    * 有关扫描的资源（数据库）和已测试的安全检查列表的概述，请单击相关服务器。
    [![按 SQL server 分组的漏洞](media/security-center-advanced-iaas-data/single-server-view.png)](media/security-center-advanced-iaas-data/single-server-view.png#lightbox)

    * 有关按特定 SQL 数据库分组的漏洞的概述，请单击相关数据库。
    [![按 SQL server 分组的漏洞](media/security-center-advanced-iaas-data/single-database-view.png)](media/security-center-advanced-iaas-data/single-database-view.png#lightbox)

    在每个视图中，安全检查按**严重性**排序。 单击特定安全检查可查看详细信息窗格，其中包含**说明**、**纠正**方法和其他相关信息（如**影响**或**基准**）。

## <a name="advanced-threat-protection-for-sql-servers-on-azure-vms-alerts"></a>针对 Azure Vm 上的 SQL server 的高级威胁防护警报
警报由异常和可能有害的访问或利用 SQL Server 的尝试生成。 这些事件可以触发以下警报：

### <a name="anomalous-access-pattern-alerts-preview"></a>异常访问模式警报（预览）

* **从异常位置访问：** 如果对 SQL server 的访问模式发生更改，而该用户在不正常的地理位置登录到 SQL server，则会触发此警报。 可能的原因：
    * 攻击者或以前的恶意使用已访问你的 SQL Server。
    * 合法用户已从新位置访问了你的 SQL Server。
* **来自可能有害的应用程序的访问**：当使用可能有害的应用程序访问数据库时，会触发此警报。 可能的原因：
    * 攻击者尝试使用常见攻击工具破坏您的 SQL。
    * 合法的渗透测试。
* **来自陌生主体的访问**：当 SQL Server 的访问模式发生更改，有人使用异常的主体（SQL 用户）登录到 SQL Server 时，会触发此警报。 可能的原因：
    * 攻击者或以前的恶意使用已访问你的 SQL Server。 
    * 合法用户已使用新主体从访问了你的 SQL Server。
* **暴力破解 SQL 凭据**：当有人使用不同的凭据异常登录并失败很多次时，会触发此警报。 可能的原因：
    * 攻击者尝试使用暴力破解来破坏您的 SQL。
    * 合法的渗透测试。

### <a name="potential-sql-injection-attacks-supported-in-sql-server-2019"></a>潜在的 SQL 注入攻击（SQL Server 2019 中支持）

* **存在易受 SQL 注入攻击的漏洞**：当某个应用程序在数据库中生成错误的 SQL 语句时，会触发此警报。 此警报会指示可能存在易受 SQL 注入攻击的漏洞。 可能的原因：
    * 应用程序代码中的缺陷导致构造出错误的 SQL 语句
    * 应用程序代码或存储过程在构造错误的 SQL 语句时无法清理用户输入，使该语句被 SQL 注入攻击利用
* **潜在 SQL 注入**：当 SQL 攻击有效利用已识别到的应用程序漏洞时，会触发此警报。 这意味着，攻击者正在尝试使用有漏洞的应用程序代码或存储过程注入恶意 SQL 语句。


### <a name="unsafe-command-supported-in-sql-server-2019"></a>Unsafe 命令（在 SQL Server 2019 中受支持）

* **可能不安全的操作**：执行具有高特权且可能不安全的命令时，将触发此警报。 可能的原因：
    * 建议禁用此命令以获得更好的安全状况。
    * 攻击者尝试利用 SQL 访问或提升权限。   


## <a name="explore-and-investigate-security-alerts"></a>探索并调查安全警报

可以在安全中心的 "警报" 页、资源的 "安全" 选项卡或通过警报电子邮件中的直接链接获取数据安全警报。

1. 查看警报：

    * 在 "安全中心" 中，单击侧栏中的 "**安全警报**"，然后选择一个警报。
    * 在资源范围中-打开相关资源页，并从侧栏中单击 "**安全性**"。 

1. 警报被设计为自包含，其中每个警报都有详细的补救步骤和调查信息。 可以使用其他 Azure 安全中心和 Azure Sentinel 功能进一步进行调查，以获得更广泛的视图：

    * 启用 SQL Server 的审核功能以进行进一步调查。 如果你是 Azure Sentinel 用户，则可以将 SQL 审核日志从 Windows 安全日志事件上传到 Sentinel，并享受丰富的调查体验。
    * 若要改善安全状况，请使用安全中心针对每个警报中所指示的主机的建议。 这将降低未来攻击的风险。 


## <a name="next-steps"></a>后续步骤

有关相关材料，请参阅以下文章：

- [如何修正建议](security-center-remediate-recommendations.md)