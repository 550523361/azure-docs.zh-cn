---
title: Azure 虚拟网络常见问题 | Microsoft 文档
description: 有关 Microsoft Azure 虚拟网络的常见问题的解答。
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 54bee086-a8a5-4312-9866-19a1fba913d0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2018
ms.author: jdial
ms.openlocfilehash: 6c429931a7a17ab62892ecc774a5cca15a532f72
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2018
ms.locfileid: "51237628"
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Azure 虚拟网络常见问题 (FAQ)

## <a name="virtual-network-basics"></a>虚拟网络基础知识

### <a name="what-is-an-azure-virtual-network-vnet"></a>Azure 虚拟网络 (VNet) 是什么？
Azure 虚拟网络 (VNet) 是自己的网络在云中的表示形式。 它是对专用于订阅的 Azure 云进行的逻辑隔离。 可以使用 VNet 设置和管理 Azure 中的虚拟专用网络 (VPN)，或者链接 VNet 与 Azure 中的其他 VNet，或链接本地 IT 基础结构，以创建混合或跨界解决方案。 创建的每个 VNet 具有其自身的 CIDR 块，只要 CIDR 块不重叠，即可链接到其他 VNet 和本地网络。 还可以控制 VNet 的 DNS 服务器设置并将 VNet 分离到子网中。

使用 VNet：

* 创建私有云专用的 VNet：有时，不需要对解决方案使用跨界配置。 创建 VNet 时，VNet 中的服务和 VM 可以在云中安全地互相直接通信。 在解决方案中，还可以为需要进行 Internet 通信的 VM 和服务配置终结点连接。
* 安全地扩展数据中心：借助 VNet，可以构建传统的站点到站点 (S2S) VPN，以便安全缩放数据中心的容量。 S2S VPN 使用 IPSEC 提供企业 VPN 网关和 Azure 之间的安全连接。
* 实现混合云方案：利用 VNet 可以灵活支持一系列混合云方案。 可以安全地将基于云的应用程序连接到任何类型的本地系统，例如大型机和 Unix 系统。

### <a name="how-do-i-get-started"></a>如何入门？
请访问[虚拟网络文档](https://docs.microsoft.com/azure/virtual-network/)帮助自己入门。 该内容提供了所有 VNet 功能的概述和部署信息。

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>没有跨界连接的情况下是否可以使用 VNet？
是的。 可以在不连接到本地的情况下使用 VNet。 例如，可以在 Azure VNet 中单独运行 Microsoft Windows Server Active Directory 域控制器和 SharePoint 场。

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>是否可以在 VNet 之间或者 VNet 与本地数据中心之间执行 WAN 优化？
是的。 可以通过 Azure 市场部署许多供应商提供 [WAN 优化网络虚拟设备](https://azure.microsoft.com/marketplace/?term=wan+optimization)。

## <a name="configuration"></a>配置

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>要使用哪些工具创建 VNet？
可以使用以下工具创建或配置 VNet：

* Azure 门户
* PowerShell
* Azure CLI
* 网络配置文件（netcfg - 仅用于经典 VNet）。 请参阅[使用网络配置文件配置 VNet](virtual-networks-using-network-configuration-file.md) 一文。

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>在我的 VNet 中可以使用哪些地址范围？
[RFC 1918](http://tools.ietf.org/html/rfc1918)中定义的任何 IP 地址范围。 例如 10.0.0.0/16。

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>我的 VNet 中是否可以有公共 IP 地址？
是的。 有关公用 IP 地址范围的详细信息，请参阅[创建虚拟网络](manage-virtual-network.md#create-a-virtual-network)。 无法从 Internet 直接访问公用 IP 地址。

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-vnet"></a>VNet 中的子网数量是否有限制？
是的。 有关详细信息，请参阅 [Azure 限制](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits)。 子网地址空间不能相互重叠。

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>使用这些子网中的 IP 地址是否有任何限制？
是的。 Azure 会保留每个子网中的某些 IP 地址。 每个子网的第一个和最后一个 IP 地址将为协议一致性而保留，每个子网的 x.x.x.1-x.x.x.3 地址用于 Azure 服务。

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>VNet 和子网的最小和最大容量是多少？
支持的最小子网为 /29，最大为 /8（使用 CIDR 子网定义）。

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>是否可以使用 VNet 将 VLAN 引入 Azure 中？
不是。 VNet 是第 3 层重叠。 Azure 不支持任何第 2 层语义。

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>是否可以在 VNet 和子网上指定自定义路由策略？
是的。 你可以创建路由表并将其关联到子网。 有关 Azure 中的路由的详细信息，请参阅[路由概述](virtual-networks-udr-overview.md#custom-routes)。

### <a name="do-vnets-support-multicast-or-broadcast"></a>VNet 是否支持多播或广播？
不是。 不支持多播和广播。

### <a name="what-protocols-can-i-use-within-vnets"></a>在 VNet 中可以使用哪些协议？
可以在 VNet 中使用 TCP、UDP 和 ICMP TCP/IP 协议。 VNet 内支持单播放，但通过单播（源端口 UDP/68/目标端口 UDP/67）的动态主机配置协议 (DHCP) 除外。 VNet 中会阻止多播、广播、在 IP 里面封装 IP 的数据包以及通用路由封装 (GRE) 数据包。 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>是否可以在 VNet 中 ping 默认路由器？
不是。

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>是否可以使用 tracert 诊断连接？
不是。

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>创建 VNet 后是否可以添加子网？
是的。 可以随时向 VNet 中添加子网，只要子网地址范围不是另一子网的一部分并且虚拟网络的地址范围中有剩余的可用空间。

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>创建后是否可以修改子网的大小？
是的。 如果子网中未部署任何 VM 或服务，可以添加、删除、扩展或收缩该子网。

### <a name="can-i-modify-subnets-after-i-created-them"></a>创建子网后是否可以对其进行修改？
是的。 可以添加、删除和修改 VNet 使用的 CIDR 块。

### <a name="if-i-am-running-my-services-in-a-vnet-can-i-connect-to-the-internet"></a>如果我在 VNet 中运行服务，是否可以连接到 Internet？
是的。 VNet 中部署的所有服务都可以在出站方向连接到 Internet。 若要详细了解 Azure 中的出站 Internet 连接，请参阅[出站连接](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json)。 如果希望在入站方向连接到通过资源管理器部署的某个资源，该资源必须具有分配给它的公用 IP 地址。 若要详细了解公用 IP 地址，请参阅[公用 IP 地址](virtual-network-public-ip-address.md)。 Azure 中部署的每个云服务都具有分配给它的可公开寻址的 VIP。 你将定义 PaaS 角色的输入终结点和虚拟机的终结点，以使这些服务可以接受来自 Internet 的连接。

### <a name="do-vnets-support-ipv6"></a>VNet 是否支持 IPv6？
不是。 目前不能将 IPv6 用于 VNet。 但是，可以将 IPv6 地址分配给 Azure 负载均衡器来对虚拟机进行负载均衡。 有关详细信息，请参阅 [Azure 负载均衡器的 IPv6 概述](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)。

### <a name="can-a-vnet-span-regions"></a>VNet 是否可以跨区域？
不是。 一个 VNet 限制为单个区域。 但是，虚拟网络可以跨可用性区域。 若要详细了解可用性区域，请参阅[可用性区域概述](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)。 可以通过虚拟网络对等互连来连接不同区域中的虚拟网络。 有关详细信息，请参阅[虚拟网络对等互连概述](virtual-network-peering-overview.md)

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>是否可以将 VNet 连接到 Azure 中的另一个 VNet？
是的。 可以使用以下任一方式将一个 VNet 连接到另一个 VNet：
- **虚拟网络对等互连**：有关详细信息，请参阅 [VNet 对等互连概述](virtual-network-peering-overview.md)
- **Azure VPN 网关**：有关详细信息，请参阅[配置 VNet 到 VNet 连接](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)。 

## <a name="name-resolution-dns"></a>名称解析 (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>VNet 的 DNS 选项有哪些？
使用 [VM 和角色实例的名称解析](virtual-networks-name-resolution-for-vms-and-role-instances.md)页上的决策表，可引导用户浏览可用的所有 DNS 选项。

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>是否可以为 VNet 指定 DNS 服务器？
是的。 可以在 VNet 设置中指定 DNS 服务器 IP 地址。 此设置将应用为 VNet 中的所有 VM 的默认 DNS 服务器。

### <a name="how-many-dns-servers-can-i-specify"></a>可以指定多少 DNS 服务器？
请参考 [Azure 限制](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits)。

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>创建网络后是否可以修改 DNS 服务器？
是的。 可以随时更改 VNet 的 DNS 服务器列表。 如果更改 DNS 服务器列表，则需要重新启动 VNet 中的每个 VM，以使其拾取新的 DNS 服务器。

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>什么是 Azure 提供的 DNS？它是否适用于 VNet？
Azure 提供的 DNS 是由 Microsoft 提供的多租户 DNS 服务。 Azure 在此服务中注册所有 VM 和云服务角色实例。 此服务通过主机名为相同云服务内包含的 VM 和角色实例提供名称解析，并通过 FQDN 为相同 VNet 中的 VM 和角色实例提供名称解析。 若要详细了解 DNS，请参阅 [VM 和云服务角色实例的名称解析](virtual-networks-name-resolution-for-vms-and-role-instances.md)。

使用 Azure 提供的 DNS 进行跨租户名称解析时，VNet 中的前 100 个云服务存在限制。 如果使用自己的 DNS 服务器，此限制则不适用。

### <a name="can-i-override-my-dns-settings-on-a-per-vm-or-cloud-service-basis"></a>是否可以基于每个 VM 或云服务重写 DNS 设置？
是的。 可以基于每个 VM 或云服务设置 DNS 服务器，以替代默认网络设置。 但是，建议尽可能使用网络级别的 DNS。

### <a name="can-i-bring-my-own-dns-suffix"></a>是否可以引入我自己的 DNS 后缀？
不是。 不能为 VNet 指定自定义的 DNS 后缀。

## <a name="connecting-virtual-machines"></a>连接虚拟机

### <a name="can-i-deploy-vms-to-a-vnet"></a>是否可以将 VM 部署到 VNet？
是的。 附加到通过资源管理器部署模型部署的 VM 的所有网络接口 (NIC) 必须连接到 VNet。 可以选择性地将通过经典部署模型部署的 VM 连接到 VNet。

### <a name="what-are-the-different-types-of-ip-addresses-i-can-assign-to-vms"></a>可向 VM 分配哪些不同类型的 IP 地址？
* **专用：** 分配到每个 VM 中的每个 NIC。 使用静态或动态方法分配地址。 应该分配 VNet 子网设置中指定的范围内的专用 IP 地址。 将为通过经典部署模型部署的资源分配专用 IP 地址，即使它们未连接到 VNet。 分配方法的行为根据资源是通过资源管理器还是通过经典部署模型部署的而不同： 

  - **资源管理器**：使用动态或静态方法分配的专用 IP 地址保持分配给虚拟机（资源管理器），直到该资源被删除。 差别在于，使用静态方法时由你来选择地址，而使用动态方法时由 Azure 来选择地址。 
  - **经典**：如果虚拟机（经典）VM 在处于停止（解除分配）状态后重新启动，则使用动态方法分配的的专用 IP 地址可能会变化。 如果需要确保通过经典部署模型部署的资源的专用 IP 地址永远不会变化，请使用静态方法分配专用 IP 地址。

* **公共：** 选择性地分配给附加到通过 Azure 资源管理器部署模型部署的 VM 的 NIC。 可以使用静态或动态分配方法分配地址。 通过经典部署模型部署的所有 VM 和云服务角色实例位于分配有*动态*公共虚拟 IP (VIP) 地址的云服务中。 可以选择性地将某个公共*静态* IP 地址（称为[保留 IP 地址](virtual-networks-reserved-public-ip.md)）分配为 VIP。 可将公共 IP 地址分配给通过经典部署模型部署的单个 VM 或云服务角色实例。 这些地址称为[实例级公共 IP (ILPIP](virtual-networks-instance-level-public-ip.md) 地址，可动态分配。

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>是否可为以后创建的 VM 保留专用 IP 地址？
不是。 无法保留专用 IP 地址。 如果某个专用 IP 地址可用，则 DHCP 服务器会将其分配给某个 VM 或角色实例。 该 VM 可能是你希望将专用 IP 地址分配到的 VM，也可能不是。 但是，可将已创建的 VM 的专用 IP 地址更改为任何可用的专用 IP 地址。

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>VNet 中 VM 的专用 IP 地址是否会变化？
视情况而定。 如果 VM 是通过资源管理器部署的，则无论 IP 地址是使用静态还是动态分配方法分配的，该 IP 地址都不会变化。 如果 VM 是通过经典部署模型部署的，则动态 IP 地址在 VM 处于停止（解除分配）状态后重新启动时可能会变化。 当删除通过任一部署模型部署的 VM 时，会从该 VM 释放地址。

### <a name="can-i-manually-assign-ip-addresses-to-nics-within-the-vm-operating-system"></a>是否可以在 VM 操作系统中手动将 IP 地址分配到 NIC？
可以，但是除非必要，不建议这样做，例如为虚拟机分配多个 IP 地址时。 有关详细信息，请参阅[为虚拟机添加多个 IP 地址](virtual-network-multiple-ip-addresses-portal.md#os-config)。 如果分配给附加到 VM 的 Azure NIC 的 IP 地址更改，并且 VM 操作系统内的 IP 地址不同，则会丢失到 VM 的连接。

### <a name="if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-the-operating-system-what-happens-to-my-ip-addresses"></a>如果在操作系统中停止云服务部署槽或关闭 VM，IP 地址会发生什么情况？
无变化。 IP 地址（公共 VIP、公共和专用）将保留分配给该云服务部署槽或 VM。

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-redeploying"></a>在无需重新部署的情况下，是否可以将 VM 从一个子网移动到 VNet 中的另一个子网？
是的。 可在[如何将 VM 或角色实例移到其他子网](virtual-networks-move-vm-role-to-subnet.md)一文中找到详细信息。

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>是否可以为我的 VM 配置静态 MAC 地址？
不是。 MAC 地址不能以静态方式配置。

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-its-created"></a>创建 VM 后，其 MAC 地址是否将保持不变？
是的，通过 Resource Manager 和经典部署模型部署的 VM 在被删除之前，其 MAC 地址将保持不变。 以前，如果停止（解除分配）VM，会释放 MAC 地址，但现在，即使 VM 处于解除分配状态，也会保留其 MAC 地址。

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>是否可以通过 VNet 中的 VM 连接到 Internet？
是的。 VNet 中部署的所有 VM 和云服务角色实例都可连接到 Internet。

## <a name="azure-services-that-connect-to-vnets"></a>连接到 VNet 的 Azure 服务

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>是否可以在 VNet 中使用 Azure 应用服务 Web 应用？
是的。 可以使用 ASE（应用服务环境）在 VNet 中部署 Web 应用。 如果为 VNet 配置了点到站点连接，则所有 Web 应用都可以安全地连接和访问 VNet 中的资源。 有关详细信息，请参阅以下文章：

* [在应用服务环境中创建 Web 应用](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [将应用与 Azure 虚拟网络集成](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [将 VNet 集成和混合连接用于 Web 应用](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json#hybrid-connections-and-app-service-environments)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>是否可以在 VNet 中部署云服务与 Web 和辅助角色 (PaaS)？
是的。 （可选）可在 VNet 中部署云服务角色实例。 为此，请在服务配置的网络配置部分中指定 VNet 名称和角色/子网映射。 不需要更新任何二进制文件。

### <a name="can-i-connect-a-virtual-machine-scale-set-vmss-to-a-vnet"></a>是否可将虚拟机规模集 (VMSS) 连接到 VNet？
是的。 必须将 VMSS 连接到 VNet。

### <a name="is-there-a-complete-list-of-azure-services-that-can-i-deploy-resources-from-into-a-vnet"></a>是否存在我可以将其中的资源部署到 VNet 的 Azure 服务完整列表？
是的，有关详细信息，请参阅[Azure 服务的虚拟网络集成](virtual-network-for-azure-services.md)。

### <a name="which-azure-paas-resources-can-i-restrict-access-to-from-a-vnet"></a>可以从 VNet 限制对哪些 Azure PaaS 资源的访问？

通过某些 Azure PaaS 服务（例如 Azure 存储和 Azure SQL 数据库）部署的资源只能通过使用虚拟网络服务终结点限制对 VNet 中的资源的访问。 有关详细信息，请参阅[虚拟网络概述](virtual-network-service-endpoints-overview.md)。

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>是否可以将服务移入和移出 VNet？
不是。 不能将服务移入和移出 VNet。 若要将某个资源移动到另一个 VNet，必须删除并重新部署该资源。

## <a name="security"></a>安全

### <a name="what-is-the-security-model-for-vnets"></a>VNet 的安全模型是什么？
VNet 相互之间以及与 Azure 基础结构中托管的其他服务之间相互隔离。 VNet 是一条信任边界。

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-to-vnet-connected-resources"></a>是否可以限制入站或出站流量流向与 VNet 连接的资源？
是的。 可向 VNet 中的单个子网和/或附加到 VNet 的 NIC 应用[网络安全组](security-overview.md)。

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>是否可在与 VNet 连接的资源之间实施防火墙？
是的。 可以通过 Azure 市场部署许多供应商提供[防火墙网络虚拟设备](https://azure.microsoft.com/marketplace/?term=firewall)。

### <a name="is-there-information-available-about-securing-vnets"></a>是否有介绍如何保护 VNet 的信息？
是的。 有关详细信息，请参阅 [Azure 网络安全概述](../security/security-network-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)。

## <a name="apis-schemas-and-tools"></a>API、架构和工具

### <a name="can-i-manage-vnets-from-code"></a>是否可以通过代码管理 VNet？
是的。 可在 [Azure 资源管理器](/rest/api/virtual-network)和[经典（服务管理）](https://go.microsoft.com/fwlink/?LinkId=296833)部署模型中使用适用于 VNet 的 REST API。

### <a name="is-there-tooling-support-for-vnets"></a>是否有 VNet 的工具支持？
是的。 详细了解以下操作：
- 使用 Azure 门户通过 [Azure 资源管理器](manage-virtual-network.md#create-a-virtual-network)和[经典](virtual-networks-create-vnet-classic-pportal.md)部署模型部署 VNet。
- 使用 PowerShell 管理通过[资源管理器](/powershell/module/azurerm.network)和[经典](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0)部署模型部署的 VNet。
- 使用 Azure 命令行接口 (CLI) 管理通过[资源管理器](/cli/azure/network/vnet)和[经典](../virtual-machines/azure-cli-arm-commands.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-commands-to-manage-network-resources)部署模型部署的 VNet。  

## <a name="vnet-peering"></a>VNet 对等互连

### <a name="what-is-vnet-peering"></a>什么是 VNet 对等互连？
使用 VNet 对等互连（或虚拟网络对等互连）可连接虚拟网络。 使用虚拟网络之间的 VNet 对等互连连接，可通过 IPv4 地址在这些虚拟网络之间私下路由流量。 对等互连的 VNet 中的虚拟机可相互通信，如同它们处于同一网络中一样。 这些虚拟网络可以位于相同区域或不同区域中（也称为全球 VNet 对等互连）。 此外，还可跨 Azure 订阅创建 VNet 对等互连连接。

### <a name="can-i-create-a-peering-connection-to-a-vnet-in-a-different-region"></a>是否可以在另一区域创建到 VNet 的对等互连连接？
是的。 全球 VNet 对等互连可以将不同区域中的 VNet 对等互连。 全球 VNet 对等互连适用于所有 Azure 公共区域。 不能通过全球对等互连的方式从 Azure 公共区域连接到国家/地区云。 全球对等互连目前不适用于国家/地区云。

### <a name="can-i-enable-vnet-peering-if-my-virtual-networks-belong-to-subscriptions-within-different-azure-active-directory-tenants"></a>如果虚拟网络所属的订阅位于不同的 Azure Active Directory 租户中，能否启用 VNet 对等互连？
是的。 如果订阅属于不同的 Azure Active Directory 租户，则可以建立 VNet 对等互连（无论是本地还是全球）。 可以通过 PowerShell 或 CLI 来执行此操作。 尚不支持门户。

### <a name="my-vnet-peering-connection-is-in-initiated-state-why-cant-i-connect"></a>我的 VNet 对等互连连接处于“已启动”状态，为什么我不能连接？
如果对等互连连接处于“已启动”状态，则意味着只创建了一个链接。 必须创建双向链接才能成功地建立连接。 例如，若要从 VNet A 对等互连到 VNet B，必须创建从 VNetA 到 VNetB 以及从 VNetB 到 VNetA 的链接。 创建两个链接后，状态会更改为“已连接”。

### <a name="my-vnet-peering-connection-is-in-disconnected-state-why-cant-i-create-a-peering-connection"></a>我的 VNet 对等互连连接处于“已断开连接”状态，为什么我无法创建对等互连连接？
VNet 对等互连连接处于“已断开连接”状态意味着创建的某个链接已被删除。 若要重新建立对等互连连接，需要删除该链接并重新创建。

### <a name="can-i-peer-my-vnet-with-a-vnet-in-a-different-subscription"></a>是否可以将我的 VNet 与另一订阅中的 VNet 对等互连？
是的。 可以跨订阅和跨区域进行 VNet 对等互连。

### <a name="can-i-peer-two-vnets-with-matching-or-overlapping-address-ranges"></a>是否可以将两个地址范围匹配或重叠的 VNet 对等互连？
不是。 若要启用 VNet 对等互连，地址空间不得重叠。

### <a name="how-much-do-vnet-peering-links-cost"></a>VNet 对等互连链接的费用如何？
创建 VNet 对等互连连接不收费。 跨对等互连连接进行数据传输收费。 请[参阅此文](https://azure.microsoft.com/pricing/details/virtual-network/)。

### <a name="is-vnet-peering-traffic-encrypted"></a>VNet 对等互连流量是否加密？
不是。 对等互连 VNet 中的资源之间的流量是专用的，处于隔离状态。 它始终局限在 Microsoft 主干上。

### <a name="why-is-my-peering-connection-in-a-disconnected-state"></a>为什么我的对等互连连接处于已断开状态？
删除某个 VNet 对等互连链接时，VNet 对等互连连接就会进入“已断开”状态。 必须删除两个链接才能重新建立成功的对等互连连接。

### <a name="if-i-peer-vneta-to-vnetb-and-i-peer-vnetb-to-vnetc-does-that-mean-vneta-and-vnetc-are-peered"></a>如果我从 VNetA 对等互连到 VNetB，然后又从 VNetB 对等互连到 VNetC，这是否意味着 VNetA 和 VNetC 已对等互连？
不是。 不支持可传递对等互连。 必须单独将 VNetA 和 VNetC 对等互连。

### <a name="are-there-any-bandwidth-limitations-for-peering-connections"></a>对等互连连接是否存在带宽限制？
不是。 VNet 对等互连不管是本地的还是全球的，都没有任何带宽限制。 带宽仅受 VM 或计算资源的限制。

## <a name="virtual-network-tap"></a>虚拟网络 TAP

### <a name="which-azure-regions-are-available-for-virtual-network-tap"></a>可以在哪些 Azure 区域使用虚拟网络 TAP？
在开发人员预览版期间，此功能将在美国中西部区域提供。 受监视的网络接口、虚拟网络 TAP 资源和收集器或分析解决方案必须部署在同一区域中。

### <a name="does-virtual-network-tap-support-any-filtering-capabilities-on-the-mirrored-packets"></a>虚拟网络 TAP 是否支持对镜像数据包使用任何筛选功能？
虚拟网络 TAP 预览版不支持筛选功能。 当 TAP 配置被添加到网络接口后，此网络接口上所有入口和出口流量的一个深层副本会被流式传输到 TAP 目标。

### <a name="can-multiple-tap-configurations-be-added-to-a-monitored-network-interface"></a>是否可以向受监视的网络接口添加多个 TAP 配置？
受监视的网络接口仅能拥有一个 TAP 配置。 查看单个[合作伙伴解决方案](virtual-network-tap-overview.md#virtual-network-tap-partner-solutions)，寻找将 TAP 流量的多个副本流式传输到你所选择的分析工具的功能。

### <a name="can-the-same-virtual-network-tap-resource-aggregate-traffic-from-monitored-network-interfaces-in-more-than-one-virtual-network"></a>同一虚拟网络 TAP 资源是否可以聚合多个虚拟网络中来自受监视的网络接口的流量？
是的。 同一虚拟网络 TAP 资源可用于聚合同一订阅或不同订阅中的对等虚拟网络中来自受监视的网络接口的镜像流量。 虚拟网络 TAP 资源和目标负载均衡器或目标网络接口必须位于同一订阅中。 所有订阅必须在同一 Azure Active Directory 租户下。

### <a name="are-there-any-performance-considerations-on-production-traffic-if-i-enable-a-virtual-network-tap-configuration-on-a-network-interface"></a>如果我在网络接口上启用虚拟网络 TAP 配置，是否需要考虑生产流量的性能问题？

虚拟网络 TAP 现为开发人员预览版。 在预览版期间，没有服务级别协议。 容量不应用于生产工作负荷。 使用 TAP 配置启用虚拟网络接口后，Azure 主机上分配给虚拟机以发送生产流量的相同资源将用于执行镜像功能并发送镜像数据包。 选择正确的 [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 或 [Windows](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 虚拟机大小，确保有足够的资源可用于虚拟机以发送生产流量和镜像流量。

### <a name="is-accelerated-networking-for-linuxcreate-vm-accelerated-networking-climd-or-windowscreate-vm-accelerated-networking-powershellmd-supported-with-virtual-network-tap"></a>虚拟网络 TAP 是否支持适用于 [Linux](create-vm-accelerated-networking-cli.md) 或 [Windows](create-vm-accelerated-networking-powershell.md) 的加速网络？

你将能够在附加到已启用加速网络的虚拟机的网络接口上添加 TAP 配置。 但是，通过添加 TAP 配置将使虚拟机上的性能和延迟情况受到影响，因为 Azure 加速网络目前不支持卸载镜像流量。
