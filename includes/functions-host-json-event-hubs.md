---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: b5d8f67a70961aab21312b6f241081dcb33f66fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "67172744"
---
```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|properties  |默认 | 描述 |
|---------|---------|---------| 
|maxBatchSize|64|每个接收循环收到的最大事件计数。|
|prefetchCount|不适用|基础 EventProcessorHost 将要使用的默认 PrefetchCount。| 
|batchCheckpointFrequency|1|创建 EventHub 游标检查点之前要处理的事件批数。| 
