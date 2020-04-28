---
title: 删除 Microsoft Azure 恢复服务保管库
description: 本文介绍如何删除依赖项，然后删除 Azure 备份恢复服务保管库。
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 5fcf8004cd5792b30ec57537d5d8ab0bc085dfb3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82183749"
---
# <a name="delete-an-azure-backup-recovery-services-vault"></a>删除 Azure 备份恢复服务保管库

本文介绍如何删除 [Azure 备份](backup-overview.md)恢复服务保管库。 其中分别说明了如何删除依赖项，以及如何删除保管库。

## <a name="before-you-start"></a>开始之前

无法删除具有依赖项（例如，与保管库关联的受保护服务器或备份管理服务器）的恢复服务保管库。

- 无法删除包含备份数据的保管库（即，停止了保护，但保留了备份数据）。

- 如果删除包含依赖项的保管库，将显示以下消息：

  ![删除保管库出错。](./media/backup-azure-delete-vault/error.png)

- 如果从门户中删除包含依赖项的本地受保护项，将显示警告消息：

  ![删除受保护的服务器出错。](./media/backup-azure-delete-vault/error-message.jpg)

- 如果备份项处于软删除状态，将显示以下警告消息，你将需要等到这些备份项被永久删除。 有关详细信息，请参阅[此文](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud)。

   ![删除保管库出错。](./media/backup-azure-delete-vault/error-message-soft-delete.png)

- 不能删除已注册存储帐户的保管库。 若要了解如何注销帐户，请参阅[取消注册存储帐户](manage-afs-backup.md#unregister-a-storage-account)。
  
若要删除保管库，请选择与设置相匹配的方案，然后执行建议的步骤：

方案 | 删除依赖项以删除保管库的步骤 |
-- | --
我的本地文件和文件夹已备份到 Azure，并已使用 Azure 备份代理进行保护 | 执行[从 MARS 管理控制台删除备份项](#delete-backup-items-from-the-mars-management-console)中的步骤
我的本地计算机已在 Azure 中使用 MABS（Microsoft Azure 备份服务器）或 DPM (System Center Data Protection Manager) 进行保护 | 执行[从 MABS 管理控制台删除备份项](#delete-backup-items-from-the-mabs-management-console)中的步骤
我在云中有受保护的项（例如 laaS 虚拟机或 Azure 文件共享）  | 执行[删除云中的受保护项](#delete-protected-items-in-the-cloud)中的步骤
我在本地和云中都有受保护的项 | 按顺序执行以下所有部分中的步骤： <br> 1.[删除云中受保护的项](#delete-protected-items-in-the-cloud)<br> 2.[从 MARS 管理控制台删除备份项](#delete-backup-items-from-the-mars-management-console) <br> 3.[从 MABS 管理控制台中删除备份项](#delete-backup-items-from-the-mabs-management-console)
我在本地或云中没有任何受保护的项，但仍然收到保管库删除错误 | 执行[使用 Azure 资源管理器删除恢复服务保管库](#delete-the-recovery-services-vault-by-using-azure-resource-manager)中的步骤 <br><br> 确保没有向保管库注册存储帐户。 若要了解如何注销帐户，请参阅[取消注册存储帐户](manage-afs-backup.md#unregister-a-storage-account)。

## <a name="delete-protected-items-in-the-cloud"></a>删除云中的受保护项

首先，请阅读**[开始之前](#before-you-start)** 部分，以了解依赖项和保管库删除过程。

若要停止保护并删除备份数据，请执行以下操作：

1. 在门户中依次转到“恢复服务保管库”、“备份项”。******** 然后，选择云中的受保护项（例如，Azure 虚拟机、Azure 存储 [Azure 文件服务] 或 Azure 虚拟机上的 SQL Server）。

    ![选择备份类型。](./media/backup-azure-delete-vault/azure-storage-selected.png)

2. 右键单击并选择备份项。 根据该备份项是否受保护，菜单将显示“停止备份”窗格或“删除备份数据”窗格。********

    - 如果显示了“停止备份”窗格，请从下拉菜单中选择“删除备份数据”。******** 输入备份项的名称（此字段区分大小写），然后从下拉菜单中选择原因。 输入备注（如果有）。 然后选择“停止备份”。****

        ![“停止备份”窗格。](./media/backup-azure-delete-vault/stop-backup-item.png)

    - 如果显示了“删除备份数据”窗格，请输入备份项的名称（此字段区分大小写），然后从下拉菜单中选择原因。**** 输入备注（如果有）。 然后选择“删除”****。

         ![“删除备份数据”窗格。](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

3. 检查**通知**图标： ![通知图标。](./media/backup-azure-delete-vault/messages.png) 完成此过程后，服务将显示以下消息：*正在停止备份并删除 "* 备份项 *"* 的备份数据。 已成功完成该操作”。**
4. 在“备份项”菜单中选择“刷新”，确保已删除该备份项。********

      ![“删除备份项”页。](./media/backup-azure-delete-vault/empty-items-list.png)

## <a name="delete-protected-items-on-premises"></a>删除本地的受保护项

首先，请阅读**[开始之前](#before-you-start)** 部分，以了解依赖项和保管库删除过程。

1. 在保管库仪表板菜单中，选择“备份基础结构”****。
2. 根据本地方案选择以下选项之一：

      - 对于 MARS，请依次选择“受保护的服务器”、“Azure 备份代理”。******** 然后选择要删除的服务器。

        ![对于 MARS，请选择保管库打开其仪表板。](./media/backup-azure-delete-vault/identify-protected-servers.png)

      - 对于 MABS 或 DPM，请选择“备份管理服务器”。**** 然后选择要删除的服务器。

          ![对于 MABS，请选择保管库打开其仪表板。](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. 此时会显示包含警告消息的“删除”窗格。****

     ![“删除”窗格。](./media/backup-azure-delete-vault/delete-protected-server.png)

     查看警告消息，以及许可复选框中的说明。
    > [!NOTE]
    >
    >- 如果受保护的服务器已与 Azure 服务同步，并且备份项存在，则许可复选框将显示相关备份项的数量，以及用于查看备份项的链接。
    >- 如果受保护的服务器未与 Azure 服务同步，并且备份项存在，则许可复选框只会显示备份项的数量。
    >- 如果没有任何备份项，则许可复选框将要求确认删除。

4. 选中许可复选框，然后选择“删除”。****

5. 查看“通知”图标![删除备份数据](./media/backup-azure-delete-vault/messages.png)。**** 操作完成后，服务将显示消息：*正在停止备份并删除 "备份项" 的备份数据。* 已成功完成该操作”。**
6. 在“备份项”菜单中选择“刷新”，确保已删除该备份项。********

此过程完成后，可以继续从管理控制台删除备份项：

- [从 MARS 管理控制台删除备份项](#delete-backup-items-from-the-mars-management-console)
- [从 MABS 管理控制台删除备份项](#delete-backup-items-from-the-mabs-management-console)

### <a name="delete-backup-items-from-the-mars-management-console"></a>从 MARS 管理控制台删除备份项

1. 打开 MARS 管理控制台，转到“操作”窗格并选择“计划备份”。********
2. 在“修改或停止计划的备份”页中选择“停止使用此备份计划并删除所有存储的备份”选项。******** 然后，选择“下一步”  。

    ![修改或停止计划的备份。](./media/backup-azure-delete-vault/modify-schedule-backup.png)

3. 在“停止计划的备份”页中选择“完成”。********

    ![停止计划的备份。](./media/backup-azure-delete-vault/stop-schedule-backup.png)
4. 系统会提示输入安全 PIN（个人标识号）。必须手动生成该 PIN。 为此，请先登录到 Azure 门户。
5. 请参阅**恢复服务保管库** > **设置** > **属性**。
6. 在“安全 PIN”下选择“生成”********。 复制此 PIN。 该 PIN 的有效时间仅为五分钟。
7. 在管理控制台中粘贴该 PIN，然后选择“确定”。****

    ![生成安全 PIN。](./media/backup-azure-delete-vault/security-pin.png)

8. 在 "**修改备份进度**" 页中，将显示以下消息：*已删除的备份数据将保留14天。之后，备份数据将被永久删除。*  

    ![删除备份基础结构。](./media/backup-azure-delete-vault/deleted-backup-data.png)

删除本地备份项后，遵循门户中的后续步骤。

### <a name="delete-backup-items-from-the-mabs-management-console"></a>从 MABS 管理控制台删除备份项

可以使用两种方法从 MABS 管理控制台删除备份项。

#### <a name="method-1"></a>方法 1

若要停止保护并删除备份数据，请执行以下步骤：

1. 打开 DPM 管理员控制台，在导航栏中选择“保护”****。
2. 在显示窗格中，选择要删除的保护组成员。 右键单击并选择“停止对组成员的保护”选项。****
3. 在“停止保护”对话框中选择“删除受保护的数据”，并选中“删除在线存储”复选框。************ 然后选择“停止保护”。****

    ![在“停止保护”窗格中选择“删除受保护的数据”。](./media/backup-azure-delete-vault/delete-storage-online.png)

    受保护成员状态更改为“非活动副本可用”。**

4. 右键单击停用保护组，然后选择“删除停用保护”。****

    ![删除非活动保护。](./media/backup-azure-delete-vault/remove-inactive-protection.png)

5. 在“删除非活动保护”窗口中选中“删除在线存储”复选框，然后选择“确定”。************

    ![删除在线存储。](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

#### <a name="method-2"></a>方法 2

打开 **MABS 管理**控制台。 在“选择数据保护方法”下，清除“我需要在线保护”复选框。********

  ![选择数据保护方法。](./media/backup-azure-delete-vault/data-protection-method.png)

删除本地备份项后，遵循门户中的后续步骤。

## <a name="delete-the-recovery-services-vault"></a>删除恢复服务保管库

1. 删除所有依赖项后，在保管库菜单中滚动到“概要”窗格。****
2. 确认是否未列出任何“备份项”、“备份管理服务器”或“复制的项”。 如果项仍显示在保管库中，请参阅[开始之前](#before-you-start)部分。

3. 当保管库中没有其他任何项时，在保管库仪表板上选择“删除”****。

    ![在保管库仪表板上选择“删除”。](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. 选择“是”确认要删除该保管库****。 随即会删除该保管库。 门户返回到“新建”服务菜单。****

## <a name="delete-the-recovery-services-vault-by-using-powershell"></a>使用 PowerShell 删除恢复服务保管库

首先，请阅读**[开始之前](#before-you-start)** 部分，以了解依赖项和保管库删除过程。

若要停止保护并删除备份数据：

- 如果在 Azure VM 备份中使用 SQL 并为 SQL 实例启用了自动保护，请先禁用自动保护。

    ```PowerShell
        Disable-AzRecoveryServicesBackupAutoProtection
           [-InputItem] <ProtectableItemBase>
           [-BackupManagementType] <BackupManagementType>
           [-WorkloadType] <WorkloadType>
           [-PassThru]
           [-VaultId <String>]
           [-DefaultProfile <IAzureContextContainer>]
           [-WhatIf]
           [-Confirm]
           [<CommonParameters>]
    ```

  [详细了解](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupautoprotection?view=azps-2.6.0)如何对 Azure 备份保护的项禁用保护。

- 停止保护并删除云中所有受备份保护的项的数据（例如 laaS VM、Azure 文件共享等）：

    ```PowerShell
       Disable-AzRecoveryServicesBackupProtection
       [-Item] <ItemBase>
       [-RemoveRecoveryPoints]
       [-Force]
       [-VaultId <String>]
       [-DefaultProfile <IAzureContextContainer>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
    ```

    [详细了解如何](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection?view=azps-2.6.0&viewFallbackFrom=azps-2.5.0) 为受保护的项目禁用保护。

- 对于使用 Azure 备份代理 (MARS) 备份到 Azure 的受保护本地文件和文件夹，请使用以下 PowerShell 命令从每个 MARS PowerShell 模块中删除备份的数据：

    ```powershell
    Get-OBPolicy | Remove-OBPolicy -DeleteBackup -SecurityPIN <Security Pin>
    ```

    然后，将显示以下提示：

    *Microsoft Azure 备份是否确实要删除此备份策略？删除的备份数据将保留14天。之后，备份数据将被永久删除。<br/> [Y] 是 [A] 所有 [N] 否 [L] 所有 [S] 挂起 [？]帮助（默认值为 "Y"）：*

- 对于使用 MABS（Microsoft Azure 备份服务器）或 DPM (System Center Data Protection Manager) 在 Azure 中进行保护的本地计算机，请使用以下命令删除 Azure 中的备份数据。

    ```powershell
    Get-OBPolicy | Remove-OBPolicy -DeleteBackup -SecurityPIN <Security Pin>
    ```

    然后，将显示以下提示：

   Microsoft Azure 备份** 确实要删除此备份策略吗? 删除的备份数据会保留 14 天。 该时间过后，备份数据会被永久删除。 <br/>
   [Y] 是 [A] 全部是 [N] 否 [L] 全部否 [S] 暂停  [?] 帮助 (默认值为 "Y"):*

删除备份的数据后，取消注册所有本地容器和管理服务器。

- 对于使用 Azure 备份代理 (MARS) 备份到 Azure 的受保护本地文件和文件夹：

    ```PowerShell
    Unregister-AzRecoveryServicesBackupContainer
              [-Container] <ContainerBase>
              [-PassThru]
              [-VaultId <String>]
              [-DefaultProfile <IAzureContextContainer>]
              [-WhatIf]
              [-Confirm]
              [<CommonParameters>]
    ```

    [详细了解](https://docs.microsoft.com/powershell/module/az.recoveryservices/unregister-azrecoveryservicesbackupcontainer?view=azps-2.6.0)如何从保管库中取消注册 Windows 服务器或其他容器。

- 对于使用 MABS（Microsoft Azure 备份服务器）或 DPM (System Center Data Protection Manager) 在 Azure 中进行保护的本地计算机：

    ```PowerShell
        Unregister-AzRecoveryServicesBackupManagementServer
          [-AzureRmBackupManagementServer] <BackupEngineBase>
          [-PassThru]
          [-VaultId <String>]
          [-DefaultProfile <IAzureContextContainer>]
          [-WhatIf]
          [-Confirm]
          [<CommonParameters>]
    ```

    [详细了解](https://docs.microsoft.com/powershell/module/az.recoveryservices/unregister-azrecoveryservicesbackupcontainer?view=azps-2.6.0)如何从保管库中取消注册备份管理容器。

永久删除备份的数据并取消注册所有容器后，继续删除保管库。

若要强制删除恢复服务保管库，请执行以下操作：

   ```PowerShell
       Remove-AzRecoveryServicesVault
      -Vault <ARSVault>
      [-DefaultProfile <IAzureContextContainer>]
      [-WhatIf]
      [-Confirm]
      [<CommonParameters>]
   ```

[详细了解](https://docs.microsoft.com/powershell/module/az.recoveryservices/remove-azrecoveryservicesvault)如何删除恢复服务保管库。

## <a name="delete-the-recovery-services-vault-by-using-cli"></a>使用 CLI 删除恢复服务保管库

首先，请阅读**[开始之前](#before-you-start)** 部分，以了解依赖项和保管库删除过程。

> [!NOTE]
> 目前，Azure 备份 CLI 仅支持管理 Azure VM 备份，因此，仅当保管库包含 Azure VM 备份时，以下命令才能删除保管库。 如果保管库包含非 Azure VM 类型的任何备份项，则无法使用 Azure 备份 CLI 删除该保管库。

若要删除现有的恢复服务保管库，请执行以下命令：

- 停止保护并删除备份数据

    ```azurecli
    az backup protection disable --container-name
                             --item-name
                             [--delete-backup-data {false, true}]
                             [--ids]
                             [--resource-group]
                             [--subscription]
                             [--vault-name]
                             [--yes]
    ```

    有关详细信息，请参阅此 [文](/cli/azure/backup/protection#az-backup-protection-disable)。

- 删除现有的恢复服务保管库：

    ```azurecli
    az backup vault delete [--force]
                       [--ids]
                       [--name]
                       [--resource-group]
                       [--subscription]
                       [--yes]
    ```

    有关详细信息，请参阅 [此文](https://docs.microsoft.com/cli/azure/backup/vault?view=azure-cli-latest)

## <a name="delete-the-recovery-services-vault-by-using-azure-resource-manager"></a>使用 Azure 资源管理器删除恢复服务保管库

仅当已删除所有依赖项后，仍然收到“保管库删除错误”时，才建议使用删除恢复服务保管库的选项。** 请尝试以下任意或所有提示：

- 在“概要”窗格中的保管库菜单内，确认是否未列出任何备份项、备份管理服务器或复制的项。**** 如果存在备份项，请参阅[开始之前](#before-you-start)部分。
- 重试[从门户删除保管库](#delete-the-recovery-services-vault)。
- 如果删除了所有依赖项，但仍收到“保管库删除错误”，请使用 ARMClient 工具执行以下步骤（注释后面的步骤）。**

1. 访问 [chocolatey.org](https://chocolatey.org/) 下载并安装 Chocolatey。 然后运行以下命令安装 ARMClient：

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. 登录到 Azure 帐户，然后运行以下命令：

    `ARMClient.exe login [environment name]`

3. 在 Azure 门户中，收集所要删除的保管库的订阅 ID 和资源组名称。

有关 ARMClient 命令的详细信息，请参阅 [ARMClient README](https://github.com/projectkudu/ARMClient/blob/master/README.md)。

### <a name="use-the-azure-resource-manager-client-to-delete-a-recovery-services-vault"></a>使用 Azure 资源管理器客户端删除恢复服务保管库

1. 使用订阅 ID、资源组名称和保管库名称运行以下命令。 如果没有任何依赖项，则在运行以下命令时，将删除该保管库：

   ```azurepowershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```

2. 如果保管库不为空，则将收到以下错误消息：*无法删除保管库，因为此保管库中存在现有资源。* 若要删除保管库中受保护的项或容器，请运行以下命令：

   ```azurepowershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. 在 Azure 门户中，确保已删除该保管库。

## <a name="next-steps"></a>后续步骤

[了解恢复服务保管库](backup-azure-recovery-services-vault-overview.md)<br/>
[了解如何监视和管理恢复服务保管库](backup-azure-manage-windows-server.md)
