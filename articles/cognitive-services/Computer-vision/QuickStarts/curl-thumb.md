---
title: 快速入门：生成缩略图 - REST、cURL
titleSuffix: Azure Cognitive Services
description: 在该快速入门中，你将使用计算机视觉 API 和 cURL 基于图像生成缩略图。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: d60f1101fc858e9dcaa22162be97417544e13693
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2020
ms.locfileid: "81405045"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-curl"></a>快速入门：使用计算机视觉 REST API 和 cURL 生成缩略图

本快速入门将使用计算机视觉 REST API 基于图像生成缩略图。 可以指定所需的高度和宽度，可以与输入图像的纵横比不同。 计算机视觉使用智能裁剪来智能识别感兴趣的区域并围绕该区域生成裁剪坐标。

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services)。

## <a name="prerequisites"></a>先决条件

- 必须具有 [cURL](https://curl.haxx.se/windows)。
- 必须具有计算机视觉的订阅密钥。 可以从[试用认知服务](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision)获取免费的试用密钥。 或者，按照[创建认知服务帐户](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)中的说明订阅计算机视觉并获取密钥。

## <a name="create-and-run-the-sample-command"></a>创建并运行示例命令

要创建和运行示例，请执行以下步骤：

1. 将以下命令复制到文本编辑器中。
1. 必要时在命令中进行如下更改：
    1. 将 `<subscriptionKey>` 的值替换为你的订阅密钥。
    1. 将 `<thumbnailFile>` 的值替换为要保存缩略图的文件的路径和名称。
    1. 将请求 URL 的第一部分 (`westcentralus`) 替换为你自己的终结点 URL 中的文本。
        [!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]
    1. （可选）将请求正文 (`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`) 中的图像 URL 更改为要从中生成缩略图的其他图像的 URL。
1. 打开命令提示符窗口。
1. 将文本编辑器中的命令粘贴到命令提示符窗口，然后运行命令。

    ```bash
    curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.1/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
    ```

## <a name="examine-the-response"></a>检查响应

成功的响应会将缩略图写入 `<thumbnailFile>` 中指定的文件。 如果请求失败，则响应包含错误代码和消息，以帮助确定出错的地方。 如果请求似乎是成功的，但创建的缩略图不是有效的图像文件，则可能是因为订阅密钥无效。

## <a name="next-steps"></a>后续步骤

探索计算机视觉 API，了解如何分析图像、检测名人和地标、创建缩略图，并提取印刷体文本和手写文本。 要快速体验计算机视觉 API，请尝试使用 [Open API 测试控制台](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console)。

> [!div class="nextstepaction"]
> [探索计算机视觉 API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
