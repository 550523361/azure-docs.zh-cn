---
title: 发行说明 - 语音服务
titleSuffix: Azure Cognitive Services
description: 不断更新的语音服务功能版本、改进、bug 修复和已知问题的日志。
services: cognitive-services
author: oliversc
manager: jhakulin
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 08/17/2020
ms.author: oliversc
ms.custom: seodec18
ms.openlocfilehash: e786c4caae13ffaa541697d083158e17d42383fb
ms.sourcegitcommit: b8a175b6391cddd5a2c92575c311cc3e8c820018
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122563"
---
# <a name="speech-service-release-notes"></a>语音服务发行说明

## <a name="text-to-speech-2020-october-release"></a>文本到语音转换 2020-10 月发行版

**新功能**
- 来自支持新 `newscast` 样式。 请参阅[如何在 SSML 中使用说话风格](speech-synthesis-markup.md#adjust-speaking-styles)。
- **神经语音升级到 HiFiNet vocoder，其音频保真更高，合成速度更快**。 这为其方案依赖于 wi-fi 音频或长交互的客户带来了益处，包括视频配音、音频书籍或在线教育材料。 [阅读有关此案例的详细信息，倾听我们的技术社区博客上的语音示例](https://techcommunity.microsoft.com/t5/azure-ai/azure-neural-tts-upgraded-with-hifinet-achieving-higher-audio/ba-p/1847860)
- **[自定义语音](https://speech.microsoft.com/customvoice)  &  [音频内容创建工作室](https://speech.microsoft.com/audiocontentcreation)本地化为17个区域设置**。 用户可以轻松地将 UI 切换为本地语言，以获得更好的体验。   
- **音频内容创建**：为 XiaoxiaoNeural 添加了样式度控件;优化自定义中断功能以包含50ms 的增量中断。 

**一般性的 TTS 语音质量改进**
- 提高了 (错误率降低的单词级别的发音准确度 `pl-PL` ： 51% ) 和 `fi-FI` (错误率降低： 58% ) 
- 改进了 `ja-JP` 字典方案的单字阅读。 将发音错误减小了80%。
- `zh-CN-XiaoxiaoNeural`：改进了情绪/CustomerService/Newscast/文字/生气风格的语音质量。
- `zh-CN`：改进了 Erhua 发音和淡音和精致的空间诗体论，大大提高了 intelligibility。 

## <a name="speech-sdk-1140-2020-october-release"></a>Speech SDK 1.14.0： 2020-10 月发行版

**注意**：Windows 版语音 SDK 依赖于 Visual Studio 2015、2017 和 2019 的共享 Microsoft Visual C++ Redistributable。 可从[此处](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)下载。

**新功能**
- **Linux**：添加了对 Debian 10 和 UBUNTU 20.04 LTS 的支持。
- **Python/目标-C**：添加了对 API 的支持 `KeywordRecognizer` 。 [此处](https://docs.microsoft.com/azure/cognitive-services/speech-service/custom-keyword-basics)提供了文档。
- **C + +/Java/C #**：添加了对 `HttpHeader` 通过设置任何键/值的支持 `ServicePropertyChannel::HttpHeader` 。
- **JavaScript**：添加了对 API 的支持 `ConversationTranscriber` 。 阅读 [此处](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-use-conversation-transcription?pivots=programming-language-javascript)的文档。 
- **C + +/c #**：添加了 `AudioDataStream FromWavFileInput` 要读取 (新方法。WAV 文件) [此处 (c + +) ](https://docs.microsoft.com/cpp/cognitive-services/speech/audiodatastream) ， [此处 (c # ) ](
https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.audiodatastream?view=azure-dotnet)。
-  **C + +/c #/Java/Python/Objective-C/Swift**：已添加 `stopSpeakingAsync()` 停止文本到语音合成的方法。 请参阅此处 [ (c + +) ](https://docs.microsoft.com/cpp/cognitive-services/speech/microsoft-cognitiveservices-speech-namespace)的参考文档，此处 [ (c # ) ](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech?view=azure-dotnet)，此处 [ (Java) ](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech)， [此处 (Python) ](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech?view=azure-python)， [此处 (目标-C/Swift) ](https://docs.microsoft.com/objectivec/cognitive-services/speech/)。
- **C #、c + +、Java**：向类添加了一个 `FromDialogServiceConnector()` 函数， `Connection` 该函数可用于监视的连接和断开连接事件 `DialogServiceConnector` 。 请参阅此处的参考文档 [ (c # ) ](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.connection?view=azure-dotnet)， [此处 (c + +) ](https://docs.microsoft.com/cpp/cognitive-services/speech/connection)， [此处 (Java) ](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.connection)。
- **C + +/c #/Java/Python/Objective-C/Swift**：添加了对发音评估的支持，该支持评估语音发音，并为演讲音频提供精确和熟练反馈。 在[此处](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-pronunciation-assessment)阅读该文档。

**中断性变更**
- **JavaScript**： PullAudioOutputStream ( # A1 具有从内部承诺到本机 JavaScript 承诺的返回类型更改。

**Bug 修复**
- **All**：固定的1.13 回归 `SetServiceProperty` ，其中包含某些特殊字符的值被忽略。
- **C #**：修复了 Visual Studio 2019 上的 Windows 控制台示例未能找到本机 dll。
- **C #**：如果将 stream 用作输入，则修复了内存管理故障 `KeywordRecognizer` 。
- **ObjectiveC/Swift**：如果 stream 用作识别器输入，则修复了内存管理故障。
- **Windows**：修复了针对 UWP 的 BT HFP/A2DP 的共存问题。
- **JavaScript**：修复了会话 id 的映射，以改进日志记录并帮助进行内部调试/服务关联。
- **JavaScript**：在 `DialogServiceConnector` `ListenOnce` 第一次调用后添加了用于禁用调用的修补程序。
- **JavaScript**：修复了结果输出只能是 "简单" 的问题。
- **JavaScript**：修复了 MacOS 上 Safari 中的连续识别问题。
- **JavaScript**：高请求吞吐量方案的 CPU 负载缓解。
- **JavaScript**：允许访问语音配置文件注册结果的详细信息。
- **JavaScript**：已在中添加了持续识别的修补程序 `IntentRecognizer` 。
- **C + +/c #/Java/Python/Swift/ObjectiveC**：修复了中 australiaeast 和 brazilsouth 的错误 url `IntentRecognizer` 。
- **C + +/c #**： `VoiceProfileType` 创建对象时作为参数添加 `VoiceProfile` 。
- **C + +/c #/Java/Python/Swift/ObjectiveC**： `SPX_INVALID_ARG` 在尝试从给定位置读取时可能已修复 `AudioDataStream` 。
- **IOS**：已修复 Unity 上的语音识别故障

**示例**
- **ObjectiveC**： [此处](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/objective-c/ios/speech-samples)为关键字识别添加了示例。
- **C #/JavaScript**： [此处 (c # )](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/dotnet/conversation-transcription) 中添加了对话脚本的快速入门， [ (JavaScript)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/conversation-transcription)。
- **C + +/c #/Java/Python/Swift/ObjectiveC**：[在此](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples)添加了发音评估示例
- **Xamarin**： [在此处](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/xamarin)更新到最新的 Visual Studio 模板。

**已知问题**
- 默认情况下，在 HoloLens 2 和 Android 4.4 (KitKat) 中不支持 DigiCert 全局根 G2 证书，需要将其添加到系统中，才能使语音 SDK 正常运行。 不久后，证书将添加到 HoloLens 2 OS 映像。 Android 4.4 客户需要将更新的证书添加到系统。

**COVID-19 缩减测试：** 由于过去几周一直在远程工作，我们无法像往常那样执行那么多手动验证测试。 我们没有做我们认为可能会造成任何破坏的任何更改，我们的自动化测试已全部通过。 如果我们遗漏了某些内容，请在 [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen) 上告诉我们。<br>
请保重身体！

## <a name="speech-cli-also-known-as-spx-2020-october-release"></a>语音 CLI (也称为 SPX) ： 2020-10 月发行版
SPX 是用于在不编写代码的情况下使用 Azure 语音服务的命令行界面。 [在此处](https://docs.microsoft.com/azure/cognitive-services/speech-service/spx-basics)下载最新版本。 <br>

**新功能**
- `spx csr dataset upload --kind audio|language|acoustic` –从本地数据（而不仅仅是从 Url）创建数据集。
- `spx csr evaluation create|status|list|update|delete` -将新模型与基准事实/其他模型进行比较。
- `spx * list` –支持非分页体验 (不需要--top X--skip X) 。
- `spx * --http header A=B` –支持自定义标头 (为 Office 添加了自定义身份验证) 。 
- `spx help` –改进后的文本和反向文本颜色 (蓝色) 编码。

## <a name="text-to-speech-2020-september-release"></a>文本到语音转换 2020-九月版本

### <a name="new-features"></a>新增功能

* **神经 TTS** 
    * **扩展以支持18种新语言/区域设置。** 它们是保加利亚语、捷克语、德语 (奥地利) 、德语 (瑞士) 、希腊语、英语 (爱尔兰) 、法语 (瑞士) 、希伯来语、克罗地亚语、匈牙利语、印度尼西亚语、马来语、罗马尼亚语、斯洛伐克语、斯洛文尼亚语、泰卢固语和越南语。 
    * **发布了14个新语音，以丰富现有语言的各种语言。** 请参阅 [完整语言和语音列表](language-support.md#neural-voices)。
    * **和语音的新说话样式 `en-US` `zh-CN` 。** 来自是新的英语 (US) ，它支持聊天机器人、客户服务和助理样式。 在我们的 zh-chs voice，XiaoXiao 中提供了10个新的说话风格。 此外，XiaoXiao 神经语音支持 `StyleDegree` 优化。 请参阅[如何在 SSML 中使用说话风格](speech-synthesis-markup.md#adjust-speaking-styles)。

* **容器：在公共预览版中发布的神经 TTS 容器，提供14种语言版本的16个声音。** 详细了解 [如何部署用于神经 TTS 的语音容器](speech-container-howto.md)  

阅读 [Ignite 2020 的 TTS 更新的完整公告](https://techcommunity.microsoft.com/t5/azure-ai/ignite-2020-neural-tts-updates-new-language-support-more-voices/ba-p/1698544) 

## <a name="text-to-speech-2020-august-release"></a>文本转语音 2020 年 8 月发行版

### <a name="new-features"></a>新增功能

* **神经网络 TTS：`en-US` Aria 语音的一种新的说话风格**。 阅读新闻时，AriaNeural 可能会像新闻 caster。 “newscast-formal”风格听起来更严肃，而“newscast-casual”风格则更为放松和随意。 请参阅[如何在 SSML 中使用说话风格](speech-synthesis-markup.md)。

* **自定义语音：发布了一项新功能，可自动检查定型数据质量**。 当你上传数据时，系统将检查你的音频和记录数据的各个方面，并自动修复或筛选问题以提高语音模型的质量。 这涉及到音频的音量、噪音级别、语音的读音准确性、语音与规范化文本的对齐方式、音频中的静默性，以及音频和脚本格式。 

* **音频内容创建：一组新功能，可实现更强大的语音优化和音频管理功能**。

    * 发音：将发音优化功能更新为最新的音素集。 可以从库中选取正确的音素元素，并优化所选字词的发音。 

    * 下载：音频“下载”/“导出”这一功能得到增强，支持按段落生成音频。 可以编辑同一文件/SSML 中的内容，同时生成多个音频输出。 “下载”的文件结构也得到了完善。 现在，可以轻松地在一个文件夹中获取所有音频文件。 

    * 任务状态：多文件导出体验得到改善。 过去导出多个文件时，如果其中一个文件失败，则整个任务将失败。 但现在，所有其余文件都将成功导出。 任务报表中包含了更加详细和结构化的信息。 可以通过报表查看所有失败文件和句子的记录。 

    * SSML 文档：链接到 SSML 文档，查看有关如何使用所有优化功能的规则。

* **语音列表 API 进行了更新，以包括用户友好的显示名称和神经语音支持的说话样式**。

### <a name="general-tts-voice-quality-improvements"></a>一般性的 TTS 语音质量改进

* 降低了单词级别发音错误 %：`ru-RU`（错误减少 56%）、`sv-SE`（错误减少 49%）

* 对 `en-US` 神经网络语音的复音词读取能力提高了 40%。 复音词的示例包括“read”、“live”、“content”、“record”、“object”等。 

* 使 `fr-FR` 中的疑问语气更加自然。 MOS（平均意见得分）增益：+0.28

* 更新了以下语音的 vocoder，提高了保真度，整体性能提高 40%。

    | Locale | 语音 |
    |---|---|    
    | `en-GB` | Mia |
    | `es-MX` | Dalia |
    | `fr-CA` | Sylvie |
    | `fr-FR` | Denise |
    | `ja-JP` | Nanami |
    | `ko-KR` | Sun-Hi |

### <a name="bug-fixes"></a>Bug 修复

* 修复了音频内容创建工具的一些 Bug 
    * 修复了自动刷新问题。 
    * 修复了 zh-chs-南部东亚区域中的语音样式的问题。
    * 修复了稳定性问题，其中包含带有 "break" 标记的导出错误，以及标点符号错误。

## <a name="new-speech-to-text-locales-2020-august-release"></a>新的语音到文本区域设置： 2020-8 月发行版
在8月发布的 "语音到文本" 发布了26个新区域设置：2个欧洲语言 `cs-CZ` 和 `hu-HU` 5 个英语区域设置，以及涵盖最多南美国家/地区的19个西班牙语区域。 下面是新区域设置的列表。 请在 [此处](./language-support.md)查看完整的语言列表。

| Locale  | 语言                          |
|---------|-----------------------------------|
| `cs-CZ` | 捷克语（捷克共和国）            | 
| `en-HK` | 英语（香港）               | 
| `en-IE` | 英语（爱尔兰）                 | 
| `en-PH` | 英语（菲律宾）             | 
| `en-SG` | 英语（新加坡）               | 
| `en-ZA` | 英语（南非）            | 
| `es-AR` | 西班牙语（阿根廷）               | 
| `es-BO` | 西班牙语（玻利维亚）                 | 
| `es-CL` | 西班牙语（智利）                   | 
| `es-CO` | 西班牙语（哥伦比亚）                | 
| `es-CR` | 西班牙语（哥斯达黎加）              | 
| `es-CU` | 西班牙语（古巴）                    | 
| `es-DO` | 西班牙语（多米尼加共和国）      | 
| `es-EC` | 西班牙语（厄瓜多尔）                 | 
| `es-GT` | 西班牙语（危地马拉）               | 
| `es-HN` | 西班牙语（洪都拉斯）                | 
| `es-NI` | 西班牙（尼加拉瓜）               | 
| `es-PA` | 西班牙语（巴拿马）                  | 
| `es-PE` | 西班牙语（秘鲁）                    | 
| `es-PR` | 西班牙语（波多黎各）             | 
| `es-PY` | 西班牙语（巴拉圭）                | 
| `es-SV` | 西班牙语（萨尔瓦多）             | 
| `es-US` | 西班牙语（美国）                     | 
| `es-UY` | 西班牙语（乌拉圭）                 | 
| `es-VE` | 西班牙语（委内瑞拉）               | 
| `hu-HU` | 匈牙利语(匈牙利)               | 


## <a name="speech-sdk-1130-2020-july-release"></a>语音 SDK 1.13.0：2020 年 7 月发行版

**注意**：Windows 版语音 SDK 依赖于 Visual Studio 2015、2017 和 2019 的共享 Microsoft Visual C++ Redistributable。 从 [此处](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)下载并安装它。

**新功能**
- **C #**：添加了对异步会话脚本的支持。 参阅[此处](./how-to-async-conversation-transcription.md)的文档。  
- **JavaScript**：添加了对 [浏览器](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser/speaker-recognition) 和 [node.js](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/speaker-recognition)的演讲者识别支持。
- **JavaScript**：添加了对自动语言检测/语言 ID 的支持。 参阅[此处](./how-to-automatic-language-detection.md?pivots=programming-language-javascript)的文档。
- **目标-C**：添加了对 [多设备对话](./multi-device-conversation.md) 和 [对话](./conversation-transcription.md)脚本的支持。 
- **Python**：针对 Windows 和 Linux 上的 Python 添加了压缩音频支持。 参阅[此处](./how-to-use-codec-compressed-audio-input-streams.md)的文档。 

**Bug 修复**
- **全部**：修复了一个问题，导致 KeywordRecognizer 在识别后不向前移动流。
- **全部**：修复了一个问题，该问题导致从 KeywordRecognitionResult 获取的流不包含关键字。
- **全部**：修复了 SendMessageAsync 在用户等待消息时不通过网络发送消息的问题。
- **全部**：修复了在用户多次调用 VoiceProfileClient：： SpeakerRecEnrollProfileAsync 方法并且未等待调用完成的情况时，发言人识别 api 中的崩溃。
- **All**：修复了 VoiceProfileClient 和 SpeakerRecognizer 类中的启用文件日志记录。
- **JavaScript**：修复了在最小化浏览器时与限制相关的一个 [问题](https://github.com/microsoft/cognitive-services-speech-sdk-js/issues/74)。
- **JavaScript**：修复了流中的一个内存泄漏 [问题](https://github.com/microsoft/cognitive-services-speech-sdk-js/issues/78)。
- **JavaScript**：为来自 NodeJS 的 OCSP 响应添加了缓存。
- **Java**：修复了导致 BigInteger 字段总是返回 0 的问题。
- **ios**：修复了在 IOS 应用商店中发布基于语音 SDK 的应用的 [问题](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/702) 。

**示例**
- **C + +**： [在此处](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/cpp/windows/console/samples/speaker_recognition_samples.cpp)添加了演讲者识别的示例代码。

**COVID-19 缩减测试：** 由于过去几周一直在远程工作，我们无法像往常那样执行那么多手动验证测试。 我们没有做我们认为可能会造成任何破坏的任何更改，我们的自动化测试已全部通过。 如果我们遗漏了某些内容，请在 [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen) 上告诉我们。<br>
请保重身体！

## <a name="text-to-speech-2020-july-release"></a>文本转语音 2020 年 7 月发行版

### <a name="new-features"></a>新增功能

* **神经 TTS，15 种新的神经语音**：添加到神经 TTS 组合中的新语音包括：`ar-EG` 阿拉伯文（埃及）中的 Salma，`ar-SA` 阿拉伯文（沙特阿拉伯）中的 Zariyah，`ca-ES` 加泰罗尼亚语（西班牙）中的 Alba，`da-DK` 丹麦文（丹麦）中的 Christel，`es-IN` 英文（印度）中的 Neerja，`fi-FI` 芬兰文（芬兰）中的 Noora，`hi-IN` 印地语（印度）中的 Swara，`nl-NL` 荷兰语（荷兰）中的 Colette，`pl-PL` 波兰文（波兰）中的 Zofia，`pt-PT` 葡萄牙语（葡萄牙）中的 Fernanda，`ru-RU` 俄语（俄罗斯）中的 Dariya，`sv-SE` 瑞典文（瑞典）中的 Hillevi，`th-TH` 泰文（泰国）中的 Achara，`zh-HK` 中文（广东话，繁体）中的 HiuGaai，`zh-TW` 中文（台湾普通话）中的 HsiaoYu。 请检查所有[支持的语言](./language-support.md#neural-voices)。  

* **自定义语音，简化了训练流中的语音测试，从而简化了用户体验**：使用新的测试功能，将使用针对每种语言进行了优化的预定义测试集来自动测试每个语音，以涵盖一般方案和语音助手方案。 这些测试集经过仔细选择并经过测试，包含语言中的典型用例和音素。 除此之外，用户还可以选择在训练模型时上传自己的测试脚本。

* **音频内容创建：发布了一组新功能，可实现更强大的语音调整和音频管理功能**

    * 对 `Pitch`、`rate` 和 `volume` 进行了增强，以支持使用预定义值（例如慢、中和快）进行优化的功能。 现在，用户可以直接选择一个“常数”值来进行音频编辑。

    ![音频优化](media/release-notes/audio-tuning.png)

    * 用户现在可以查看其工作文件的 `Audio history`。 使用此功能，用户可以轻松地跟踪与工作文件相关的所有生成的音频。 他们可以检查历史版本并在优化时比较质量。 

    ![音频历史记录](media/release-notes/audio-history.png)

    * `Clear` 功能现在更加灵活。 用户可以清除特定的优化参数，同时保留可用于所选内容的其他参数。  

    * [登陆页面](https://speech.microsoft.com/audiocontentcreation)上添加了教程视频，可帮助用户快速开始使用 TTS 语音优化和音频管理。 

### <a name="general-tts-voice-quality-improvements"></a>一般性的 TTS 语音质量改进

* 改进了 TTS vocoder 以提高保真度并降低延迟。

    * 已将 `it-IT` 中的 Elsa 更新为新的 vocoder，它实现了 +0.464 CMOS（相对平均意见分数）的语音质量提高，合成速度提高 40%，首个字节延迟降低 30%。 
    * 已将 `zh-CN` 中的 Xiaoxiao 更新为新的 vocoder，对于一般领域提高了 +0148 CMOS，对于 newscast 风格提高了 +0.348，对于 lyrical 风格提高了 +0.195。 

* 更新了 `de-DE` 和 `ja-JP` 语音模型，使 TTS 输出更加自然。
    
    * 使用最新的韵律建模方法更新了 `de-DE` 中的 Katja，使 MOS（平均意见分数）提高了 +0.13。 
    * 使用一种新的音高重音韵律模型更新了 `ja-JP` 中的 Nanami，使 MOS（平均意见分数）提高了 +0.19；  

* 改进了五种语言的单词级别的读音准确性。

    | 语言 | 发音错误减少 |
    |---|---|
    | `en-GB` | 51% |
    | `ko-KR` | 17% |
    | `pt-BR` | 39% |
    | `pt-PT` | 77% |
    | `id-ID` | 46% |

### <a name="bug-fixes"></a>Bug 修复

* 货币读取
    * 修复了 `es-ES` 和 `es-MX` 的货币读取问题
     
    | 语言 | 输入 | 改进后的读出 |
    |---|---|---|
    | `es-MX` | $1.58 | un peso cincuenta y ocho centavos |
    | `es-ES` | $1.58 | un dólar cincuenta y ocho centavos |

    * 支持以下区域设置中的负货币（例如“-325 €”）：`en-US`、`en-GB`、`fr-FR`、`it-IT`、`en-AU`、`en-CA`。

* 改进了 `pt-PT` 中的地址读取。
* 修复了单词“for”和“four”的 Natasha (`en-AU`) 和 Libby (`en-UK`) 发音问题。  
* 修复了音频内容创建工具的 bug
    * 修复了在第二个段落后意外发生的额外停顿。  
    * 根据一个回归 bug 重新添加了“无中断”功能。 
    * 修复了 Speech Studio 的随机刷新问题。  

### <a name="samplessdk"></a>示例/SDK

* JavaScript：修复了 FireFox 中的以及 macOS 和 iOS 上的 Safari 中的播放问题。 

## <a name="speech-sdk-1121-2020-june-release"></a>语音 SDK 1.12.1：2020 年 6 月版本
**语音 CLI（也称为 SPX）**
-   添加了 CLI 内帮助搜索功能：
    -   `spx help find --text TEXT`
    -   `spx help find --topic NAME`
-   更新为可与新部署的 v3.0 批处理和自定义语音识别 API 配合使用：
    -   `spx help batch examples`
    -   `spx help csr examples`

**新功能**
-   **C \# ，c + +**：演讲者识别预览版：此功能使演讲者识别 (说？ ) 并且扬声器验证 (是他们宣称的演讲者？ ) 。 首先[请参阅 "](./speaker-recognition-overview.md)[演讲者识别基础" 文章](./get-started-speaker-recognition.md)或[API 参考文档](/rest/api/speakerrecognition/)。

**Bug 修复**
-   **C\#、C++** ：在 1.12 版中，修复的麦克风录音在说话人辨识中未正常工作。
-   **JavaScript**：针对 FireFox 中的以及 macOS 和 iOS 上的 Safari 中的文本转语音进行了修复。
-   解决在使用8个通道流时会话脚本上出现的 Windows 应用程序验证器访问冲突崩溃的问题。
-   针对多设备对话翻译中 Windows 应用程序验证工具访问冲突崩溃的修复。

**示例**
-   **C #**：用于演讲者识别的 [代码示例](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/dotnet/speaker-recognition) 。
-   **C + +**：用于演讲者识别的 [代码示例](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp/windows/speaker-recognition) 。
-   **Java**：Android 上用于意向识别的 [代码示例](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/intent-recognition)。 

**COVID-19 缩减测试：** 由于过去几周一直在远程工作，我们无法像往常那样执行那么多手动验证测试。 我们没有做我们认为可能会造成任何破坏的任何更改，我们的自动化测试已全部通过。 如果我们遗漏了某些内容，请在 [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen) 上告诉我们。<br>
请保重身体！


## <a name="speech-sdk-1120-2020-may-release"></a>语音 SDK 1.12.0：2020 年 5 月版本
**语音 CLI（也称为 SPX）**
- SPX 是新的命令行工具，可用于从命令行执行识别、合成、翻译、批量听录和自定义语音管理。 使用它来测试语音服务，或为需要执行的语音服务任务编写脚本。 下载该工具，并在[此处](./spx-overview.md)查看文档。

**新功能**
- **转到**[语音识别](./get-started-speech-to-text.md?pivots=programming-language-go)和 [自定义语音助手](./quickstarts/voice-assistants.md?pivots=programming-language-go)的全新转语言支持。 在[此处](./quickstarts/setup-platform.md?pivots=programming-language-go)设置开发环境。 有关示例代码，请参阅下面的“示例”部分。 
- **JavaScript**：添加了对文本转语音的浏览器支持。 参阅[此处](./get-started-text-to-speech.md?pivots=programming-language-JavaScript)的文档。
- **C++、C#、Java**：Windows、Android、Linux 和 iOS 平台上支持的新 `KeywordRecognizer` 对象和 API。 在[此处](./custom-keyword-overview.md)阅读该文档。 有关示例代码，请参阅下面的“示例”部分。 
- **Java**：添加了带翻译支持的多设备对话。 在[此处](/java/api/com.microsoft.cognitiveservices.speech.transcription)参阅参考文档。

**改进与优化**
- **JavaScript**：优化了浏览器麦克风实现，改善了语音识别的准确性。
- **Java**：使用直接 JNI 实现（没有 SWIG）重构了绑定。 此更改减少了用于 Windows、Android、Linux 和 Mac 的所有 Java 包的绑定大小的10倍，并简化了语音 SDK Java 实现的进一步开发。
- Linux：使用最新的 RHEL 7 特定说明更新了支持[文档](./speech-sdk.md?tabs=linux)。
- 改进了连接逻辑，以便尝试在发生服务和网络错误时多次连接。
- 更新了 [Portal.azure.com](https://portal.azure.com) Speech 快速入门页面，帮助开发人员在 azure 语音旅程中进行下一步。

**Bug 修复**
- **C #、Java**：修复了在 Linux ARM 上加载 SDK 库 (32 位和64位) 的 [问题](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/587) 。
- **C#** ：修复了 TranslationRecognizer、IntentRecognizer 和 Connection 对象的本机句柄的显式处理。
- **C#** ：修复了 ConversationTranscriber 对象的音频输入生存期管理。
- 修复了从简单短语识别意图时 `IntentRecognizer` 结果原因未正确设置的问题。
- 修复了未正确设置 `SpeechRecognitionEventArgs` 结果偏移量的问题。
- 修复了在打开 websocket 连接前 SDK 尝试发送网络消息的争用条件。 添加参与者时，针对 `TranslationRecognizer` 可重现。
- 修复了关键字识别器引擎中的内存泄漏。

**示例**
- **转到**：添加了 [语音识别](./get-started-speech-to-text.md?pivots=programming-language-go) 和 [自定义语音助手](./quickstarts/voice-assistants.md?pivots=programming-language-go)的快速入门。 在[此处](https://github.com/microsoft/cognitive-services-speech-sdk-go/tree/master/samples)查找示例代码。 
- **JavaScript**：添加了 [文本到语音](./get-started-text-to-speech.md?pivots=programming-language-javascript)、 [翻译](./get-started-speech-translation.md?pivots=programming-language-csharp&tabs=script)和 [意向识别](./quickstarts/intent-recognition.md?pivots=programming-language-javascript)的快速入门。
- 适用于[C \# ](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/uwp/keyword-recognizer)和[Java](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android/keyword-recognizer)的关键字识别示例 (Android) 。  

**COVID-19 缩减测试：** 由于过去几周一直在远程工作，我们无法像往常那样执行那么多手动验证测试。 我们没有做我们认为可能会造成任何破坏的任何更改，我们的自动化测试已全部通过。 如果错过了某些内容，请在 [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen)上告诉我们。<br>
请保重身体！

## <a name="speech-sdk-1110-2020-march-release"></a>语音 SDK 1.11.0：2020 年 3 月版
**新功能**
- Linux：增加了对 Red Hat Enterprise Linux (RHEL)/CentOS 7 x64 的支持，并提供了有关如何针对语音 SDK 配置系统的[说明](./how-to-configure-rhel-centos-7.md)。
- Linux：在 Linux ARM32 和 ARM64 上增加了对 .NET Core C# 的支持。 在[此处](./speech-sdk.md?tabs=linux)了解详细信息。 
- C#、C++：在 `ConversationTranscriptionResult` 中添加了 `UtteranceId`，这是在所有中间产物和最终的语音识别结果中保持一致的一个 ID。 请参阅适用于 [C#](/dotnet/api/microsoft.cognitiveservices.speech.transcription.conversationtranscriptionresult?preserve-view=true&view=azure-dotnet)、[C++](/cpp/cognitive-services/speech/transcription-conversationtranscriptionresult) 的详细信息。
- Python:增加了对 `Language ID` 的支持。 请 speech_sample 参阅 [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/python/console)存储库中的 py。
- Windows:在 Windows 平台上为所有 win32 控制台应用程序增加了对压缩的音频输入格式的支持。 有关详细信息，请参阅[此文](./how-to-use-codec-compressed-audio-input-streams.md)。 
- JavaScript：在 NodeJS 中支持语音合成（文本转语音）。 在[此处](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/text-to-speech)了解更多信息。 
- JavaScript：添加了新的 API，用于检查发送和接收的所有消息。 在[此处](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript)了解更多信息。 
        
**Bug 修复**
- C#、C++：修复了一个问题，因此 `SendMessageAsync` 现在以二进制类型发送二进制消息。 请参阅适用于 [C#](/dotnet/api/microsoft.cognitiveservices.speech.connection.sendmessageasync?view=azure-dotnet#Microsoft_CognitiveServices_Speech_Connection_SendMessageAsync_System_String_System_Byte___System_UInt32_)、[C++](/cpp/cognitive-services/speech/connection) 的详细信息。
- C#、C++：修复了当使用 `Connection MessageReceived` 事件时在 `Connection` 对象之前释放 `Recognizer` 可能会导致故障的问题。 请参阅适用于 [C#](/dotnet/api/microsoft.cognitiveservices.speech.connection.messagereceived?preserve-view=true&view=azure-dotnet)、[C++](/cpp/cognitive-services/speech/connection#messagereceived) 的详细信息。
- Android：麦克风的音频缓冲区大小从 800 毫秒减小到 100 毫秒，降低了延迟。
- Android：修复了 Android Studio 中 x86 Android 模拟器的一个[问题](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/563)。
- JavaScript：在 `fromSubscription` API 中增加了对中国的区域的支持。 有关详细信息，请参阅[此文](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?preserve-view=true&view=azure-node-latest#fromsubscription-string--string-)。 
- JavaScript：针对 NodeJS 中的连接失败添加了更多错误信息。
        
**示例**
- Unity：修复了意向识别公共示例（其中的 LUIS json 导入失败）。 有关详细信息，请参阅[此文](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/369)。
- Python:为 `Language ID` 添加了示例。 有关详细信息，请参阅[此文](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/python/console/speech_sample.py)。
    
**Covid19 缩减测试：** 由于过去几周一直在远程工作，我们无法像往常那样执行那么多手动的设备验证测试。 例如，我们无法在 Linux、iOS 和 macOS 上测试麦克风输入和扬声器输出。 我们没有做我们认为可能会破坏这些平台上的任何东西的任何更改，我们的自动化测试已全部通过。 如果我们遗漏了某些内容，请在 [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen) 上告诉我们。<br> 感谢你长久以来的支持。 与往常一样，请在 [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues?q=is%3Aissue+is%3Aopen) 或 [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/731) 上发布问题或反馈。<br>
请保重身体！

## <a name="speech-sdk-1100-2020-february-release"></a>语音 SDK 1.10.0：2020 年 2 月版

**新功能**

 - 添加了 Python 包以支持新的 3.8 版 Python。
 - Red Hat Enterprise Linux (RHEL)/CentOS 8 x64 支持（C++、C#、Java、Python）。
   > [!NOTE] 
   > 客户必须根据[这些说明](./how-to-configure-openssl-linux.md)配置 OpenSSL。
 - 针对 Debian 和 Ubuntu 的 Linux ARM32 支持。
 - DialogServiceConnector 现在支持 BotFrameworkConfig 上的可选 "机器人 ID" 参数。 此参数允许将多个直接连线语音机器人用于单个 Azure 语音资源。 如果未指定参数，则将使用 "直接线路语音通道配置") 页 (确定的默认机器人。
 - DialogServiceConnector 现在具有一个 SpeechActivityTemplate 属性。 直接 Line Speech 将使用此 JSON 字符串的内容来预填充到达直接语音机器人的所有活动中各种受支持的字段，包括自动生成的活动，以响应语音识别之类的事件。
 - TTS 现在使用订阅密钥进行身份验证，降低了创建合成器后第一个合成结果的第一个字节延迟。
 - 更新了 19 个区域设置的语音识别模型，平均单词错误率降低了 18.6%（es-ES、es-MX、fr-CA、fr-FR、it-IT、ja-JP、ko-KR、pt-BR、zh-CN、zh-HK、nb-NO、fi-FL、ru-RU、pl-PL、ca-ES、zh-TW、th-TH、pt-PT、tr-TR）。 新模型在多个领域提供了重大改进，其中包括听写、呼叫中心脚本和视频索引方案。

**Bug 修复**

 - 修复了在 JAVA API 中聊天听录器未正确等待的 Bug 
 - Android x86 仿真器修复（针对 Xamarin [GitHub 问题](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/363)）
 - 为 AudioConfig 添加了缺失的 (Get|Set)Property 方法
 - 修复了无法在连接失败时停止 audioDataStream 的 TTS Bug
 - 使用无区域的终结点会导致聊天翻译器出现 USP 故障
 - 现在，在通用 Windows 应用程序中生成 ID 时会使用适当的唯一 GUID 算法；它以前无意中默认为存根实现，这种实现通常会在大型交互集上造成冲突。
 
 **示例**
 
 - Unity 示例，可以将语音 SDK 与 [Unity 麦克风和推送模式流式处理](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/unity/from-unitymicrophone)结合使用

**其他更改**

 - [适用于 Linux 的 OpenSSL 配置文档已更新](./how-to-configure-openssl-linux.md)

## <a name="speech-sdk-190-2020-january-release"></a>语音 SDK 1.9.0：2020 年 1 月版

**新功能**

- 多设备对话：将多个设备连接到相同的语音或基于文本的对话，并选择性地转换在它们之间发送的消息。 请参阅 [本文](multi-device-conversation.md)中的详细信息。 
- 已为 aar 包添加关键字识别支持，并为 x86 和 x64 风格添加了支持。 
- Objective-C：已将 `SendMessage` 和 `SetMessageProperty` 方法添加到 `Connection` 对象。 参阅[此处](/objectivec/cognitive-services/speech/spxconnection)的文档。
- TTS C++ API 现在支持将 `std::wstring` 用作合成文本输入，这样，在将 wstring 传递给 SDK 之前，无需先将其转换为字符串。 请参阅[此处](/cpp/cognitive-services/speech/speechsynthesizer#speaktextasync)的详细信息。 
- C#：现在提供[语言 ID](./how-to-automatic-language-detection.md?pivots=programming-language-csharp) 和[源语言配置](./how-to-specify-source-language.md?pivots=programming-language-csharp)。
- JavaScript：已将一项功能添加到 `Connection` 对象，以便从语音服务以回调 `receivedServiceMessage` 的形式传递自定义消息。
- JavaScript：添加了对的支持， `FromHost API` 便于与本地容器和主权云一起使用。 参阅[此处](speech-container-howto.md)的文档。
- JavaScript：感谢 [orgads](https://github.com/orgads) 的贡献，我们现在可以采用 `NODE_TLS_REJECT_UNAUTHORIZED`。 请参阅[此处](https://github.com/microsoft/cognitive-services-speech-sdk-js/pull/75)的详细信息。

**重大更改**

- `OpenSSL` 已更新到版本 1.1.1b，并静态链接到适用于 Linux 的语音 SDK 核心库。 如果未在系统上的 `/usr/lib/ssl` 目录中安装收件箱 `OpenSSL`，这可能会造成中断。 请查看语音 SDK 的[文档](how-to-configure-openssl-linux.md)来解决此问题。
- 我们已将为 C# `WordLevelTimingResult.Offset` 返回的数据类型从 `int` 更改为 `long`，以便在语音数据超过 2 分钟时能够访问 `WordLevelTimingResults`。
- `PushAudioInputStream` 和 `PullAudioInputStream` 现在可以根据 `AudioStreamFormat`（创建这两个类时选择性地指定）将 wav 标头信息发送到语音服务。 现在，客户必须使用[支持的音频输入格式](how-to-use-audio-input-streams.md)。 任何其他格式将获取不理想的识别结果，也可能会导致其他问题。 

**Bug 修复**

- 请参阅上述“中断性变更”中的 `OpenSSL` 更新。 修复了 Linux 和 Java 中的间歇性崩溃和性能问题（负载较高时发生锁争用）。 
- Java:改进了高并发方案中的对象封闭。
- 重构了我们的 NuGet 包。 我们删除了 lib 文件夹下 `Microsoft.CognitiveServices.Speech.core.dll` 和 `Microsoft.CognitiveServices.Speech.extension.kws.dll` 的三个副本，使 NuGet 包更小、下载更快，并添加了编译某些 C++ 本机应用所需的标头。
- 修复了[此处](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp)的快速入门示例。 修复的问题是在 Linux、macOS、Windows 上退出但不显示“未找到麦克风”异常。
- 修复了长语音识别的 SDK 崩溃，其中包含了某些代码路径，如 [本示例](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/csharp/uwp/speechtotext-uwp)中所示。
- 修复了 Azure Web 应用环境中的 SDK 部署错误，并解决了[此客户问题](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/396)。
- 修复了在使用多 `<voice>` 标记或 `<audio>` 标记时出现的 TTS 错误以解决[此客户问题](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/433)。 
- 修复了从挂起状态恢复 SDK 时出现的 TTS 401 错误。
- JavaScript：感谢 [euirim](https://github.com/euirim) 的贡献，修复了音频数据的循环导入。 
- JavaScript：添加了设置服务属性的支持（版本 1.7 中已添加此项支持）。
- JavaScript：修复了以下问题：连接错误可能导致 websocket 重新连接尝试连续失败。

**示例**

- [此处](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/java/android/sdkdemo)添加了适用于 Android 的关键字识别示例。
- 为[此处](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/speech_synthesis_server_scenario_sample.cs)的服务器方案添加了 TTS 示例。
- 为 [c # 和 c + +](quickstarts/multi-device-conversation.md)添加了多设备对话快速入门。

**其他更改**

- 优化了 Android 上的 SDK 核心库大小。
- 1\.9.0 及更高版本中的 SDK 支持聊天听录器的语音签名版本字段中的 `int` 和 `string` 类型。

## <a name="speech-sdk-180-2019-november-release"></a>语音 SDK 1.8.0：2019-November 版本

**新功能**

- 添加了 `FromHost()` API，使其易于与本地容器和主权云一起使用。
- 为语音识别添加了自动源语言检测功能（在 Java 和 C++中）
- 为语音识别添加了 `SourceLanguageConfig` 对象，用于指定所需的源语言（在 Java 和 C++ 中）
- 通过 NuGet 和 Unity 包在 Windows (UWP)、Android 和 iOS 上添加了 `KeywordRecognizer` 支持
- 添加了远程对话 Java API，用于以异步批的方式进行对话听录。

**重大更改**

- 对话听录器功能已移到 `Microsoft.CognitiveServices.Speech.Transcription` 命名空间下。
- 会话 Transcriber 方法的各个部分会移到新 `Conversation` 类中。
- 放弃了对 32 位（ARMv7 和 x86）iOS 的支持

**Bug 修复**

- 针对以下问题进行了修复：如果在不使用有效语音服务订阅密钥的情况下使用本地 `KeywordRecognizer`，则会发生故障

**示例**

- `KeywordRecognizer` 的 Xamarin 示例
- `KeywordRecognizer` 的 Unity 示例
- 用于自动源语言检测的 C++ 和 Java 示例。

## <a name="speech-sdk-170-2019-september-release"></a>语音 SDK 1.7.0：2019-September 版本

**新功能**

- 添加了对通用 Windows 平台 (UWP)、Android 和 iOS 上的 Xamarin 的支持
- 添加了对 Unity 的 iOS 支持
- 添加了对 Android、iOS 和 Linux 上的 ALaw、Mulaw、FLAC 的 `Compressed` 输入支持
- 在 `Connection` 类中添加了 `SendMessageAsync`，用于向服务发送消息
- 在用于设置消息属性 `Connection` 类中添加了 `SetMessageProperty`
- TTS 为 Java（JRE 和 Android）、Python、Swift 和 Objective-C 添加了绑定
- TTS 添加了对 macOS、iOS 和 Android 的播放支持。
- 为 TTS 添加了“字边界”信息。

**Bug 修复**

- 修复了 Unity 2019 for Android 上的 IL2CPP 生成问题
- 修复了 wav 文件输入中格式错误的标头被错误处理的问题
- 修复了 UUID 在某些连接属性中不唯一的问题
- 修复了一些有关 Swift 绑定中存在为 Null 性说明符的警告（可能需要小的代码更改）
- 修复了一个 Bug，该 Bug 导致 websocket 连接在网络负载下被意外关闭
- 修复了 Android 上的一个问题，该问题有时候导致 `DialogServiceConnector` 使用的印象 ID 重复
- 改进了进行多轮交互时连接的稳定性，以及它们发生在 `DialogServiceConnector` 上时（通过 `Canceled` 事件）对故障进行的报告
- 现在，`DialogServiceConnector` 会话开始时会正确提供事件，包括在活动 `StartKeywordRecognitionAsync()` 期间调用 `ListenOnceAsync()` 的时候
- 解决了与收到的 `DialogServiceConnector` 活动相关联的崩溃

**示例**

- Xamarin 的快速入门
- 使用 Linux ARM64 信息更新了 CPP 快速入门
- 使用 iOS 信息更新了 Unity 快速入门

## <a name="speech-sdk-160-2019-june-release"></a>语音 SDK 1.6.0：2019 年 6 月发布

**示例**

- UWP 和 Unity 上的文本转语音快速入门示例
- iOS 上的 Swift 快速入门示例
- 语音和意向识别及翻译 Unity 示例
- `DialogServiceConnector` 的更新的快速入门示例

**改进 / 更改**

- 对话命名空间：
  - `SpeechBotConnector` 已重名为 `DialogServiceConnector`
  - `BotConfig` 已重名为 `DialogServiceConfig`
  - `BotConfig::FromChannelSecret()` 已重新映射到 `DialogServiceConfig::FromBotSecret()`
  - 重命名后，仍旧支持所有现有的 Direct Line 语音客户端
- 更新了 TTS REST 适配器以支持代理和持久连接
- 改写了传递无效区域时出现的错误消息
- Swift/Objective-C：
  - 改进了错误报告：可能导致出错的方法现在有两个版本：一个版本公开用于错误处理的 `NSError` 对象，另一个版本引发异常。 前者向 Swift 公开。 此项更改需要对现有的 Swift 代码进行改编。
  - 改进了事件处理

**Bug 修复**

- 针对 TTS 进行了以下问题的修复：`SpeakTextAsync` 不等到音频完成渲染就会提前返回
- 修复了 C# 中的封送字符串，以支持完整语言
- 修复了示例中的 .NET Core 应用问题，以使用 net461 目标框架加载核心库
- 修复了示例中的偶发性问题，以将本机库部署到输出文件夹
- 修复了 Web 套接字可靠关闭的问题
- 在 Linux 上的重负载下打开连接时修复可能的故障
- 修复了 macOS 框架捆绑包中缺少元数据的问题
- 修复了 Windows 上的 `pip install --user` 问题

## <a name="speech-sdk-151"></a>语音 SDK 1.5.1

这是一个 Bug 修复版本，只影响本机/托管 SDK。 它不影响 SDK 的 JavaScript 版本。

**Bug 修复**

- 修复了 FromSubscription 与对话听录一起使用时出现的问题。
- 修复关键字发现中的 bug 以获取语音助手。

## <a name="speech-sdk-150-2019-may-release"></a>语音 SDK 1.5.0：2019 年 5 月发布

**新功能**

- 关键字发现 (KWS) 现在适用于 Windows 和 Linux。 KWS 功能可能适用于任何麦克风类型，不过，官方的 KWS 支持目前仅限于 Azure Kinect DK 硬件或语音设备 SDK 中的麦克风阵列。
- 短语提示功能通过 SDK 提供。 有关详细信息，请参阅[此文](./get-started-speech-to-text.md)。
- 对话听录功能通过 SDK 提供。 [请参阅](./conversation-transcription.md)。
- 添加对使用语音助手的语音助手的支持。

**示例**

- 添加了 SDK 支持的新功能或新服务的示例。

**改进 / 更改**

- 添加了各种识别器属性，以调整服务行为或服务结果（例如屏蔽猥亵内容等）。
- 现在，即使你创建了识别器 `FromEndpoint`，也能通过标准配置属性来配置识别器。
- Objective-C：已将 `OutputFormat` 属性添加到 `SPXSpeechConfiguration`。
- SDK 现在支持将 Debian 9 用作 Linux 分发版。

**Bug 修复**

- 修复了文本转语音中过早销毁讲述人资源的问题。

## <a name="speech-sdk-142"></a>语音 SDK 1.4.2

这是一个 Bug 修复版本，只影响本机/托管 SDK。 它不影响 SDK 的 JavaScript 版本。

## <a name="speech-sdk-141"></a>语音 SDK 1.4.1

这是一个仅限 JavaScript 的版本。 未增加任何功能。 进行了以下修复：

- 阻止 Web 包加载 https-proxy-agent。

## <a name="speech-sdk-140-2019-april-release"></a>语音 SDK 1.4.0：2019 年 4 月发布

**新功能**

- SDK 现在支持 beta 版本的文本转语音服务。 Windows 和 Linux 桌面版中的 C++ 和 C# 支持该版本。 有关详细信息，请查看[文本转语音概述](text-to-speech.md#get-started)。
- SDK 现在支持将 MP3 和 Opus/OGG 音频文件用作流输入文件。 此功能只能通过 C++ 和 C# 在 Linux 上使用，目前为 beta 版（更多详细信息请参见[此处](how-to-use-codec-compressed-audio-input-streams.md)）。
- 适用于 Java、.NET Core C++和 Objective-C 的语音 SDK 已获得 macOS 支持。 macOS 的 Objective-C 支持目前以 beta 版提供。
- iOS：适用于 iOS (Objective-C) 的语音 SDK 现在也已作为 CocoaPod 发布。
- JavaScript：支持将非默认麦克风用作输入设备。
- JavaScript：Node.js 的代理支持。

**示例**

- 添加了有关在 macOS 上的 C++ 和 Objective-C 中使用语音 SDK 的示例。
- 已添加用于演示文本转语音服务用法的示例。

**改进 / 更改**

- Python:现在会通过 `properties` 属性公开识别结果的附加属性。
- 若要获得更多开发和调试支持，可将 SDK 日志记录和诊断信息重定向到日志文件中（更多详细信息请参见[此处](how-to-use-logging.md)）。
- JavaScript：提高了音频处理性能。

**Bug 修复**

- Mac/iOS：修复了未能与语音服务建立连接时导致长时间等待的 bug。
- Python：改进了 Python 回调中的参数的错误处理。
- JavaScript：修复了 RequestSession 中结束的语音的错误状态报告。

## <a name="speech-sdk-131-2019-february-refresh"></a>语音 SDK 1.3.1：2019 年 2 月刷新

这是一个 Bug 修复版本，只影响本机/托管 SDK。 它不影响 SDK 的 JavaScript 版本。

**Bug 修复**

- 修复了使用麦克风输入时出现的内存泄漏问题。 基于流的输入或文件输入不受影响。

## <a name="speech-sdk-130-2019-february-release"></a>语音 SDK 1.3.0：2019 年 2 月版本

**新功能**

- 语音 SDK 支持通过 `AudioConfig` 类来选择输入麦克风。 这样，便可以将音频数据从非默认麦克风流式传输到语音服务。 有关详细信息，请参阅介绍[音频输入设备选择](how-to-select-audio-input-devices.md)的文档。 此功能在 JavaScript 中尚不可用。
- 语音 SDK 目前在 beta 版本中支持 Unity。 请通过 [GitHub 示例存储库](https://aka.ms/csspeech/samples)中的问题部分来提供反馈。 此版本支持在 Windows x86 和 x64（桌面或通用 Windows 平台应用程序）以及 Android（ARM32/64，x86）上使用 Unity。 [Unity 快速入门](./get-started-speech-to-text.md?pivots=programming-language-csharp&tabs=unity)中提供了更多信息。
- 不再需要 `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` 文件（在以前的版本中提供）。 此功能现在集成到核心 SDK 中。

**示例**

[示例存储库](https://aka.ms/csspeech/samples)中提供了以下新内容：

- `AudioConfig.FromMicrophoneInput` 的其他示例。
- 有关意向识别和翻译的更多 Python 示例。
- 有关在 iOS 中使用 `Connection` 对象的更多示例。
- 有关具有音频输出的翻译的更多 Java 示例。
- 有关使用[批量听录 REST API](batch-transcription.md) 的新示例。

**改进 / 更改**

- Python
  - 改进了 `SpeechConfig` 中的参数验证和错误消息。
  - 添加 `Connection` 对象的支持。
  - 支持 Windows 上的 32 位 Python (x86)。
  - 适用于 Python 的语音 SDK 已完成 beta 版本。
- iOS
  - SDK 现在是基于 iOS SDK 版本 12.1 构建的。
  - SDK 现在支持 iOS 版本 9.2 及更高版本。
  - 改进了参考文档并修复了多个属性名称。
- Javascript
  - 添加 `Connection` 对象的支持。
  - 添加了捆绑的 JavaScript 的类型定义文件
  - 首次支持并实现了短语提示。
  - 随服务 JSON 返回属性集合以用于识别
- Windows DLL 现在包含一个版本资源。
- 如果创建识别器 `FromEndpoint`，则可将参数直接添加到终结点 URL。 使用 `FromEndpoint` 时，无法通过标准的配置属性来配置识别器。

**Bug 修复**

- 过去无法正确处理空的代理用户名和代理密码。 在此版本中，如果将代理用户名和代理密码设置为空字符串，则在连接到代理时不会提交它们。
- 对于某些语言&nbsp;/ 环境，由 SDK 创建的 SessionId 并非总是真正随机的。 已添加了随机生成器初始化来修复此问题。
- 改进了对授权令牌的处理。 如果希望使用授权令牌，请在 `SpeechConfig` 中进行指定并将订阅密钥保留为空。 然后，像往常一样创建识别器。
- 过去，在某些情况下，`Connection` 对象不能正确释放。 现在已修复此问题。
- JavaScript 示例已修复，在 Safari 上也支持用于翻译合成的音频输出。

## <a name="speech-sdk-121"></a>语音 SDK 1.2.1

这是一个仅限 JavaScript 的版本。 未增加任何功能。 进行了以下修复：

- 在 turn.end 处触发流结束，在 speech.end 处不触发。
- 修复了音频泵中在当前发送失败时不安排下一次发送的 bug。
- 修复了使用身份验证令牌进行的连续识别。
- 对不同识别器 / 终结点的 bug 修复。
- 文档改进。

## <a name="speech-sdk-120-2018-december-release"></a>语音 SDK 1.2.0：2018 年 12 月版本

**新功能**

- Python
  - 此版本支持 Python 的 Beta 版本（3.5 及更高版本）。 有关详细信息，请参阅此文](quickstart-python.md)。
- Javascript
  - 适用于 JavaScript 的语音 SDK 已开放了源代码。 [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js) 上提供了源代码。
  - 我们现在支持 Node.js，可以在[此处](./get-started-speech-to-text.md)找到详细信息。
  - 已删除了对音频会话的长度限制，将自动在后台进行重新连接。
- （属于`Connection` 对象）的父级。
  - 可以从 `Recognizer` 中访问 `Connection` 对象。 此对象允许你显式启动服务连接并订阅连接事件和断开连接事件。
    （此功能在 JavaScript 和 Python 中尚不可用。）
- 支持 Ubuntu 18.04。
- Android
  - 在生成 APK 期间启用了 ProGuard 支持。

**改进**

- 改进了内部线程的使用，减少了线程、锁和互斥的数量。
- 改进了错误报告 / 信息。 在某些情况下，错误消息没有完全传播出去。
- 更新了 JavaScript 中的开发依赖项来使用最新模块。

**Bug 修复**

- 修复了由于 `RecognizeAsync` 中的类型不匹配导致的内存泄漏。
- 在某些情况下，异常会被泄露。
- 修复了翻译事件参数中的内存泄漏。
- 修复了长时间运行的会话中与重新连接相关的锁定问题。
- 修复了可能会导致失败的翻译缺少最终结果的问题。
- C#：如果在主线程中没有等待 `async` 操作，则可能会在异步任务完成之前释放识别器。
- Java:修复了导致 Java VM 故障的一个问题。
- Objective-C：修复了枚举映射；之前返回 RecognizedIntent 而非 `RecognizingIntent`。
- JavaScript：在 `SpeechConfig` 中将默认输出格式设置为“simple”。
- JavaScript：删除了 JavaScript 和其他语言中配置对象中的属性之间的不一致。

**示例**

- 更新并修复了几个示例（例如，翻译的输出语音，等等）。
- 在[示例存储库](https://aka.ms/csspeech/samples)中添加了 Node.js 示例。

## <a name="speech-sdk-110"></a>语音 SDK 1.1.0

**新功能**

- 对 Android x86/x64 的支持。
- 代理支持：在 `SpeechConfig` 对象中，现在可以调用某个函数来设置代理信息（主机名、端口、用户名和密码）。 此功能在 iOS 上尚不可用。
- 改进了错误代码和消息。 如果识别返回了错误，这在过去会将 `Reason`（在已取消事件中）或 `CancellationDetails`（在识别结果中）设置为 `Error`。 取消的事件现在包含两个附加的成员：`ErrorCode` 和 `ErrorDetails`。 如果服务器随所报告的错误返回了附加的错误信息，则现在将在新成员中提供该信息。

**改进**

- 在识别器配置中添加了附加的验证并添加了附加的错误消息。
- 改进了对音频文件中间的长时间静默的处理。
- NuGet 包：对于 .NET Framework 项目，它阻止使用 AnyCPU 配置进行构建。

**Bug 修复**

- 修复了在识别器中发现的几处异常。 此外，还会捕获异常并将其转换为 `Canceled` 事件。
- 修复了属性管理中的内存泄漏。
- 修复了音频输入文件可能会导致识别器发生故障的 bug。
- 修复了在会话停止事件后无法检索事件的 bug。
- 修复了线程中的一些争用条件。
- 修复了可能会导致故障的 iOS 兼容性问题。
- 改进了对 Android 麦克风的支持的稳定性。
- 修复了 JavaScript 中的识别器将忽略识别语言的 bug。
- 修复了阻止在 JavaScript 中设置 `EndpointId`（在某些情况下）的 bug。
- 更改了 JavaScript 中的 AddIntent 中的参数顺序，并添加了缺少的 `AddIntent` JavaScript 签名。

**示例**

- 在[示例存储库](https://aka.ms/csspeech/samples)中添加了拉取和推送流用法的 C++ 和 C# 示例。

## <a name="speech-sdk-101"></a>语音 SDK 1.0.1

可靠性改进和 bug 修复：

- 修复了处理识别器时由于争用条件而导致的潜在严重错误
- 修复了在未设置属性的情况下可能发生的错误。
- 添加了其他错误检查和参数检查。
- Objective-C：修复了 NSString 中名称替代而引起的潜在严重错误。
- Objective-C：调整了 API 的可见性
- JavaScript：针对事件及其有效负载进行了修复。
- 文档改进。

在[示例存储库](https://aka.ms/csspeech/samples)中已添加了适用于 JavaScript 的新示例。

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>认知服务语音 SDK 1.0.0：2018 年 9 月版本

**新功能**

- 支持 iOS 中的 Objective-C。 请查看[适用于 iOS 的 Objective-C 快速入门](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/objectivec/ios/from-microphone)。
- 支持浏览器中的 JavaScript。 请查看 [JavaScript 快速入门](./get-started-speech-to-text.md)。

**重大更改**

- 该版本中推出了大量重大更改。
  有关详细信息，请查看[此页](https://aka.ms/csspeech/breakingchanges_1_0_0)。

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>认知服务语音 SDK 0.6.0：2018 年 8 月版本

**新功能**

- 使用语音 SDK 生成的 UWP 应用现在可以通过 Windows 应用认证工具包 (WACK)。
  请查看 [UWP 快速入门](./get-started-speech-to-text.md?pivots=programming-language-chsarp&tabs=uwp)。
- 在 Linux (Ubuntu 16.04 x64) 上支持 .NET Standard 2.0。
- 试验：在 Windows (64-bit) 和 Linux (Ubuntu 16.04 x64) 上支持 Java 8。
  请查看 [Java 运行时环境快速入门](./get-started-speech-to-text.md?pivots=programming-language-java&tabs=jre)。

**功能性更改**

- 公开了关于连接错误的更多错误详细信息。

**重大更改**

- 在 Java (Android) 中，`SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` 函数不再需要 path 参数。 现在，在所有受支持的平台上都会自动检测路径。
- 在 Java 和 C# 中，属性 `EndpointUrl` 的 get 访问器已被删除。

**Bug 修复**

- 在 Java 中，目前在翻译识别器上实现了音频合成结果。
- 修复了一个 bug，该 bug 可能会导致非活动线程和更多的已打开且未使用的套接字。
- 修复了一个问题，该问题导致在传输过程中无法终止长时间运行的识别。
- 修复了识别器关闭过程中的一个争用条件。

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>认知服务语音 SDK 0.5.0：2018 年 7 月版本

**新功能**

- 支持 Android 平台（API 23：Android 6.0 Marshmallow 或更高版本）。 查看 [Android 快速入门](./get-started-speech-to-text.md?pivots=programming-language-java&tabs=android)。
- 在 Windows 上支持 .NET Standard 2.0。 查看 [.NET Core 快速入门](./get-started-speech-to-text.md?pivots=programming-language-csharp&tabs=dotnetcore)。
- 试验：在 Windows 上支持 UWP（版本 1709 或更高版本）。
  - 请查看 [UWP 快速入门](./get-started-speech-to-text.md?pivots=programming-language-csharp&tabs=uwp)。
  - 注意：使用语音 SDK 生成的 UWP 应用尚未通过 Windows 应用认证工具包 (WACK)。
- 通过自动重新连接支持识别功能长时间运行。

**功能性更改**

- `StartContinuousRecognitionAsync()` 支持识别功能长时间运行。
- 识别结果包含更多字段。 这些字段是识别文本的音频开始和持续时间（时钟周期数）的偏移量和表示识别状态的其他值（例如 `InitialSilenceTimeout`、`InitialBabbleTimeout`）。
- 支持 AuthorizationToken 用于创建工厂实例。

**重大更改**

- 识别事件：`NoMatch` 事件类型已合并到 `Error` 事件中。
- C# 中的 SpeechOutputFormat 已重命名为 `OutputFormat` 以与 C++ 保持一致。
- `AudioInputStream` 接口的某些方法的返回类型略有更改：
  - 在 Java 中，`read` 方法现返回 `long` 而不是 `int`。
  - 在 C# 中，`Read` 方法现返回 `uint` 而不是 `int`。
  - 在 C++ 中，`Read` 和 `GetFormat` 方法现返回 `size_t` 而不是 `int`。
- C++：音频输入流的实例现在只能作为 `shared_ptr` 传递。

**Bug 修复**

- 修复了 `RecognizeAsync()` 超时时结果中的错误返回值。
- 删除了 Windows 上媒体基础库的依赖项。 SDK 现在使用 Core Audio API。
- 文档修复：添加了一个[区域](regions.md)页来描述支持的区域。

**已知问题**

- 适用于 Android 的语音 SDK 不报告用于翻译的语音合成结果。 此问题将在下一版本中修复。

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>认知服务语音 SDK 0.4.0：2018 年 6 月版本

**功能性更改**

- AudioInputStream

  一种现可将流用作音频源的识别器。 有关详细信息，请参阅相关[操作说明指南](how-to-use-audio-input-streams.md)。

- 详细输出格式

  创建 `SpeechRecognizer` 时，可请求 `Detailed` 或 `Simple` 输出格式。 `DetailedSpeechRecognitionResult` 包含置信度分数、识别的文本、原始词法形式、标准化形式和已屏蔽不当字词的标准化形式。

**重大更改**

- 将 C# 中的 `SpeechRecognitionResult.RecognizedText` 更改为 `SpeechRecognitionResult.Text`。

**Bug 修复**

- 修复了关闭期间 USP 层中可能出现的回叫问题。
- 如果识别器使用了音频输入文件，则它在文件句柄上停留的时间将超过必要时间。
- 删除了消息泵和识别器之间的多个死锁。
- 在服务的响应超时后触发 `NoMatch` 结果。
- Windows 上的媒体基础库为延迟加载。 此库仅用于麦克风输入。
- 音频数据的上传速度约限制为原始音频速度的两倍。
- 在 Windows 上，C# .NET 程序集现在为强命名。
- 文档修复：`Region` 是创建识别器所必需的信息。

已添加更多示例，还将持续更新。 有关最新的示例集，请参阅[语音 SDK 示例 GitHub 存储库](https://aka.ms/csspeech/samples)。

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>认知服务语音 0.2.12733：2018 年 5 月版本

此版本是认知服务语音 SDK 的第一个公共预览版本。