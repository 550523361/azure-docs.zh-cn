---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 8f3a58d3a7470867ab23249bbd645289e010ad89
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223122"
---
### <a name="functions-2x-and-higher"></a>Functions 2.x 及更高版本

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            }
        }
    }
}  
```

|属性  |默认 | 说明 |
|---------|---------|---------|
|maxBatchSize|10 个|每个接收循环收到的最大事件计数。|
|prefetchCount|300|基础 `EventProcessorHost` 使用的默认预提取计数。 允许的最小值为 10。|
|batchCheckpointFrequency|1|创建 EventHub 游标检查点之前要处理的事件批数。|

> [!NOTE]
> 有关 Azure Functions 2.x 及更高版本中的 host.json 参考，请参阅 [Azure Functions 的 host.json 参考](../articles/azure-functions/functions-host-json.md)。

### <a name="functions-1x"></a>Functions 1.x

```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|属性  |默认 | 说明 |
|---------|---------|---------| 
|maxBatchSize|64|每个接收循环收到的最大事件计数。|
|prefetchCount|不适用|基础 `EventProcessorHost` 将使用的默认预提取。| 
|batchCheckpointFrequency|1|创建 EventHub 游标检查点之前要处理的事件批数。| 

> [!NOTE]
> 有关 Azure Functions 1.x 中 host.json 的参考，请参阅 [Azure Functions 1.x 的 host.json 参考](../articles/azure-functions/functions-host-json-v1.md)。
