---
title: Azure Application Insights 数据模型 - 跟踪遥测
description: 适用于跟踪遥测的 Application Insights 数据模型
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 31958b26cdb8a7897cf0051af6600014c07949fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671947"
---
# <a name="trace-telemetry-application-insights-data-model"></a>跟踪遥测：Application Insights 数据模型

[Application Insights](../../azure-monitor/app/app-insights-overview.md) 中的跟踪遥测表示搜索文本的 `printf` 样式跟踪语句。 `Log4Net`、`NLog` 和其他基于文本的日志文件条目将转换成此类型的实例。 跟踪没有作为可扩展性的度量。

## <a name="message"></a>消息

跟踪消息。

最大长度：32768 个字符

## <a name="severity-level"></a>严重性级别

跟踪严重性级别。 值可以是 `Verbose`、`Information`、`Warning`、`Error`、`Critical`。

## <a name="custom-properties"></a>自定义属性

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>后续步骤

- [在 Application Insights 中浏览 .NET 跟踪日志](../../azure-monitor/app/asp-net-trace-logs.md)。
- [在应用程序见解中浏览 Java 跟踪日志](../../azure-monitor/app/java-trace-logs.md)。
- 有关 Application Insights 的类型和数据模型，请参阅[数据模型](data-model.md)。
- [编写自定义跟踪遥测](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)
- 查看 Application Insights 支持的[平台](../../azure-monitor/app/platforms.md)。
