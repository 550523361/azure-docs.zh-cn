---
title: 将 Azure 帐户链接到合作伙伴 ID | Microsoft Docs
description: 通过将合作伙伴 ID 链接到用于管理客户资源的用户帐户来跟踪 Azure 客户的互动。
services: billing
author: dhirajgandhi
manager: dhgandhi
ms.author: banders
ms.date: 03/12/2018
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: c38b28a247feb94efd5f4b73e690d30aac9ed73a
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2019
ms.locfileid: "54900232"
---
# <a name="link-a-partner-id-to-your-azure-accounts"></a>将合作伙伴 ID 链接到 Azure 帐户

作为合作伙伴，可以在客户互动中跟踪影响。 可以将合作伙伴 ID 链接到用于管理客户资源的帐户。

此功能目前以公共预览版提供。

## <a name="get-access-from-your-customer"></a>从客户获取访问权限

在你链接合作伙伴 ID 之前，客户必须使用以下选项之一，授权你访问其 Azure 资源：

- **来宾用户**：客户可将你添加为来宾用户并分配任何基于角色的访问控制 (RBAC) 角色。 有关详细信息，请参阅[添加另一个目录中的来宾用户](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)。

- **目录帐户**：客户可以为你在其自己的目录中创建用户帐户，并分配任何 RBAC 角色。

- **服务主体**：客户可在其目录中为你的组织添加一个应用或脚本，并分配任何 RBAC 角色。 该应用或脚本的标识称为服务主体。

## <a name="link-to-a-partner-id"></a>链接到合作伙伴 ID

获取客户资源的访问权限后，请使用 Azure 门户、PowerShell 或 Azure CLI 将 Microsoft 合作伙伴网络 ID (MPN ID) 链接到用户 ID 或服务主体。 链接每个客户租户中的合作伙伴 ID。

### <a name="use-the-azure-portal-to-link-to-a-new-partner-id"></a>使用 Azure 门户链接新合作伙伴 ID

1. 可以在 Azure 门户中[链接到合作伙伴 ID](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade)。

2. 登录到 Azure 门户。

3. 输入 Microsoft 合作伙伴 ID。 合作伙伴 ID 是组织的 [Microsoft 合作伙伴网络](https://partner.microsoft.com/) ID。

   ![显示链接到合作伙伴 ID 的屏幕截图](./media/billing-link-partner-id/link-partner-ID.PNG)

4. 若要链接另一个客户的合作伙伴 ID，请切换目录。 在“切换目录”下，选择你的目录。

   ![显示“切换”目录的屏幕截图](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-to-a-new-partner-id"></a>使用 PowerShell 链接到新合作伙伴 ID

1. 安装 [AzureRM.ManagementPartner](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner) PowerShell 模块。

2. 使用用户帐户或服务主体登录到客户的租户。 有关详细信息，请参阅[使用 PowerShell 登录](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-5.2.0)。
 
   ```azurepowershell-interactive
    C:\> Connect-AzureRmAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
   ```


3. 链接到新合作伙伴 ID。 合作伙伴 ID 是组织的 [Microsoft 合作伙伴网络](https://partner.microsoft.com/) ID。

    ```azurepowershell-interactive
    C:\> new-AzureRmManagementPartner -PartnerId 12345 
    ```

#### <a name="get-the-linked-partner-id"></a>获取链接的合作伙伴 ID
```azurepowershell-interactive
C:\> get-AzureRmManagementPartner 
```

#### <a name="update-the-linked-partner-id"></a>更新链接的合作伙伴 ID
```azurepowershell-interactive
C:\> Update-AzureRmManagementPartner -PartnerId 12345 
```
#### <a name="delete-the-linked-partner-id"></a>删除链接的合作伙伴 ID
```azurepowershell-interactive
C:\> remove-AzureRmManagementPartner -PartnerId 12345 
```

### <a name="use-the-azure-cli-to-link-to-a-new-partner-id"></a>使用 Azure CLI 链接到新合作伙伴 ID
1. 安装 Azure CLI 扩展。

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ``` 

2. 使用用户帐户或服务主体登录到客户的租户。 有关详细信息，请参阅[使用 Azure CLI 登录](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)。

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ``` 

3. 链接到新合作伙伴 ID。 合作伙伴 ID 是组织的 [Microsoft 合作伙伴网络](https://partner.microsoft.com/) ID。

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>获取链接的合作伙伴 ID
```azurecli-interactive
C:\ az managementpartner show
``` 

#### <a name="update-the-linked-partner-id"></a>更新链接的合作伙伴 ID
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
``` 

#### <a name="delete-the-linked-partner-id"></a>删除链接的合作伙伴 ID
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
``` 

## <a name="next-steps"></a>后续步骤

加入 [Microsoft 合作伙伴社区](https://aka.ms/PALdiscussion)中的讨论，以接收更新或发送反馈。

## <a name="frequently-asked-questions"></a>常见问题

**谁可以链接合作伙伴 ID？**

合作伙伴组织中管理客户的 Azure 资源的任何用户都可将合作伙伴 ID 链接到帐户。

**链接合作伙伴 ID 后，是否可以更改？**

是的。 可以更改、添加或删除已链接的合作伙伴 ID。

**如果某个用户在多个客户租户中具有帐户怎么办？**

合作伙伴 ID 与帐户之间的链接是针对每个客户租户执行的。 链接每个客户租户中的合作伙伴 ID。

**其他合作伙伴或客户是否可以编辑或删除合作伙伴 ID 的链接？**

该链接在用户帐户级别关联。 只有你可以编辑或删除合作伙伴 ID 的链接。 客户和其他合作伙伴无法更改合作伙伴 ID 的链接。 
