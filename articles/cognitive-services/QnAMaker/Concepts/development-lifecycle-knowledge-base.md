---
title: 知识库生命周期 - QnA Maker
description: QnA Maker 在反复的模型变更、表述示例、发布以及从终结点查询收集信息等周期中，会取得最佳的学习成效。
ms.topic: conceptual
ms.date: 02/27/2020
ms.openlocfilehash: 98fbd81baa717c981486f33cfb2b3a608cec27c7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "77914946"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>QnA Maker 中的知识库生命周期
QnA Maker 在反复的模型变更、表述示例、发布以及从终结点查询收集信息等周期中，会取得最佳的学习成效。

![创作周期](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>创建 QnA Maker 知识库
QnA Maker 知识库 (KB) 终结点基于知识库的内容为用户查询提供匹配度最高的答案。 创建知识库是用于设置问题、解答和相关元数据的内容存储库的一次性操作。 可以通过提取预先存在的内容（例如常见问题解答页面、产品手册或结构化问-答对）创建知识库。 了解如何[创建知识库](../quickstarts/create-publish-knowledge-base.md)。

## <a name="testing-and-updating-the-knowledge-base"></a>测试和更新知识库

知识库只要填充了内容（无论是以编辑方式填充还是通过自动提取填充），就可以用于测试。 可以通过 "**测试**" 面板在 QnA Maker 门户中完成交互式测试，方法是输入常见的用户查询，并验证返回的响应是否正确，以及是否有足够的置信度得分。

* **若要修复置信度较低的评分**：添加替代问题。
* **如果查询错误地返回[默认响应](../How-to/change-default-answer.md)**：将新答案添加到正确的问题。

测试更新这一紧凑周期会持续至得到满意的结果为止。 了解如何[测试知识库](../How-To/test-knowledge-base.md)。

对于大型 Kb，请使用带有[GENERATEANSWER API](../how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api)的自动测试和`isTest` body 属性，该属性查询`test`知识库而不是已发布的知识库。

```json
{
  "question": "example question",
  "top": 3,
  "userId": "Default",
  "isTest": true
}
```

## <a name="publish-the-knowledge-base"></a>发布知识库
完成知识库的测试后，即可发布此知识库。 发布将已测试知识库的最新版本推送到表示**已发布**知识库的专用 Azure 认知搜索索引。 还会创建一个终结点，可在应用程序或聊天机器人中调用此终结点。

这样一来，对知识库的测试版本进行的任何更改都不会影响可能已进入生产应用程序的已发布版本。

所有这些知识库都可以单独作为测试目标。 使用 Api，你可以在 generateAnswer 调用中以`isTest` body 属性作为目标的知识库的测试版本。

了解如何[发布知识库](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base)。

## <a name="monitor-usage"></a>监视使用情况
若要能记录服务的聊天日志，需要在[创建 QnA Maker 服务](../How-To/set-up-qnamaker-service-azure.md)时启用 Application Insights。

可以获取服务使用情况的各种分析信息。 了解如何使用 Application Insights 获取 [QnA Maker 服务的分析](../How-To/get-analytics-knowledge-base.md)。

根据从分析中得出的结论，适当[更新知识库](../How-To/edit-knowledge-base.md)。

## <a name="version-control-for-data-in-your-knowledge-base"></a>知识库中数据的版本控制

数据的版本控制是通过 QnA Maker 门户的 "**设置**" 页上的 "导入/导出" 功能提供的。

您可以通过导出知识库中的`.tsv`或`.xls`格式来备份知识库。 导出后，将此文件作为常规源代码管理检查的一部分包含在内。

需要返回到特定版本时，需要从本地系统导入该文件。 导出的**知识库只能通过**"**设置**" 页上的 "导入" 使用。 不能将它用作文件或 URL 文档数据源。 这会将知识库中当前存在的问题和答案替换为导入的文件的内容。

## <a name="test-and-production-knowledge-base"></a>测试和生产知识库
知识库是通过 QnA Maker 创建、维护和使用的问题和答案集的存储库。 每个 QnA Maker 资源可以保存多个知识库。

知识库有两种状态：*测试*和*发布*。

### <a name="test-knowledge-base"></a>测试知识库

*测试知识库*是当前已编辑、保存和测试的版本，它是响应的准确性和完整性。 对测试知识库所做的更改不会影响应用程序或聊天机器人的最终用户。 测试知识库`test`在 HTTP 请求中称为。 QnA Maker `test`的门户交互**测试**窗格中提供了该知识。

### <a name="production-knowledge-base"></a>生产知识库

*已发布的知识库*是在聊天机器人或应用程序中使用的版本。 发布知识库的操作会将测试知识库的内容放入知识库中的已发布版本。 由于已发布的知识库是应用程序通过终结点使用的版本，因此请确保内容正确并经过测试。 已发布的知识库`prod`在 HTTP 请求中称为。


## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [活动学习建议](./active-learning-suggestions.md)