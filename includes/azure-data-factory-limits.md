---
title: include 文件
description: include 文件
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 06/20/2018
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 4aa4809c57eaf26b10053d432f9191580ec143a0
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2018
ms.locfileid: "44381062"
---
数据工厂是一项多租户服务，具有以下默认限制，目的是确保客户订阅不受彼此工作负荷的影响。 订阅的许多限制只需联系支持部门即可提高，最多可提高到最大限制。

### <a name="version-2"></a>版本 2

| 资源 | 默认限制 | 最大限制 |
| -------- | ------------- | ------------- |
| Azure 订阅中的数据工厂 | 50 | [联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 数据工厂中的实体（管道、数据集、触发器、链接服务以及集成运行时）总数 | 5000 | [联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 一个订阅中 Azure-SSIS 集成运行时的总 CPU 内核数 | 128 | [联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 每个管道的并行管道运行 | 100 | [联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 每个数据工厂中的并行管道运行量 | 10,000  | [联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 每个管道中的最大活动数（包括容器的内部活动） | 40 | 40 |
| 每个管道的最大参数个数 | 50 | 50 |
| ForEach 项 | 100,000 | 100,000 |
| ForEach 并行度 | 20 | 50 |
| 每个表达式的字符数 | 8,192 | 8,192 |
| 最小翻转窗口触发间隔 | 15 分钟 | 15 分钟 |
| 管道活动运行的最大超时时间 | 7 天 | 7 天 |
| 管道对象的每个对象字节数 <sup>1</sup> | 200 KB | 200 KB |
| 数据集和关联服务对象的每个对象字节数 <sup>1</sup> | 100 KB | 2000 KB |
| 每个复制活动运行的数据集成单位 <sup>3</sup> | 256 | [联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 编写 API 调用 | 2500/小时<br/><br/> 此限制是由 Azure 资源管理器而不是 Azure 数据工厂所强加的。 | 请[联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)。 |
| 读取 API 调用 | 12,500/小时<br/><br/> 此限制是由 Azure 资源管理器而不是 Azure 数据工厂所强加的。 | [联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 每分钟监视的查询数 | 1000 | [联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 每分钟的实体 CRUD 操作数 | 50 | [联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |


### <a name="version-1"></a>版本 1

| **资源** | **默认限制** | **最大限制** |
| --- | --- | --- |
| Azure 订阅中的数据工厂 |50 |[联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 数据工厂中的管道 |2500 |[联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 数据工厂中的数据集 |5000 |[联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 每个数据集的并发切片数 |10 |10 |
| 管道对象的每个对象字节数 <sup>1</sup> |200 KB |200 KB |
| 数据集和关联服务对象的每个对象字节数 <sup>1</sup> |100 KB |2000 KB |
| 订阅中的 HDInsight 按需群集核心数 <sup>2</sup> |60 |[联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 每个复制活动运行的云数据移动单位 <sup>3</sup> |32 |[联系支持人员](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) |
| 管道活动运行的重试次数 |1000 |MaxInt（32 位） |

<sup>1</sup> 管道、数据集和链接服务对象代表工作负荷的逻辑组。 对这些对象的限制与可以使用 Azure 数据工厂服务移动或处理的数据量无关。 可以缩放数据工厂以处理 PB 量级的数据。

<sup>2</sup> 按需 HDInsight 核心数从包含数据工厂的订阅中分配。 因此，上述限制是数据工厂针对按需 HDInsight 核心强制实施的核心限制，不同于与 Azure 订阅关联的核心限制。

<sup>3</sup> 云到云的复制操作采用用于 v2 的数据集成单位 (DIU) 或用于 v1 的云数据移动单位 (DMU)。 它是一种度量单位，代表单个单位在数据工厂中的能力（包含 CPU、内存、网络资源分配）。 就某些方案来说，使用更多 DMU 可以提高复制吞吐量。 请参阅[数据集成单位 (V2)](../articles/data-factory/copy-activity-performance.md#data-integration-units) 和[云数据移动单位 (V1)](../articles/data-factory/v1/data-factory-copy-activity-performance.md#cloud-data-movement-units) 部分中的详细信息，以及 [Azure 数据工厂定价页](https://azure.microsoft.com/pricing/details/data-factory/)以了解计费详情。

<sup>4</sup> 集成运行时 (IR) 是 Azure 数据工厂用于在不同的网络环境之间提供以下数据集成功能的计算基础结构：数据移动、调度活动来计算服务，SSIS 包的执行。 有关详细信息，请参阅[集成运行时概述](../articles/data-factory/concepts-integration-runtime.md)。

| **资源** | **默认下限** | **最小限制** |
| --- | --- | --- |
| 计划间隔 |15 分钟 |15 分钟 |
| 重试尝试之间的间隔 |1 秒 |1 秒 |
| 重试超时值 |1 秒 |1 秒 |

#### <a name="web-service-call-limits"></a>Web 服务调用限制
Azure 资源管理器限制 API 调用。 可以根据 [Azure 资源管理器 API 限制](../articles/azure-subscription-service-limits.md#resource-group-limits)中规定的频率执行 API 调用。
