---
title: 容器支持
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/07/2019
ms.author: dapine
ms.openlocfilehash: 2cb2cfbdfbac5d496f109d85977b41a050766ab0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "73499105"
---
## <a name="create-an-computer-vision-resource"></a>创建计算机视觉资源

1. 登录到[Azure 门户](https://portal.azure.com)。
1. 单击"[创建**计算机视觉**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision)资源"。
1. 输入所有必需的设置：

    |设置|“值”|
    |--|--|
    |“属性”|所需名称（2-64 个字符）|
    |订阅|选择相应的订阅|
    |位置|选择任何附近的可用位置|
    |定价层|`F0` - 最低定价层|
    |资源组|选择可用的资源组|

1. 单击“创建”**** 并等待创建资源。 创建资源后，导航到资源页。
1. 有关详细信息，`{ENDPOINT_URI}`请参阅`{API_KEY}`收集已配置和 ，请参阅[收集所需的参数](../computer-vision-how-to-install-containers.md#gathering-required-parameters)。
