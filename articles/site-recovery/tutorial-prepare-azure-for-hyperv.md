---
title: 准备 Azure 以使用 Azure Site Recovery 对本地 Hyper-V VM 进行灾难恢复 | Microsoft Docs
description: 了解如何使用 Azure Site Recovery 准备 Azure，对本地 Hyper-V VM 进行灾难恢复。
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 48101e49429225018381ed2a3b1e8e4e351c15a6
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59362604"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>准备 Azure 资源，以便对本地计算机进行灾难恢复

 [Azure Site Recovery](site-recovery-overview.md) 通过在计划内和计划外中断期间使商业应用程序保持启动和运行状态，有助于实施业务连续性和灾难恢复 (BCDR) 策略。 Site Recovery 管理并安排本地计算机和 Azure 虚拟机 (VM) 的灾难恢复，包括复制、故障转移和恢复。

本文是此系列的第一个教程，演示如何为本地 VM 设置灾难恢复。 如果要保护 Hyper-V VM，它是相关的。

> [!NOTE]
> 教程旨在向你展示方案的最简单部署路径。 它们尽可能使用默认选项，并且不显示所有可能的设置和路径。 有关详细说明，请参阅相应方案的“操作方法”部分。

本文介绍如何在将本地 VM (Hyper-V) 复制到 Azure 时准备 Azure 组件。 本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 验证 Azure 帐户是否具有复制权限。
> * 创建 Azure 存储帐户。 已复制计算机的映像存储在其中。
> * 创建恢复服务保管库。 保管库保存 VM 和其他复制组件的元数据和配置信息。
> * 设置 Azure 网络。 在故障转移后创建的 Azure VM 会加入此 Azure 网络。

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="sign-in-to-azure"></a>登录 Azure

登录到 [Azure 门户](http://portal.azure.com)。

## <a name="verify-account-permissions"></a>验证帐户权限

如果你刚刚创建了免费 Azure 帐户，那么你就是订阅的管理员。 如果你不是订阅管理员，请要求管理员分配你所需的权限。 若要为新虚拟机启用复制，必须有权执行以下操作：

- 在所选资源组中创建 VM。
- 在所选虚拟网络中创建 VM。
- 向所选存储帐户进行写入。

若要完成这些任务，应为帐户分配“虚拟机参与者”内置角色。 此外，若要在保管库中管理 Site Recovery 操作，应为帐户分配“Site Recovery 参与者”内置角色。

## <a name="create-a-storage-account"></a>创建存储帐户

已复制计算机的映像保存在 Azure 存储中。 在从本地故障转移到 Azure 时，会从该存储中创建 Azure VM。 存储帐户必须位于与恢复服务保管库相同的区域。 在本教程中，我们将使用西欧。

1. 在 [Azure 门户](https://portal.azure.com)菜单中，选择“创建资源” > “存储” > “存储帐户 - blob、文件、表、队列”。
2. 在“创建存储帐户”中，输入帐户的名称。 对于这些教程，我们使用 **contosovmsacct1910171607**。 所选名称在 Azure 中必须唯一，长度介于 3-24 个字符，且仅包含数字和小写字母。
3. 在“部署模型”中，选择“资源管理器”。
4. 在“帐户类型”中，选择“存储(常规用途 v1)”。 请不要选择 blob 存储。
5. 在“复制”中，选择默认的“读取访问异地冗余存储”作为存储冗余。 我们将“需要安全传输”保留为“已禁用”。
6. 在“性能”中，选择“标准”，在“访问层”中选择默认选项“热”。
7. 在“订阅”中，选择要在其中创建新存储帐户的订阅。
8. 在“资源组”中，输入新的资源组。 Azure 资源组是在其中部署和管理 Azure 资源的逻辑容器。 对于这些教程，我们使用 **ContosoRG**。
9. 在“位置”中，选择存储帐户的地理位置。 

   ![创建存储帐户](media/tutorial-prepare-azure/create-storageacct.png)

9. 选择“创建”以创建存储帐户。

## <a name="create-a-recovery-services-vault"></a>创建恢复服务保管库

1. 在 Azure 门户中单击“+创建资源”，然后在市场中搜索“恢复服务”。
2. 单击“备份和站点恢复(OMS)”，然后在“备份和站点恢复”页中单击“创建”。 
1. 在“恢复服务保管库” > “名称”中，输入一个友好名称以标识此保管库。 对于这组教程，我们使用 **ContosoVMVault**。
2. 在**资源组**中，选择现有资源组或创建新资源组。 在本教程中，我们使用 **contosoRG**。
3. 在**位置**中，选择保管库应位于的区域。 我们将使用“西欧”。
4. 若要从仪表板快速访问保管库，请选择“固定到仪表板” > “创建”。

   ![创建新的保管库](./media/tutorial-prepare-azure/new-vault-settings.png)

   新保管库显示在“仪表板” > “所有资源”中，以及“恢复服务保管库”主页上。

## <a name="set-up-an-azure-network"></a>设置 Azure 网络

在故障转移后创建的 Azure VM 会加入此网络。

1. 在 [Azure 门户](https://portal.azure.com)中，选择“创建资源” > “网络” > “虚拟网络”。
2. 我们保留将“资源管理器”选为部署模型。
3. 在“名称”中，输入网络名称。 名称在 Azure 资源组中必须唯一。 在本教程中我们将使用 **ContosoASRnet**。
4. 指定将在其中创建网络的资源组。 我们将使用现有资源组 contosoRG。
5. 在“地址范围”中，输入网络范围 **10.0.0.0/24**。 在此网络中，我们不使用子网。
6. 在“订阅”中，选择要在其中创建网络的订阅。
7. 在“位置”中，选择“西欧”。 该网络必须位于与恢复服务保管库相同的区域中。
8. 我们将保留基本 DDoS 防护的默认选项，网络上没有服务终结点。
9. 单击“创建”。

   ![创建虚拟网络](media/tutorial-prepare-azure/create-network.png)

   创建虚拟网络需要几秒钟的时间。 创建后，即可在 Azure 门户仪表板中看到它。

## <a name="useful-links"></a>有用链接

- [了解](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) Azure 网络。
- [了解](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)托管磁盘。



## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [准备本地 Hyper-V 基础结构以灾难恢复到 Azure](hyper-v-prepare-on-premises-tutorial.md)
