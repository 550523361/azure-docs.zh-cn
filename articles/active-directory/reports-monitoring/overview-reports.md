---
title: 什么是 Azure Active Directory 报表？ | Microsoft Docs
description: 简要介绍 Azure Active Directory 报表。
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 220f27ccf2d3eaefd8347e1d52824be2d601d9c9
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364368"
---
# <a name="what-are-azure-active-directory-reports"></a>什么是 Azure Active Directory 报表？

借助 Azure Active Directory 报表，可深入了解环境当前的情况。  
可以将提供的数据用于：

- 确定用户如何使用你的应用和服务
- 检测影响环境运行状况的潜在风险
- 排查妨碍用户完成其工作的问题  

报告体系结构依赖于两大支柱：

- 安全报表
- 活动报表

![报告](./media/overview-reports/01.png)


## <a name="security-reports"></a>安全报表

Azure Active Directory 中的安全报表有助于保护组织的标识。  
Azure Active Directory 中有两种类型的安全报表：

- 标记为存在风险的用户 - 可以通过[标记为存在风险的用户的安全报表](concept-user-at-risk.md)大致了解可能已遭到入侵的用户帐户。

- 风险登录 - 可以通过[风险登录安全报表](concept-risky-sign-ins.md)了解可能由非用户帐户合法拥有者进行的登录尝试。 

访问安全报表需要什么 Azure AD 许可证？  

所有版本的 Azure Active Directory 都提供标记为存在风险的用户的报表和风险登录报表。  
但是，各版本的报表粒度级别有所不同： 

- 在“Azure Active Directory 免费版和基本版”中，你已经获得了一个列表，其中包含标记为存在风险的用户和风险登录。 

- Azure Active Directory Premium 1 版本还允许你检查每个报表中检测到的部分潜在性风险事件，从而扩展了该模型。 

- Azure Active Directory Premium 2 版本提供有关潜在风险事件的最详细信息，并且还允许配置可自动响应已配置风险级别的安全策略。


## <a name="activity-reports"></a>活动报表

Azure Active Directory 中有两种类型的活动报表：

- 审核日志 - 可以通过[审核日志活动报表](concept-audit-logs.md)访问在租户中执行的每个任务的历史记录。

- 登录 -  可以通过[登录活动报表](concept-sign-ins.md)来确定谁执行了审核日志报表所报告的任务。


审核日志报表提供系统活动记录以确保符合性。 可通过此数据处理常见方案，例如：

- 我的租户中有人获得了访问管理员组的权限。 谁给予他们访问权限？ 

- 我想要了解登录到特定应用的用户的列表，因为我最近将该应用上架了，想要了解其是否正常运行

- 我想要知道在我的租户中进行了多少次密码重置


访问审核日志报表需要什么 Azure AD 许可证？  

审核日志报表适用于你有许可证的功能。 如果有特定功能的许可证，则还可以访问其审核日志信息。

有关更多详细信息，请参阅 [Azure Active Directory 特性和功能](https://www.microsoft.com/cloud-platform/azure-active-directory-features)中的“比较免费版、基本版和高级版中正式推出的功能”。   


可以通过“登录活动报告”找到如下所示问题的答案：

- 什么是用户的登录模式？
- 多少用户超过一周都有登录行为？
- 这些登录的状态怎样？


访问登录活动报表需要什么 Azure AD 许可证？  

若要访问登录活动报表，租户必须具有与之关联的 Azure AD Premium 许可证。

## <a name="programmatic-access"></a>以编程方式访问

除了用户界面，Azure Active Directory 报告还允许[以编程方式访问](concept-reporting-api.md)报告数据。 对于应用程序（如 SIEM 系统、审核）和商业智能工具，这些报告的数据可能非常有用。 Azure AD 报告 API 通过一组基于 REST 的 API，可提供对该数据的编程访问权限。 可从各种编程语言和工具中调用这些 API。 


## <a name="next-steps"></a>后续步骤

- [风险登录报表](concept-risky-sign-ins.md)
- [审核日志报表](concept-audit-logs.md)
- [登录日志报表](concept-sign-ins.md)