---
title: 使用 REST API 和模板部署资源
description: 使用 Azure Resource Manager 和 Resource Manager REST API 将资源部署到 Azure。 资源在 Resource Manager 模板中定义。
ms.topic: conceptual
ms.date: 06/04/2019
ms.openlocfilehash: 9cdb7b668e5170917b41ef49639bd9a17e538766
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153227"
---
# <a name="deploy-resources-with-arm-templates-and-resource-manager-rest-api"></a>使用 ARM 模板和资源管理器 REST API 部署资源

本文介绍如何使用资源管理器 REST API 与 Azure 资源管理器 （ARM） 模板一起将资源部署到 Azure。

可以在请求正文中包含模板或链接到文件。 使用文件时，它可以是本地文件，也可以是通过 URI 提供的外部文件。 如果模板位于存储帐户中，可以限制对该模板的访问，并在部署过程中提供共享访问签名 (SAS) 令牌。

## <a name="deployment-scope"></a>部署范围

可将部署目标设定为管理组、Azure 订阅或资源组。 大多数情况下，我们会将部署目标设定为资源组。 使用管理组或订阅部署在指定范围内应用策略和角色分配。 还可以使用订阅部署创建资源组并向其部署资源。 你将根据部署范围使用不同的命令。

若要部署到**资源组**，请使用“[部署 - 创建](/rest/api/resources/deployments/createorupdate)”。 请求将发送到：

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

若要部署到**订阅**，请使用“[部署 - 在订阅范围创建](/rest/api/resources/deployments/createorupdateatsubscriptionscope)”。 请求将发送到：

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

有关订阅级部署的详细信息，请参阅[在订阅级别创建资源组和资源](deploy-to-subscription.md)。

若要部署到**管理组**，请使用[部署 - 在管理组范围内创建](/rest/api/resources/deployments/createorupdateatmanagementgroupscope)。 请求将发送到：

```HTTP
PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{groupId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

有关管理组级部署的详细信息，请参阅[在管理组级别创建资源](deploy-to-management-group.md)。

本文中的示例使用资源组部署。

## <a name="deploy-with-the-rest-api"></a>使用 REST API 进行部署

1. 设置[常见参数和标头](/rest/api/azure/)，包括身份验证令牌。

1. 如果目前没有资源组，请创建资源组。 提供订阅 ID、新资源组的名称，以及解决方案所需的位置。 有关详细信息，请参阅[创建资源组](/rest/api/resources/resourcegroups/createorupdate)。

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2019-05-01
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

1. 若要部署模板，请在请求 URI 中提供订阅 ID、资源组名称和部署名称。

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2019-05-01
   ```

   在请求正文中，提供指向模板和参数文件的链接。 有关参数文件的详细信息，请参阅[创建资源管理器参数文件](parameter-files.md)。

   请注意，**mode** 设置为 **Incremental**。 要运行完全部署，将**模式**设置为 **"完成**"。 使用完整模式时要小心，因为可能会无意中删除不在模板中的资源。

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental"
    }
   }
   ```

    如果想要记录响应内容或/和请求内容，请在请求中包括 **debugSetting**。

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    可以将存储帐户设置为使用共享访问签名 (SAS) 令牌。 有关详细信息，请参阅[使用共享访问签名委托访问权限](/rest/api/storageservices/delegating-access-with-a-shared-access-signature)。

    如果需要为参数（如密码）提供敏感值，请将该值添加到密钥保管库。 在部署过程中检索密钥保管库，如前面的示例所示。 有关详细信息，请参阅[在部署期间传递安全值](key-vault-parameter.md)。

1. 可以将模板和参数包含在请求正文中，而不是链接到模板和参数的文件。 以下示例显示了内联模板和参数的请求正文：

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
            "apiVersion": "2018-02-01",
            "name": "[variables('storageAccountName')]",
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

1. 若要获取模板部署的状态，请使用[部署 - 获取](/rest/api/resources/deployments/get)。

   ```HTTP
   GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
   ```

## <a name="next-steps"></a>后续步骤

- 若要在出错时回退到成功的部署，请参阅[出错时回退到成功的部署](rollback-on-error.md)。
- 若要指定如何处理存在于资源组中但未在模板中定义的资源，请参阅 [Azure 资源管理器部署模式](deployment-modes.md)。
- 若要了解如何处理异步 REST 操作，请参阅[跟踪异步 Azure 操作](../management/async-operations.md)。
- 要了解有关模板的更多信息，请参阅[了解 ARM 模板的结构和语法](template-syntax.md)。

