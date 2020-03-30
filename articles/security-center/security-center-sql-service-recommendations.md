---
title: 数据&存储建议 - Azure 安全中心
description: 本文档介绍了 Azure 安全中心提供的建议，这些建议有助于保护数据和 Azure SQL 服务，并遵守安全策略。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2019
ms.author: memildin
ms.openlocfilehash: 74ed55e1d460495bfa8d3d4c00bd37bb7f05260e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75552859"
---
# <a name="protect-azure-data-and-storage-services"></a>保护 Azure 数据和存储服务
在 Azure 安全中心识别出潜在的安全漏洞时，它会创建建议，指导你完成配置所需控件以强化和保护资源的过程。

本文介绍了安全中心资源安全部分**的数据安全页面**。

有关您可能在此页上看到的建议的完整列表，请参阅[数据和存储建议](recommendations-reference.md#recs-datastorage)。


## <a name="view-your-data-security-information"></a>查看数据安全信息

1. 在 **"资源安全卫生**"部分中，单击 **"数据和存储资源**"。

    ![数据&存储资源](./media/security-center-monitoring/click-data.png)

    "**数据安全**"页将打开，并包含数据资源的建议。

    [![数据资源](./media/security-center-monitoring/sql-overview.png)](./media/security-center-monitoring/sql-overview.png#lightbox)

    在此页中，可以：

    * 单击"**概述"** 选项卡列出要修复的所有数据资源建议。 
    * 单击每个选项卡，并按资源类型查看建议。

    > [!NOTE]
    > 有关存储加密的详细信息，请参阅[静态数据的 Azure 存储加密](../storage/common/storage-service-encryption.md)。


## <a name="remediate-a-recommendation-on-a-data-resource"></a>修正对数据资源的建议

1. 从任何资源选项卡中，单击资源。 将打开信息页，列出要修复的建议。

    ![资源信息](./media/security-center-monitoring/sql-recommendations.png)

2. 单击建议。 "建议"页将打开并显示实施建议的 **"修正"步骤**。

   ![补救步骤](./media/security-center-monitoring/remediate1.png)

3. 单击 **"执行操作**"。 将显示资源设置页。

    ![启用建议](./media/security-center-monitoring/remediate2.png)

4. 按照**修复步骤**，然后单击"**保存**"。


## <a name="next-steps"></a>后续步骤

要了解有关适用于其他 Azure 资源类型的建议，请参阅以下主题：

* [Azure 安全中心安全建议的完整参考列表](recommendations-reference.md)
* [在 Azure 安全中心保护计算机和应用程序](security-center-virtual-machine-protection.md)
* [保护 Azure 安全中心中的网络](security-center-network-recommendations.md)