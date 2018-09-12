---
title: 使用参考数据在 Azure 流分析中查找
description: 本文介绍如何使用参考数据在 Azure 流分析作业的查询设计中查找或关联数据。
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: 2a6172a4e163d937f5a0a2140831b730bca23c3f
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696517"
---
# <a name="using-reference-data-for-lookups-in-stream-analytics"></a>使用参考数据在流分析中查找
参考数据（也称为查找表）是一个静态的或本质上缓慢变化的有限数据集，用于执行查找或与数据流相关联。 Azure 流分析在内存中加载参考数据以实现低延迟流处理。 为了在 Azure 流分析作业中利用引用数据，通常会在查询中使用[引用数据联合](https://msdn.microsoft.com/library/azure/dn949258.aspx)。 流分析使用 Azure Blob 存储作为引用数据的存储层，并且通过 Azure 数据工厂，可以从[基于云和本地的任意数量的数据存储](../data-factory/copy-activity-overview.md)将引用数据转换和/或复制到 Azure Blob 存储，以用作引用数据。 引用数据建模为 blob 序列（在输入配置中定义），这些 blob 按blob 名称中指定的日期/时间顺序升序排列。 它**仅**支持使用**大于**序列中最后一个 blob 指定的日期/时间的日期/时间添加到序列的末尾。

流分析支持**最大大小为 300 MB** 的参考数据。 只有简单的查询才能达到参考数据最大大小 300 MB 限制。 随着查询复杂性增加以包括有状态处理（如开窗聚合、临时联接接和临时分析函数），预计参考数据的最大支持大小将减少。 如果 Azure 流分析无法加载参考数据并执行复杂操作，则作业将耗尽内存并失败。 在这种情况下，SU % 利用率指标将达到 100%。    

|**流单元数**  |**大约支持的最大大小（以 MB 为单位）**  |
|---------|---------|
|1   |50   |
|3   |150   |
|至少 6   |300   |

作业增加的流单元数量超过 6 不会增加参考数据支持的最大大小。

对压缩的支持不可用于参考数据。 

## <a name="configuring-reference-data"></a>配置引用数据
若要配置引用数据，首先需要创建一个属于“引用数据”类型的输入。 下表介绍在根据说明创建引用数据输入时需要提供的每个属性：

|**属性名称**  |**说明**  |
|---------|---------|
|输入别名   | 一个友好名称会用于作业查询，以便引用此输入。   |
|存储帐户   | 存储 blob 的存储帐户的名称。 如果其与流分析作业所在订阅相同，则可从下拉菜单中进行选择。   |
|存储帐户密钥   | 与存储帐户关联的密钥。 如果存储帐户的订阅与流分析作业相同，则自动填充此密钥。   |
|存储容器   | 容器对存储在 Microsoft Azure Blob 服务中的 blob 进行逻辑分组。 将 blob 上传到 Blob 服务时，必须为该 blob 指定一个容器。   |
|路径模式   | 用于对指定容器中的 blob 进行定位的路径。 在路径中，可以选择指定一个或多个使用以下 2 个变量的实例：<BR>{date}、{time}<BR>示例 1：products/{date}/{time}/product-list.csv<BR>示例 2：products/{date}/product-list.csv<BR><br> 如果指定路径中不存在 blob，流分析作业将无限期地等待 blob 变为可用状态。   |
|日期格式 [可选]   | 如果在指定的路径模式中使用了 {date}，则可从支持格式的下拉列表中选择组织 blob 所用的日期格式。<BR>示例：YYYY/MM/DD、MM/DD/YYYY，等等。   |
|时间格式 [可选]   | 如果在指定的路径模式中使用了 {time}，则可从支持格式的下拉列表中选择组织 blob 所用的时间格式。<BR>示例：HH、HH/mm、或 HH-mm。  |
|事件序列化格式   | 为确保查询按预计的方式运行，流分析需要了解你对传入数据流使用哪种序列化格式。 对于引用数据，所支持的格式是 CSV 和 JSON。  |
|编码   | 目前只支持 UTF-8 这种编码格式。  |

## <a name="static-reference-data"></a>静态参考数据
如果不希望参考数据发生变化，则可以通过在输入配置中指定静态路径来启用对静态参考数据的支持。 Azure 流分析从指定路径中获取 Blob。 不需要 {date} 和 {time} 替换令牌。 参考数据在流分析中不可变。 因此，不建议覆盖静态参考数据 Blob。

## <a name="generating-reference-data-on-a-schedule"></a>按计划生成引用数据
如果引用数据是缓慢变化的数据集，则使用 {date} 和 {time} 替换令牌在输入配置中指定路径模式即可实现对刷新引用数据的支持。 流分析会根据此路径模式选取更新的引用数据定义。 例如，使用 `sample/{date}/{time}/products.csv` 模式时，日期格式为“YYYY-MM-DD”，时间格式为“HH-mm”，可指示流分析在 2015 年 4 月 16 日下午 5:30（UTC 时区）提取更新的 Blob `sample/2015-04-16/17-30/products.csv`。

Azure 流分析每间隔一分钟都会自动扫描刷新的参考数据 Blob。

> [!NOTE]
> 当前，流分析作业仅在计算机时间提前于 blob 名称中的编码时间时才查找 blob 刷新。 例如，该作业将尽可能查找 `sample/2015-04-16/17-30/products.csv`，但不会早于 2015 年 4 月 16 日下午 5:30（UTC 时区）。 它*永远不会*查找编码时间早于发现的上一个 blob 的 blob。
> 
> 例如 作业找到 blob `sample/2015-04-16/17-30/products.csv` 后，它将忽略编码日期早于 2015 年 4 月 16 日下午 5:30 的任何文件，因此如果晚到达的 `sample/2015-04-16/17-25/products.csv` blob 在同一容器中创建，该作业将不会使用它。
> 
> 同样，如果 `sample/2015-04-16/17-30/products.csv` 仅在 2015 年 4 月 16 日晚上 10:03 生成，但容器中没有更早日期的 blob，则该作业将从 2015 年 4 月 16 日晚上 10:03 起开始使用此文件，而在此之前使用以前的引用数据。
> 
> 这种情况的一个例外是，当作业需要按时重新处理数据时或第一次启动作业时。 开始时，作业查找的是在指定的作业开始时间之前生成的最新 blob。 这样做是为了确保在作业开始时存在**非空**引用数据集。 如果找不到引用数据集，该作业会显示以下诊断：`Initializing input without a valid reference data blob for UTC time <start time>`。
> 
> 

[Azure 数据工厂](https://azure.microsoft.com/documentation/services/data-factory/)可用来安排以下任务：创建流分析更新引用数据定义所需的已更新 blob。 数据工厂是一项基于云的数据集成服务，可对数据移动和转换进行安排并使其实现自动化。 数据工厂支持[连接到大量基于云和本地的数据存储](../data-factory/copy-activity-overview.md)以及按指定的定期计划轻松地移动数据。 有关如何将数据工厂管道设置为生成按预定义计划刷新的流分析引用数据的详细信息和分步指导，请查看此 [GitHub 示例](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs)。

## <a name="tips-on-refreshing-your-reference-data"></a>有关刷新引用数据的提示
1. 请勿覆盖参考数据 Blob，因为它们是不可变的。
2. 刷新参考数据的推荐方法是：
    * 使用路径模式中的 {date}/{time}
    * 使用作业输入中定义的相同容器和路径模式来添加新 Blob
    * 使用大于序列中最后一个 Blob 指定的日期/时间。
3. 引用数据 blob 并不按 blob 的“上次修改”时间排序，而是按 blob 名称中使用 {date} 和 {time} 替换项指定的日期和时间排序。
3. 为了避免必须列出大量 blob，请考虑删除不再对其进行处理的非常旧的 blob。 请注意，在某些情况下（如重新启动），ASA 可能需要重新处理一小部分 blob。

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [快速入门：使用 Azure 门户创建流分析作业](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
