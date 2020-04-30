---
title: 使用 Azure 自动化将服务器配置为所需状态并管理偏移
description: 教程 - 使用 Azure 自动化状态配置管理服务器配置
services: automation
ms.subservice: dsc
ms.topic: conceptual
ms.date: 08/08/2018
ms.openlocfilehash: a02c664ddf0802ad5ac306f98de14b7c0d5d7271
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678710"
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>将服务器配置到所需状态并管理偏移

使用 Azure 自动化状态配置可以指定服务器配置，并确保这些服务器在一段时间后处于指定状态。

> [!div class="checklist"]
> - 登记要由 Azure Automation DSC 管理的 VM
> - 将配置上传到 Azure 自动化
> - 将配置编译为节点配置
> - 将节点配置分配给托管节点
> - 检查托管节点的符合性状态

对于本教程，我们将使用一个简单的[DSC 配置](/powershell/scripting/dsc/configurations/configurations)，以确保在 VM 上安装 IIS。

>[!NOTE]
>本文进行了更新，以便使用新的 Azure PowerShell Az 模块。 你仍然可以使用 AzureRM 模块，至少在 2020 年 12 月之前，它将继续接收 bug 修补程序。 若要详细了解新的 Az 模块和 AzureRM 兼容性，请参阅[新 Azure Powershell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 有关混合 Runbook 辅助角色上的 Az 模块安装说明，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。 对于自动化帐户，可参阅[如何更新 Azure 自动化中的 Azure PowerShell 模块](automation-update-azure-modules.md)，将模块更新到最新版本。

## <a name="prerequisites"></a>先决条件

要完成本教程，需要：

- 一个 Azure 自动化帐户。 有关如何创建 Azure 自动化运行方式帐户的说明，请参阅 [Azure 运行方式帐户](automation-sec-configure-azure-runas-account.md)。
- 运行 Windows Server 2008 R2 或更高版本的 Azure 资源管理器 VM （非经典）。 有关创建 VM 的说明，请参阅[在 Azure 门户中创建第一个 Windows 虚拟机](../virtual-machines/virtual-machines-windows-hero-tutorial.md)。
- Azure PowerShell 模块 3.6 版或更高版本。 运行 `Get-Module -ListAvailable Az` 即可查找版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](/powershell/azure/azurerm/install-azurerm-ps)（安装 Azure PowerShell 模块）。
- 熟悉所需状态配置 (DSC)。 有关 DSC 的详细信息，请参阅[Windows PowerShell Desired State Configuration 概述](/powershell/scripting/dsc/overview/overview)。

## <a name="support-for-partial-configurations"></a>支持部分配置

Azure 自动化状态配置支持使用[部分配置](/powershell/scripting/dsc/pull-server/partialconfigs)。 在此方案中，DSC 配置为独立管理多个配置，并且每个配置都从 Azure 自动化中检索。 但是，每个自动化帐户只能为一个节点分配一个配置。 这意味着，如果使用节点的两个配置，则需要两个自动化帐户。

有关如何从请求服务注册部分配置的详细信息，请参阅[部分配置](https://docs.microsoft.com/powershell/scripting/dsc/pull-server/partialconfigs#partial-configurations-in-pull-mode)的文档。

若要详细了解团队如何协同工作以使用配置作为代码协作管理服务器，请参阅[了解 DSC 在 CI/CD 管道中的角色](/powershell/scripting/dsc/overview/authoringadvanced)。

## <a name="log-in-to-azure"></a>登录 Azure

通过[AzAccount](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount?view=azps-3.7.0) cmdlet 登录到 Azure 订阅，并按照屏幕上的说明进行操作。

```powershell
Connect-AzAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>创建配置并将配置上传到 Azure 自动化


在文本编辑器中，键入以下内容并将其保存到本地**TestConfig**。

```powershell
configuration TestConfig {
   Node WebServer {
      WindowsFeature IIS {
         Ensure               = 'Present'
         Name                 = 'Web-Server'
         IncludeAllSubFeature = $true
      }
   }
}
```

> [!NOTE]
> 在需要导入多个提供 DSC 资源的模块的更高级方案中，请确保每个模块在配置中都有唯一的 `Import-DscResource` 行。

调用[AzAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/Az.Automation/Import-AzAutomationDscConfiguration?view=azps-3.7.0) cmdlet，将配置上传到自动化帐户。

```powershell
 Import-AzAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>将配置编译为节点配置

必须先将 DSC 配置编译为节点配置，然后才能将它分配给节点。 参阅 [DSC 配置](/powershell/scripting/dsc/configurations/configurations)。

调用[AzAutomationDscCompilationJob](https://docs.microsoft.com/powershell/module/Az.Automation/Start-AzAutomationDscCompilationJob?view=azps-3.7.0) cmdlet，将`TestConfig`配置编译为自动化帐户中名为`TestConfig.WebServer`的节点配置。

```powershell
Start-AzAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

## <a name="register-a-vm-to-be-managed-by-state-configuration"></a>注册要由状态配置管理的 VM

可以使用 Azure 自动化状态配置来管理 Azure VM（包括经典 VM 和资源管理器 VM）、本地 VM、Linux 计算机、AWS VM，以及本地物理机。 在本主题中，我们介绍如何仅注册 Azure 资源管理器 VM。 有关注册其他类型的计算机的信息，请参阅[登记由 Azure 自动化状态配置管理的计算机](automation-dsc-onboarding.md)。

调用[AzAutomationDscNode](https://docs.microsoft.com/powershell/module/Az.Automation/Register-AzAutomationDscNode?view=azps-3.7.0) cmdlet，将 VM 作为托管节点注册到 Azure 自动化状态配置。 

```powershell
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm'
```

### <a name="specify-configuration-mode-settings"></a>指定配置模式设置

使用[AzAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) CMDLET 将 VM 注册为托管节点，并指定配置属性。 例如，你可以通过将指定`ApplyOnly`为`ConfigurationMode`属性的值来指定仅应用计算机的状态一次。 状态配置在初始检查后不会尝试应用配置。

```powershell
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationMode 'ApplyOnly'
```

还可以通过使用`ConfigurationModeFrequencyMins`属性来指定 DSC 检查配置状态的频率。 有关 DSC 配置设置的详细信息，请参阅[配置本地配置管理器](/powershell/scripting/dsc/managing-nodes/metaConfig)。

```powershell
# Run a DSC check every 60 minutes
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationModeFrequencyMins 60
```

## <a name="assign-a-node-configuration-to-a-managed-node"></a>将节点配置分配给托管节点

现在我们可以将已编译的节点配置分配给我们想要配置的 VM。

```powershell
# Get the ID of the DSC node
$node = Get-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -NodeId $node.Id
```

这会将名`TestConfig.WebServer`为的节点配置分配给已`DscVm`注册的 DSC 节点。 默认情况下，每隔 30 分钟会检查一次 DSC 节点是否符合节点配置。 有关如何更改符合性检查间隔的信息，请参阅[配置本地 Configuration Manager](/powershell/scripting/dsc/managing-nodes/metaConfig)。

## <a name="check-the-compliance-status-of-a-managed-node"></a>检查托管节点的符合性状态

可以使用[AzAutomationDscNodeReport](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationDscNodeReport?view=azps-3.7.0) cmdlet 获取有关托管节点的符合性状态的报告。

```powershell
# Get the ID of the DSC node
$node = Get-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeId $node.Id

# Display the most recent report
$reports[0]
```

## <a name="remove-nodes-from-service"></a>从服务中删除节点

当节点添加到 Azure 自动化 State Configuration 时，本地配置管理器中的设置会设置为注册到服务，并拉取配置和所需的模块，以配置计算机。
如果选择从服务中删除节点，则可以使用 Azure 门户或 Az cmdlet 进行删除。

> [!NOTE]
> 从服务注销节点只会设置本地配置管理器设置，这样，该节点便不再连接到该服务。
> 这不影响当前应用于该节点的配置。
> 若要删除当前配置，请使用 [PowerShell](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/remove-dscconfigurationdocument?view=powershell-5.1) 或删除本地配置文件（这是适用于 Linux 节点的唯一选项）。

### <a name="azure-portal"></a>Azure 门户

在 Azure 自动化中，单击目录中的“状态配置(DSC)”****。
接下来，单击“节点”****，以查看注册到服务的节点的列表。
单击要删除的节点的名称。
在打开的“节点”视图中，单击“注销”****。

### <a name="powershell"></a>PowerShell

若要使用 PowerShell 从 Azure 自动化 State Configuration 服务中注销节点，请按照 cmdlet [Unregister-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-2.0.0) 的文档进行操作。

## <a name="next-steps"></a>后续步骤

- 若要开始，请参阅[Azure 自动化状态配置](automation-dsc-getting-started.md)入门。
- 若要了解如何装入节点，请参阅[通过 Azure 自动化状态配置载入计算机进行管理](automation-dsc-onboarding.md)。
- 若要了解如何编译 DSC 配置，以便可以将其分配给目标节点，请参阅[在 Azure 自动化状态配置中编译配置](automation-dsc-compile.md)。
- 有关 PowerShell cmdlet 参考，请参阅[Azure 自动化状态配置 cmdlet](/powershell/module/azurerm.automation/#automation)。
- 有关定价信息，请参阅[Azure 自动化状态配置定价](https://azure.microsoft.com/pricing/details/automation/)。
- 若要查看在持续部署管道中使用 Azure Automation State Configuration 的示例，请参阅[使用 Azure Automation State Configuration 和 Chocolatey 进行持续部署](automation-dsc-cd-chocolatey.md)
