---
title: 机器人 - C# - v3
titleSuffix: Language Understanding - Azure Cognitive Services
description: 使用 C# 构建集成了语言理解 (LUIS) 的聊天机器人。 此聊天机器人使用预生成的 HomeAutomation 域来快速实现机器人解决方案。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: d1534f76c4ef0c1edd8d83522f2d0855def48f25
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2019
ms.locfileid: "55880863"
---
# <a name="luis-bot-in-c-with-the-bot-framework-3x-and-the-azure-web-app-bot"></a>使用 Bot Framework 3.x 和 Azure Web 应用机器人的 C# 中的 LUIS 机器人

使用 C# 构建集成了语言理解 (LUIS) 的聊天机器人。 此聊天机器人使用预生成的 HomeAutomation 域来快速实现机器人解决方案。 该机器人是使用 Bot Framework 3.x 和 Azure Web 应用机器人生成的。

## <a name="prerequisite"></a>先决条件

* [HomeAutomation LUIS 应用](luis-get-started-create-app.md)。 此 LUIS 应用中的意向映射到机器人的对话处理程序。 

## <a name="luis-homeautomation-intents"></a>LUIS HomeAutomation 意向

| 意向 | 示例陈述 | 机器人功能 |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | 开灯。 | 检测到 LUIS 意向 `HomeAutomation.TurnOn` 时，机器人调用 `OnIntent` 对话框处理程序。 可以在此对话框中调用 IoT 服务以打开设备，并告知用户设备已打开。 |
| HomeAutomation.TurnOff | 关掉卧室灯。 | 检测到 LUIS 意向 `HomeAutomation.TurnOff` 时，机器人调用 `OffIntent` 对话框处理程序。 可以在此对话框中调用 IoT 服务以关闭设备，并告知用户设备已关闭。 |

## <a name="create-a-language-understanding-bot-with-bot-service"></a>使用机器人服务创建语言理解机器人

1. 在 [Azure 门户](https://portal.azure.com)中，选择左上角菜单中的“新建资源”。

    ![在 Azure 门户中新建资源](./media/luis-tutorial-cscharp-web-bot/bot-service-creation.png)

2. 在搜索框中，搜索“Web 应用机器人”。 

    ![选择 Web 应用机器人作为资源类型](./media/luis-tutorial-cscharp-web-bot/bot-service-selection.png)

3. 在 Web 应用机器人窗口中单击“创建”。

4. 在“机器人服务”中，提供所需信息，然后单击“创建”。 此操作可创建机器人服务和 LUIS 应用并将其部署到 Azure。 如果想要使用[语音启动](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)，请在创建机器人前查看[区域要求](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming)。 
    * 将“应用名称”设置为机器人名称。 将机器人部署到云（例如，mynotesbot.azurewebsites.net）时，该名称用作子域。 <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * 选择“订阅”、“[资源组](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)”、“应用服务计划”和“[位置](https://azure.microsoft.com/regions/)”。
    * 对于“机器人模板”，请选择：
        * **SDK v3**
        * **C#**
        * **语言理解**
    * 选择“LUIS 应用位置”。 这是创建应用的创作[区域](luis-reference-regions.md)。
    * 勾选法律声明的确认复选框。 法律声明条款在该复选框下方。

    ![Bot 服务](./media/luis-tutorial-cscharp-web-bot/bot-service-setting-callout-template.png)


5. 确认已部署机器人服务。
    * 单击“通知”（Azure 门户顶部边缘的钟形图标）。 通知从“部署已开始”更改为“部署已成功”。
    * 通知更改为“部署已成功”后，在通知上单击“转到资源”。

> [!Note]
> 此 Web 应用机器人创建过程还会创建一个新的 LUIS 应用。 已为你训练并发布该应用。 

## <a name="try-the-default-bot"></a>尝试使用默认机器人

勾选“通知”复选框，确认已部署机器人。 通知从“正在进行部署...”更改为“部署已成功”。 单击“转到资源”按钮，打开机器人的资源。

部署机器人后，单击“通过网上聊天执行测试”，打开“网上聊天”窗格。 在网上聊天中键入“你好”。

  ![通过网上聊天测试机器人](./media/luis-tutorial-cscharp-web-bot/bot-service-web-chat.png)

机器人响应说：“你已经进行了问候。 你说：你好”。  此响应确认机器人已接收消息，并将其传递到其创建的默认 LUIS 应用。 此默认 LUIS 应用检测到了问候意向。 在下一步中，需要将机器人连接到之前创建的 LUIS 应用，而不是默认 LUIS 应用。

## <a name="connect-your-luis-app-to-the-bot"></a>将 LUIS 应用连接到机器人

打开“应用程序设置”并编辑“LuisAppId”字段，以包含 LUIS 应用的应用程序 ID。 如果在美国西部以外的区域创建了 HomeAutomation LUIS 应用，则还需要更改 LuisAPIHostName。 LuisAPIKey 当前设为创作密钥。 在流量超出免费层配额时，将其更改为终结点密钥。 

  ![更新 Azure 中的 LUIS 应用 ID](./media/luis-tutorial-cscharp-web-bot/bot-service-app-settings.png)

> [!Note]
> 如果没有[家庭自动化应用](luis-get-started-create-app.md)的 LUIS 应用 ID，请使用登录 Azure 所用的同一帐户登录到 [LUIS](luis-reference-regions.md) 网站。 
> 1. 单击“我的应用”。 
> 2. 查找之前创建的 LUIS 应用，该应用包含 HomeAutomation 域中的意向和实体。
> 3. 在 LUIS 应用的“设置”页上，查找并复制应用 ID。 请确保该应用[已训练](luis-interactive-test.md)且[已发布](luis-how-to-publish-app.md)。 

    > [!WARNING]
    > If you delete your app ID or LUIS key, the bot will stop working.

## <a name="modify-the-bot-code"></a>修改机器人代码

1. 单击“生成”，然后单击“打开联机代码编辑器”。

   ![打开联机代码编辑器](./media/luis-tutorial-cscharp-web-bot/bot-service-build.png)

2. 右键单击 `build.cmd`，并选择“从控制台运行”以生成应用。 该服务可自动完成几个生成步骤。 如果结束时显示“已成功完成”，即表示生成完成。

3. 在代码编辑器中，打开 `/Dialogs/BasicLuisDialog.cs`。 它包含以下代码：

   [!code-csharp[Default BasicLuisDialog.cs](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/Default_BasicLuisDialog.cs "Default BasicLuisDialog.cs")]

## <a name="change-code-to-homeautomation-intents"></a>将代码更改为 HomeAutomation 意向


1. 删除“问候”、“取消”和“帮助”的三个意向属性和方法。 这些意向不用于 HomeAutomation 预生成域。 请务必保留“None”意向属性和方法。 

2. 将依赖项添加到文件顶部，与其他依赖项在一起：

   [!code-csharp[Dependencies](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=4-5&dedent=8 "dependencies")]

3. 添加常数以管理 `BasicLuisDialog ` 类顶部的字符串：

   [!code-csharp[Add Intent and Entity Constants](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=23-32&dedent=8 "Add Intent and Entity Constants")]

4. 添加 `BasicLuisDialog ` 类中的新意向 `HomeAutomation.TurnOn` 和 `HomeAutomation.TurnOff` 的代码：

   [!code-csharp[Add Intents](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=61-71&dedent=8 "Add Intents")]

5. 添加该代码以获取 `BasicLuisDialog ` 类中 LUIS 所找到的任何实体：

   [!code-csharp[Collect entities](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=34-53&dedent=8 "Collect entities")]

6. 更改 `BasicLuisDialog ` 类中的 ShowLuisResult 方法，以便在聊天机器人中将分数四舍五入、收集实体并显示响应消息：

   [!code-csharp[Display message in chatbot](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=73-83&dedent=8 "Display message in chatbot")]

## <a name="build-the-bot"></a>生成机器人
在代码编辑器中，右键单击 `build.cmd`，并选择“从控制台运行”。

![生成 Web 机器人 ](./media/luis-tutorial-cscharp-web-bot/bot-service-build-run-from-console.png)

代码视图替换为显示生成进度和结果的终端窗口。

![生成 Web 机器人成功](./media/luis-tutorial-cscharp-web-bot/bot-service-build-success.png)

> [!TIP]
> 另外一种生成机器人的方法是在蓝色顶部栏中选择机器人名称，并选择“打开 Kudu 控制台”。 控制台打开至目录 D:\home。 
> 
> 键入 `cd site\wwwroot`，将目录更改为 D:\home\site\wwwroot
>
> 通过键入 `build.cmd` 运行生成脚本

## <a name="test-the-bot"></a>测试机器人

在 Azure 门户中，选择“通过网上聊天执行测试”以测试机器人。 键入“开灯”和“关掉暖气”以调用添加到其中的意向。

   ![通过网上聊天测试 HomeAutomation 机器人](./media/luis-tutorial-cscharp-web-bot/bot-service-chat-results.png)

> [!TIP]
> 无需对机器人代码进行任何修改即可重新训练 LUIS 应用。 请参阅[添加示例陈述](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances)和[训练和测试 LUIS 应用](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-interactive-test)。 

## <a name="download-the-bot-to-debug"></a>下载机器人以进行调试
如果机器人不能正常运行，请将此项目下载至本地计算机并继续[调试](https://docs.microsoft.com/bot-framework/bot-service-debug-bot)。 

## <a name="learn-more-about-bot-framework"></a>深入了解 Bot Framework
深入了解 [Bot Framework](https://dev.botframework.com/) 以及 [3.x](https://github.com/Microsoft/BotBuilder) 和 [4.x](https://github.com/Microsoft/botbuilder-dotnet) SDK。

## <a name="next-steps"></a>后续步骤

添加 LUIS 意向和机器人服务对话框，以处理“帮助”、“取消”和“问候”意向。 请记得训练、发布和[生成](#build-the-bot) Web 应用机器人。 LUIS 和机器人应具备相同的意向。

查看更多使用聊天机器人的[示例](https://github.com/Microsoft/AI)。 

> [!div class="nextstepaction"]
> [添加意向](./luis-how-to-add-intents.md)
> [语音启动](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)

<!-- tested on Win10 -->
