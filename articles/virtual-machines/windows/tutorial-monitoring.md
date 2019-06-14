---
title: 教程 - 监视和更新 Azure 中的 Windows 虚拟机 | Microsoft Docs
description: 本教程介绍如何在 Windows 虚拟机上监视启动诊断和性能指标，以及管理程序包更新
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: aa423fc441c50c774a9670feec64d0f844a4f5ec
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755287"
---
# <a name="tutorial-monitor-and-update-a-windows-virtual-machine-in-azure"></a>教程：监视和更新 Azure 中的 Windows 虚拟机

Azure 监视使用代理从 Azure VM 收集启动和性能数据，将此数据存储在 Azure 存储中，并使其可供通过门户、Azure PowerShell 模块和 Azure CLI 进行访问。 使用更新管理可以管理 Azure Windows VM 的更新和修补程序。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 在 VM 上启用启动诊断
> * 查看启动诊断
> * 查看 VM 主机指标
> * 安装诊断扩展
> * 查看 VM 指标
> * 创建警报
> * 管理 Windows 更新
> * 监视器更改和清单
> * 设置高级监视

## <a name="launch-azure-cloud-shell"></a>启动 Azure Cloud Shell

Azure Cloud Shell 是免费的交互式 shell，可以使用它运行本文中的步骤。 它预安装有常用 Azure 工具并将其配置与帐户一起使用。 

若要打开 Cloud Shell，只需要从代码块的右上角选择“试一试”。  也可以通过转到 [https://shell.azure.com/powershell](https://shell.azure.com/powershell) 在单独的浏览器标签页中启动 Cloud Shell。 选择“复制”以复制代码块，将其粘贴到 Cloud Shell 中，然后按 Enter 来运行它。 

## <a name="create-virtual-machine"></a>创建虚拟机

若要在本教程中配置 Azure 监视和更新管理，需要 Azure 中的 Windows VM。 首先，使用 [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) 设置 VM 的管理员用户名和密码：

```azurepowershell-interactive
$cred = Get-Credential
```

现在，使用 [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) 创建 VM。 以下示例在“EastUS”位置  创建一个名为 myVM  的 VM。 如果资源组 *myResourceGroupMonitorMonitor* 和支持的网络资源不存在，则会创建它们：

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroupMonitor" `
    -Name "myVM" `
    -Location "East US" `
    -Credential $cred
```

创建资源和 VM 需要几分钟的时间。

## <a name="view-boot-diagnostics"></a>查看启动诊断

当 Windows 虚拟机启动时，启动诊断代理将捕获屏幕输出，可以使用该输出进行故障排除。 此功能是默认启用的。 捕获的屏幕截图存储在一个 Azure 存储帐户中，该帐户也是默认创建的。

可以使用 [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/az.compute/get-azvmbootdiagnosticsdata) 命令获取启动诊断数据。 在下面的示例中，启动诊断下载到了 *c:\* 驱动器的根目录中。

```powershell
Get-AzVMBootDiagnosticsData -ResourceGroupName "myResourceGroupMonitor" -Name "myVM" -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>查看主机指标

Windows VM 在 Azure 中有一个与它交互的专用主机 VM。 系统会自动收集该主机的指标，可以在 Azure 门户中查看这些指标。

1. 在 Azure 门户中单击“资源组”，选择“myResourceGroupMonitor”，并在资源列表中选择“myVM”。   
2. 要查看主机 VM 的性能情况，请在 VM 边栏选项卡上单击“指标”，并选择“可用指标”下的任一主机指标。  

    ![查看主机指标](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>安装诊断扩展

可以使用基本的主机指标，但若要查看更详细的指标和 VM 特定的指标，需在 VM 上安装 Azure 诊断扩展。 使用 Azure 诊断扩展可从 VM 检索其他监视数据和诊断数据。 可以查看这些性能指标，并根据 VM 的性能情况创建警报。 诊断扩展是通过 Azure 门户安装的，如下所述：

1. 在 Azure 门户中单击“资源组”，选择“myResourceGroupMonitor”，并在资源列表中选择“myVM”。   
2. 单击“诊断设置”。  列表中会显示已在上一部分启用的“启动诊断”。  单击“基本指标”对应的复选框。 
3. 单击“启用来宾级监视”按钮。 

    ![查看诊断指标](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>查看 VM 指标

可以像查看主机 VM 指标一样查看 VM 指标：

1. 在 Azure 门户中单击“资源组”，选择“myResourceGroupMonitor”，并在资源列表中选择“myVM”。   
2. 要查看 VM 的性能情况，请在 VM 边栏选项卡上单击“指标”，并选择“可用指标”下的任一诊断指标。  

    ![查看 VM 指标](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>创建警报

可以根据特定的性能指标创建警报。 例如，当平均 CPU 使用率超过特定的阈值或者可用磁盘空间低于特定的空间量时，警报可以发出通知。 警报显示在 Azure 门户中，也可以通过电子邮件发送。 还可以触发 Azure 自动化 Runbook 或 Azure 逻辑应用来响应生成的警报。

以下示例针对平均 CPU 使用率创建警报。

1. 在 Azure 门户中单击“资源组”，选择“myResourceGroupMonitor”，并在资源列表中选择“myVM”。   
2. 在 VM 边栏选项卡上单击“警报规则”，并单击警报边栏选项卡顶部的“添加指标警报”。  
3. 为警报提供**名称**，例如 *myAlertRule*
4. 若要在 CPU 百分比持续 5 分钟超过 1.0 时触发警报，请保留选中其他所有默认值。
5. （可选）选中“电子邮件所有者、参与者和阅读者”对应的框，以便向他们发送电子邮件通知。  默认操作是在门户中显示通知。
6. 单击“确定”  按钮。

## <a name="manage-windows-updates"></a>管理 Windows 更新

使用更新管理可以管理 Azure Windows VM 的更新和修补程序。
可以直接在 VM 中快速评估可用更新的状态、计划所需更新的安装以及查看部署结果，验证更新是否已成功应用到 VM。

有关定价信息，请参阅[更新管理的自动化定价](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management"></a>启用更新管理

为 VM 启用更新管理：

1. 在屏幕的左侧，选择“虚拟机”  。
2. 从列表中选择一个虚拟机。
3. 在 VM 屏幕上的“操作”  部分，单击“更新管理”  。 “启用更新管理”  屏幕随即打开。

执行验证以确定是否为该 VM 启用了更新管理。
验证包括检查 Log Analytics 工作区和链接的自动化帐户，以及解决方案是否在工作区中。

[Log Analytics](../../log-analytics/log-analytics-overview.md) 工作区用于收集由功能和服务（如更新管理）生成的数据。
工作区提供了一个位置来查看和分析来自多个数据源的数据。
若要在需要更新的 VM 上执行其他操作，可使用 Azure 自动化运行针对 VM 的 Runbook，例如下载和应用更新。

验证过程还会检查 VM 是否预配了 Microsoft Monitoring Agent (MMA) 和自动化混合 Runbook 辅助角色。
此代理用于与虚拟机通信并获取关于更新状态的信息。

选择 Log Analytics 工作区和自动化帐户，然后单击“启用”以启用此解决方案。  启用此解决方案最长需要 15 分钟的时间。

如果在载入过程中发现缺少下列任何先决条件，则会自动添加这些条件：

* [Log Analytics](../../log-analytics/log-analytics-overview.md) 工作区
* [自动化](../../automation/automation-offering-get-started.md)
* VM 上已启用[混合 runbook 辅助角色](../../automation/automation-hybrid-runbook-worker.md)

“更新管理”  屏幕随即打开。 配置要使用的位置、Log Analytics 工作区和自动化帐户，然后单击“启用”。  如果这些字段灰显，则意味着已为 VM 启用其他自动化解决方案，因此必须使用同一工作区和自动化帐户。

![启用“更新管理解决方案”](./media/tutorial-monitoring/manageupdates-update-enable.png)

启用解决方案最多可能需要 15 分钟。 在此期间，不应关闭浏览器窗口。 启用该解决方案后，VM 中缺少的更新信息会流向 Azure Monitor 日志。 这些数据需花费 30 分钟到 6 小时的时间才能用于分析。

### <a name="view-update-assessment"></a>查看更新评估

启用“更新管理”  后，“更新管理”  屏幕随即显示。 评估更新完成后，可在“缺失更新”  选项卡上查看缺失更新的列表。

 ![查看更新状态](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>计划更新部署

若要安装更新，请计划一个遵循你的发布时间和服务窗口的部署。 可选择在部署中包括哪种更新类型。 例如，可包括关键或安全更新，排除更新汇总。

单击“更新管理”  屏幕顶部的“计划更新部署”  ，计划用于虚拟机的新的更新部署。 在“新建更新部署”  屏幕中，指定以下信息：

若要创建新的更新部署，请选择“计划更新部署”  。 此时会打开“新建更新部署”页。  为下表中介绍的属性输入值，然后单击“创建”  ：

| 属性 | 说明 |
| --- | --- |
| 名称 |用于标识更新部署的唯一名称。 |
|操作系统| Linux 或 Windows|
| 要更新的组 |对于 Azure 计算机，请定义基于一组订阅、资源组、位置和标记的查询，生成要在部署中包含的 Azure VM 动态组。 </br></br>对于非 Azure 计算机，请选择现有的已保存搜索，以选择要包含在部署中的非 Azure 计算机组。 </br></br>有关详细信息，请参阅[动态组](../../automation/automation-update-management.md#using-dynamic-groups)|
| 要更新的计算机 |选择已保存的搜索、已导入的组或者从下拉列表中选择“计算机”并选择单个计算机。 如果选择“计算机”，则计算机的就绪状态将在“更新代理商准备情况”列中显示   。</br> 要了解在 Azure Monitor 日志中创建计算机组的不同方法，请参阅 [Azure Monitor 日志中的计算机组](../../azure-monitor/platform/computer-groups.md) |
|更新分类|选择所需的所有更新分类|
|包括/排除更新|这将打开“包括/排除”  页。 要包含或排除的更新位于单独的选项卡上。 有关如何处理包含的详细信息，请参阅[包含行为](../../automation/automation-update-management.md#inclusion-behavior) |
|计划设置|选择启动时间，然后选择任“一次”或“重复”|
| 前脚本 + 后脚本|选择要在部署前和部署后运行的脚本|
| 维护时段 |为更新设置的分钟数。 该值不能小于 30 分钟，且不能大于 6 小时 |
| 重新启动控制| 确定应如何处理重新启动。 可用选项包括：</br>需要时重新启动(默认)</br>始终重新启动</br>从不重新启动</br>仅重启 - 不安装更新|

此外，能够以编程方式创建更新部署。 若要了解如何使用 REST API 创建更新部署，请参阅[软件更新配置 - 创建](/rest/api/automation/softwareupdateconfigurations/create)。 此外，还有一个示例 Runbook，可用于创建每周更新部署。 若要了解有关此 Runbook 的详细信息，请参阅[为资源组中的一个或多个 VM 创建每周更新部署](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1)。

完成配置计划后，单击“创建”  按钮，然后返回到状态仪表板。
请注意，“已计划”  表显示你创建的部署计划。

### <a name="view-results-of-an-update-deployment"></a>查看更新部署结果

在计划性部署开始后，可以在“更新管理”  屏幕的“更新部署”  选项卡上查看该部署的状态。
如果部署当前正在运行，则状态显示为“正在运行”  。 如果部署已成功完成，则状态会更改为“成功”  。
如果部署中有一个或多个更新失败，则状态为“部分失败”  。
单击已完成的更新部署，查看该更新部署的仪表板。

![特定部署的更新部署状态仪表板](./media/tutorial-monitoring/manageupdates-view-results.png)

在“更新结果”  中，磁贴总结了 VM 上更新和部署结果的总数。
右侧的表格详细列出了每个更新的细目以及安装结果，结果可能是以下值之一：

* 未尝试  - 由于定义的维护时段时长不足，因而未安装更新。
* 成功  - 更新成功
* 失败  - 更新失败

单击“所有日志”  ，查看部署创建的所有日志条目。

单击“输出”  磁贴，查看负责管理目标虚拟机更新部署的 runbook 的作业流。

单击“错误”  ，查看有关部署中的任何错误的详细信息。

## <a name="monitor-changes-and-inventory"></a>监视器更改和清单

可以收集和查看清单，了解计算机上的软件、文件、Linux 守护程序、Windows 服务和 Windows 注册表项。 跟踪计算机的配置有助于查明环境中的操作问题，更好地了解计算机的状态。

### <a name="enable-change-and-inventory-management"></a>启用更改和清单管理

为 VM 启用更改和清单管理：

1. 在屏幕的左侧，选择“虚拟机”  。
2. 从列表中选择一个虚拟机。
3. 在 VM 屏幕上的“操作”  部分中，单击“清单”  或“更改跟踪”  。 此时会打开“启用更改跟踪和清单”屏幕。 

配置要使用的位置、Log Analytics 工作区和自动化帐户，然后单击“启用”。  如果这些字段灰显，则意味着已为 VM 启用其他自动化解决方案，因此必须使用同一工作区和自动化帐户。 即使这些解决方案在菜单上是分开的，它们也是同一解决方案。 启用一个解决方案就会为 VM 启用两个解决方案。

![启用更改和清单跟踪](./media/tutorial-monitoring/manage-inventory-enable.png)

启用解决方案后，可能需要一些时间在 VM 上收集清单，然后才显示数据。

### <a name="track-changes"></a>跟踪更改

在 VM 中的“操作”下选择“更改跟踪”。   单击“编辑设置”  ，此时会显示“更改跟踪”  页。 选择要跟踪的设置类型，然后单击“+ 添加”  配置设置。 适用于 Windows 的可用选项包括：

* Windows 注册表
* Windows 文件

有关更改跟踪的详细信息，请参阅[排查 VM 上的更改问题](../../automation/automation-tutorial-troubleshoot-changes.md)

### <a name="view-inventory"></a>查看清单

在 VM 中的“操作”下选择“清单”。   在“软件”选项卡上有一个表，列出了已发现的软件。  可在表中查看每个软件记录的高级详细信息。 这些详细信息包括软件名称、版本、发布者和上次刷新时间。

![查看清单](./media/tutorial-monitoring/inventory-view-results.png)

### <a name="monitor-activity-logs-and-changes"></a>监视活动日志和更改

在 VM 的“更改跟踪”页中，选择“管理活动日志连接”。   此任务打开“Azure 活动日志”页。  选择“连接”，  将更改跟踪连接到 VM 的 Azure 活动日志。

启用此设置后，导航到 VM 的“概览”页，然后选择“停止”以停止 VM。   出现提示时，选择“是”  即可停止 VM。 将 VM 解除分配以后，请选择“启动”以重启 VM。 

停止和启动 VM 时，会在活动日志中记录一个事件。 导航回到“更改跟踪”页。  选择页面底部的“事件”选项卡。  一段时间后，事件会显示在图表和表中。 可以选择每个事件来查看其详细信息。

![在活动日志中查看更改](./media/tutorial-monitoring/manage-activitylog-view-results.png)

此图表显示了一段时间内发生的更改。 添加活动日志连接以后，顶部的线形图会显示 Azure 活动日志事件。 条形图的每一行代表不同类型的可跟踪更改。 这些类型是 Linux 守护程序、文件、Windows 注册表项、软件、Windows 服务。 “更改”选项卡显示在可视化效果中显示的更改的详细信息，按更改发生时间以降序方式排列（最近发生的排在最前面）。

## <a name="advanced-monitoring"></a>高级监视

可以使用 [Azure 自动化](../../automation/automation-intro.md)提供的“更新管理”及“更改和清单”等解决方案对 VM 进行更高级的监视。

可以访问 Log Analytics 工作区时，可以通过选择“设置”  下的“高级设置”  来找到工作区密钥和工作区标识符。 使用 [Set-AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) 命令将 Microsoft Monitoring Agent 扩展添加到 VM。 更新以下示例中的变量值以反映 Log Analytics 工作区密钥和工作区 ID。

```powershell
$workspaceId = "<Replace with your workspace Id>"
$key = "<Replace with your primary key>"

Set-AzVMExtension -ResourceGroupName "myResourceGroupMonitor" `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName "myVM" `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $workspaceId} `
  -ProtectedSettings @{"workspaceKey" = $key} `
  -Location "East US"
```

几分钟后，应该会在 Log Analytics 工作区中看到新 VM。

![“Log Analytics 工作区”边栏选项卡](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>后续步骤

在本教程中，已使用 Azure 安全中心配置并查看了 VM。 你已了解如何：

> [!div class="checklist"]
> * 创建虚拟网络
> * 创建资源组和 VM
> * 在 VM 上启用启动诊断
> * 查看启动诊断
> * 查看主机指标
> * 安装诊断扩展
> * 查看 VM 指标
> * 创建警报
> * 管理 Windows 更新
> * 监视器更改和清单
> * 设置高级监视

请转到下一教程来了解 Azure 安全中心。

> [!div class="nextstepaction"]
> [管理 VM 安全性](./tutorial-azure-security.md)
