---
title: 为 Azure 流分析作业设置监视警报
description: 本文介绍如何使用 Azure 门户为 Azure 流分析作业设置监视和警报。
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/26/2017
ms.openlocfilehash: 4c676ab3039a02a4fda27ab00312133e5de8077a
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2018
ms.locfileid: "47090960"
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>为 Azure 流分析作业设置警报
可以设置警报，以便在指标达到指定的条件时触发警报。 例如，可为如下条件设置警报：

`If there are zero input events in the last 5 minutes, send email notification to sa-admin@example.com`

可以通过门户对指标设置规则，也可以依据操作日志数据[通过编程方式](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a)进行配置。

## <a name="set-up-alerts-in-the-azure-portal"></a>在 Azure 门户中设置警报
1. 在 Azure 门户中，打开要为其创建警报的流分析作业。 

2. 在“作业”边栏选项卡中，单击“监视”部分。  

3. 在“指标”边栏选项卡中，单击“添加警报”命令。

      ![Azure 门户设置](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. 输入名称和描述。

5. 使用选择器定义需发送警报的条件。

6. 提供有关警报应发送到何处的信息。

      ![为 Azure 流分析作业设置警报](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

有关在 Azure 门户中配置警报的详细信息，请参阅[接收警报通知](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)。  


## <a name="get-help"></a>获取帮助
如需进一步的帮助，请尝试我们的 [Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>后续步骤
* [Azure 流分析简介](stream-analytics-introduction.md)
* [Azure 流分析入门](stream-analytics-get-started.md)
* [缩放 Azure 流分析作业](stream-analytics-scale-jobs.md)
* [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)

