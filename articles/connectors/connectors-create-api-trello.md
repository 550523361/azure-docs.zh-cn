---
title: 从 Azure 逻辑应用连接到 Trello
description: 使用 Azure 逻辑应用自动执行任务和工作流，用以监视和管理你的 Trello 帐户中的列表、仪表板和卡
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: d1426bbe2064067b26f53a8f8e5d4aa842ede8c0
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2020
ms.locfileid: "83829692"
---
# <a name="monitor-and-manage-trello-with-azure-logic-apps"></a>使用 Azure 逻辑应用监视和管理 Trello

使用 Azure 逻辑应用和 Trello 连接器，可以创建用于监视和管理 Trello 列表、卡、仪表板和团队成员等等的自动化任务和工作流，例如：

* 监视何时向仪表板和列表中添加了新卡。 
* 创建、获取和管理仪表板、卡和列表。
* 向卡中添加注释和成员。
* 列出仪表板、仪表板标签、仪表板上的卡、卡注释、卡成员、团队成员，以及你所属的团队。 
* 获取团队。

可以使用触发器从 Trello 帐户获取响应，并使输出可用于其他操作。 可以使用通过 Trello 帐户执行任务的操作。 还可以让其他操作使用来自 Trello 操作的输出。 例如，当向仪表板或列表中添加了新卡时，可以通过 Slack 连接器发送消息。 如果你不熟悉逻辑应用，请查看[什么是 Azure 逻辑应用？](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果没有 Azure 订阅，请[注册一个免费 Azure 帐户](https://azure.microsoft.com/free/)。 

* Trello 帐户和用户凭据

  你的凭据授权逻辑应用创建连接并访问你的 Trello 帐户。

* 有关[如何创建逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)的基本知识

* 要在其中访问 Trello 帐户的逻辑应用。 若要使用 Trello 触发器启动，请[创建空白的逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。 若要使用 Trello 操作，请使用触发器（例如**定期**触发器）启动逻辑应用。

## <a name="connect-to-trello"></a>连接到 Trello

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. 登录到 [Azure 门户](https://portal.azure.com)，在逻辑应用设计器中打开逻辑应用（如果尚未打开）。

1. 对于空白逻辑应用，请在搜索框中输入“trello”作为筛选器。 在触发器列表下，选择所需的触发器。 

   -或-

   对于现有逻辑应用，请在要添加操作的最后一个步骤下，选择“新建步骤”。 
   在搜索框中，输入“trello”作为筛选器。 
   在操作列表下，选择所需的操作。

   若要在步骤之间添加操作，请将鼠标指针移到步骤之间的箭头上。 
   选择出现的加号 ( **+** )，然后选择“添加操作”。

1. 如果提示你登录到 Trello，请授权你的逻辑应用进行访问并登录。

1. 为所选触发器或操作提供所需的详细信息，然后继续生成逻辑应用的工作流。

## <a name="connector-reference"></a>连接器参考

有关触发器、操作和限制（请参阅连接器的 OpenAPI（以前称为 Swagger）说明）的技术详细信息，请查看连接器的[参考页](/connectors/trello/)。

## <a name="get-support"></a>获取支持

* 如有问题，请访问[有关 Azure 逻辑应用的 Microsoft 问答页](https://docs.microsoft.com/answers/topics/azure-logic-apps.html)。
* 若要提交功能建议或对功能建议进行投票，请访问[逻辑应用用户反馈网站](https://aka.ms/logicapps-wish)。

## <a name="next-steps"></a>后续步骤

* 了解其他[逻辑应用连接器](../connectors/apis-list.md)