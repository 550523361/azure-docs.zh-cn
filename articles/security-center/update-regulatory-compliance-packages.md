---
title: 如何在 Azure 安全中心规章符合性仪表板中更新为动态规章相容性监视 |Microsoft Docs
description: 更新法规符合性包
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: fa5027ed285456247891c84e559b74a14237f553
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "81537774"
---
# <a name="update-to-dynamic-compliance-packages-in-your-regulatory-compliance-dashboard"></a>在规章相容性仪表板中更新为动态符合性包

Azure 安全中心会不断地将资源的配置与行业标准、法规和基准中的要求进行比较。 **规章相容性仪表板**根据你如何满足特定的符合性控制和要求，提供对符合性状态的见解。

可以跟踪符合性状况的一个标准是[AZURE CIS 1.1.0](https://www.cisecurity.org/benchmark/azure/) （更正式地说，"CIS Microsoft Azure 地基基准版本 1.1.0"）。 

最初出现在符合性仪表板中的 Azure CIS 表示形式依赖于安全中心随附的一组静态规则。

利用**动态符合性包**功能，安全中心会在一段时间内自动提高行业标准的覆盖范围。 符合性包实质上是在 Azure 策略中定义的。 可以将其分配给所选的作用域（订阅、管理组等）。 若要查看在仪表板中映射为评估的符合性数据，请从安全策略中将符合性包添加到管理组或订阅中。 添加符合性包会有效地将法规遵从性计划分配给所选的作用域。 通过这种方式，你可以在仪表板中跟踪新发布的法规方案，作为合规性标准。 当 Microsoft 发布新的计划内容（映射到标准中的更多控件的新策略）时，其他内容会自动显示在仪表板中。

Azure cis 基准的动态符合性包： **AZURE cis 1.1.0 （新）** 通过以下方式改进原始*静态*版本：

* 包括更多策略
* 添加新覆盖率时自动更新 

更新到如下所述的新动态包。

## <a name="adding-a-dynamic-compliance-package"></a>添加动态符合性包

以下步骤说明如何添加动态包，以监视与 Azure CIS 基准1.1.0 的符合性。   

### <a name="update-to-the-azure-cis-110-new-dynamic-compliance-package"></a>更新到 Azure CIS 1.1.0 （新）动态符合性包 

1. 打开 "**安全策略**" 页。 此页显示管理组、订阅、工作区和你的管理组结构的数量。

1. 选择要为其管理法规符合性状况的订阅或管理组。 建议选择标准适用的最高作用域，以便为所有嵌套资源聚合和跟踪符合性数据。 

1. 在行业 & 规章标准部分中，你将看到 Azure CIS 1.1.0 可以针对新内容进行更新。 单击 "**立即更新**"。 

1. （可选）单击 "**添加更多标准**" 以打开 "**添加合规性标准**" 页。 在这里，你可以针对其他符合性标准（如**NIST SP 800-53 R4**、 **SWIFT CSP CSCF-v2020**、 **UKO 和英国 NHS**以及**加拿大 PBMM**）手动搜索**Azure CIS 1.1.0 （新）** 和动态包。
    
    > [!TIP]
    > 只有作为所有者或策略参与者的用户具有添加符合性标准所必需的权限。 

    ![将规章包添加到 Azure 安全中心的符合性仪表板](./media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-additional-standards.png)


1. 在安全中心的边栏中，选择 "**符合**性"，以打开 "合规性" 仪表板。 
    * Azure CIS 1.1.0 （新）现在显示在行业 & 规章标准的列表中。 
    * Azure CIS 1.1.0 符合性的原始*静态*视图也将保留在它的旁边。 将来可能会自动删除它。

    > [!NOTE]
    > 新添加的标准可能需要几个小时才能显示在 "符合性" 仪表板中。


    [![显示旧的和新的 Azure CIS 的规章相容性仪表板](media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-cis-old-and-new.png)](media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-cis-old-and-new.png#lightbox)


## <a name="next-steps"></a>后续步骤

在本文中，我们已了解到：

* 如何将你的规章相容性仪表板中显示的**标准升级**到新的*动态*包
* 如何**添加符合性包**来监视与其他标准的符合性。 

有关其他相关材料，请参阅以下文章： 

- [安全中心规章相容性仪表板](security-center-compliance-dashboard.md)
- [使用安全策略](tutorial-security-policy.md)
- [管理 Azure 安全中心安全建议](security-center-recommendations.md) - 了解如何使用 Azure 安全中心的建议来保护 Azure 资源。