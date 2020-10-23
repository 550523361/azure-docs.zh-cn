---
title: 方案：使用自定义设置通过 Nva 路由流量
titleSuffix: Azure Virtual WAN
description: 此方案有助于通过 Nva 使用不同的 NVA 路由流量来路由流量。
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 031cbb48a7e0c572866dc591d26fb1e6b6b12dba
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "92424723"
---
# <a name="scenario-route-traffic-through-nvas---custom-preview"></a>方案：通过 Nva、自定义 (预览路由流量) 

使用虚拟 WAN 虚拟中心路由时，有很多可用方案。 在此 NVA (网络虚拟设备) 方案，目的是通过 NVA 路由流量以便在 Vnet 和分支之间进行通信，并为 Internet 绑定的流量使用不同的 NVA。 有关虚拟中心路由的详细信息，请参阅[关于虚拟中心路由](about-virtual-hub-routing.md)。

## <a name="design"></a><a name="design"></a>设计

在此方案中，我们将使用命名约定：

* 连接到虚拟中心的虚拟网络的 "轮辐" (VNet 1、VNet 2 和) **图 1** 中的 vnet 3。
* 适用于虚拟网络的 "服务 VNet"，其中用户已在 **图 1**) 中部署了 NVA (VNet 4，以检查非 Internet 流量，并可能包含轮辐访问的公共服务。
* 适用于虚拟网络的 "DMZ VNet"，其中用户已部署了用于 **检查)  (** VNet 5 的 Internet 绑定流量。
* Microsoft 托管的虚拟 WAN 中心的 "中心"。

以下连接矩阵汇总了此方案中支持的流：

**连接性矩阵**

| 源          | 到:|*分支*|*服务 VNet*|*分支*|*Internet*|
|---|---|:---:|:---:|:---:|:---:|:---:|
| **分支**| &#8594;| 直接 |直接 | 通过服务 VNet |通过 DMZ VNet |
| **服务 VNet**| &#8594;| 直接 |不适用| 直接 | |
| **分支** | &#8594;| 通过服务 VNet |直接| 直接 |  |

连接矩阵中的每个单元都说明了连接是直接通过虚拟广域网还是通过 NVA Vnet 的一个。 让我们详细了解不同的行：

* 辐射
  * 轮辐会直接通过虚拟 WAN 中心访问其他轮辐。
  * 轮辐将通过指向服务 VNet 的静态路由来连接到分支。 它们不应从分支中了解特定的前缀 (否则，它们将更具体，并覆盖摘要) 。
  * 轮辐会通过直接 VNet 对等互连将 Internet 流量发送到外围 VNet。
* 分支：
  * 分支将通过指向服务 VNet 的静态路由来进入辐射。 它们不应从 Vnet 中了解用于替代汇总静态路由的特定前缀。
* 服务 VNet 将类似于需要从每个 VNet 和每个分支访问的共享服务 VNet。
* DMZ VNet 并不真正需要通过虚拟 WAN 建立连接，因为它所支持的唯一流量将会通过直接 VNet 对等互连来实现。 但是，我们将使用与外围 VNet 相同的连接模型来简化配置。

我们的连接矩阵提供三个不同的连接模式，这种模式可转换为三个路由表。 不同 Vnet 的关联将如下所示：

* 辐射
  * 关联的路由表： **RT_V2B**
  * 传播到路由表： **RT_V2B** 和 **RT_SHARED**
* NVA Vnet (Service VNet 和 DMZ VNet) ：
  * 关联的路由表：RT_SHARED
  * 传播到路由表： **RT_SHARED**
* 分支：
  * 关联的路由表：**默认**
  * 传播到路由表：RT_SHARED 和 Default

我们需要这些静态路由，以确保 VNet 到分支和分支到 VNet 的流量经过服务 VNet (VNet 4) 中的 NVA：

| 说明 | 路由表 | 静态路由              |
| ----------- | ----------- | ------------------------- |
| 分支    | RT_V2B      | 10.2.0.0/16-> vnet4conn  |
| NVA 轮辐  | 默认     | 10.1.0.0/16-> vnet4conn  |

现在，虚拟 WAN 知道要将数据包发送到的连接，但连接需要知道在接收这些数据包时要执行的操作：这是使用连接路由表的位置。

| 说明 | 连接 | 静态路由            |
| ----------- | ---------- | ----------------------- |
| VNet2Branch | vnet4conn  | 10.2.0.0/16-> 10.4.0。5 |
| Branch2VNet | vnet4conn  | 10.1.0.0/16-> 10.4.0。5 |

此时一切都应该到位。

有关虚拟中心路由的详细信息，请参阅[关于虚拟中心路由](about-virtual-hub-routing.md)。

## <a name="architecture"></a><a name="architecture"></a>体系结构

**图 1**中有一台集线器**1**。

* **集线器 1** 直接连接到 NVA vnet **Vnet 4** 和 **vnet 5**。

* Vnet 1、2和3之间的流量 (VPN/ER/P2S) 应通过 **VNet 4 NVA** 10.4.0.5。

* 来自 Vnet 1、2和3的所有 Internet 绑定流量应通过 **VNet 5 NVA** 10.5.0.5。

**图 1**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-1.png" alt-text="图 1":::

## <a name="workflow"></a><a name="workflow"></a>工作流

若要通过 NVA 设置路由，请执行以下步骤：

1. 为了使 Internet 绑定的流量通过 VNet 5 进行，需要 Vnet 1、2和3才能直接通过 VNet 对等互连连接到 VNet 5。 还需要在 Vnet 中设置为 0.0.0.0/0 和下一跃点10.5.0.5 的 UDR。 目前，虚拟 WAN 不允许虚拟中心的下一跃点 NVA 0.0.0.0/0。

1. 在 Azure 门户中，导航到你的虚拟中心，并创建自定义路由表 **RT_Shared** ，通过从所有 Vnet 和分支连接传播来了解路由。 在 **图 2**中，这是一个空的自定义路由表 **RT_Shared**。

   * **路由：** 无需添加任何静态路由。

   * **关联：** 选择 Vnet 4 和5，这意味着 Vnet 4 和5连接关联到 **RT_Shared**的路由表。

   * **传播：** 由于你希望所有分支和 VNet 连接都动态地将其路由传播到此路由表，因此请选择 "分支" 和 "所有 Vnet"。

1. 创建自定义路由表 **RT_V2B** 用于将流量从 vnet 1、2和3定向到分支。

   * **路由：** 在 **图) 2** 中为分支添加聚合静态路由条目 (VPN/ER/P2S)  (下一跃点作为 VNet 4 连接。 还需要为分支前缀在 VNet 4 的连接中配置静态路由，并指示下一跃点是 VNet 4 中 NVA 的特定 IP。

   * **关联：** 选择所有 Vnet 1、2和3。 这意味着 VNet 连接1、2和3将与此路由表相关联，并且可以通过传播) 此路由表中的传播来了解 (静态和动态的路由。

   * **传播：** 连接将路由传播到路由表。 选择 Vnet 1、2和3会启用将路由从 Vnet 1、2和3传播到此路由表。 不需要将路由从分支连接传播到 RT_V2B，因为分支 VNet 流量通过 VNet 4 中的 NVA。
  
1. 编辑默认路由表 **DefaultRouteTable**。

   所有 VPN、ExpressRoute 和用户 VPN 连接都与默认路由表相关联。 所有 VPN、ExpressRoute 和用户 VPN 连接将路由传播到同一组路由表。

   * **路由：** 在 **图) 2** 中，为 vnet 1、2和 3 (10.1.0.0/16 添加聚合静态路由条目，下一跃点是 VNet 4 连接。 还需要为 VNet 1、2和3聚合前缀在 VNet 4 的连接中配置静态路由，并指示下一跃点是 VNet 4 中 NVA 的特定 IP。

   * **关联：** 请确保选择 "分支 (VPN/ER/P2S) 的选项，确保本地分支连接与 *defaultroutetable*相关联。

   * **传播自：** 请确保选择 "分支 (VPN/ER/P2S) 的选项，确保本地连接将路由传播到 *defaultroutetable*。

**图 2**

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-2.png" alt-text="图 1" lightbox="./media/routing-scenarios/nva-custom/figure-2.png":::

## <a name="next-steps"></a>后续步骤

* 有关虚拟 WAN 的详细信息，请参阅[常见问题解答](virtual-wan-faq.md)。
* 有关虚拟中心路由的详细信息，请参阅[关于虚拟中心路由](about-virtual-hub-routing.md)。
