---
title: 无法登录到 Azure HDInsight 群集
description: 解决无法登录到 Azure HDInsight 中的 Apache Hadoop 群集的原因
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 2099d1b7583017733498946a5866ab37de43ba9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894054"
---
# <a name="scenario-unable-to-log-into-azure-hdinsight-cluster"></a>方案：无法登录到 Azure HDInsight 群集

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

无法登录到 Azure HDInsight 群集。

## <a name="cause"></a>原因

原因可能有所不同。 请记住，在登录到群集或应用仪表板时，请使用“群集登录名”或 HTTP 凭据。 从远程连接时，请使用安全外壳 (SSH) 或远程桌面凭据。

## <a name="resolution"></a>解决方法

若要解决常见问题，请尝试下面的一个或多个步骤。

* 尝试在新的浏览器选项卡中以隐私模式打开群集仪表板。

* 如果忘记了 SSH 凭据，可以[在 Ambari UI 中重置凭据](../hdinsight-administer-use-portal-linux.md#change-passwords)。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* 通过 [Azure 社区支持](https://azure.microsoft.com/support/community/)获取 Azure 专家的解答。

* 与[@AzureSupport](https://twitter.com/azuresupport)- 正式的 Microsoft Azure 帐户连接 Azure 社区，以将 Azure 社区连接到正确的资源：答案、支持和专家，从而改善客户体验。

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”****，或打开“帮助 + 支持”**** 中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
