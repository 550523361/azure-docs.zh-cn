---
title: 创建具有逻辑应用 REST API 的集成服务环境（ISEs）
description: 使用逻辑应用 REST API 创建集成服务环境（ISE），以便你可以从 Azure 逻辑应用访问 Azure 虚拟网络（Vnet）
services: logic-apps
ms.suite: integration
ms.reviewer: rarayudu, logicappspm
ms.topic: conceptual
ms.date: 05/29/2020
ms.openlocfilehash: d33207639ebef912307a3c594ec274fd9609bd67
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "84656541"
---
# <a name="create-an-integration-service-environment-ise-by-using-the-logic-apps-rest-api"></a>使用逻辑应用 REST API 创建集成服务环境 (ISE)

本文介绍如何在逻辑应用和集成帐户需要访问[Azure 虚拟网络](../virtual-network/virtual-networks-overview.md)的情况下，通过逻辑应用创建[*集成服务环境*（ISE）](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) REST API。 ISE 是一个专用环境，它使用专用存储和其他与“全局”多租户逻辑应用服务分离的资源。 这种分离还减少了其他 Azure 租户可能对应用性能产生的影响。 ISE 还为你提供你自己的静态 IP 地址。 这些 IP 地址与公共、多租户服务中的逻辑应用共享的静态 IP 地址分隔开。

还可以通过使用[示例 Azure 资源管理器快速入门模板](https://github.com/Azure/azure-quickstart-templates/tree/master/201-integration-service-environment)或使用[AZURE 门户](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)来创建 ISE。

> [!IMPORTANT]
> 在 ISE 中运行的逻辑应用、内置触发器、内置操作和连接器使用与基于消费的定价计划不同的定价计划。 要了解 ISE 的定价和计费原理，请参阅[逻辑应用定价模型](../logic-apps/logic-apps-pricing.md#fixed-pricing)。 有关定价费率，请参阅[逻辑应用定价](../logic-apps/logic-apps-pricing.md)。

## <a name="prerequisites"></a>先决条件

* 为[ise 启用访问权限](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access)的[先决条件](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#prerequisites)和要求与在 Azure 门户中创建 ise 时相同。

* 一种工具，可用于通过调用具有 HTTPS PUT 请求 REST API 的逻辑应用来创建 ISE。 例如，可以使用[Postman](https://www.getpostman.com/downloads/)，也可以生成执行此任务的逻辑应用。

## <a name="send-the-request"></a>发送请求

若要通过调用逻辑应用 REST API 创建 ISE，请发出此 HTTPS PUT 请求：

`PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/integrationServiceEnvironments/{integrationServiceEnvironmentName}?api-version=2019-05-01`

> [!IMPORTANT]
> 逻辑应用 REST API 2019-05-01 版本要求你为 ISE 连接器自行发出 HTTP PUT 请求。

部署通常需要两个小时才能完成。 有时，部署过程可能长达四个小时。 若要检查部署状态，请在 " [Azure 门户](https://portal.azure.com)的 Azure 工具栏上，选择" 通知 "图标，打开" 通知 "窗格。

> [!NOTE]
> 如果部署失败或删除 ISE，Azure 在释放子网之前可能需要长达一个小时。 此延迟意味着可能需要等待一段时间才能在另一个 ISE 中重复使用这些子网。
>
> 如果删除虚拟网络，Azure 通常需要长达两小时才能释放子网，但是此操作可能需要更长的时间。 
> 删除虚拟网络时，请确保没有资源仍处于连接状态。 
> 请参阅[删除虚拟网络](../virtual-network/manage-virtual-network.md#delete-a-virtual-network)。

## <a name="request-header"></a>请求标头

在请求标头中，包括以下属性：

* `Content-type`：将此属性值设置为 `application/json` 。

* `Authorization`：将此属性值设置为有权访问要使用的 Azure 订阅或资源组的客户的持有者令牌。

<a name="request-body"></a>

## <a name="request-body"></a>请求正文

下面是请求正文语法，描述创建 ISE 时要使用的属性。 若要创建允许使用在该位置安装的自签名证书的 ISE `TrustedRoot` ，请在 `certificates` ise 定义的部分中包括该对象 `properties` 。 对于现有 ISE，只能为对象发送修补请求 `certificates` 。 有关使用自签名证书的详细信息，请参阅[HTTP 连接器-自签名证书](../connectors/connectors-native-http.md#self-signed)。

```json
{
   "id": "/subscriptions/{Azure-subscription-ID/resourceGroups/{Azure-resource-group}/providers/Microsoft.Logic/integrationServiceEnvironments/{ISE-name}",
   "name": "{ISE-name}",
   "type": "Microsoft.Logic/integrationServiceEnvironments",
   "location": "{Azure-region}",
   "sku": {
      "name": "Premium",
      "capacity": 1
   },
   "properties": {
      "networkConfiguration": {
         "accessEndpoint": {
            // Your ISE can use the "External" or "Internal" endpoint. This example uses "External".
            "type": "External"
         },
         "subnets": [
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-1}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-2}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-3}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-4}",
            }
         ]
      },
      // Include `certificates` object to enable self-signed certificate support
      "certificates": {
         "testCertificate": {
            "publicCertificate": "{base64-encoded-certificate}",
            "kind": "TrustedRoot"
         }
      }
   }
}
```

### <a name="request-body-example"></a>请求正文示例

此示例请求正文显示示例值：

```json
{
   "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Logic/integrationServiceEnvironments/Fabrikam-ISE",
   "name": "Fabrikam-ISE",
   "type": "Microsoft.Logic/integrationServiceEnvironments",
   "location": "WestUS2",
   "sku": {
      "name": "Premium",
      "capacity": 1
   },
   "properties": {
      "networkConfiguration": {
         "accessEndpoint": {
            // Your ISE can use the "External" or "Internal" endpoint. This example uses "External".
            "type": "External"
         },
         "subnets": [
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-1",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-2",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-3",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-4",
            }
         ]
      },
      "certificates": {
         "testCertificate": {
            "publicCertificate": "LS0tLS1CRUdJTiBDRV...",
            "kind": "TrustedRoot"
         }
   }
}
```

## <a name="next-steps"></a>后续步骤

* [向集成服务环境添加资源](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [管理集成服务环境](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)

