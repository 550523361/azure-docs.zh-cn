---
title: Azure 上的 SAP HANA 基础结构配置和操作 | Microsoft Docs
description: Azure 虚拟机上部署的 SAP HANA 系统的操作指南。
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/01/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 07c8f84f2e37abd87953d8e4cb20b37258b25fda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920450"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Azure 上的 SAP HANA 基础结构配置和操作
本文档提供有关配置 Azure 基础结构以及操作 Azure 本机虚拟机 (VM) 上部署的 SAP HANA 系统的指导。 本文档还包含有关 M128s VM SKU 的 SAP HANA 横向扩展的配置信息。 本文档并不旨在取代标准 SAP 文档，后者包括以下内容：

- [SAP 管理指南](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP 安装指南](https://service.sap.com/instguides)
- [SAP 说明](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>先决条件
要使用本指南，需要具备以下 Azure 组件的基础知识：

- [Azure 虚拟机](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure 网络和虚拟网络](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure 存储](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

若要了解有关 Azure 上的 SAP NetWeaver 和其他 SAP 组件的详细信息，请参阅 [Azure 文档](https://docs.microsoft.com/azure/)的 [Azure 上的 SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) 部分。

## <a name="basic-setup-considerations"></a>基本设置注意事项
以下各节介绍了在 Azure VM 上部署 SAP HANA 系统的基本设置注意事项。

### <a name="connect-into-azure-virtual-machines"></a>连接到 Azure 虚拟机
如 [Azure 虚拟机规划指南](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)中所述，可通过两种基本方法连接到 Azure VM：

- 通过跳接 VM 或运行 SAP HANA 的 VM 上的 Internet 和公共终结点进行连接。
- 通过 [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) 或 Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/) 进行连接。

生产方案需要通过 VPN 或 ExpressRoute 建立的站点到站点连接。 对于送入使用 SAP 软件的生产方案的非生产方案，也需要此类型的连接。 下图显示了跨站点连接的示例：

![跨站点连接](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>选择 Azure VM 类型
[IAAS 的 SAP 文档](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html)中列出了可用于生产方案的 Azure VM 类型。 对于非生产方案，可使用更广泛的本机 Azure VM 类型。

>[!NOTE]
> 对于非生产方案，请使用 [SAP 说明 #1928533](https://launchpad.support.sap.com/#/notes/1928533) 中列出的 VM 类型。 若要将 Azure VM 用于生产场景，请在 SAP 发布的[已认证 IaaS 平台列表](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)中查看已通过 SAP HANA 认证的 VM。

通过使用以下方法在 Azure 中部署 VM：

- Azure 门户。
- Azure PowerShell cmdlet。
- Azure CLI。

还可通过 [SAP 云平台](https://cal.sap.com/)在 Azure VM 服务上部署整个已安装的 SAP HANA 平台。 [在 Azure 上部署 SAP S/4HANA 或 BW/4HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) 中介绍了安装过程，也可以使用[此处](https://github.com/AzureCAT-GSI/SAP-HANA-ARM)所述的自动化功能完成安装。

>[!IMPORTANT]
> 为了使用M208xx_v2 VM，您需要小心从 Azure VM 映像库中选择 Linux 映像。 为了阅读详细信息，请阅读文章["内存优化虚拟机大小](../../mv2-series.md)"。
> 


### <a name="storage-configuration-for-sap-hana"></a>SAP HANA 的存储配置
有关要在 Azure 中与 SAP HANA 一起使用的存储配置和存储类型，请阅读文档[SAP HANA Azure 虚拟机存储配置](./hana-vm-operations-storage.md)


### <a name="set-up-azure-virtual-networks"></a>设置 Azure 虚拟网络
当通过 VPN 或 ExpressRoute 与 Azure 建立站点到站点连接时，必须至少有一个 Azure 虚拟网络已通过虚拟网关连接到 VPN 或 ExpressRoute 线路。 在简单部署中，也可以将虚拟网关部署在托管 SAP HANA 实例的 Azure 虚拟网络 (VNet) 的子网中。 若要安装 SAP HANA，需要在 Azure 虚拟网络中另外创建两个子网。 一个子网托管 VM 以运行 SAP HANA 实例。 另一子网运行 Jumpbox 或管理 VM，以托管 SAP HANA Studio、其他管理软件或应用程序软件。

> [!IMPORTANT]
> 出于功能原因，但更重要的是出于性能原因，不支持在 SAP 应用程序与 SAP NetWeaver、Hybris 或基于 S/4HANA 的 SAP 系统的 DBMS 层之间的通信路径中配置 [Azure 网络虚拟设备](https://azure.microsoft.com/solutions/network-appliances/)。 SAP 应用程序层与 DBMS 层之间的通信必须为直接通信。 只要 [Azure ASG 和 NSG 规则](https://docs.microsoft.com/azure/virtual-network/security-overview)允许进行直接通信，限制就不包括这些规则。 更多不支持 NVA 的场景出现在代表 Linux Pacemaker 群集节点的 Azure VM 与 SBD 设备之间的通信路径中（如 [SUSE Linux Enterprise Server for SAP Applications 上的 Azure VM 上 SAP NetWeaver 的高可用性](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)所述）。 或者是在按[使用 Azure 中的文件共享在 Windows 故障转移群集上群集化 SAP ASCS/SCS 实例](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)所述设置的 Azure VM 与 Windows Server SOFS 之间的通信路径中。 通信路径中的 NVA 可能容易导致两个通信合作伙伴之间的网络延迟加倍，可能会限制 SAP 应用程序层与 DBMS 层之间的重要路径中的吞吐量。 在客户遇到的某些场景中，当 Linux Pacemaker 群集节点之间的通信需要通过 NVA 与 SBD 设备进行通信时，NVA 可能会导致 Pacemaker Linux 群集失败。  
> 

> [!IMPORTANT]
> 另一个不受支持的设计是将 SAP 应用程序层和 DBMS 层分到相互不[对等互连](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)的不同 Azure 虚拟网络****。 建议使用 Azure 虚拟网络中的子网（而不是使用其他 Azure 虚拟网络）将 SAP 应用程序层与 DBMS 层隔离开来。 如果决定不遵循建议，而是将两个层分到不同的虚拟网络，则这两个虚拟网络必须[对等互连](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)。 请注意，两个[对等互连](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)的 Azure 虚拟网络之间的网络流量视传输费用而定。 由于 SAP 应用程序层与 DBMS 层之间交换了数 TB 的数据，因此如果将 SAP 应用程序层和 DBMS 层分到两个对等互连的 Azure 虚拟网络，则可以累计大笔费用。 

安装 VM 以运行 SAP HANA 时，VM 需要：

- 已安装两个虚拟 NIC：其中一个 NIC 连接到管理子网，另一个从本地或其他网络连接到 Azure VM 中的 SAP HANA 实例。
- 为两个虚拟 NIC 部署静态专用 IP 地址。

> [!NOTE]
> 应该通过 Azure 方式将静态 IP 地址分配给单个 vNIC。 不应将来宾 OS 中的静态 IP 地址分配给 vNIC。 某些 Azure 服务（例如 Azure 备份服务）依赖于至少主 vNIC 设置为 DHCP 而不是静态 IP 地址这一事实。 另请参阅文档[排查 Azure 虚拟机备份问题](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking)。 如果需要将多个静态 IP 地址分配给某个 VM，需要将多个 vNIC 分配给该 VM。
>
>

但是，对于持久性部署，需要在 Azure 中创建虚拟数据中心网络体系结构。 此体系结构建议将连接到本地的 Azure VNet 网关分离到单独的 Azure VNet 中。 此单独的 VNet 应承载所有离开本地或互联网的流量。 使用此方法可以部署软件用于审核和记录进入此独立中心 VNet 中 Azure 虚拟数据中心的流量。 因此，有一个 VNet 托管传入和传出 Azure 部署的流量相关的所有软件和配置。

文章 [Azure 虚拟数据中心：网络透视](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter)和 [Azure 虚拟数据中心和企业控制平面](https://docs.microsoft.com/azure/architecture/vdc/)提供了有关虚拟数据中心方法和相关Azure VNet 设计的详细信息。


>[!NOTE]
>使用 [Azure VNet 对等互连](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)在中心 VNet 与分支 VNet 之间传送的流量会产生额外的[成本](https://azure.microsoft.com/pricing/details/virtual-network/)。 鉴于这些成本，可能需要考虑在运行严格的中心和分支网络设计，以及在运行多个连接到“分支”的 [Azure ExpressRoute 网关](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways)以绕过 VNet 对等互连之间做出取舍。 但是，Azure ExpressRoute 网关也会造成额外的[成本](https://azure.microsoft.com/pricing/details/vpn-gateway/)。 此外，用于网络流量日志记录、审核和监视的第三方软件也会产生额外的成本。 根据在一端通过 VNet 对等互连进行数据交换产生的成本，以及附加 Azure ExpressRoute 网关和附加软件许可证产生的成本，可以决定使用子网作为隔离单元而不使用 VNet，在一个 VNet 中进行微型分段。


有关分配 IP 地址的不同方法的概述，请参阅 [Azure 中的 IP 地址类型和分配方法](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)。 

对于运行 SAP HANA 的 VM，应该使用分配的静态 IP 地址。 原因是 HANA 的某些配置属性引用 IP 地址。

[Azure 网络安全组 (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) 用于定向路由到 SAP HANA 实例或 Jumpbox 的流量。 NSG 与最终的[应用程序安全组](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups)关联到 SAP HANA 子网和管理子网。

下图显示了遵循中心和分支 VNet 体系结构的 SAP HANA 的大致部署架构概况：

![SAP HANA 的大致部署架构](media/hana-vm-operations/hana-simple-networking-dmz.png)

若要在不建立站点到站点连接的情况下在 Azure 中部署 SAP HANA，仍需在公共 Internet 中屏蔽 SAP HANA 实例，并将其隐藏在转发代理的后面。 在此基本方案中，部署依赖于 Azure 内置的 DNS 服务来解析主机名。 对于使用面向公众的 IP 地址的更复杂部署，Azure 内置的 DNS 服务尤为重要。 使用 Azure NSG 和 [Azure NVA](https://azure.microsoft.com/solutions/network-appliances/) 来控制与监视从 Internet 到 Azure 中 Azure VNet 体系结构的路由。 下图显示了在不建立站点到站点连接的情况下用于在中心和分支 VNet 体系结构中部署 SAP HANA 的大致架构：
  
![未建立站点到站点连接的大致 SAP HANA 部署架构](media/hana-vm-operations/hana-simple-networking-dmz.png)
 

可以在[部署高可用性网络虚拟设备](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha)一文中找到有关如何在没有中心和分支 VNet 体系结构的情况下，使用 Azure NVA 控制和监视来自 Internet 的访问的说明。


## <a name="configuring-azure-infrastructure-for-sap-hana-scale-out"></a>为 SAP HANA 横向扩展配置 Azure 基础结构
为了找出经过 OLAP 横向扩展或 S/4HANA 横向扩展认证的 Azure VM 类型，请查看[SAP HANA 硬件目录](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)。 列"聚类"中的复选标记表示横向扩展支持。 应用程序类型指示是否支持 OLAP 横向扩展或 S/4HANA 横向扩展。 有关每个 VM 的横向扩展认证的节点的详细信息，请查看 SAP HANA 硬件目录中列出的特定 VM SKU 中条目的详细信息。

用于在 Azure VM 中部署横向扩展配置的最小操作系统版本，请检查 SAP HANA 硬件目录中列出的特定 VM SKU 中条目的详细信息。 在 n 节点 OLAP 横向扩展配置中，一个节点充当主节点。 其他到认证限制的节点充当辅助节点。 其他备用节点不计入经认证的节点数

>[!NOTE]
> 只能通过[Azure NetApp 文件](https://azure.microsoft.com/services/netapp/)存储进行具有备用节点的 SAP HANA 的 Azure VM 横向扩展部署。 没有其他 SAP HANA 认证的 Azure 存储允许配置 SAP HANA 备用节点
>

对于 /hana/共享，我们还建议使用[Azure NetApp 文件](https://azure.microsoft.com/services/netapp/)。 

横向扩展配置中单个节点的典型基本设计将如下所示：

![单个节点的横向扩展基本设计](media/hana-vm-operations/scale-out-basics-anf-shared.PNG)

SAP HANA 横向扩展的 VM 节点基本配置如下所示：

- 对于 **/hana/shared**，您可以使用通过 Azure NetApp 文件提供的本机 NFS 服务。 
- 所有其他磁盘卷不会在不同节点之间共享，也不基于 NFS。 本文档稍后将提供具有非共享 **/hana/数据和****/hana/log**的横向扩展 HANA 安装的安装配置和步骤。 对于可以使用的 HANA 认证存储，请查看文章[SAP HANA Azure 虚拟机存储配置](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)。


调整卷或磁盘的大小，您需要检查文档[SAP HANA TDI 存储要求](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html)，具体取决于辅助节点的数量所需的大小。 文档发布您需要应用的公式，以获得卷所需的容量

横向扩展 SAP HANA VM 的单节点配置图形中显示的其他设计条件是 VNet，或者更好的子网配置。 SAP 强烈建议将面向客户端/应用程序的流量与 HANA 节点之间的通信进行隔离。 如图所示，可以通过将两个不同的 vNIC 附加到 VM 来实现此目的。 这两个 vNIC 位于不同的子网中，并使用两个不同的 IP 地址。 然后，使用 NSG 或用户定义的路由通过路由规则控制流量流。

具体而言，在 Azure 中，没有任何方法可以在特定的 vNIC 上强制实施服务质量和配额。 因此，通过面向客户端/应用程序的流量与节点内部通信的隔离，没有任何机会使一个流量流优先于另一个流量流。 隔离保持为一种安全度量，用于屏蔽横向扩展配置的节点内部通信。  

>[!NOTE]
>SAP 建议将网络流量分离到客户端/应用程序端和节点内流量，如本文档所述。 因此，建议将体系结构放在原图形中所示的位置。 有关偏离建议的要求，请咨询您的安全和合规团队 
>

从网络角度看，最低要求的网络体系结构如下所示：

![单个节点的横向扩展基本设计](media/hana-vm-operations/overview-scale-out-networking.png)



### <a name="installing-sap-hana-scale-out-n-azure"></a>在 Azure 中安装 SAP HANA 横向扩展
安装横向扩展 SAP 配置时，需要执行以下大致步骤：

- 部署新的或调整现有的 Azure VNet 基础结构
- 使用基于 ANF 的 Azure 托管高级存储、超磁盘卷和/或 NFS 卷部署新 VM
- - 改编网络路由，确保不会通过 [NVA](https://azure.microsoft.com/solutions/network-appliances/) 路由 VM 之间的节点内部通信。 
- 安装 SAP HANA 主节点。
- 编辑 SAP HANA 主节点的配置参数
- 继续安装 SAP HANA 工作节点

#### <a name="installation-of-sap-hana-in-scale-out-configuration"></a>安装采用横向扩展配置的 SAP HANA
部署 Azure VM 基础结构并完成其他所有准备工作后，需要通过以下步骤安装 SAP HANA 横向扩展配置：

- 根据 SAP 文档安装 SAP HANA 主节点
- 如果将 Azure 高级存储或超磁盘存储与 /hana/数据和 /hana/log 的非共享磁盘一起使用，则需要更改 global.ini 文件，并将参数"basepath_shared = 否"添加到 global.ini 文件中。 此参数使 SAP HANA 能够在横向扩展中运行，而无需节点之间的"共享 **"/hana/数据和** **/hana/log**卷。 [SAP 说明 #2080991](https://launchpad.support.sap.com/#/notes/2080991) 中提供了详细信息。 如果您使用基于 ANF 的 NFS 卷/哈纳/数据和 /哈纳/日志，则无需进行此更改
- 全局.ini 参数的最终更改后，重新启动 SAP HANA 实例
- 添加其他工作节点。 另请参阅 <https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html>。 在安装期间或之后，使用 local hdblcm 等命令为 SAP HANA 节点内部通信指定内部网络。 有关更详细的文档，请参阅 [SAP 说明 #2183363](https://launchpad.support.sap.com/#/notes/2183363)。 

在[部署在 Azure VM 上具有备用节点的 SAP HANA 横向系统的详细信息，在 SUSE Linux 企业服务器上使用 Azure NetApp 文件](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)。 在"[使用红帽企业 Linux 上的 Azure NetApp 文件"上部署具有 Azure VM 备用节点的 SAP HANA 横向扩展系统](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)，可以找到红帽的等效文档。 


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>适用于 Azure 虚拟机的 SAP HANA Dynamic Tiering 2.0

除 Azure M 系列 VM 上的 SAP HANA 认证之外，Microsoft Azure 还支持 SAP HANA Dynamic Tiering 2.0（请参阅 SAP HANA Dynamic Tiering 文档链接进行详细了解）。 虽然安装该产品或运行该产品没有任何区别（例如，通过 Azure 虚拟机中的 SAP HANA），但要获得 Azure 上的官方支持，有几点是必不可少的。 这些要点如下所述。 在整个文章中，将使用缩写"DT 2.0"而不是全名动态分层 2.0。

SAP BW 或 S4HANA 不支持 SAP HANA Dynamic Tiering 2.0。 现在的主要用例是本机 HANA 应用程序。


### <a name="overview"></a>概述

下图概述了有关 Microsoft Azure 支持的 DT 2.0。 还有一套强制性要求，必须遵循符合官方认证：

- DT 2.0 必须安装在专用 Azure 虚拟机上。 它可能无法在运行 SAP HANA 的同一 VM 上运行
- SAP HANA 和 DT 2.0 VM 必须部署在同一 Azure Vnet 中
- 在启用 Azure 加速网络的情况下，才能部署 SAP HANA 和 DT 2.0 VM
- DT 2.0 虚拟机的存储类型必须是 Azure 高级存储
- 多个 Azure 磁盘必须附加到 DT 2.0 VM
- 必须通过跨 Azure 磁盘的条带化来创建软件 RAID /条带化卷（通过 lvm 或 mdadm）

更多细节将在以下各节中解释。

![SAP HANA DT 2.0 体系结构概述](media/hana-vm-operations/hana-data-tiering.png)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>适用于 SAP HANA DT 2.0 的专用 Azure VM

在 Azure IaaS 上，DT 2.0 仅在专用 VM 上受支持。 不允许在运行 HANA 实例的同一 Azure VM 上运行 DT 2.0。 最初可以使用两种 VM 类型来运行 SAP HANA DT 2.0：

- M64-32ms 
- E32sv3 

请在[此处](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)查看 VM 类型说明

鉴于 DT 2.0 的基本理念，卸载“warm”数据以节省成本，最好使用相应的 VM 大小。 但对于可能的组合，并没有严格的规定。 这取决于特定的客户工作负载。

建议的配置是：

| SAP HANA VM 类型 | DT 2.0 VM 类型 |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


SAP HANA 认证的 M 系列 VM 与受支持的 DT 2.0 VM（M64-32ms 和 E32sv3）的所有组合形式都是可行的。


### <a name="azure-networking-and-sap-hana-dt-20"></a>Azure 网络和 SAP HANA DT 2.0

如果要在专用 VM 上安装 DT 2.0，DT 2.0 VM 和 SAP HANA VM 之间的网络吞吐量必须至少为 10 Gb。 因此，必须将所有 VM 放在相同的 Azure Vnet 中，并启用 Azure 加速网络。

有关 Azure 加速网络的更多信息，请参阅[此处](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)

### <a name="vm-storage-for-sap-hana-dt-20"></a>适用于 SAP HANA DT 2.0 的 VM 存储

根据 DT 2.0 最佳做法指南，每个物理核心的磁盘 IO 吞吐量最低应为 50 MB/秒。 查看两种 Azure VM 类型的规范，DT 2.0 支持该类型，VM 的最大磁盘 IO 吞吐量限制如下所示：

- E32sv3：768 MB/秒（非缓存）表示每个物理核心的比率为 48 MB /秒
- M64-32ms：1000 MB/秒（非缓存）表示每个物理核心的比率为 62.5 MB /秒

必须将多个 Azure 磁盘连接到 DT 2.0 VM 并在 OS 级别创建软件 RAID（带区），以实现每个 VM 的磁盘吞吐量的最大限制。 在这方面，单个 Azure 磁盘无法提供该吞吐量以达到最大 VM 限制。 Azure 高级存储是运行 DT 2.0 的必需条件。 

- 有关可用的 Azure 磁盘类型的详细信息，请参阅[此处](../../windows/disks-types.md)
- 有关通过 mdadm 创建软件 RAID 的详细信息，请参阅[此处](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid)
- 有关配置 LVM 以创建最大吞吐量的带区卷的详细信息，请参阅[此处](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm)

根据大小要求，可使用不同的选项来达到 VM 的最大吞吐量。 以下是针对每个 DT 2.0 VM 类型以实现 VM 的吞吐量上限可能的数据卷磁盘配置。 E32sv3 VM 应视为较小工作负载的入门级别。 如果它的速度不够快，可能需要将 VM 的大小调整为 M64-32ms。
由于 M64-32ms VM 具有大量内存，因此 IO 负载可能无法达到限制，尤其是对于读取密集型工作负载而言。 因此，根据客户特定的工作负荷，在条带集中使用更少的磁盘可能就足够了。 但为了安全起见，请选择下面的磁盘配置以保证最大吞吐量：


| VM SKU | 磁盘配置 1 | 磁盘配置 2 | 磁盘配置 3 | 磁盘配置 4 | 磁盘配置 5 | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 -> 16 TB | 4 x P40 -> 8 TB | 5 x P30 -> 5 TB | 7 x P20 -> 3.5 TB | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB | 4 x P30 -> 4 TB | 5 x P20 -> 2.5 TB | 6 x P15 -> 1.5 TB | 


特别是在工作负载读取密集的情况下，它可以提高 IO 性能，以打开 Azure 主机缓存“只读”，如数据库软件数据卷所建议的那样。 而对于事务日志，Azure 主机磁盘缓存必须是“无”。 

关于日志卷的大小，推荐的起始点是数据大小的 15％。 可使用不同的 Azure 磁盘类型来完成日志卷的创建，具体取决于成本和吞吐量要求。 对于日志卷，需要高 I/O 吞吐量。  在使用 VM 类型 M64-32ms 时，必须启用[写入加速器](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator)。 Azure 写入加速器为事务日志提供最佳磁盘写入延迟（仅适用于 M 系列）。 有一些事项需要注意，比如每个 VM 类型的最大磁盘数。 在[此处](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)可以找到有关写入加速器的详细信息


下面是一些有关调整日志卷大小的示例：

| 数据卷的大小和磁盘类型 | 日志卷和磁盘类型配置 1 | 日志卷和磁盘类型配置 2 |
| --- | --- | --- |
| 4 x P50 -> 16 TB | 5 x P20 -> 2.5 TB | 3 x P30 -> 3 TB |
| 6 x P15 -> 1.5 TB | 4 x P6 -> 256 GB | 1 x P15 -> 256 GB |


与 SAP HANA 横向扩展类似，必须在 SAP HANA VM 和 DT 2.0 VM 之间共享 /hana/shared 目录。 建议使用与专用 VM 的 SAP HANA 横向扩展相同的体系结构，这些 VM 可充当高可用性 NFS 服务器。 为了提供共享的备份卷，可以使用完全相同的设计。 但是，是否必须使用 HA，或者只需使用具有足够存储容量的专用 VM 作为备份服务器就足够了，这些都可由客户决定。



### <a name="links-to-dt-20-documentation"></a>DT 2.0 文档的链接 

- [SAP HANA Dynamic Tiering 安装和更新指南](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US)
- [SAP HANA Dynamic Tiering 教程和资源](https://help.sap.com/viewer/fb9c3779f9d1412b8de6dd0788fa167b/2.0.03/en-US)
- [SAP HANA Dynamic Tiering PoC](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/)
- [SAP HANA 2.0 SPS 02 Dynamic Tiering 增强功能](https://blogs.sap.com/2017/07/31/sap-hana-2.0-sps-02-dynamic-tiering-enhancements/)




## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>在 Azure VM 上部署 SAP HANA 的操作
以下各节介绍了在 Azure VM 上部署 SAP HANA 系统的一些相关操作。

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Azure VM 上的备份和还原操作
以下文档介绍如何备份和还原 SAP HANA 部署：

- [SAP HANA 备份概述](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [SAP HANA 文件级备份](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [SAP HANA 存储快照基准](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>启动并重启包含 SAP HANA 的 VM
Azure 公有云的一个突出特性是只需为计算时间付费。 例如，当关闭正在运行 SAP HANA 的 VM 时，只需为此期间的存储成本付费。 在初始部署中为 VM 指定静态 IP 地址时，可利用另一特性。 重启具有 SAP HANA 的 VM 时，VM 使用其先前的 IP 地址重启。 


### <a name="use-saprouter-for-sap-remote-support"></a>使用 SAProuter 实现 SAP 远程支持
如果已在本地位置与 Azure 之间建立了站点到站点的连接，并且正在运行 SAP 组件，那么可能已在运行 SAProuter。 在此情况下，请通过完成以下操作实现远程支持：

- 在 SAProuter 配置中保留托管 SAP HANA 的 VM 的专用和静态 IP 地址。
- 配置托管 HANA VM 的子网的 NSG，允许流量通过 TCP/IP 端口 3299。

如果通过 Internet 连接到 Azure，且包含 SAP HANA 的 VM 没有 SAP 路由器，则需安装该组件。 在管理子网中某个独立的 VM 上安装 SAProuter。 下图显示了在不建立站点到站点连接且不包含 SAProuter 的情况下部署 SAP HANA 的大致架构：

![未建立站点到站点连接且不包含 SAProuter 的大致 SAP HANA 部署架构](media/hana-vm-operations/hana-simple-networking-saprouter.png)

确保在独立的 VM 中安装 SAProuter，而不是在 Jumpbox VM 中安装。 该独立 VM 必须具有静态 IP 地址。 要将 SAProuter 连接到 SAP 托管的 SAProuter，请联系 SAP 获取 IP 地址。 （SAP 托管的 SAP 路由器是您在 VM 上安装的 SAProuter 实例的对应项。使用 SAP 中的 IP 地址配置 SAP 路由器实例。 在配置设置中，唯一必需的端口是 TCP 端口 3299。

有关如何通过 SAPRouter 设置和维护远程支持连接的详细信息，请参阅 [SAP 文档](https://support.sap.com/en/tools/connectivity-tools/remote-support.html)。

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Azure 本机 VM 上的 SAP HANA 的高可用性
如果您正在运行 SUSE Linux 企业服务器或红帽，则可以使用 STONITH 设备建立起搏器群集。 可以使用这些设备设置一个可将同步复制与 HANA 系统复制和自动故障转移配合使用的 SAP HANA 配置。 有关详细信息，在"后续步骤"部分中列出。

## <a name="next-steps"></a>后续步骤
熟悉列出的文章
- [SAP HANA Azure 虚拟机存储配置](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)
- [在 SUSE Linux 企业服务器上使用 Azure NetApp 文件，在 Azure VM 上部署具有备用节点的 SAP HANA 横向扩展系统](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)
- [使用红帽企业 Linux 上的 Azure NetApp 文件，在 Azure VM 上部署具有备用节点的 SAP HANA 横向扩展系统](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)
- [SUSE Linux Enterprise Server 上 Azure VM 中 SAP HANA 的高可用性](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability)
- [Red Hat Enterprise Linux 上 Azure VM 中 SAP HANA 的高可用性](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability-rhel)

 

