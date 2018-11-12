---
title: 如何向对话学习器模型中添加预构建实体 - Microsoft 认知服务 | Microsoft Docs
titleSuffix: Azure
description: 了解如何向对话学习器模型添加预构建实体。
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 2dbbf2a47cdc4240e5b0ba38658a4cb8d5307ff8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260051"
---
# <a name="how-to-add-pre-built-entities"></a>如何添加预构建实体
本教程展示了如何向对话学习器模型添加预构建实体。

## <a name="video"></a>视频

[![教程 7 预览](https://aka.ms/cl-tutorial-07-preview)](https://aka.ms/blis-tutorial-07)

## <a name="requirements"></a>要求
本教程要求运行常规教程机器人

    npm run tutorial-general

## <a name="details"></a>详细信息

预构建实体可以识别常见类型的实体，例如数字、日期、货币金额和其他实体。  与自定义实体不同，它们可以立即工作，不需要进行任何训练。  与自定义实体不同，它们的行为无法更改。  默认情况下，预构建实体是多值的，也就是说，机器人的内存将累计该实体的每个已识别实例。

## <a name="steps"></a>Steps

### <a name="create-the-model"></a>创建模型

1. 在 Web UI 中，单击“新建模型”
2. 在“名称”中输入 BuiltInEntities。 然后单击“创建”。

### <a name="create-an-entity"></a>创建实体

1. 依次单击“实体”和“新建实体”。
2. 单击“实体类型”下拉列表并选择 datetimev2。
    - “可编程”和“可否定”选项被禁用，因为它们不适用于预构建实体。
3. 单击“创建”。

![](../media/tutorial7_entities.PNG)

### <a name="create-two-actions"></a>创建两个操作

1. 依次单击“操作”和“新建操作”
2. 在“响应”中，键入“The date is $luis-datetimev2”。
3. 单击“创建”。

![](../media/tutorial7_actions.PNG)

然后，创建第二个操作：

1. 依次单击“操作”和“新建操作”以创建第二个操作。
3. 在“响应”中，键入“What's the date?”。
4. 在“取消实体资格”中，输入“luis-datetimev2”。
4. 单击“法律条款” 

![](../media/tutorial7_actions2.PNG)

现在已有两个操作。

### <a name="train-the-bot"></a>定型机器人

1. 依次单击“定型”对话框和“新建定型”对话框。
2. 键入“hello”。
3. 单击“对操作打分”，然后选择“What's the date?”
2. 输入“today”。 
    - 请注意，today 已被标记并且显示在第二行中，因为它是一个预构建实体并且不可编辑。
5. 单击“对操作打分”
    - 请注意，日期现在显示在“实体内存”部分中。 
    - 如果将鼠标指针悬停在日期上，则会看到由 LUIS 提供的更多数据，这些数据是可用的并且可以进一步在代码中进行操作。 
6. 选择“The date is $luis-datetimev2”。
7. 单击“完成教学”

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [备用输入](./8-alternative-inputs.md)
