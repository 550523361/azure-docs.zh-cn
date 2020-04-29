---
title: 映射数据流中的接收器转换
description: 了解如何在映射数据流中配置接收器转换。
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
manager: anandsub
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: 4b10a4c98abd6bec4074bf35764a9cbb85d5b157
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "81605967"
---
# <a name="sink-transformation-in-mapping-data-flow"></a>映射数据流中的接收器转换

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

转换数据后，可以将数据接收到目标数据集。 每个数据流都需要至少一个接收器转换，但你可以根据需要写入任意多个接收器来完成转换流。 若要写入其他接收器，请通过新的分支和条件拆分创建新的流。

每个接收器转换只与一个数据工厂数据集相关联。 数据集定义要写入的数据的形状和位置。

## <a name="supported-sink-connectors-in-mapping-data-flow"></a>映射数据流中支持的接收器连接器

当前，以下数据集可用于接收器转换：
    
* [Azure Blob 存储](connector-azure-blob-storage.md#mapping-data-flow-properties)（JSON、Avro、Text、Parquet）
* [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md#mapping-data-flow-properties) （JSON，Avro，Text，Parquet）
* [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#mapping-data-flow-properties) （JSON，Avro，Text，Parquet）
* [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#mapping-data-flow-properties)
* [Azure SQL 数据库](connector-azure-sql-database.md#mapping-data-flow-properties)
* [Azure CosmosDB](connector-azure-cosmos-db.md#mapping-data-flow-properties)

特定于这些连接器的设置位于 "**设置**" 选项卡中。有关这些设置的信息位于连接器文档中。 

Azure 数据工厂可访问超过[90 个本机连接器](connector-overview.md)。 若要将数据写入数据流中的其他源，请使用复制活动在数据流完成后从支持的暂存区域中加载数据。

## <a name="sink-settings"></a>接收器设置

添加接收器后，通过 "**接收器**" 选项卡进行配置。可在此处选取或创建接收器写入的数据集。 以下视频介绍了几种用于文本分隔文件类型的不同接收器选项：

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4tf7T]

![接收器设置](media/data-flow/sink-settings.png "接收器设置")

**架构偏差：** [架构偏差](concepts-data-flow-schema-drift.md)是指数据工厂本机处理数据流中的灵活架构的能力，无需显式定义列更改。 启用 "**允许架构偏移**" 可以在接收器数据架构中定义的内容的基础上写入其他列。

**验证架构：** 如果选择了 "验证架构"，则在源投影中找不到传入源架构的任何列或数据类型不匹配时，数据流将失败。 使用此设置可强制源数据满足定义投影的协定。 它在数据库源方案中非常有用，用来指示列名称或类型已更改。

## <a name="field-mapping"></a>字段映射

与选择转换类似，在接收器的 "**映射**" 选项卡中，您可以决定写入哪些传入列。 默认情况下，映射所有输入列，包括偏移列。 这就是所谓的**自动映射**。

关闭自动映射时，可以选择添加基于列的固定映射或基于规则的映射。 基于规则的映射允许您编写具有模式匹配的表达式，而固定映射将映射逻辑列名和物理列名。 有关基于规则的映射的详细信息，请参阅[映射数据流中的列模式](concepts-data-flow-column-pattern.md#rule-based-mapping-in-select-and-sink)。

## <a name="custom-sink-ordering"></a>自定义接收器排序

默认情况下，数据按非确定性顺序写入多个接收器。 当转换逻辑完成并且接收顺序可能因每次运行而异时，执行引擎将并行写入数据。 若要指定和确切的接收顺序，请在数据流的 "常规" 选项卡中启用**自定义接收器排序**。 启用后，将按递增顺序写入接收器。

![自定义接收器排序](media/data-flow/custom-sink-ordering.png "自定义接收器排序")

## <a name="data-preview-in-sink"></a>接收器中的数据预览

在调试群集上提取数据预览时，不会将任何数据写入接收器。 将返回数据的外观的快照，但不会向目标写入任何内容。 若要测试将数据写入接收器，请从管道画布运行管道调试。

## <a name="next-steps"></a>后续步骤
创建数据流后，请将数据流[活动添加到管道](concepts-data-flow-overview.md)。
