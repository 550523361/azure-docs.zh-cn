---
title: InvalidNetworkSecurityGroupSecurityRules 错误 - Azure HDInsight
description: 群集创建失败，显示错误代码 InvalidNetworkSecurityGroupSecurityRules
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 1662be3bedb9ec0f2dc21e98ffbd57d8f8ed5777
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2020
ms.locfileid: "92540612"
---
# <a name="scenario-invalidnetworksecuritygroupsecurityrules---cluster-creation-fails-in-azure-hdinsight"></a>方案：InvalidNetworkSecurityGroupSecurityRules - 在 Azure HDInsight 中创建群集失败

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

你收到错误代码 `InvalidNetworkSecurityGroupSecurityRules`，其说明类似于“为子网配置的网络安全组中的安全规则不允许所需的入站和/或出站连接”。

## <a name="cause"></a>原因

可能是为群集配置的入站[网络安全组](../../virtual-network/virtual-network-vnet-plan-design-arm.md)规则有问题。

## <a name="resolution"></a>解决方法

转到 Azure 门户并找到与部署了群集的子网关联的 NSG。 在 **入站安全规则** 部分中，确保规则允许对 [此处](../control-network-traffic.md)提到的 IP 地址的端口 443 进行入站访问。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* 通过 [Azure 社区支持](https://azure.microsoft.com/support/community/)获取 Azure 专家的解答。

* 与 [@AzureSupport](https://twitter.com/azuresupport)（Microsoft Azure 官方帐户）联系，它可以将 Azure 社区与适当的资源（解答、支持人员和专家）相关联来改善客户体验。

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”，或打开“帮助 + 支持”中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](../../azure-portal/supportability/how-to-create-azure-support-request.md)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。