---
title: 业务连续性计划 - QnA Maker
titleSuffix: Azure Cognitive Services
description: 业务连续性计划的主要目标是创建弹性知识库终结点，确保机器人或应用程序使用该终结点时不会发生停机。
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: 2100426923418104c31933bf9d1406a6441d3841
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410914"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>为 QnA Maker 服务创建业务连续性计划

业务连续性计划的主要目标是创建弹性知识库终结点，确保机器人或应用程序使用该终结点时不会发生停机。

![QnA Maker bcp 计划](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

上述高级想法如下：

1. 在 [Azure 配对区域](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)中设置两个并行 [QnA Maker 服务](../How-To/set-up-qnamaker-service-azure.md)。

2. 使主 Azure 和辅助 Azure 搜索索引保持同步。使用[此处](https://github.com/pchoudhari/QnAMakerBackupRestore)的 GitHub 示例了解如何备份还原 Azure 索引。

3. 使用[连续导出](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry)备份 Application Insights。

4. 主要和辅助堆栈设置完成后，使用[流量管理器](https://docs.microsoft.com/azure/traffic-manager/)配置两个终结点并设置路由方法。

5. 您需要为流量管理器终结点创建传输层安全 （TLS），以前称为安全套接字层 （SSL）。 在应用服务中[绑定 TLS/SSL 证书](https://docs.microsoft.com/azure/app-service/configure-ssl-bindings)。

6. 最后，在机器人或应用中使用流量管理器终结点。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [选择冠状](./improve-knowledge-base.md)
