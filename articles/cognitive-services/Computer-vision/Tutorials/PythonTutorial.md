---
title: 教程：计算机视觉 API Python
titlesuffix: Azure Cognitive Services
description: 通过 Jupyter Notebook 了解如何使用 Python 中的计算机视觉 API。 使用常用库直观显示结果。
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: tutorial
ms.date: 02/25/2017
ms.author: kefre
ms.openlocfilehash: 046250d3d2142badaac35490eff27bcac220fea9
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344891"
---
# <a name="tutorial-computer-vision-api-python"></a>教程：计算机视觉 API Python

本教程介绍如何使用 Python 中的计算机视觉 API 以及如何使用某些常用库直观显示结果。 使用 Jupyter 运行本教程。 若要了解如何开始使用交互式 Jupyter Notebook，请参阅 [Jupyter 文档](http://jupyter.readthedocs.io/en/latest/index.html)。 

## <a name="open-the-tutorial-notebook-in-jupyter"></a>在 Jupyter 中打开教程 Notebook 

1. 导航到 [GitHub 中的教程笔记本](https://github.com/Microsoft/Cognitive-Vision-Python)。 
2. 单击绿色按钮以克隆或下载教程。 
3. 打开命令提示符并转到文件夹 Cognitive-Vision-Python-master\Jupyter Notebook。 
4. 从命令提示符运行命令 jupyter notebook。 这将启动 Jupyter。
5. 在 Jupyter 窗口中，单击“计算机视觉 API Example.ipynb”以打开教程笔记本 

## <a name="run-the-tutorial"></a>运行教程

若要使用此笔记本，将需要计算机视觉 API 的订阅密钥。 访问[订阅页面](https://azure.microsoft.com/try/cognitive-services/)进行注册。 在“登录”页面上，使用 Microsoft 帐户登录，便能够订阅并获取免费密钥。 完成注册过程后，将密钥粘贴到笔记本的变量部分（如下所示）。 主密钥或辅助密钥均有效。 确保将密钥括在引号中以使其成为字符串。

```python
# Variables

_url = 'https://westcentralus.api.cognitive.microsoft.com/vision/v1/analyses'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```
