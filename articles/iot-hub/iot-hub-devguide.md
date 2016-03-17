<properties
 pageTitle="IoT 中心开发人员指南主题 | Microsoft Azure"
 description="Azure IoT 中心开发人员指南，其中介绍了 IoT 中心终结点、安全性、设备标识注册表和消息传送"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.date="02/03/2016"
 wacn.date=""/>

# Azure IoT 中心开发人员指南

Azure IoT 中心是一项完全托管的服务，可在数百万个 IoT 设备和一个应用程序后端之间实现安全可靠的双向通信。

Azure IoT 中心可以实现：

* 使用每个设备的安全凭据和访问控制来保护通信安全。
* 提供可靠的设备到云和云到设备的超大规模消息传送。
* 通过最流行语言和平台的设备库来方便建立设备连接。

本文涵盖以下主题：

- [终结点](#endpoints)。此部分说明每个 IoT 中心针对运行时和管理操作公开的各种终结点。
- [设备标识注册表](#device-identity-registry)。此部分说明各 IoT 中心的设备标识注册表存储的信息，以及访问和修改这些信息的方式。
- [安全性](#security)。此部分说明用于向设备和云组件授予 IoT 中心功能访问权限的安全模型。
- [消息传送](#messaging)。此部分说明 IoT 中心公开的消息传送功能（设备到云和云到设备）。
- [配额和限制](#throttling)。此部分汇总了 IoT 中心的使用配额。

## 终结点<a id="endpoints"></a>

Azure IoT 中心属于多租户服务，向各种执行组件公开功能。下图显示了 IoT 中心公开的各种终结点。

![IoT 中心终结点][img-endpoints]

以下是终结点的说明：

* **资源提供程序**：IoT 中心资源提供程序公开 [Azure Resource Manager][lnk-arm] 界面，可让 Azure 订阅所有者创建 IoT 中心、更新 IoT 中心属性和删除 IoT 中心。IoT 中心属性可管理中心级别的安全策略，相对于设备级别的访问控制（请参阅下面的[访问控制](#accesscontrol)）和云到设备及设备到云消息传送的功能选项。资源提供程序还可让你[导出设备标识](#importexport)。
* **设备标识管理**：每个 IoT 中心公开一组用于管理设备标识的 HTTP REST 终结点（创建、检索、更新和删除）。设备标识用于设备身份验证和访问控制。有关详细信息，请参阅[设备标识注册表](#device-identity-registry)。
* **设备终结点**：对于设备标识注册表中预配的每个设备，IoT 中心公开设备可用来发送和接收消息的一组终结点：
    - *发送设备到云的消息*。使用此终结点发送设备到云的消息。有关详细信息，请参阅[设备到云的消息传送](#d2c)。
    - *接收云到设备的消息*设备使用此终结点来接收目标云到设备的消息。有关详细信息，请参阅[云到设备的消息传送](#c2d)。

    这些终结点是使用 HTTP、[MQTT][lnk-mqtt] 和 [AMQP][lnk-amqp] 协议公开的。请注意，也可以通过端口 443 上的 [WebSockets][lnk-websockets] 来实现 AMQP。
* **服务终结点**：每个 IoT 中心公开应用程序后端可用来与设备通信的一组终结点。目前仅使用 [AMQP][lnk-amqp] 协议公开这些终结点。
    - *接收设备到云的消息*。此终结点与 [Azure 事件中心][lnk-event-hubs]兼容，后端服务可用它来读取由设备发送的所有设备到云的消息。有关详细信息，请参阅[设备到云的消息传送](#d2c)。
    - *发送云到设备的消息及接收传送确认*。这些终结点可让应用程序后端发送可靠的云到设备消息，以及接收对应的传送或过期确认。有关详细信息，请参阅[云到设备的消息传送](#c2d)。

[IoT 中心 API 和 SDK][lnk-apis-sdks] 一文介绍了可用于访问这些终结点的各种方式。

最后请务必注意，所有的 IoT 中心终结点都使用 [TLS][lnk-tls] 协议，且绝不会在未加密/不安全的通道上公开任何终结点。

### 如何从事件中心兼容的终结点读取信息<a id="eventhubcompatible"></a>

当你使用[适用于 .NET 的 Azure 服务总线 SDK](https://www.nuget.org/packages/WindowsAzure.ServiceBus) 或[事件中心 - 事件处理器主机][]时，可以将任何 IoT 中心连接字符串与正确的权限配合使用，然后使用**消息/事件**作为事件中心名称。

使用无法识别 IoT 中心的 SDK（或产品集成）时，必须从 [Azure 门户][]的 IoT 中心配置中检索事件中心兼容终结点和事件中心名称：

1. 在 IoT 中心边栏选项卡中单击“设置”，然后单击“消息”。
2. 在“设备到云的设置”部分中，你可以看到“事件中心兼容的终结点”、“事件中心兼容的名称”和“分区”值。

    ![][img-eventhubcompatible]

> [AZURE.NOTE] SDK 有时需要“主机名”或“命名空间”值。在此情况下，请从“事件中心兼容的终结点”中删除方案。例如，如果事件中心兼容的终结点为 **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**，则**主机名**为 **iothub-ns-myiothub-1234.servicebus.windows.net**，**命名空间** 为 **iothub-ns-myiothub-1234**。

然后，可以使用具有 **ServiceConnect** 权限的任何共享访问安全策略连接到指定的事件中心。

如果需要构建使用以前信息的事件中心连接字符串，请使用以下模式：

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

以下是可以配合 IoT 中心公开的事件中心兼容终结点使用的 SDK 和集成项目列表：

* [Java 事件中心客户端](https://github.com/hdinsight/eventhubs-client)
* [Apache Storm Spout](/documentation/articles/hdinsight-storm-develop-csharp-event-hub-topology)。可以在 GitHub 上查看 [Spout 源代码](https://github.com/apache/storm/tree/master/external/storm-eventhubs)。
* [Apache Spark 集成](/documentation/articles/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming)

## 设备标识注册表

每个 IoT 中心都有一个设备标识注册表，用于在服务中创建各设备的资源（例如包含即时云到设备消息的队列），以及让你访问[访问控制](#accesscontrol)部分中所述的面向设备的终结点。

概括地说，设备标识注册表是支持 REST 的设备标识资源集合。以下部分将详细说明设备标识资源属性，以及支持对注册表中的标识执行的操作。

> [AZURE.NOTE] 有关可用来与设备标识注册表交互的 HTTP 协议和 SDK 的详细信息，请参阅 [IoT 中心 API 和 SDK][lnk-apis-sdks]。

### 设备标识属性 <a id="deviceproperties"></a>

设备识别以包含以下属性的 JSON 文档表示。

| 属性 | 选项 | 说明 |
| -------- | ------- | ----------- |
| deviceId | 必需，更新时只读 | ASCII 7 位字母数字字符 + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`.的区分大小写字符串（最长为 128 个字符）。 |
| generationId | 必需，只读 | 中心生成的区分大小写字符串，最长为 128 个字符。在删除并重新创建设备时用于区分具有相同 **deviceId** 的设备。 |
| etag | 必需，只读 | 一个字符串，根据 [RFC7232][lnk-rfc7232] 表示设备标识的弱 etag。|
| auth | 可选 | 包含身份验证信息和安全材料的复合对象。 |
| auth.symkey | 可选 | 包含主密钥和辅助密钥的复合对象，以 base64 格式存储。 |
| status | 必填 | 可以是 **Enabled** 或 **Disabled**。如果是 **Enabled**，则允许设备连接。如果是 **Disabled**，则此设备无法访问任何面向设备的终结点。 |
| statusReason | 可选 | 128 个字符的字符串，用于存储设备标识状态的原因。允许所有 UTF-8 字符。 |
| statusUpdateTime | 只读 | 上次状态更新的日期和时间。 |
| connectionState | 只读 | **Connected** 或 **Disconnected**，表示设备连接状态的 IoT 中心视图。**重要说明**：此字段只用于开发/调试目的。只有使用 AMQP 或 MQTT 的设备更新连接状态。此外，它基于协议级别的 ping（MQTT ping 或 AMQP ping），并且最多有 5 分钟的延迟。出于这些原因，可能会发生误报，例如，将设备报告为已连接，但实际上已断开连接。 |
| connectionStateUpdatedTime | 只读 | 上次更新连接状态的日期和时间。 |
| lastActivityTime | 只读 | 设备上次连接、接收或发送消息的日期和时间。 |

> [AZURE.NOTE] 连接状态只能表示连接状态的 IoT 中心视图。根据网络状态和配置，可能会延迟此状态的更新。

### 设备标识操作

IoT 中心设备标识注册表公开以下操作：

* 创建设备标识
* 更新设备标识
* 按 ID 检索设备标识
* 删除设备标识
* 最多列出 1000 个标识
* 将所有标识导出到 Blob 存储
* 从 Blob 存储导入标识

上述所有操作允许使用 [RFC7232][lnk-rfc7232] 中指定的乐观并发。

> [AZURE.IMPORTANT] 如果要检索中心标识注册表中的所有标识，唯一方法是使用[导出](#importexport)功能。

IoT 中心设备标识注册表：

- 不包含任何应用程序元数据。
- 可以将 **deviceId** 作为键进行访问，就像字典一样。
- 不支持表达性查询。

IoT 解决方案通常具有不同的解决方案特定存储，其中包含应用程序特定的元数据。例如，智能建筑物解决方案中的解决方案特定存储将记录部署温度感应器的房间信息。

> [AZURE.IMPORTANT] 请只将设备标识注册表用于设备管理和预配操作。运行时的高吞吐量操作不应依赖于在设备标识注册表中执行操作。例如，在发送命令前先检查设备的连接状态就是不支持的模式。请务必检查设备标识注册表的[限制速率](#throttling)以及[设备检测信号][lnk-guidance-heartbeat]模式。

### 禁用设备

可以通过更新注册表中标识的 **status** 属性来禁用设备。通常在两种情况下使用此操作：

- 在预配协调过程中。有关详细信息，请参阅[设计你的解决方案 - 设备预配][lnk-guidance-provisioning]。
- 你出于任何原因认为设备遭到入侵或未经授权。

### 导出设备标识 <a id="importexport"></a>

导出是长时间运行的操作，它使用客户提供的 Blob 容器来保存从标识注册表读取的设备标识数据。

可以使用 [IoT 中心资源提供程序终结点](#endpoints)上的异步操作，从 IoT 中心的标识注册表批量导出设备标识。

以下是可以对导出作业执行的操作：

* 创建导出作业
* 检索正在运行的作业的状态
* 取消正在运行的作业

> [AZURE.NOTE] 在任意给定时间，每个中心只可以有一项正在运行的作业。

有关导入和导出 API 的详细信息，请参阅 [Azure IoT 中心 - 资源提供程序 API][lnk-resource-provider-apis]。

若要详细了解如何运行导入和导出操作，请参阅[批量管理 IoT 中心的设备标识][lnk-bulk-identity]

### 导出作业

所有导出作业都具有以下属性：

| 属性 | 选项 | 说明 |
| -------- | ------- | ----------- |
| jobId | 由系统生成，创建时被忽略 | |
| creationTime | 由系统生成，创建时被忽略 | |
| endOfProcessingTime | 由系统生成，创建时被忽略 | |
| type | 只读 | **ExportDevices** |
| status | 由系统生成，创建时被忽略 | **Enqueued**、**Started**、**Completed**、**Failed** |
| 进度 | 由系统生成，创建时被忽略 | 完成百分比的整数值。 |
| outputBlobContainerURI | 所有作业都需要 | 具有 Blob 容器写入权限的 Blob 共享访问签名 URI（请参阅[创建 SAS 并将其用于 Blob 服务][lnk-createuse-sas]）。用于输出作业的状态和结果。 |
| excludeKeysInExport | 可选 | 如果为 **false**，则密钥将包含在导出输出中；否则像为 **null** 时一样导出密钥。默认值为 **false**。 |
| failureReason | 由系统生成，创建时被忽略 | 如果状态为 **Failed**，则为包含原因的字符串。 |

导出作业使用 Blob 共享访问签名 URI 作为参数。这会授予 Blob 容器的写入权限，使作业能够输出其结果。

该作业将输出结果写入名为 **devices.txt** 的文件中指定的 Blob 容器。此文件包含按[设备标识属性](#deviceproperties)中的指定序列化为 JSON 的设备标识。如果 **excludeKeysInExport** 参数设置为 **true**，则 **devices.txt** 文件中每个设备的身份验证值将设置为 **null**。

**示例**：

```
{"id":"devA","eTag":"MQ==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"123","secondaryKey":"123"}}}
{"id":"devB","eTag":"MQ==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"123","secondaryKey":"123"}}}
{"id":"devC","eTag":"MQ==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"123","secondaryKey":"123"}}}
```

### 导入设备标识

导入是长时间运行的操作，它使用客户提供的 Blob 容器中的数据，将设备标识数据写入设备标识注册表。

可以使用 [IoT 中心资源提供程序终结点](#endpoints)上的异步操作，将设备标识批量导入 IoT 中心的标识注册表。

以下是可以对导入作业执行的操作：

* 创建导入作业
* 检索正在运行的作业的状态
* 取消正在运行的作业

> [AZURE.NOTE] 在任意给定时间，每个中心只可以有一项正在运行的作业。

有关导入和导出 API 的详细信息，请参阅 [Azure IoT 中心 - 资源提供程序 API][lnk-resource-provider-apis]。

若要详细了解如何运行导入和导出操作，请参阅[批量管理 IoT 中心的设备标识][lnk-bulk-identity]

### 导入作业

所有导入作业都具有以下属性：

| 属性 | 选项 | 说明 |
| -------- | ------- | ----------- |
| jobId | 由系统生成，创建时被忽略 | |
| creationTime | 由系统生成，创建时被忽略 | |
| endOfProcessingTime | 由系统生成，创建时被忽略 | |
| type | 只读 | **ImportDevices** |
| status | 由系统生成，创建时被忽略 | **Enqueued**、**Started**、**Completed**、**Failed** |
| 进度 | 由系统生成，创建时被忽略 | 完成百分比的整数值。 |
| outputBlobContainerURI | 所有作业都需要 | 具有 Blob 容器写入权限的 Blob 共享访问签名 URI（请参阅[创建 SAS 并将其用于 Blob 服务][lnk-createuse-sas]）。用于输出作业的状态。 |
| inputBlobContainerURI | 必填 | 具有 Blob 容器读取权限的 Blob 共享访问签名 URI（请参阅[创建 SAS 并将其用于 Blob 服务][lnk-createuse-sas]）。作业从此 Blob 中读取要导入的设备信息。 |
| failureReason | 由系统生成，创建时被忽略 | 如果状态为 **Failed**，则为包含原因的字符串。 |

导入作业使用两个 Blob 共享访问签名 URI 作为参数。其中一个 URI 向 Blob 容器授予写入权限以让作业输出其状态，另一个 URI 向 Blob 容器授予读取权限以让作业读取其输入数据。

作业从名为 **devices.txt** 的文件中指定的 Blob 容器读取输入数据。此文件包含按[设备标识属性](#deviceproperties)中的指定序列化为 JSON 的设备标识。可以通过添加 **importMode** 属性重写每个设备的默认导入行为。此属性接受以下值之一：

| importMode | 说明 |
| -------- | ----------- |
| **createOrUpdate** | 如果不存在具有指定 **ID** 的设备，则表示是新注册的设备。<br/>如果设备已存在，则以所提供的输入数据覆盖现有信息，而不管 **ETag** 值为何。 |
| **create** | 如果不存在具有指定 **ID** 的设备，则表示是新注册的设备。<br/>如果设备已存在，则在日志文件中写入错误。 |
| **更新** | 如果已存在具有指定 **ID** 的设备，则以提供的输入数据覆盖现有信息，而不管 **ETag** 值为何。<br/>如果设备不存在，则在日志文件中写入错误。 |
| **updateIfMatchETag** | 如果已存在具有指定 **ID** 的设备，则仅当 **ETag** 匹配时，才以提供的输入数据覆盖现有信息。<br/>如果设备不存在，则在日志文件中写入错误。<br/>如果 **ETag** 不匹配，则在日志文件中写入错误。 |
| **createOrUpdateIfMatchETag** | 如果不存在具有指定 **ID** 的设备，则表示是新注册的设备。<br/>如果设备已存在，则仅当 **ETag** 匹配时，才以提供的输入数据覆盖现有信息。<br/>如果 **ETag** 不匹配，则在日志文件中写入错误。 |
| **删除** | 如果已存在具有指定 **ID** 的设备，则将它删除，而不管 **ETag** 值为何。<br/>如果设备不存在，则在日志文件中写入错误。 |
| **deleteIfMatchETag** | 如果已存在具有指定 **ID** 的设备，则仅当 **ETag** 匹配时才将它删除。如果设备不存在，则在日志文件中写入错误。<br/>如果 ETag 不匹配，则在日志文件中写入错误。 |

**示例**：

```
{"id":"devA","eTag":"MQ==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"123","secondaryKey":"123"}}, "importMode":"delete"}
{"id":"devB","eTag":"MQ==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"123","secondaryKey":"123"}}, "importMode":"createOrUpdate"}
{"id":"devC","eTag":"MQ==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"123","secondaryKey":"123"}}, "importMode":"create"}
```

## 安全性 <a id="security"></a>

本部分介绍用于保护 Azure IoT 中心的选项。

### 访问控制 <a id="accesscontrol"></a>

IoT 中心使用以下一组*权限*向每个 IoT 中心终结点授予访问权限。权限可根据功能限制对 IoT 中心的访问。

* **RegistryRead**。授予对设备标识注册表的读取访问权限。有关详细信息，请参阅[设备标识注册表](#device-identity-registry)。
* **RegistryReadWrite**。授予对设备标识注册表的读取和写入访问权限。有关详细信息，请参阅[设备标识注册表](#device-identity-registry)。
* **ServiceConnect**。授予对面向云服务的通信和监视终结点的访问权限。例如，它授权后端云服务接收设备到云的消息、发送云到设备的消息，以及检索对应的传送确认。
* **DeviceConnect**。授予对面向设备的通信终结点的访问权限。例如，它授予发送设备到云的消息和接收云到设备的消息的权限。此权限由设备使用。

可以通过以下方式授予权限：

* **中心级别的共享访问策略**。共享访问策略可以授予前一部分中所列权限的任意组合。你可以在 [Azure 门户][lnk-management-portal]中定义策略，或使用 [Azure IoT 中心资源提供程序 API][lnk-resource-provider-apis] 以编程方式定义策略。新建的 IoT 中心有以下默认策略：

    - *iothubowner*：包含所有权限的策略
    - *service*：包含 **ServiceConnect** 权限的策略
    - *device*：包含 **DeviceConnect** 权限的策略
    - *registryRead*：包含 **RegistryRead** 权限的策略
    - *registryReadWrite*：包含 **RegistryRead** 和 **RegistryWrite** 权限的策略

* **每个设备的安全凭据**。每个 IoT 中心包含一个[设备标识注册表](#device-identity-registry)。对于此注册表中的每个设备，可以配置安全凭据，以相应设备终结点为范围来授予 **DeviceConnect** 权限。

**示例**。在典型的 IoT 解决方案中：
- 设备管理组件使用 *registryReadWrite* 策略。
- 事件处理器组件使用 *service* 策略。
- 运行时设备业务逻辑组件使用 *service* 策略。
- 各个设备使用 IoT 中心标识注册表中存储的凭据进行连接。

有关 IoT 中心安全主题的指导，请参阅[设计你的解决方案][lnk-guidance-security]中的“安全性”部分。

### 身份验证

Azure IoT 中心可根据共享访问策略和设备标识注册表安全凭据来验证令牌，以授予对终结点的访问权限。

安全凭据（例如对称密钥）永远不会通过网络发送。

> [AZURE.NOTE] 如同 [Azure Resource Manager][lnk-azure-resource-manager] 中的所有提供程序一样，Azure IoT 中心资源提供程序也是通过 Azure 订阅受到保护。

#### 安全令牌格式 <a id="tokenformat"></a>

安全令牌采用以下格式：

	SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

以下是预期值：

| 值 | 说明 |
| ----- | ----------- |
| {signature} | HMAC-SHA256 签名字符串，格式为：`{URL-encoded-resourceURI} + "\n" + expiry`。**重要说明**：密钥是从 base64 解码得出的，用作执行 HMAC-SHA256 计算的密钥。 |
| {resourceURI} | 可以使用此令牌访问的终结点的 URI 前缀（根据分段）。例如 `/events` |
| {expiry} | 从纪元 1970 年 1 月 1日 00:00:00 UTC 时间至今秒数的 UTF8 字符串。 |
| {URL-encoded-resourceURI} | 小写资源 URI 的小写 URL 编码 |
| {policyName} | 此令牌所引用的共享访问策略名称。在令牌引用设备注册表凭据的情况下不存在。 |

**有关前缀的说明**：URI 前缀是根据分段而不是字符计算的。例如，`/a/b` 是 `/a/b/c` 的前缀，而不是 `/a/bc` 的前缀。

可以在 IoT 设备和服务 SDK 中找到签名算法的实现：

* [适用于 Java 的 IoT 服务 SDK](https://github.com/Azure/azure-iot-sdks/tree/master/java/service/iothub-service-sdk/src/main/java/com/microsoft/azure/iot/service/auth)
* [适用于 Java 的 IoT 设备 SDK](https://github.com/Azure/azure-iot-sdks/tree/master/java/device/iothub-java-client/src/main/java/com/microsoft/azure/iothub/auth)
* [适用于 Node.js 的 IoT 设备和服务 SDK](https://github.com/Azure/azure-iot-sdks/blob/master/node/common/core/lib/shared_access_signature.js)

#### 协议详情

每个支持的协议（例如 AMQP、MQTT 和 HTTP）以不同的方式传输令牌。

HTTP 通过在 **Authorization** 请求标头中包含有效的令牌来实现身份验证。名为 **Authorization** 的查询参数也可以传输令牌。

使用 [AMQP][lnk-amqp] 时，IoT 中心支持 [SASL PLAIN][lnk-sasl-plain] 和 [AMQP 基于声明的安全性][lnk-cbs]。

对于 AMQP 基于声明的安全性，标准将指定如何传输这些令牌。

在 SASL PLAIN 中，**username** 可以是：

* 对于中心级别的令牌，为 `{policyName}@sas.root.{iothubName}`。
* 对于设备范围的令牌，为 `{deviceId}`。

在这两种情况下，密码字段都包含[令牌格式](#tokenformat)部分中所述的令牌。

使用 MQTT 时，CONNECT 数据包具有用作 ClientId 的 deviceId，在 Username 字段中具有 {iothubhostname}/{deviceId}，在 Password 字段中具有 SAS 令牌。{iothubhostname} 应是 IoT 中心的完整 CName（例如，contoso.azure-devices.net）。

> [AZURE.NOTE] [Azure IoT 中心 SDK][lnk-apis-sdks] 在连接到服务时自动生成令牌。在某些情况下，SDK 不支持所有的协议或所有身份验证方法。

#### SASL PLAIN 与 CBS 的比较

使用 SASL PLAIN 时，连接到 IoT 中心的客户端可为每个 TCP 连接使用单个令牌。当令牌过期时，TCP 将从服务断开连接，并触发重新连接。此行为虽然不对应用程序后端组件造成问题，但对设备端应用程序极为不利，原因如下：

*  网关通常代表许多设备连接。使用 SASL PLAIN 时，它们必须针对连接到 IoT 中心的每个设备创建不同的 TCP 连接。这会大幅提高电源与网络资源的消耗并增大每个设备连接的延迟。
* 在每个令牌过期后，增加使用要重新连接的资源通常会对资源受限的设备造成不良影响。

### 设置中心级凭据的范围

可以通过使用受限资源 URI 创建令牌，来设置中心级安全策略的范围。例如，要从设备发送设备对云消息的终结点是 **/devices/{deviceId}/events**。也可以使用中心级别的共享访问策略配合 **DeviceConnect** 权限，签署 resourceURI 为 **/devices/{deviceId}** 的令牌，创建只能用于代表设备 **deviceId** 发送消息的令牌。

此机制类似于[事件中心发布者策略][lnk-event-hubs-publisher-policy]，并让你根据[设计你的解决方案][lnk-guidance-security]的“安全性”部分中所述，实现自定义身份验证方法。

## 消息传递

IoT 中心提供消息传送基元用于通信：
- [云到设备](#c2d)：从应用程序后端（*服务*或*云*）。
- [设备到云](#d2c)：从设备到应用程序后端。

IoT 中心消息传送功能的核心属性是消息的可靠性和持久性。这可在设备端上恢复间歇性连接，以及在云恢复事件处理的负载高峰。IoT 中心针对设备到云和云到设备实现*至少一次*传送保证。

IoT 中心支持多个面向设备的协议（例如 AMQP 和 HTTP/1）。为了支持无缝的跨协议互操作性，IoT 中心定义了所有面向设备的协议均可支持的通用消息格式。

### 消息格式 <a id="messageformat"></a>

IoT 中心消息包含：

* 一组*系统属性*。这些是 IoT 中心解释或设置的属性。此集合是预先确定的。
* 一组*应用程序属性*。这是应用程序可以定义的字符串属性字典，而不需将消息正文反序列化即可进行访问。IoT 中心永不修改这些属性。
* 不透明的二进制正文。

有关如何在不同的协议中将消息编码的详细信息，请参阅 [IoT 中心 API 和 SDK][lnk-apis-sdks]。

这是 IoT 中心消息中的系统属性集。

| 属性 | 说明 |
| -------- | ----------- |
| MessageId | 用户可设置的消息标识符，通常用于请求-答复模式。格式：ASCII 7 位字母数字字符 + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`.的区分大小写字符串（最长为 128 个字符）。 |
| 序列号 | IoT 中心分配给每条云到设备消息的编号（对每个设备队列是唯一的）。 |
| 如果 | 在[云到设备](#c2d)的消息中用于指定目标。 |
| ExpiryTimeUtc | 消息过期的日期和时间。 |
| EnqueuedTime | IoT 中心收到消息的日期和时间。 |
| CorrelationId | 响应消息中的字符串属性，通常包含采用“请求-答复”模式的请求的 MessageId。 |
| UserId | 用于指定消息的源。如果消息是由 IoT 中心生成的，则设置为 `{iot hub name}`。 |
| Ack | 在云到设备的消息中用于请求 IoT 中心因为设备使用消息而生成反馈消息。可能的值：**none**（默认值）：不生成任何反馈消息。**positive**：如果消息已完成，则接收反馈消息。**negative**：如果消息未由设备完成就过期（或已达到最大传送计数），则收到反馈消息。**full**：positive 和 negative。若需详细信息，请参阅[消息反馈](#feedback)。 |
| ConnectionDeviceId | 由 IoT 中心对设备到云的消息进行设置。包含发送消息的设备的 **deviceId**。 |
| ConnectionDeviceGenerationId | 由 IoT 中心对设备到云的消息进行设置。包含发送消息的设备的 **generationId**（根据[设备标识属性](#deviceproperties)）。 |
| ConnectionAuthMethod | 由 IoT 中心对设备到云的消息进行设置。用于验证发送消息的设备的身份验证方法的相关信息。有关详细信息，请参阅[设备到云的反欺骗](#antispoofing)。|

### 选择通信协议 <a id="amqpvshttp"></a>

在设备端通信方面，Iot 中心支持 [AMQP][lnk-amqp]、基于 WebSockets 的 AMQP、MQTT 和 HTTP/1 协议。以下是有关这些协议用法的注意事项列表。

* **云到设备模式**。HTTP/1 不会以有效的方式实现服务器推送。因此，使用 HTTP/1 时，设备将在 IoT 中心轮询云到设备的消息。这对于设备和 IoT 中心而言是非常低效的。使用 HTTP/1 时，目前的指导原则是让每个设备每隔 25 分钟或以上轮询一次。另一方面，AMQP 和 MQTT 在接收云到设备的消息时支持服务器推送，而且能立即将消息从 IoT 中心推送到设备。如果传送延迟是一大考虑因素，最好使用 AMQP 或 MQTT 协议。另一方面，HTTP/1 能够很好地适应连接不良的设备。
* **现场网关**。使用 HTTP/1 和 MQTT 时，无法使用相同的 TLS 连接来连接到多个设备（各有自身的设备凭据）。因此，在实施[现场网关方案][lnk-azure-gateway-guidance]时，这些并不是最理想的协议，因为对于每个连接到现场网关的设备，这些协议需要在现场网关和 IoT 中心之间建立 TLS 连接。
* **低资源设备**。MQTT 和 HTTP/1 库的占用空间比 AMQP 库更小。因此，如果设备拥有的资源很少（例如，小于 1 Mb RAM），这些协议可能是唯一可用的协议实现。
* **网络遍历**。MQTT 标准在端口 8883 上侦听。这可能会导致对非 HTTP 协议关闭的网络发生问题。HTTP 和 AMQP（基于 WebSockets）均可用于此方案。
* **负载大小**。AMQP 和 MQTT 是二进制协议，因此明显比 HTTP/1 更精简。

概括而言，你应该尽可能使用 AMQP（或基于 WebSockets 的 AMQP），而且仅当资源的约束阻止使用 AMQP 时才使用 MQTT。仅当网络遍历和网络配置阻止使用 MQTT 和 AMQP 时，才应该使用 HTTP/1。此外，在使用 HTTP/1 时，每个设备应该每隔 25 分钟或以上轮询一次云到设备的消息。

> [AZURE.NOTE] 显然，在开发期间以比每隔 25 分钟一次还频繁的间隔进行轮询是可接受的情况。

#### 有关 MQTT 支持的说明
IoT 中心实现 MQTT v3.1.1 协议，但具有以下限制和特定行为：

  * **不支持 QoS 2**：如果设备客户端使用 **QoS 2** 发布消息，IoT 中心将关闭网络连接。当设备客户端使用 **QoS 2** 订阅主题时，IoT 中心将在 **SUBACK** 数据包中授予最大 QoS 级别 1。
  * **保留**：如果设备客户端发布 RETAIN 标志设置为 1 的消息，IoT 中心将在消息中添加 **x-opt-retain** 应用程序属性。这意味着 IoT 中心不会持续保留消息，而是将它传递到后端应用程序。

最后请检查 [Azure IoT 协议网关][lnk-azure-protocol-gateway]，它可让你部署直接与 IoT 中心交互的高性能自定义协议网关。Azure IoT 协议网关可让你自定义设备协议，以适应要重建的 MQTT 部署或其他自定义协议。此方法的代价是需要自我托管和运行自定义协议网关。

### 设备到云 <a id="d2c"></a>

如[终结点](#endpoints)部分中所详述，设备到云的消息是通过面向设备的终结点 (**/devices/{deviceId}/messages/events**) 发送的，并通过与[事件中心][lnk-event-hubs]兼容的面向服务的终结点 (**/messages/events**) 接收。因此，你可以使用标准事件中心集成和 SDK 来接收设备到云的消息。

IoT 中心实现设备到云消息传送的方式类似于[事件中心][lnk-event-hubs]，而比起[服务总线][lnk-servicebus]*消息*，IoT 中心的设备到云消息更像事件中心的*事件*。

这种实现具有以下含义：

* 与事件中心*事件*类似，设备到云的消息可持久保留在 IoT 中心多达 7 天（请参阅[设备到云的配置选项](#d2cconfiguration)）。
* 设备到云的消息分区存放到创建时设置的一组固定数据分区中（请参阅[设备到云的配置选项](#d2cconfiguration)）。
* 与事件中心类似，读取设备到云消息的客户端必须处理数据分区和检查点。请参阅[事件中心 - 使用事件][lnk-event-hubs-consuming-events]。
* 如同事件中心的事件，设备到云的消息最大可为 256 Kb，而且可分成多个批以优化发送。批最大可为 256 Kb，最多包含 500 条消息。

不过，IoT 中心的设备到云与事件中心之间还有一些重要差异：

* 如[安全性](#security)部分中所述，IoT 中心允许基于设备的身份验证和访问控制。
* IoT 中心允许同时连接数百万个设备（请参阅[配额和限制](#throttling)），而事件中心则受限于每个命名空间 5000 个 AMQP 连接。
* IoT 中心不允许使用 **PartitionKey** 进行任意数据分区。设备到云的消息根据其源的 **deviceId** 分区。
* IoT 中心的缩放方式与事件中心稍有不同。有关详细信息，请参阅[缩放 IoT 中心][lnk-guidance-scale]。

请注意，这并不表示在所有情况下都能使用 IoT 中心代替事件中心。例如，在某些事件处理计算中，可能需要在分析数据流之前，根据不同属性或字段重新分区事件。在这种情况下，你可以使用事件中心来减少流处理管道的两个部分。有关详细信息，请参阅 [Azure 事件中心概述][lnk-eventhub-partitions]中的*分区*。

有关如何使用设备到云消息传送的详细信息，请参阅 [IoT 中心 API 和 SDK][lnk-apis-sdks]。

> [AZURE.NOTE] 使用 HTTP 来发送设备到云的消息时，以下字符串只能包含 ASCII 字符：系统属性值，以及应用程序属性名称和值。

#### 非遥测流量

在许多情况下，除了遥测数据点之外，设备还会发送消息以及要求执行和处理应用程序业务逻辑层的请求。例如，必须在后端触发特定操作的关键警报，或者对由后端发送的命令做出的设备响应。

有关这种消息的最佳处理方式，请参阅[设备到云的处理][lnk-guidance-d2c-processing]。

#### 设备到云的配置选项 <a id="d2cconfiguration"></a>

IoT 中心公开以下属性让你控制设备到云的消息传送。

* **分区计数**。在创建时设置此属性，以便为设备到云事件引入定义分区数。
* **保留时间**。此属性指定设备到云消息的保留时间。默认值为一天，但可以增加到七天。

此外，类似于事件中心，IoT 中心也可让你管理设备到云接收终结点上的“使用者组”。

可以使用 [Azure 门户][lnk-management-portal]修改上述所有属性，或通过 [Azure IoT 中心 - 资源提供程序 API][lnk-resource-provider-apis] 以编程方式进行修改。

#### 防欺骗属性 <a id="antispoofing"></a>

为了避免设备到云的消息中出现设备欺骗，IoT 中心使用以下属性在所有消息上加上戳记：

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

前两个属性包含源设备的 **deviceId** 和 **generationId**（根据[设备标识属性](#deviceproperties)）。

**ConnectionAuthMethod** 属性包含具有以下属性的 JSON 序列化对象：

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

### 云到设备 <a id="c2d"></a>

如[终结点](#endpoints)部分中所详述，可以通过面向服务的终结点 (**/messages/devicebound**) 发送云到设备的消息，设备可通过设备特定的终结点 (**/devices/{deviceId}/messages/devicebound**) 接收这些消息。

每个云到设备的消息都以单个设备为目标，并将 **to** 属性设置为 **/devices/{deviceId}/messages/devicebound**。

**重要说明**：每个设备队列最多可以保留 50 条云到设备的消息。尝试将更多的消息传送到同一设备会导致错误。

> [AZURE.NOTE] 在发送云到设备的消息时，以下字符串只能包含 ASCII 字符：系统属性值，以及应用程序属性名称和值。

#### 消息生命周期 <a id="message lifecycle"></a>

为了实现*至少一次*传送保证，云到设备的消息将保存在每个设备的队列中，而设备必须显式确认*完成*，IoT 中心才会从队列中删除消息。这是为了保证连接失败和设备故障时能够恢复。

下图显示了云到设备消息的生命周期状态图。

![云到设备的消息生命周期][img-lifecycle]

当服务发送消息时，该消息被视为*已加入队列*。当设备想要*接收*消息时，IoT 中心将*锁定*该消息（将状态设置为 **Invisible**），以便让同一设备上的其他线程开始接收其他消息。当设备线程完成设备的处理时，将通过*完成*消息来通知 IoT 中心。

设备还可以：
- *拒绝*消息，这会导致 IoT 中心将其状态设置为 **Deadlettered**。
- *放弃*消息，这会导致 IoT 中心将消息放回队列并将其状态设置为 **Enqueued**。

线程可能无法处理消息，且不通知 IoT 中心。在此情况下，消息在过了*可见性（或锁定）超时*一分钟默认值之后，自动从 **Invisible** 状态转换回 **Enqueued** 状态。消息可以在 **Enqueued** 与 **Invisible** 状态之间转换的次数，以 IoT 中心上*最大传送计数*属性中指定的次数为限。在该转换次数之后，IoT 中心会将消息的状态设置为 **Deadlettered**。同样，IoT 中心也会在消息的过期时间过后（请参阅[生存时间](#ttl)），将消息的状态设置为 **Deadlettered**。

有关云到设备消息的教程，请参阅 [Azure IoT 中心云到设备的消息入门][lnk-getstarted-c2d-tutorial]。有关不同 API 和 SDK 如何公开云到设备功能的参考主题，请参阅 [IoT 中心 API 和 SDK][lnk-apis-sdks]。

> [AZURE.NOTE] 通常只要丢失消息不影响应用程序逻辑，就会完成云到设备的消息。这可能在许多不同的情况下发生。例如，消息内容已成功保存在本地存储中、已成功执行操作，或消息中包含丢失时不影响应用程序功能的暂时性信息。有时，对于长时间执行的作业，在本地存储中保存任务描述后，可以完成云到设备的消息，然后在作业进度的各个阶段，以一条或多条设备到云的消息通知应用程序后端。

#### 生存时间 <a id="ttl"></a>

每条云到设备的消息都有过期时间。这可以由服务（在 **ExpiryTimeUtc** 属性中）显式设置，或者由 IoT 中心使用指定为 IoT 中心属性的默认*生存时间*来设置。请参阅[云到设备的配置选项](#c2dconfiguration)。

#### 消息反馈 <a id="feedback"></a>

当你发送云到设备的消息时，服务可以请求传送每条消息的反馈（关于该消息的最终状态）。

- 如果将 **Ack** 属性设置为 **positive**，则只有在云到设备的消息达到 **Completed** 状态时，IoT 中心才生成反馈消息。
- 如果将 **Ack** 属性设置为 **negative**，则只有在云到设备的消息达到 **Deadletterd** 状态时，IoT 中心才生成反馈消息。
- 如果将 **Ack** 属性设置为 **full**，则 IoT 中心在上述任一情况下都会生成反馈消息。

> [AZURE.NOTE] 当 **Ack** 为 **full** 时，如果未收到任何反馈消息，即表示反馈消息已过期，服务无法得知原始消息发生什么状况。实际上，服务应该确保它可以在反馈过期之前对其进行处理。最长过期时间是两天，因此当发生失败时，应该有相当充裕的时间可以让服务启动并正常执行。

如[终结点](#endpoints)中所述，IoT 中心通过服务对应终结点 (**/messages/servicebound/feedback**) 以消息方式传送反馈。反馈的接收语义与云到设备消息的接收语义相同，并且具有相同的 [消息生命周期](#message lifecycle)。可能的话，消息反馈将放入单个消息中，其格式如下。

设备从反馈终结点检索的每条消息具有以下属性：

| 属性 | 说明 |
| -------- | ----------- |
| EnqueuedTime | 指示创建消息时的时间戳。 |
| UserId | `{iot hub name}` |
| ContentType | `application/vnd.microsoft.iothub.feedback.json` |

正文是记录的 JSON 序列化数组，每条记录具有以下属性：

| 属性 | 说明 |
| -------- | ----------- |
| EnqueuedTimeUtc | 指示消息结果出现时的时间戳。例如，设备已完成或消息已过期。 |
| OriginalMessageId | 与此反馈信息相关的云到设备消息的 **MessageId**。 |
| StatusCode | 必须是整数。在 IoT 中心生成的反馈消息中使用。<br/>0 = 成功 <br/> 1 = 消息已过期 <br/> 2 = 超出最大传送计数 <br/> 3 = 消息被拒绝 |
| 说明 | **StatusCode** 的字符串值。 |
| DeviceId | 与此反馈相关的云到设备消息的目标设备 **DeviceId**。 |
| DeviceGenerationId | 与此反馈相关的云到设备消息的目标设备 **DeviceGenerationId**。 |


**重要说明**。服务必须指定云到设备消息的 **MessageId**，才能使其反馈与原始消息相关联。

**示例**。这是反馈消息的正文示例。

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

#### 云到设备的配置选项 <a id="c2dconfiguration"></a>

每个 IoT 中心都针对云到设备消息传送公开以下配置选项：

| 属性 | 说明 | 范围和默认值 |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | 云到设备消息的默认 TTL。 | ISO\_8601 间隔高达 2D（最小为 1 分钟）。默认值：1 小时。 |
| maxDeliveryCount | 每个设备队列的云到设备最大传送计数。 | 1 到 100。默认值：10。 |
| feedback.ttlAsIso8601 | 服务绑定反馈消息的保留时间。 | ISO\_8601 间隔高达 2D（最小为 1 分钟）。默认值：1 小时。 |
| feedback.maxDeliveryCount | 反馈队列的最大传送计数。 | 1 到 100。默认值：100。 |

有关更多信息，请参阅[管理 IoT 中心][lnk-manage]。

## 配额和限制 <a id="throttling"></a>

每个 Azure 订阅最多可以有 10 个 IoT 中心。

每个 IoT 中心以特定 SKU 预配了特定数目的单位（有关详细信息，请参阅 [Azure IoT 中心定价][lnk-pricing]）。SKU 和单位数目确定可以发送的消息的每日配额上限。

SKU 还确定了 IoT 中心对所有操作强制实施的限制。

### 操作限制

操作限制是在分钟范围内应用的速率限制，主要是为了避免不当使用。IoT 中心会尽可能避免返回错误，但如果违反限制太久，就会开始返回异常。

以下是强制实施的限制列表。值与单个中心相关。

| 限制 | 每个中心的值 |
| -------- | ------------- |
| 标识注册表操作（创建、检索、列出、更新、删除） | 100/分钟/单位，最高 5000/分钟 |
| 设备连接 | 120/秒/单位（适用于 S2）, 12/秒/单位（适用于 S1）。<br/>最小值为 100/秒。<br/>例如，两个 S1 单位是 2*12 = 24/秒，但是在所有单位中至少有 100/秒。如果有 9 个 S1 单位，则所有单位就有 108/秒 (9*12)。 |
| 设备到云的发送 | 120/秒/单位（适用于 S2）, 12/秒/单位（适用于 S1）。<br/>最小值为 100/秒。<br/>例如，两个 S1 单位是 2*12 = 24/秒，但是在所有单位中至少有 100/秒。如果有 9 个 S1 单位，则所有单位就有 108/秒 (9*12)。 |
| 云到设备的发送 | 100/分钟/单位 |
| 云到设备的接收 | 1000/分钟/单位 |

**注意**。无论何时，都可以通过增加 IoT 中心预配的单位来提高配额或限制。

**重要说明**：标识注册表操作用于设备管理与预配方案中的运行时用途。通过[导入/导出操作](#importexport)可以支持读取或更新大量的设备标识。

## 后续步骤

现在你已了解开发 IoT 中心的概述，接下来请单击以下链接来了解更多信息：

- [IoT 中心入门（教程）][lnk-get-started]
- [操作系统平台和硬件兼容性][lnk-compatibility]
- [Azure IoT 开发人员中心][lnk-iotdev]
- [设计你的解决方案][lnk-guidance]

[事件中心 - 事件处理器主机]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx

[Azure 门户]: https://manage.windowsazure.cn

[img-endpoints]: ./media/iot-hub-devguide/endpoints.png
[img-lifecycle]: ./media/iot-hub-devguide/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide/eventhubcompatible.png

[lnk-compatibility]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/tested_configurations.md
[lnk-apis-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-pricing]: /pricing/details/iot-hub
[lnk-resource-provider-apis]: https://msdn.microsoft.com/zh-cn/library/mt548492.aspx

[lnk-azure-gateway-guidance]: /documentation/articles/iot-hub-guidance/#field-gateways
[lnk-guidance-provisioning]: /documentation/articles/iot-hub-guidance/#provisioning
[lnk-guidance-scale]: /documentation/articles/iot-hub-scaling
[lnk-guidance-security]: /documentation/articles/iot-hub-guidance/#customauth
[lnk-guidance-heartbeat]: /documentation/articles/iot-hub-guidance/#heartbeat

[lnk-azure-protocol-gateway]: /documentation/articles/iot-hub-protocol-gateway
[lnk-get-started]: /documentation/articles/iot-hub-csharp-csharp-getstarted
[lnk-guidance]: /documentation/articles/iot-hub-guidance
[lnk-getstarted-c2d-tutorial]: /documentation/articles/iot-hub-csharp-csharp-c2d

[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: /documentation/articles/resource-group-overview
[lnk-azure-resource-manager]: /documentation/articles/resource-group-overview/
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-createuse-sas]: /documentation/articles/storage-dotnet-shared-access-signature-part-2/
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-event-hubs]: /documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: /documentation/articles/event-hubs-programming-guide/#event-consumers
[lnk-guidance-d2c-processing]: /documentation/articles/iot-hub-csharp-csharp-process-d2c
[lnk-management-portal]: https://manage.windowsazure.cn
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-servicebus]: /documentation/services/service-bus/
[lnk-tls]: https://tools.ietf.org/html/rfc5246
[lnk-iotdev]: /develop/iot/
[lnk-bulk-identity]: /documentation/articles/iot-hub-bulk-identity-mgmt
[lnk-eventhub-partitions]: /documentation/articles/event-hubs-overview/#partitions
[lnk-manage]: /documentation/articles/iot-hub-manage-through-portal

<!---HONumber=Mooncake_0307_2016-->