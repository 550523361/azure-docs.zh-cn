---
title: Azure 备份常见问题解答
description: '针对以下常见问题的解答：包括恢复服务保管库在内的 Azure 备份功能、能够备份的内容、原理、加密和限制。 '
services: backup
author: markgalioto
manager: carmonm
keywords: 备份和灾难恢复;备份服务
ms.service: backup
ms.topic: conceptual
ms.date: 8/2/2018
ms.author: markgal
ms.openlocfilehash: 2151733a5d91fb17c69fa1f4f6aac64a70928824
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364296"
---
# <a name="questions-about-the-azure-backup-service"></a>有关 Azure 备份服务的问题
本文回答有关 Azure 备份组件的常见问题。 某些答案提供内含全面信息的文章的链接。 单击“评论”（右侧）即可提问有关 Azure 备份的问题。 评论显示在本文末尾。 也可以在 [论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup)中发布有关 Azure 备份服务的问题。

若要快速浏览本文的各个部分，请使用右侧“本文内容”下的链接。


## <a name="recovery-services-vault"></a>恢复服务保管库

### <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>在每个 Azure 订阅中可以创建的保管库数量是否有任何限制？ <br/>
是的。 在 Azure 备份支持的每个区域中，可以为每个订阅创建多达 500 个恢复服务保管库。 如果需要更多保管库，请创建另一订阅。

### <a name="are-there-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>可针对每个保管库注册的服务器/计算机数量是否有限制？ <br/>
每个保管库最多可以注册 1000 个 Azure 虚拟机。 如果使用 MAB 代理，每个保管库最多可以注册 50 个 MAB 代理。 可以将 50 个 MAB 服务器/DPM 服务器注册到一个保管库。

### <a name="can-i-use-a-rest-api-to-query-the-size-of-protected-items-in-a-vault-br"></a>可以使用 REST API 查询保管库中受保护项的大小吗？ <br/>
可以，[使用情况 - 按保管库列出](https://docs.microsoft.com/rest/api/recoveryservices/usages/usages_listbyvaults)一文中列出了可从恢复服务保管库获取的信息。

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>如果本组织有一个保管库，如何在还原数据时会一个服务器的数据与另一个服务器隔离？<br/>
注册到同一个保管库的所有服务器都能够恢复由 *使用同一密码*的其他服务器备份的数据。 如果想要隔离服务器中的备份数据与组织中的其他服务器，请使用这些服务器的指定通行短语。 例如，人力资源服务器可能使用一个加密通行短语，会计结算服务器使用另一个通行短语，而存储服务器使用第三个通行短语。

### <a name="can-i-migrate-my-vault-between-subscriptions-br"></a>是否可以在订阅之间迁移我的保管库？ <br/>
不是。 保管库是在订阅级别创建的，无法重新分配到另一订阅。

### <a name="can-i-migrate-backup-data-to-another-vault-br"></a>是否可以将备份数据迁移到另一个保管库？ <br/>
不是。 保管库中存储的备份数据无法移动到不同的保管库。

### <a name="can-i-change-from-grs-to-lrs-after-a-backup-br"></a>能否在备份后从 GRS 更改为 LRS？ <br/>
不是。 仅在存储任何备份之后，恢复服务保管库才可更改存储选项。

### <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-still-supported-br"></a>恢复服务保管库基于 Resource Manager。 是否仍支持备份保管库？ <br/>
备份保管库已转换为恢复服务保管库。 如果你未将备份保管库转换为恢复服务保管库，则系统已为你将备份保管库转换为恢复服务保管库。

### <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>是否可以将备份保管库迁移到恢复服务保管库？ <br/>
所有备份保管库已转换为恢复服务保管库。 如果你未将备份保管库转换为恢复服务保管库，则系统已为你将备份保管库转换为恢复服务保管库。

## <a name="azure-backup-agent"></a>Azure 备份代理
[Azure 文件-文件夹备份常见问题解答](backup-azure-file-folder-backup-faq.md)中提供了问题的详细列表

## <a name="azure-vm-backup"></a>Azure VM 备份
[Azure VM 备份常见问题解答](backup-azure-vm-backup-faq.md)中提供了问题的详细列表

## <a name="back-up-vmware-servers"></a>备份 VMware 服务器

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>是否可以将 VMware vCenter 服务器备份到 Azure？
是的。 可以使用 Azure 备份服务器将 VMware vCenter 和 ESXi 备份到 Azure。 若要了解支持的 VMware 版本，请参阅 [Azure 备份服务器保护矩阵](backup-mabs-protection-matrix.md)一文。 有关分步说明，请参阅[使用 Azure 备份服务器备份 VMware 服务器](backup-azure-backup-server-vmware.md)。

### <a name="do-i-need-a-separate-license-to-recover-a-full-on-premises-vmwarehyper-v-cluster-from-dpm-or-azure-backup-serverbr"></a>是否需要单独的许可证才能从 DPM 或 Azure 备份服务器恢复完整的本地 VMware/Hyper-V 群集？<br/>
不需要单独的 VMware/Hyper-V 保护许可。 如果是 System Center 客户，请使用 DPM 保护 VMware VM。 如果不是 System Center 客户，可以使用 Azure 备份服务器（即用即付）来保护 VMware VM。

## <a name="azure-backup-server-and-system-center-data-protection-manager"></a>Azure 备份服务器和 System Center Data Protection Manager
### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>是否可以使用 Azure 备份服务器为物理服务器创建裸机恢复 (BMR) 备份？ <br/>
是的。

### <a name="can-i-register-my-dpm-server-to-multiple-vaults-br"></a>是否可以向多个保管库注册 DPM 服务器？ <br/>
不是。 一个 DPM 或 MABS 服务器只能注册到一个保管库。

### <a name="which-version-of-system-center-data-protection-manager-is-supported"></a>支持哪个版本的 System Center Data Protection Manager？
建议在适用于 System Center Data Protection Manager (DPM) 的最新更新汇总版本 (UR) 上安装[最新](http://aka.ms/azurebackup_agent)的 Azure 备份代理。
- 对于 System Center DPM 2012 R2，[更新汇总 14](https://support.microsoft.com/help/4043315/update-rollup-14-for-system-center-2012-r2-data-protection-manager) 是最新的更新。
- 对于 System Center DPM 2016，[更新汇总 2](https://support.microsoft.com/en-us/help/3209593) 是最新的更新。

### <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-install-system-center-dpm-to-protect-on-premises-applicationvm-workloads-to-azure"></a>我已安装 Azure 备份代理来保护我的文件和文件夹。 可以安装 System Center DPM 以在 Azure 中保护本地应用程序/VM 工作负荷吗？
是的。 但是，若要将 Azure 备份与 System Center Data Protection Manager (DPM) 一起使用，请先安装 DPM，然后再安装 Azure 备份代理。 按此顺序安装 Azure 备份组件可以确保 Azure 备份代理能够与 DPM 一起工作。 不建议也不支持在安装 DPM 之前安装 Azure 备份代理。

### <a name="can-i-use-dpm-to-back-up-apps-in-azure-stack"></a>可以使用 DPM 来备份 Azure Stack 中的应用吗？
不是。 虽然你可以使用 Azure 备份来保护 Azure Stack，但 Azure 备份当前不支持使用 DPM 来备份 Azure Stack 中的应用。

## <a name="how-azure-backup-works"></a>Azure 备份工作原理
### <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>如果在备份作业开始后取消，是否会删除已传输的备份数据？ <br/>
不是。 在备份作业取消之前传输到保管库中的所有数据将保留在保管库中。 Azure 备份使用检查点机制，在备份过程中偶尔要对备份数据添加检查点。 由于备份数据中有检查点，下次备份过程可以验证文件的完整性。 下一次备份作业将是以前备份的数据的增量。 增量备份仅传输新增或更改的数据，这相当于更好地利用带宽。

如果取消了 Azure VM 的备份作业，则已传输的数据会被忽略。 下次备份作业将传输上次成功的备份作业之后的增量数据。

### <a name="are-there-limits-on-when-or-how-many-times-a-backup-job-can-be-scheduledbr"></a>备份作业可计划的时间或次数是否有限制？<br/>
是的。 一天可以在 Windows Server 或 Windows 工作站上运行备份操作最多三次。 一天可以在 System Center DPM 上运行备份操作最多两次。 一天可以运行 IaaS VM 的备份作业一次。 将计划策略用于 Windows Server 或 Windows 工作站，以便指定每日或每周计划。 通过 System Center DPM，可以指定每日、每周、每月和每年计划。

### <a name="why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-i-backed-upbr"></a>为什么传输到恢复服务保管库的数据的大小小于我备份的数据？<br/>
 从 Azure 备份代理、SCDPM 或 Azure 备份服务器备份的所有数据都会在传输之前进行压缩和加密。 应用压缩和加密后，恢复服务保管库中的数据将减少 30-40%。

### <a name="can-i-delete-individual-files-from-a-recovery-point-in-the-vaultbr"></a>可以从保管库中的恢复点删除单个文件吗？<br/>
不可以，Azure 备份不支持从存储的备份中删除或清除单个项。

## <a name="what-can-i-back-up"></a>我能够备份的内容
### <a name="which-operating-systems-does-azure-backup-support-br"></a>Azure 备份支持哪些操作系统？ <br/>
Azure 备份支持以下列表中的操作系统使用 Azure 备份服务器和 System Center Data Protection Manager (DPM) 对文件和文件夹以及受保护的工作负荷应用程序进行备份。

| 操作系统 | 平台 | SKU |
|:--- | --- |:--- |
| Windows 8 和最新的 SP |64 位 |Enterprise、Pro |
| Windows 7 和最新的 SP |64 位 |Ultimate、Enterprise、Professional、Home Premium、Home Basic、Starter |
| Windows 8.1 和最新的 SP |64 位 |Enterprise、Pro |
| Windows 10 |64 位 |Enterprise、Pro、Home |
| Windows Server 2016 |64 位 |Standard、Datacenter、Essentials |
| Windows Server 2012 R2 和最新的 SP |64 位 |Standard、Datacenter、Foundation |
| Windows Server 2012 和最新的 SP |64 位 |Datacenter、Foundation、Standard |
| Windows Storage Server 2016 和最新的 SP |64 位 |Standard、Workgroup |
| Windows Storage Server 2012 R2 和最新的 SP |64 位 |Standard、Workgroup |
| Windows Storage Server 2012 和最新的 SP |64 位 |Standard、Workgroup |
| Windows Server 2012 R2 和最新的 SP |64 位 |Essential |
| Windows Server 2008 R2 SP1 |64 位 |Standard、Enterprise、Datacenter、Foundation |

**对于 Azure VM 备份：**

* **Linux**：Azure 备份支持 [Azure 认可的分发版列表](../virtual-machines/linux/endorsed-distros.md) ，但 Core OS Linux 除外。  只要虚拟机上装有 VM 代理且支持 Python，其他自带 Linux 分发版应该也能正常运行。
* **Windows Server**：不支持低于 Windows Server 2008 R2 的版本。


### <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>要备份的每个数据源的大小是否有限制？ <br/>
虽然 Azure 备份针对数据源强制设定了最大大小，但源的上限值很大。 截至 2015 年 8 月，受支持操作系统的数据源的最大大小为：

| S.No | 操作系统 | 数据源的最大大小 |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 或更高版本 |54,400 GB |
| 2 |Windows 8 或更高版本 |54,400 GB |
| 3 |Windows Server 2008、Windows Server 2008 R2 |1700 GB |
| 4 |Windows 7 |1700 GB |

下表说明了如何确定每个数据源大小。

| 数据源 | 详细信息 |
|:---:|:--- |
| 数据量(Volume) |从服务器或客户端计算机的单个卷备份的数据量 |
| Hyper-V 虚拟机 |所备份虚拟机的所有 VHD 的数据总和 |
| Microsoft SQL Server 数据库 |所备份的单个 SQL 数据库的大小 |
| Microsoft SharePoint |所备份 SharePoint 场中内容和配置数据库的总和 |
| Microsoft Exchange |所备份 Exchange 服务器中所有 Exchange 数据库的总和 |
| BMR/系统状态 |所备份计算机的 BMR 或系统状态的每个副本 |

如果是 Azure IaaS VM 备份，则每个 VM 最多具有 32 个数据磁盘，而每个数据磁盘存储空间最大为 4095 GB。

### <a name="is-there-a-limit-on-the-amount-of-data-held-in-a-recovery-services-vault"></a>恢复服务保管库中备份的数据量是否有限制？
对可以备份到恢复服务保管库的数据量没有限制。

## <a name="retention-policy-and-recovery-points"></a>保留策略和恢复点
### <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-that-is-on-windows-server-without-dpmbr"></a>DPM 和 Windows Server/客户端（即，在不带 DPM 的 Windows Server 上）的保留策略是否有差别？<br/>
否，DPM 和 Windows Server/客户端都有每日、每周、每月和每年保留策略。

### <a name="can-i-configure-my-retention-policies-selectively--that-is-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>是否可以选择性地配置我的保留策略 – 即配置每周和每日保留策略，但不配置每年和每月保留策略？<br/>
是的，Azure 备份保留结构允许根据自己的要求十分灵活地定义保留策略。

### <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>我是否可以计划下午 6 点的备份，同时指定不同时间的保留策略？<br/>
不是。 只能在备份时间点应用保留策略。 在下图中，保留策略是针对上午 12 点和下午 6 点生成的备份指定的。 <br/>

![计划备份和保留期](./media/backup-azure-backup-faq/Schedule.png)
<br/>

### <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>如果备份保留了很长一段时间，是否需要更多时间才能恢复较旧的数据点？ <br/>
否，恢复最旧或最新时间点所需的时间相同。 每个恢复点的行为类似一个完整的点。

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>如果每个恢复点相当于完整的点，它会影响总体可计费备份存储吗？<br/>
典型的长期保留点产品将备份数据存储为完整的点。 完整点的存储 *效率不高* ，但能使还原变得更方便和快速。 增量复制是 *高效* 存储，但要求还原数据链，这会影响恢复时间。 Azure 备份存储体系结构提供这两个领域的最佳产品，它以最佳方式用于快速恢复的数据存储中，产生较低的存储成本。 这种数据存储方法可确保提高（入口和出口）带宽使用效率。 数据存储量和恢复数据所需的时间都会尽量减少。 详细了解[增量备份](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/)的效用。

### <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>可创建的恢复点数量是否有限制？<br/>
最多可为单个受保护实例创建 9999 个恢复点。 受保护的实例是计算机、服务器（物理或虚拟）或配置为向 Azure 备份数据的工作负荷。 有关详细信息，请参阅[备份和保留](./backup-introduction-to-azure-backup.md#backup-and-retention)和[什么是受保护实例？](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance)的说明

### <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>对于已备份到 Azure 的数据，可以执行多少次恢复？<br/>
从 Azure 备份执行恢复的次数没有限制。

### <a name="when-restoring-data-do-i-pay-for-the-egress-traffic-from-azure-br"></a>还原数据时，Azure 的出口流量是否需要付费？ <br/>
不是。 恢复是免费的，不收取传出流量费。

### <a name="what-happens-when-i-change-my-backup-policy"></a>如果更改备份策略，会发生什么情况？
应用新策略时，将遵循新策略的计划和保留期。 如果延长保留期，则会对现有的恢复点进行标记，按新策略要求保留它们。 如果缩短保留期，则会将其标记为在下一清理作业中删除，随后会将其删除。

## <a name="azure-backup-encryption"></a>Azure 备份加密
### <a name="is-the-data-sent-to-azure-encrypted-br"></a>发送到 Azure 的数据会加密吗？ <br/>
是的。 数据会在本地 SCDPM 服务器/客户端/SCDPM 计算机上使用 AES256 加密，并通过安全的 HTTPS 链接发送。

### <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Azure 中的备份数据也会加密吗？<br/>
是的。 发送到 Azure 的数据将保持加密（静态加密）。 Microsoft 不会解密任何位置的备份数据。 备份 Azure VM 时，Azure 备份依赖于虚拟机的加密。 例如，如果使用 Azure 磁盘加密或其他某种加密技术对 VM 进行了加密，则 Azure 备份会使用同样的加密技术来保护数据。

### <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>用于加密备份数据的加密密钥的最小长度是多少？ <br/>
使用 Azure 备份代理时，加密密钥至少应该为 16 个字符。 就 Azure VM 来说，Azure KeyVault 所使用的密钥没有长度限制。

### <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>如果我丢失了加密密钥，会发生什么情况？ 是否可以恢复数据（或者）Microsoft 是否可以恢复数据？ <br/>
用于加密备份数据的密钥只能放置在客户场地。 Microsoft 不会在 Azure 中保留副本，并且无权访问密钥。 如果客户丢失了密钥，Microsoft 将无法恢复备份的数据。
