---
title: 与其他应用程序集成-QnA Maker
description: QnA Maker 集成到客户端应用程序中，例如聊天 bot 以及其他自然语言处理服务，例如语言理解 (LUIS) 。
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: ff5e64dd31542b1e31df6d9ca709567b322995ce
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91776521"
---
# <a name="design-knowledge-base-for-client-applications"></a>适用于客户端应用程序的设计知识库

QnA Maker 集成到客户端应用程序中，例如聊天 bot 以及其他自然语言处理服务，例如语言理解 (LUIS) 。

## <a name="integration-with-a-conversational-client"></a>与对话客户端集成

QnA Maker 与会话客户端应用程序（如 [Microsoft Bot Framework](https://dev.botframework.com/)）集成。 不需要清除或转换发送到 QnA Maker 的文本。 QnA Maker 接受自然语言并返回最佳答案。

## <a name="create-a-bot-without-writing-any-code"></a>创建机器人，无需编写任何代码

发布知识库后，通过选择 "**创建机器人**" 按钮，从 "**发布**" 页创建一个 bot。 使用 [机器人教程](../Quickstarts/create-publish-knowledge-base.md) 了解选择按钮后会发生的情况。

## <a name="providing-multi-turn-conversations"></a>提供多轮对话

机器人客户端从您的知识库提供最佳选择的答案，如果答案是多轮 QnA 对的一部分，则可以提供跟进提示。 了解 [如何](../how-to/multiturn-conversation.md) 向知识库添加多轮会话问题和答案集。

## <a name="natural-language-processing"></a>自然语言处理

虽然 QnA Maker 处理使用自然语言处理的问题，但也可以将其用于从多个知识库回答问题的较大系统。 您可以将 QnA Maker 与另一种认知服务组合在一起，语言理解 (LUIS) ，在获取特定知识库之前提供自然语言处理。 了解有关何时以及如何将 [LUIS 和 QnA Maker](../../luis/choose-natural-language-processing-service.md?toc=/azure/cognitive-services/qnamaker/toc.json) 一起使用的详细信息。

## <a name="next-steps"></a>后续步骤

了解 QnA Maker 的开发周期 [概念](development-lifecycle-knowledge-base.md) 。