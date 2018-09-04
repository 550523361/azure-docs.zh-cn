---
title: 使用 Java 将事件发送到 Azure 事件中心 | Microsoft 文档
description: 使用 Java 发送到事件中心入门
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 08/27/2018
ms.author: shvija
ms.openlocfilehash: f67982eda60a8fdfdf0d50785827c513275fd202
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124749"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>使用 Java 将事件发送到 Azure 事件中心

事件中心是一个具备高度伸缩性的引入系统，每秒可收入大量事件，从而使应用程序能够处理和分析连接的设备和应用程序所产生的海量数据。 将数据采集到事件中心后，可以使用任何实时分析提供程序或存储群集来转换和存储数据。

有关详细信息，请参阅[事件中心概述][Event Hubs overview]。

本教程演示如何使用用 Java 编写的控制台应用程序将事件发送到事件中心。 若要使用 Java 事件处理器主机库接收事件，请参阅[本文](event-hubs-java-get-started-receive-eph.md)，或单击左侧目录中的相应接收语言。

## <a name="prerequisites"></a>先决条件

若要完成本教程，需要具备以下先决条件：

* Java 开发环境。 本教程使用 [Eclipse](https://www.eclipse.org/)。
* 有效的 Azure 帐户。 如果还没有 Azure 订阅，可以在开始前创建一个[免费帐户][]。

本教程中的代码基于 [SimpleSend GitHub 示例](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend)，可检查该代码以查看完整的工作应用程序。

## <a name="send-events-to-event-hubs"></a>将事件发送到事件中心

事件中心的 Java 客户端库可用于[Maven 中央存储库](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)中的 Marven 项目。 可在 Maven 项目文件中使用以下依赖项声明引用此库。 当前版本为 1.0.2：    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.2</version>
</dependency>
```

对于不同类型的生成环境，可以从 [Maven 中央存储库](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)显式获取最新发布的 JAR 文件。  

对于简单的事件发布服务器，请导入事件中心客户端类的 *com.microsoft.azure.eventhubs* 包和实用程序类（如与 Azure 服务总线消息传递客户端共享的常见异常）的 *com.microsoft.azure.servicebus* 包。 

### <a name="declare-the-send-class"></a>声明“发送”类

对于以下示例，请首先在你最喜欢的 Java 开发环境中为控制台/shell 应用程序创建一个新的 Maven 项目。 将类 `SimpleSend` 命名为：     

```java
package com.microsoft.azure.eventhubs.samples.send;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

public class SimpleSend {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
            
            
    }
 }
```

### <a name="construct-connection-string"></a>构造连接字符串

使用 ConnectionStringBuilder 类构造要传递到事件中心客户端实例的连接字符串值。 将占位符替换为创建命名空间和事件中心时获取的值：

```java
final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
        .setNamespaceName("Your Event Hubs namespace name")
        .setEventHubName("Your event hub")
        .setSasKeyName("Your policy name")
        .setSasKey("Your primary SAS key");
```

### <a name="send-events"></a>发送事件

通过将字符串转换为其 UTF-8 字节编码创建单一事件。 然后，使用连接字符串创建一个新的事件中心客户端实例并发送该消息：   

```java 
String payload = "Message " + Integer.toString(i);
byte[] payloadBytes = gson.toJson(payload).getBytes(Charset.defaultCharset());
EventData sendEvent = EventData.create(payloadBytes);

final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);
ehClient.sendSync(sendEvent);
    
// close the client at the end of your program
ehClient.closeSync();

``` 

### <a name="how-messages-are-routed-to-eventhub-partitions"></a>如何将消息路由到 EventHub 分区

在使用者检索消息之前，必须先由发布者将消息发布到分区。 当使用 com.microsoft.azure.eventhubs.EventHubClient 对象上的 sendSync() 方法同步将消息发布到事件中心时，可以将消息发送到特定分区或以循环方式分发到所有可用分区，具体取决于 是否指定了分区键。

指定了表示分区键的字符串时，将对该键进行哈希处理以确定将事件发送到哪个分区。

如果未设置分区键，则消息将循环分发到所有可用分区

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

// close the client at the end of your program
eventHubClient.closeSync();

```

## <a name="next-steps"></a>后续步骤

访问以下链接可以了解有关事件中心的详细信息：

* [使用 EventProcessorHost 接收事件](event-hubs-java-get-started-receive-eph.md)
* [事件中心概述][Event Hubs overview]
* [创建事件中心](event-hubs-create.md)
* [事件中心常见问题解答](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[免费帐户]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

