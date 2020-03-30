---
title: Azure 函数的 Azure 队列存储输出绑定
description: 了解如何在 Azure 函数中创建 Azure 队列存储消息。
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2020
ms.author: cshoe
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 76af5f398edd736874fa79095f2e80c02298eac0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277331"
---
# <a name="azure-queue-storage-output-bindings-for-azure-functions"></a>Azure 函数的 Azure 队列存储输出绑定

Azure 函数可以通过设置输出绑定创建新的 Azure 队列存储消息。

有关设置和配置详细信息的信息，请参阅[概述](./functions-bindings-storage-queue.md)。

## <a name="example"></a>示例

# <a name="c"></a>[C#](#tab/csharp)

以下示例演示针对收到的每个 HTTP 请求创建队列消息的[C# 函数](functions-dotnet-class-library.md)。

```csharp
[StorageAccount("MyStorageConnectionAppSetting")]
public static class QueueFunctions
{
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  ILogger log)
    {
        log.LogInformation($"C# function processed: {input.Text}");
        return input.Text;
    }
}
```

# <a name="c-script"></a>[C# 脚本](#tab/csharp-script)

以下示例演示 function.json 文件中的一个 HTTP 触发器绑定以及使用该绑定的 [C# 脚本 (.csx)](functions-reference-csharp.md) 代码**。 该函数针对收到的每个 HTTP 请求创建一个包含 CustomQueueMessage 对象有效负载的队列项****。

下面是*函数.json*文件：

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting"
    }
  ]
}
```

[配置](#configuration)部分解释了这些属性。

下面是可创建一条队列消息的 C# 脚本代码：

```cs
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, ILogger log)
{
    return input;
}
```

可以使用 `ICollector` 或 `IAsyncCollector` 参数一次性发送多条消息。 以下 C# 脚本代码发送多条消息，其中一条消息包含 HTTP 请求数据，另一条消息包含硬编码值：

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItems, 
    ILogger log)
{
    myQueueItems.Add(input);
    myQueueItems.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

以下示例演示 function.json 文件中的一个 HTTP 触发器绑定以及使用该绑定的 [JavaScript 函数](functions-reference-node.md)**。 该函数针对收到的每个 HTTP 请求创建一个队列项。

下面是*函数.json*文件：

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting"
    }
  ]
}
```

[配置](#configuration)部分解释了这些属性。

JavaScript 代码如下所示：

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

可以通过定义 `myQueueItem` 输出绑定的消息数组，一次性发送多条消息。 以下 JavaScript 代码针对收到的每个 HTTP 请求发送两条包含硬编码值的队列消息。

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

下面的示例演示如何将单个值和多个值输出到存储队列。 无论哪种方式，function.json** 所需的配置都是相同的。

存储队列绑定在 function.json** 中定义，其中 type** 设置为 `queue`。

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureStorageQueuesConnectionString"
    }
  ]
}
```

要在队列中设置单个消息，可以将单个值传递给`set`方法。

```python
import azure.functions as func

def main(req: func.HttpRequest, msg: func.Out[str]) -> func.HttpResponse:

    input_msg = req.params.get('message')

    msg.set(input_msg)

    return 'OK'
```

若要在队列中创建多个消息，请将参数声明为适当的列表类型，并将值的数组（与列表类型匹配）传递给 `set` 方法。

```python
import azure.functions as func
import typing

def main(req: func.HttpRequest, msg: func.Out[typing.List[str]]) -> func.HttpResponse:

    msg.set(['one', 'two'])

    return 'OK'
```

# <a name="java"></a>[Java](#tab/java)

 下面的示例显示了一个 Java 函数，该函数在由 HTTP 请求触发时为其创建队列消息。

```java
@FunctionName("httpToQueue")
@QueueOutput(name = "item", queueName = "myqueue-items", connection = "MyStorageConnectionAppSetting")
 public String pushToQueue(
     @HttpTrigger(name = "request", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS)
     final String message,
     @HttpOutput(name = "response") final OutputBinding<String> result) {
       result.setValue(message + " has been added.");
       return message;
 }
```

在 [Java 函数运行时库](/java/api/overview/azure/functions/runtime)中，对其值将写入队列存储的参数使用 `@QueueOutput` 注释。  参数类型应为`OutputBinding<T>`，其中`T`是 POJO 的任何本机 Java 类型。

---

## <a name="attributes-and-annotations"></a>特性和注释

# <a name="c"></a>[C#](#tab/csharp)

在 [C# 类库](functions-dotnet-class-library.md)中，使用 [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues/QueueAttribute.cs)。

该特性将应用到 `out` 参数，或应用到函数的返回值。 该特性的构造函数采用队列的名称，如以下示例中所示：

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  ILogger log)
{
    ...
}
```

可以设置 `Connection` 属性来指定要使用的存储帐户，如以下示例中所示：

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items", Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  ILogger log)
{
    ...
}
```

有关完整示例，请参阅[输出示例](#example)。

可以使用 `StorageAccount` 特性在类、方法或参数级别指定存储帐户。 有关详细信息，请参阅“触发器 - 特性”。

# <a name="c-script"></a>[C# 脚本](#tab/csharp-script)

C# 脚本不支持特性。

# <a name="javascript"></a>[Javascript](#tab/javascript)

JavaScript 不支持特性。

# <a name="python"></a>[Python](#tab/python)

Python 不支持属性。

# <a name="java"></a>[Java](#tab/java)

注释`QueueOutput`允许您将消息写入函数的输出。 下面的示例显示了创建队列消息的 HTTP 触发函数。

```java
package com.function;
import java.util.*;
import com.microsoft.azure.functions.annotation.*;
import com.microsoft.azure.functions.*;

public class HttpTriggerQueueOutput {
    @FunctionName("HttpTriggerQueueOutput")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION) HttpRequestMessage<Optional<String>> request,
            @QueueOutput(name = "message", queueName = "messages", connection = "MyStorageConnectionAppSetting") OutputBinding<String> message,
            final ExecutionContext context) {

        message.setValue(request.getQueryParameters().get("name"));
        return request.createResponseBuilder(HttpStatus.OK).body("Done").build();
    }
}
```

| properties    | 描述 |
|-------------|-----------------------------|
|`name`       | 在函数签名中声明参数名称。 触发函数时，此参数的值具有队列消息的内容。 |
|`queueName`  | 声明存储帐户中的队列名称。 |
|`connection` | 指向存储帐户连接字符串。 |

与`QueueOutput`注释关联的参数类型为[输出绑定\<T\>](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/OutputBinding.java)实例。

---

## <a name="configuration"></a>Configuration

下表介绍了您在*函数.json*文件和`Queue`属性中设置的绑定配置属性。

|function.json 属性 | Attribute 属性 |描述|
|---------|---------|----------------------|
|**type** | 不适用 | 必须设置为 `queue`。 在 Azure 门户中创建触发器时，会自动设置此属性。|
|direction**** | 不适用 | 必须设置为 `out`。 在 Azure 门户中创建触发器时，会自动设置此属性。 |
|**name** | 不适用 | 表示函数代码中的队列的变量的名称。 设置为 `$return` 可引用函数返回值。|
|**queueName** |**队列名称** | 队列的名称。 |
|**连接** | **连接** |包含要用于此绑定的存储连接字符串的应用设置的名称。 如果应用设置名称以“AzureWebJobs”开始，则只能在此处指定该名称的余下部分。 例如，如果将 `connection` 设置为“MyStorage”，Functions 运行时将会查找名为“MyStorage”的应用设置。 如果将 `connection` 留空，函数运行时将使用名为 `AzureWebJobsStorage` 的应用设置中的默认存储连接字符串。|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>使用情况

# <a name="c"></a>[C#](#tab/csharp)

使用 方法参数（如`out T paramName`）编写单个队列消息。 可以使用方法返回类型而不使用 `out` 参数，`T` 可为以下任何类型：

* 可序列化为 JSON 的对象
* `string`
* `byte[]`
* [云队列消息] 

如果尝试绑定到 `CloudQueueMessage` 并收到错误消息，请确保已引用[正确的存储 SDK 版本](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x)。

在 C# 和 C# 脚本中，可使用以下类型之一编写多条队列消息： 

* `ICollector<T>` 或 `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.azure.storage.queue.cloudqueue)

# <a name="c-script"></a>[C# 脚本](#tab/csharp-script)

使用 方法参数（如`out T paramName`）编写单个队列消息。 是`paramName`函数 的属性中`name`指定的值 *。* 可以使用方法返回类型而不使用 `out` 参数，`T` 可为以下任何类型：

* 可序列化为 JSON 的对象
* `string`
* `byte[]`
* [云队列消息] 

如果尝试绑定到 `CloudQueueMessage` 并收到错误消息，请确保已引用[正确的存储 SDK 版本](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x)。

在 C# 和 C# 脚本中，可使用以下类型之一编写多条队列消息： 

* `ICollector<T>` 或 `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.azure.storage.queue.cloudqueue)

# <a name="javascript"></a>[Javascript](#tab/javascript)

输出队列项可通过`context.bindings.<NAME>`与`<NAME>`*函数.json*中定义的名称匹配的位置可用。 可对队列项有效负载使用字符串或 JSON 可序列化对象。

# <a name="python"></a>[Python](#tab/python)

从函数中提供事件中心消息有两个选项：

- **返回值**：将`name`*函数.json*中的`$return`属性设置为 。 使用此配置，函数的返回值将保留为队列存储消息。

- **命令 ：** 将值传递给声明为[Out](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python)类型的参数的[集](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python#set-val--t-----none)方法。 传递给`set`的值将保留为队列存储消息。

# <a name="java"></a>[Java](#tab/java)

使用[队列输出](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.queueoutput)注释从函数中输出事件中心消息有两个选项：

- **返回值**：通过将注释应用于函数本身，函数的返回值将保留为事件中心消息。

- **命令：** 要显式设置消息值，请将注释应用于类型[`OutputBinding<T>`](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.OutputBinding)的特定参数，其中`T`POJO 或任何本机 Java 类型。 使用此配置，将值传递给`setValue`该方法会将该值保留为事件中心消息。

---

## <a name="exceptions-and-return-codes"></a>异常和返回代码

| 绑定 |  参考 |
|---|---|
| 队列 | [队列错误代码](https://docs.microsoft.com/rest/api/storageservices/queue-service-error-codes) |
| Blob、表、队列 | [存储错误代码](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob、表、队列 |  [疑难解答](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>host.json 设置

本部分介绍版本 2.x 及更高版本中可用于此绑定的全局配置设置。 下面的示例 host.json 文件仅包含此绑定的 2.x 版及更高版本设置。 若要详细了解 2.x 版及更高版本中的全局配置设置，请参阅 [Azure Functions 的 host.json 参考](functions-host-json.md)。

> [!NOTE]
> 有关 Functions 1.x 中 host.json 的参考，请参阅 [Azure Functions 1.x 的 host.json 参考](functions-host-json-v1.md)。

```json
{
    "version": "2.0",
    "extensions": {
        "queues": {
            "maxPollingInterval": "00:00:02",
            "visibilityTimeout" : "00:00:30",
            "batchSize": 16,
            "maxDequeueCount": 5,
            "newBatchThreshold": 8
        }
    }
}
```

|properties  |默认 | 描述 |
|---------|---------|---------|
|maxPollingInterval|00:00:01|队列轮询的最大间隔时间。 最小值为 00:00:00.100（100 毫秒），可递增至 00:01:00（1 分钟）。  在 1.x 中，数据类型是毫秒，在 2.x 及更高版本中，数据类型是 TimeSpan。|
|visibilityTimeout|00:00:00|消息处理失败时的重试间隔时间。 |
|batchSize|16|Functions 运行时同时检索并并行处理的队列消息数。 当处理的数量下降到 `newBatchThreshold` 时，运行时可获取另一个批，并开始处理这些消息。 因此，每个函数处理的最大并发消息数是 `batchSize` 加上 `newBatchThreshold`。 此限制分别应用于各个队列触发的函数。 <br><br>如果要避免对队列上收到的消息并行执行，可以将 `batchSize` 设置为 1。 但是，只有在函数于单个虚拟机 (VM) 上运行时，此设置才可消除并发。 如果函数应用横向扩展到多个 VM，每个 VM 可运行每个队列触发的函数的一个实例。<br><br>`batchSize` 的最大值为 32。 |
|maxDequeueCount|5|在将某个消息移到有害队列之前，尝试处理该消息的次数。|
|newBatchThreshold|batchSize/2|只要同时处理的消息数下降到此数值，运行时即检索另一个批次。|

## <a name="next-steps"></a>后续步骤

- [在队列存储数据更改时运行函数（触发器）](./functions-bindings-storage-queue-trigger.md)

<!-- LINKS -->

[云队列消息]: /dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage
