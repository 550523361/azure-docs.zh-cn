<properties
 pageTitle="IoT 中心 MQTT 支持 | Microsoft Azure"
 description="介绍 IoT 中心级别的 MQTT 支持"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.date="02/03/2016"
 wacn.date=""/>

# IoT 中心 MQTT 支持

IoT 中心允许设备在端口 8883 上使用 [MQTT v3.1.1][lnk-mqtt-org] 协议来与 IoT 中心设备终结点通信。IoT 中心要求使用 TLS/SSL 保护所有的设备通信。

## 连接到 IoT 中心

设备既可以使用 [Microsoft Azure IoT SDK][lnk-device-sdks] 中的库，也可以直接使用 MQTT 协议连接到 IoT 中心。

## 使用设备客户端 SDK

支持 MQTT 协议的设备[客户端 SDK][lnk-device-sdks] 提供 Java、Node.js、C 和 C# 版本。设备客户端 SDK 使用标准 IoT 中心连接字符串来连接到 IoT 中心。若要使用 MQTT 协议，必须将客户端协议参数设置为 **MQTT**。默认情况下，设备客户端 SDK 连接到 **CleanSession** 标志设置为 **0** 的 IoT 中心，并使用 **QoS 1** 来与 IoT 中心交换消息。

当设备连接到 IoT 中心时，设备客户端 SDK 将提供方法让设备在 IoT 中心发送和接收消息。

下表包含了每种受支持语言的代码示例链接，并指定了以 MQTT 协议连接到 IoT 中心时要使用的参数。

| 语言 | 协议参数 |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | azure-iot-device-mqtt |
| [Java][lnk-sample-java] | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] | MQTT\_Protocol |
| [C#][lnk-sample-csharp] | TransportType.Mqtt |

## 直接使用 MQTT 协议

如果设备无法使用设备客户端 SDK，仍可使用 MQTT 协议连接到公共设备终结点。在 **CONNECT** 数据包中，设备应使用以下值：

- 使用 **deviceId** 作为 **ClientId**
- “用户名”字段中的 `{iothubhostname}/{device_id}`，其中 {iothubhostname} 是 IoT 中心的完整 CName（例如，contoso.azure-devices.net）。
- “密码”字段中的 SAS 令牌。[SAS 令牌的格式][lnk-iothub-security]与针对 HTTP 和 AMQP 协议所述的格式相同：<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

对于 MQTT 的连接和断开连接数据包，IoT 中心将在“操作监视”通道上发出事件。

### 将消息发送到 IoT 中心

成功建立连接后，设备可以使用 `devices/{did}/messages/events/` 或 `devices/{did}/messages/events/{property_bag}` 作为**主题名称**来将消息发送到 IoT 中心。`{property_bag}` 元素可让设备使用 url 编码格式发送包含其他属性的消息。例如：

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```
 
> [AZURE.NOTE] 此编程与 HTTP 协议中用于查询字符串的编码相同。

设备客户端应用程序还可以使用 `devices/{did}/messages/events/{property_bag}` 作为 **Will 主题名称**，来定义要以遥测消息形式转发的 *Will 消息*。

### 接收消息

若要从 IoT 中心接收消息，设备应使用 `devices/{did}/messages/devicebound/#”` 作为**主题筛选器**来进行订阅。如有任何消息属性，IoT 中心将传送包含**主题名称** `devices/{did}/messages/devicebound/` 或 `devices/{did}/messages/devicebound/{property_bag}` 的消息。`{property_bag}` 包含 url 编码的消息属性键/值对。属性包中只包含应用程序属性和用户可设置的系统属性（例如 **messageId** 或 **correlationId**）。系统属性名称具有前缀 **$**，但应用程序属性使用没有前缀的原始属性名称。

## 后续步骤

若要详细了解如何使用设备客户端 SDK 来与 IoT 中心通信，请参阅 [Azure IoT 中心入门][lnk-iot-get-stated]。

若要了解有关 MQTT 协议的详细信息，请参阅 [MQTT 文档][lnk-mqtt-docs]。

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-iot-get-stated]: /documentation/articles/iot-hub-csharp-csharp-getstarted
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-iothub-security]: /documentation/articles/iot-hub-devguide/#security
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples

<!---HONumber=Mooncake_0307_2016-->