---
title: Azure 数据工厂中的 Filter 活动
description: Filter 活动将筛选输入。
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/04/2018
ms.openlocfilehash: 5794a2c754e2082fdf45ad689bdb89ea494541c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "73679860"
---
# <a name="filter-activity-in-azure-data-factory"></a>Azure 数据工厂中的 Filter 活动
可以在管道中使用 Filter 活动将筛选器表达式应用到输入数组。 

## <a name="syntax"></a>语法

```json
{
    "name": "MyFilterActivity",
    "type": "filter",
    "typeProperties": {
        "condition": "<condition>",
        "items": "<input array>"
    }
}
```

## <a name="type-properties"></a>Type 属性

properties | 描述 | 允许的值 | 必选
-------- | ----------- | -------------- | --------
name | `Filter` 活动的名称。 | String | 是
type | 必须设置为****“filter”。 | String | 是
条件 (condition) | 要用于筛选输入的条件。 | 表达式 | 是
items | 应该应用筛选器的输入数组。 | 表达式 | 是

## <a name="example"></a>示例

在此示例中，管道有两个活动：Filter**** 和 ForEach****。 Filter 活动配置为筛选输入数组中值大于 3 的项。 然后，ForEach 活动会循环访问筛选的值，并将变量 **test** 设置为当前值。

```json
{
    "name": "PipelineName",
    "properties": {
        "activities": [{
                "name": "MyFilterActivity",
                "type": "filter",
                "typeProperties": {
                    "condition": "@greater(item(),3)",
                    "items": "@pipeline().parameters.inputs"
                }
            },
            {
            "name": "MyForEach",
            "type": "ForEach",
            "dependsOn": [
                {
                    "activity": "MyFilterActivity",
                    "dependencyConditions": [
                        "Succeeded"
                    ]
                }
            ],
            "userProperties": [],
            "typeProperties": {
                "items": {
                    "value": "@activity('MyFilterActivity').output.value",
                    "type": "Expression"
                },
                "isSequential": "false",
                "batchCount": 1,
                "activities": [
                    {
                        "name": "Set Variable1",
                        "type": "SetVariable",
                        "dependsOn": [],
                        "userProperties": [],
                        "typeProperties": {
                            "variableName": "test",
                            "value": {
                                "value": "@string(item())",
                                "type": "Expression"
                            }
                        }
                    }
                ]
            }
        }],
        "parameters": {
            "inputs": {
                "type": "Array",
                "defaultValue": [1, 2, 3, 4, 5, 6]
            }
        },
        "variables": {
            "test": {
                "type": "String"
            }
        },
        "annotations": []
    }
}
```

## <a name="next-steps"></a>后续步骤
查看数据工厂支持的其他控制流活动： 

- [If Condition 活动](control-flow-if-condition-activity.md)
- [执行管道活动](control-flow-execute-pipeline-activity.md)
- [对于每个活动](control-flow-for-each-activity.md)
- [获取元数据活动](control-flow-get-metadata-activity.md)
- [Lookup 活动](control-flow-lookup-activity.md)
- [网站活动](control-flow-web-activity.md)
- [Until 活动](control-flow-until-activity.md)
