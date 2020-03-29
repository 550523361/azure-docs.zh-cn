---
title: 不雅内容筛选 - 翻译文本 API
titleSuffix: Azure Cognitive Services
description: 使用亵渎筛选来确定在 Azure 认知服务翻译器文本 API 中翻译的文本中的亵渎程度。
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: e7e2097e0d3daf360f1fa0f30bf3fd2c62c07163
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "73836227"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>使用翻译文本 API 添加不雅内容筛选

通常，Translator 服务在翻译中会保留源中存在的不雅内容。 不雅程度和使词语不雅的语境在不同的文化之间有所不同。 因此，在目标语言中的不雅程度可能会被放大或降低。

如果希望避免在翻译中看到不雅内容（即使源文本中存在不雅内容），可以使用 Translate() 方法中提供的不雅内容筛选选项。 使用该选项可以选择是要看到不雅内容被删除、标有相应标记还是不被执行任何操作。

Translate() 方法采用“options”参数，该参数包含新元素“ProfanityAction”。 可接受的 ProfanityAction 值为“NoAction”、“Marked”和“Deleted”。

## <a name="accepted-values-of-profanityaction-and-examples"></a>可接受的 ProfanityAction 值和示例
|ProfanityAction 值 | 操作 | 示例：源 - 日语 | 示例：目标 - 英语|
| :---|:---|:---|:---|
| NoAction | 默认。 与不设置此选项等效。 不雅内容从源传递到目标。 | 彼は変態です。 | He is a jerk. |
| Marked | 将不雅词语括在 XML 标记 \<profanity> ... \</profanity> 内。 | 彼は変態です。 | He is a \<profanity>jerk\</profanity>. |
| Deleted | 将不雅词语从输出中删除且不替换。 | 彼は。 | 他是一个。 |

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [通过 Translator API 调用应用不雅内容筛选](reference/v3-0-translate.md)
