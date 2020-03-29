---
title: Azure Linux 虚拟机的安全控制 - Linux
description: 用于评估 Azure Linux 虚拟机的安全控制的清单
services: virtual-machines
ms.service: virtual-machines
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 300acaf4a9d2a11ef107e19df99452c909257d54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190575"
---
# <a name="security-controls-for-linux-virtual-machines"></a>Linux 虚拟机的安全控制

本文介绍了 Linux 虚拟机中内置的安全控制。

[!INCLUDE [Security controls header](../../../includes/security-controls-header.md)]

## <a name="network"></a>网络

| 安全控制 | Yes/No | 说明 |
|---|---|--|
| 服务终结点支持| 是 | |
| VNet 注入支持| 是 | |
| 网络隔离和防火墙支持| 是 |  |
| 强制隧道支持| 是 | 请参阅[使用 Azure 资源管理器部署模型配置强制隧道](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm)。 |

## <a name="monitoring--logging"></a>监视和日志记录

| 安全控制 | Yes/No | 说明|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | 请参阅[监视和更新 Azure 中的 Linux 虚拟机](/azure/virtual-machines/linux/tutorial-monitoring)。 |
| 控制和管理平面日志记录和审核| 是 |  |
| 数据平面日志记录和审核 | 否 |  |

## <a name="identity"></a>标识

| 安全控制 | Yes/No | 说明|
|---|---|--|
| 身份验证| 是 |  |
| 授权| 是 |  |

## <a name="data-protection"></a>数据保护

| 安全控制 | Yes/No | 说明 |
|---|---|--|
| 服务器端静态加密：微软管理的密钥 | 是 | 请参阅[适用于 Linux VM 的 Azure 磁盘加密](disk-encryption-overview.md)。 |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 是 | Azure 虚拟机支持 [ExpressRoute](/azure/expressroute) 和 VNet 加密。 请参阅 [VM 中的传输中加密](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms)。 |
| 服务器端静态加密：客户管理的密钥 (BYOK) | 是 | 客户托管密钥是受支持的 Azure 加密方案；请参阅 [Azure 加密概述](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms)。|
| 列级加密（Azure 数据服务）| 空值 | |
| 加密的 API 调用| 是 | 通过 HTTPS 和 TLS。 |

## <a name="configuration-management"></a>配置管理

| 安全控制 | Yes/No | 说明|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 |  | 

## <a name="next-steps"></a>后续步骤

- 详细了解 Azure[服务中的内置安全控件](../../security/fundamentals/security-controls.md)。
