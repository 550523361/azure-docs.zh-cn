---
title: Azure 容器实例食谱
titleSuffix: Azure Cognitive Services
description: 了解如何在 Azure 容器实例上部署认知服务容器
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: f247465c7e2c0a212df2821ebc7165d3ee5b15f3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "80876651"
---
# <a name="deploy-and-run-container-on-azure-container-instance"></a>在 Azure 容器实例上部署和运行容器

通过以下步骤，可以轻松地通过 Azure[容器实例](https://docs.microsoft.com/azure/container-instances/)缩放云中的 Azure 认知服务应用程序。 容器化可帮助你专注于构建应用程序，而不是管理基础结构。 有关使用容器的详细信息，请参阅[功能和优势](../cognitive-services-container-support.md#features-and-benefits)。

## <a name="prerequisites"></a>先决条件

食谱适用于任何认知服务容器。 使用食谱之前，必须在 Azure 门户中创建认知服务资源。 支持容器的每个认知服务都有一个 "如何安装" 文档，专门用于安装和配置容器的服务。 某些服务需要一个文件或一组文件作为容器的输入，在使用此解决方案之前，请务必了解并成功使用容器。

* 在 Azure 门户中创建的认知服务资源。
* 认知服务**终结点 URL** -查看容器的特定服务 "如何安装"，以查找 endpoint URL 在 Azure 门户中的位置，以及正确的 url 示例。 确切的格式可以从服务更改为服务。
* 认知服务**密钥**-密钥位于 Azure 资源的 "**密钥**" 页上。 您只需要两个密钥中的一个即可。 此键是32字母数字字符的字符串。
* 本地主机（您的计算机）上的单个认知服务容器。 请确保可以：
  * 使用`docker pull`命令拉取映像。
  * 使用`docker run`命令通过所有必需的配置设置成功运行本地容器。
  * 调用容器的终结点，获取 HTTP 2xx 和 JSON 响应的响应。

尖括号`<>`中的所有变量都需要替换为自己的值。 此替换包括尖括号。

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](includes/create-container-instances-resource.md)]

## <a name="use-the-container-instance"></a>使用容器实例

1. 选择 "**概述**" 并复制 "IP 地址"。 它将是数字 IP 地址，例如`55.55.55.55`。
1. 打开新的浏览器选项卡，并使用 IP 地址（例如 `http://<IP-address>:5000 (http://55.55.55.55:5000`）。 你将看到容器的主页，让你知道容器正在运行。

1. 选择 "**服务 API 说明**" 以查看容器的 swagger 页面。

1. 选择任何**POST** api，并选择 "**试用**"。 将显示参数，包括输入。 填写参数。

1. 选择 "**执行**" 将请求发送到容器实例。

    已成功在 Azure 容器实例中创建并使用了认知服务容器。
