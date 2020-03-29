---
title: Azure 媒体服务旧组件 | Microsoft Docs
description: 本主题介绍 Azure 媒体服务旧组件。
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2020
ms.author: juliako
ms.openlocfilehash: 94a70a1234d902787f248890f0cb538a4ba9c2f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921073"
---
# <a name="azure-media-services-legacy-components"></a>Azure 媒体服务旧组件

随着时间的推移，对媒体服务组件进行了稳定的改进和增强。 因此，某些旧组件已经停用。 可以在以下文章中找到有关如何将应用程序从旧组件迁移到当前组件的说明。
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>旧组件的停用计划和迁移指南

我们宣布弃用 Windows Azure 媒体编码器** (WAME) 和 Azure 媒体编码器** (AME) 媒体处理器。 这些处理器于 2020 年 3 月 31 日停用。

* [从 Windows Azure 媒体编码器迁移到 Media Encoder Standard](migrate-windows-azure-media-encoder.md)
* [从 Azure 媒体编码器迁移到 Media Encoder Standard](migrate-azure-media-encoder.md)

我们还宣布停用以下媒体分析媒体处理器： 
 
|媒体处理器名称|停用日期|附加说明|
|---|---|
|[Azure Media Indexer 2](media-services-process-content-with-indexer2.md)|2020年1月1日|此媒体处理器正在被 Azure[媒体服务视频索引器](https://docs.microsoft.com/azure/media-services/video-indexer/)替换。 有关详细信息，请参阅从[Azure 媒体索引器 2 迁移到 Azure 媒体服务视频索引器](migrate-indexer-v1-v2.md)。|
|[Azure Media Indexer](media-services-index-content.md)|2023年3月1日|此媒体处理器正在被 Azure[媒体服务视频索引器](https://docs.microsoft.com/azure/media-services/video-indexer/)替换。 有关详细信息，请参阅从[Azure 媒体索引器迁移到 Azure 媒体服务视频索引器](migrate-indexer-v1-v2.md)|
|[运动检测](media-services-motion-detection.md)|2020年6月1日|目前没有更换计划。|
|[视频摘要](media-services-video-summarization.md)|2020年6月1日|目前没有更换计划。|
|[视频光学字符识别](media-services-video-optical-character-recognition.md)|2020年6月1日|此媒体处理器正在被 Azure[媒体服务视频索引器](https://docs.microsoft.com/azure/media-services/video-indexer/)替换。 此外，请考虑使用[Azure 媒体服务 v3 API](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept)。 <br/>请参阅[比较 Azure 媒体服务 v3 预设和视频索引器](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets)|
|[面部检测器](media-services-face-and-emotion-detection.md)|2020年6月1日|此媒体处理器正在被 Azure[媒体服务视频索引器](https://docs.microsoft.com/azure/media-services/video-indexer/)替换。 此外，请考虑使用[Azure 媒体服务 v3 API](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept)。 <br/>请参阅[比较 Azure 媒体服务 v3 预设和视频索引器](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets)|
|[内容审阅者](media-services-content-moderation.md)|2020年6月1日|此媒体处理器正在被 Azure[媒体服务视频索引器](https://docs.microsoft.com/azure/media-services/video-indexer/)替换。 此外，请考虑使用[Azure 媒体服务 v3 API](https://docs.microsoft.com/azure/media-services/latest/analyzing-video-audio-files-concept)。 <br/>请参阅[比较 Azure 媒体服务 v3 预设和视频索引器](https://docs.microsoft.com/azure/media-services/video-indexer/compare-video-indexer-with-media-services-presets)|

## <a name="next-steps"></a>后续步骤

[有关从媒体服务 v2 迁移到 v3 的指导](../latest/migrate-from-v2-to-v3.md)
