---
title: 使用 Azure DevOps 和 Azure Application Insights 连续监视 DevOps 发布管道 | Microsoft Docs
description: 提供有关快速设置以启用 Application Insights 连续监视的说明
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 11/13/2017
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 41999defb01e024773b6364f169a1ce3b1377237
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "60902275"
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>向发布管道添加连续监视

Azure DevOps Services 与 Azure Application Insights 集成，可连续监视整个软件开发生命周期内的 DevOps 发布管道。 

Azure DevOps Services 现在支持连续监视，因此发布管道可以结合来自 Application Insights 和其他 Azure 资源的监视数据。 检测到 Application Insights 警报时，部署可以保持封闭或回退，直到警报解除。 如果所有检查都通过，可以从测试到生产全程自动执行部署，无需手动干预。 

## <a name="configure-continuous-monitoring"></a>配置连续监视

1. 选择一个现有的 Azure DevOps Services 项目。

2. 将鼠标悬停在“生成和发布”上 > 选择“版本”，依次单击加号 > “创建发布定义”>搜索“监视” >  使用连续监视的 Azure App Service 部署       。

   ![新建 Azure DevOps Services 发布管道](media/continuous-monitoring/001.png)

3. 单击“应用”  。

4. 在红色感叹号旁边，选择蓝色文本以“查看环境任务”  。

   ![查看环境任务](media/continuous-monitoring/002.png)

   将显示配置对话框，请使用下表填写输入字段。

    | 参数        | 值 |
   | ------------- |:-----|
   | **环境名称**      | 描述发布管道环境的名称 |
   | **Azure 订阅** | 下拉列表中填充的内容是链接到 Azure DevOps Services 组织的所有 Azure 订阅|
   | **应用服务名称** | 此字段可能需要手动输入新值，具体取决于其他选择 |
   | **资源组**    | 下拉列表的填充内容是可用资源组 |
   | **Application Insights 资源名称** | 下拉列表填充的内容是与之前选定资源组对应的所有 Application Insights 资源。

5. 选择“配置 Application Insights 警报” 

6. 对于默认警报规则，选择“保存”>“输入描述性注释”>单击“确定”  

## <a name="modify-alert-rules"></a>修改警报规则

1. 若要修改预定义的警报设置，单击“警报规则”右侧的省略号“...”框   。

   （将显示四个现有的警报规则：可用性、失败请求、服务器响应时间、服务器异常。）

2. 单击“可用性”旁边的下拉列表符号  。

3. 修改可用性“阈值”以满足服务级别要求  。

   ![修改警报](media/continuous-monitoring/003.png)

4. 选择“确定” > “保存”>输入描述性注释>单击“确定”    。

## <a name="add-deployment-conditions"></a>添加部署条件

1. 单击“管道”>选择“预部署条件”或“部署后条件”符号，具体取决于需要持续监视入口的阶段    。

   ![预部署条件](media/continuous-monitoring/004.png)

2. 将“入口”设置为“启用” > “批准入口”>单击“添加”     。

3. 选择“Azure Monitor”（此选项使你能够从 Azure Monitor 和 Application Insights 访问警报） 

    ![Azure Monitor](media/continuous-monitoring/005.png)

4. 输入“入口超时”值  。

5. 输入“采样间隔”  。

## <a name="deployment-gate-status-logs"></a>部署入口状态日志

添加部署入口后，超出之前定义阈值的 Application Insights 警报可防止部署进行不必要的版本升级。 警报解除后，部署会自动继续执行。

若要了解此行为，请选择“版本”>右键单击“版本名称” > 打开“日志”    。

![日志](media/continuous-monitoring/006.png)

## <a name="next-steps"></a>后续步骤

若要详细了解 Azure Pipelines，请尝试使用这些[快速入门](https://docs.microsoft.com/azure/devops/pipelines)。
