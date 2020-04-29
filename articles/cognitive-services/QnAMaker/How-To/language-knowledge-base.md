---
title: 语言概念-QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker 支持多种语言版本的知识库内容。 但是，每个 QnA Maker 服务应保留一种语言。 创建的第一个知识库（面向特定 QnA Maker 服务）设置该服务的语言。
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: diberry
ms.openlocfilehash: 38701e8bbef1c5d78eca2242105e81fe7261c0f6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "79220629"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>QnA Maker 的知识库内容的语言支持

在资源中创建第一个知识库时，将选择服务语言。 资源中的所有其他知识库必须采用相同的语言。

该语言确定 QnA Maker 提供的结果与用户查询的相关关系。

## <a name="one-language-for-all-knowledge-bases-in-resource"></a>资源中所有知识库的一种语言

QnA Maker 允许在创建第一个知识库时为 QnA 服务选择语言。 对于 QnA Maker 资源中的所有知识库，它们必须使用相同的语言。 不能更改此语言。

在一个资源中以不同的语言创建知识库会对 QnA Maker 为响应用户查询提供的结果的相关性产生负面影响。

查看[支持的语言](../overview/language-support.md#languages-supported)列表，以及如何影响[匹配和相关性](#query-matching-and-relevance)。

## <a name="select-language-when-creating-first-knowledge-base"></a>创建第一个知识库时选择语言

语言选择是在资源中创建第一个知识库的步骤的一部分。

![为第一个知识库选择语言 QnA Maker 门户屏幕截图](../media/language-support/select-language-when-creating-knowledge-base.png)

## <a name="query-matching-and-relevance"></a>查询匹配和相关性
QnA Maker 依赖于[Azure 认知搜索语言分析器](https://docs.microsoft.com/rest/api/searchservice/language-support)来提供结果。

尽管 Azure 认知搜索功能对于支持的语言是相同的，但 QnA Maker 有一个位于 Azure 搜索结果之上的其他 ranker。 在此 ranker 模型中，使用以下语言的一些特殊语义和基于 word 的功能。

|具有其他 ranker 的语言|
|--|
|中文|
|捷克语|
|荷兰语|
|英语|
|法语|
|德语|
|匈牙利语|
|意大利语|
|日语|
|朝鲜语|
|波兰语|
|葡萄牙语|
|西班牙语|
|瑞典语|

此附加排名是 QnA Maker 的 ranker 的内部工作。

## <a name="verify-language"></a>验证语言

可以从 QnA Maker 中的 "服务设置" 页验证 QnA Maker 资源的语言。

![QnA Maker "服务设置" 页的门户屏幕截图](../media/language-support/language-knowledge-base.png)


## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [迁移知识库](../Tutorials/migrate-knowledge-base.md)
