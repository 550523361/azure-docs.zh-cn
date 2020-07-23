---
title: 映射数据流逆透视转换
description: Azure 数据工厂映射数据流逆透视转换
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 07/14/2020
ms.openlocfilehash: b7a05f33c199ab46508fa2b9bc0ef576f888ef27
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537708"
---
# <a name="azure-data-factory-unpivot-transformation"></a>Azure 数据工厂逆透视转换

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

通过将单个记录中多个列的值扩展为单个列中具有相同值的多个记录，可以在 ADF 映射数据流中使用逆透视。

![逆透视转换](media/data-flow/unpivot1.png "Unpivot 选项1")

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4B1RR]

## <a name="ungroup-by"></a>分组依据

![逆透视转换](media/data-flow/unpivot5.png "Unpivot 选项2")

首先，为你的透视聚合设置要用作分组依据的列。 使用列列表旁边的 + 号设置一个或多个列用于取消组合。

## <a name="unpivot-key"></a>逆透视键

![逆透视转换](media/data-flow/unpivot6.png "Unpivot 选项3")

透视键是 ADF 从行透视到列的列。 默认情况下，此字段的数据集中的每个唯一值都将透视到列。 但是，你也可以选择输入此数据集中要透视到列值的值。

## <a name="unpivoted-columns"></a>已逆透视列

![逆透视转换](media/data-flow//unpivot7.png "Unpivot 选项4")

最后，选择要用于透视值的聚合以及如何在转换的新输出投影中显示列。

（可选）你可以设置一个命名模式，在其中包含要在行值中的每个新列名称中添加的前缀、中间名和后缀。

例如，通过“区域”透视“销售”只会为你提供每个销售值的新列值。 例如： "25"，"50"，"1000"，.。。但是，如果您设置了前缀值 "Sales"，则 "Sales" 将作为这些值的前缀。

<img src="media/data-flow/unpivot3.png" width="400">

将列排列设置为“正常”，将所有透视列与其聚合值组合在一起。 将列排列设置为“横向”将在列和值之间交替。

![逆透视转换](media/data-flow//unpivot7.png "Unpivot 选项5")

最终的已逆透视数据结果集显示已逆透视到单独行值的列总值。

## <a name="next-steps"></a>后续步骤

使用[透视转换](data-flow-pivot.md)将行透视到列。
