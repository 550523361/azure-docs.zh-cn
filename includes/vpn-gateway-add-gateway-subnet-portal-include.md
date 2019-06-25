---
title: include 文件
description: include 文件
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 04/04/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9132cf438cab518e20e6c2ddfdb7d0928753bd19
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2019
ms.locfileid: "67172797"
---
1. 在门户中，导航到要为其创建虚拟网关的虚拟网络。
2. 在 VNet 页的“设置”部分单击“子网”，展开“子网”页   。
3. 在“子网”页中，单击顶部的“+网关子网”打开“添加子网”页    。

   ![添加网关子网](./media/vpn-gateway-add-gateway-subnet-portal-include/gateway-subnet.png "添加网关子网")
  
4. 子网的“名称”  自动填充为值“GatewaySubnet”。 Azure 需要 GatewaySubnet 值才能识别作为网关子网的子网。 调整自动填充的**地址范围**值，使其匹配配置要求。

   ![添加网关子网](./media/vpn-gateway-add-gateway-subnet-portal-include/add-gateway-subnet.png "添加网关子网")
  
5. 若要创建子网，请单击页底部的“确定”  。
