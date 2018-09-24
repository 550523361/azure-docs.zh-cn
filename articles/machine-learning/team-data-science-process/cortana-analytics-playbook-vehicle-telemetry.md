---
title: 预测车辆的运行状况和行驶习惯 - Azure | Microsoft Docs
description: 通过 Cortana Intelligence 的功能获得对车辆运行状况和驾驶习惯的实时和预测性深入了解。
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.openlocfilehash: 02a12e917ed36367ffac1ac2e7a1fef1c6098ea7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985361"
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>车辆遥测分析解决方案操作手册
此菜单链接至此操作手册的此章节。 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>概述
超级计算机已经搬出了实验室，现在停在车库中。 这些超级计算机现在正放置在包含无数传感器的先进汽车中。 这些传感器使它们每秒能够跟踪和监视数百万个事件。 到 2020 年，这些汽车中的大部分将连接到 Internet。 挖掘这些丰富的数据可以提供更高的安全性、可靠性，因此可提供更好的驾驶体验。 Microsoft 通过 Cortana Intelligence 使得这个美梦成真。

Cortana Intelligence 是一个完全托管的大数据和高级分析套件，可将数据转换为智能操作。 Cortana Intelligence 车辆遥测分析解决方案模板演示了汽车经销商、汽车制造商和保险公司如何能够获得车辆运行状况和驾驶习惯的实时和预测性见解。

该解决方案的实现为 [lambda 体系结构模式](https://en.wikipedia.org/wiki/Lambda_architecture)，显示了 Cortana Intelligence 平台在实时和批处理方面的全部潜力。

## <a name="architecture"></a>体系结构
下图演示了车辆遥测分析解决方案体系结构：

![解决方案体系结构示意图](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


此解决方案包括以下 Cortana Intelligence 组件并展示了它们的集成：

* **Azure 事件中心**：将数百万车辆遥测事件引入 Azure。
* **Azure 流分析**：提供对车辆运行状况的实时深入了解，并将数据持续存储到长期存储中，以进行更丰富的批处理分析。
* **Azure 机器学习**：实例检测异常，并使用批处理提供预测性见解。
* **Azure HDInsight**：大规模转换数据。
* **Azure 数据工厂**：处理批处理管道的业务流程、计划、资源管理和监视。
* **Power BI** 为该解决方案提供了丰富的仪表板，用于实时数据和预测分析可视化。

该解决方案访问两个不同的数据源： 

* **模拟车辆信号和诊断**：车辆远程信息处理模拟器在给定时间点发出对应于车辆的状态和驾驶模式的诊断信息和信号。 
* **车辆目录**：此参考数据集将 VIN 数映射到模型。

