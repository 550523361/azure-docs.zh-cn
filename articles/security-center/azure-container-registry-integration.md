---
title: Azure 安全中心和 Azure 容器注册表
description: 了解 Azure 安全中心与 Azure 容器注册表的集成
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/19/2019
ms.author: memildin
ms.openlocfilehash: 1c1b48d3715d838827f88f99fc0849d25677fdcc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "80585745"
---
# <a name="azure-container-registry-integration-with-security-center"></a>Azure 容器注册表与安全中心集成

Azure 容器注册表（ACR）是一项托管的专用 Docker 注册表服务，用于在中央注册表中存储和管理 Azure 部署的容器映像。 它基于开源 Docker 注册表2.0。

如果你在 Azure 安全中心的标准层上，则可以添加容器注册表包。 此可选功能使你能够更深入地了解基于 ARM 的注册表中的映像的漏洞。 在订阅级别启用或禁用捆绑包，以涵盖订阅中的所有注册表。 此功能按映像收费，如[定价页](security-center-pricing.md)中所示。 启用容器注册表绑定，可确保安全中心已准备好扫描推送到注册表中的映像。 

每当将映像推送到注册表时，安全中心会自动扫描该映像。 若要触发映像扫描，请将其推送到存储库。

扫描完成后（通常在大约10分钟后），将在安全中心建议中获得结果，如下所示：

[![有关 azure 容器注册表（ACR）托管图像中发现的漏洞的示例 Azure 安全中心建议](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>集成的好处

安全中心在订阅中识别基于 ARM 的 ACR 注册表并无缝提供：

* 针对所有推送的 Linux 映像的**Azure 本机漏洞扫描**。 安全中心使用 Qualys 中业界领先的漏洞扫描供应商提供的扫描程序扫描映像。 默认情况下，此本机解决方案无缝集成。

* 针对具有已知漏洞的 Linux 映像的**安全建议**。 安全中心提供每个报告的漏洞和严重性分类的详细信息。 此外，它还提供了有关如何修正推送到注册表的每个映像上发现的特定漏洞的指南。

![Azure 安全中心和 Azure 容器注册表（ACR）高级概述](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)

## <a name="next-steps"></a>后续步骤

若要了解有关安全中心容器安全功能的详细信息，请参阅：

* [Azure 安全中心和容器安全性](container-security.md)

* [与 Azure Kubernetes 服务集成](azure-kubernetes-service-integration.md)

* [虚拟机保护](security-center-virtual-machine-protection.md)-介绍安全中心的建议