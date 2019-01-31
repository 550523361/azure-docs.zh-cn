---
title: 监视 Azure IoT 中心的运行状况 | Microsoft Docs
description: 使用 Azure Monitor 和 Azure 资源运行状况监视 IoT 中心并快速诊断问题
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: kgremban
ms.openlocfilehash: 3b56097f8805b4c6d95256ae1753daf5ded266fb
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2019
ms.locfileid: "54888390"
---
# <a name="monitor-the-health-of-azure-iot-hub-and-diagnose-problems-quickly"></a>监视 Azure IoT 中心的运行状况并快速诊断问题

实施 Azure IoT 中心的企业期望其资源具有可靠的性能。 为了帮助你密切监视自己的操作，IoT 中心与 [Azure Monitor][lnk-AM] 和 [Azure 资源运行状况][lnk-ARH]完全集成。 这两个服务相辅相成，提供所需的数据来让 IoT 解决方案保持正常的运行。 

Azure Monitor 是监视所有 Azure 服务并记录其日志的单一源。 可将 Azure Monitor 生成的诊断日志发送到 Log Analytics、事件中心或 Azure 存储进行自定义处理。 借助 Azure Monitor 的指标和诊断设置，可以洞察资源的性能。 请继续阅读本文，了解如何对 IoT 中心[使用 Azure Monitor](#use-azure-monitor)。 

> [!IMPORTANT]
> IoT 中心服务使用 Azure Monitor 诊断日志发出的事件不保证可靠或有序。 某些事件可能会丢失或未按顺序传送。 诊断日志也不是实时的，可能需要几分钟的时间才能将事件记录到所选的目标。

Azure 资源运行状况可以帮助你在 Azure 问题影响资源时进行诊断和获取支持。 个性化的仪表板提供 IoT 中心的当前和过去运行状态。 请继续阅读本文，了解如何对 IoT 中心[使用 Azure 资源运行状况](#use-azure-resource-health)。 

IoT 中心还提供了其自己的指标，可使用这些指标了解 IoT 资源的状态。 有关详细信息，请参阅[了解 IoT 中心指标][lnk-metrics]。

## <a name="use-azure-monitor"></a>使用 Azure Monitor

Azure Monitor 提供 Azure 资源的诊断信息，这意味着，你可以监视在 IoT 中心内部发生的操作。 

Azure Monitor 的诊断设置会取代 IoT 中心操作监视功能。 如果当前正在使用操作监视，应迁移工作流。 有关详细信息，请参阅[从操作监视迁移到诊断设置][lnk-migrate]。

若要详细了解 Azure Monitor 监视的具体指标和事件，请参阅 [Azure Monitor 支持的指标][lnk-AM-metrics]和 [Azure 诊断日志支持的服务、架构和类别][lnk-AM-schemas]。

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="understand-the-logs"></a>了解日志

Azure Monitor 跟踪 IoT 中心内发生的不同操作。 每个类别都有一个定义如何报告该类别中的事件的架构。 

#### <a name="connections"></a>连接

连接类别跟踪设备连接，并断开事件与 IoT 中心和错误的连接。 此类别用于识别未经授权的连接尝试或者在失去与设备的连接时发出警报。

> [!NOTE]
> 若要获得设备的可靠连接状态，请检查[设备检测信号][lnk-devguide-heartbeat]。


```json
{
    "records": 
    [
        {
            "time": " UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "deviceConnect",
            "category": "Connections",
            "level": "Information",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"maskedIpAddress\":\"<maskedIpAddress>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="cloud-to-device-commands"></a>云到设备的命令

云到设备的命令类别跟踪在 IoT 中心发生且与云到设备的消息管道相关的错误。 此类别包括在以下情况下发生的错误：

* 发送云到设备消息时（例如“未经授权的发件人”错误），
* 接收云到设备消息时（例如“超出传递计数”错误），以及
* 接收云到设备消息反馈时（例如“反馈已过期”错误）。 

此类别不捕捉当云到设备消息已成功传递但设备未正确进行处理时出现的错误。

```json
{
    "records": 
    [
        {
            "time": " UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "messageExpired",
            "category": "C2DCommands",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"deviceId\":\"<deviceId>\",\"messageId\":\"<messageId>\",\"messageSizeInBytes\":\"<messageSize>\",\"protocol\":\"Amqp\",\"deliveryAcknowledgement\":\"<None, NegativeOnly, PositiveOnly, Full>\",\"deliveryCount\":\"0\",\"expiryTime\":\"<timestamp>\",\"timeInSystem\":\"<timeInSystem>\",\"ttl\":<ttl>, \"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\", \"maskedIpAddresss\": \"<maskedIpAddress>\", \"statusCode\": \"4XX\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-identity-operations"></a>设备标识操作

设备标识操作类别跟踪你尝试在 IoT 中心的标识注册表中创建、更新或删除条目时所发生的错误。 预配方案就很适合跟踪此类别。

```json
{
    "records": 
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "get",
            "category": "DeviceIdentityOperations",
            "level": "Error",    
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"maskedIpAddress\":\"<maskedIpAddress>\",\"deviceId\":\"<deviceId>\", \"statusCode\":\"4XX\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="routes"></a>路由

消息路由类别跟踪消息路由评估期间发生的错误以及 IoT 中心感知到的终结点运行状况。 此类别包括诸如下列项的事件：

* 规则评估结果为“未定义”，
* IoT 中心将某个终结点标记为死终结点，或者
* 从终结点收到的任何错误。 

此类别不包含有关消息本身的特定错误（例如设备限制错误），这些错误在“设备遥测”类别下报告。

```json
{
    "records": 
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "endpointUnhealthy",
            "category": "Routes",
            "level": "Error",
            "properties": "{\"deviceId\": \"<deviceId>\",\"endpointName\":\"<endpointName>\",\"messageId\":<messageId>,\"details\":\"<errorDetails>\",\"routeName\": \"<routeName>\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-telemetry"></a>设备遥测

设备遥测类别跟踪在 IoT 中心发生的、与遥测管道相关的错误。 此类别包括发送遥测事件（例如限制）和接收遥测事件（例如未经授权的读取者）时发生的错误。 此类别无法捕捉设备本身运行的代码所造成的错误。

```json
{
    "records": 
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "ingress",
            "category": "DeviceTelemetry",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "properties": "{\"deviceId\":\"<deviceId>\",\"batching\":\"0\",\"messageSizeInBytes\":\"<messageSizeInBytes>\",\"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\",\"partitionId\":\"1\"}", 
            "location": "Resource location"
        }
    ]
}
```

#### <a name="file-upload-operations"></a>文件上传操作

文件上传类别跟踪在 IoT 中心发生的、与文件上传功能相关的错误。 此类别包括：

* SAS URI 发生的错误，例如，它在设备向中心通知某个完成的上传前失效。
* 由设备报告的失败上传。
* 创建 IoT 中心通知消息期间在存储中找不到文件时发生的错误。

此类别不能捕获在设备将文件上传到存储时直接发生的错误。

```json
{
    "records": 
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "ingress",
            "category": "FileUploadOperations",
            "level": "Error",
            "resultType": "Event status",
            "resultDescription": "MessageDescription",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"blobUri\":\"http//bloburi.com\"}",
            "location": "Resource location"
        }
    ]
}
```

#### <a name="cloud-to-device-twin-operations"></a>云到设备孪生操作

云到设备孪生操作类别跟踪设备孪生上服务发起的事件。 这些操作可能获取孪生、更新或替换标记，以及更新或替换所需属性。 

```json
{
    "records": 
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "read",
            "category": "C2DTwinOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\"}", 
            "location": "Resource location"
        }
    ]
}
```

#### <a name="device-to-cloud-twin-operations"></a>设备到云孪生操作

设备到云孪生操作类别跟踪设备孪生上设备发起的事件。 这些操作可能包括获取孪生、更新报告属性和订阅所需属性。

```json
{
    "records": 
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "update",
            "category": "D2CTwinOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authenticationType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\"}", 
            "location": "Resource location"
        }
    ]
}
```

#### <a name="twin-queries"></a>孪生查询

孪生查询类别报告在云中针对设备孪生发起的查询请求。 

```json
{
    "records": 
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "query",
            "category": "TwinQueries",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"query\":\"<twin query>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\",\"pageSize\":\"<pageSize>\", \"continuation\":\"<true, false>\", \"resultSize\":\"<resultSize>\"}", 
            "location": "Resource location"
        }
    ]
}
```

#### <a name="jobs-operations"></a>作业操作

作业操作类别报告在多个设备上更新设备孪生或调用直接方法的作业请求。 这些请求在云中发起。 

```json
{
    "records": 
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "jobCompleted",
            "category": "JobsOperations",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"jobId\":\"<jobId>\", \"sdkVersion\": \"<sdkVersion>\",\"messageSize\": <messageSize>,\"filter\":\"DeviceId IN ['1414ded9-b445-414d-89b9-e48e8c6285d5']\",\"startTimeUtc\":\"Wednesday, September 13, 2017\",\"duration\":\"0\"}", 
            "location": "Resource location"
        }
    ]
}
```

#### <a name="direct-methods"></a>直接方法

直接方法类别跟踪发送到单个设备的请求-响应交互。 这些请求在云中发起。 

```json
{
    "records": 
    [
        {
            "time": "UTC timestamp",
            "resourceId": "Resource Id",
            "operationName": "send",
            "category": "DirectMethods",
            "level": "Information",
            "durationMs": "1",
            "properties": "{\"deviceId\":\"<deviceId>\", \"RequestSize\": 1, \"ResponseSize\": 1, \"sdkVersion\": \"2017-07-11\"}", 
            "location": "Resource location"
        }
    ]
}
```

### <a name="read-logs-from-azure-event-hubs"></a>从 Azure 事件中心读取日志

通过诊断设置设置事件日志记录后，可以创建应用程序用于读出日志，以便可以根据日志中的信息采取措施。 以下示例代码从事件中心检索日志：

```csharp
class Program 
{ 
    static string connectionString = "{your AMS eventhub endpoint connection string}"; 
    static string monitoringEndpointName = "{your AMS event hub endpoint name}"; 
    static EventHubClient eventHubClient; 
//This is the Diagnostic Settings schema 
    class AzureMonitorDiagnosticLog 
    { 
        string time { get; set; } 
        string resourceId { get; set; } 
        string operationName { get; set; } 
        string category { get; set; } 
        string level { get; set; } 
        string resultType { get; set; } 
        string resultDescription { get; set; } 
        string durationMs { get; set; } 
        string callerIpAddress { get; set; } 
        string correlationId { get; set; } 
        string identity { get; set; } 
        string location { get; set; } 
        Dictionary<string, string> properties { get; set; } 
    }; 
    static void Main(string[] args) 
    { 
        Console.WriteLine("Monitoring. Press Enter key to exit.\n"); 
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName); 
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds; 
        CancellationTokenSource cts = new CancellationTokenSource(); 
        var tasks = new List<Task>(); 
        foreach (string partition in d2cPartitions) 
        { 
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token)); 
        } 
        Console.ReadLine(); 
        Console.WriteLine("Exiting..."); 
        cts.Cancel(); 
        Task.WaitAll(tasks.ToArray()); 
    } 
    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct) 
    { 
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow); 
        while (true) 
        { 
            if (ct.IsCancellationRequested) 
            { 
                await eventHubReceiver.CloseAsync(); 
                break; 
            } 
            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10)); 
            if (eventData != null) 
            { 
                string data = Encoding.UTF8.GetString(eventData.GetBytes()); 
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data); 
                var deserializer = new JavaScriptSerializer(); 
                //deserialize json data to azure monitor object 
                AzureMonitorDiagnosticLog message = new JavaScriptSerializer().Deserialize<AzureMonitorDiagnosticLog>(result); 
 
            } 
        } 
    } 
} 
```

## <a name="use-azure-resource-health"></a>使用 Azure 资源运行状况

使用 Azure 资源运行状况可以监视 IoT 中心是否已启动并正在运行。 此外，还可以了解是否发生了影响 IoT 中心运行状况的区域性服务中断。 若要了解有关 Azure IoT 中心运行状态的具体详细信息，我们建议[使用 Azure Monitor](#use-azure-monitor)。 

Azure IoT 中心指示区域级别的运行状况。 如果区域性服务中断影响了你的 IoT 中心，则运行状态显示为“未知”。 若要了解详细信息，请参阅[Azure 资源运行状况中的资源类型和运行状况检查][lnk-ARH-checks]。

若要检查 IoT 中心的运行状况，请遵循以下步骤：

1. 登录到 [Azure 门户](https://portal.azure.com)。
1. 导航到“服务运行状况” > “资源运行状况”。
1. 从下拉列表中选择自己的订阅和“IoT 中心”。

若要详细了解如何解释运行状况数据，请参阅 [Azure 资源运行状况概述][lnk-ARH]

## <a name="next-steps"></a>后续步骤

- [了解 IoT 中心指标][lnk-metrics]
- [通过连接 IoT 中心和邮箱的 Azure 逻辑应用进行 IoT 远程监视并发送通知][lnk-monitoring-notifications]


[lnk-AM]: ../azure-monitor/index.yml
[lnk-ARH]: ../service-health/resource-health-overview.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-migrate]: iot-hub-migrate-to-diagnostics-settings.md
[lnk-AM-metrics]: ../azure-monitor/platform/metrics-supported.md
[lnk-AM-schemas]: ../azure-monitor/platform/diagnostic-logs-schema.md
[lnk-ARH-checks]: ../service-health/resource-health-checks-resource-types.md
[lnk-monitoring-notifications]: iot-hub-monitoring-notifications-with-azure-logic-apps.md
[lnk-devguide-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
