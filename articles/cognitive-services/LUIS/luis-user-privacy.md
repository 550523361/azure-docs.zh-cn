---
title: 导出和删除数据 - LUIS
titleSuffix: Azure Cognitive Services
description: 你对于查看、导出和删除其数据具有完全控制权。 删除客户数据以确保隐私和合规性。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 04/02/2019
ms.author: diberry
ms.openlocfilehash: 4e3e0d04b0086905b80e26fb4f838c36b5b5545e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273359"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>在认知服务中的语言理解 (LUIS) 中导出和删除客户数据

删除客户数据以确保隐私和合规性。

## <a name="summary-of-customer-data-request-features"></a>客户数据请求功能摘要
语言理解智能服务 (LUIS) 保存客户内容来使服务运转，但 LUIS 用户拥有完全控制权限来查看、导出以及删除其数据。 这可以通过 LUIS Web [门户](luis-reference-regions.md)或 [LUIS 创作（也称为“编程”）API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) 来完成。

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

客户内容以加密方式存储在 Microsoft 区域性 Azure 存储中，并且包括：

- 在注册时收集的用户帐户内容
- 构建模型时所需的训练数据
- [主动学习](luis-concept-review-endpoint-utterances.md)使用记录的用户查询来帮助改进模型
  - 用户可以通过将 `&log=false` 追加到请求来关闭查询日志记录，[此处](troubleshooting.md#how-can-i-disable-the-logging-of-utterances)提供了详细信息

## <a name="deleting-customer-data"></a>删除客户数据
LUIS 用户具有完全控制权限，可通过 LUIS Web 门户或 LUIS 创作（也称“编程”）API 来删除任何用户内容。 下表显示了对这两种方式有帮助的链接：

| | **用户帐户** | **应用程序** | **示例** | **最终用户查询** |
| --- | --- | --- | --- | --- |
| **门户** | [链接](luis-concept-data-storage.md#delete-an-account) | [链接](luis-how-to-start-new-app.md#delete-app) | [链接](luis-concept-data-storage.md#utterances-in-an-intent) | [主动学习话语](luis-how-to-review-endpoint-utterances.md#disable-active-learning)<br>[记录的话语](luis-concept-data-storage.md#disable-logging-utterances) |
| **Api** | [链接](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [链接](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [链接](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [链接](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>导出客户数据
LUIS 用户具有完全控制权限可在门户上查看数据，但是，若要导出数据，必须通过 LUIS 创作（也称为“编程”）API 进行。 下表显示了对通过 LUIS 创作（也称为“编程”）API 导出数据有帮助的链接：

| | **用户帐户** | **应用程序** | **Utterance(s)** | **最终用户查询** |
| --- | --- | --- | --- | --- |
| **Api** | [链接](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [链接](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [链接](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [链接](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |

## <a name="location-of-active-learning"></a>主动学习的位置

为了启用[主动学习](luis-how-to-review-endpoint-utterances.md#enable-active-learning)，在已发布的 LUIS 终结点接收的用户记录的话语存储在以下 Azure 地理位置中：

* [欧洲](#europe)
* [澳大利亚](#australia)
* [美国](#united-states)

除了主动学习数据（详见下文）外，LUIS 遵循[区域服务的数据存储做法](https://azuredatacentermap.azurewebsites.net/)。

### <a name="europe"></a>欧洲

[eu.luis.ai](https://eu.luis.ai)门户和欧洲创作（也称为编程 API）托管在 Azure 的欧洲地理中。 eu.luis.ai门户和欧洲创作（也称为编程 API）支持将终结点部署到以下 Azure 地理位置：

* 欧洲
* 法国
* United Kingdom

部署到这些 Azure 地理位置时，从应用最终用户处接收的终结点接收的话语将存储在 Azure 的欧洲地理中以进行主动学习。 可以禁用主动学习，请参阅[禁用主动学习](luis-how-to-review-endpoint-utterances.md#disable-active-learning)。 若要管理存储的话语，请参阅[删除话语](luis-how-to-review-endpoint-utterances.md#delete-utterance)。

### <a name="australia"></a>澳大利亚

[au.luis.ai](https://au.luis.ai)门户和澳大利亚创作（也称为编程 API）托管在 Azure 的澳大利亚地理中。 au.luis.ai门户和澳大利亚创作（也称为编程 API）支持将终结点部署到以下 Azure 地理位置：

* 澳大利亚

部署到这些 Azure 地理位置时，从应用最终用户处接收的终结点接收的话语将存储在 Azure 的澳大利亚地理中以进行主动学习。 可以禁用主动学习，请参阅[禁用主动学习](luis-how-to-review-endpoint-utterances.md#disable-active-learning)。 若要管理存储的话语，请参阅[删除话语](luis-how-to-review-endpoint-utterances.md#delete-utterance)。

### <a name="united-states"></a>United States

[luis.ai](https://www.luis.ai)门户和美国创作（也称为编程 API）托管在 Azure 的美国地理位置中。 luis.ai门户和美国创作（也称为编程 API）支持将终结点部署到以下 Azure 地理位置：

* 欧洲或澳大利亚创作区域不支持的 Azure 地理位置

部署到这些 Azure 地理位置时，从应用最终用户处接收的终结点接收的话语将存储在 Azure 的美国地理位置中，以便进行主动学习。 可以禁用主动学习，请参阅[禁用主动学习](luis-how-to-review-endpoint-utterances.md#disable-active-learning)。 若要管理存储的话语，请参阅[删除话语](luis-how-to-review-endpoint-utterances.md#delete-utterance)。


## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [LUIS 区域参考](./luis-reference-regions.md)
