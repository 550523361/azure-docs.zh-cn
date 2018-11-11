---
title: 演示对话学习器模型，密码重置 - Microsoft 认知服务 | Microsoft Docs
titleSuffix: Azure
description: 了解如何创建演示对话学习器模型。
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: bd0bcd79bb21dc3973b34086f6dad21b47a95c2f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240862"
---
# <a name="demo-password-reset"></a>演示：密码重置
此演示展示了一个简单的技术支持机器人，它可以帮助进行密码重置。 

它展示了对话学习器如何学习重要的对话框流、多轮序列，包括域外类。 此演示未使用任何代码或实体。

## <a name="video"></a>视频

[![演示密码预览](https://aka.ms/cl-demo-password-preview)](https://aka.ms/blis-demo-password)

## <a name="requirements"></a>要求
本教程要求密码重置机器人正在运行

    npm run demo-password

### <a name="open-the-demo"></a>打开演示

在 Web UI 的“模型”列表中，单击“教程演示密码重置”。 

### <a name="actions"></a>操作

我们已创建了操作集，用户在其中查找有关其密码的帮助，包括解决方案。

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>训练对话

有大量的训练对话。 还有针对域外类的演示 -- 例如，诸如“驾驶路线”之类的用户请求就是域外的；已向机器人提供了一些域外请求的示例，并且机器人能够以“我能帮忙做这个”做出响应。

![](../media/tutorial_pw_reset_entities.PNG)

例如，让我们尝试一个教学会话。

1. 单击 Train Dialogs，然后单击 New Train Dialog。
1. 输入“I lost my password”。
2. 单击“Score Action”。
3. 单击以选择“Is that for your local account or Microsoft account?”
4. 输入“Local account”。
5. 单击“Score Actions”。
3. 单击以选择“Which version of Windows do you have?”
4. 输入“Windows 8”。
5. 单击“Score Actions”。
6. 选择“SOLUTION: how to reset password on Windows 8”。
4. 单击“Done Teaching”。

让我们试验一下机器人如何学习域外类。

1. 单击 Train Dialogs，然后单击 New Train Dialog。
1. 输入“web search”。
    - 这是域外类的一个示例。 
2. 单击“Score Action”。
3. 单击以选择“Sorry, I can't help with that”。
    - 注意，此选项的得分当前较低。 但是，在进行更多一些教学后，得分将变高。
4. 单击“Done Teaching”。

现在，你已了解了如何创建一个基本的技术支持演示，以及它如何进行学习来提供解决方案并处理不符合示例的查询。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [演示 - 披萨订单](./demo-pizza-order.md)
