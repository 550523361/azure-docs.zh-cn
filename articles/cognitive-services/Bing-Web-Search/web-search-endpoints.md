---
title: Web 搜索终结点
titleSuffix: Azure Cognitive Services
description: Web 搜索 API 终结点摘要。
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 11/14/2018
ms.author: v-gedod
ms.openlocfilehash: 2f7e6cd577b1eabbaabdfe87fca8ea0f036a062d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149384"
---
# <a name="web-search-endpoint"></a>Web 搜索终结点

“Web 搜索 API”返回网页、新闻、图像、视频和[实体](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web)。 实体包含有关人员、地点或主题的摘要信息。

## <a name="endpoint"></a>终结点

若要使用必应 API 获取 Web 搜索结果，请向以下终结点发送 `GET` 请求。 标头和 URL 参数定义了更多规范。

**终结点**：返回与 `?q=""` 定义的用户搜索查询相关的 Web 结果。

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```

终结点：有关标头、参数、市场代码、响应对象、错误等内容的详细信息，请参阅[必应 Web API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) 参考。

## <a name="response-json"></a>响应 JSON

对 Web 搜索请求的响应包括作为 JSON 对象的所有结果。 分析结果需要处理每种类型元素的过程。 有关示例，请参阅[教程](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app)和[源代码](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/Tutorials/Bing-Web-Search)。

## <a name="next-steps"></a>后续步骤

必应 API 支持根据其类型返回结果的搜索操作。 所有搜索终结点均将结果作为 JSON 响应对象返回。  所有终结点均支持按经度、纬度和搜索半径返回特定语言和位置的查询。

若要完整了解每个终结点支持的参数，请参阅每种类型对应的参考页面。
有关使用 Web 搜索 API 的基本请求的示例，请参阅[搜索 Web 快速入门](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web)。
