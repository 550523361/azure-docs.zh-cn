---
title: Azure Monitor 工作簿访问控制
description: 使用基于角色的访问控制，通过预生成的自定义参数化工作簿简化复杂报表
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 20116ab105e4eb12875ba3cb279fb261eb5c70e4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "77658415"
---
# <a name="access-control"></a>访问控制

工作簿中的访问控制是指两项内容：

* 读取工作簿中的数据所需的访问权限。 此访问权限由标准[Azure 角色](https://docs.microsoft.com/azure/role-based-access-control/overview)在工作簿中使用的资源上进行控制。 工作簿不指定或配置对这些资源的访问权限。 用户通常使用这些资源上的 "[监视读取](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader)者" 角色来访问这些资源。

* 保存工作簿所需的访问权限

    - 保存私有`("My")`工作簿不需要额外的权限。 所有用户都可以保存私有工作簿，并且只有他们才能看到这些工作簿。
    - 保存共享工作簿需要资源组中的写入权限才能保存工作簿。 这些权限通常由[监视参与者](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor)角色指定，但也可以通过*工作簿参与者*角色进行设置。
    
## <a name="standard-roles-with-workbook-related-privileges"></a>具有工作簿相关权限的标准角色

[监视读取器](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader)包含用于从资源中读取数据的监视工具（包括工作簿）使用的标准/read 特权。

[监视参与者](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor)包含用于`/write`保存项（包括`workbooks/write`保存共享工作簿的权限）的各种监视工具所使用的常规特权。
"工作簿参与者" 向对象添加 "工作簿/写入" 特权以保存共享工作簿。
用户无需特殊的权限即可保存仅可看到的私有工作簿。

对于自定义的基于角色的访问控制：

添加`microsoft.insights/workbooks/write`以保存共享工作簿。 有关更多详细信息，请参阅[工作簿参与者](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor)角色。

## <a name="next-steps"></a>后续步骤

* [开始](workbooks-visualizations.md)了解有关工作簿许多丰富可视化效果选项的详细信息。
