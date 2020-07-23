---
title: 什么是语音服务？
titleSuffix: Azure Cognitive Services
description: 语音服务在单个 Azure 订阅中统合了语音转文本、文本转语音以及语音翻译功能。 使用语音 SDK、语音设备 SDK 或 REST API 在应用程序、工具和设备中添加语音。
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 06/25/2020
ms.author: trbye
ms.openlocfilehash: 43efc770f3a3b29a1b0fc214d2927c8c153dff96
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85391632"
---
# <a name="what-is-the-speech-service"></a>什么是语音服务？

语音服务在单个 Azure 订阅中统合了语音转文本、文本转语音以及语音翻译功能。 使用[语音 SDK](speech-sdk-reference.md)、[语音设备 SDK](https://aka.ms/sdsdk-quickstart) 或 [REST API](rest-apis.md) 可以轻松在应用程序、工具和设备中启用语音。

> [!IMPORTANT]
> 语音服务已替代必应语音 API 和语音翻译。 有关迁移说明，请参阅_操作指南 > 迁移_。

这些功能构成了语音服务。 请使用下表中的链接详细了解每项功能的常见用例或浏览 API 参考信息。

| 服务 | Feature | 说明 | SDK 中 IsInRole 中的声明 | REST |
|---------|---------|-------------|-----|------|
| [语音转文本](speech-to-text.md) | 实时语音转文本 | 语音转文本可将音频流或本地文件实时转录或翻译为文本，应用程序、工具或设备可以使用或显示这些文本。 结合[语言理解 (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/) 使用语音转文本可以从听录的语音中派生用户意向，以及处理语音命令。 | [是](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [是](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [批量语音转文本](batch-transcription.md) | 批量语音转文本支持对 Azure Blob 存储中存储的大量语音音频数据进行异步语音到文本转录。 除了将语音音频转换为文本，批量语音转文本还允许进行分割聚类和情感分析。 | 否 | [是](https://westus.cris.ai/swagger/ui/index) |
| | [多设备对话](multi-device-conversation.md) | 在对话中连接多个设备或客户端以发送基于语音或文本的消息，并轻松支持听录和翻译| 是 | 否 |
| | [对话听录](conversation-transcription-service.md) | 启用实时语音识别、说话人识别和分割聚类。 它非常适合用于听录能够区分说话人的面对面会谈场景。 | 是 | 否 |
| | [创建自定义语音识别模型](#customize-your-speech-experience) | 如果使用语音转文本在独特的环境中进行识别和听录，则可以创建并训练自定义的声学、语言和发音模型，以解决环境干扰或行业特定的词汇。 | 否 | [是](https://westus.cris.ai/swagger/ui/index) |
| [文本转语音](text-to-speech.md) | 文本转语音 | 文本转语音可使用[语音合成标记语言 (SSML)](speech-synthesis-markup.md) 将输入文本转换为类似人类的合成语音。 可以选择标准语音或神经语音（请参阅[语言支持](language-support.md)）。 | [是](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [是](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [创建自定义语音](#customize-your-speech-experience) | 创建专属于品牌或产品的自定义语音字体。 | 否 | [是](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [语音翻译](speech-translation.md) | 语音翻译 | 使用语音翻译可在应用程序、工具和设备中实现实时的多语言语音翻译。 进行语音转语音和语音转文本翻译时可以使用此服务。 | [是](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | 否 |
| [语音助手](voice-assistants.md) | 语音助手 | 语音助手使用语音服务为开发人员助力，使他们可为其应用程序和体验创建自然的、类似于人类的对话界面。 语音助手服务在设备和助手实现之间提供快速且可靠的交互。该实现使用 Bot Framework 的 Direct Line 语音通道或集成的自定义命令（预览版）服务来完成任务。 | [是](voice-assistants.md) | 否 |
| [说话人识别](speaker-recognition-overview.md) | 说话人验证和标识 | 说话人识别服务提供根据其独特的语音特征来验证和识别说话人的算法。 说话人识别用于回答“谁在说话？”的问题。 | 是 | [是](https://docs.microsoft.com/rest/api/speakerrecognition/) |


[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

## <a name="try-the-speech-service"></a>试用语音服务

我们提供了适用于大多数流行编程语言的快速入门，旨在帮助你在 10 分钟以内运行代码。 下表包含有关每项功能在最流行编程语言中的用法的快速入门。 使用左侧的导航栏可以浏览其他语言和平台。

| 语音转文本 (SDK) | 文本转语音 (SDK) | 翻译 (SDK) |
|----------------------|----------------------|-------------------|
| [识别来自音频文件的语音](quickstarts/speech-to-text-from-file.md) | [将语音合成为音频文件](quickstarts/text-to-speech-audio-file.md) | [将语音转换为文本](quickstarts/translate-speech-to-text.md) |
| [使用麦克风识别语音](quickstarts/speech-to-text-from-microphone.md) | [将语音合成到扬声器](quickstarts/text-to-speech.md) | [将语音翻译为多种目标语言](quickstarts/translate-speech-to-text-multiple-languages.md) |
| [识别存储在 Blob 存储中的语音](quickstarts/from-blob.md) | [用于长格式音频的异步合成](quickstarts/text-to-speech/async-synthesis-long-form-audio.md) | [将语音转换为语音](quickstarts/translate-speech-to-speech.md) |

> [!NOTE]
> “语音转文本”和“文本转语音”功能也有 REST 终结点和相关联的快速入门。

在你有机会使用语音服务后，请尝试一下我们的教程，了解如何解决各种情况。

- [教程：使用适用于 C# 的语音 SDK 和 LUIS 从语音中识别意向](how-to-recognize-intents-from-speech-csharp.md)
- [教程：使用语音 SDK、C# 为机器人启用语音](tutorial-voice-enable-your-bot-speech-sdk.md)
- [教程：生成 Flask 应用以翻译文本、分析情绪以及将翻译后的文本合成为语音 - REST](https://docs.microsoft.com/azure/cognitive-services/translator/tutorial-build-flask-app-translation-synthesis?toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json&bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json&toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fspeech-service%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)

## <a name="get-sample-code"></a>获取示例代码

GitHub 上提供了语音服务的示例代码。 这些示例涵盖了常见方案，例如，从文件或流中读取音频、连续和单次识别，以及使用自定义模型。 使用以下链接查看 SDK 和 REST 示例：

- [语音转文本、文本转语音和语音翻译示例 (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [批量听录示例 (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [文本转语音示例 (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)
- [语音助手示例 (SDK)](https://aka.ms/csspeech/samples)

## <a name="customize-your-speech-experience"></a>自定义语音体验

语音服务能够很好地与内置模型配合工作，但是，你可能想要根据自己的产品或环境，进一步自定义和优化体验。 自定义选项的范围从声学模型优化，到专属于自有品牌的语音字体。

| 语音服务 | 平台 | 说明 |
| -------------- | -------- | ----------- |
| 语音转文本 | [自定义语音识别](https://aka.ms/customspeech) | 根据需要和可用数据自定义语音识别模型。 克服语音识别障碍，如说话风格、词汇和背景噪音。 |
| 文本转语音 | [自定义语音](https://aka.ms/customvoice) | 使用可用语音数据为文本转语音应用生成可识别的独一无二的语音。 可以通过调整一组语音参数来进一步微调语音输出。 |

## <a name="reference-docs"></a>参考文档

- [语音 SDK](speech-sdk-reference.md)
- [语音设备 SDK](speech-devices-sdk.md)
- [REST API：语音转文本](rest-speech-to-text.md)
- [REST API：文本转语音](rest-text-to-speech.md)
- [REST API：批量听录和自定义](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [免费获取语音服务订阅密钥](get-started.md)
