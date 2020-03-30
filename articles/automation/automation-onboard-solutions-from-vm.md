---
title: 从 Azure VM 载入“更新管理”、“更改跟踪”和“清单”解决方案
description: 了解如何载入包含属于 Azure 自动化的“更新管理”、“更改跟踪”和“清单”解决方案的 Azure 虚拟机。
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 621b429f5dc3a6b6620e4d41ad46763e1d4fa226
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299514"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>从 Azure 虚拟机载入“更新管理”、“更改跟踪”和“清单”解决方案

Azure 自动化提供了解决方案来帮助管理操作系统安全更新、跟踪更改以及列出计算机上所安装项的清单。 可以通过多种方式来载入计算机。 可以通过虚拟机、[通过自动化帐户](automation-onboard-solutions-from-automation-account.md)、[通过浏览多个计算机](automation-onboard-solutions-from-browse.md)或通过使用 [Runbook](automation-onboard-solutions.md) 载入解决方案。 本文介绍了如何从 Azure 虚拟机载入这些解决方案。

## <a name="sign-in-to-azure"></a>登录 Azure

通过 https://portal.azure.com 登录到 Azure 门户。

## <a name="enable-the-solutions"></a>启用解决方案

首先，在 VM 上启用一个或所有三个解决方案：

1. 在[Azure 门户](https://portal.azure.com)中，从左侧窗格中选择 **"虚拟机"，** 或从**主页**搜索和选择**虚拟机**。
2. 选择要为其启用解决方案的 VM。
3. 在 VM 页上，在 **"操作"** 下，选择 **"更新管理**、**库存**"或 **"更改跟踪**"。 无论自动化帐户的位置如何，虚拟机都可以存在于任何区域中。 从 VM 载入解决方案时，需要具有确定 VM`Microsoft.OperationalInsights/workspaces/read`是否已载入工作区的权限。 要了解所需的其他权限，请参阅[板载计算机所需的权限](automation-role-based-access-control.md#onboarding)。

若要了解如何一次加入多台计算机，请参阅[载入“更新管理”、“更改跟踪”和“清单”解决方案](automation-onboard-solutions-from-automation-account.md)。

选择 Azure Log Analytics 工作区和自动化帐户，然后选择“启用”以启用此解决方案。**** 启用此解决方案最长需要 15 分钟的时间。

![载入“更新管理”解决方案](media/automation-tutorial-update-management/manageupdates-update-enable.png)

请转到其他解决方案，然后选择“启用”****。 日志分析工作区和自动化帐户下拉列表被禁用，因为这些解决方案使用与以前启用的解决方案相同的工作区和自动化帐户。

> [!NOTE]
> **更改跟踪**和**清单**使用同一解决方案。 启用其中一个解决方案时，另一个解决方案也会被启用。

## <a name="scope-configuration"></a>作用域配置

每个解决方案在工作区中使用作用域配置来确定获取解决方案的计算机。 作用域配置是包含一个或多个已保存搜索的组，这些搜索用来将解决方案的作用域限制为特定计算机。 要访问范围配置，请在自动化帐户中，在 **"相关资源**"下，选择**工作区**。 在工作区中，在**工作区数据源**下，选择 **"范围配置**"。

如果所选工作区中还没有“更新管理”或“更改跟踪”解决方案，则将创建以下作用域配置：

* **MicrosoftDefaultScopeConfig-ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

如果所选工作区已有解决方案，则不会重新部署解决方案，也不会添加作用域配置。

选择任何配置上的省略号 (...) 并选择“编辑”。******** 在“编辑作用域配置”**** 窗格中，选择“选择计算机组”****。 “计算机组”**** 窗格将显示用来创建作用域配置的已保存搜索。

## <a name="saved-searches"></a>保存的搜索

向“更新管理”、“更改跟踪”和“清单”解决方案添加计算机时，计算机会添加到工作区中的两个已保存搜索之一。 这些已保存搜索是包含这些解决方案所针对的目标计算机的查询。

转到你的工作区。 在“常规”**** 下，选择“保存的搜索”****。 下表中将显示这些解决方案使用的两个已保存搜索：

|“属性”     |类别  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | 更新        | Updates__MicrosoftDefaultComputerGroup         |

选择任一已保存的搜索以查看用来填充组的查询。 下图显示了查询及其结果：

![保存的搜索](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="unlink-workspace"></a>取消链接工作区

以下解决方案依赖于 Log Analytics 工作区：

* [更新管理](automation-update-management.md)
* [更改跟踪](automation-change-tracking.md)
* [在非工作时间启动/停止 VM](automation-solution-vm-management.md)

如果您决定不再希望将自动化帐户与日志分析工作区集成，则可以直接从 Azure 门户取消链接帐户。  在继续之前，首先需要删除前面所述的解决方案，否则此过程将无法继续。 查看已导入的特定解决方案的主题，了解删除该解决方案所需的步骤。

删除这些解决方案后，可以执行以下步骤取消链接自动化帐户。

> [!NOTE]
> 某些解决方案（包括早期版本的 Azure SQL 监视解决方案）可能已创建自动化资产并可能还需要在取消链接工作区之前删除。

1. 从 Azure 门户中打开自动化帐户，并在“自动化帐户”页左侧的“相关资源”部分下，选择“链接工作区”********。

2. 在“取消链接工作区”页上，单击“取消链接工作区”****。

   ![“取消链接工作区”页](media/automation-onboard-solutions-from-vm/automation-unlink-workspace-blade.png).

   系统会提示用户确认是否要继续。

3. 当 Azure 自动化尝试从 Log Analytics 工作区中取消链接该帐户时，可以在菜单中的“通知”**** 下跟踪进度。

如果使用了“更新管理”解决方案，可能会选择要删除在删除该解决方案后不再需要的以下项。

* 更新计划 - 每个计划将具有与您创建的更新部署相匹配的名称。

* 为解决方案创建的混合工作组 - 每个组将命名为类似机器1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8）。

如果使用了“在非工作时间启动/停止 VM”解决方案，可能会选择要删除在删除该解决方案后不再需要的以下项。

* 启动和停止 VM Runbook 计划
* 启动和停止 VM Runbook
* 变量

或者，您也可以从日志分析工作区取消将工作区从自动化帐户中取消链接。 在工作区上，选择 **"相关资源**"下的 **"自动化帐户**"。 在"自动化帐户"页上，选择 **"取消链接帐户**"。

## <a name="clean-up-resources"></a>清理资源

从更新管理中删除 VM：

* 在 Log Analytics 工作区中，从范围配置 `MicrosoftDefaultScopeConfig-Updates` 的已保存的搜索中删除 VM。 已保存的搜索位于工作区的“常规”下****。
* 删除 [Microsoft Monitoring Agent](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) 或 [适用于 Linux 的 Log Analytics 代理](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)。

## <a name="next-steps"></a>后续步骤

继续学习有关解决方案的教程来了解如何使用它们：

* [教程 - 管理 VM 的更新](automation-tutorial-update-management.md)

* [教程 - 识别 VM 上的软件](automation-tutorial-installed-software.md)

* [教程 - 对 VM 上的更改进行故障排除](automation-tutorial-troubleshoot-changes.md)
