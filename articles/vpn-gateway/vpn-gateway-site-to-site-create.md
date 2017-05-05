---
title: "将本地网络连接到 Azure 虚拟网络：站点到站点 VPN：经典门户 | Microsoft 文档"
description: "通过公共 Internet 创建从本地网络到 Azure 虚拟网络的 IPsec 连接的步骤。 这些步骤将帮助你使用经典门户和经典部署模型创建跨界站点到站点 VPN 网关连接。"
services: vpn-gateway
documentationcenter: 
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 024ecb29-64de-4ff1-84f1-1a45a8595f0b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/24/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: d0cedf73aa3f73e672a73b6abaca5eb8c22a76a7
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-classic-portal-classic"></a>使用经典门户创建具有站点到站点连接的 VNet（经典）

本文介绍如何使用经典门户创建站点到站点 VPN 网关连接，以便从本地网络连接到 VNet。 本文中的步骤适用于经典部署模型。 也可使用不同的部署工具或部署模型来创建此配置，方法是从以下列表中选择另一选项：

> [!div class="op_single_selector"]
> * [Resource Manager - Azure 门户](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager - CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [经典 - Azure 门户](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [经典 - 经典门户](vpn-gateway-site-to-site-create.md)
> 
>

![站点到站点 VPN 网关跨界连接示意图](./media/vpn-gateway-site-to-site-create/site-to-site-connection-diagram.png)


使用站点到站点 VPN 网关连接，通过 IPsec/IKE（IKEv1 或 IKEv2）VPN 隧道将本地网络连接到 Azure 虚拟网络。 此类型的连接要求位于本地的 VPN 设备分配有一个面向外部的公共 IP 地址。 有关 VPN 网关的详细信息，请参阅[关于 VPN 网关](vpn-gateway-about-vpngateways.md)。

#### <a name="additional-configurations"></a>其他配置

如果想要将多个 VNet 连接到一起，请参阅 [为经典部署模型配置 VNet 到 VNet 连接](virtual-networks-configure-vnet-to-vnet-connection.md)。 如果想要向已具有连接的 VNet 添加站点到站点连接，请参阅[使用现有 VPN 网关连接将 S2S 连接添加到 VNet](vpn-gateway-multi-site.md)。
## <a name="before-you-begin"></a>开始之前

[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

在开始配置之前，请确认具有以下各项：

* 一台兼容的 VPN 设备和能够对其进行配置的人员。 请参阅 [关于 VPN 设备](vpn-gateway-about-vpn-devices.md)。 如果不熟悉 VPN 设备的配置，或者不熟悉本地网络配置中的 IP 地址范围，则需咨询能够为你提供此类详细信息的人员。
* 一个用于 VPN 设备的面向外部的公共 IP 地址。 此 IP 地址不得位于 NAT 之后。
* Azure 订阅。 如果你还没有 Azure 订阅，可以激活 [MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details)或注册获取[免费帐户](https://azure.microsoft.com/pricing/free-trial)。

## <a name="CreateVNet"></a>创建虚拟网络
1. 登录到 [Azure 经典门户](https://manage.windowsazure.com/)。
2. 在屏幕左下角，单击“新建”。 在导航窗格中，单击“网络服务”，然后单击“虚拟网络”。 单击“自定义创建”以启动配置向导  。
3. 若要创建 VNet，请在以下页面上输入配置设置：

## <a name="Details"></a>虚拟网络详细信息页
输入以下信息：

* **名称**- 为虚拟网络命名。 例如 *EastUSVNet*。 在部署你的 VM 和 PaaS 实例时，将使用此虚拟网络名称，因此最好不要让此名称太复杂。
* **位置**：位置直接与你想让资源 (VM) 驻留在的物理位置（区域）有关。 例如，如果你希望部署到此虚拟网络的 VM 的物理位置位于 *美国东部*，请选择该位置。 创建虚拟网络后，将无法更改与虚拟网络关联的区域。

## <a name="DNS"></a>DNS 服务器和 VPN 连接页
请输入以下信息，然后单击右下的“下一步”箭头。

* **DNS 服务器**：输入 DNS 服务器名称和 IP 地址，或从快捷菜单中选择一个以前注册的 DNS 服务器。 此设置不创建 DNS 服务器。 此设置允许指定要用于对此虚拟网络进行名称解析的 DNS 服务器。
* **配置站点到站点 VPN**：选中“配置站点到站点 VPN”复选框。
* **本地网络**：本地网络代表你的物理本地位置。 你可以选择以前创建的本地网络，也可以创建一个新的本地网络。 但是，如果选择使用之前创建的本地网络，请转到“本地网络”  配置页面，并验证 VPN 设备的 VPN 设备 IP 地址（面向公众的 IPv4 地址）是否正确。

## <a name="Connectivity"></a>站点到站点连接页
如果要创建新的本地网络，则会看到“站点到站点连接”页。 如果你要使用此前创建的本地网络，则此页不会显示在向导中，你可以转到下一部分。

请输入以下信息，然后单击“下一步”箭头。

* **名称**：你希望用于称呼本地（内部）网络站点的名称。
* **VPN 设备 IP 地址**：本地 VPN 设备的面向公众的 IPv4 地址，可用来连接到 Azure。 VPN 设备不能位于 NAT 之后。
* **地址空间**：包括“起始 IP”和 CIDR（地址计数）。 指定要通过虚拟网络网关发送到本地内部位置的地址范围。 如果目标 IP 地址处于你在此处指定的范围之内，将通过虚拟网络网关路由它。
* **添加地址空间**：如果拥有多个要通过虚拟网络网关发送的地址范围，请指定每个额外的地址范围。 可以稍后在“本地网络”页中添加或删除范围。

## <a name="Address"></a>虚拟网络地址空间页
指定要用于虚拟网络的地址范围。 这些都是动态 IP 地址 (DIPS)，将分配给你部署到此虚拟网络的 VM 和其他角色实例。

所选范围不要与本地网络所用范围重叠，这一点尤其重要。 需要与网络管理员协调。 网络管理员可能需要从本地网络地址空间为你划分一个 IP 地址范围，以供你的虚拟网络使用。

输入以下信息，然后单击右下角的复选标记以配置你的网络。

* **地址空间**：包括起始 IP 和地址计数。 请验证你指定的地址空间不与本地网络的任一个地址空间相重叠。
* **添加子网**：包括起始 IP 和地址计数。 附加的子网不是必需的，但你可能需要为具有静态 DIP 的 VM 创建一个单独的子网。 或者，你可能需要在子网中拥有与其他角色实例分开的 VM。
* **添加网关子网**：单击此项可添加网关子网。 网关子网仅用于此虚拟网络网关，并且是此配置必需的。

单击页面底部的复选标记即可创建虚拟网络。 创建完成后，在 Azure 经典门户的“网络”页上，你将看到“状态”下面列出了“已创建”。 在创建 VNet 之后，你就可以配置虚拟网络网关。

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="VNetGateway"></a>配置虚拟网络网关
配置虚拟网络网关，创建安全的站点到站点连接。 请参阅 [在 Azure 经典门户中配置虚拟网络网关](vpn-gateway-configure-vpn-gateway-mp.md)。

## <a name="next-steps"></a>后续步骤
 连接完成后，即可将虚拟机添加到虚拟网络。 有关详细信息，请参阅[虚拟机](https://docs.microsoft.com/azure/#pivot=services&panel=Compute)。


