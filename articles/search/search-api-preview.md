---
title: 适用于 Azure 搜索的预览版 REST API 2017-11-11-Preview - Azure 搜索
description: Azure 搜索服务 REST API 版本 2017-11-11-Preview 包括试验功能，如同义词和 moreLikeThis 搜索。
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 06/28/2018
ms.author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: 524c1a6d083db02349c7dae9a0131228613dc170
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/11/2019
ms.locfileid: "55997604"
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>Azure 搜索服务 REST API 版本 2017-11-11-Preview
本文介绍了 `api-version=2017-11-11-Preview` 版的 Azure 搜索服务 REST API，该版提供尚未正式发布的实验性功能。

> [!NOTE]
> 预览功能可用于目的为收集反馈的测试和试验，可能随时更改。 强烈建议不要在生产应用程序中使用预览版 API。


## <a name="new-in-2017-11-11-preview"></a>2017-11-11-Preview 中的新增功能

[自动完成](search-autocomplete-tutorial.md)加入现有的[建议 API](https://docs.microsoft.com/rest/api/searchservice/suggestions)，可为搜索栏添加互补的预先输入体验。 自动完成返回用户可以选择作为后续搜索的查询字符串的候选查询词。 建议会返回实际文档以响应部分输入：搜索结果是即时的，并且随着搜索词输入的长度和特殊性的增加而动态变化。

[**认知搜索**](cognitive-search-concept-intro.md)是 Azure 搜索中的新扩充功能，用于查找非文本源和无差别文本中的潜在信息，将其转换为 Azure 搜索中可全文搜索的内容。 预览版 REST API 中引入或修改了以下资源。 无论是调用正式发布的版本，还是预览版，所有其他 REST API 都是相同的。

+ [技能集操作 (api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/skillset-operations)

+ [创建索引器 (api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

+ [预定义技能](cognitive-search-predefined-skills.md)

不管你如何设置 API 版本，所有其他 REST API 均相同。 例如，`GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` 和 `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11`（无 `Preview`）在功能上是相同的。

## <a name="other-preview-features"></a>其他预览功能

公共预览版中仍包含早期预览版中发布的功能。 如果你正在调用使用较早预览版 API 版本的 API，可以继续使用该版本或切换到 `2017-11-11-Preview`，而不会对预期行为作出任何更改。

+ [Azure Blob 索引中的 CSV 文件](search-howto-index-csv-blobs.md)在 `api-version=2015-02-28-Preview` 中引入，保留了一项预览功能。 此功能是 Azure Blob 索引的一部分，通过参数设置调用。 CSV 文件中的每行作为单独的文档进行索引。

+ [Azure Blob 索引中的 JSON 数组](search-howto-index-json-blobs.md)在 `api-version=2015-02-28-Preview` 中引入，保留了一项预览功能。 此功能是 Azure Blob 索引的一部分，通过参数设置调用。 其中数组中的每个元素都作为单独的文档索引。

+ [moreLikeThis 查询参数](search-more-like-this.md)查找与特定文档相关的文档。 早期预览版中已有此功能。 


## <a name="how-to-call-a-preview-api"></a>如何调用预览版 API

早期预览版仍然可用，但随着时间推移会变得过时。 如果代码调用 `api-version=2016-09-01-Preview` 或 `api-version=2015-02-28-Preview`，那些调用仍然有效。 但是，只有最新预览版会获得改进。 

以下示例语法说明了对预览 API 版本的调用。

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

Azure 搜索服务在多个版本内可用。 有关详细信息，请参阅 [API 版本](search-api-versions.md)。
