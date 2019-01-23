---
title: 审查内容时连接到其他服务 - 内容审查器
titlesuffix: Azure Cognitive Services
description: 了解如何使用连接器访问内容审查器工作流的其他 API。
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 99d8b3603278a9c6c432ca32a1d85e9abe34e1da
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265577"
---
# <a name="connect-to-other-cognitive-services"></a>连接到其他认知服务

除了内容审查器 API 之外，Azure 内容审查器工作流还可以使用其他 API。 可以使用内容审查器中的连接器访问其他 API。 连接器可链接到其他 API。

内容审查器包括下面这些默认连接器：

* 情感 API
* 人脸 API
* PhotoDNA 云服务

![内容审查器可用的连接器](images/connectors-1.png)

## <a name="verify-your-credentials"></a>验证凭据 

定义工作流前，请先确保拥有要使用的连接器 API 的有效凭据：

1.  在“审阅”工具仪表板上，依次选择“设置” > “连接器”。

  ![在内容审查器中选择“连接器”](images/connectors-2.png)

2.  选择要验证其凭据的连接器旁边的“编辑”符号。

  ![在内容审查器中选择“编辑”符号](images/connectors-3.png)

3.  此时，订阅密钥显示。 如果执行任何编辑，请在完成后选择“保存”。

  ![内容审查器中的“编辑连接器”页](images/connectors-4-1.png)
 
## <a name="add-a-connector"></a>添加连接器

1.  必须有订阅密钥，才能添加连接器。 在“审阅”工具仪表板上，依次选择“设置” > “凭据”。 选择并复制“Ocp-Admin-Subscription-Key”框中的值。

2.  选择“连接器”。 选择“审阅”工具仪表板上显示的可用连接器之一。 然后，选择“连接”。 

  ![内容审查器中的“添加连接器”页](images/connectors-5.png)

3.  在“Ocp-Admin-Subscription-Key”框中，粘贴所复制的密钥。 然后选择“保存”。

## <a name="next-steps"></a>后续步骤

* 了解如何使用连接器[定义自定义工作流](workflows.md)。
