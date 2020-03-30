---
title: 在 Azure Automation State Configuration 中编译配置
description: 本文介绍如何编译 Azure 自动化的 Desired State Configuration (DSC) 配置。
services: automation
ms.subservice: dsc
ms.date: 09/10/2018
ms.topic: conceptual
ms.openlocfilehash: 48593920bdfcf743fceaeaeec891c0d5c4f2e108
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "80057639"
---
# <a name="compiling-dsc-configurations-in-azure-automation-state-configuration"></a>在 Automation State Configuration 中编译 DSC 配置

您可以通过以下方式在 Azure 自动化状态配置中编译所需的状态配置 （DSC） 配置：

- Azure State Configuration 编译服务
  - 使用交互式用户界面的初级方法
   - 轻松跟踪作业状态

- Windows PowerShell
  - 从本地工作站上的 Windows PowerShell 调用或生成服务
  - 与开发测试管道集成
  - 提供复杂的参数值
  - 大规模使用节点和非节点数据
  - 显著的性能提升

有关编译详细信息，请参阅[Azure 资源管理器模板所需的状态配置扩展](https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-template#details)。

## <a name="compiling-a-dsc-configuration-in-azure-state-configuration"></a>在 Azure 状态配置中编译 DSC 配置

### <a name="portal"></a>门户

1. 从自动化帐户中，单击“State Configuration (DSC)”****。
1. 单击“配置”选项卡，然后单击要编译的配置名称。****
1. 单击“编译”****。
1. 如果配置没有参数，系统将提示您确认是否要编译该配置。 如果配置具有参数，则将打开 **"编译配置"** 边栏选项卡，以便提供参数值。
1. 将打开"编译作业"页，以便可以跟踪编译作业状态。 您还可以使用此页面跟踪放置在 Azure 自动化状态配置拉取服务器上的节点配置（MOF 配置文档）。

### <a name="azure-powershell"></a>Azure PowerShell

您可以使用[启动-Az自动化编译作业](/powershell/module/az.automation/start-azautomationdsccompilationjob)开始使用 Windows PowerShell 编译。 以下示例代码开始编译称为示例配置的 DSC 配置。

```powershell
Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'
```

**启动-AzAutomationDsc编译作业**返回可用于跟踪作业状态的编译作业对象。 然后，您可以使用此编译作业对象与[Get-AzAutomationDsc 编译作业](/powershell/module/az.automation/get-azautomationdsccompilationjob)来确定编译作业的状态，[然后获取-AzAutomationD编译作业输出](/powershell/module/az.automation/get-azautomationdscconfiguration)以查看其流（输出）。 以下示例开始编译 SampleConfig 配置，等待它完成，然后显示其流。

```powershell
$CompilationJob = Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'

while($null -eq $CompilationJob.EndTime -and $null -eq $CompilationJob.Exception)
{
    $CompilationJob = $CompilationJob | Get-AzAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzAutomationDscCompilationJobOutput –Stream Any
```

### <a name="declare-basic-parameters"></a>声明基本参数

DSC 配置中的参数声明（包括参数类型和属性）的工作方式与 Azure 自动化 Runbook 中相同。 若要了解有关 Runbook 参数的详细信息，请参阅 [Starting a runbook in Azure Automation](automation-starting-a-runbook.md)（在 Azure 自动化中启动 Runbook）。

以下示例使用名为 *FeatureName* 和 *IsPresent* 的两个参数来确定在编译期间生成的 ParametersExample.sample 节点配置中的属性值。

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]
        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = 'Present'
    if($IsPresent -eq $false)
    {
        $EnsureString = 'Absent'
    }

    Node 'sample'
    {
        WindowsFeature ($FeatureName + 'Feature')
        {
            Ensure = $EnsureString
            Name   = $FeatureName
        }
    }
}
```

可以编译在 Azure 自动化状态配置门户或与 Azure PowerShell 中使用基本参数的 DSC 配置。

#### <a name="portal"></a>门户

在门户中，可在单击“编译”**** 后输入参数值。

![配置编译参数](./media/automation-dsc-compile/DSC_compiling_1.png)

#### <a name="azure-powershell"></a>Azure PowerShell

PowerShell 需要[哈希表中](/powershell/module/microsoft.powershell.core/about/about_hash_tables)的参数，其中键与参数名称匹配，值等于参数值。

```powershell
$Parameters = @{
    'FeatureName' = 'Web-Server'
    'IsPresent' = $False
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ParametersExample' -Parameters $Parameters
```

若要了解如何将 PSCredentials 作为参数传递，请参阅下面的[凭据资产](#credential-assets)。

### <a name="compile-configurations-containing-composite-resources-in-azure-automation"></a>编译包含 Azure 自动化中复合资源的配置

**复合资源**功能允许您将 DSC 配置用作配置中的嵌套资源。 此功能支持将多个配置应用于单个资源。 请参阅[复合资源：使用 DSC 配置作为资源](/powershell/scripting/dsc/resources/authoringresourcecomposite)来了解有关复合资源的更多信息。

> [!NOTE]
> 因此，包含复合资源的配置必须首先导入 Azure 自动化，以将复合依赖于的任何 DSC 资源导入 Azure 自动化。 添加 DSC 复合资源与向 Azure 自动化添加任何 PowerShell 模块没有什么不同。 此过程记录[在 Azure 自动化中的管理模块中](/azure/automation/shared-resources/modules)。

### <a name="manage-configurationdata-when-compiling-configurations-in-azure-automation"></a>在 Azure 自动化中编译配置时管理配置数据

通过 **ConfigurationData** 可在使用 PowerShell DSC 时分开结构化配置与任何环境特定配置。 有关详细信息，请参阅在[PowerShell DSC 中将"什么"与"地点"分开](https://devblogs.microsoft.com/powershell/separating-what-from-where-in-powershell-dsc/)。

> [!NOTE]
> 在 Azure 自动化状态配置中编译时，可以在 Azure PowerShell 中使用配置数据，但不能在 Azure 门户中使用**配置数据**。

以下示例 DSC 配置通过 $ConfigurationData 和 $AllNodes 关键字来使用 **ConfigurationData**。 对于此示例，您还需要[xWeb 管理模块](https://www.powershellgallery.com/packages/xWebAdministration/)。

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq 'WebServer'}.NodeName
    {
        xWebsite Site
        {
            Name         = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure       = 'Present'
        }
    }
}
```

可以使用 Windows PowerShell 编译上述 DSC 配置。 以下脚本将两个节点配置添加到 Azure 自动化状态配置拉取服务：配置数据示例.MyVM1 和配置数据示例.MyVM3。

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = 'MyVM1'
            Role = 'WebServer'
        },
        @{
            NodeName = 'MyVM2'
            Role = 'SQLServer'
        },
        @{
            NodeName = 'MyVM3'
            Role = 'WebServer'
        }
    )

    NonNodeData = @{
        SomeMessage = 'I love Azure Automation State Configuration and DSC!'
    }
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ConfigurationDataSample' -ConfigurationData $ConfigData
```

### <a name="work-with-assets-in-azure-automation-during-compilation"></a>在编译期间处理 Azure 自动化中的资产

在 Azure 自动化状态配置和 Runbook 中，资产引用都是相同的。 有关详细信息，请参阅以下主题：

- [证书](automation-certificates.md)
- [连接](automation-connections.md)
- [凭据](automation-credentials.md)
- [变量](automation-variables.md)

#### <a name="credential-assets"></a>凭据资产

Azure 自动化中的 DSC 配置可以使用**获取自动化 PS凭据**cmdlet 引用自动化凭据资产。 如果配置具有指定**PS凭据**对象的参数，请使用**获取自动化PS凭据**，通过将 Azure 自动化凭据资产的字符串名称传递给 cmdlet 来检索凭据。 然后，将该对象用于需要**PSCredential**对象的参数。 在后台将检索具有该名称的 Azure 自动化凭据资产并将其传递给配置。 下面的示例显示了此方案在操作中。

要在节点配置（MOF 配置文档）中保持凭据的安全，需要在节点配置 MOF 文件中为凭据加密。 目前，您必须授予 PowerShell DSC 在节点配置 MOF 生成期间以纯文本输出凭据的权限。 PowerShell DSC 不知道 Azure 自动化在生成整个 MOF 文件后通过编译作业对其进行加密。

可告知 PowerShell DSC，使用配置数据在生成的节点配置 MOF 中以纯文本形式输出凭据是可行的。 您应该`PSDscAllowPlainTextPassword = $true`通过**配置数据来**传递 DSC 配置中显示的每个节点块名称并使用凭据。

以下示例演示使用自动化凭据资产的 DSC 配置。

```powershell
Configuration CredentialSample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    $Cred = Get-AutomationPSCredential 'SomeCredentialAsset'

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath      = '\\Server\share\path\file.ext'
            DestinationPath = 'C:\destinationPath'
            Credential      = $Cred
        }
    }
}
```

可以使用 PowerShell 编译上述 DSC 配置。 以下 PowerShell 代码将两个节点配置添加到 Azure 自动化状态配置拉取服务器：凭据示例.MyVM1 和凭据示例.MyVM2。

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = '*'
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = 'MyVM1'
        },
        @{
            NodeName = 'MyVM2'
        }
    )
}

Start-AzAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'CredentialSample' -ConfigurationData $ConfigData
```

> [!NOTE]
> 编译完成后，您可能会收到错误消息"'Microsoft.PowerShell.Management'"模块未导入，因为"Microsoft.PowerShell.管理"的卡入式已导入。 可以放心忽略此消息。

## <a name="compiling-your-dsc-configuration-in-windows-powershell"></a>在 Windows PowerShell 中编译 DSC 配置

还可以导入已在 Azure 外部编译的节点配置 (MOF)。 导入包括来自开发人员工作站或服务（如[Azure DevOps）](https://dev.azure.com)的编译。 这种方法具有多种优点，包括性能和可靠性。

在 Windows PowerShell 中编译还提供了签署配置内容的选项。 DSC 代理在托管节点上本地验证已签名的节点配置。 验证可确保应用于节点的配置来自授权源。

> [!NOTE]
> 节点配置文件不得大于 1 MB，以便将其导入 Azure 自动化。

有关节点配置签名的详细信息，请参阅[WMF 5.1 中的改进 - 如何对配置和模块进行签名](/powershell/scripting/wmf/whats-new/dsc-improvements#dsc-module-and-configuration-signing-validations)。

### <a name="compile-the-dsc-configuration"></a>编译 DSC 配置

在 Windows PowerShell 中编译 DSC 配置的过程包含在以下 PowerShell DSC 文档中：[编写、编译和应用配置](/powershell/scripting/dsc/configurations/write-compile-apply-configuration#compile-the-configuration)。
可以从开发人员工作站或在生成服务（如[Azure DevOps）](https://dev.azure.com)中执行此过程。 然后，可以通过将配置编译到 Azure 状态配置服务来导入生成的 MOF 文件。

### <a name="import-a-node-configuration-in-the-azure-portal"></a>在 Azure 门户中导入节点配置

1. 在“自动化帐户”中的“配置管理”下，单击“State Configuration (DSC)”。********
1. 在"状态配置 （DSC）"页上，单击"**配置"** 选项卡，然后单击 **"添加**"。
1. 在"导入"页上，单击 **"节点配置文件"** 文本框旁边的文件夹图标，以浏览本地计算机上的节点配置文件 （MOF）。

   ![浏览本地文件](./media/automation-dsc-compile/import-browse.png)

1. 在 **"配置名称"** 字段中输入名称。 此名称必须与编译节点配置的配置名称匹配。
1. 单击“确定”。

### <a name="import-a-node-configuration-with-azure-powershell"></a>使用 Azure PowerShell 导入节点配置

您可以使用[导入-AzAutomationDscNode配置](/powershell/module/az.automation/import-azautomationdscnodeconfiguration)cmdlet 将节点配置导入到自动化帐户中。

```powershell
Import-AzAutomationDscNodeConfiguration -AutomationAccountName 'MyAutomationAccount' -ResourceGroupName 'MyResourceGroup' -ConfigurationName 'MyNodeConfiguration' -Path 'C:\MyConfigurations\TestVM1.mof'
```

## <a name="next-steps"></a>后续步骤

- 要开始，请参阅[开始使用 Azure 自动化状态配置](automation-dsc-getting-started.md)。
- 要了解如何编译 DSC 配置以便将它们分配给目标节点，请参阅[在 Azure 自动化状态配置中编译配置](automation-dsc-compile.md)。
- 有关 PowerShell cmdlet 引用，请参阅[Azure 自动化状态配置 cmdlet](/powershell/module/az.automation)。
- 有关定价信息，请参阅[Azure 自动化状态配置定价](https://azure.microsoft.com/pricing/details/automation/)。
- 要查看在连续部署管道中使用 Azure 自动化状态配置的示例，请参阅[使用 Azure 自动化状态配置和巧克力 连续部署到虚拟机](automation-dsc-cd-chocolatey.md)。
