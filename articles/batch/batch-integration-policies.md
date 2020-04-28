---
title: 与 Azure 策略的集成
description: Azure 策略是 Azure 中的一项服务，可用于创建、分配和管理对资源强制执行规则的策略。
ms.topic: article
ms.date: 02/24/2020
ms.custom: seodec18
ms.openlocfilehash: a160de1277afea026a16f470c8f76cdc2ec1733f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82184259"
---
# <a name="integration-with-azure-policy"></a>与 Azure 策略的集成

Azure 策略是 Azure 中的一项服务，可用于创建、分配和管理策略，这些策略对资源强制执行规则，以确保这些资源符合公司标准和服务级别协议。 Azure 策略会评估你的资源是否与分配的策略不相容。 

Azure Batch 提供了两个内置扩展，可帮助你管理策略符合性。 

|**名称**.。。|   **说明**|**效果**|  **Version**|    **源**
|----------------|----------|----------|----------------|---------------|
|应启用 Batch 帐户中的诊断日志|   审核是否已启用诊断日志。 这样便可以在发生安全事件或网络受到威胁时重新创建活动线索以用于调查目的|AuditIfNotExists、Disabled|  2.0.0|  GitHub|
|应在批处理帐户上配置指标警报规则| 审核是否已针对 Batch 帐户配置指标警报规则，以启用所需指标|   AuditIfNotExists、Disabled| 1.0.0|  GitHub|

策略定义介绍了需要满足的条件。 条件将资源属性与所需的值进行比较。 使用预定义的别名访问资源属性字段。您可以使用属性别名来访问资源类型的特定属性。 通过别名，可限制允许用于资源属性的值和条件。 每个别名会映射到给定资源类型不同 API 版本的路径。 在策略评估期间，策略引擎会获取该 API 版本的属性路径。

Batch 所需的资源包括：帐户、计算节点、池、作业和任务。 因此，您将使用属性别名来访问这些资源的特定属性。 了解有关[别名](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)的详细信息

若要确保了解当前别名并查看资源和策略，请使用适用于 Visual Studio Code 的 Azure 策略扩展。 它可以安装在 Visual Studio Code 支持的所有平台上。 支持的平台包括 Windows、Linux 和 macOS。 请参阅[安装指南](https://docs.microsoft.com/azure/governance/policy/how-to/extension-for-vscode)。



