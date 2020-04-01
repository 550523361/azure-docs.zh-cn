---
title: 有关视频索引器的常见问题解答 - Azure
titleSuffix: Azure Media Services
description: 本文回答了有关 Azure 媒体服务视频索引器的常见问题。
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 03/30/2020
ms.author: juliako
ms.openlocfilehash: dd41596b6631bb63e1625325f8bec065b43881cd
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421396"
---
# <a name="video-indexer-frequently-asked-questions"></a>视频索引器常见问题

本文解答了有关视频索引器的常见问题。

## <a name="general-questions"></a>一般问题

### <a name="what-is-video-indexer"></a>什么是视频索引器？

视频索引器是 Microsoft Azure 媒体服务中的一项人工智能服务。 视频索引器能够协调多个机器学习模型，这些模型可让你从视频中提取深入见解。 为了提供高级且准确的见解，视频索引器将使用多个视频通道：音频、语音和视觉对象。 可以通过多种方式来使用视频索引器的见解，例如改进内容可发现性和可访问性、创建新的盈利机会，或构建利用见解的新体验。 视频索引器提供基于 Web 的界面用于测试、配置和自定义帐户中的模型。 开发人员可以使用基于 REST 的 API 将视频索引器集成到生产系统中。 

### <a name="what-can-i-do-with-video-indexer"></a>我可以使用视频索引器进行哪些操作？

视频索引器可以对媒体文件执行的一些操作包括：

* 识别和提取语音，并识别发言人。
* 识别和提取视频中屏幕上的文本。
* 检测视频文件中的对象。
* 从视频中的音轨和屏幕文本中识别品牌（例如：微软）。
* 从名人数据库和用户定义的人脸数据库中检测和识别人脸。
* 提取音频和视频内容中讨论的但不一定提及的主题。
* 基于音轨创建隐藏式字幕或副标题。

有关详细信息和更多的视频索引器功能，请参阅[概述](video-indexer-overview.md)。

### <a name="how-do-i-get-started-with-video-indexer"></a>如何快速入门视频索引器？

视频索引器附带的免费试用版在基于 Web 的界面中提供 600 分钟的试用，通过 API 提供 2,400 分钟的试用。 你可以[登录到视频索引器的基于 Web 的界面](https://www.videoindexer.ai/)，然后使用任何 Web 标识自行试用，而无需设置 Azure 订阅。 

若要将大量视频和音频文件编入索引，可将视频索引器连接到付费的 Microsoft Azure 订阅。 可在[定价页](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/)上找到定价详细信息。

可在[入门](video-indexer-get-started.md)中找到更多详细信息。

### <a name="do-i-need-coding-skills-to-use-video-indexer"></a>我是否需要编码技能才能使用视频索引器？

可以使用视频索引器的基于 Web 的界面来评估、配置和管理帐户，而无需编写任何代码。****  准备好开发更复杂的应用程序时，可以使用[视频索引器 API](https://api-portal.videoindexer.ai/) 将视频索引器集成到自己的应用程序、网站或[使用 Azure 逻辑应用或 Azure Functions 等无服务器技术的自定义工作流](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/)中。

### <a name="do-i-need-machine-learning-skills-to-use-video-indexer"></a>我是否需要机器学习技能才能使用视频索引器？

否。视频索引器允许将多个机器学习模型集成到一个管道中。 通过视频索引器为视频或音频文件编制索引会检索在一条共享时间线上提取的整组见解，客户无需具备任何机器学习技能或了解算法。

### <a name="what-media-formats-does-video-indexer-support"></a>视频索引器支持哪些媒体格式？

视频索引器支持最常见的媒体格式。 有关详细信息，请参阅[Azure 媒体编码器标准格式](https://docs.microsoft.com/azure/media-services/latest/media-encoder-standard-formats)列表。

### <a name="how-to-do-i-upload-a-media-into-video-indexer"></a>如何将媒体上传到视频索引器？

在视频索引器的基于 Web 的门户中，可以使用文件上传对话框上传媒体文件，或者指向直接托管源文件的 URL（参阅[示例](https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/Ignite-short.mp4)）。 使用 iFrame 或嵌入代码托管媒体内容的任何 URL 不起作用（参阅[示例](https://www.videoindexer.ai/accounts/7e1282e8-083c-46ab-8c20-84cae3dc289d/videos/5cfa29e152/?t=4.11)）。 视频索引器 API 要求通过 URL 或字节数组指定输入文件。 使用 API 通过 URL 上传的大小限制为 10 GB，但没有时长限制。 有关详细信息，请参阅[操作方法指南](https://docs.microsoft.com/azure/media-services/video-indexer/upload-index-videos)。

### <a name="how-long-does-it-take-video-indexer-to-extract-insights-from-media"></a>视频索引器从媒体中提取见解需要多长时间？

使用视频索引器 API 和视频索引器基于 Web 的界面为视频或音频文件编制索引所需的时间取决于多个参数，例如文件长度和质量、文件中找到的见解数、可用的[预留单位](https://docs.microsoft.com/azure/media-services/previous/media-services-scale-media-processing-overview)数以及是否启用了[流式处理终结点](https://docs.microsoft.com/azure/media-services/previous/media-services-streaming-endpoints-overview)。 我们建议使用自己的内容运行多个测试文件，并根据平均数获取更好的想法。

### <a name="can-i-create-customized-workflows-to-automate-processes-with-video-indexer"></a>是否可以创建自定义工作流来自动化视频索引器的流程？

是的，可将视频索引器集成到逻辑应用、Flow 和 [Azure Functions](https://azure.microsoft.com/services/functions/) 等无服务器技术。 在[此处](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/)可以找到有关适用于视频索引器的[逻辑应用](https://azure.microsoft.com/services/logic-apps/)和 [Flow](https://flow.microsoft.com/en-us/) 连接器的更多详细信息。 

### <a name="in-which-azure-regions-is-video-indexer-available"></a>视频索引器在哪些 Azure 区域是可用的？

可在[区域](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all)页上查看提供视频索引器的 Azure 区域。

### <a name="can-i-customize-video-indexer-models-for-my-specific-use-case"></a>我可以为特定用例自定义视频索引器模型吗？ 

是的。 在视频索引器中，您可以自定义一些可用的模型，以更好地满足您的需求。 

例如，我们的 Person 模型支持开箱即用的 1，000，000 张名人识别人脸，但您也可以训练它识别该数据库中不的其他人脸。 

有关详细信息，请参阅有关自定义[人员](customize-person-model-overview.md)、[品牌](customize-brands-model-overview.md)和[语言](customize-language-model-overview.md)模型的文章。 

### <a name="what-is-the-sla-for-video-indexer"></a>什么是适用于视频索引器的 SLA？

Azure 媒体服务的 SLA 涵盖了视频索引器，可在 [SLA](https://azure.microsoft.com/support/legal/sla/media-services/v1_2/) 页上找到。 该 SLA 仅适用于视频索引器付费帐户，而不适用于免费试用版。

## <a name="privacy-questions"></a>隐私问题

### <a name="are-video-and-audio-files-indexed-by-video-indexer-stored"></a>你们是否会存储由视频索引器编制索引的音频和视频文件？

是的，除非使用视频索引器网站或 API 从视频索引器中删除该文件，否则将存储视频和音频文件。 对于免费试用版，编制索引的视频和音频文件将存储在美国东部 Azure 区域。 否则，视频和音频文件将存储在 Azure 订阅的存储帐户中。

### <a name="can-i-delete-my-files-that-are-stored-in-video-indexer-portal"></a>可以删除存储在视频索引器门户中的文件吗？

是的，始终可以删除视频和音频文件，以及视频索引器从中提取的任何元数据和见解。 从视频索引器中删除某个文件后，该文件及其元数据和见解将从视频索引器中永久删除。 但是，如果你在 Azure 存储中实施了自己的备份解决方案，则该文件将保留在 Azure 存储中。

### <a name="can-i-control-user-access-to-my-video-indexer-account"></a>是否可以控制用户对我的视频索引器帐户的访问？

是的，只有帐户管理员可以邀请和取消邀请某人加入其帐户，以及分配编辑特权和只读访问权限。

### <a name="who-has-access-to-my-video-and-audio-files-that-have-been-indexed-andor-stored-by-video-indexer-and-the-metadata-and-insights-that-were-extracted"></a>谁有权访问我的已由视频索引器编制索引和/或存储的视频和音频文件，以及提取的元数据和见解？

获得该视频或音频内容及其见解的链接的任何人都可以访问将隐私设置设为公开的视频或音频内容。 只有受邀加入视频或音频内容帐户的用户才能访问将隐私设置设为私密的视频或音频内容。 内容的隐私设置也将应用到视频索引器提取的元数据和见解。 可以在上传视频或音频文件时指定隐私设置。 也可以在编制索引后更改隐私设置。

### <a name="what-access-does-microsoft-have-to-my-video-or-audio-files-that-have-been-indexed-andor-stored-by-video-indexer-and-the-metadata-and-insights-that-were-extracted"></a>Microsoft 对我的已由视频索引器编制索引和/或存储的视频或音频文件，以及提取的元数据和见解拥有哪些访问权限？

根据 [Azure 联机服务条款](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) (OST)，你的内容完全由你拥有，Microsoft 只会根据 OST 和 Microsoft 隐私声明访问你的内容，以及视频索引器从你的内容中提取的元数据和见解。

### <a name="are-the-custom-models-that-i-build-in-my-video-indexer-account-available-to-other-accounts"></a>我在视频索引器帐户中生成的自定义模型是否可用于其他帐户？

 不可以，在你的帐户中创建的自定义模型不可用于任何其他帐户。 视频索引器目前允许在帐户中生成自定义[品牌](customize-brands-model-overview.md)、[语言](customize-language-model-overview.md)和[人员](customize-person-model-overview.md)模型。 这些模型仅可用于在其中创建这些模型的帐户。
  
### <a name="is-the-content-indexed-by-video-indexer-kept-within-the-azure-region-where-i-am-using-video-indexer"></a>由视频索引器编入索引的内容是否保留在我使用视频索引器的 Azure 区域内？

是的，内容及其见解将保留在 Azure 区域中，除非在 Azure 订阅中手动配置使用多个 Azure 区域。 

### <a name="what-is-the-privacy-policy-for-video-indexer"></a>视频索引器的隐私政策是什么？

视频索引器在 [Microsoft 隐私声明](https://privacy.microsoft.com/privacystatement)中有所介绍。 隐私声明解释了 Microsoft 处理的个人数据、Microsoft 的处理方式，以及 Microsoft 出于哪些目的处理个人数据。 若要详细了解隐私保护，请访问 [Microsoft 信任中心](https://www.microsoft.com/trustcenter)。

### <a name="what-certifications-does-video-indexer-have"></a>视频索引器通过了哪些认证？

视频索引器目前已通过 SOC 认证。 若要查看视频索引器通过的认证，请参阅 [Microsoft 信任中心](https://www.microsoft.com/trustcenter/compliance/complianceofferings?product=Azure)。

### <a name="what-is-the-difference-between-private-and-public-videos"></a>私人视频和公共视频有什么区别？ 

将视频上载到视频索引器时，您可以从两种隐私设置中进行选择：私有设置和公共设置。 任何人（包括匿名和身份不明的用户）均可访问公共视频。 私人帐户成员仅限于帐户成员。 

### <a name="i-tried-to-upload-a-video-as-public-and-it-was-flagged-for-inappropriate-or-offensive-content-what-does-that-mean"></a>我试图上传一个视频作为公共，它被标记为不当或冒犯性的内容，这意味着什么？ 

将视频上载到视频索引器时，算法和模型会自动进行内容分析，以确保不会公开显示任何不适当的内容。 如果发现视频可疑，即包含显式内容，则无法将其设置为公共内容。 但是，帐户成员仍可以以专用视频形式访问它（查看该视频、下载见解和提取的工件，并执行可供帐户成员使用的其他操作）。   

为了设置视频供公众访问，您可以： 

* 构建您自己的界面层（如应用或网站），并使用它与视频索引器服务进行交互。 这样，视频在我们的门户中保持私密性，您的用户可以通过您的界面与之交互。 例如，您仍然可以获取见解或允许在您自己的界面中观看视频。 
* 要求对内容进行人工审阅，这将导致删除限制，前提是内容不明确。 

    如果用户直接使用视频索引器网站作为接口层，并且用于公共（未经身份验证的）查看，则可以探索此选项。 

## <a name="api-questions"></a>API 问题

### <a name="what-apis-does-video-indexer-offer"></a>视频索引器提供哪些 API？

使用视频索引器 API 可以编制索引、提取元数据、管理资产、翻译、嵌入、自定义模型，等等。 若要查找有关使用视频索引器 API 的更多详细信息，请参阅[视频索引器开发人员门户](https://api-portal.videoindexer.ai/)。

### <a name="what-client-sdks-does-video-indexer-offer"></a>视频索引器提供哪些客户端 SDK？

目前不提供客户端 SDK。 视频索引器团队正在努力开发 SDK，并计划在不久后推出。

### <a name="how-do-i-get-started-with-video-indexers-api"></a>如何快速入门视频索引器的 API？

请遵循[教程：视频索引器 API 入门](video-indexer-use-apis.md)进行操作。

### <a name="what-is-the-difference-between-the-video-indexer-api-and-the-azure-media-service-v3-api"></a>视频索引器 API 与 Azure 媒体服务 v3 API 之间有什么区别？

目前，视频索引器 API 和 Azure 媒体服务 v3 API 提供的功能存在一定的重叠。 在[此处](compare-video-indexer-with-media-services-presets.md)可以找到有关如何比较这两个服务的详细信息。

### <a name="what-is-an-api-access-token-and-why-do-i-need-it"></a>什么是 API 访问令牌，为何需要使用它？

视频索引器 API 包含授权 API 和操作 API。 授权 API 包含提供访问令牌的调用。 对操作 API 的每个调用应该与匹配调用授权范围的访问令牌相关联。

出于安全目的，需要访问令牌来使用视频索引器 API。 这可确保任何调用都来自你或拥有帐户访问权限的用户。 

### <a name="what-is-the-difference-between-account-access-token-user-access-token-and-video-access-token"></a>帐户访问令牌、用户访问令牌和视频访问令牌之间的区别是什么？

* 帐户级别 – 使用帐户级访问令牌可在帐户级别或视频级别执行操作。 例如，上传视频、列出所有视频、获取视频见解。
* 用户级别 - 使用用户级访问令牌可在用户级别执行操作。 例如，获取关联的帐户。
* 视频级别 – 使用视频级访问令牌可对特定的视频执行操作。 例如，获取视频见解、下载隐藏式字幕、获取小组件，等等。

### <a name="how-often-do-i-need-to-get-a-new-access-token-when-do-access-tokens-expire"></a>我需要多久获取一次新的访问令牌？ 访问令牌何时过期？

访问令牌每隔一小时会过期，因此需要每隔一小时生成一次新的访问令牌。 

### <a name="what-are-the-login-options-to-video-indexer-developer-portal"></a>视频索引器开发人员门户的登录选项是什么？

您可以使用 Azure AD、微软帐户、Google 帐户或 Facebook 帐户登录。 

使用标识提供商注册电子邮件帐户后，您不能向其他标识提供商使用此电子邮件帐户。

## <a name="billing-questions"></a>计费问题

### <a name="how-much-does-video-indexer-cost"></a>视频索引器如何收费？

视频索引器使用简单的即用即付定价模型，该模型基于要编制索引的内容输入的持续时间。 可能对编码、流式处理、存储、网络使用情况和编码预留单位收取额外费用。 有关详细信息，请参阅[定价](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/)页面。

### <a name="when-am-i-billed-for-using-video-indexer"></a>何时使用视频索引器收费？

发送要编制索引的视频时，用户将索引定义为视频分析、音频分析或两者。 这将确定将收取哪些 SKU。 如果处理过程中存在严重级别错误，则错误代码将作为响应返回。 在这种情况下，不会发生计费。  严重错误可能由代码中的 Bug 或服务的内部依赖项中的严重故障引起。 错误（如错误识别或透视提取）不被视为关键错误，并返回响应。 在任何情况下，如果返回有效的（非错误代码）响应，就会发生计费。
 
### <a name="does-video-indexer-offer-a-free-trial"></a>视频索引器是否提供免费试用版？

是的，视频索引器提供免费试用版，该版本具有完整的服务和 API 功能。 配额是，Web 界面用户可以免费为 600 分钟的视频编制索引，对于 API 用户，则为 2,400 分钟。 

## <a name="next-steps"></a>后续步骤

[概述](video-indexer-overview.md)
