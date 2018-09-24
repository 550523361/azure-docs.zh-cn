---
title: include 文件
description: include 文件
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: df9c291136fc6a48effb08cd59eeb66486eb641c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003615"
---
### <a name="virtual-networks"></a>虚拟网络

|  |  |
|---------|---------|
| [允许的应用程序网关 SKU](../articles/governance/policy/samples/allowed-app-gate-sku.md) | 要求应用程序网关使用已批准的 SKU。 指定一个已批准的 SKU 的数组。 |
| [无网络与 ER 网络对等](../articles/governance/policy/samples/no-peering-er-net.md) | 禁止将网络对等关联到指定资源组中的网络。 用于防止与中心托管的网络基础结构连接。 指定要防止关联的资源组的名称。 |
| [无用户定义的路由表](../articles/governance/policy/samples/no-user-def-route-table.md)  |禁止通过用户定义的路由表部署虚拟网络。 |
| [每个子网上的 NSG X](../articles/governance/policy/samples/nsg-on-subnet.md) | 要求将特定网络安全组用于所有子网。 指定要使用的网络安全组的 ID。 |
| [对 VM 网络接口使用已批准的子网](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) | 要求网络接口使用已批准的子网。 指定已批准的子网的 ID。 |
| [对 VM 网络接口使用已批准的 vNet](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) | 要求网络接口使用已批准的虚拟网络。 由你指定已批准的虚拟网络的 ID。 |