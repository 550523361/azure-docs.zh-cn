---
title: 更新 Azure 备份中心报表内容包
description: 介绍如何更新 Power BI 中的 Azure 备份内容包
services: backup
documentationcenter: ''
author: adigan
manager: shivamg
editor: ''
ms.assetid: 59df5bec-d959-457d-8731-7b20f7f1013e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/24/2018
ms.author: adigan
ms.openlocfilehash: 65ab497b2467846a324d10f8487ab384a01441ee
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968879"
---
# <a name="update-the-azure-backup-central-reporting-content-pack"></a>更新 Azure 备份中心报表内容包 

[Azure 备份内容包](https://docs.microsoft.com/azure/backup/backup-azure-configure-reports#view-reports-in-power-bi)可用于查看关于集中备份的报表。 内容包定期更新，以添加更多的功能并修复 Bug。 本文介绍如何更新内容包。 还介绍如何推迟更新并查看按时间顺序所做的更新。

## <a name="get-updates-to-the-content-pack"></a>获取内容包的更新

### <a name="get-the-updated-content-pack"></a>获取更新的内容包
如果未对内容包副本进行任何更改，则其它将自动更新。 内容包更改时，你将在 Power BI 中收到通知和一封电子邮件通知。 可选择在方便时获取更新的内容包。 

### <a name="postpone-the-update"></a>推迟更新
最佳做法是将内容包导入到[自定义工作区](https://youtu.be/26zyOtyHPJM?t=1m57s)。 现在可以编辑此报表。
如上所述，如果内容包更改，则将在 Power BI 中看到通知。 可以选择稍后获取内容包。 

## <a name="coming-soon"></a>即将支持
   
更新 Azure 备份内容包以支持更多的工作负载。 工作负载包括用于 IaaS VM 备份的 Azure SQL 数据库和 System Center Data Protection Manager。 此支持将添加到 Azure 备份和 Azure VM 备份的当前支持中。 这种支持意味着你可以在一个中心位置查看和分析所有备份数据。 [也可自定义报表](https://youtu.be/26zyOtyHPJM)以满足组织的需求。

正在对 Azure 备份内容包随附的预配置报表进行更改。 这些更改可使报表在各个工作负载中更有意义。 可在下方抢先了解即将推出的一组报表。

### <a name="summary"></a>摘要
   
![摘要](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Summary.png)

### <a name="billing"></a>计费

![计费](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Billing.png)

### <a name="compliance"></a>合规性

![合规性](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Compliance.png)

### <a name="storage"></a>存储

![存储](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Storage.png)

### <a name="backup-items"></a>备份项
![备份项](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-BackupItem.png)

### <a name="alerts"></a>警报

![警报](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Alerts.png)

### <a name="jobs"></a>作业

![作业](./media/backup-azure-central-reporting/AzBackup-Central-Reporting-Jobs.png)
    

## <a name="next-steps"></a>后续步骤

* [在整个组织中共享报表](https://youtu.be/26zyOtyHPJM)
* [Azure 备份常见问题](backup-azure-backup-faq.md)
