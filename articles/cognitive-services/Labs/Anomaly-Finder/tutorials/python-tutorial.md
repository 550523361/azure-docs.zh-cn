---
title: 教程：异常情况检测、Python
titlesuffix: Azure Cognitive Services
description: 探索使用异常情况检测 API 的 Python Notebook。 将原始数据点发送到 API 并获得预期值和异常点。
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 98f83fbc4db8685526833c3d4f1d15903419ad3f
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887745"
---
# <a name="tutorial-anomaly-detection-with-python-application"></a>教程：使用 Python 应用程序进行异常情况检测

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

本教程介绍了如何使用 Python 中的异常情况检测 API 以及如何使用常用库直观显示结果。 使用 Jupyter 运行教程并使用你的订阅密钥尝试查看自己的数据。 若要了解如何开始使用交互式 Jupyter 笔记本，请参阅 [Jupyter 文档](http://jupyter.readthedocs.io/en/latest/index.html)。 

## <a name="prerequisites"></a>先决条件

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>订阅异常情况检测并获取订阅密钥 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="download-the-example-code"></a>下载示例代码

1. 导航到 [Github 中的教程笔记本](https://github.com/MicrosoftAnomalyDetection/python-sample)。
2. 单击绿色按钮以克隆或下载教程。 

## <a name="opening-the-tutorial-notebook-in-jupyter"></a>在 Jupyter 中打开教程笔记本

1. 打开命令提示符并转到 python-sample 文件夹。
2. 从命令提示符运行命令 Jupyter 笔记本，从而启动 Jupyter。
3. 在 Jupyter 窗口中，单击 <em>异常情况检测 API Example.ipynb</em> 以打开教程笔记本。   

## <a name="running-the-tutorial"></a>运行教程

若要使用此笔记本，将需要异常情况检测 API 的订阅密钥。 访问订阅页面进行注册。 在“登录”页面上，使用你的 Microsoft 帐户登录，你将能够订阅并获取你的密钥。 完成注册过程后，将你的密钥粘贴到笔记本的变量部分（下面重新编写）。 主密钥或辅助密钥均有效。 确保将密钥括在引号中以使其成为字符串。

```Python

            # Variables
            endpoint = 'https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection'
            subscription_key = None #Here you have to paste your primary key

```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [REST API 参考](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
