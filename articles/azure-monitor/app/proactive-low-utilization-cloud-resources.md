---
title: 智能检测 - Azure Application Insights 检测到的云资源使用率低下 | Microsoft Docs
description: 使用 Azure Application Insights 监视应用程序是否存在云资源使用率低下的情况。
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: 7cf72068b9cabceb0c5b535986ac4dfb62151b94
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61297497"
---
# <a name="low-cpu-utilization-in-cloud-resources-preview"></a>云资源（预览）CPU 使用率低下

Application Insights 自动分析应用程序中每个角色实例的 CPU 使用情况，并检测 CPU 使用率低下的实例。 通过此检测，用户可以通过减少每个角色使用的角色实例数，或通过减少角色数来减少 Azure 资源并降低成本。

此功能需要[配置性能计数器](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters)，除此之外，不需要其他特殊设置。 当应用生成足够的 CPU 性能计数器遥测数据（处理器时间百分比）时，它处于活动状态。

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>何时会收到此类型的智能检测通知？
当很多 Web/辅助角色实例出现 CPU 利用率低下的情况时，系统会发出典型通知。

## <a name="does-my-app-definitely-consume-too-many-resources"></a>应用程序是否一定消耗了过多资源？
不是，通知并不意味着应用肯定消耗了过多资源。 尽管这种 CPU 使用率低下的情况通常表明可以减少资源消耗，但这种行为可能只是特定角色的典型表现，或者可能存在业务合理性，并且可以忽略。 例如，可能其他资源（如内存/网络，而不是 CPU）需要多个实例。

## <a name="how-do-i-fix-it"></a>如何解决问题？
通知包括诊断信息，以在诊断进程中提供支持：
1. **会审。** 通知会显示应用中 CPU 使用率低下的角色。 这可以帮助你对问题分配优先级。
2. **划分范围。** 有多少角色表现出 CPU 使用率低下，每个角色中有多少个实例 CPU 使用率低下？ 可以从通知中获取此信息。
3. **诊断。** 检测包含所使用的 CPU 百分比，显示一段时间内每个实例的 CPU 利用率。 还可以使用链接到支持信息（如 CPU 使用率百分点值）的相关项和报告，帮助进一步诊断问题。
