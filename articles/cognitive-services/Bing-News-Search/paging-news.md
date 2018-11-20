---
title: 如何翻阅可返回的新闻文章 - 必应新闻搜索
titlesuffix: Azure Cognitive Services
description: 介绍了如何翻阅必应新闻搜索可返回的所有新闻文章。
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 0507f2cfb1d75025d1b6aadccc442326a52ceebc
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739798"
---
# <a name="paging-news"></a>翻页新闻

调用必应新闻搜索 API 时，必应会返回结果的列表。 此列表是与查询相关的所有结果的一部分。 若要获取估计的可用结果总数，请访问答案对象的 [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) 字段。  
  
以下示例显示了新闻答案包含的 `totalEstimatedMatches` 字段。  
  
```  
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
若要翻页浏览可用文章，请使用 [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) 和 [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset) 查询参数。  
  
`count` 参数指定响应中要返回的结果数。 可以在响应中请求的最大结果数为 100。 默认值为 10。 提供的实际结果数可能小于请求获取的结果数。

`offset` 参数指定要跳过的结果数。 `offset` 从零开始且应小于 (`totalEstimatedMatches` - `count`)。  

如果希望每页显示 20 篇文章，可以将 `count` 设置为 20，并将 `offset` 设置为 0，以获得第一页结果。 对于每个后续页面，将按 20 增加 `offset`（例如，20、40）。  
  
以下显示了从偏移量 40 开始请求 20 篇新文章的示例。  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
如果 `count` 默认值适用于实现，请仅指定 `offset` 查询参数，如以下示例所示：  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
> [!NOTE]
> 翻页仅适用于新闻搜索 (/news/search)，而不适用于热门主题 (/news/trendingtopics) 或新闻类别 (/news)。

> [!NOTE]
> `TotalEstimatedAnswers` 字段是可以为当前查询检索的搜索结果总数的估计值。  设置 `count` 和 `offset` 参数时，`TotalEstimatedAnswers` 数值可能会更改。 