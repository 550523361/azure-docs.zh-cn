---
title: 映射数据流可视化监控
description: 如何以可视化方式监视 Azure 数据工厂数据流
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/07/2019
ms.openlocfilehash: dea0f9a038958ea747147a179020545f2f6922a2
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605347"
---
# <a name="monitor-data-flows"></a>监视数据流

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

完成数据流的生成和调试之后，需要在管道上下文中将数据流安排为按计划执行。 可以使用触发器从 Azure 数据工厂计划管道。 或者，可以使用 Azure 数据工厂管道生成器中的“立即触发”选项执行单次运行，以便在管道上下文中测试数据流。

执行管道时，可以监视该管道及其包含的所有活动，包括数据流活动。 在 Azure 数据工厂 UI 的左侧面板中单击监视图标。 将会看到下面所示的屏幕。 使用突出显示的图标可以深入到管道中的活动，包括数据流活动。

![数据流监控](media/data-flow/mon001.png "数据流监视")

您将看到此级别的统计信息，包括运行时间和状态。 活动级别的运行 ID 不同于管道级别的运行 ID。 上一级别的运行 ID 适用于管道。 单击眼镜图标可以查看有关数据流执行的深入详细信息。

![数据流监控](media/data-flow/mon002.png "数据流监视")

在图形节点监视视图中，将会看到数据流图形的简化只读版本。

![数据流监控](media/data-flow/mon003.png "数据流监视")

## <a name="view-data-flow-execution-plans"></a>查看数据流执行计划

在 Spark 中执行数据流时，Azure 数据工厂会根据整个数据流确定最佳代码路径。 此外，执行路径可能出现在不同的横向扩展节点和数据分区上。 因此，监视图形表示流的设计，它考虑到了转换的执行路径。 单击单个节点时，会看到“分组”，它们表示在群集上统一执行的代码。 看到的计时和计数表示相对于设计中各个步骤的组。

![数据流监控](media/data-flow/mon004.png "数据流监视")

* 在监视窗口中单击空白区域时，底部窗格中的统计信息将显示每个接收器的计时和行数，以及生成转换沿袭的接收器数据的转换。

* 选择单个转换时，您将在右侧面板上收到其他反馈，该面板显示分区统计信息、列计数、偏斜度（跨分区分布的数据是否均匀）和峰度（数据有多尖尖）。

* 在节点视图中单击“接收器”时，会看到列沿袭。 在整个数据流中，可通过三种不同的方法来累积要载入接收器的列。 它们分别是：

  * 已计算：使用列进行条件处理或数据流中的表达式中，但不要将其放在 Sink 中
  * 派生：该列是您在流中生成的新列，即它不存在于源中
  * 映射：列源自源，而您的列正在将其映射到汇字段
  * 数据流状态：执行的当前状态
  * 群集启动时间：获取 JIT Spark 计算环境以执行数据流的时间
  * 转换数：在流中执行的转换步骤数
  
![数据流监控](media/data-flow/monitornew.png "数据流监控新增")  
  
## <a name="monitor-icons"></a>监视图标

此图标表示转换数据已在群集中缓存，因此计时和执行路径已考虑到这种情况：

![数据流监控](media/data-flow/mon004.png "数据流监视")

在转换中还会看到绿色的圆圈图标。 它们表示数据流入的接收器数目。
