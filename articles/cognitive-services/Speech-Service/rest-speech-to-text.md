---
title: 语音转文本 API 参考 (REST) - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何使用语音转文本 REST API。 本文介绍授权选项、查询选项，以及如何构建请求和接收响应。
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: trbye
ms.openlocfilehash: fbb4d114d1fee21d7950e53b06fc16c96b5c930b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400180"
---
# <a name="speech-to-text-rest-api"></a>语音转文本 REST API

作为[语音 SDK](speech-sdk.md) 的一种替代方法，语音服务允许使用 REST API 转换语音转文本。 每个可访问的终结点都与某个区域相关联。 应用程序需要所用终结点的订阅密钥。 REST API 非常有限，只有在[语音 SDK](speech-sdk.md)不能的情况下才应使用它。

使用语音转文本 REST API 之前，请先了解：

* 使用 REST API 并直接传输音频的请求最多只能包含 60 秒的音频。
* 语音转文本 REST API 仅返回最终结果。 不提供部分结果。

如果应用程序需要发送更长的音频，请考虑使用[语音 SDK](speech-sdk.md) 或基于文件的 REST API，如[批量转录](batch-transcription.md)。

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="regions-and-endpoints"></a>区域和终结点

REST API 的终结点具有以下格式：

```
https://<REGION_IDENTIFIER>.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1
```

替换`<REGION_IDENTIFIER>`与匹配此表中订阅区域的标识符：

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> 必须将语言参数追加到 URL 以避免收到 4xx HTTP 错误。 例如，使用“美国西部”终结点设置为美国英语的语言为：`https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US`。

## <a name="query-parameters"></a>查询参数

可将以下参数包含在 REST 请求的查询字符串中。

| 参数 | 说明 | 必需/可选 |
|-----------|-------------|---------------------|
| `language` | 标识所要识别的口语。 请参阅[支持的语言](language-support.md#speech-to-text)。 | 必选 |
| `format` | 指定结果格式。 接受的值为 `simple` 和 `detailed`。 简单结果包括 `RecognitionStatus`、`DisplayText`、`Offset` 和 `Duration`。 详细响应包括多个具有置信度值的结果，以及四种不同的表示形式。 默认设置为 `simple`。 | 可选 |
| `profanity` | 指定如何处理识别结果中的不雅内容。 接受的值为 `masked`（将亵渎内容替换为星号）、`removed`（删除结果中的所有亵渎内容）或 `raw`（包含结果中的亵渎内容）。 默认设置为 `masked`。 | 可选 |
| `cid` | 使用[自定义语音门户](how-to-custom-speech.md)创建自定义模型时，可以通过 **"部署"** 页上的**终结点 ID**使用自定义模型。 使用**终结点 ID**作为查询字符串参数`cid`的参数。 | 可选 |

## <a name="request-headers"></a>请求标头

下表列出了语音转文本请求的必需和可选标头。

|标头| 说明 | 必需/可选 |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | 语音服务订阅密钥。 | 此标头或 `Authorization` 是必需的。 |
| `Authorization` | 前面带有单词 `Bearer` 的授权令牌。 有关详细信息，请参阅[身份验证](#authentication)。 | 此标头或 `Ocp-Apim-Subscription-Key` 是必需的。 |
| `Content-type` | 描述所提供音频数据的格式和编解码器。 接受的值为 `audio/wav; codecs=audio/pcm; samplerate=16000` 和 `audio/ogg; codecs=opus`。 | 必选 |
| `Transfer-Encoding` | 指定要发送分块的音频数据，而不是单个文件。 仅当要对音频数据进行分块时才使用此标头。 | 可选 |
| `Expect` | 如果使用分块传输，则发送 `Expect: 100-continue`。 语音服务将确认初始请求并等待附加的数据。| 如果发送分块的音频数据，则是必需的。 |
| `Accept` | 如果提供此标头，则值必须是 `application/json`。 语音服务以 JSON 格式提供结果。 某些请求框架提供不兼容的默认值。 最好始终包含 `Accept`。 | 可选，但建议提供。 |

## <a name="audio-formats"></a>音频格式

在 HTTP `POST` 请求的正文中发送音频。 它必须采用下表中的格式之一：

| 格式 | 编解码器 | Bitrate | 采样率  |
|--------|-------|---------|--------------|
| WAV    | PCM   | 16 位  | 16 kHz，单声道 |
| OGG    | OPUS  | 16 位  | 16 kHz，单声道 |

>[!NOTE]
>通过语音服务中的 REST API 和 WebSocket 支持上述格式。 [语音 SDK](speech-sdk.md)目前支持带有 PCM 编解码器[以及其他格式](how-to-use-codec-compressed-audio-input-streams.md)的 WAV 格式。

## <a name="sample-request"></a>示例请求

以下示例包括主机名和必需的标头。 必须注意，服务同时预期提供音频数据，但此示例未包括这些数据。 如前所述，建议进行分块，但不是非要这样做。

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codecs=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

## <a name="http-status-codes"></a>HTTP 状态代码

每个响应的 HTTP 状态代码指示成功或一般错误。

| HTTP 状态代码 | 说明 | 可能的原因 |
|------------------|-------------|-----------------|
| `100` | 继续 | 已接受初始请求。 继续发送剩余的数据。 （与块传输一起使用） |
| `200` | OK | 请求成功；响应正文是一个 JSON 对象。 |
| `400` | 错误的请求 | 语言代码未提供、不是支持的语言、音频文件无效等。 |
| `401` | 未授权 | 指定区域中的订阅密钥或授权令牌无效，或终结点无效。 |
| `403` | 禁止 | 缺少订阅密钥或授权令牌。 |

## <a name="chunked-transfer"></a>分块传输

分块传输 (`Transfer-Encoding: chunked`) 有助于降低识别延迟。 它允许语音服务在传输音频文件时开始处理该文件。 REST API 不提供部分结果或临时结果。

此代码示例演示如何以块的形式发送音频。 只有第一个区块应该包含音频文件的标头。 `request`是`HttpWebRequest`连接到相应 REST 终结点的对象。 `audioFile` 是音频文件在磁盘上的路径。

```csharp
var request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = host;
request.ContentType = @"audio/wav; codecs=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";
request.AllowWriteStreamBuffering = false;

using (var fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{
    // Open a request stream and write 1024 byte chunks in the stream one at a time.
    byte[] buffer = null;
    int bytesRead = 0;
    using (var requestStream = request.GetRequestStream())
    {
        // Read 1024 raw bytes from the input audio file.
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        requestStream.Flush();
    }
}
```

## <a name="response-parameters"></a>响应参数

结果以 JSON 格式提供。 `simple` 格式包含以下顶级字段。

| 参数 | 说明  |
|-----------|--------------|
|`RecognitionStatus`|状态，例如 `Success` 表示成功识别。 请参阅下表。|
|`DisplayText`|大写后识别的文本，标点符号，反文本规范化（将口语文本转换为较短的形式，如200表示"200"或"史密斯博士"为"史密斯医生"），以及亵渎性掩盖。 仅在成功时提供。|
|`Offset`|在音频流中开始识别语音的时间（以 100 纳秒为单位）。|
|`Duration`|在音频流中识别语音的持续时间（以 100 纳秒为单位）。|

`RecognitionStatus` 字段可包含以下值：

| 状态 | 说明 |
|--------|-------------|
| `Success` | 识别成功并且存在 `DisplayText` 字段。 |
| `NoMatch` | 在音频流中检测到语音，但没有匹配目标语言的字词。 通常表示识别语言不同于讲话用户所用的语言。 |
| `InitialSilenceTimeout` | 音频流的开始仅包含静音，并且服务在等待语音时超时。 |
| `BabbleTimeout` | 音频流的开始仅包含噪音，并且服务在等待语音时超时。 |
| `Error` | 识别服务遇到内部错误，无法继续。 如果可能，请重试。 |

> [!NOTE]
> 如果音频仅包含亵渎内容，并且 `profanity` 查询参数设置为 `remove`，则服务不会返回语音结果。

`detailed` 格式包含的数据与 `simple` 格式相同，另外还包含 `NBest`，这是相同识别结果的替代解释列表。 这些结果从最有可能到最不可能进行排序。 第一个条目与主要识别结果相同。  使用 `detailed` 格式时，将以 `Display` 形式为 `NBest` 列表中的每条结果提供 `DisplayText`。

`NBest` 列表中的每个对象包括：

| 参数 | 说明 |
|-----------|-------------|
| `Confidence` | 条目的置信度评分，从 0.0（完全不可信）到 1.0（完全可信） |
| `Lexical` | 已识别文本的词法形式：识别的实际单词。 |
| `ITN` | 已识别文本的反向文本规范化（“规范”）形式，已应用电话号码、数字、缩写（“doctor smith”缩写为“dr smith”）和其他转换。 |
| `MaskedITN` | 可根据请求提供应用了亵渎内容屏蔽的 ITN 形式。 |
| `Display` | 已识别文本的显示形式，其中添加了标点符号和大小写形式。 此参数与将格式设置为 `simple` 时提供的 `DisplayText` 相同。 |

## <a name="sample-responses"></a>示例响应

`simple` 识别的典型响应：

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

`detailed` 识别的典型响应：

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="next-steps"></a>后续步骤

- [获取语音试用订阅](https://azure.microsoft.com/try/cognitive-services/)
- [自定义声学模型](how-to-customize-acoustic-models.md)
- [自定义语言模型](how-to-customize-language-model.md)
