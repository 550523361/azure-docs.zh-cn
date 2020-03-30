---
title: 为 Azure 事件网格主题或域配置专用终结点
description: 本文介绍如何为 Azure 事件网格主题或域配置专用终结点。
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: how-to
ms.date: 03/11/2020
ms.author: spelluru
ms.openlocfilehash: d08afe00c13a3f96b9526c3cb29804cfad688ddc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299902"
---
# <a name="configure-private-endpoints-for-azure-event-grid-topics-or-domains-preview"></a>为 Azure 事件网格主题或域配置专用终结点（预览）
您可以使用[专用终结点](../private-link/private-endpoint-overview.md)允许事件直接从虚拟网络通过[专用链路](../private-link/private-link-overview.md)安全地进入您的主题和域，而无需通过公共互联网。 专用终结点使用主题或域的 VNet 地址空间中的 IP 地址。 有关详细信息概念信息，请参阅[网络安全](network-security.md)。

本文介绍如何为主题或域配置专用终结点。

> [!IMPORTANT]
> 专用终结点功能仅适用于高级层中的主题和域。 要从基本层升级到高级层，请参阅[更新定价层](update-tier.md)一文。 

## <a name="use-azure-portal"></a>使用 Azure 门户 
本节介绍如何使用 Azure 门户为主题或域创建专用终结点。

> [!NOTE]
> 本节中显示的步骤主要针对主题。 您可以使用类似的步骤为**域**创建专用终结点。 

1. 登录到 Azure[门户](https://portal.azure.com)并导航到主题或域。
2. 切换到主题页面的 **"网络**"选项卡。 选择 **" 工具栏上的专用终结点**"。

    ![添加专用终结点](./media/configure-private-endpoints/add-button.png)
2. 其中一个**基础知识**页面，按照以下步骤操作： 
    1. 选择要在其中创建专用终结点的**Azure 订阅**。 
    2. 为专用终结点选择**Azure 资源组**。 
    3. 输入终结点**的名称**。 
    4. **选择终结点**的区域。 专用终结点必须与虚拟网络位于同一区域，但可以位于与专用链接资源不同的区域（在此示例中为事件网格主题）。 
    5. 然后，选择**页面底部的"下一步：资源>"** 按钮。 

      ![专用终结点 - 基础知识页面](./media/configure-private-endpoints/basics-page.png)
3. 在 **"资源"** 页上，按照以下步骤操作： 
    1. 对于连接方法，如果选择 **"连接到目录中的 Azure 资源**"，请按照以下步骤操作。 此示例演示如何连接到目录中的 Azure 资源。 
        1. 选择**主题/域**存在的**Azure 订阅**。 
        1. 对于**资源类型**，选择**Microsoft.eventGrid/主题**或"**资源类型****"的"事件网格/域**"。
        2. 对于**资源**，从下拉列表中选择主题/域。 
        3. 确认**目标子资源**设置为**主题**或**域**（基于您选择的资源类型）。    
        4. 选择 **"下一步"：页面底部的"配置>"** 按钮。 

            ![专用终结点 - 资源页](./media/configure-private-endpoints/resource-page.png)
    2. 如果选择 **"使用资源 ID 或别名连接到资源**"，请按照以下步骤操作：
        1. 输入资源的 ID。 例如：`/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>`。  
        2. 对于**资源**，输入**主题**或**域**。 
        3. （可选）添加请求消息。 
        4. 选择 **"下一步"：页面底部的"配置>"** 按钮。 

            ![专用终结点 - 资源页](./media/configure-private-endpoints/connect-azure-resource-id.png)
4. 在 **"配置"** 页上，选择虚拟网络中的子网，以选择要部署专用终结点的位置。 
    1. 选择**虚拟网络**。 下拉列表中仅列出当前选择的订阅和位置中的虚拟网络。 
    2. 在所选虚拟网络中选择**子网**。 
    3. 选择 **"下一步"：在页面底部标记>** 按钮。 

    ![专用终结点 - 配置页](./media/configure-private-endpoints/configuration-page.png)
5. 在 **"标记"** 页上，创建要与专用终结点资源关联的任何标记（名称和值）。 然后，选择页面底部的 **"查看 + 创建**"按钮。 
6. 在 **"审阅 + 创建**"上，查看所有设置，然后选择 **"创建"** 以创建专用终结点。 

    ![专用终结点 - 查看&创建页面](./media/configure-private-endpoints/review-create-page.png)
    

## <a name="manage-private-link-connection"></a>管理专用链接连接

创建专用终结点时，必须批准连接。 如果要为其创建专用终结点的资源位于目录中，则可以批准连接请求，前提是您具有足够的权限。 如果要连接到另一个目录中的 Azure 资源，则必须等待该资源的所有者批准连接请求。

有四种预配状态：

| 服务操作 | 服务使用者专用终结点状态 | 描述 |
|--|--|--|
| 无 | 挂起的 | 连接是手动创建的，正在等待专用链接资源所有者的批准。 |
| 审批 | 已批准 | 连接已自动或手动批准，可供使用。 |
| 拒绝 | 已拒绝 | 连接被专用链接资源所有者拒绝。 |
| 删除 | 已断开连接 | 连接已被专用链接资源所有者删除，专用终结点仅供参考，应将其删除以清理资源。 |
 
###  <a name="how-to-manage-a-private-endpoint-connection"></a>如何管理专用终结点连接
以下各节介绍如何批准或拒绝专用终结点连接。 

1. 登录到 Azure[门户](https://portal.azure.com)。
1. 在搜索栏中，键入**事件网格主题**或**事件网格域**。
1. 选择要管理**的主题**或**域**。
1. 选择 **"网络"** 选项卡。
1. 如果存在任何挂起的连接，您将看到与 **"挂起"** 处于预配状态的连接。 

### <a name="to-approve-a-private-endpoint"></a>批准专用终结点
您可以批准处于挂起状态的私有终结点。 要批准，请按照以下步骤操作： 

> [!NOTE]
> 本节中显示的步骤主要针对主题。 您可以使用类似的步骤来批准**域**的专用终结点。 

1. 选择要批准的**专用终结点**，并在工具栏上选择 **"批准**"。

    ![专用终结点 - 挂起状态](./media/configure-private-endpoints/pending.png)
1. 在 **"批准连接**"对话框中，输入注释（可选），然后选择 **"是**"。 

    ![专用终结点 - 批准](./media/configure-private-endpoints/approve.png)
1. 确认将终结点的状态视为 **"已批准**"。 

    ![专用终结点 - 已批准状态](./media/configure-private-endpoints/approved-status.png)

### <a name="to-reject-a-private-endpoint"></a>拒绝专用终结点
您可以拒绝处于挂起状态或已批准状态的私有终结点。 要拒绝，请按照以下步骤操作： 

> [!NOTE]
> 本节中显示的步骤适用于主题。 您可以使用类似的步骤来拒绝**域**的专用终结点。 

1. 选择要拒绝**的专用终结点**，并在工具栏上选择 **"拒绝**"。

    ![专用终结点 - 拒绝](./media/configure-private-endpoints/reject-button.png)
1. 在 **"拒绝连接**"对话框中，输入注释（可选），然后选择 **"是**"。 

    ![专用终结点 - 拒绝](./media/configure-private-endpoints/reject.png)
1. 确认将终结点的状态视为 **"已拒绝**"。 

    ![专用终结点 - 拒绝状态](./media/configure-private-endpoints/rejected-status.png)

    > [!NOTE]
    > 拒绝 Azure 门户中的专用终结点后，无法批准该终结点。 


## <a name="use-azure-cli"></a>使用 Azure CLI
要创建专用终结点，请使用[az 网络专用终结点创建](/cli/azure/network/private-endpoint?view=azure-cli-latest#az-network-private-endpoint-create)方法，如以下示例所示：

```azurecli-interactive
az network private-endpoint create \
    --resource-group <RESOURECE GROUP NAME> \
    --name <PRIVATE ENDPOINT NAME> \
    --vnet-name <VIRTUAL NETWORK NAME> \
    --subnet <SUBNET NAME> \
    --private-connection-resource-id "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<TOPIC NAME> \
    --connection-name <PRIVATE LINK SERVICE CONNECTION NAME> \
    --location <LOCATION> \
    --group-ids topic
```

有关示例中使用的参数的说明，请参阅[az 网络专用终结点创建](/cli/azure/network/private-endpoint?view=azure-cli-latest#az-network-private-endpoint-create)的文档。 此示例中需要注意的几点包括： 

- 对于`private-connection-resource-id`，请指定**主题**或**域**的资源 ID。 前面的示例使用类型： 主题。
- 指定`group-ids``topic`或`domain`。 在前面的示例中，`topic`使用了。 

要删除专用终结点，请使用[az 网络专用终结点删除](/cli/azure/network/private-endpoint?view=azure-cli-latest#az-network-private-endpoint-delete)方法，如以下示例所示：

```azurecli-interactive
az network private-endpoint delete --resource-group <RESOURECE GROUP NAME> --name <PRIVATE ENDPOINT NAME>
```

> [!NOTE]
> 本节中显示的步骤适用于主题。 您可以使用类似的步骤为**域**创建专用终结点。 

### <a name="create-a-private-endpoint"></a>创建专用终结点
下面是创建以下 Azure 资源的示例脚本：

- 资源组
- 虚拟网络
- 虚拟网络中的子网
- Azure 事件网格主题（高级层）
- 主题的专用终结点

> [!NOTE]
> 本节中显示的步骤适用于主题。 可以使用类似的步骤为域创建专用终结点。

```azurecli-interactive
subscriptionID="<AZURE SUBSCRIPTION ID>"
resourceGroupName="<RESOURCE GROUP NAME>"
location="<LOCATION>"
vNetName="<VIRTUAL NETWORK NAME>"
subNetName="<SUBNET NAME>"
topicName = "<TOPIC NAME>"
connectionName="<ENDPOINT CONNECTION NAME>"
endpointName=<ENDPOINT NAME>

# URI for the topic. replace <SUBSCRIPTION ID>, <RESOURCE GROUP NAME>, and <TOPIC NAME>
topicUri="/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<TOPIC NAME>?api-version=2020-04-01-preview"

# resource ID of the topic. replace <SUBSCRIPTION ID>, <RESOURCE GROUP NAME>, and <TOPIC NAME>
topicResourceID="/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<TOPIC NAME>"

# select subscription
az account set --subscription $subscriptionID

# create resource group
az group create --name $resourceGroupName --location $location

# create vnet 
az network vnet create \
    --resource-group $resourceGroupName \
    --name $vNetName \
    --address-prefix 10.0.0.0/16

# create subnet
az network vnet subnet create \
    --resource-group $resourceGroupName \
    --vnet-name $vNetName \
    --name $subNetName \
    --address-prefixes 10.0.0.0/24

# disable private endpoint network policies for the subnet
az network vnet subnet update \
    --resource-group $resourceGroupName \
    --vnet-name $vNetName \
    --name $subNetName \
    --disable-private-endpoint-network-policies true

# create event grid topic. update <LOCATION>
az rest --method put \
    --uri $topicUri \
    --body "{\""location\"":\""LOCATION\"", \""sku\"": {\""name\"": \""premium\""}, \""properties\"": {\""publicNetworkAccess\"":\""Disabled\""}}"

# verify that the topic was created.
az rest --method get \
    --uri $topicUri

# create private endpoint for the topic you created
az network private-endpoint create 
    --resource-group $resourceGroupName \
    --name $endpointName \
    --vnet-name $vNetName \
    --subnet $subNetName \
    --private-connection-resource-id $topicResourceID \
    --connection-name $connectionName \
    --location $location \
    --group-ids topic

# get topic 
az rest --method get \
    --uri $topicUri

```

### <a name="approve-a-private-endpoint-connection"></a>批准专用终结点连接
以下示例 CLI 代码段演示如何批准专用终结点连接。 

```azurecli-interactive
az rest --method put --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview" --body "{\""properties\"":{\""privateLinkServiceConnectionState\"": {\""status\"":\""approved\"",\""description\"":\""connection approved\"", \""actionsRequired\"": \""none\""}}}"
```


### <a name="reject-a-private-endpoint-connection"></a>拒绝专用终结点连接
以下示例 CLI 代码段演示如何拒绝专用终结点连接。 

```azurecli-interactive
az rest --method put --uri "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview" --body "{\""properties\"":{\""privateLinkServiceConnectionState\"": {\""status\"":\""rejected\"",\""description\"":\""connection rejected\"", \""actionsRequired\"": \""none\""}}}"
```


## <a name="use-powershell"></a>使用 PowerShell
本节介绍如何使用 PowerShell 为主题或域创建专用终结点。 

### <a name="prerequisite"></a>先决条件
按照["如何操作"的说明：使用门户创建 Azure AD 应用程序和服务主体，该应用程序和服务主体可以访问资源](../active-directory/develop/howto-create-service-principal-portal.md)以创建 Azure 活动目录应用程序，并记下**目录（租户）ID、****应用程序（客户端）ID**和**应用程序（客户端）机密**的值。 

### <a name="prepare-token-and-headers-for-rest-api-calls"></a>为 REST API 调用准备令牌和标头 
运行以下先决条件命令，获取用于 REST API 调用和授权和其他标头信息的身份验证令牌。 

```azurepowershell-interactive
$body = "grant_type=client_credentials&client_id=<CLIENT ID>&client_secret=<CLIENT SECRET>&resource=https://management.core.windows.net"

# get authentication token
$Token = Invoke-RestMethod -Method Post `
    -Uri https://login.microsoftonline.com/<TENANT ID>/oauth2/token  `
    -Body $body  `
    -ContentType 'application/x-www-form-urlencoded' 

# set authorization and content-type headers
$Headers = @{}
$Headers.Add("Authorization","$($Token.token_type) "+ " " + "$($Token.access_token)")
```

### <a name="create-a-subnet-with-endpoint-network-policies-disabled"></a>创建禁用终结点网络策略的子网

```azurepowershell-interactive

# create resource group
New-AzResourceGroup -ResourceGroupName <RESOURCE GROUP NAME>  -Location <LOCATION>

# create virtual network
$virtualNetwork = New-AzVirtualNetwork `
                    -ResourceGroupName <RESOURCE GROUP NAME> `
                    -Location <LOCATION> `
                    -Name <VIRTUAL NETWORK NAME> `
                    -AddressPrefix 10.0.0.0/16

# create subnet with endpoint network policy disabled
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
                    -Name example-privatelinksubnet `
                    -AddressPrefix 10.0.0.0/24 `
                    -PrivateEndpointNetworkPoliciesFlag "Disabled" `
                    -VirtualNetwork $virtualNetwork

# update virtual network
$virtualNetwork | Set-AzVirtualNetwork
```

### <a name="create-an-event-grid-topic-with-a-private-endpoint"></a>使用专用终结点创建事件网格主题

> [!NOTE]
> 本节中显示的步骤适用于主题。 您可以使用类似的步骤为**域**创建专用终结点。 


```azurepowershell-interactive
$body = @{"location"="<LOCATION>"; "sku"= @{"name"="premium"}; "properties"=@{"publicNetworkAccess"="disabled"}} | ConvertTo-Json

# create topic
Invoke-RestMethod -Method 'Put'  `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview"  `
    -Headers $Headers  `
    -Body $body

# verify that the topic was created
$topic=Invoke-RestMethod -Method 'Get'  `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>?api-version=2020-04-01-preview"   `
    -Headers $Headers  

# create private link service connection
$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
                                -Name "<PRIVATE LINK SERVICE CONNECTION NAME>" `
                                -PrivateLinkServiceId $topic.Id `
                                -GroupId "topic"

# get virtual network info 
$virtualNetwork = Get-AzVirtualNetwork  `
                    -ResourceGroupName  <RESOURCE GROUP NAME> `
                    -Name <VIRTUAL NETWORK NAME>

# get subnet info
$subnet = $virtualNetwork | Select -ExpandProperty subnets `
                             | Where-Object  {$_.Name -eq <SUBNET NAME>}  

# now, you are ready to create a private endpoint 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName <RESOURCE GROUP NAME>  `
                                        -Name <PRIVATE ENDPOINT NAME>   `
                                        -Location <LOCATION> `
                                        -Subnet  $subnet   `
                                        -PrivateLinkServiceConnection $privateEndpointConnection

# verify that the endpoint was created
Invoke-RestMethod -Method 'Get'  `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections?api-version=2020-04-01-preview"   `
    -Headers $Headers   `
    | ConvertTo-Json -Depth 5

```

验证终结点是否已创建时，应看到类似于以下内容的结果：

```json

{
  "value": [
    {
      "properties": {
        "privateEndpoint": {
          "id": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Network/privateEndpoints/<PRIVATE ENDPOINT NAME>"
        },
        "groupIds": [
          "topic"
        ],
        "privateLinkServiceConnectionState": {
          "status": "Approved",
          "description": "Auto-approved",
          "actionsRequired": "None"
        },
        "provisioningState": "Succeeded"
      },
      "id": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>",
      "name": "myConnection",
      "type": "Microsoft.EventGrid/topics/privateEndpointConnections"
    }
  ]
}
```

### <a name="approve-a-private-endpoint-connection"></a>批准专用终结点连接
下面的示例 PowerShell 代码段演示如何批准专用终结点。 

> [!NOTE]
> 本节中显示的步骤适用于主题。 您可以使用类似的步骤来批准**域**的专用终结点。 

```azurepowershell-interactive
$approvedBody = @{"properties"=@{"privateLinkServiceConnectionState"=@{"status"="approved";"description"="connection approved";"actionsRequired"="none"}}} | ConvertTo-Json

# approve endpoint connection
Invoke-RestMethod -Method 'Put'  `
    -Uri "https://management.azure.com/subscriptions/<AzuRE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview"  `
    -Headers $Headers  `
    -Body $approvedBody

# confirm that the endpoint connection was approved
Invoke-RestMethod -Method 'Get'  `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview"  `
    -Headers $Headers

```

### <a name="reject-a-private-endpoint-connection"></a>拒绝专用终结点连接
下面的示例演示如何使用 PowerShell 拒绝私有终结点。 可以从上一个 GET 命令的结果获取私有终结点的 GUID。 

> [!NOTE]
> 本节中显示的步骤适用于主题。 您可以使用类似的步骤来拒绝**域**的专用终结点。 


```azurepowershell-interactive
$rejectedBody = @{"properties"=@{"privateLinkServiceConnectionState"=@{"status"="rejected";"description"="connection rejected";"actionsRequired"="none"}}} | ConvertTo-Json

# reject private endpoint
Invoke-RestMethod -Method 'Put'  `
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview"  `
    -Headers $Headers  `
    -Body $rejectedBody

# confirm that endpoint was rejected
Invoke-RestMethod -Method 'Get' 
    -Uri "https://management.azure.com/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventGrid/topics/<EVENT GRID TOPIC NAME>/privateEndpointConnections/<PRIVATE ENDPOINT NAME>.<GUID>?api-version=2020-04-01-preview" ` 
    -Headers $Headers
```

即使在通过 API 拒绝连接后，您也可以批准该连接。 如果使用 Azure 门户，则无法批准已被拒绝的终结点。 

## <a name="next-steps"></a>后续步骤
要了解如何配置 IP 防火墙设置，请参阅[为 Azure 事件网格主题或域配置 IP 防火墙](configure-firewall.md)。