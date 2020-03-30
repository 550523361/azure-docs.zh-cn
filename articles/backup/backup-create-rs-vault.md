---
title: 创建恢复服务保管库
description: 本文介绍如何创建用于存储备份和恢复点的恢复服务保管库。
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: e722996f516d21445d8e0028df925ca44eb02bfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295018"
---
# <a name="create-a-recovery-services-vault"></a>创建恢复服务保管库

恢复服务保管库是用于存储在不同时间创建的备份和恢复点的实体。 恢复服务保管库还包含与受保护虚拟机关联的备份策略。

若要创建恢复服务保管库，请执行以下操作：

1. 在 [Azure 门户](https://portal.azure.com/)中登录到自己的订阅。

2. 在左侧菜单中，选择“所有服务”  。

    ![选择“所有服务”](./media/backup-create-rs-vault/click-all-services.png)

3. 在“所有服务”对话框中，输入“恢复服务”   。 资源列表根据输入进行筛选。 在资源列表中，选择“恢复服务保管库”  。

    ![输入并选择“恢复服务保管库”](./media/backup-create-rs-vault/all-services.png)

    此时会显示订阅中的恢复服务保管库列表。

4. 在“恢复服务保管库”仪表板上，选择“添加”   。

    ![添加恢复服务保管库](./media/backup-create-rs-vault/add-button-create-vault.png)

    此时会打开“恢复服务保管库”对话框  。 提供“名称”、“订阅”、“资源组”和“位置”的值     。

    ![配置恢复服务保管库](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **名称**：输入一个友好名称以标识此保管库。 名称对于 Azure 订阅必须是唯一的。 指定的名称应至少包含 2 个字符，最多不超过 50 个字符。 名称必须以字母开头且只能包含字母、数字和连字符。
   - **订阅**：选择要使用的订阅。 如果你仅是一个订阅的成员，则会看到该名称。 如果不确定要使用哪个订阅，请使用默认的（建议的）订阅。 仅当工作或学校帐户与多个 Azure 订阅关联时，才会显示多个选项。
   - **资源组**：使用现有资源组或创建新组。 要查看订阅中可用的资源组列表，请选择“使用现有资源”****，然后从下拉列表框中选择一个资源。 若要创建新资源组，请选择“新建”，然后输入名称  。 有关资源组的完整信息，请参阅 [Azure 资源管理器概述](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)。
   - **位置**：选择保管库的地理区域。 要创建保管库以保护虚拟机，保管库必须与虚拟机位于同一区域中  。

      > [!IMPORTANT]
      > 如果不确定 VM 的位置，请关闭对话框。 转到门户中的虚拟机列表。 如果虚拟机位于多个区域，请在每个区域中创建一个恢复服务保管库。 先在第一个位置创建保管库，然后再为其他位置创建保管库。 无需指定存储帐户即可存储备份数据。 恢复服务保管库和 Azure 备份服务会自动处理这种情况。
      >
      >

5. 准备好创建恢复服务保管库后，选择“创建”  。

    ![创建恢复服务保管库](./media/backup-create-rs-vault/click-create-button.png)

    创建恢复服务保管库可能需要一段时间。 可在门户右上角“通知”区域监视状态通知  。 创建保管库后，它会显示在“恢复服务保管库”的列表中。 如果未看到创建的保管库，请选择“刷新”  。

     ![刷新备份保管库列表](./media/backup-create-rs-vault/refresh-button.png)

## <a name="set-storage-redundancy"></a>设置存储冗余

Azure 备份会自动处理保管库的存储。 需要指定如何复制该存储。

1. 在“恢复服务保管库”**** 边栏选项卡中，单击新保管库。 在“设置”部分下，单击“属性”********。
2. 在“属性”**** 中的“备份配置”**** 下，单击“更新”****。

3. 选择存储复制类型，然后单击“保存”****。

     ![设置新保管库的存储配置](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

   - 如果使用 Azure 作为主要备份存储终结点，则我们建议继续使用默认的“异地冗余”设置。****
   - 如果不使用 Azure 作为主要的备份存储终结点，则请选择“本地冗余”，减少 Azure 存储费用。****
   - 详细了解[异地冗余](../storage/common/storage-redundancy-grs.md)和[本地冗余](../storage/common/storage-redundancy-lrs.md)。

> [!NOTE]
> 在保管库中配置备份之前，必须更改恢复服务保管库的**存储复制类型**（本地冗余/异地冗余）。 配置备份后，将禁用修改选项，并且无法更改**存储复制类型**。

## <a name="set-cross-region-restore"></a>设置交叉区域还原

作为还原选项之一，跨区域还原 （CRR） 允许您在辅助区域（即[Azure 配对区域](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)）中还原 Azure VM。 此选项允许您：

- 当有审计或合规性要求时进行演习
- 如果主区域中发生灾难，请还原 VM 或其磁盘。

要选择此功能，请从 **"备份配置"** 边栏选项卡中选择 **"启用跨区域还原**"。

对于此过程，存在定价影响，因为它是在存储级别。

>[!NOTE]
>开始之前：
>
>- 查看[支持类型和区域列表的支持矩阵](backup-support-matrix.md#cross-region-restore)。
>- 跨区域还原 （CRR） 功能目前仅在以下区域可用： 
>    - 美国中西部
>    - 美国西部 2
>    - 美国中南部
>    - 美国东部
>    - 美国东部 2
>    - 美国中北部
>    - 加拿大中部
>    - 加拿大东部
>    - 澳大利亚东部
>    - 澳大利亚东南部
>    - 印度中部
>    - 印度南部
>    - 日本东部
>    - 日本西部
>    - 东南亚
>    - 英国南部
>    - 英国西部
>    - 法国中部
>    - 韩国中部
>    - 韩国南部
>- CRR 是任何 GRS 保管库的保管库级选择加入功能（默认情况下关闭）。
>- 请使用以下命令将订阅上载此功能：<br>
>  `Register-AzProviderFeature -FeatureName CrossRegionRestore -ProviderNamespace Microsoft.RecoveryServices`
>- 如果您在公共有限预览期间加入此功能，审核审批电子邮件将包括定价政策详细信息。
>- 选择加入后，备份项目可能需要长达 48 小时才能在辅助区域中可用。
>- 目前，CRR 仅支持备份管理类型 - ARM Azure VM（不支持经典 Azure VM）。  当其他管理类型支持 CRR 时 **，它们将自动**注册。

### <a name="configure-cross-region-restore"></a>配置跨区域还原

使用 GRS 冗余创建的保管库包括配置跨区域还原功能的选项。 每个 GRS 保管库都将有一个横幅，该横幅将链接到文档。 要为保管库配置 CRR，请转到备份配置边栏选项卡，该边栏选项卡包含启用此功能的选项。

 ![备份配置横幅](./media/backup-azure-arm-restore-vms/banner.png)

1. 从门户转到恢复服务保管库>设置>属性。
2. 单击**此保管库中启用跨区域还原**以启用该功能。

   ![单击此保管库中的启用跨区域还原之前](./media/backup-azure-arm-restore-vms/backup-configuration1.png)

   ![单击此保管库中的启用跨区域还原后](./media/backup-azure-arm-restore-vms/backup-configuration2.png)

了解如何[查看辅助区域中的备份项](backup-azure-arm-restore-vms.md#view-backup-items-in-secondary-region)。

了解如何[在辅助区域中还原](backup-azure-arm-restore-vms.md#restore-in-secondary-region)。

了解如何[监视辅助区域还原作业](backup-azure-arm-restore-vms.md#monitoring-secondary-region-restore-jobs)。

## <a name="modifying-default-settings"></a>修改默认设置

我们强烈建议您在在保管库中配置备份之前查看**存储复制类型****和安全设置**的默认设置。

- 默认情况下 **，存储复制类型**设置为**异地冗余**。 配置备份后，将禁用修改选项。 按照[以下步骤](https://docs.microsoft.com/azure/backup/backup-create-rs-vault#set-storage-redundancy)查看和修改设置。

- 默认情况下，**软删除**在新创建的保管库中**启用**，以保护备份数据免遭意外或恶意删除。 按照[以下步骤](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud#disabling-soft-delete)查看和修改设置。

## <a name="next-steps"></a>后续步骤

[了解](backup-azure-recovery-services-vault-overview.md)恢复服务保管库。
[了解](backup-azure-delete-vault.md)如何删除恢复服务保管库。
