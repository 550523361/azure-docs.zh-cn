---
title: "在 Azure 自动化中编辑文本 Runbook"
description: "本文提供的不同过程适用于在 Azure 自动化中通过文本编辑器来处理 PowerShell Runbook 和 PowerShell 工作流 Runbook。"
services: automation
documentationcenter: 
author: georgewallace
manager: stevenka
editor: tysonn
ms.assetid: 6f5b48fb-6f30-4e99-9e14-9061b5554b08
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: magoedte;bwren
ms.openlocfilehash: e166700be0ec6b32d40f34bd47f92a7cff1cc7bf
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2017
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>在 Azure 自动化中编辑文本 Runbook
Azure 自动化中的文本编辑器可以用来编辑 [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks) 和 [PowerShell 工作流 Runbook](automation-runbook-types.md#powershell-workflow-runbooks)。 该编辑器具有其他代码编辑器的典型功能（例如智能感知和颜色编码），并提供其他特殊功能来协助访问 Runbook 的常用资源。  本文提供了使用该编辑器执行不同功能的详细步骤。

该文本编辑器包含的一项功能是将活动、资产和子 Runbook 的代码插入 Runbook 中。 不需要亲自键入代码，只需从可用资源列表中进行选择，然后即可将相应代码插入 Runbook 中。

Azure 自动化中的每个 Runbook 都有两个版本：草稿版和已发布版。 先对 Runbook 的草稿版进行编辑，然后将其发布，这样便可以执行了。 无法编辑已发布版本。 有关详细信息，请参阅[发布 Runbook](automation-creating-importing-runbook.md#publishing-a-runbook)。

若要使用[图形 Runbook](automation-runbook-types.md#graphical-runbooks)，请参阅 [Azure 自动化中的图形创作](automation-graphical-authoring-intro.md)。

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>使用 Azure 门户编辑 Runbook
通过以下过程打开 Runbook 即可在文本编辑器中进行编辑。

1. 在 Azure 门户中，选择自动化帐户。
2. 单击“Runbook”磁贴打开 Runbook 的列表。
3. 单击想要编辑的 Runbook 的名称，并单击“编辑”按钮。
4. 执行所需的编辑。
5. 完成编辑后，单击“保存”。
6. 若要发布最新的 Runbook 草稿版，请单击“发布”。

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>将 cmdlet 插入 Runbook
1. 在文本编辑器的“画布”中，将光标置于要放置该 cmdlet 的地方。
2. 展开“库”控件中的“Cmdlet”节点。
3. 展开包含要使用的 cmdlet 的模块。
4. 右键单击要插入的 cmdlet，并选择“添加到画布”。  如果该 cmdlet 具有多个参数集，则会添加默认集。  还可以展开 cmdlet 来选择不同的参数集。
5. 该 cmdlet 的代码在插入时附带整个参数列表。
6. 对于任何必需的参数，请提供合适的值来替换大括号 <> 中的数据类型。  删除不需要的任何参数。

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>将子 Runbook 的代码插入 Runbook 中
1. 在文本编辑器的“画布”中，将光标置于要放置[子 Runbook](automation-child-runbooks.md) 代码的位置。
2. 展开“库”控件中的“Runbook”节点。
3. 右键单击要插入的 Runbook，并选择“添加到画布”。
4. 此时会插入带 Runbook 参数占位符的子 Runbook 的代码。
5. 将占位符替换为每个参数的相应值。

### <a name="to-insert-an-asset-into-a-runbook"></a>将资产插入 Runbook
1. 在文本编辑器的“画布”中，将光标置于要放置子 Runbook 代码的地方。
2. 展开“库”控件中的“资产”节点。
3. 展开所需资产类型的节点。
4. 右键单击要插入的资产，并选择“添加到画布”。  对于[变量资产](automation-variables.md)，可选择“将‘获取变量’添加到画布”或“将‘设置变量’添加到画布”，具体取决于是要获取变量还是要设置变量。
5. 此时会将资产的代码插入到 Runbook 中。

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>使用 Azure 门户编辑 Runbook
通过以下过程打开 Runbook 即可在文本编辑器中进行编辑。

1. 在 Azure 门户中，选择“自动化”，并单击自动化帐户的名称。
2. 选择“Runbook”选项卡。
3. 单击想要编辑的 Runbook 的名称，并选择“创作”选项卡。
4. 单击屏幕底部的“编辑”按钮。
5. 执行所需的编辑。
6. 完成编辑后，单击“保存”。
7. 若要发布最新的 Runbook 草稿版，请单击“发布”。

### <a name="to-insert-an-activity-into-a-runbook"></a>将活动插入 Runbook
1. 在文本编辑器的“画布”中，将光标置于要放置该活动的地方。
2. 在屏幕底部，单击“插入”，并单击“活动”。
3. 在“集成模块”列中，选择包含活动的模块。
4. 在“活动”窗格中，选择一个活动。
5. 在“说明”列中，记下活动说明。 也可以选择单击“查看详细帮助”，以便在浏览器中启动活动的帮助。
6. 单击右箭头。  如果该活动具有参数，则会将其列出供你参考。
7. 单击勾选按钮。  此时会将运行活动的代码插入 Runbook 中。
8. 如果活动需要参数，请提供合适的值来替换大括号 <> 中的数据类型。

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>将子 Runbook 的代码插入 Runbook 中
1. 在文本编辑器的“画布”中，将光标置于要放置[子 Runbook](automation-child-runbooks.md) 的位置。
2. 在屏幕底部，单击“插入”，并单击“Runbook”。
3. 选择要从中心列插入的 Runbook，并单击右箭头。
4. 如果该 Runbook 具有参数，则会将其列出供你参考。
5. 单击勾选按钮。  此时会将运行所选 Runbook 的代码插入当前 Runbook 中。
6. 如果 Runbook 需要参数，请提供合适的值来替换大括号 <> 中的数据类型。

### <a name="to-insert-an-asset-into-a-runbook"></a>将资产插入 Runbook
1. 在文本编辑器的“画布”中，将光标置于要放置该活动以检索资产的地方。
2. 在屏幕底部，单击“插入”，并单击“设置”。
3. 在“设置操作”列中，选择所需操作。
4. 在中心列的可用资产中进行选择。
5. 单击勾选按钮。  此时会将获取或设置资产的代码插入 Runbook 中。

## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>使用 Windows PowerShell 编辑 Azure 自动化 Runbook
要使用 Windows PowerShell 来编辑 Runbook，可使用所选编辑器进行操作，然后将其保存到 .ps1 文件。 可以先使用 [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) cmdlet 来检索 Runbook 的内容，然后使用 [Set-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) cmdlet 将现有的草稿 Runbook 替换为已修改的 Runbook。

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>使用 Windows PowerShell 检索 Runbook 的内容
以下示例命令演示了如何检索 Runbook 的脚本并将其保存到脚本文件。 在此示例中，检索的是草稿版本。 也可以检索 Runbook 的已发布版本，不过该版本不能进行更改。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>使用 Windows PowerShell 更改 Runbook 的内容
以下示例命令演示了如何使用脚本文件的内容替换 Runbook 的现有内容。 请注意，此示例过程与[使用 Windows PowerShell 从脚本文件中导入 Runbook](automation-creating-importing-runbook.md) 中的相同。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>相关文章
* [在 Azure 自动化中创建或导入 Runbook](automation-creating-importing-runbook.md)
* [了解 PowerShell 工作流](automation-powershell-workflow.md)
* [Azure 自动化中的图形创作](automation-graphical-authoring-intro.md)
* [证书](automation-certificates.md)
* [连接](automation-connections.md)
* [凭据](automation-credentials.md)
* [计划](automation-schedules.md)
* [变量](automation-variables.md)
