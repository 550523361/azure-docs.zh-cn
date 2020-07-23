---
title: 什么是 Azure 内容审查器？
titleSuffix: Azure Cognitive Services
description: 了解如何使用内容审查器跟踪、标记、评估和筛选用户生成的内容中不适合的材料。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: overview
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: f28f2bcf5d04c9a6354b8135bd39546b9d8b9bf3
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "81404297"
---
# <a name="what-is-azure-content-moderator"></a>什么是 Azure 内容审查器？

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure 内容审查器是一项认知服务，用于检查文本、图像和视频中是否存在可能的冒犯性内容、有风险内容或其他令人不适的内容。 找到此类内容时，此服务会将相应的标签（标记）应用到该内容。 然后，应用会处理标记的内容，使之符合法规的要求，或者为用户维持一个理想的环境。 请参阅[审查 API](#moderation-apis) 部分，详细了解不同内容标记表示的意思。

## <a name="where-its-used"></a>使用场合

下面是软件开发人员或团队会使用内容审察器的一些场景：

- 在联机市场中审查产品目录和其他用户生成的内容。
- 在游戏公司中审查用户生成的游戏项目和聊天室。
- 在社交消息平台中审查用户添加的图像、文本和视频。
- 企业媒体公司对其内容进行集中式审查。
- K-12 教育解决方案提供商为学生和教师筛选掉不当的内容。

> [!NOTE]
> 不能使用内容审查器检测非法儿童剥削图像。 不过，合格组织可以使用 [PhotoDNA 云服务](https://www.microsoft.com/photodna "Microsoft PhotoDNA 云服务")来筛查此类内容。

## <a name="what-it-includes"></a>组成部分

内容审查器服务包含多个可以通过 REST 调用和 .NET SDK 使用的 Web 服务 API。 它还包括评审工具，允许人工审阅者帮助服务并改进或微调其审查功能。

## <a name="moderation-apis"></a>审查 API

内容审查器服务包括了审查 API，这些 API 检查内容中是否存在可能不适当或令人反感的材料。

![内容审查器审查 API 的框图](images/content-moderator-mod-api.png)

下表介绍了各种类型的审查 API。

| API 组 | 说明 |
| ------ | ----------- |
|[**文本审查**](text-moderation-api.md)| 扫描文本中是否存在冒犯性内容、明确的或暗示性的色情内容、不雅内容和个人数据。|
|[**自定义术语列表**](try-terms-list-api.md)| 扫描文本中是否存在自定义术语列表以及内置的术语。 根据你自己的内容策略，使用自定义列表阻止或允许内容。|  
|[**图像审查**](image-moderation-api.md)| 扫描图像中是否存在成人内容或不雅内容，通过光学字符识别 (OCR) 功能检测图像中的文本，以及检测人脸。|
|[**自定义图像列表**](try-image-list-api.md)| 针对自定义图像列表对图像进行扫描。 使用自定义图像列表筛选掉常常反复出现的内容的实例，这些实例不需再次分类。|
|[**视频审查**](video-moderation-api.md)| 扫描视频中是否存在成人内容或挑逗性内容，并针对上述内容返回时间标记。|

## <a name="review-apis"></a>查看 API

审阅 API 允许你将审查管道与人工审阅者进行集成。 使用[作业](review-api.md#jobs)、[审阅](review-api.md#reviews)和[工作流](review-api.md#workflows)操作在[审阅工具](#review-tool)（下文）中创建和自动执行人工干预工作流。

> [!NOTE]
> 工作流 API 在 .NET SDK 中尚不可用，但可以与 REST 终结点一起使用。

![内容审查器审阅 API 的框图](images/content-moderator-rev-api.png)

## <a name="review-tool"></a>审阅工具

内容审查器服务还包括基于 Web 的[审阅工具](Review-Tool-User-Guide/human-in-the-loop.md)，其中托管的内容审阅可供审阅人处理。 人工输入不会训练服务，但是将服务与人工审阅团队组合起来以后，开发人员就可以在效率和准确性之间取得适当的平衡。 评审工具还为多种内容审查器资源提供用户友好型前端。

![内容审查器评审工具主页](images/homepage.PNG)

## <a name="data-privacy-and-security"></a>数据隐私和安全性

与所有认知服务一样，使用内容审查器服务的开发人员应该了解 Microsoft 针对客户数据的政策。 请参阅 Microsoft 信任中心上的[“认知服务”页面](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices)来了解详细信息。

## <a name="next-steps"></a>后续步骤

按照[在 Web 上试用内容审查器](quick-start.md)中的说明开始使用内容审查器服务。
