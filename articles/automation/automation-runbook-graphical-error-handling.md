---
title: Azure 自动化图形 Runbook 中的错误处理
description: 本文介绍如何在 Azure 自动化图形 Runbook 中实现错误处理逻辑。
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: f1aa605b3e6f32b260ea4a9eee9c056277fcd12d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "79367064"
---
# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Azure 自动化图形 Runbook 中的错误处理

对于 Azure 自动化图形 Runbook，需要考虑的一个关键设计原则是确定 Runbook 在执行期间可能会遇到的问题。 这些情况可能包括：成功、预计的错误状态，以及意外的错误条件。

通常情况下，如果 Runbook 活动出现的是非终止错误，则 Windows PowerShell 会无视该错误而继续处理后续的任何活动。 该错误可能会生成异常，但下一活动仍允许运行。

图形 Runbook 应包含错误处理代码来处理执行问题。 若要验证某个活动的输出或对错误进行处理，可以使用 PowerShell 代码活动、定义活动的输出链接的条件逻辑，或者应用其他方法。

Azure 自动化图形 Runbook 在改进后可以进行错误处理。 用户现在可以将例外变成非终止性错误，并在活动之间创建错误链接。 此改进的流程使得 runbook 可以捕获错误并管理已实现条件或意外条件。 

>[!NOTE]
>本文进行了更新，以便使用新的 Azure PowerShell Az 模块。 你仍然可以使用 AzureRM 模块，至少在 2020 年 12 月之前，它将继续接收 bug 修补程序。 若要详细了解新的 Az 模块和 AzureRM 兼容性，请参阅[新 Azure Powershell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 有关混合 Runbook 辅助角色上的 Az 模块安装说明，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。 对于自动化帐户，可参阅[如何更新 Azure 自动化中的 Azure PowerShell 模块](automation-update-azure-modules.md)，将模块更新到最新版本。

## <a name="powershell-error-types"></a>PowerShell 错误类型

Runbook 执行期间可能会发生的 PowerShell 错误的类型将终止错误和非终止错误。
 
### <a name="terminating-error"></a>终止错误

终止错误是执行过程中发生的严重错误，该错误会完全停止执行命令或脚本。 示例包括不存在的 cmdlet、阻止某个 cmdlet 运行的语法错误以及其他错误。

### <a name="non-terminating-error"></a>非终止错误

非终止错误是一个不严重的错误，允许继续执行（尽管出现错误）。 示例包括操作错误，如 "找不到文件" 错误和权限问题。

## <a name="when-to-use-error-handling"></a>何时使用错误处理

当关键活动引发错误或异常时，使用 runbook 中的错误处理。 务必要阻止 runbook 中的下一个活动进行处理，并相应地处理错误。 当你的 runbook 支持业务或服务操作过程时，处理错误非常重要。

对于可能产生错误的每个活动，可以添加指向任何其他活动的错误链接。 目标活动可以是任何类型，包括代码活动、cmdlet 的调用、其他 runbook 的调用，等等。 目标活动还可以有传出链接，即常规链接或错误链接。 链接使 runbook 可以实现复杂的错误处理逻辑，而无需使用代码活动。

建议的做法是使用常见功能创建专用错误处理 runbook，但这不是必需的。 例如，假设有一个 runbook 尝试启动虚拟机并在其上安装应用程序。 如果 VM 不能正常启动，则：

1. 发送有关此问题的通知。
2. 启动其他自动预配新 VM 的 runbook。

一种解决方法是在 runbook 中包含一个指向处理第一步的活动的错误链接。 例如，runbook 可以将`Write-Warning` cmdlet 连接到步骤2的活动，例如[AzAutomationRunbook](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.5.0) cmdlet。

你还可以通过将这两个活动放在单独的错误处理 runbook 中来通用化此行为以供许多 runbook 使用。 在原始 runbook 调用此错误处理 runbook 之前，它可以从其数据构造自定义消息，然后将其作为参数传递给错误处理 runbook。

## <a name="how-to-use-error-handling"></a>如何使用错误处理

Runbook 中的每个活动都有一个将异常转换为非终止错误的配置设置。 此设置默认已禁用。 建议在 runbook 处理错误的任何活动上启用此设置。 此设置可确保 runbook 使用错误链接将活动中的终止和非终止错误作为非终止错误处理。  

启用配置设置后，让 runbook 创建处理错误的活动。 如果活动生成任何错误，则会遵循传出错误链接。 不遵循常规链接，即使活动同时生成了常规输出。<br><br> ![自动化 Runbook 错误链接示例](media/automation-runbook-graphical-error-handling/error-link-example.png)

在下面的示例中，runbook 检索包含 VM 的计算机名称的变量。 然后，它会尝试通过下一个活动启动 VM。<br><br> ![自动化 runbook 错误处理示例](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

`Get-AutomationVariable`活动和[new-azvm](https://docs.microsoft.com/powershell/module/Az.Compute/Start-AzVM?view=azps-3.5.0) cmdlet 将配置为将异常转换为错误。 如果获取变量或启动 VM 时出现问题，则代码将生成错误。<br><br> ![自动化 runbook 错误处理活动设置](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)。

错误链接从这些活动流向单个`error management`代码活动。 此活动使用简单的 PowerShell 表达式进行配置，该表达式`throw`使用关键字来停止处理，并`$Error.Exception.Message`使用来获取描述当前异常的消息。<br><br> ![自动化 runbook 错误处理代码示例](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)

## <a name="next-steps"></a>后续步骤

* 若要详细了解图形 Runbook 中的链接和链接类型，请参阅 [Azure 自动化中的图形创作](automation-graphical-authoring-intro.md#links-and-workflow)。

* 若要详细了解 runbook 执行、runbook 作业的监视和其他技术详细信息，请参阅[在 Azure 自动化中执行 runbook](automation-runbook-execution.md)。