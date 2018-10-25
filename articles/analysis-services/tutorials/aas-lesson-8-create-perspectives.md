---
title: Azure Analysis Services 教程第 8 课：创建透视 | Microsoft Docs
description: 介绍了在 Azure Analysis Services 教程项目中如何创建透视。
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: e92e86f59058f1f226c67f873530cd9c06b5d5fc
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "49428328"
---
# <a name="create-perspectives"></a>创建透视

在本课中，将创建“Internet 销售”透视。 透视定义模型的可查看子集，它提供集中的特定于业务的或特定于应用程序的视点。 当用户使用某个透视连接到模型时，他们只能看到在该透视中作为字段定义的那些模型对象（表、列、度量值、层次结构和 KPI）。 若要了解详细信息，请参阅[透视](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular)。
  
本课中创建的“Internet 销售”透视将排除 DimCustomer 表对象。 创建从视图中排除某对象的透视时，该对象仍然存在于模型中。 但是，它在报告客户端字段列表中不可见。 仍然可以基于排除的对象数据计算透视中包括或未包括的计算列和度量值。  
  
本课的目的是介绍如何创建透视并熟悉表格模型创作工具。 如果之后扩展此模型来包括其他表，可以创建其他透视来定义模型的不同视点，例如“库存”和“销售”。  
  
本课预计完成时间：5 分钟  
  
## <a name="prerequisites"></a>先决条件  
本主题是表格建模教程的一部分，应当按顺序完成。 在执行本课中的任务之前，应当已完成上一课：[第 7 课：创建关键绩效指标](../tutorials/aas-lesson-7-create-key-performance-indicators.md)。  
  
## <a name="create-perspectives"></a>创建透视  
  
#### <a name="to-create-an-internet-sales-perspective"></a>创建“Internet 销售”透视  
  
1.  单击“模型”菜单 >“透视” > “创建和管理”。  
  
2.  在“透视”对话框中，单击“新建透视”。  
  
3.  双击“新建透视”列标题，并重命名“Internet 销售”。  
  
4.  选择除 DimCustomer 之外的所有表。  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    在后面的课程中，将使用“在 Excel 中分析”功能来测试此透视。 Excel 数据透视表字段列表包括除 DimCustomer 表之外的所有表。  

## <a name="whats-next"></a>后续步骤
[第 9 课：创建层次结构](../tutorials/aas-lesson-9-create-hierarchies.md)。
  
  
  
  
