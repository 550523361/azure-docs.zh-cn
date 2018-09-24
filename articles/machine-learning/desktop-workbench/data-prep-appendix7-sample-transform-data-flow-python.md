---
title: 可用于 Azure 机器学习数据准备的转换数据流转换示例 | Microsoft Docs
description: 本文档提供一组可用于 Azure 机器学习数据准备的转换数据流转换示例
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 82295e412c70065ff8ce3a686aec790614f39f2e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "46947194"
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>自定义数据流转换示例 (Python) 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


菜单中的转换名称为“转换数据流(脚本)”。 阅读本附录前，请先阅读 [Python 扩展性概述](data-prep-python-extensibility-overview.md)。

## <a name="transform-frame"></a>转换帧
### <a name="create-a-new-column-dynamically"></a>动态新建列 
动态创建一个列 (**city2**)，然后将 San Francisco 的多个不同版本协调为现有 city 列中的某个版本。
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>添加新的聚合
使用通过 first 和 last 聚合函数针对 score 列计算所得的结果新建帧。 这些结果按 **risk_category** 列分组。
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>对列进行去极值处理 
重新表述数据，满足减少列中离群值的公式的要求。
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>转换数据流
### <a name="fill-down"></a>向下填充 

向下填充需要两个转换。 它假设数据如下表所示：

|省/直辖市/自治区         |城市       |
|--------------|-----------|
|Washington    |Redmond    |
|              |Bellevue   |
|              |Issaquah   |
|              |西雅图    |
|California    |洛杉矶|
|              |San Diego  |
|              |San Jose   |
|Texas         |达拉斯     |
|              |San Antonio|
|              |Houston    |

1. 使用以下代码创建“添加列(脚本)”转换：
```python
    row['State'] if len(row['State']) > 0 else None
```

2. 创建包含以下代码的“转换数据流(脚本)”转换：
```python
    df = df.fillna( method='pad')
```

数据现在如下表所示：

|省/直辖市/自治区         |newState         |城市       |
|--------------|--------------|-----------|
|Washington    |Washington    |Redmond    |
|              |Washington    |Bellevue   |
|              |Washington    |Issaquah   |
|              |Washington    |西雅图    |
|California    |California    |洛杉矶|
|              |California    |San Diego  |
|              |California    |San Jose   |
|Texas         |Texas         |达拉斯     |
|              |Texas         |San Antonio|
|              |Texas         |Houston    |


### <a name="min-max-normalization"></a>最小值最大值规范化
```python
    df["NewCol"] = (df["Col1"]-df["Col1"].mean())/df["Col1"].std()
```