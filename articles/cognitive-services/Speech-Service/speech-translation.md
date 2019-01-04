---
title: 关于语音翻译 - 语音服务
titlesuffix: Azure Cognitive Services
description: 使用语音服务 API，可以将端到端、实时和多语言语音翻译添加到应用程序、工具和设备。 相同 API 可以用于语音到语音和语音到文本的转换。
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 9d94105377597ca8e79cc43ed0903371e185d510
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2018
ms.locfileid: "53086702"
---
# <a name="about-the-speech-translation-api"></a>关于语音翻译 API

使用语音服务 API，可以将端到端、实时和多语言语音翻译添加到应用程序、工具和设备。 相同 API 可以用于语音到语音和语音到文本的转换。

借助语音翻译 API，客户端应用程序将语音音频流式传输到服务，然后接收返回的结果流。 这些结果包含源语言识别文本及其目标语言翻译。 可以提供临时翻译直到一个陈述完成，然后会提供最终翻译。

（可选）可以准备最终翻译的合成音频版本，以启用真正的语音到语音转换。

语音翻译 API 使用 Websocket 协议在客户端和服务器之间提供全双工信道。 但无需处理 Websocket；语音 SDK 会替你处理。

语音翻译 API 采用支持各种 Microsoft 产品和服务的相同技术。 此服务已被全球范围内数以千计的公司在其应用程序和工作流中使用。

## <a name="about-the-technology"></a>有关技术

基础 Microsoft 翻译引擎有两种不同的方法：统计机器翻译 (SMT) 和神经机器翻译 (NMT)。 后者是采用神经网络的人工智能方法，是更现代的机器翻译方法。 NMT 提供更好的翻译 — 不仅更准确，而且更流畅、更自然。 这种流动性的主要原因在于 NMT 使用一个句子的完整上下文来翻译单词。

今天，Microsoft 已迁移到涵盖最热门语言的 NMT，SMT 仅用于不太频繁使用的语言。 NMT 支持所有[可用于语音到语音转换的语言](language-support.md#speech-translation)。 语音到文本转换可能会使用 SMT 或 NMT，具体取决于语言对。 如果 NMT 支持目标语言，则 NMT 支持全译。 如果 NMT 不支持目标语言，则翻译是 NMT 和 SMT 的结合，将英语作为两种语言之间的“枢轴”。

模型之间的差异在于翻译引擎内部。 最终用户只需注意改进的翻译质量，尤其是中文、日语和阿拉伯语。

> [!NOTE]
> 有兴趣了解更多关于 Microsoft 翻译引擎背后的技术吗？ 请参阅[机器翻译](https://www.microsoft.com/en-us/translator/mt.aspx)。

## <a name="next-steps"></a>后续步骤

* [获取语音试用订阅](https://azure.microsoft.com/try/cognitive-services/)
* [参阅如何在 C# 中转换语音](how-to-translate-speech-csharp.md)
* [请参阅如何在 C# 中转换语音](how-to-translate-speech-cpp.md)
* [请参阅如何在 Java 中转换语音](how-to-translate-speech-java.md)
