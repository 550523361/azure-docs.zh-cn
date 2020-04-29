---
title: Azure 中的 SQL Server 的安全注意事项 | Microsoft Docs
description: 本主题提供有关保护 Azure 虚拟机中运行的 SQL Server 的常规指南。
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: d710c296-e490-43e7-8ca9-8932586b71da
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/23/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: f5ea0ddff38532b119d8d984f2dabd6d898b44a5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "77031350"
---
# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Azure 虚拟机中 SQL Server 的安全注意事项

本主题包括总体安全指南，可帮助建立对 Azure 虚拟机 (VM) 中 SQL Server 实例的安全访问。

Azure 遵守多个行业法规和标准，使用户能够使用虚拟机中运行的 SQL Server 生成符合规定的解决方案。 有关 Azure 合规性的信息，请参阅 [Azure 信任中心](https://azure.microsoft.com/support/trust-center/)。

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="control-access-to-the-sql-vm"></a>控制对 SQL VM 的访问

创建 SQL Server 虚拟机时，请考虑如何谨慎控制有权访问虚拟机和 SQL Server 的人员。 通常，应执行以下操作：

- 仅限对此有需要的应用程序和客户端拥有 SQL Server 访问权限。
- 遵循管理用户帐户和密码的最佳做法。

在考虑这些要点时，以下部分提供了相关建议。

## <a name="secure-connections"></a>安全连接

创建具有库映像的 SQL Server 虚拟机时，“SQL Server 连接”  选项提供以下选择：“本地（VM 内）”  、“专用（虚拟网络内）”  或“公共 (Internet)”  。

![SQL Server 连接](./media/virtual-machines-windows-sql-security/sql-vm-connectivity-option.png)

为了达到最佳安全性，请为方案选择最严格的选项。 例如，如果正在运行访问同一 VM 上的 SQL Server 的应用程序，则“本地”  选项最安全。 如果正在运行需要访问 SQL Server 的 Azure 应用程序，选择“专用”  选项可确保与 SQL Server 的通信仅在指定的 [Azure 虚拟网络](../../../virtual-network/virtual-networks-overview.md)内安全进行。 如果需要使用“公共(Internet)”选项访问 SQL Server VM，请确保遵照本主题中的其他最佳做法，以减小受攻击面  。

在门户中选择的选项使用 VM [网络安全组](../../../virtual-network/security-overview.md) (NSG) 上的入站安全规则来允许或拒绝发往虚拟机的网络流量。 可修改或创建新的入站 NSG 规则，允许到 SQL Server 端口（默认为 1433）的流量。 还可指定允许通过此端口进行通信的特定 IP 地址。

![网络安全组规则](./media/virtual-machines-windows-sql-security/sql-vm-network-security-group-rules.png)

除了使用 NSG 规则限制网络流量外，还可以对虚拟机使用 Windows 防火墙。

如果通过经典部署模型使用终结点，不使用它们时，请删除虚拟机上的所有终结点。 有关在终结点上使用 ACL 的说明，请参阅[管理终结点上的 ACL](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#manage-the-acl-on-an-endpoint)。 使用 Resource Manager 的 VM 并不需要 ACL。

最后，请考虑针对 Azure 虚拟机中的 SQL Server 数据库引擎实例启用加密连接。 使用签名证书配置 SQL Server 实例。 有关详细信息，请参阅[为数据库引擎启用加密连接](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine)和[连接字符串语法](https://msdn.microsoft.com/library/ms254500.aspx)。

## <a name="encryption"></a>加密

托管磁盘提供服务器端加密和 Azure 磁盘加密。 [服务器端加密](/azure/virtual-machines/windows/disk-encryption)提供静态加密并保护数据，以满足组织的安全性和符合性承诺。 [Azure 磁盘加密](/azure/security/fundamentals/azure-disk-encryption-vms-vmss)使用 BITLOCKER 或 DM dm-crypt 技术，并与 Azure Key Vault 集成，以对 OS 和数据磁盘进行加密。 

## <a name="use-a-non-default-port"></a>使用非默认端口

SQL Server 默认在已知端口 1433 上侦听。 为了提高安全性，请将 SQL Server 配置为在非默认端口（例如 1401）上侦听。 如果在 Azure 门户中预配 SQL Server 库映像，可以在“SQL Server 设置”边栏选项卡中指定此端口。****

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

若要在预配后配置此端口，可以使用两个选项：

- 对于资源管理器 Vm，你可以从[SQL 虚拟机资源](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource)中选择 "**安全性**"。 这会提供一个用于更改端口的选项。

  ![在门户中更改 TCP 端口](./media/virtual-machines-windows-sql-security/sql-vm-change-tcp-port.png)

- 对于经典 VM 或者未使用门户预配的 SQL Server VM，可以通过远程连接到 VM 来手动配置端口。 有关配置步骤，请参阅[将服务器配置为在特定的 TCP 端口上侦听](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port)。 如果使用这种手动方法，则还需要添加一个 Windows 防火墙规则来允许该 TCP 端口上的传入流量。

> [!IMPORTANT]
> 如果 SQL Server 端口向公共 Internet 连接开放，则指定非默认端口是一种很好的做法。

当 SQL Server 在非默认端口上侦听时，必须在连接时指定该端口。 例如，假设存在这种情况：服务器 IP 地址为 13.55.255.255，SQL Server 在端口 1401 上侦听。 若要连接到 SQL Server，应在连接字符串中指定 `13.55.255.255,1401`。

## <a name="manage-accounts"></a>管理帐户

我们都不希望攻击者能够轻松猜出帐户名或密码。 以下提示可以提供帮助：

- 创建一个未命名为 **Administrator** 的唯一本地管理员帐户。

- 对所有帐户使用复杂的强密码。 有关如何创建强密码的详细信息，请参阅[创建强密码](https://support.microsoft.com/instantanswers/9bd5223b-efbe-aa95-b15a-2fb37bef637d/create-a-strong-password)一文。

- 默认情况下，Azure 在 SQL Server 虚拟机安装期间选择 Windows 身份验证。 因此，禁用 **SA** 登录名，并由安装程序分配密码。 我们不建议使用或启用 **SA** 登录名。 如果必须使用 SQL 登录名，请使用以下策略之一：

  - 创建一个具有 **sysadmin** 成员身份且名称唯一的 SQL 帐户。 在门户中预配期间启用“SQL 身份验证”即可实现此目的。****

    > [!TIP] 
    > 如果在预配期间未启用“SQL 身份验证”，则必须手动将身份验证模式更改为“SQL Server 和 Windows 身份验证模式”。**** 有关详细信息，请参阅[更改服务器身份验证模式](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode)。

  - 如果必须使用 **SA** 登录名，请在预配后启用该登录名，并分配一个新的强密码。

## <a name="additional-best-practices"></a>其他最佳做法

除了本主题中所述的做法之外，我们还建议你查看并实施传统的本地安全做法以及虚拟机安全最佳做法中的最佳安全方案。 

有关本地安全做法的详细信息，请参阅 SQL Server 安装和[安全中心](/sql/relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database)的[安全注意事项](/sql/sql-server/install/security-considerations-for-a-sql-server-installation)。 

有关虚拟机安全的详细信息，请参阅[虚拟机安全概述](/azure/security/fundamentals/virtual-machines-overview)。


## <a name="next-steps"></a>后续步骤

如果还对性能最佳实践感兴趣，请参阅 [Azure 虚拟机中 SQL Server 的性能最佳实践](virtual-machines-windows-sql-performance.md)。

有关其他与在 Azure VM 中运行 SQL Server 相关的主题，请参阅 [Azure 虚拟机上的 SQL Server 概述](virtual-machines-windows-sql-server-iaas-overview.md)。 如果对 SQL Server 虚拟机有任何疑问，请参阅[常见问题解答](virtual-machines-windows-sql-server-iaas-faq.md)。

