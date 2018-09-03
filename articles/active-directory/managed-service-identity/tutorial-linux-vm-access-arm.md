---
title: 使用 Linux VM 托管服务标识访问 Azure 资源管理器
description: 本教程逐步介绍了如何使用 Linux VM 托管服务标识访问 Azure 资源管理器。
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: bryanla
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: 6ef3c901005b34d7ae849a2358f1f8af42bb339b
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887039"
---
# <a name="use-a-linux-vm-managed-service-identity-to-access-azure-resource-manager"></a>使用 Linux VM 托管服务标识访问 Azure 资源管理器

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

本快速入门介绍如何使用 Linux 虚拟机 (VM) 的系统分配标识来访问 Azure 资源管理器 API。 托管服务标识由 Azure 自动管理，可用于通过支持 Azure AD 身份验证的服务的身份验证，这样就无需在代码中插入凭据了。 学习如何：

> [!div class="checklist"]
> * 在 Linux 虚拟机上启用托管服务标识 
> * 授予 VM 对 Azure 资源管理器中资源组的访问权限 
> * 使用 VM 标识获取访问令牌，并使用它调用 Azure 资源管理器 

## <a name="prerequisites"></a>先决条件

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [登录到 Azure 门户](https://portal.azure.com)

- [创建 Linux 虚拟机](/azure/virtual-machines/linux/quick-create-portal)

- [在虚拟机上启用系统分配的标识](/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm#enable-system-assigned-identity-on-an-existing-vm)

## <a name="grant-your-vm-access-to-a-resource-group-in-azure-resource-manager"></a>授予 VM 对 Azure 资源管理器中资源组的访问权限 

使用托管服务标识，代码可以获取访问令牌，对支持 Azure AD 身份验证的资源进行身份验证。 Azure 资源管理器 API 支持 Azure AD 身份验证。 首先，需要授予此 VM 标识对 Azure 资源管理器中资源（在此示例中，为包含 VM 的资源组）的访问权限。  

1. 转到“资源组”选项卡。
2. 选择之前创建的特定资源组。
3. 转到左侧面板中的“访问控制(IAM)”。
4. 单击“添加”，为 VM 添加新的角色分配。 选择“阅读器”作为“角色”。
5. 在下一个下拉列表中，把“将访问权限分配给”设置为资源“虚拟机”。
6. 接下来，请确保“订阅”下拉列表中列出的订阅正确无误。 对于“资源组”，请选择“所有资源组”。
7. 最后，在“选择”下拉列表中，选择 Linux 虚拟机，再单击“保存”。

    ![Alt 图像文本](media/msi-tutorial-linux-vm-access-arm/msi-permission-linux.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>使用 VM 标识获取访问令牌，并使用它调用资源管理器 

若要完成这些步骤，需要使用 SSH 客户端。 如果使用的是 Windows，可以在[适用于 Linux 的 Windows 子系统](https://msdn.microsoft.com/commandline/wsl/about)中使用 SSH 客户端。 如果需要有关配置 SSH 客户端密钥的帮助，请参阅[如何在 Azure 上将 SSH 密钥与 Windows 配合使用](../../virtual-machines/linux/ssh-from-windows.md)或[如何创建和使用适用于 Azure 中 Linux VM 的 SSH 公钥和私钥对](../../virtual-machines/linux/mac-create-ssh-keys.md)。

1. 在门户中，转到 Linux VM，并单击“概述”中的“连接”。  
2. 使用所选的 SSH 客户端连接到 VM。 
3. 在终端窗口中，使用 CURL 向本地托管服务标识终结点发出请求，以获取 Azure 资源管理器的访问令牌。  
 
    下面是用于获取访问令牌的 CURL 请求。  
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true   
    ```
    
    > [!NOTE]
    > “resource”参数值必须与 Azure AD 预期值完全一致。  若为资源管理器资源 ID，必须在 URI 的结尾添加斜线。 
    
    响应包括访问 Azure 资源管理器所需的访问令牌。 
    
    响应：  

    ```bash
    {"access_token":"eyJ0eXAiOi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"} 
    ```
    
    可以使用此访问令牌访问 Azure 资源管理器。例如，读取之前授予此 VM 有权访问的资源组的详细信息。 将 \<SUBSCRIPTION ID\>、\<RESOURCE GROUP\> 和 \<ACCESS TOKEN\> 的值替换为前面创建的值。 
    
    > [!NOTE]
    > URL 区分大小写。因此，请确保大小写与之前在命名资源组时使用的大小写完全相同，并确保“resourceGroup”使用的是大写“G”。  
    
    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```
    
    返回的响应包含具体的资源组信息： 
     
    ```bash
    {"id":"/subscriptions/98f51385-2edc-4b79-bed9-7718de4cb861/resourceGroups/DevTest","name":"DevTest","location":"westus","properties":{"provisioningState":"Succeeded"}} 
    ```     

## <a name="next-steps"></a>后续步骤

在本教程中，你学习了如何创建用户分配标识并将其附加到 Azure 虚拟机，以访问 Azure 资源管理器 API。  若要详细了解 Azure 资源管理器，请参阅：

> [!div class="nextstepaction"]
>[Azure 资源管理器](/azure/azure-resource-manager/resource-group-overview)