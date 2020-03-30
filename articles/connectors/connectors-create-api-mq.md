---
title: 连接到 IBM MQ 服务器
description: 使用 Azure 或本地 IBM MQ 服务器和 Azure 逻辑应用发送和检索消息
services: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: valthom, logicappspm
ms.topic: article
ms.date: 06/19/2019
tags: connectors
ms.openlocfilehash: 6bfd626c1ce69029ee720d24b0b143e7b4c3dd56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650941"
---
# <a name="connect-to-an-ibm-mq-server-from-azure-logic-apps"></a>从 Azure 逻辑应用连接到 IBM MQ 服务器

IBM MQ 连接器会发送和检索存储在 IBM MQ 服务器本地或 Azure 中的消息。 此连接器包括要在 TCP/IP 网络上与远程 IBM MQ 服务器计算机通信的 Microsoft MQ 客户端。 本文是使用 MQ 连接器的初学者指南。 可以首先浏览队列中的单个消息，然后尝试其他操作。

IBM MQ 连接器包含这些操作，但不提供触发器：

- 浏览单个消息，而不从 IBM MQ 服务器中删除该消息
- 浏览一批消息，而不从 IBM MQ 服务器中删除这些消息
- 接收单个消息，并从 IBM MQ 服务器中删除该消息
- 接收一批消息，并从 IBM MQ 服务器中删除这些消息
- 将单个消息发送到 IBM MQ 服务器

## <a name="prerequisites"></a>先决条件

* 如果使用本地 MQ 服务器，则在网络中的服务器上[安装本地数据网关](../logic-apps/logic-apps-gateway-install.md)。 安装本地数据网关的服务器还必须安装 .NET Framework 4.6，才能使 MQ 连接器正常运行。 还必须在 Azure 中创建本地数据网关的资源。 有关详细信息，请参阅[设置网关连接](../logic-apps/logic-apps-gateway-connection.md)。

  但是，如果 MQ 服务器公开可用，或在 Azure 中可用，则不需使用数据网关。

* 官方支持的 IBM WebSphere MQ 版本：

  * MQ 7.5
  * MQ 8.0
  * MQ 9.0

* 要在其中添加 MQ 操作的逻辑应用。 此逻辑应用必须使用与本地数据网关连接相同的位置，并且必须已经有一个用于启动工作流的触发器。 

  MQ 连接器没有任何触发器，因此必须先将触发器添加到逻辑应用。 例如，可以使用定期触发器。 如果不熟悉逻辑应用，请尝试此[快速入门：创建你的第一个逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。 

## <a name="browse-a-single-message"></a>浏览单个消息

1. 在逻辑应用的该触发器下或另一操作下，选择“新建步骤”****。 

1. 在搜索框中，键入"mq"，然后选择此操作：**浏览邮件**

   ![浏览消息](media/connectors-create-api-mq/Browse_message.png)

1. 如果没有现有 MQ 连接，请创建连接：  

   1. 在操作中，选择“通过本地数据网关连接”。****
   
   1. 输入 MQ 服务器的属性。  

      对于“服务器”****，可以输入 MQ 服务器名称，或输入后跟冒号和端口号的 IP 地址。
    
   1. 打开**网关**列表，其中会显示任何以前配置的网关连接。 选择你的网关。
    
   1. 完成后，选择“创建”****。 
   
      你的连接如以下示例所示：

      ![连接属性](media/connectors-create-api-mq/Connection_Properties.png)

1. 设置操作的属性：

   * **队列**：指定与连接不同的队列。

   * **消息 Id**、**关联 Id**、**组 Id**和其他属性：根据不同的 MQ 消息属性浏览消息

   * **包括信息**：指定**True**以在输出中包含其他消息信息。 或者，将它指定为“False”****，这样输出中就不会包含其他消息信息。

   * **超时**：输入值以确定等待消息以空队列到达的时间。 如果未输入任何内容，则检索队列中的第一个消息，并且不花费时间等待消息出现。

     ![浏览消息属性](media/connectors-create-api-mq/Browse_message_Props.png)

1. **保存**更改，然后**运行**逻辑应用。

   ![“保存”和“运行”](media/connectors-create-api-mq/Save_Run.png)

   运行完以后，会显示运行的步骤，你可以查看输出。

1. 若要查看每个步骤的详细信息，请选择绿色复选标记。 若要查看输出数据的详细信息，请选择“显示原始输出”。****

   ![浏览消息输出](media/connectors-create-api-mq/Browse_message_output.png)  

   下面是一些示例性的原始输出：

   ![浏览消息原始输出](media/connectors-create-api-mq/Browse_message_raw_output.png)

1. 如果将“IncludeInfo”**** 设置为 true，则会显示以下输出：

   ![浏览消息包含信息](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>浏览多个消息

“浏览消息”**** 操作包含“BatchSize”**** 选项，用于指示应从队列返回的消息数。  如果“BatchSize”**** 没有输入，则返回所有消息。 返回的输出是消息数组。

1. 添加“浏览消息”**** 操作时，默认情况下会选择以前配置的第一个连接。 若要创建新连接，请选择“更改连接”。**** 也可选择另一连接。

1. 逻辑应用运行完成以后，可以看到“浏览消息”操作的一些示例输出：****

   ![浏览消息输出](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-single-message"></a>接收单个消息

“接收消息”**** 操作具有与“浏览消息”**** 操作相同的输入和输出。 使用“接收消息”**** 时，消息会从队列中删除。

## <a name="receive-multiple-messages"></a>接收多个消息

“接收消息”**** 操作具有与“浏览消息”**** 操作相同的输入和输出。 使用“接收消息”**** 时，消息会从队列中删除。

如果进行浏览或接收时队列中不存在消息，则步骤会失败，同时显示以下输出：  

![MQ 无消息错误](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-message"></a>发送消息

添加“发送消息”**** 操作时，默认情况下会选择以前配置的第一个连接。 若要创建新连接，请选择“更改连接”。**** 也可选择另一连接。

1. 选择有效的消息类型：**数据图**、**回复**或**请求**  

   ![发送消息属性](media/connectors-create-api-mq/Send_Msg_Props.png)

1. 逻辑应用完成运行以后，可以看到“发送消息”操作的一些示例输出：****

   ![发送消息输出](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-reference"></a>连接器参考

有关此连接器的更多技术详细信息，例如连接器的 Swagger 文件所述的触发器、操作和限制，请参阅[连接器的参考页](https://docs.microsoft.com/connectors/mq/)。

> [!NOTE]
> 对于[集成服务环境 （ISE）](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)中的逻辑应用，此连接器的 ISE 标记版本使用[ISE 消息限制](../logic-apps/logic-apps-limits-and-config.md#message-size-limits)。

## <a name="next-steps"></a>后续步骤

* 了解其他[逻辑应用连接器](../connectors/apis-list.md)
