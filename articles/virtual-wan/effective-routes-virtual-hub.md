---
title: 查看虚拟中心的有效路由： Azure 虚拟 WAN |Microsoft Docs
description: 查看 Azure 虚拟 WAN 中的虚拟中心的有效路由
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: cherylmc
ms.openlocfilehash: 1173da81736661048d1e4e12d9919bc2aadf73ee
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "73515845"
---
# <a name="view-effective-routes-of-a-virtual-hub"></a>查看虚拟中心的有效路由

可以在 Azure 门户中查看虚拟 WAN 中心的所有路由。 若要查看路由，请导航到虚拟中心，然后选择“路由”>“查看有效路由”  。

## <a name="understanding-routes"></a><a name="understand"></a>了解路由

以下示例可帮助你更好地了解虚拟 WAN 路由的显示方式。

在此示例中，我们有一个具有三个中心的虚拟 WAN。 第一个中心位于 "美国东部" 区域，第二个中心位于 "西欧" 区域中，第三个中心位于 "美国西部" 区域。 在虚拟 WAN 中，所有中心都是互联的。 在此示例中，我们假定美国东部和西欧中心与本地分支（轮辐）和 Azure 虚拟网络（轮辐）建立连接。

包含网络虚拟设备 (10.4.0.6) 的 Azure VNet 辐射 (10.4.0.0/16) 进一步对等互连到一个 VNet (10.5.0.0/16)。 有关中心路由表的详细信息，请参阅本文下文中的[其他信息](#abouthubroute)。

在此示例中，我们还假定西欧分支1连接到美国东部中心和西欧中心。 美国东部的 ExpressRoute 线路将分支2连接到美国东部中心。

![示意图](./media/effective-routes-virtual-hub/diagram.png)

## <a name="view-effective-routes"></a><a name="view"></a>查看有效路由

当你在门户中选择 "查看有效路由" 时，它将生成在美国东部中心的[中心路由表](#routetable)中显示的输出。

若要将其放在透视中，第一行意味着美国东部中心已经了解 10.20.1.0/24 （Branch 1）的路线，因为 VPN*下一跃点类型*连接（"下一跃点" Vpn 网关 Instance0 ip 10.1.0.6，Instance1 IP 10.1.0.7）。 “路由原点”  指向资源 ID。 “AS 路径”  表示分支 1 的 AS 路径。

### <a name="hub-route-table"></a><a name="routetable"></a>中心路由表

可以使用表底部的滚动条查看“AS 路径”。

| **Prefix** |  **下一跃点类型** | **下一跃点** |  **路由原点** |**AS 路径** |
| ---        | ---                | ---          | ---               | ---         |
| 10.20.1.0/24|VPN |10.1.0.6、10.1.0.7| /subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-eastus-gw| 20000|
|10.21.1.0/24 |ExpressRoute|10.1.0.10、10.1.0.11|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/expressRouteGateways/4444a6ac74e4d85555-eastus-gw|21000|
|10.23.1.0/24| VPN |10.1.0.6、10.1.0.7|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-eastus-gw|23000|
|10.4.0.0/16|虚拟网络连接| 在链路上 |  |  |
|10.5.0.0/16| IP 地址| 10.4.0.6|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/easthub_1/routeTables/table_1| |
|0.0.0.0/0| IP 地址| `<Azure Firewall IP>` |/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/easthub_1/routeTables/table_1| |
|10.22.1.0/16| 远程中心|10.8.0.6、10.8.0.7|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/microsoft.network/virtualhubs/westhub_| 4848-22000 |
|10.9.0.0/16| 远程中心|  在链路上 |/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/microsoft.network/virtualhubs/westhub_1| |

>[!NOTE]
> 如果美国东部和西欧中心未在示例拓扑中相互通信，则获知的路由（10.9.0.0/16）不存在。 中心仅播发直接连接到它们的网络。
>

## <a name="additional-information"></a><a name="additional"></a>其他信息

### <a name="about-the-hub-route-table"></a><a name="abouthubroute"></a>关于中心路由表

可以创建一个虚拟中心路由，并将该路由应用于虚拟中心路由表。 可以将多个路由应用于虚拟中心路由表。 这允许你通过 IP 地址（通常是辐射 VNet 中的网络虚拟设备 (NVA)）设置目标 VNet 的路由。 有关 NVA 的详细信息，请参阅[将流量从虚拟中心路由到 NVA](virtual-wan-route-table-portal.md)。

### <a name="about-default-route-00000"></a><a name="aboutdefaultroute"></a>关于默认路由（0.0.0.0/0）

如果连接上的标志为 "Enabled"，则虚拟中心能够将已获知的默认路由传播到虚拟网络、站点到站点 VPN 和 ExpressRoute 连接。 当你编辑虚拟网络连接、VPN 连接或 ExpressRoute 连接时，将显示此标志。 默认情况下，“EnableInternetSecurity”在中心 VNet、ExpressRoute 和 VPN 连接上始终为 false。

默认路由并非源自虚拟 WAN 中心。 当虚拟 WAN 中心由于在中心部署防火墙而获知了默认路由或另一个已连接的站点已启用强制隧道时，将会传播默认路由。

## <a name="next-steps"></a>后续步骤

有关虚拟 WAN 的详细信息，请参阅[虚拟 WAN 概述](virtual-wan-about.md)。