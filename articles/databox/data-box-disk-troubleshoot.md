---
title: Azure Data Box 磁盘故障排除 | Microsoft Docs
description: 介绍如何排查 Azure Data Box 磁盘中出现的问题。
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/09/2018
ms.author: alkohli
ms.openlocfilehash: 9b2c03c13cf687af7cdebc9c4d2624a6a7c5907f
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "49069193"
---
# <a name="troubleshoot-issues-in-azure-data-box-disk-preview"></a>排查 Azure Data Box 磁盘（预览版）中的问题

本文适用于运行预览版的 Microsoft Azure Data Box。 本文介绍了可对 Data Box 和 Data Box 磁盘执行的一些复杂工作流与管理任务。 

可以通过 Azure 门户管理 Data Box 磁盘。 本文重点介绍可以使用 Azure 门户执行的任务。 使用 Azure 门户可以管理订单、管理设备，以及跟踪订单在从头到尾的处理过程中的状态。

本文包括以下教程：

- 下载诊断日志
- 查询活动日志


> [!IMPORTANT]
> Data Box 以预览版提供。 在部署此解决方案之前，请查看 [Azure 预览版服务的条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

## <a name="download-diagnostic-logs"></a>下载诊断日志

如果在复制数据的过程中出现任何错误，门户会显示诊断日志所在的文件夹的路径。 

诊断日志可以是：
- 错误日志
- 详细日志  

若要导航到复制日志的路径，请转到与 Data Box 订单关联的存储帐户。 

1.  转到“常规”>“订单详细信息”，记下与订单关联的存储帐户。
 

2.  转到“所有资源”，搜索在上一步骤中识别的存储帐户。 选择并单击该存储帐户。

    ![复制日志 1](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs1.png)

3.  转到“Blob 服务”>“浏览 Blob”，查找对应于该存储帐户的 Blob。 转到“diagnosticslogcontainer”>“waies”。 

    ![复制日志 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

    应会看到数据复制活动的错误日志和详细日志。 选择并单击每个文件，然后下载本地副本。

## <a name="query-activity-logs"></a>查询活动日志

使用活动日志可在故障排除时查找错误，或者监视组织内用户对资源的修改。 通过活动日志，可以确定：

- 对订阅中的资源执行了什么操作。
- 谁启动了该操作。 
- 操作何时发生。
- 操作状态。
- 其他可能有助于调查操作的属性值。

活动日志包含针对资源执行的所有写入操作（例如 PUT、POST、DELETE），但不包含读取操作（例如 GET）。 

活动日志将保留 90 天。 可以查询任何日期范围，只要开始日期不早于过去 90 天。 此外，还可以根据 Insights 中的某个内置查询进行筛选。 例如，单击某个错误，然后选择并单击特定的失败以了解根本原因。

## <a name="data-box-disk-unlock-tool-errors"></a>Data Box 磁盘解锁工具错误


| 错误消息/工具行为      | 建议                                                                                               |
|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| 无<br><br>Data Box 磁盘解锁工具崩溃。                                                                            | 未安装 Bitlocker。 确保运行 Data Box 磁盘解锁工具的主机上已安装 BitLocker。                                                                            |
| 当前的 .NET Framework 不受支持。 支持的版本为 4.5 和以上。<br><br>工具退出并显示一条消息。  | 未安装 .NET 4.5。 在运行 Data Box 磁盘解锁工具的主机上安装 .NET 4.5 或更高版本。                                                                            |
| 无法解锁或验证任何卷。 请联系 Microsoft 支持。  <br><br>工具无法解锁或验证任何已锁定的驱动器。 | 工具无法解锁任何已使用提供的支持密钥锁定的驱动器。 联系 Microsoft 支持部门了解后续步骤。                                                |
| 已解锁并验证以下卷。 <br>卷驱动器号: E:<br>无法使用以下支持密钥解锁任何卷: werwerqomnf、qwerwerqwdfda <br><br>工具解锁了一些驱动器，并列出了成功和失败的驱动器号。| 部分成功。 使用提供的支持密钥无法解锁某些驱动器。 联系 Microsoft 支持部门了解后续步骤。 |
| 找不到锁定的卷。 验证从 Microsoft 收到的磁盘是否已正确连接并处于锁定状态。          | 工具找不到任何已锁定的驱动器。 驱动器已解锁，或检测不到。 确保驱动器已连接并已锁定。                                                           |
| 严重错误: 参数无效<br>参数名称: invalid_arg<br>用法:<br>DataBoxDiskUnlock /PassKeys:<passkey_list_separated_by_semicolon><br><br>示例: DataBoxDiskUnlock /PassKeys:passkey1;passkey2;passkey3<br>示例: DataBoxDiskUnlock /SystemCheck<br>示例: DataBoxDiskUnlock /Help<br><br>/PassKeys:       从 Azure DataBox 磁盘订单中获取此支持密钥。 该支持密钥用于解锁磁盘。<br>/Help:           此选项提供有关 cmdlet 用法和示例的帮助。<br>/SystemCheck:    此选项检查你的系统是否满足运行该工具的要求。<br><br>按任意键退出。 | 输入了无效的参数。 只允许参数 /SystemCheck、/PassKey 和 /Help。                                                                            |

## <a name="data-box-disk-split-copy-tool-errors"></a>Data Box 磁盘拆分复制工具错误

|错误消息/警告  |建议 |
|---------|---------|
|[信息] 正在检索卷 m: 的 bitlocker 密码 <br>[错误] 检索卷 m: 的 bitlocker 密钥时捕获到异常<br> 序列未包含任何元素。|如果目标 Data Box 磁盘处于脱机状态，则会引发此错误。 <br> 使用 `diskmgmt.msc` 工具将磁盘联机。|
|[错误] 引发异常：WMI 操作失败：<br> Method=UnlockWithNumericalPassword，ReturnValue=2150694965， <br>Win32Message=所提供的恢复密码的格式无效。 <br>BitLocker 恢复密码有 48 位。 <br>请验证恢复密码的格式是否正确，然后重试。|使用 Data Box 磁盘解锁工具首先解锁磁盘，然后重试该命令。 有关详细信息，请转到 <li> [为 Windows 客户端解锁 Data Box 磁盘。](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client) </li><li> [为 Linux 客户端解锁 Data Box 磁盘。](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client) </li>|
|[错误] 引发了异常：目标驱动器上存在 DriveManifest.xml 文件。 <br> 这表明可能已使用不同的日志文件准备了目标驱动器。 <br>若要向同一驱动器添加更多数据，请使用之前的日志文件。 若要删除现有数据并将目标驱动器重复用于新的导入作业，请删除驱动器上的 DriveManifest.xml。 使用新的日志文件重新运行此命令。| 当尝试将同一组驱动器用于多个导入会话时会收到此错误。 <br> 将一组驱动器仅用于一个拆分和复制会话。|
|[错误] 引发了异常：CopySessionId importdata-sept-test-1 引用了以前的复制会话，无法将其重复用于新的复制会话。|当尝试为新作业使用与以前成功完成的作业相同的名称时，会报告此错误。<br> 为新作业分配唯一的名称。|
|[信息] 目标文件或目录名称超出了 NTFS 长度限制。 |当目标文件因为文件路径太长而被重命名时，会报告此消息。<br> 修改 `config.json` 文件中的 disposition 选项来控制此行为。|
|[错误] 引发了异常：JSON 转义序列不正确。 |当 Config.json 具有无效格式时，会报告此消息。 <br> 在保存文件之前使用 [JSONlint](https://jsonlint.com/) 验证 `config.json`。|



## <a name="next-steps"></a>后续步骤

- 了解如何[通过 Azure 门户管理 Data Box 磁盘](data-box-portal-ui-admin.md)。
