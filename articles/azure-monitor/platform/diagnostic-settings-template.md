---
title: 使用资源管理器模板在 Azure 中创建诊断设置
description: 使用资源管理器模板创建诊断设置，以便将 Azure 平台日志转发到 Azure Monitor 日志、Azure 存储或 Azure 事件中心。
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 73f2b3ea90cc94fa3411552c7b812fe53eb4dbbb
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87008092"
---
# <a name="create-diagnostic-setting-in-azure-using-a-resource-manager-template"></a>使用资源管理器模板在 Azure 中创建诊断设置
Azure Monitor 中的[诊断设置](diagnostic-settings.md)指定 Azure 资源及其依赖的 Azure 平台所收集的[平台日志](platform-logs-overview.md)的发送位置。 本文详细地举例说明了如何使用 [Azure 资源管理器模板](../../azure-resource-manager/templates/template-syntax.md)将诊断设置创建并配置为将平台日志收集到不同的目标。

> [!NOTE]
> 由于不能使用 PowerShell 或 CLI 为 Azure 活动日志[创建诊断设置](diagnostic-settings.md)（例如其他 Azure 资源的诊断设置），因此请根据本文中的信息为活动日志创建资源管理器模板，并使用 PowerShell 或 CLI 部署模板。

## <a name="deployment-methods"></a>部署方法
可以使用任何有效的方法（包括 PowerShell 和 CLI）部署资源管理器模板。 活动日志的诊断设置必须使用适用于 CLI 的 `az deployment create` 或 PowerShell 的 `New-AzDeployment` 部署到订阅。 资源日志的诊断设置必须使用适用于 CLI 的 `az group deployment create` 或 PowerShell 的 `New-AzResourceGroupDeployment` 部署到资源组。

有关详细信息，请参阅[使用资源管理器模板和 Azure PowerShell 部署资源](../../azure-resource-manager/templates/deploy-powershell.md)和[使用资源管理器模板和 Azure CLI 部署资源](../../azure-resource-manager/templates/deploy-cli.md)。 





## <a name="resource-logs"></a>资源日志
对于资源日志，请将类型为 `<resource namespace>/providers/diagnosticSettings` 的资源添加到模板。 “属性”部分遵循[诊断设置 - 创建或更新](/rest/api/monitor/diagnosticsettings/createorupdate)中所述的格式。 在 `logs` 部分中为要收集的资源提供每个有效类别的 `category`。 如果[资源支持指标](metrics-supported.md)，则添加 `metrics` 属性以将资源指标收集到相同的目标。

下面是一个模板，可将特定资源的资源日志类别收集到 Log Analytics 工作区、存储帐户和事件中心。

```json
"resources": [
  {
    "type": "/<resource namespace>/providers/diagnosticSettings",
    "name": "[concat(parameters('resourceName'),'/microsoft.insights/', parameters('settingName'))]",
    "dependsOn": [
      "[<resource Id for which resource logs will be enabled>]"
    ],
    "apiVersion": "2017-05-01-preview",
    "properties": {
      "name": "[parameters('settingName')]",
      "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
      "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
      "eventHubName": "[parameters('eventHubName')]",
      "workspaceId": "[parameters('workspaceId')]",
      "logs": [
        {
          "category": "<category name>",
          "enabled": true
        }
      ],
      "metrics": [
        {
          "category": "AllMetrics",
          "enabled": true
        }
      ]
    }
  }
]
```



### <a name="example"></a>示例
下面是一个示例，该示例创建自动缩放设置的诊断设置，而该设置允许将资源日志流式传输到事件中心、存储帐户和 Log Analytics 工作区。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "autoscaleSettingName": {
            "type": "string",
            "metadata": {
                "description": "The name of the autoscale setting"
            }
        },
        "settingName": {
            "type": "string",
            "metadata": {
                "description": "The name of the diagnostic setting"
            }
        },
        "workspaceId": {
            "type": "string",
            "metadata": {
                "description": "ResourceIDl of the Log Analytics workspace in which resource logs should be saved."
            }
        },
        "storageAccountId": {
            "type": "string",
            "metadata": {
              "description": "ResourceID of the Storage Account in which resource logs should be saved."
            }
        },
        "eventHubAuthorizationRuleId": {
            "type": "string",
            "metadata": {
              "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
            }
        },
        "eventHubName": {
            "type": "string",
            "metadata": {
                "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
            }
        }
    },
    "variables": {},
    "resources": [
    {
      "type": "microsoft.insights/autoscalesettings/providers/diagnosticSettings",
      "apiVersion": "2017-05-01-preview",
      "name": "[concat(parameters('autoscaleSettingName'),'/microsoft.insights/', parameters('settingName'))]",
      "dependsOn": [
        "[resourceId('Microsoft.Insights/autoscalesettings', parameters('autoscaleSettingName'))]"
      ],
      "properties": {
        "workspaceId": "[parameters('workspaceId')]",
        "storageAccountId": "[parameters('storageAccountId')]",
        "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
        "eventHubName": "[parameters('eventHubName')]",
        "logs": [
          {
            "category": "AutoscaleScaleActions",
            "enabled": true
          },
          {
            "category": "AutoscaleEvaluations",
            "enabled": true
          }
        ]
      }
    }
  ]
}
```

## <a name="activity-log"></a>活动日志
对于 Azure 活动日志，请添加一个 `Microsoft.Insights/diagnosticSettings` 类型的资源。 可用类别在[活动日志中的类别](./activity-log.md#view-the-activity-log)中列出。 下面是一个模板，可将所有活动日志类别收集到 Log Analytics 工作区、存储帐户和事件中心。


```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "settingName": {
            "type": "string",
            "metadata": {
                "description": "The name of the diagnostic setting"
            }
        },
        "workspaceId": {
            "type": "string",
            "metadata": {
                "description": "ResourceID of the Log Analytics workspace in which resource logs should be saved."
            }
        },
        "storageAccountId": {
            "type": "string",
            "metadata": {
              "description": "ResourceID of the Storage Account in which resource logs should be saved."
            }
        },
        "eventHubAuthorizationRuleId": {
            "type": "string",
            "metadata": {
              "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
            }
        },
        "eventHubName": {
            "type": "string",
            "metadata": {
                "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Insights/diagnosticSettings",
            "apiVersion": "2017-05-01-preview",
            "name": "[parameters('settingName')]",
            "location": "global",
            "properties": {
                "workspaceId": "[parameters('workspaceId')]",
                "storageAccountId": "[parameters('storageAccountId')]",
                "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
                "eventHubName": "[parameters('eventHubName')]",
                "logs": [
                    {
                        "category": "Administrative",
                        "enabled": true
                    },
                    {
                        "category": "Security",
                        "enabled": true
                    },
                    {
                        "category": "ServiceHealth",
                        "enabled": true
                    },
                    {
                        "category": "Alert",
                        "enabled": true
                    },
                    {
                        "category": "Recommendation",
                        "enabled": true
                    },
                    {
                        "category": "Policy",
                        "enabled": true
                    },
                    {
                        "category": "Autoscale",
                        "enabled": true
                    },
                    {
                        "category": "ResourceHealth",
                        "enabled": true
                    }
                ]
            }
        }
    ]
}
```


## <a name="next-steps"></a>后续步骤
* 阅读有关 [Azure 中的平台日志](platform-logs-overview.md)的详细信息。
* 了解[诊断设置](diagnostic-settings.md)。
