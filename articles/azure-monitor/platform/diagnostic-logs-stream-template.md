---
title: 使用资源管理器模板自动启用诊断设置
description: 了解如何使用 Resource Manager 模板创建诊断设置，以便将诊断日志流式传输到事件中心，或者将其存储在存储帐户中。
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 3/26/2018
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: 7edce5175a1dda66abf3316cb8f0eb33e9f64ef7
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58371462"
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>在创建资源时使用 Resource Manager 模板自动启用诊断设置
本文介绍如何使用 [Azure 资源管理器模板](../../azure-resource-manager/resource-group-authoring-templates.md)在创建资源时配置资源的诊断设置。 这可以自动启动流式处理将诊断日志和指标到事件中心、 将其存档在存储帐户，或将它们发送到 Log Analytics 工作区中，创建一个资源。

> [!WARNING]
> 存储帐户中日志数据的格式将在 2018 年 11 月 1 日更改为 JSON Lines。 [请参阅此文章来了解此影响，以及如何通过更新工具来处理新格式。](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

通过 Resource Manager 模板启用诊断日志时，所用方法取决于资源类型。

* **非计算**资源（例如，网络安全组、逻辑应用、自动化）使用[此文中描述的诊断设置](../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings)。
* **计算**（基于 WAD/LAD）资源使用[此文中描述的 WAD/LAD 配置文件](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines)。

本文介绍如何使用其中一种方法配置诊断。

基本步骤如下所示：

1. 以 JSON 文件格式创建一个模板，说明如何创建资源和启用诊断。
2. [使用任意部署方法部署模板](../../azure-resource-manager/resource-group-template-deploy.md)。

下面是一个需为非计算资源和计算资源生成的模板 JSON 文件的示例。

## <a name="non-compute-resource-template"></a>非计算资源模板
对于非计算资源，需要做两件事：

1. 向存储帐户名称、 事件中心授权规则 ID 和/或 Log Analytics 工作区 ID （允许在存储帐户，日志流式传输到事件中心和/或将日志发送到 Azure Monitor 中存档诊断日志） 的参数 blob 添加参数。
   
    ```json
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
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
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Azure Resource ID of the Log Analytics workspace for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. 在资源数组（包含需启用诊断日志的资源）中，添加类型为 `[resource namespace]/providers/diagnosticSettings` 的资源。
   
    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
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
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ],
          "metrics": [
            {
              "category": "AllMetrics",
              "enabled": true,
              "retentionPolicy": {
                "enabled": false,
                "days": 0
              }
            }
          ]
        }
      }
    ]
    ```

诊断设置的属性 blob 遵循 [此文所述的格式](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate)。 添加 `metrics` 属性还可将资源指标发送到这些相同输出，前提是[该资源支持 Azure Monitor 指标](../../azure-monitor/platform/metrics-supported.md)。

下面是一个完整的示例，说明了如何创建逻辑应用，以及如何启用流式传输到事件中心和在存储帐户中进行存储的功能。

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "https://azure.microsoft.com/status/feed/"
    },
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name of the setting. Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
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
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Logic/workflows",
      "name": "[parameters('logicAppName')]",
      "apiVersion": "2016-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
          "dependsOn": [
            "[resourceId('Microsoft.Logic/workflows', parameters('logicAppName'))]"
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
                "category": "WorkflowRuntime",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ],
            "metrics": [
              {
                "timeGrain": "PT1M",
                "enabled": true,
                "retentionPolicy": {
                  "enabled": false,
                  "days": 0
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>计算资源模板
若要允许对计算资源（例如虚拟机或 Service Fabric 群集）进行诊断，需执行以下操作：

1. 将 Azure 诊断扩展添加到 VM 资源定义中。
2. 以参数形式指定存储帐户和/或事件中心。
3. 将 WADCfg XML 文件的内容添加到 XMLCfg 属性中，对所有 XML 字符进行适当的转义。

> [!WARNING]
> 这最后一步操作起来比较复杂。 [参阅此文](../../virtual-machines/extensions/diagnostics-template.md#diagnostics-configuration-variables)获取相关示例，了解如何将诊断配置架构拆分成进行了正确转义和格式化操作的变量。
> 
> 

整个过程（包括示例）详见[此文档](../../virtual-machines/extensions/diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。

## <a name="next-steps"></a>后续步骤
* [详细了解 Azure 诊断日志](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [将 Azure 诊断日志流式传输到事件中心](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md)


