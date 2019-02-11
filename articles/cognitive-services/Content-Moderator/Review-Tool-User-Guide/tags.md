---
title: 将自定义标记用于内容审核 - 内容审查器
titlesuffix: Azure Cognitive Services
description: 内容审查器中有默认标记，也可以创建业务专用的内容审查自定义标记。
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 942b30ab9949a28c42949913d90e7448c22b658f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206267"
---
# <a name="create-and-use-moderation-tags"></a>创建和使用审核标记

除了两个默认标记（isadult (a) 和 isracy (r)）之外，还可以创建自定义标记来执行更有针对性的扫描。 然后，人工审阅者可以将这些自定义标记分配给图像或文本。

## <a name="create-tags"></a>创建标记

1.  选择“设置”选项卡中的“标记”。

  ![内容审查标记](images/tags-1.png)

2.  输入标记的两字母短代码。
3.  输入标记名称。 请务必命名简短的描述性名称。 例如，“isbullying”。
4.  输入说明。
5.  单击“添加”。
6.  创建完标记后，单击“保存”。

![定义内容审查标记](images/tags-2-define.png)

## <a name="using-custom-tags"></a>使用自定义标记

自定义标记在人工审阅期间使用。 审阅者通过单击预览中显示的自定义标记即可选择它。

![使用内容审查标记](images/tags-3-use.png)

可以选中或取消选中“可见”，从而启用或禁用对各个审阅使用不同的标记。
 
![禁用内容审查标记](images/tags-4-disable.png)

虽然无法删除两个默认标记（isadult 和 isracy），但可以删除已定义的任何自定义标记。 单击要删除的标记旁边的垃圾桶。

![删除内容审查标记](images/tags-5-delete.png)

## <a name="next-steps"></a>后续步骤

若要了解如何使用标记来审查图像，请参阅[审阅已审查的图像](Review-Moderated-Images.md)。
