---
title: LUIS 应用中的话语
titleSuffix: Azure Cognitive Services
description: 话语是应用需要解释的用户输入。 收集你认为用户会输入的短语。 包括意思相同但在单词长度和单词位置上以不同方式构造的陈述。
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 2b21102a5416c1fd7e2abb35be677c48ffc63263
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638063"
---
# <a name="utterances-in-luis"></a>LUIS 中的陈述

**陈述**是应用需要解释的用户输入。 若要训练 LUIS 从其中提取意向和实体，请务必为每个意向捕获各种不同的输入。 主动学习或继续针对新陈述训练的过程对于 LUIS 提供的机器学习智能至关重要。

收集你认为用户会输入的短语。 包括意思相同但在单词长度和单词位置上以不同方式构造的陈述。 

## <a name="how-to-choose-varied-utterances"></a>如何选择不同的陈述
第一次开始[将示例话语 添加](luis-how-to-add-example-utterances.md)到 LUIS 模型时，请记住以下原则。

### <a name="utterances-arent-always-well-formed"></a>陈述并非始终格式正确
它可能是一个句子，比如“Book me a ticket to Paris”，也可能是句子的片段，比如“Booking”或“Paris flight”。  用户常犯拼写错误。 在规划应用时，请考虑是否在将用户输入传递给 LUIS 之前进行拼写检查。 [必应拼写检查 API][BingSpellCheck] 与 LUIS 集成。 发布 LUIS 应用时，可以将其与必应拼写检查 API 的外部键相关联。 如果你没有对用户陈述进行拼写检查，则应该针对包含拼写错误的陈述训练 LUIS。

### <a name="use-the-representative-language-of-the-user"></a>使用用户的代表性语言
选择陈述时，请注意你认为是常用术语或短语的内容对于客户端应用程序的典型用户来说可能并不常用。 他们可能没有域经验。 因此，在使用仅当用户是专家时才会说的术语或短语时要小心。

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>选择不同的术语和措辞
你会发现，即使你努力创造不同的句型，你仍然会重复一些词汇。

以这些示例陈述为例：

|示例陈述|
|--|
|如何买计算机？|
|在哪里买计算机？|
|我想要一台计算机，我该怎么做？|
|我什么时候能有一台计算机？| 

这里的核心术语“computer”没有变化。 他们可以说“台式电脑”、“笔记本电脑”、“工作站”，甚至就说“机器”。 LUIS 会智能地从上下文中推断同义词，但当你为训练创建陈述时，最好还是改变它们。

## <a name="example-utterances-in-each-intent"></a>每个意向的示例陈述
每个意向都需要有示例陈述，至少 10 到 15 个。 如果你的意向没有任何示例陈述，则将无法训练 LUIS。 如果你的意向只有一个或非常少的示例陈述，则 LUIS 将无法准确预测意向。 

## <a name="add-small-groups-of-10-15-utterances-for-each-authoring-iteration"></a>为每个创作迭代添加由 10-15 个陈述构成的小组
在模型的每个迭代中，不要添加大量陈述。 添加数量为 10 的陈述。 再次[训练](luis-how-to-train.md)、[发布](luis-how-to-publish-app.md)和[测试](luis-interactive-test.md)。  

LUIS 使用精心挑选的陈述构建有效的模型。 添加太多陈述是没有价值的，因为它会引起混乱。  

最好先从几个陈述开始，然后[审查终结点陈述](luis-how-to-review-endoint-utt.md)以进行正确的意向预测和实体提取。

## <a name="ignoring-words-and-punctuation"></a>忽略单词和标点
如果想要忽略示例陈述中的特定单词或标点符号，请在 _ignore_ 语法中使用[模式](luis-concept-patterns.md#pattern-syntax)。 

## <a name="training-utterances"></a>训练陈述
训练是非确定性的：在不同版本或应用中，陈述预测可能略有不同。

## <a name="testing-utterances"></a>测试陈述 

开发人员应通过向终结点发送陈述来开始使用实际流量测试其 LUIS 应用程序。 这些陈述用于通过[审查陈述](luis-how-to-review-endoint-utt.md)来改善意向和实体的表现。 使用 LUIS 网站测试窗格提交的测试不会通过终结点发送，因此不会对主动学习有所帮助。 

## <a name="review-utterances"></a>审查陈述
在模型经过训练、发布并接收[终结点](luis-glossary.md#endpoint)查询后，请[审查 LUIS 建议的陈述](luis-how-to-review-endoint-utt.md)。 LUIS 会选择意向或实体得分较低的终结点陈述。 

## <a name="best-practices"></a>最佳做法
请查看[最佳做法](luis-concept-best-practices.md)以了解详细信息。

## <a name="next-steps"></a>后续步骤
有关定型 LUIS 应用以理解用户话语的信息，请参阅[添加示例话语](luis-how-to-add-example-utterances.md)。

[BingSpellCheck]: https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/proof-text