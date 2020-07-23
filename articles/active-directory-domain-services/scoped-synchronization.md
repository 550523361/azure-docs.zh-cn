---
title: Azure AD 域服务的作用域同步 |Microsoft Docs
description: 了解如何配置从 Azure AD 到 Azure Active Directory 域服务托管域的作用域同步
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/31/2020
ms.author: iainfou
ms.openlocfilehash: 5f2c823b0932db42876be6ab04ebcd82783729aa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "84734415"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-azure-active-directory-domain-services"></a>将范围内的同步从 Azure AD 配置为 Azure Active Directory 域服务

为了提供身份验证服务，Azure Active Directory 域服务（Azure AD DS）从 Azure AD 同步用户和组。 在混合环境中，可以先使用 Azure AD Connect 将本地 Active Directory 域服务（AD DS）环境中的用户和组同步到 Azure AD，然后同步到 Azure AD DS。

默认情况下，Azure AD 目录中的所有用户和组都将同步到 Azure AD DS 托管域。 如果有特定需求，可以改为选择只同步定义的一组用户。

本文说明如何创建使用范围内同步的托管域，然后更改或禁用作用域内用户集。

## <a name="scoped-synchronization-overview"></a>作用域同步概述

默认情况下，Azure AD 目录中的所有用户和组都将同步到托管域。 如果只有少数用户需要访问托管域，则只能同步这些用户帐户。 此作用域内同步是基于组的。 配置基于组的作用域同步时，只有属于指定组的用户帐户才会同步到托管域。

下表概述了如何使用作用域内同步：

| 当前状态 | 所需状态 | 必需配置 |
| --- | --- | --- |
| 将现有托管域配置为同步所有用户帐户和组。 | 只需同步属于特定组的用户帐户。 | 不能从同步所有用户更改为使用范围内的同步。 [删除现有托管域](delete-aadds.md)，然后按照本文中的步骤操作，以重新创建配置了作用域同步的托管域。 |
| 无现有托管域。 | 希望新建托管域，并仅同步属于特定组的用户帐户。 | 按照本文中的步骤操作，创建配置了作用域同步的托管域。 |
| 现有托管域配置为仅同步属于特定组的帐户。 | 你想要修改其用户应同步到托管域的组的列表。 | 按照本文中的步骤修改范围内的同步。 |

使用 Azure 门户或 PowerShell 配置作用域内同步设置：

| 操作 | | |
|--|--|--|
| 创建托管域并配置作用域同步 | [Azure 门户](#enable-scoped-synchronization-using-the-azure-portal) | [PowerShell](#enable-scoped-synchronization-using-powershell) |
| 修改作用域同步 | [Azure 门户](#modify-scoped-synchronization-using-the-azure-portal) | [PowerShell](#modify-scoped-synchronization-using-powershell) |
| 禁用范围内同步 | [Azure 门户](#disable-scoped-synchronization-using-the-azure-portal) | [PowerShell](#disable-scoped-synchronization-using-powershell) |

> [!WARNING]
> 更改同步范围会导致托管域重新同步所有数据。 请注意以下事项：
> 
>  * 如果你更改托管域的同步范围，便会发生完全重新同步。
>  * 托管域中不再需要的对象将被删除。 托管域中会新建对象。
>  * 重新同步可能需要很长时间才能完成。 同步时间取决于对象的数量，例如托管域中的用户、组和组成员身份，以及 Azure AD 目录。 对于包含数十万个对象的大型目录，重新同步可能需要几天时间才能完成。

## <a name="enable-scoped-synchronization-using-the-azure-portal"></a>使用 Azure 门户启用作用域同步

若要在 Azure 门户中启用作用域同步，请完成以下步骤：

1. 按照[教程创建和配置托管域](tutorial-create-instance-advanced.md)。 完成除同步作用域外的所有先决条件和部署步骤。
1. 在同步步骤中选择 "**作用域**"，然后选择要同步到托管域的 Azure AD 组。

托管域最多可能需要一小时才能完成部署。 在 Azure 门户中，托管域的“概览”页会显示整个部署阶段的当前状态。

当 Azure 门户显示托管域已完成预配时，需要完成以下任务：

* 为虚拟网络更新 DNS 设置，以使虚拟机能够找到用于域加入或身份验证的托管域。
    * 若要配置 DNS，请在门户中选择你的托管域。 在“概览”窗口中，系统会提示你自动配置这些 DNS 设置。
* [启用 Azure AD 域服务的密码同步](tutorial-create-instance-advanced.md#enable-user-accounts-for-azure-ad-ds)，使最终用户能够使用其企业凭据登录到托管域。

## <a name="modify-scoped-synchronization-using-the-azure-portal"></a>使用 Azure 门户修改范围内的同步

若要修改其用户应同步到托管域的组的列表，请完成以下步骤：

1. 在 Azure 门户中，搜索并选择**Azure AD 域服务**"。 选择你的托管域，例如 *aaddscontoso.com*。
1. 从左侧菜单中选择 "**同步**"。
1. 若要添加组，请选择顶部的 " **+ 选择组**"，然后选择要添加的组。
1. 若要从同步作用域中删除组，请从当前同步的组列表中选择它，然后选择 "**删除组**"。
1. 进行所有更改后，选择 "**保存同步范围**"。

更改同步范围会导致托管域重新同步所有数据。 托管域中不再需要的对象将被删除，重新同步可能需要很长时间才能完成。

## <a name="disable-scoped-synchronization-using-the-azure-portal"></a>使用 Azure 门户禁用范围内同步

若要禁用托管域的基于组的作用域同步，请完成以下步骤：

1. 在 Azure 门户中，搜索并选择**Azure AD 域服务**"。 选择你的托管域，例如 *aaddscontoso.com*。
1. 从左侧菜单中选择 "**同步**"。
1. 将 "同步作用域"**设置为 "** **全部**作用域"，然后选择 "**保存同步作用域**"。

更改同步范围会导致托管域重新同步所有数据。 托管域中不再需要的对象将被删除，重新同步可能需要很长时间才能完成。

## <a name="powershell-script-for-scoped-synchronization"></a>作用域内同步的 PowerShell 脚本

若要使用 PowerShell 配置作用域同步，请首先将以下脚本保存到名为的文件 `Select-GroupsToSync.ps1` 。 此脚本将 Azure AD DS 配置为从 Azure AD 同步所选组。 属于指定组的所有用户帐户将同步到托管域。

此脚本用于本文中的其他步骤。

```powershell
param (
    [Parameter(Position = 0)]
    [String[]]$groupsToAdd
)

Connect-AzureAD
$sp = Get-AzureADServicePrincipal -Filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
$role = $sp.AppRoles | where-object -FilterScript {$_.DisplayName -eq "User"}

Write-Output "`n****************************************************************************"

Write-Output "Total group-assignments need to be added: $($groupsToAdd.Count)"
$newGroupIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($groupName in $groupsToAdd)
{
    try
    {
        $group = Get-AzureADGroup -Filter "DisplayName eq '$groupName'"
        $newGroupIds.Add($group.ObjectId)

        Write-Output "Group-Name: $groupName, Id: $($group.ObjectId)"
    }
    catch
    {
        Write-Error "Failed to find group: $groupName. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

$currentAssignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId
Write-Output "Total current group-assignments: $($currentAssignments.Count), SP-ObjectId: $($sp.ObjectId)"

$currAssignedObjectIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($assignment in $currentAssignments)
{
    Write-Output "Assignment-ObjectId: $($assignment.PrincipalId)"

    if ($newGroupIds.Contains($assignment.PrincipalId) -eq $false)
    {
        Write-Output "This assignment is not needed anymore. Removing it! Assignment-ObjectId: $($assignment.PrincipalId)"
        Remove-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -AppRoleAssignmentId $assignment.ObjectId
    }
    else
    {
        $currAssignedObjectIds.Add($assignment.PrincipalId)
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

foreach ($id in $newGroupIds)
{
    try
    {
        if ($currAssignedObjectIds.Contains($id) -eq $false)
        {
            Write-Output "Adding new group-assignment. Role-Id: $($role.Id), Group-Object-Id: $id, ResourceId: $($sp.ObjectId)"
            New-AzureADGroupAppRoleAssignment -Id $role.Id -ObjectId $id -PrincipalId $id -ResourceId $sp.ObjectId
        }
        else
        {
            Write-Output "Group-ObjectId: $id is already assigned."
        }
    }
    catch
    {
        Write-Error "Exception occurred assigning Object-ID: $id. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
```

## <a name="enable-scoped-synchronization-using-powershell"></a>使用 PowerShell 启用作用域同步

使用 PowerShell 完成以下步骤。 请参阅[使用 PowerShell 启用 Azure Active Directory 域服务](powershell-create-instance.md)中的说明。 为了配置范围内同步，我们对这篇文章中的几个步骤略作修改。

1. 完成文章中的以下任务，使用 PowerShell 启用 Azure AD DS。 请在创建托管域的步骤停止。 你可以配置创建托管域的作用域同步。

   * [安装所需的 PowerShell 模块](powershell-create-instance.md#prerequisites)。
   * [创建所需的服务主体和 Azure AD 组以实现管理访问权限](powershell-create-instance.md#create-required-azure-ad-resources)。
   * [创建支持的 Azure 资源，如虚拟网络和子网](powershell-create-instance.md#create-supporting-azure-resources)。

1. 确定要从 Azure AD 同步的组和用户。 创建要同步到 Azure AD DS 的组的显示名称列表。

1. 运行[上一节中的脚本](#powershell-script-for-scoped-synchronization)，并使用 *-groupsToAdd*参数传递要同步的组的列表。

   > [!WARNING]
   > 对于作用域同步，必须在组列表中包含*AAD DC 管理员*组。 如果不包含此组，则托管域不可用。

   ```powershell
   .\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName2")
   ```

1. 现在，创建托管域并启用基于组的作用域同步。 在 *-Properties*参数中包括 *"filteredSync" = "Enabled"* 。

    设置 Azure 订阅 ID，然后提供托管域的名称，例如 aaddscontoso.com。 可以使用 [Get-AzSubscription][Get-AzSubscription] cmdlet 获取订阅 ID。 将资源组名称、虚拟网络名称和区域设置为之前步骤中使用的值，以创建支持的 Azure 资源：

   ```powershell
   $AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
   $ManagedDomainName = "aaddscontoso.com"
   $ResourceGroupName = "myResourceGroup"
   $VnetName = "myVnet"
   $AzureLocation = "westus"

   # Enable Azure AD Domain Services for the directory.
   New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
   -Location $AzureLocation `
   -Properties @{"DomainName"=$ManagedDomainName; "filteredSync" = "Enabled"; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
   -Force -Verbose
   ```

创建资源并将控制权返回给 PowerShell 提示符需要花费几分钟时间。 托管域将在后台继续预配，完成部署最长可能需要一小时。 在 Azure 门户中，托管域的“概览”页会显示整个部署阶段的当前状态。

当 Azure 门户显示托管域已完成预配时，需要完成以下任务：

* 为虚拟网络更新 DNS 设置，以使虚拟机能够找到用于域加入或身份验证的托管域。
    * 若要配置 DNS，请在门户中选择你的托管域。 在“概览”窗口中，系统会提示你自动配置这些 DNS 设置。
* 如果在支持可用性区域的区域中创建了托管域，请创建一个网络安全组，仅限该托管域在虚拟网络中传送流量。 创建一个要求实施这些规则的 Azure 标准负载均衡器。 此网络安全组会保护 Azure AD DS，是托管域正常运行所需的。
    * 若要创建网络安全组和所需的规则，请在门户中选择你的托管域。 “概览”窗口中会提示你自动创建并配置网络安全组。
* [启用 Azure AD 域服务的密码同步](tutorial-create-instance-advanced.md#enable-user-accounts-for-azure-ad-ds)，使最终用户能够使用其企业凭据登录到托管域。

## <a name="modify-scoped-synchronization-using-powershell"></a>使用 PowerShell 修改作用域同步

若要修改其用户应同步到托管域的组的列表，请重新运行[PowerShell 脚本](#powershell-script-for-scoped-synchronization)并指定新的组列表。 在下面的示例中，要同步的组不再包括*GroupName2*，现在包括*GroupName3*。

> [!WARNING]
> 对于作用域同步，必须在组列表中包含*AAD DC 管理员*组。 如果不包含此组，则托管域不可用。

```powershell
.\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName3")
```

更改同步范围会导致托管域重新同步所有数据。 托管域中不再需要的对象将被删除，重新同步可能需要很长时间才能完成。

## <a name="disable-scoped-synchronization-using-powershell"></a>使用 PowerShell 禁用作用域同步

若要禁用托管域的基于组的作用域同步，请在 Azure AD DS 资源上设置 *"filteredSync" = "Disabled"* ，然后更新托管域。 完成后，所有用户和组都设置为从 Azure AD 同步。

```powershell
// Retrieve the Azure AD DS resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

// Update the Azure AD DS resource
Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

更改同步范围会导致托管域重新同步所有数据。 托管域中不再需要的对象将被删除，重新同步可能需要很长时间才能完成。

## <a name="next-steps"></a>后续步骤

若要了解有关同步过程的详细信息，请参阅[了解 Azure AD 域服务中的同步](synchronization.md)。

<!-- EXTERNAL LINKS -->
[Get-AzSubscription]: /powershell/module/Az.Accounts/Get-AzSubscription
