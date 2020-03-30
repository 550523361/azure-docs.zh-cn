---
title: Azure Active Directory 报告常见问题解答 | Microsoft Docs
description: 围绕 Azure 活动目录报表的常见问题。
services: active-directory
documentationcenter: ''
author: cawrites
manager: MarkusVi
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 273fdb80475defb0576bcd29d1944c5f6c595cfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266502"
---
# <a name="frequently-asked-questions-around-azure-active-directory-reports"></a>有关 Azure Active Directory 报告的常见问题解答

本文包括了对 Azure Active Directory (Azure AD) 报告常见问题的解答。 有关详细信息，请参阅 [Azure Active Directory 报告](overview-reports.md)。 

## <a name="getting-started"></a>入门 

**问：我当前使用`https://graph.windows.net/<tenant-name>/reports/`终结点 API 以编程方式将 Azure AD 审核和集成应用程序使用情况报告拉至我们的报告系统中。我应该切换到什么？**

**答：** 请查看 [API 参考](https://developer.microsoft.com/graph/)，了解如何[使用 API 访问活动报告](concept-reporting-api.md)。 此终结点有两个报告 （**审核**和**登录**）， 它们提供您在旧 API 终结点中得到的所有数据。 此新的终结点还有一个登录报告，其中包含可用来获取应用使用情况、设备使用情况和用户登录信息的 Azure AD Premium 许可证。

---

**问：我当前使用`https://graph.windows.net/<tenant-name>/reports/`终结点 API 以编程方式将 Azure AD 安全报告（特定类型的检测，如泄露的凭据或来自匿名 IP 地址的登录）拉入我们的报告系统。我应该切换到什么？**

**答：** 您可以使用 [身份保护风险检测 API](../identity-protection/graph-get-started.md) 通过 Microsoft 图形访问安全检测。 这种新格式在如何查询数据方面提供了更大的灵活性，包括高级筛选、字段选择等，并将风险检测标准化为一种类型，以便更轻松地集成到 SIEM 和其他数据收集工具中。 因为数据采用的格式不同，所以无法用新查询替代旧查询。 不过，[新 API 使用的是 Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent)，后者是 O365 或 Azure AD 之类的 API 的 Microsoft 标准。 因此，所需的工作可以扩展您当前的 Microsoft 图形投资，或者帮助您开始过渡到这个新的标准平台。

---

**问：如何获得高级许可证？**

**答：** 请参阅 [Azure Active Directory Premium 入门](../fundamentals/active-directory-get-started-premium.md)来升级 Azure Active Directory 版本。

---

**问：获得高级许可证后多久能看见活动数据？**

**答：** 如果获得免费许可证时已有活动数据，则可以立即看到这些数据。 如果没有任何数据，则需要在一到两天后，数据才会显示在报告中。

---

**问：获得 Azure AD Premium 许可证后是否能查看上个月的数据？**

**答：** 如果您最近已切换到高级版（包括试用版），则最初最多可以看到 7 天的数据。 随着数据累积，可以看到过去 30 天的数据。

---

**问：若要查看到 Azure 门户的活动登录或通过 API 获取数据，是否需要是全局管理员？**

**答：** 否。如果你是租户的**安全读取者**或**安全管理员**，也可以通过门户或 API 访问报告数据。 当然，**全局管理员**也有权访问这些数据。

---


## <a name="activity-logs"></a>活动日志


**问：Azure 门户中活动日志（审核和登录）的数据保留是什么？** 

**答：** 下表列出了活动日志的数据保留期。 有关详细信息，请参阅 [Azure AD 报告的数据保留策略](reference-reports-data-retention.md)。

| 报表                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| 审核日志             | 7 天        | 30 天             | 30 天             |
| 登录               | 空值           | 30 天             | 30 天             |
| Azure MFA 使用情况        | 30 天       | 30 天             | 30 天             |

---

**问：完成任务后，需要多长时间才能看到活动数据？**

**答：** 审核日志的延迟为 15 分钟到 1 小时。 对于某些记录，登录活动日志可能需要花费 15 分钟到 2 小时。

---

**问：是否可以通过 Azure 门户获取 Office 365 活动日志信息？**

**答：** 即使 Office 365 活动和 Azure AD 活动日志共享大量目录资源，但如果需要 Office 365 活动日志的完整视图，则应转到 Microsoft [365 管理中心](https://admin.microsoft.com)以获取 Office 365 活动日志信息。

---

**问：应使用哪些 API 获取有关 Office 365 活动日志的信息？**

**答：** 使用[Office 365 管理 API](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview)通过 API 访问 Office 365 活动日志。

---

**问：可从 Azure 门户下载多少条记录？**

**答：** 最多可从 Azure 门户下载 5000 条记录。 记录按最近时间** 进行排序，默认情况下获取的是最近 5000 条记录。

---

## <a name="risky-sign-ins"></a>有风险的登录

**问：身份保护中有风险检测，但我在登录报告中看不到相应的登录。这是预料之中的吗？**

**答：** 是的，标识保护会评估所有身份验证流的风险，无论其为交互式还是非交互式。 但是，所有登录报告仅显示交互式登录。

---

问：如何了解 Azure 门户中被标记为存在风险的用户或登录的原因？****

**答：** 如果您有 Azure **AD 高级**订阅，则可以通过在**标记为风险的用户**中选择用户或在**风险登录**报告中选择记录来了解有关基本风险检测的更多信息。 如果您有**免费**或**基本**订阅，则可以查看有风险的用户和有风险的登录报告，但看不到潜在的风险检测信息。

---

**问：在登录和有风险的登录报告中，IP 地址是如何计算的？**

答：**** IP 地址的发布方式是，在 IP 地址和使用该地址的计算机所在的物理位置之间没有确定的连接。 有多种因素会导致映射 IP 地址进一步变得复杂，例如，从中心池发布 IP 地址的移动运营商和 VPN 通常与实际使用客户端设备的位置距离很远。 目前，在 Azure AD 报告中，最好是基于跟踪、注册表数据、反向查看和其他信息将 IP 地址转换为物理位置。 

---

**问：风险检测"检测到额外风险的登录"是什么意思？**

**答：** 为了让你深入了解环境中所有具有风险的登录，对于为了执行“Azure AD 标识保护”订阅方专用的检测而进行的登录，“登录时检测到其他风险”将充当占位符。

---

## <a name="conditional-access"></a>条件性访问

**问：此功能有什么新内容？**

**答：** 客户现在可以通过所有登录报告对条件访问策略进行故障排除。 客户可以查看条件访问状态，并深入了解应用于登录的策略的详细信息以及每个策略的结果。

**问：如何开始使用？**

**答：** 若要开始使用：

* 导航到 [Azure门户](https://portal.azure.com)中的登录报告。
* 单击要进行故障排除的登录。
* 导航到**条件访问**选项卡。在这里，您可以查看影响每个策略登录的所有策略和结果。 
    
**问：条件访问状态的可能值是什么？**

**答：** 条件访问状态可以具有以下值：

* **未应用**：这表示在范围内没有针对用户和应用程序的 CA 策略。 
* **成功**：这表示在范围内存在针对用户和应用程序的 CA 策略，并且已成功满足 CA 策略。 
* **失败**：这表示在范围内存在针对用户和应用程序的 CA 策略，但不满足 CA 策略。 
    
**问：条件访问策略结果的可能值是什么？**

**答：** 条件访问策略可以具有以下结果：

* **成功**：成功满足策略。
* **失败**：不满足策略。
* **未应用**：这可能是因为不符合策略条件。
* **未启用**：这是由于策略处于禁用状态。 
    
**问：所有登录报告中的策略名称与 CA 中的策略名称不匹配。为什么？**

**答：** 所有登录报告中的策略名称均基于登录时的 CA 策略名称。 如果你后来（即登录后）更新了策略名称，则这可能与 CA 中的策略名称不一致。

**问：由于条件访问策略，我的登录被阻止，但登录活动报告显示登录成功。为什么？**

**答：** 当前，当应用条件访问时，登录报告可能无法显示 Exchange ActiveSync 方案的准确结果。 在某些情况下，报表中的登录结果显示成功登录，但登录实际上由于条件访问策略而失败。 
