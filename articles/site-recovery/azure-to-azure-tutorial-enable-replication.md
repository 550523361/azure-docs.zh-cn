---
title: 使用 Azure Site Recovery 设置 Azure VM 灾难恢复
description: 了解如何使用 Azure Site Recovery 服务为 Azure VM 设置到其他 Azure 区域的灾难恢复。
ms.topic: tutorial
ms.date: 1/24/2020
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 50bf1ec7f21ccbc3a3fa8feaea02e45bd08a158a
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87421410"
---
# <a name="set-up-disaster-recovery-for-azure-vms"></a>为 Azure VM 设置灾难恢复

[Azure Site Recovery](site-recovery-overview.md) 服务可管理和协调本地计算机和 Azure 虚拟机 (VM) 的复制、故障转移和故障回复，因而有利于灾难恢复策略。

本教程介绍如何为 Azure VM 设置灾难恢复：将 VM 从一个 Azure 区域复制到另一个区域。 本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 创建恢复服务保管库
> * 验证目标资源设置
> * 设置 VM 的出站网络连接
> * 为虚拟机启用复制

> [!NOTE]
> 本文说明了如何使用最简单的设置来部署灾难恢复。 若要了解自定义的设置，请查看[“操作方法”部分](azure-to-azure-how-to-enable-replication.md)中的文章。

## <a name="prerequisites"></a>先决条件

完成本教程：

- 查看[方案体系结构和组件](./azure-to-azure-architecture.md)。
- 在开始之前，请查看[支持要求](./azure-to-azure-support-matrix.md)。

## <a name="create-a-recovery-services-vault"></a>创建恢复服务保管库

在除了源区域之外的任意区域中创建保管库。

1. 登录到 [Azure 门户](https://portal.azure.com)。
1. 在 Azure 门户菜单或“主页”页上，选择“创建资源” 。 然后，选择“IT 和管理工具” > “备份和 Site Recovery” 。
1. 在“名称” 中，指定一个友好名称以标识该保管库。 如果有多个订阅，请选择合适的一个。
1. 创建一个资源组或选择一个现有的资源组。 指定 Azure 区域。 若要查看受支持的区域，请参阅 [Azure Site Recovery 定价详细信息](https://azure.microsoft.com/pricing/details/site-recovery/)中的“地域可用性”。
1. 若要从仪表板访问保管库，请选择“固定到仪表板”，然后选择“创建”。

   ![新保管库](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

新保管库将添加到“仪表板”中的“所有资源”下，以及“恢复服务保管库”主页面上。

## <a name="verify-target-resource-settings"></a>验证目标资源设置

检查目标区域的 Azure 订阅。

- 验证 Azure 订阅是否允许在目标区域中创建 VM。 请联系支持部门，启用所需配额。
- 确保订阅中有足够的资源，能够支持与源 VM 匹配的 VM 大小。 Site Recovery 会为目标 VM 选择相同的大小或尽可能接近的大小。

## <a name="set-up-outbound-network-connectivity-for-vms"></a>设置 VM 的出站网络连接

若要使 Site Recovery 按预期工作，需在要复制的 VM 中对出站网络连接进行修改。

> [!NOTE]
> Site Recovery 不支持使用身份验证代理来控制网络连接。

### <a name="outbound-connectivity-for-urls"></a>URL 的出站连接

如果使用基于 URL 的防火墙代理来控制出站连接，请允许访问以下 URL：

| **名称**                  | 商用                               | 政府                                 | **说明** |
| ------------------------- | -------------------------------------------- | ---------------------------------------------- | ----------- |
| 存储                   | `*.blob.core.windows.net`                  | `*.blob.core.usgovcloudapi.net`              | 允许将数据从 VM 写入源区域中的缓存存储帐户。 |
| Azure Active Directory    | `login.microsoftonline.com`                | `login.microsoftonline.us`                   | 向 Site Recovery 服务 URL 提供授权和身份验证。 |
| 复制               | `*.hypervrecoverymanager.windowsazure.com` | `*.hypervrecoverymanager.windowsazure.com`   | 允许 VM 与 Site Recovery 服务进行通信。 |
| 服务总线               | `*.servicebus.windows.net`                 | `*.servicebus.usgovcloudapi.net`             | 允许 VM 写入 Site Recovery 监视和诊断数据。 |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>IP 地址范围的出站连接

如果使用的是网络安全组 (NSG)，请创建基于服务标记的 NSG 规则，以访问 Azure 存储、Azure Active Directory、Site Recovery 服务和 Site Recovery 监视。 [了解详细信息](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags)。

## <a name="verify-azure-vm-certificates"></a>验证 Azure VM 证书

检查要复制的 VM 是否有最新的根证书。 如果没有，则 VM 会由于安全约束而无法注册到 Site Recovery。

- 对于 Windows VM，请在 VM 上安装所有最新的 Windows 更新，使所有受信任的根证书位于该计算机上。 在未联网的环境中，请按照你的组织的标准 Windows 更新和证书更新过程执行操作。
- 对于 Linux VM，请遵循 Linux 分销商提供的指导，在 VM 上获取最新的受信任根证书和证书吊销列表。

## <a name="set-permissions-on-the-account"></a>设置帐户权限

Azure Site Recovery 提供了三个用于控制 Site Recovery 管理操作的内置角色。

- **Site Recovery 参与者** - 此角色拥有管理恢复服务保管库中的 Azure Site Recovery 操作所需的所有权限。 不过，拥有此角色的用户既无法创建或删除恢复服务保管库，也无法向其他用户分配访问权限。 此角色最适合分配给灾难恢复管理员，这样他们就可以为应用程序或整个组织启用和管理灾难恢复。

- **Site Recovery 操作员** - 此角色有权执行和管理故障转移和故障回复操作。 拥有此角色的用户无法启用或禁用复制、无法创建或删除保管库，也无法注册新的基础结构或向其他用户分配访问权限。 此角色最适合分配给灾难恢复操作员，这样他们就可以遵循应用程序所有者或 IT 管理员的指示，对虚拟机或应用程序进行故障转移。 解决灾难后，灾难恢复操作员可以对虚拟机进行重新保护和故障回复。

- **Site Recovery 读者** - 此角色有权查看所有 Site Recovery 管理操作。 此角色最适合分配给 IT 监视主管，这样他们就可以监视当前保护状态并创建支持票证。

详细了解 [Azure 内置角色](../role-based-access-control/built-in-roles.md)。

## <a name="enable-replication-for-a-vm"></a>为虚拟机启用复制

以下各部分介绍了如何启用复制。

### <a name="select-the-source"></a>选择源

若要开始进行复制设置，请选择你的 Azure VM 在其中运行的源。

1. 转到“恢复服务保管库”，选择保管库名称，然后选择“+复制”。
1. 对于“源”，选择“Azure”。
1. 在“源位置”中，选择当前运行 VM 的 Azure 源区域。
1. 选择运行虚拟机的**源订阅**。 这可以是存在恢复服务保管库的同一 Azure Active Directory 租户中的任何订阅。
1. 选择“源资源组”，然后选择“确定”以保存设置。

   ![设置源](./media/azure-to-azure-tutorial-enable-replication/source.png)

### <a name="select-the-vms"></a>选择 VM

Site Recovery 检索与订阅和资源组/云服务关联的 VM 列表。

1. 在“虚拟机”中，选择要复制的 VM。
1. 选择“确定” 。

### <a name="configure-replication-settings"></a>配置复制设置

Site Recovery 会针对目标区域创建默认设置和复制策略。 可以根据需要更改这些设置。

1. 选择“设置”以查看目标设置和复制设置。

1. 若要替代默认目标设置，请选择“资源组、网络、存储和可用性”旁边的“自定义” 。

   ![配置设置](./media/azure-to-azure-tutorial-enable-replication/settings.png)

1. 根据下表中的摘要内容自定义目标设置。

   | **设置** | **详细信息** |
   | --- | --- |
   | **目标订阅** | 默认情况下，目标订阅与源订阅相同。 选择“自定义”以在同一 Azure Active Directory 租户中选择其他目标订阅。 |
   | **目标位置** | 用于灾难恢复的目标区域。<br/><br/> 建议选择与 Site Recovery 保管库位置匹配的目标位置。 |
   | 目标资源组 | 故障转移后，目标区域中用于容纳 Azure VM 的资源组。<br/><br/> 默认情况下，Site Recovery 会在目标位置中创建一个带有 `asr` 后缀的新资源组。 目标资源组的位置可以是除托管源虚拟机区域以外的任何区域。 |
   | 目标虚拟网络 | 故障转移后，目标区域中 VM 所位于的网络。<br/><br/> 默认情况下，Site Recovery 会在目标位置中创建一个带有 `asr` 后缀的新虚拟网络（以及子网）。 |
   | 缓存存储帐户 | Site Recovery 使用源区域中的一个存储帐户。 复制到目标位置之前，对源 VM 的更改将发送到此帐户。<br/><br/> 如果使用支持防火墙的缓存存储帐户，请确保启用“允许受信任的 Microsoft 服务”。 [了解详细信息](../storage/common/storage-network-security.md#exceptions)。 同时，请确保允许访问至少一个源 Vnet 子网。 |
   | **目标存储帐户(源 VM 使用非托管磁盘)** | 默认情况下，Site Recovery 会在目标区域中创建新存储帐户，从而形成源 VM 存储帐户的镜像。<br/><br/> 如果使用支持防火墙的缓存存储帐户，请启用“允许受信任的 Microsoft 服务”。 |
   | **副本托管磁盘(如果源 VM 使用托管磁盘)** | 默认情况下，Site Recovery 在目标区域中创建副本托管磁盘，以生成和源 VM 的托管磁盘存储类型一致（标准或高级）的镜像磁盘。 你只能自定义磁盘类型。 |
   | 目标可用性集 | 默认情况下，Azure Site Recovery 会在目标区域中创建一个名称带有 `asr` 后缀（针对源区域中可用性集的 VM 部分）的新可用性集。 如果 Azure Site Recovery 创建的可用性集已存在，则会重复使用。 |
   | **目标可用性区域** | 默认情况下，Site Recovery 会在目标区域中分配与源区域相同的区域编号，前提是目标区域支持可用性区域。<br/><br/> 如果目标区域不支持可用性区域，则会将目标 VM 默认配置为单一实例。<br/><br/> 选择“自定义”，将 VM 配置为目标区域中可用性集的一部分。<br/><br/> 启用复制后，无法更改可用性类型（单一实例、可用性集或可用性区域）。 若要更改可用性类型，请先禁用然后再启用复制。 |

1. 若要自定义复制策略设置，请选择“复制策略”旁边的“自定义”，然后根据需要修改设置。

   | **设置** | **详细信息** |
   | --- | --- |
   | **复制策略名称** | 策略名称。 |
   | **恢复点保留期** | 默认情况下，Site Recovery 会将恢复点保留 24 小时。 可将此值配置为 1 - 72 小时。 |
   | **应用一致性快照频率** | 默认情况下，Site Recovery 每隔 4 小时创建应用一致性快照。 可将此值配置为 1 - 12 小时之间的任何值。<br/><br/> 应用一致的快照是 VM 内应用程序数据的时间点快照。 卷影复制服务 (VSS) 确保 VM 上的应用在拍摄快照时处于一致状态。 |
   | **复制组** | 如果应用程序需要跨 VM 的多 VM 一致性，可为这些 VM 创建一个复制组。 默认情况下，所选的 VM 不属于任何复制组。 |

1. 若要将 VM 添加到新的或现有的复制组，请在“自定义”中选择“是”以确保多 VM 一致性。 然后选择“确定”。

   > [!NOTE]
   > - 故障转移时，复制组中的所有计算机将获得共享的崩溃一致性恢复点和应用程序一致性恢复点。
   > - 启用（CPU 密集型的）多 VM 一致性会影响工作负荷的性能。 仅当计算机运行相同的工作负荷并且你需要在多个计算机之间保持一致时，才使用此功能。
   > - 在一个复制组中最多可以包含 16 个 VM。
   > - 如果启用了多 VM 一致性，则复制组中的计算机将通过端口 20004 相互通信。 确保没有防火墙阻止 VM 通过此端口进行内部通信。
   > - 对于复制组中的 Linux VM，请确保根据适用于 Linux 版本的指导手动打开端口 20004 上的出站流量。

### <a name="configure-encryption-settings"></a>配置加密设置

如果源 VM 上已启用 Azure 磁盘加密 (ADE)，请检查设置。

1. 验证设置：
   1. **磁盘加密 Key Vault**：默认情况下，Site Recovery 会在源 VM 磁盘加密密钥中创建新的密钥保管库，其名称带有 `asr` 后缀。 如果已存在密钥保管库，则会重复使用它。
   1. **密钥加密 Key Vault**：默认情况下，Site Recovery 会在目标区域中创建新的 Key Vault， 其名称带有 `asr` 后缀，并且基于源 VM 的密钥加密密钥。 如果 Site recovery 创建的 Key Vault 已存在，则会重复使用它。
1. 选择“自定义”以选择自定义密钥保管库。

>[!NOTE]
> 对于运行 Windows 操作系统的 VM，Site Recovery 当前支持 ADE（无论是否使用 Azure Active Directory (AAD)）。 对于 Linux 操作系统，我们仅支持不使用 AAD 的 ADE。 此外，对于运行 ADE 1.1 （不使用 AAD）的计算机，VM 必须使用托管磁盘。 不支持包含非托管磁盘的 VM。 如果从 ADE 0.1（使用 AAD）切换到 ADE 1.1，则需要在启用 ADE 1.1 后对 VM 先禁用复制，然后再启用复制。

### <a name="track-replication-status"></a>跟踪复制状态

启用复制后，可以跟踪作业的状态。

1. 在“设置”中，选择“刷新”以获取最新状态。
1. 跟踪进度和状态，如下所示：
   1. 在“设置” > “作业” > “Site Recovery 作业”中，跟踪“启用保护”作业的进度。
   1. 在“设置” > “复制的项”中，可以查看 VM 的状态和初始复制进度。 选择 VM 以向下钻取其设置。

## <a name="next-steps"></a>后续步骤

在本教程中，已经为 Azure VM 配置了灾难恢复。 现在，你可以运行灾难恢复演练来检查故障转移是否按预期工作。

> [!div class="nextstepaction"]
> [运行灾难恢复演练](azure-to-azure-tutorial-dr-drill.md)
