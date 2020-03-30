---
title: 在 Azure 安全中心应用系统更新 | Microsoft 文档
description: 本文档演示如何实现 Azure 安全中心建议**应用系统更新**和**在系统更新后重启**。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: 3f27753b0775f44cbdf9d4c478a19e423b8e1f19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604549"
---
# <a name="apply-system-updates-in-azure-security-center"></a>在 Azure 安全中心应用系统更新
Azure 安全中心每天对 Windows、Linux 虚拟机 (VM) 和计算机进行监控，以找出缺少的操作系统更新。 安全中心从 Windows 更新或 Windows Server Update Services (WSUS) 检索可用的安全更新和关键更新的列表，具体取决于 Windows 计算机上配置的服务。 安全中心还可以在 Linux 系统中检查最新更新。 如果 VM 或计算机缺少系统更新，安全中心将建议应用系统更新。

## <a name="implement-the-recommendation"></a>实现该建议
应用系统更新在安全中心中显示为建议。 如果 VM 或计算机缺少系统更新，此建议将在“建议”**** 和“计算”**** 下方显示。  选择该建议将打开“应用系统更新”**** 仪表板。

本示例使用“计算”****。

1. 选择安全中心主菜单下的“计算”****。

   ![选择“计算”][1]

2. 在“计算”**** 下，选择“缺少系统更新”****。 “应用系统更新”**** 仪表板将打开。

   ![“应用系统更新”仪表板][2]

   仪表板顶部显示：

    - 缺少系统更新的 Windows、Linux VM 以及计算机的总数。
    - VM 和计算机中缺少的关键更新的总数。
    - VM 和计算机中缺少的安全更新的总数。

   仪表板底部列出了 VM 和计算机中缺少的所有更新以及缺少更新的严重性。  此列表包括：

    - 名称：所缺更新的名称。
    - VM 和 计算机数：缺少此更新的 VM 和计算机的总数。
    - 状态：该建议的当前状态：

      - 未解决：建议尚未得到处理。
      - 正在进行：目前已将建议应用到相关资源，无需用户采取行动。
      - 已解决：已完成建议。 （解决问题后，此条目将变暗）。

    - 严重性：描述该特定建议的严重性：

      - 高：重要资源（应用程序、虚拟机或网络安全组）存在漏洞，需要引起注意。
      - 中：需要采取非关键步骤或额外步骤来完成某个过程或消除某个漏洞。
      - 低：漏洞需要解决，但不需立即处理。 （默认情况下，不显示严重性低的建议，但如果用户需要查看这些建议，可以将其筛选出来。）

3. 选择列表中的缺少更新以查看详细信息。

   ![缺少的安全更新][3]

4. 选择顶部功能区中的“搜索”**** 图标。  Azure 监视器日志搜索查询将筛选到缺少更新的计算机。

   ![Azure 监视器日志搜索][4]

5. 选择列表中的计算机，以了解详细信息。 此时，将会打开另一个搜索结果，其中筛选出了相应计算机的信息。

    ![Azure 监视器日志搜索][5]

## <a name="next-steps"></a>后续步骤
若要了解有关安全中心的详细信息，请参阅以下文章：

* [在 Azure 安全中心设置安全策略](tutorial-security-policy.md)-- 了解如何为 Azure 订阅和资源组配置安全策略。
* [在 Azure 安全中心管理安全建议](security-center-recommendations.md)-- 了解建议如何帮助保护 Azure 资源。
* [Azure 安全中心中的安全运行状况监视](security-center-monitoring.md)-- 了解如何监视 Azure 资源的运行状况。
* [管理和响应 Azure 安全中心中的安全警报](security-center-managing-and-responding-alerts.md)-- 了解如何管理和响应安全警报。
* [使用 Azure 安全中心监视合作伙伴解决方案](security-center-partner-solutions.md)-- 了解如何监视合作伙伴解决方案的运行状况。
* [Azure 安全博客](https://blogs.msdn.com/b/azuresecurity/)-- 查找有关 Azure 安全性和合规性的博客文章。

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/missing-system-updates.png
[2]:./media/security-center-apply-system-updates/apply-system-updates.png
[3]: ./media/security-center-apply-system-updates/detail-on-missing-update.png
[4]: ./media/security-center-apply-system-updates/log-search.png
[5]: ./media/security-center-apply-system-updates/search-details.png
