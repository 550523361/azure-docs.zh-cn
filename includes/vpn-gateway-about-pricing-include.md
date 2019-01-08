---
title: include 文件
description: include 文件
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: ae36adc78d8c87d85c64fd61cb3a50dfcae60b85
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53995652"
---
支付两项内容：虚拟网络网关的每小时计算成本和虚拟网络网关的出口数据传输。 可在 [定价](https://azure.microsoft.com/pricing/details/vpn-gateway) 页上找到定价信息。

**虚拟网络网关计算成本**<br>每个虚拟网络网关都有每小时计算成本。 价格基于创建虚拟网络网关时指定的网关 SKU。 成本与网关本身以及流经网关的数据传输相关。 主动-主动设置的成本与主动-被动设置的成本相同。

**数据传输成本**<br>数据传输成本根据源虚拟网络网关的出口流量计算。

* 如果要将流量发送到本地 VPN 设备，以 Internet 出口数据传输率收取费用。
* 如果要在不同区域的虚拟网络之间发送流量，定价因区域而异。
* 如果要仅在属于同一区域的虚拟网络之间发送流量，则没有数据成本。 同一区域的 VNet 之间的流量免费。