---
title: Azure CLI 脚本示例 - 缩放 ACS 群集
description: Azure CLI 脚本示例 - 缩放 ACS 群集
author: iainfoulds
tags: acs, azure-container-service
keywords: Docker, 容器, 微服务, Kubernetes, DC/OS, Azure
ms.assetid: ''
ms.service: container-service
ms.devlang: azurecli
ms.topic: sample
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: 7e1136c179c5729f5ed0de189a90bbbb31412ab7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "76270658"
---
# <a name="deprecated-scale-an-azure-container-service-cluster"></a>（已弃用）缩放 Azure 容器服务群集

[!INCLUDE [ACS deprecation](../../../../includes/container-service-kubernetes-deprecation.md)]

此示例缩放 Azure 容器服务。 

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
az acs scale --resource-group myResourceGroup --name myK8SCluster --new-agent-count 5
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建部署。 表中的每一项均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az acs scale](/cli/azure/acs#az-acs-scale) | 缩放 ACS 群集。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure)。

可以在 [Azure 容器服务文档](../cli-samples.md)中找到其他 Azure 容器服务 CLI 脚本示例。

