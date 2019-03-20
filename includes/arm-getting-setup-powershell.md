---
author: sdwheeler
ms.service: azure-powershell
ms.topic: include
ms.date: 11/09/2018
ms.author: sewhee
ms.openlocfilehash: c5440555c11d98fb89f8594eec1d4b7e74ea8667
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2019
ms.locfileid: "58115357"
---
## <a name="setting-up-powershell-for-resource-manager-templates"></a>为 Resource Manager 模板设置 PowerShell
必须拥有正确的 Windows PowerShell 和 Azure PowerShell 版本，才能将 Azure PowerShell 与 Resource Manager 配合使用。

### <a name="verify-powershell-versions"></a>验证 PowerShell 版本
确保安装有 Windows PowerShell 3.0 或 4.0 版。 若要查找 Windows PowerShell 版本，请在 Windows PowerShell 命令提示符下键入以下命令。

    $PSVersionTable

将收到以下类型的信息：

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


确保 **PSVersion** 的值为 3.0 或 4.0。 如果不是，请参阅 [Windows Management Framework 3.0](https://www.microsoft.com/download/details.aspx?id=34595) 或 [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855)。

### <a name="set-your-azure-account-and-subscription"></a>设置 Azure 帐户和订阅
如果还没有 Azure 订阅，可以激活 [MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或注册获取[免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

打开 Azure PowerShell 命令提示符，并运行以下命令登录 Azure。

    Connect-AzureRmAccount

如果有多个 Azure 订阅，可使用以下命令列出 Azure 订阅。

    Get-AzureRmSubscription

将收到以下类型的信息：

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

可通过在 Azure PowerShell 命令提示符下运行以下命令设置当前的 Azure 订阅。 将引号内的所有内容（包括 < and > 字符）替换为正确的名称。

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

有关 Azure 订阅和帐户的详细信息，请参阅[如何：连接到订阅](/powershell/azureps-cmdlets-docs#step-3-connect)。
