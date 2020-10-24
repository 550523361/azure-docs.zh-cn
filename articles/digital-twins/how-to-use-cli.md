---
title: 使用 Azure 数字孪生 CLI
titleSuffix: Azure Digital Twins
description: 请参阅如何开始使用和使用 Azure 数字孪生 CLI。
author: baanders
ms.author: baanders
ms.date: 05/25/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: c8eabd7d2ef02a92684b51de0bf45bdf7d995421
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494459"
---
# <a name="use-the-azure-digital-twins-cli"></a>使用 Azure 数字孪生 CLI

除了在 Azure 门户中管理 Azure 数字孪生实例以外，Azure 数字孪生还提供了一个 **命令行接口 (CLI) ** ，可用于对该服务执行大多数主要操作，包括：
* 管理 Azure 数字孪生实例
* 管理模型
* 管理数字孪生
* 管理克隆关系
* 配置终结点
* 管理 [路由](concepts-route-events.md)
* 通过基于 Azure 角色的访问控制 (Azure RBAC) 配置[安全性](concepts-security.md)

## <a name="uses-deploy-and-validate"></a>使用 (部署和验证) 

除了一般管理实例，CLI 也是用于部署和验证的有用工具。
* 使用控制平面命令可重复或自动部署新的实例。
* 数据平面命令可用于快速检查实例中的值，并验证操作是否按预期完成。

## <a name="get-the-extension"></a>获取扩展

Azure 数字孪生命令是 [适用于 Azure CLI 的 Azure IoT 扩展](https://github.com/Azure/azure-iot-cli-extension)的一部分。 您可以查看命令的完整列表及其用法，作为命令集的参考文档的一部分 `az iot` ： [ *az dt*命令参考](/cli/azure/ext/azure-iot/dt?preserve-view=true&view=azure-cli-latest)。

可以通过以下步骤确保具有最新版本的扩展。 可以在 [Azure Cloud Shell](../cloud-shell/overview.md) 或 [本地 Azure CLI](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest)中运行这些命令。

[!INCLUDE [digital-twins-cloud-shell-extensions.md](../../includes/digital-twins-cloud-shell-extensions.md)]

## <a name="next-steps"></a>后续步骤

通过参考文档浏览 CLI 及其完整的命令集：
* [*az dt* command reference](/cli/azure/ext/azure-iot/dt?preserve-view=true&view=azure-cli-latest)