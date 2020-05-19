---
title: 猥亵筛选-翻译人员
titleSuffix: Azure Cognitive Services
description: 使用猥亵筛选来确定在 Azure 认知服务翻译人员的文本中翻译的猥亵级别。
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 864f6a6d92306c40713f66b526c8a8df1683d3c4
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83586790"
---
# <a name="add-profanity-filtering-with-the-translator"></a>通过转换器添加猥亵语言筛选器

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
> [使用你的翻译人员呼叫应用猥亵语言筛选](reference/v3-0-translate.md)
