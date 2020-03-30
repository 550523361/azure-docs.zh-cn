---
title: 实体识别认知技能
titleSuffix: Azure Cognitive Search
description: 从 Azure 认知搜索中的丰富管道中的文本中提取不同类型的实体。
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 6ef5952b6413563b2c2e16ff2218f709b414fb84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297813"
---
#    <a name="entity-recognition-cognitive-skill"></a>实体识别认知技能

**实体识别**技能从文本中提取各种类型的实体。 此技能使用认知服务中的[文本分析](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)提供的机器学习模型。

> [!NOTE]
> 随着通过增加处理频率、添加更多文档或添加更多 AI 算法来扩大范围，您需要[附加计费的认知服务资源](cognitive-search-attach-cognitive-services.md)。 调用认知服务中的 API 以及在 Azure 认知搜索中的文档破解阶段提取图像时，会产生费用。 提取文档中的文本不会产生费用。
>
> 内置技能执行按现有[认知服务即用即付价格](https://azure.microsoft.com/pricing/details/cognitive-services/)计费。 图像提取定价如 [Azure 认知搜索定价页](https://go.microsoft.com/fwlink/?linkid=2042400)所述。


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.EntityRecognitionSkill

## <a name="data-limits"></a>数据限制
记录的最大大小应为 50，000 个字符（以[`String.Length`](https://docs.microsoft.com/dotnet/api/system.string.length)） 如果在将数据发送到关键短语提取器之前需要拆分数据，请使用[文本拆分技能](cognitive-search-skill-textsplit.md)。

## <a name="skill-parameters"></a>技能参数

参数区分大小写并且都是可选的。

| 参数名称     | 描述 |
|--------------------|-------------|
| categories    | 应提取的类别的数组。  可能的类别类型有：`"Person"`、`"Location"`、`"Organization"`、`"Quantity"`、`"Datetime"`、`"URL"`、`"Email"`。 如果不提供类别，则返回所有类型。|
|defaultLanguageCode |    输入文本的语言代码。 支持以下语言： `ar, cs, da, de, en, es, fi, fr, hu, it, ja, ko, nl, no, pl, pt-BR, pt-PT, ru, sv, tr, zh-hans`. 并非所有实体类别都支持所有语言;因此，并非所有实体类别都支持所有语言。参见下面的注释。|
|minimumPrecision | 一个介于 0 和 1 之间的值。 如果置信度分数（输出`namedEntities`中）低于此值，则不返回实体。 默认值为 0。 |
|includeTypelessEntities | 如果要识别`true`不适合当前类别的已知实体，请设置为"设置为" 在`entities`复杂输出字段中返回已识别的实体。 例如，"Windows 10"是一个众所周知的实体（产品），但由于"产品"不是受支持的类别，因此此实体将包含在实体输出字段中。 默认为 `false` |


## <a name="skill-inputs"></a>技能输入

| 输入名称      | 描述                   |
|---------------|-------------------------------|
| languageCode    | 可选。 默认为 `"en"`。  |
| text          | 要分析的文本。          |

## <a name="skill-outputs"></a>技能输出

> [!NOTE]
> 并非所有实体类别都支持所有语言。 上面`"Person"`完整的`"Location"`语言列表`"Organization"`支持 、 和 实体类别类型。 `"Datetime"`只有`"URL"``"Email"`_de_de、en、es、fr 和_zh-hans_ `"Quantity"`支持提取 、和 类型。 _en_ _es_ _fr_ 有关详细信息，请参阅[文本分析 API 的语言和地区支持](https://docs.microsoft.com/azure/cognitive-services/text-analytics/language-support)。  

| 输出名称      | 描述                   |
|---------------|-------------------------------|
| 人员       | 一个字符串数组，其中，一个字符串表示一个人员名称。 |
| 位置  | 一个字符串数组，其中，一个字符串表示一个位置。 |
| 组织  | 一个字符串数组，其中，一个字符串表示一个组织。 |
| quantities  | 一个字符串数组，其中，每个字符串都表示一个数量。 |
| dateTimes  | 一个字符串数组，其中，每个字符串都表示一个日期时间（因为它以文本形式显示）值。 |
| urls | 一个字符串数组，其中，每个字符串都表示一个 URL |
| emails | 一个字符串数组，其中，每个字符串都表示一个电子邮件地址 |
| namedEntities | 复杂类型的数组，包含以下字段： <ul><li>category</li> <li>value（实际实体名称）</li><li>偏移（在文本中找到它的位置）</li><li>置信度（较高的值意味着它更成为一个真正的实体）</li></ul> |
| 实体 | 一个复杂类型数组，包含有关从文本提取的实体的丰富信息，具有以下字段 <ul><li> name（实际实体名称。 这表示一个“规范化”窗体）</li><li> wikipediaId</li><li>wikipediaLanguage</li><li>wikipediaUrl（实体的 Wikipedia 页面的链接）</li><li>bingId</li><li>type（识别的实体的类别）</li><li>subType（仅适用于某些类别，这提供实体类型的更精细视图）</li><li> matches（包含的复杂集合）<ul><li>text（实体的原始文本）</li><li>offset（找到它的位置）</li><li>length（原始实体文本的长度）</li></ul></li></ul> |

##    <a name="sample-definition"></a>示例定义

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "categories": [ "Person", "Email"],
    "defaultLanguageCode": "en",
    "includeTypelessEntities": true,
    "minimumPrecision": 0.5,
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      },
      {
        "name": "emails",
        "targetName": "contact"
      },
      {
        "name": "entities"
      }
    ]
  }
```
##    <a name="sample-input"></a>示例输入

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Contoso corporation was founded by John Smith. They can be reached at contact@contoso.com",
             "languageCode": "en"
           }
      }
    ]
}
```

##    <a name="sample-output"></a>示例输出

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "persons": [ "John Smith"],
        "emails":["contact@contoso.com"],
        "namedEntities": 
        [
          {
            "category":"Person",
            "value": "John Smith",
            "offset": 35,
            "confidence": 0.98
          }
        ],
        "entities":  
        [
          {
            "name":"John Smith",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Person",
            "subType": null,
            "matches": [{
                "text": "John Smith",
                "offset": 35,
                "length": 10
            }]
          },
          {
            "name": "contact@contoso.com",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Email",
            "subType": null,
            "matches": [
            {
                "text": "contact@contoso.com",
                "offset": 70,
                "length": 19
            }]
          },
          {
            "name": "Contoso",
            "wikipediaId": "Contoso",
            "wikipediaLanguage": "en",
            "wikipediaUrl": "https://en.wikipedia.org/wiki/Contoso",
            "bingId": "349f014e-7a37-e619-0374-787ebb288113",
            "type": null,
            "subType": null,
            "matches": [
            {
                "text": "Contoso",
                "offset": 0,
                "length": 7
            }]
          }
        ]
      }
    }
  ]
}
```

请注意，此技能输出中实体返回的偏移量直接从[文本分析 API](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)返回，这意味着如果您使用它们索引到原始字符串中，则应使用 .NET 中的[StringInfo](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo?view=netframework-4.8)类来提取正确的内容。  [更多详情可在此处找到。](https://docs.microsoft.com/azure/cognitive-services/text-analytics/concepts/text-offsets)

## <a name="error-cases"></a>错误案例
如果文档的语言代码不受支持，则返回错误，并且不提取任何实体。

## <a name="see-also"></a>请参阅

+ [内置技能](cognitive-search-predefined-skills.md)
+ [如何定义技能集](cognitive-search-defining-skillset.md)
