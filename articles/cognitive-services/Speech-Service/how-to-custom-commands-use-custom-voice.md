---
title: 如何：将自定义命令用于自定义语音语音服务
titleSuffix: Azure Cognitive Services
description: 在本文中，你将指定自定义命令应用程序的输出声音。
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: sausin
ms.openlocfilehash: 4a5c14909606dcb862fcf53d99bc5bc00fba63bd
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2020
ms.locfileid: "95025678"
---
# <a name="use-custom-commands-with-custom-voice"></a>通过自定义语音使用自定义命令

本文介绍如何为自定义命令应用程序选择自定义输出声音。

## <a name="select-a-custom-voice"></a>选择自定义语音

1. 在自定义命令应用程序的左窗格中，选择 " **设置** "。
1. 从中间窗格中选择 " **自定义语音** "。
1. 从表中选择所需的自定义或公共语音。
1. 选择“保存”。

> [!div class="mx-imgBorder"]
> ![带参数的示例句子](media/custom-commands/select-custom-voice.png)

> [!NOTE]
> - 对于 **公共语音**， **神经类型** 仅适用于特定区域。 若要查看可用性，请参阅 [按区域/终结点的标准和神经声音](./regions.md#standard-and-neural-voices)。
> - 对于 **自定义** 语音，可以从自定义语音项目页创建。 请参阅 [自定义语音入门](./how-to-custom-voice.md)。

现在，应用程序将在选定的语音中做出响应，而不是默认的语音。