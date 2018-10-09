---
title: 对 LUIS 应用进行批处理测试 - 语言理解
titleSuffix: Azure Cognitive Services
description: 使用批处理测试持续优化应用程序并改进其语言理解能力。
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 414862d7ba1956f9449fde3ba063906fae6ef01b
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041602"
---
# <a name="batch-testing-in-luis"></a>LUIS 中的批处理测试

批处理测试验证你的[活动](luis-concept-version.md#active-version)定型模型，以判断其预测准确性。 批处理测试可帮助你在图表中查看当前定型的模型中的每个意向和实体的准确性。 查看批处理测试结果，以采取适当操作来提升准确性，例如，如果应用经常无法标识正确意向，则向意向添加更多示例表达。

## <a name="group-data-for-batch-test"></a>批处理测试的组数据
对于 LUIS 来说，用于批处理测试的表达必须是全新，这一点很重要。 如果具有表达数据集，将其划分为三组：添加到意向的表达、从已发布的终结点接收的表达，以及在定型 LUIS 后对其进行批处理测试的表达。 

## <a name="a-dataset-of-utterances"></a>表达数据集
提交表达批处理文件（称为数据集），以用于批处理测试。 该数据集是一个 JSON 格式的文件，包含最多 1,000 个被标记为非重复的表达。 可以在一个应用中测试最多 10 个数据集。 如果需要测试更多数据集，请删除数据集，然后添加新数据集。

|**规则**|
|--|
|*无重复表达|
|无分层的实体子级|
|1000 个表达以内|

*重复项被视为完全字符串匹配，不匹配的项将先进行标记。 

## <a name="entities-allowed-in-batch-tests"></a>允许在批处理测试中使用的实体
实体包括简单、分层父级和复合。 即使批处理文件中没有相应的实体，这些类型的所有实体也会出现在批处理测试实体筛选器中。


<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>
## <a name="batch-file-format"></a>批处理文件格式
批处理文件包含表达。 每个表达都必须具有任何你预测可被检测到的[机器学习实体](luis-concept-entity-types.md#types-of-entities)随附的预期意向预测。 

以下是具有正确语法的批处理文件的示例：

```JSON
[
  {
    "text": "Are there any janitorial jobs currently open?",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 14,
            "endPos": 23
        }
    ]
  },
  {
    "text": "I would like a fullstack typescript programming with azure job",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 15,
            "endPos": 46
        }
    ]
  },
  {
    "text": "Is there a database position open in Los Colinas?",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 11,
            "endPos": 18
        }
    ]
  },
  {
    "text": "Please find database jobs open today in Seattle",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 12,
            "endPos": 19
        }
    ]
  }
]
```

## <a name="batch-syntax-template"></a>批处理语法模板

使用以下模板启动批处理文件：

```JSON
[
  {
    "text": "example utterance goes here",
    "intent": "intent name goes here",
    "entities": 
    [
        {
            "entity": "entity name 1 goes here",
            "startPos": 14,
            "endPos": 23
        },
        {
            "entity": "entity name 2 goes here",
            "startPos": 14,
            "endPos": 23
        }
    ]
  }
]
```

批处理文件使用 startPos 和 endPos 属性来记录实体的开始和结束。 值从零开始，不得以空格开始或结束。 

这与使用 startIndex 和 endIndex 属性的查询日志不同。 


## <a name="common-errors-importing-a-batch"></a>导入批处理文件的常见错误
常见错误包括： 

> * 超过 1,000 个表达
> * 不具有实体属性的表达 JSON 对象
> * 在多个实体中标记的字词

## <a name="batch-test-state"></a>批处理测试状态
LUIS 跟踪每个数据集的最后一次测试的状态。 这包括大小（批处理中的表达数）、上次运行日期和最后结果（成功预测的表达数）。

<a name="sections-of-the-results-chart"></a>
## <a name="batch-test-results"></a>批处理测试结果
批处理测试结果是散点图，称为错误矩阵。 此图表对文件中的表达和当前模型的预测意向和实体进行四个方面的比较。 

“假正”和“假负”部分上的数据点指示错误，应对其进行调查。 如果所有数据点都在“真正”和“真负”部分上，则你的应用在此数据集上的准确性很好。

![图表的四个部分](./media/luis-concept-batch-test/chart-sections.png)

此图表可基于 LUIS 的当前定型来帮助你查找 LUIS 预测不正确的表达。 结果将按图表的每个区域来显示。 选择图表上的单个点，以查看表达信息，或选择区域名称，以查看该区域的表达结果。

![批处理测试](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>结果中的错误
批处理测试中的错误指示意向未按批处理文件中的指示进行预测。 在图表的两个红色部分中指示错误。 

假正部分指示表达匹配了不应匹配的意向或实体。 假负指示表达未匹配应匹配的意向或实体。 

## <a name="fixing-batch-errors"></a>修复批处理错误
如果在批处理测试中出现错误，可以向意向添加更多表达，和/或在实体中标记更多表达，以帮助 LUIS 在意向间进行区分。 如果你已添加了表达，且对其进行了标记，但在批处理测试中仍收到预测错误，请考虑添加[短语列表](luis-concept-feature.md)功能，其中包含特定于域的词汇，以帮助 LUIS 更快地理解。 

## <a name="next-steps"></a>后续步骤

* 了解如何[测试批处理](luis-how-to-batch-test.md)
