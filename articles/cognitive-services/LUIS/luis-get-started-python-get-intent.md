---
title: 快速入门：了解如何使用 Python 调用语言理解 (LUIS) 应用 | Microsoft Docs
description: 本快速入门介绍了如何使用 Python 调用 LUIS 应用。
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 06/27/2018
ms.author: diberry
ms.openlocfilehash: bc7ae912d762a98c34b9a1b2d6a82d5630c4794b
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "43768797"
---
# <a name="quickstart-call-a-luis-endpoint-using-python"></a>快速入门：使用 Python 调用 LUIS 终结点
在本快速入门中，你将向 LUIS 终结点传递话语并返回意向和实体。

<!-- green checkmark -->
<!--
> [!div class="checklist"]
> * Create LUIS subscription and copy key value for later use
> * View LUIS endpoint results from browser to public sample IoT app
> * Create Visual Studio C# console app to make HTTPS call to LUIS endpoint
-->

本文需要一个免费的 [LUIS](luis-reference-regions.md#luis-website) 帐户，以便创作 LUIS 应用程序。

<a name="create-luis-subscription-key"></a>
## <a name="create-luis-endpoint-key"></a>创建 LUIS 终结点密钥
需要使用一个认知服务 API 密钥对本演练中使用的示例 LUIS 应用发出调用。 

若要获取 API 密钥，请执行以下步骤： 

1. 首先，需要在 Azure 门户中创建[认知服务 API 帐户](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)。 如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

2. 通过 https://portal.azure.com 登录到 Azure 门户。 

3. 遵循[使用 Azure 创建终结点密钥](./luis-how-to-azure-subscription.md)中的步骤获取密钥。

4. 返回 [LUIS](luis-reference-regions.md) 网站，使用 Azure 帐户登录。 

    [![](media/luis-get-started-node-get-intent/app-list.png "应用列表的屏幕截图")](media/luis-get-started-node-get-intent/app-list.png)

## <a name="understand-what-luis-returns"></a>了解 LUIS 返回的内容

若要了解 LUIS 应用返回的内容，可将示例 LUIS 应用的 URL 粘贴到浏览器窗口中。 示例应用是一个 IoT 应用，可检测用户是要打开还是关闭灯光。

1. 示例应用的终结点采用以下格式：`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_API_KEY>&verbose=false&q=turn%20on%20the%20bedroom%20light`请复制该 URL，并将 `subscription-key` 字段的值替换为你的终结点密钥。
2. 将该 URL 粘贴到浏览器窗口中，然后按 Enter。 浏览器显示的 JSON 结果指示 LUIS 检测到 `HomeAutomation.TurnOn` 意向，以及值为 `bedroom` 的 `HomeAutomation.Room` 实体。

    ![JSON 结果指示检测到意向 TurnOn](./media/luis-get-started-node-get-intent/turn-on-bedroom.png)
3. 将 URL 中的 `q=` 参数值更改为 `turn off the living room light`，然后按 Enter。 现在，结果指示 LUIS 检测到 `HomeAutomation.TurnOff` 意向，以及值为 `living room` 的 `HomeAutomation.Room` 实体。 

    ![JSON 结果指示检测到意向 TurnOff](./media/luis-get-started-node-get-intent/turn-off-living-room.png)


## <a name="consume-a-luis-result-using-the-endpoint-api-with-python"></a>在 Python 中通过终结点 API 使用 LUIS 结果

可以使用 Python 来访问上一步骤中浏览器窗口显示的相同结果。

1. 将以下代码片段之一复制到名为 `quickstart-call-endpoint.py` 的文件中：

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/2.x/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/3.x/quickstart-call-endpoint-3-6.py)]

2. 将 `Ocp-Apim-Subscription-Key` 字段的值替换为你的 LUIS 终结点密钥。

3. 使用 `pip install requests` 安装依赖项。

4. 使用 `python ./quickstart-call-endpoint.py` 运行该脚本。 其中会显示前面在浏览器窗口中显示的相同 JSON。
<!-- 
![Console window displays JSON result from LUIS](./media/luis-get-started-python-get-intent/console-turn-on.png)
-->

## <a name="clean-up-resources"></a>清理资源
本教程中创建的两个资源是 LUIS 终结点密钥和 C# 项目。 请从 Azure 门户中删除 LUIS 终结点密钥。 关闭 Visual Studio 项目，并从文件系统中删除目录。 

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [添加话语](luis-get-started-python-add-utterance.md)