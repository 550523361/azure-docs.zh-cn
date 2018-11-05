---
title: 使用 REST API 和模板部署资源 | Microsoft Docs
description: 使用 Azure 资源管理器和资源管理器 REST API 将资源部署到 Azure。 资源在 Resource Manager 模板中定义。
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2018
ms.author: tomfitz
ms.openlocfilehash: 058d6d398f6bb54e8569e727f118a325c338049d
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "50154736"
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>使用 Resource Manager 模板和 Resource Manager REST API 部署资源

本文介绍如何将 Resource Manager REST API 与 Resource Manager 模板配合使用向 Azure 部署资源。  

> [!TIP]
> 有关在部署过程中调试错误的帮助，请参阅：
> 
> * [查看部署操作](resource-manager-deployment-operations.md)，了解如何获取有助于排查错误的信息
> * [排查使用 Azure 资源管理器将资源部署到 Azure 时的常见错误](resource-manager-common-deployment-errors.md)，了解如何解决常见的部署错误
> 
> 

可以在请求正文中包含模板或链接到文件。 使用文件时，它可以是本地文件，也可以是通过 URI 提供的外部文件。 如果模板位于存储帐户中，可以限制对该模板的访问，并在部署过程中提供共享访问签名 (SAS) 令牌。

## <a name="deploy-with-the-rest-api"></a>使用 REST API 进行部署
1. 设置[常见参数和标头](/rest/api/azure/)，包括身份验证令牌。

1. 如果目前没有资源组，请创建资源组。 提供订阅 ID、新资源组的名称，以及解决方案所需的位置。 有关详细信息，请参阅[创建资源组](/rest/api/resources/resourcegroups/createorupdate)。

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2018-05-01
  ```

  使用如下所示请求正文：
  ```json
  {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
  }
  ```

1. 在执行部署之前，通过运行[验证模板部署](/rest/api/resources/deployments/validate)操作来验证部署。 测试部署时，请提供与执行部署时所提供的完全相同的参数（如下一步中所示）。

1. 创建部署。 提供订阅 ID、资源组的名称、部署的名称以及模板的链接。 有关模板文件的信息，请参阅[参数文件](#parameter-file)。 有关使用 REST API 创建资源组的详细信息，请参阅[创建模板部署](/rest/api/resources/deployments/createorupdate)。 请注意，**mode** 设置为 **Incremental**。 要运行完整部署，请将 **mode** 设置为 **Complete**。 使用完整模式时要小心，因为可能会无意中删除不在模板中的资源。

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
  ```

  使用如下所示请求正文：

   ```json
  {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      }
    }
  }
  ```

    如果想要记录响应内容或/和请求内容，请在请求中包括 **debugSetting**。

  ```json
  "debugSetting": {
    "detailLevel": "requestContent, responseContent"
  }
  ```

    可以将存储帐户设置为使用共享访问签名 (SAS) 令牌。 有关详细信息，请参阅[使用共享访问签名委托访问权限](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature)。

1. 可以将模板和参数包含在请求正文中，而不是链接到模板和参数的文件。

  ```json
  {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2018-02-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
  }
  ```

5. 获取模板部署的状态。 有关详细信息，请参阅[获取有关模板部署的信息](/rest/api/resources/deployments/get)。

  ```HTTP
  GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
  ```

## <a name="redeploy-when-deployment-fails"></a>部署失败时，重新部署

对于失败的部署，可以指定从部署历史记录自动重新部署以前的部署。 若要使用此选项，部署必须具有唯一的名称，以便可以在历史记录中标识它们。 如果没有唯一名称，则当前失败的部署可能会覆盖历史记录中以前成功的部署。 只能将此选项用于根级别部署。 从嵌套模板进行的部署不可用于重新部署。

若要在当前部署失败的情况下，重新部署上一个成功部署，请使用：

```json
"onErrorDeployment": {
  "type": "LastSuccessful",
},
```

若要在当前部署失败的情况下，重新部署特定部署，请使用：

```json
"onErrorDeployment": {
  "type": "SpecificDeployment",
  "deploymentName": "<deploymentname>"
}
```

指定的部署必须已成功。

## <a name="parameter-file"></a>参数文件

如果要在部署期间使用参数文件传递参数值，需要使用类似于以下示例的格式创建一个 JSON 文件：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

参数文件的大小不能超过 64 KB。

如果需要为参数（如密码）提供敏感值，请将该值添加到密钥保管库。 在部署过程中检索密钥保管库，如前面的示例所示。 有关详细信息，请参阅[在部署期间传递安全值](resource-manager-keyvault-parameter.md)。 

## <a name="next-steps"></a>后续步骤
* 若要指定如何处理存在于资源组中但未在模板中定义的资源，请参阅 [Azure 资源管理器部署模式](deployment-modes.md)。
* 若要了解如何处理异步 REST 操作，请参阅[跟踪异步 Azure 操作](resource-manager-async-operations.md)。
* 有关通过 .NET 客户端库部署资源的示例，请参阅[使用 .NET 库和模板部署资源](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。
* 若要在模板中定义参数，请参阅[创作模板](resource-group-authoring-templates.md#parameters)。
* 有关企业可如何使用 Resource Manager 有效管理订阅的指南，请参阅 [Azure 企业基架 - 出于合规目的监管订阅](/azure/architecture/cloud-adoption-guide/subscription-governance)。

