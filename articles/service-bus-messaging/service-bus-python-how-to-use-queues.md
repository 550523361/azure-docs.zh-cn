---
title: 教程：通过 Python 使用 Azure 服务总线队列
description: 了解如何使用 Python 中的 Azure 服务总线队列
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 04/10/2019
ms.author: aschhab
ms.custom: seo-python-october2019
ms.openlocfilehash: 69ae02ea7c0c04312dd4e64125c80384172c6528
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2019
ms.locfileid: "72438111"
---
# <a name="tutorial-use-azure-service-bus-queues-with-python"></a>教程：通过 Python 使用 Azure 服务总线队列

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

本教程介绍如何创建 Python 应用程序，以便将消息发送到服务总线队列并从服务总线队列接收消息。 

## <a name="prerequisites"></a>必备组件
1. Azure 订阅。 要完成本教程，需要一个 Azure 帐户。 可以激活[MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF)或注册[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)。
2. 按照[使用 Azure 门户创建服务总线队列一](service-bus-quickstart-portal.md)文中的步骤进行操作。
    1. 阅读服务总线**队列**的快速**概述**。 
    2. 创建一个服务总线**命名空间**。 
    3. 获取**连接字符串**。 

        > [!NOTE]
        > 你将在本教程中使用 Python 在 Service Bus 命名空间中创建**队列**。 
1. 安装 Python 或[Python Azure 服务总线包][Python Azure Service Bus package]，请参阅[python 安装指南](/azure/python/python-sdk-azure-install)。 请参阅[此处](/python/api/overview/azure/servicebus?view=azure-python)的服务总线 Python SDK 的完整文档。

## <a name="create-a-queue"></a>创建队列
通过**ServiceBusClient**对象，您可以使用队列。 将以下代码添加到任何 Python 文件的顶部附近，你希望在其中以编程方式访问服务总线：

```python
from azure.servicebus import ServiceBusClient
```

下面的代码创建一个**ServiceBusClient**对象。 将 `<CONNECTION STRING>` 替换为您的。

```python
sb_client = ServiceBusClient.from_connection_string('<CONNECTION STRING>')
```

SAS 密钥名称和值的值可以在[Azure 门户][Azure portal]连接信息中找到，也可以在服务器资源管理器中选择服务总线命名空间时在 Visual Studio 的 "**属性**" 窗格中找到（如前一部分中所示）。

```python
sb_client.create_queue("taskqueue")
```

`create_queue` 方法还支持其他选项，通过这些选项可以重写默认队列设置，例如消息生存时间 (TTL) 或最大队列大小。 以下示例将最大队列大小设置为 5GB，将 TTL 值设置为 1 分钟：

```python
sb_client.create_queue("taskqueue", max_size_in_megabytes=5120,
                       default_message_time_to_live=datetime.timedelta(minutes=1))
```

有关详细信息，请参阅[Azure 服务总线 Python 文档](/python/api/overview/azure/servicebus?view=azure-python)。

## <a name="send-messages-to-a-queue"></a>向队列发送消息
若要将消息发送到服务总线队列，应用程序需要对 @no__t 对象调用 @no__t 的方法。

以下示例演示如何使用 `send_queue_message` 向名为 `taskqueue` 的队列发送一条测试消息：

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient
queue_client = QueueClient.from_connection_string(
    "<CONNECTION STRING>", "<QUEUE NAME>")

# Send a test message to the queue
msg = Message(b'Test Message')
queue_client.send(msg)
```

服务总线队列在[标准层](service-bus-premium-messaging.md)中支持的最大消息大小为 256 KB，在[高级层](service-bus-premium-messaging.md)中则为 1 MB。 标头最大大小为 64 KB，其中包括标准和自定义应用程序属性。 一个队列可包含的消息数不受限制，但消息的总大小受限。 此队列大小是在创建时定义的，上限为 5 GB。 有关配额的详细信息，请参阅[服务总线配额][Service Bus quotas]。

有关详细信息，请参阅[Azure 服务总线 Python 文档](/python/api/overview/azure/servicebus?view=azure-python)。

## <a name="receive-messages-from-a-queue"></a>从队列接收消息
使用 `ServiceBusService` 对象上的 `get_receiver` 方法从队列接收消息：

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient
queue_client = QueueClient.from_connection_string(
    "<CONNECTION STRING>", "<QUEUE NAME>")

# Receive the message from the queue
with queue_client.get_receiver() as queue_receiver:
    messages = queue_receiver.fetch_next(timeout=3)
    for message in messages:
        print(message)
        message.complete()
```

有关详细信息，请参阅[Azure 服务总线 Python 文档](/python/api/overview/azure/servicebus?view=azure-python)。


`peek_lock` 参数设置为“False”时，会在读取消息后将其从队列中删除。 通过将参数 `peek_lock` 设置为“True”，可读取（速览）并锁定消息而不会将其从队列中删除。

在接收过程中读取并删除消息的行为是最简单的模式，并且最适合在发生故障时应用程序可以容忍不处理消息的情况。 为了理解这一点，可以考虑这样一种情形：使用方发出接收请求，但在处理该请求前发生了崩溃。 由于服务总线会将消息标记为“将使用”，因此当应用程序重启并重新开始使用消息时，它会丢失在发生崩溃前使用的消息。

如果将 `peek_lock` 参数设置为“True”，则接收会变成一个两阶段操作，从而可支持无法容忍遗漏消息的应用程序。 当服务总线收到请求时，它会查找下一条要使用的消息，锁定该消息以防其他使用者接收，然后将该消息返回到应用程序。 在应用程序处理完消息（或安全存储该消息以供将来处理）后，它会通过对 **Message** 对象调用 **delete** 方法来完成接收过程的第二个阶段。 **delete** 方法会将消息标记为已使用，并从队列中将其删除。

```python
msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>如何处理应用程序崩溃和不可读消息
服务总线提供了相关功能，帮助你轻松地从应用程序错误或消息处理问题中恢复。 如果接收方应用程序因某种原因无法处理消息，则它可以对 **Message** 对象调用 **unlock** 方法。 这会导致 Service Bus 解锁队列中的消息并使其能够重新被同一个正在使用的应用程序或其他正在使用的应用程序接收。

还存在与队列中已锁定消息关联的超时，并且如果应用程序无法在锁定超时到期之前处理消息（例如，如果应用程序崩溃），服务总线会自动解锁该消息并使它可再次被接收。

如果应用程序在处理消息之后，但在调用 **delete** 方法之前崩溃，则在应用程序重新启动时会将该消息重新传送给它。 此情况通常称作**至少处理一次**，即每条消息将至少被处理一次，但在某些情况下，同一消息可能会被重新传送。 如果某个场景不允许重复处理，则应用程序开发人员应在其应用程序中添加额外逻辑来处理重复消息传送。 这通常可以通过使用消息的 **MessageId** 属性来实现，该属性在多次传送尝试中保持不变。

> [!NOTE]
> 可以使用[服务总线资源管理器](https://github.com/paolosalvatori/ServiceBusExplorer/)管理服务总线资源。 服务总线资源管理器允许用户连接到服务总线命名空间并以一种简单的方式管理消息传送实体。 该工具提供高级功能，如导入/导出功能或用于对主题、队列、订阅、中继服务、通知中心和事件中心进行测试的功能。 

## <a name="next-steps"></a>后续步骤
现在，已了解有关服务总线队列的基础知识，请参阅下面的文章了解更多信息。

* [队列、主题和订阅][Queues, topics, and subscriptions]

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

