---
title: 新增功能通告
titleSuffix: Azure Cognitive Search
description: 新增功能和增强功能的通告，包括 Azure 搜索服务已重命名为 Azure 认知搜索。
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 06/08/2020
ms.openlocfilehash: 97defe2af5b82cccbaf289ccbd805b608b978a43
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84736078"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Azure 认知搜索中的新增功能

了解服务中的新增功能。 请将本页加入书签，以随时了解该服务的最新信息。

## <a name="feature-announcements"></a>功能增强

### <a name="june-2020"></a>2020 年 6 月

Azure 机器学习技能是集成 Azure 机器学习推断终结点的新技能类型。 门户体验支持在认知搜索技能集中发现和集成 Azure 机器学习终结点。 此发现要求将认知搜索和 Azure ML 服务部署在同一订阅中。 要注册 AML 技能预览版，[请填写表单](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0jK7x7HQYdDm__YfEsbtcZUMTFGTFVTOE5XMkVUMFlDVFBTTlYzSlpLTi4u)。 请先查看[此教程](cognitive-search-tutorial-aml-custom-skill.md)。

### <a name="may-2020-microsoft-build"></a>2020 年 5 月（Microsoft Build 大会）

+ [调试会话](cognitive-search-debug-session.md)功能现提供预览。 [注册以请求访问](https://aka.ms/DebugSessions)。 调试会话提供基于门户的接口来调查和解决技能组的问题。 在调试会话中创建的修补程序可以保存到生产技能集。 请先查看[此教程](cognitive-search-tutorial-debug-sessions.md)。

+ 安全增强功能包括能够[设置无法在公共 Internet 上访问的专用搜索终结点（预览）](service-create-private-endpoint.md)。 还可以[配置用于入站防火墙支持的 IP 规则（预览）](service-configure-firewall.md)。

+ 使用[系统托管标识（预览）](search-howto-managed-identities-data-sources.md)设置与 Azure 数据源的连接以编制索引。 适用于从 Azure 数据源（例如 Azure SQL 数据库、Azure Cosmos DB 和 Azure 存储）引入内容的[索引器](search-indexer-overview.md)。

+ 使用 [scoringStatistics=global](index-similarity-and-scoring.md#scoring-statistics) 和 sessionId 查询参数，将搜索评分的计算方式从每个分片更改为所有分片。

### <a name="march-2020"></a>2020 年 3 月

+ [本机 blob 软删除（预览版）](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection)意味着 Azure 认知搜索中的 Azure blob 存储索引器会识别处于软删除状态的 blob，并在编制索引过程中删除相应的搜索文档。

+ 新的稳定版[管理 REST API (2020-03-13)](https://docs.microsoft.com/rest/api/searchmanagement/management-api-versions) 现已可用。 

### <a name="february-2020"></a>2020 年 2 月

+ [PII 检测（预览版）](cognitive-search-skill-pii-detection.md)是编制索引期间使用的一项认知技能，它可以从输入文本中提取个人身份信息，并可让你通过多种方式在该文本中屏蔽此类信息。

+ [自定义实体查找（预览版）](cognitive-search-skill-custom-entity-lookup.md )可在用户自定义的单词和短语列表中查找文本。 它使用此列表为包含任何匹配实体的所有文档加上标签。 该技能还支持一定程度的模糊匹配，应用此匹配方法可以查找类似但不完全相同的匹配项。 

### <a name="january-2020"></a>2020 年 1 月

+ [客户管理的加密密钥](search-security-manage-encryption-keys.md)现已推出正式版。 如果使用 REST，可以通过 `api-version=2019-05-06` 访问该功能。 对于托管代码，即使该功能现在不再以预览版提供，但正确的包仍是 [.NET SDK 版本 8.0-preview](search-dotnet-sdk-migration-version-9.md)。 

+ 可以通过两种机制（两者的功能目前以预览版提供）对搜索服务进行私密访问：

  + 可以使用管理 REST API `api-version=2019-10-01-Preview` 创建服务，来仅限特定的 IP 地址进行访问。 预览版 API 在 [CreateOrUpdate API](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service) 中提供 **IpRule** 和 **NetworkRuleSet** 属性。 此预览版功能可在选定的区域中使用。 有关详细信息，请参阅[如何使用管理 REST API](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api)。

  + 可以预配一个支持 Azure 专用终结点的 Azure 搜索服务，用于从同一虚拟网络中的客户端建立连接。此功能目前以受限访问预览版提供。 有关详细信息，请参阅[创建专用终结点以建立安全连接](service-create-private-endpoint.md)。

### <a name="december-2019"></a>2019 年 12 月

+ [创建应用（预览版）](search-create-app-portal.md)是门户中用于生成可下载 HTML 文件的新向导。 文件附带了嵌入式脚本，用于呈现可正常运行的“localhost”式 Web 应用，该应用绑定到搜索服务中的索引。 页面在向导中可配置，可以包含搜索栏、结果区域、边栏导航和自动提示查询支持。 可以脱机修改 HTML 以扩展或者自定义工作流或外观。

+ [创建专用终结点以建立安全连接（预览版）](service-create-private-endpoint.md)中介绍了如何设置专用链接，以便与搜索服务建立安全连接。 此预览版功能按请求提供，使用 [Azure 专用链接](../private-link/private-link-overview.md)和 [Azure 虚拟网络](../virtual-network/virtual-networks-overview.md)作为解决方案的一部分。

### <a name="november-2019---ignite-conference"></a>2019 年 11 月- Ignite 大会

+ [增量扩充（预览版）](cognitive-search-incremental-indexing-conceptual.md)将缓存和有状态性添加到了扩充管道，使你能够处理特定的步骤或阶段，且不丢失已处理的内容。 以前，对扩充管道进行任何更改都需要重新生成整个管道。 使用增量扩充时，会保留高开销分析（尤其是图像分析）的输出。

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ [文档提取（预览版）](cognitive-search-skill-document-extraction.md)是编制索引期间使用的一项认知技能，用于从技能集中提取文件内容。 以前，文档破解只能在技能集执行之前发生。 使用此项新增技能，还可以在技能集的执行过程中执行此操作。

+ [文本翻译](cognitive-search-skill-text-translation.md)是编制索引期间使用的一项认知技能，它可以评估文本，并为每个记录返回翻译成指定目标语言的文本。

+ [Power BI 模板](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md)可以在 Power BI Desktop 中立即开始可视化和分析知识存储中的扩充内容。 此模板适用于通过[导入数据向导](knowledge-store-create-portal.md)创建的 Azure 表投影。

+ 索引器现在支持 [Azure Data Lake Storage Gen2（预览版）](search-howto-index-azure-data-lake-storage.md)、[Cosmos DB Gremlin API（预览版）](search-howto-index-cosmosdb.md)和 [Cosmos DB Cassandra API（预览版）](search-howto-index-cosmosdb.md)。 可以使用[此表单](https://aka.ms/azure-cognitive-search/indexer-preview)进行注册。 在我们同意你参与预览计划后，你会收到确认电子邮件。

### <a name="july-2019"></a>2019 年 7 月

+ 在 [Azure 政府云](../azure-government/documentation-government-services-webandmobile.md#azure-cognitive-search)中推出了正式版。

<a name="new-service-name"></a>

## <a name="new-service-name"></a>新服务名称

Azure 搜索现已重命名为 **Azure 认知搜索**，以反映认知技能和 AI 处理在核心操作中的更广泛用途（但仍为可选）。 API 版本、NuGet 包、命名空间和终结点未有变化。 新的和现有的搜索解决方案不受服务名称更改的影响。

## <a name="service-updates"></a>服务更新

在 Azure 网站上可以找到 Azure 认知搜索的[服务更新通告](https://azure.microsoft.com/updates/?product=search&status=all)。