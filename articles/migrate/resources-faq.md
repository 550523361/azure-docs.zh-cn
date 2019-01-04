---
title: Azure Migrate - 常见问题解答 (FAQ) | Microsoft Docs
description: 解答有关 Azure Migrate 的常见问题
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: snehaa
ms.openlocfilehash: ebc4393341341b3b73165a166a650ae1a6f431ff
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "53257788"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate - 常见问题解答 (FAQ)

本文包含有关 Azure Migrate 的常见问题。 如果在阅读本文之后还有其他问题，请在 [Azure Migrate 论坛](https://aka.ms/AzureMigrateForum)中提问。

## <a name="general"></a>常规

### <a name="does-azure-migrate-support-assessment-of-only-vmware-workloads"></a>Azure Migrate 是否仅支持对 VMware 工作负载的评估？

是，Azure Migrate 目前仅支持对 VMware 工作负载的评估。 以后将实现对 Hyper-V 和物理服务器的支持。

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Azure Migrate 是否需要使用 vCenter Server 来发现 VMware 环境？

是的，Azure Migrate 需要使用 vCenter Server 来发现 VMware 环境。 它不支持发现不受 vCenter Server 管理的 ESXi 主机。

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Azure Migrate 与 Azure Site Recovery 有何不同？

Azure Migrate 是评估服务，可帮助发现本地工作负荷以及规划到 Azure 的迁移。 [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) 属于灾难恢复解决方案，可帮助将本地工作负荷迁移到 Azure 中的 IaaS VM。

### <a name="whats-the-difference-between-using-azure-migrate-for-assessments-and-the-map-toolkit"></a>使用 Azure Migrate 和 Map Toolkit 进行评估有什么区别？

[Azure Migrate](migrate-overview.md) 提供迁移评估，专门用于协助本地工作负载到 Azue 的迁移就绪和评估。 [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=7826) 具有其他功能。 例如，较新版本的 Windows 客户端和服务器操作系统、软件使用情况跟踪等的迁移规划。对于这些情况，继续使用 MAP Toolkit。


### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Azure Migrate 与 Azure Site Recovery 部署规划器有何不同？

Azure Migrate 是一个迁移规划工具，而 Azure Site Recovery 部署规划器是一个灾难恢复 (DR) 规划工具。

**从 VMware 迁移到 Azure**：如果想要将本地工作负荷迁移到 Azure，请使用 Azure Migrate 进行迁移规划。 Azure Migrate 会评估本地工作负荷并提供指导、见解和机制，以帮助迁移到 Azure。 准备好迁移计划后，可以使用 Azure Site Recovery 和 Azure 数据库迁移服务之类的服务将计算机迁移到 Azure。

**从 Hyper-V 迁移到 Azure**：Azure Migrate 目前仅支持评估要迁移到 Azure 的 VMware 虚拟机。 Azure Migrate 对 Hyper-V 的支持已在规划中。 在此期间，可以使用 Site Recovery 部署规划器。 在 Azure Migrate 实现 Hyper-V 支持后，可以使用 Azure Migrate 来规划 Hyper-V 工作负荷的迁移。

**从 VMware/Hyper-V 到 Azure 的灾难恢复**：如果想要使用 Azure Site Recovery (Site Recovery) 在 Azure 中实现灾难恢复 (DR)，请使用 Site Recovery 部署规划器进行 DR 规划。 Site Recovery 部署规划器会针对本地环境执行深度的、特定于 ASR 的评估。 它会提供所需的建议，让 Site Recovery 成功执行复制、虚拟机故障转移等 DR 操作。  

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>Azure Migrate 支持哪些 Azure 地域？

Azure Migrate 当前支持将美国和 Azure 政府作为项目地域。 即使只能在这些地域创建迁移项目，也仍可以针对[多个目标位置](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties)评估计算机。 项目地域仅用于存储已发现的元数据。

**地域** | **元数据存储位置**
--- | ---
美国 | 美国中西部或美国东部
Azure Government  | 美国政府弗吉尼亚州

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>本地站点如何连接到 Azure Migrate？

可以通过 Internet 或使用 ExpressRoute 与公共对等互连。

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>我可以使用 OVA 模板强化 VM 设置吗？

只要 Azure Migrate 设备工作所需的通信和防火墙规则保持不变，就可以将其他组件（例如防病毒）添加到 OVA 模板中。   

## <a name="discovery"></a>发现

### <a name="what-data-is-collected-by-azure-migrate"></a>Azure Migrate 收集哪些数据？

Azure Migrate 支持两种发现：基于设备的发现和基于代理的发现。
基于设备的发现收集有关本地 VM 的元数据。下面列出了设备收集的元数据的完整列表：

**VM 的配置数据**
- VM 显示名称（在 vCenter 上）
- VM 库存路径（vCenter 中的主机/群集/文件夹）
- IP 地址
- MAC 地址
- 操作系统
- 核心数、磁盘数、NIC 数
- 内存大小、磁盘大小

**VM 的性能数据**
- CPU 使用率
- 内存使用率
- 对于附加到 VM 的每个磁盘：
  - 磁盘读取吞吐量
  - 磁盘写入吞吐量
  - 每秒的磁盘读取操作次数
  - 每秒的磁盘写入操作次数
- 对于附加到 VM 的每个网络适配器：
  - 进网络
  - 出网络

基于代理的发现是可以在基于设备的发现的基础上使用的选项，它会帮助客户将本地 VM 的[依赖项可视化](how-to-create-group-machine-dependencies.md)。 依赖项代理收集 FQDN、OS、IP 地址、MAC 地址、VM 中运行的进程，以及 VM 的传入/传出 TCP 连接等详细信息。 基于代理的发现是可选的，如果不想要将 VM 依赖项可视化，则可以选择不安装代理。

### <a name="would-there-be-any-performance-impact-on-the-analyzed-esxi-host-environment"></a>是否会对已分析的 ESXi 主机环境造成任何性能影响？

在[一次性发现方法](https://docs.microsoft.com/azure/migrate/concepts-collector#discovery-methods)中，为了收集性能数据，vCenter Server 上的统计信息级别将设置为 3。 设置为此级别将收集大量的故障排除数据，这些数据将存储在 vCenter Server 数据库中。 因此，它可能会在 vCenter Server 上导致一些性能问题。 对 ESXi 主机的影响微不足道。

我们已引入了性能数据持续分析（目前为预览版）。 使用持续分析，不再需要更改 vCenter Server 统计信息级别来运行基于性能的评估。 收集器设备现在将对本地计算机进行分析来度量虚拟机的性能数据。 这几乎不会对 ESXi 主机和 vCenter Server 造成性能影响。

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>收集的数据存储在何处，存储多久？

收集器设备收集的数据存储在创建迁移项目时指定的 Azure 位置。 数据安全存储在 Microsoft 订阅中，当用户删除 Azure Migrate 项目时，会删除这些数据。

对于依赖项可视化，如果在 VM 上安装了代理，则依赖项代理收集的数据将存储在美国的某个 Log Analytics 工作区中，该工作区是在用户的订阅中创建的。 在订阅中删除 Log Analytics 工作区时会删除此数据。 [了解详细信息](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization)。

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>数据是否经过静态加密和传输中加密？

是的，收集的数据同时经过静态加密和传输中加密。 设备收集的元数据将使用 https 通过 Internet 安全发送到 Azure Migrate 服务。 收集的元数据存储在 [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) 中以及 Microsoft 订阅中的 [Azure Blob 存储](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)内，并经过静态加密。

依赖项代理收集的数据也会经过传输中加密（安全的 https 通道），并存储在用户订阅中的 Log Analytics 工作区内。 这些数据也会经过静态加密。

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>收集器如何与 vCenter Server 和 Azure Migrate 服务通信？

收集器设备使用用户在设备中提供的凭据连接到 vCenter Server（通过端口 443）。 收集器使用 VMware PowerCLI 查询 vCenter Server，以收集有关 vCenter Server 管理的 VM 的元数据。 它会从 vCenter Server 收集有关 VM 的配置数据（核心、内存、磁盘、NIC 等），以及每个 VM 在过去一个月的性能历史记录。 然后，将收集的元数据发送到 Azure Migrate 服务（使用 https 通过 Internet）进行评估。 [了解详细信息](concepts-collector.md)

### <a name="can-i-connect-the-same-collector-appliance-to-multiple-vcenter-servers"></a>可以将相同的收集器设备连接到多个 vCenter 服务器吗？

可以，可以使用单个收集器设备来发现多个 vCenter 服务器，但不能同时使用多个收集器。 需要依次运行发现。

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>Site Recovery 使用的 OVA 模板是否与 Azure Migrate 使用的 OVA 集成？

当前没有集成。 Site Recovery 中的 .OVA 模板用于为 VMware VM/物理服务器复制设置 Site Recovery 配置服务器。 Azure Migrate 使用的 .OVA 用于发现由 vCenter 服务器托管的 VMware VM，以便进行迁移评估。

### <a name="i-changed-my-machine-size-can-i-rerun-the-assessment"></a>我更改了计算机的大小。 是否可以重新运行评估？

如果在要评估的 VM 上更改设置，则触发器会使用收集器设备再次发现。 在设备中，使用“再次启动收集”选项来执行此操作。 收集完成以后，请在门户中选择用于评估的“重新计算”选项，以便获取更新的评估结果。

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>如何在 Azure Migrate 中发现多租户环境？

如果你有在租户之间共享的环境并且不希望在一个租户的订阅中发现另一个租户的 VM，可以使用收集器设备中的“范围”字段来设置发现范围。 如果各个租户共享主机，请创建一个仅对属于特定租户的 VM 具有只读访问权限的凭据，然后在收集器设备中使用此凭据并将范围指定为主机来执行发现。 另外，也可以在 vCenter Server 中创建文件夹（例如，用于 tenant1 的 folder1 和用于 tenant2 的 folder2），在共享的主机下，将 tenant1 的 VM 移到 folder1 中，将 tenant2 的 VM 移动到 folder2 中，然后通过指定合适的文件夹相应地在收集器中设置发现范围。

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>在单个迁移项目中可以发现多少个虚拟机？

在单个迁移项目中可以发现 1500 个虚拟机。 如果本地环境包含更多的计算机，请[详细了解](how-to-scale-assessment.md)如何在 Azure Migrate 中发现大型环境。

## <a name="assessment"></a>评估

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>Azure Migrate 是否支持基于企业协议 (EA) 的成本估计？

Azure Migrate 目前不支持[企业协议套餐](https://azure.microsoft.com/offers/enterprise-agreement-support/)的成本估计。 解决方法是指定即用即付作为套餐，并在评估属性的“折扣”字段中手动指定折扣百分比（适用于订阅）。

  ![折扣](./media/resources-faq/discount.png)


## <a name="dependency-visualization"></a>依赖项可视化

> [!NOTE]
> 依赖项可视化功能在 Azure 政府中不可用。

### <a name="what-is-dependency-visualization"></a>什么是依赖项可视化？

使用依赖项可视化，你可以在运行评估之前通过交叉检查计算机依赖项来更自信地评估要迁移的 VM 组。 依赖项可视化可帮助你确保不遗漏任何事项，以避免在迁移到 Azure 时发生意外的服务中断。 Azure Migrate 利用 Log Analytics 中的服务映射解决方案来实现依赖项可视化。

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>是否需要支付依赖项可视化功能的使用费？

不是。 [在此处](https://azure.microsoft.com/pricing/details/azure-migrate/)详细了解 Azure Migrate 定价。

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>要实现依赖项可视化，是否需要安装任何软件？

若要使用依赖项可视化，你需要在要评估的每台本地计算机上下载并安装代理。

- 需要在每台计算机上安装 [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)。
- 需要在每台计算机上安装[依赖项代理](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure)。
- 此外，如果计算机未连接到 Internet，则需要在计算机上下载并安装 Log Analytics 网关。

除非要使用依赖项可视化，否则在要评估的计算机上不需要安装这些代理。

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>是否可将现有的工作区用于依赖项可视化？

可以，Azure Migrate 现在允许将现有工作区附加到迁移项目并利用它进行依赖项可视化。 [了解详细信息](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization#how-does-it-work)。

### <a name="can-i-export-the-dependency-visualization-report"></a>是否可以导出依赖项可视化报表？

否，依赖项可视化报表不能导出。 但是，由于 Azure Migrate 使用服务映射进行依赖项可视化，因此可以使用[服务映射 REST API](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) 获取 JSON 格式的依赖项。

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>如何才能自动安装 Microsoft Monitoring Agent (MMA) 和依赖项代理？

[此处](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples)有一个脚本，可以使用它来安装依赖项代理。 对于 MMA，TechNet 上的[此处](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab)有一个可以使用的脚本。

除了脚本，还可以利用 System Center Configuration Manager (SCCM)、[Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) 之类的部署工具来部署代理。

### <a name="what-are-the-operating-systems-supported-by-mma"></a>MMA 支持的操作系统有哪些？

[此处](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)列出了 MMA 支持的 Windows 操作系统。
[此处](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems)列出了 MMA 支持的 Linux 操作系统。

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>依赖项代理支持的操作系统有哪些？

[此处](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems)列出了依赖项代理支持的 Windows 操作系统。
[此处](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems)列出了依赖项代理支持的 Linux 操作系统。

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>我在 Azure Migrate 中可视化依赖项时是否可以超过一小时的持续时间？
否，Azure Migrate 允许依赖项可视化持续最多一小时的时间。 尽管 Azure Migrate 允许返回到历史记录中的某一特定日期可以推至上个月，但可视化依赖项的最长持续时间最多为一小时。 例如，你可以使用依赖项映射中的持续时间功能来查看昨天的依赖项，但只能查看一小时。

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-10-vms"></a>包含 10 个以上 VM 的组是否支持依赖项可视化？
你可以[可视化依赖项的组最多只能有 10 个 VM](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies)，如果某个组的 VM 超过 10 个，建议先将该组拆分成较小的组，然后再可视化依赖项。


## <a name="next-steps"></a>后续步骤

- 阅读 [Azure Migrate 概述](migrate-overview.md)
- 了解如何[发现和评估](tutorial-assessment-vmware.md) VMware 环境
