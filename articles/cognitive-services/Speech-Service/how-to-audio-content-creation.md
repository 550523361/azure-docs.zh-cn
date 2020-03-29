---
title: 音频内容创建 - 语音服务
titleSuffix: Azure Cognitive Services
description: 音频内容创建是一个在线工具，允许您自定义和微调 Microsoft 的应用和产品的文本到语音输出。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: dapine
ms.openlocfilehash: ab0d2b8d95b4cb5996dd93fa0bb24085c9de26d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78331530"
---
# <a name="improve-synthesis-with-audio-content-creation"></a>通过音频内容创建改进合成

[音频内容创建](https://aka.ms/audiocontentcreation)是一个在线工具，允许您自定义和微调 Microsoft 的应用和产品的文本到语音输出。 您可以使用此工具微调公共和自定义语音，以便更准确的自然表达，并在云中管理输出。

音频内容创建工具基于[语音合成标记语言 （SSML）。](speech-synthesis-markup.md) 为了简化自定义和调优，音频内容创建允许您实时直观地检查文本到语音输出。

## <a name="how-does-it-work"></a>工作原理

此图显示了调整和导出自定义语音到文本输出的步骤。 使用以下链接详细了解每个步骤。

![](media/audio-content-creation/audio-content-creation-diagram.jpg)

1. 第一步是创建[Azure 帐户、注册语音资源并获得订阅密钥](#create-a-speech-resource)。 拥有订阅密钥后，可以使用它调用语音服务，并访问[音频内容创建](https://aka.ms/audiocontentcreation)。
2. 使用纯文本或 SSML[创建音频调谐文件](#create-an-audio-tuning-file)。
3. 选择要调整的声音和语言。 音频内容创建包括所有[微软文本到语音语音](language-support.md#text-to-speech)。 您可以使用标准、神经或您自己的自定义语音。
   >[!NOTE]
   > 门控访问可用于自定义神经语音，允许您创建类似于自然语音的高清语音。 有关详细信息，请参阅[门控过程](https://aka.ms/ignite2019/speech/ethics)。

4. 查看默认结果。 然后使用调优工具调整发音、音调、速率、语调、语音样式等。 有关选项的完整列表，请参阅[语音合成标记语言](speech-synthesis-markup.md)。
5. 保存并[导出已调谐的音频](#export-tuned-audio)。 在系统中保存调谐轨道时，可以继续工作并在输出上迭代。 当您对输出满意时，可以使用导出功能创建音频创建任务。 您可以观察导出任务的状态，并下载输出以用于应用和产品。
6. 最后一步是在应用和产品中使用自定义调谐语音。

## <a name="create-a-speech-resource"></a>创建语音资源

按照以下步骤创建语音资源并将其连接到语音工作室。

1. 按照这些说明注册[Azure 帐户](get-started.md#new-resource)并[创建语音资源](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-the-resource)。 请确保您的定价层设置为**S0**。 如果使用"神经"语音之一，请确保在[受支持的区域](regions.md#standard-and-neural-voices)中创建资源。
2. 登录[音频内容创建](https://aka.ms/audiocontentcreation)。
3. 选择现有项目，或单击"**新建**"。
4. 您可以随时使用位于顶部导航的 **"设置"** 选项修改订阅。

## <a name="create-an-audio-tuning-file"></a>创建音频调优文件

有两种方法可以将内容放入"音频内容创建"工具中。

**选项 1：**

1. 登录[音频内容创建](https://aka.ms/audiocontentcreation)后，单击 **"音频调优"** 以创建新的音频调优文件。
2. 当出现编辑窗口时，您最多可以输入 10，000 个字符。
3. 别忘了存钱。

**选项 2：**

1. 登录[音频内容创建](https://aka.ms/audiocontentcreation)后，单击"**上传**"以导入一个或多个文本文件。 支持纯文本和 SSML。
2. 上传文本文件时，请确保内容满足这些要求。

   | properties | 值/注释 |
   |----------|---------------|
   | 文件格式 | 纯文本 (.txt)<br/> SSML 文本 （.txt）<br/> 不支持 Zip 文件 |
   | 编码格式 | UTF-8 |
   | 文件名 | 每个文件必须具有唯一的名称。 不支持重复项。 |
   | 文本长度 | 文本文件不得超过 10，000 个字符。 |
   | SSML 限制 | 每个 SSML 文件只能包含一段 SSML。 |

### <a name="plain-text-example"></a>纯文本示例

```txt
Welcome to use Audio Content Creation to customize audio output for your products.
```

### <a name="ssml-text-example"></a>SSML 文本示例

```xml
<speak xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="http://www.w3.org/2001/mstts" version="1.0" xml:lang="en-US">
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, JessaNeural)">
    Welcome to use Audio Content Creation <break time="10ms" />to customize audio output for your products.
    </voice>
</speak>
```

## <a name="export-tuned-audio"></a>导出调谐音频

查看音频输出并对调优和调整感到满意后，可以导出音频。

1. 在["音频内容创建](https://aka.ms/audiocontentcreation)"工具中，单击"**导出**"以创建音频创建任务。
2. 选择已调谐音频的输出格式。 支持格式和采样率的列表如下。
3. 您可以在 **"导出任务"** 选项卡上查看任务的状态。如果任务失败，请参阅完整报表的详细信息页。
4. 任务完成后，您的音频可在 **"音频库"** 选项卡上下载。
5. 单击“下载”****。 现在，您可以在应用或产品中使用自定义调谐音频。

### <a name="supported-audio-formats"></a>支持的音频格式

| 格式 | 16 kHz 采样率 | 24 kHz 采样率 |
|--------|--------------------|--------------------|
| wav | 里夫-16khz-16位单件 | riff-24khz-16 位单位 pcm |
| mp3 | 音频-16khz-128kbitrate-单声道mp3 | 音频-24khz-160kbitrate-单声道mp3 |

## <a name="see-also"></a>请参阅

* [长音频 API](https://aka.ms/long-audio-api)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [Speech Studio](https://speech.microsoft.com)
