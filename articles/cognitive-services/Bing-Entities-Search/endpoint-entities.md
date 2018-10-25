---
title: 必应实体搜索终结点
titlesuffix: Azure Cognitive Services
description: 总结了必应实体搜索 API 终结点。
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: d781a4b3cd0119f5624b4dd20b514894ea339414
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816576"
---
# <a name="entity-search-endpoint"></a>实体搜索终结点
实体搜索 API 包括一个端点，该端点根据查询从 Web 返回实体。

## <a name="endpoint"></a>终结点
若要使用必应 API 获取实体结果，请向以下终结点发送 `GET` 请求。 使用标头和 URL 参数来定义更多规范。

终结点：返回与 `?q=""` 定义的用户搜索查询相关的实体。
```
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

有关标头、参数、市场代码、响应对象、错误等详细信息，请参阅[必应实体搜索 API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) 参考。

## <a name="response-json"></a>响应 JSON
对实体搜索请求的响应包括作为 JSON 对象的结果。 有关结果示例，请参阅[入门](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start)。

## <a name="next-steps"></a>后续步骤
必应 API 支持根据其类型返回结果的搜索操作。 所有搜索终结点均将结果作为 JSON 响应对象返回。  所有终结点支持后列查询：按经度、纬度和搜索半径返回特定语言和/或位置的查询。

有关每个终结点支持的参数的完整信息，请参阅每个类型的参考页面。
有关使用实体搜索 API 的示例，请参阅[入门](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start)和[调整大小并裁剪缩略图](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/resize-and-crop-thumbnails)。