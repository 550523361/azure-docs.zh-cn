---
title: 专用链接 - Azure 门户 - MariaDB 的 Azure 数据库
description: 了解如何从 Azure 门户为 MariaDB 配置 Azure 数据库的专用链接
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 3f421cad64caf91b898bb1ec13dc909b93b7f72d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370332"
---
# <a name="create-and-manage-private-link-for-azure-database-for-mariadb-using-portal"></a>使用门户为 MariaDB 创建和管理 Azure 数据库的专用链接

专用终结点是 Azure 中专用链接的构建基块。 它使 Azure 资源（例如虚拟机 (VM)）能够以私密方式来与专用链接资源通信。  在本文中，您将学习如何使用 Azure 门户在 Azure 虚拟网络中创建 VM，以及使用 Azure 专用终结点为 MariaDB 服务器创建 Azure 数据库。

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

> [!NOTE]
> 此功能在所有 Azure 区域都可用，其中 MariaDB 的 Azure 数据库支持通用和内存优化定价层。

## <a name="sign-in-to-azure"></a>登录 Azure
登录到 Azure[门户](https://portal.azure.com)。

## <a name="create-an-azure-vm"></a>创建 Azure VM

在本节中，您将创建虚拟网络和子网来承载用于访问专用链接资源的 VM（Azure 中的 MariaDB 服务器）。

### <a name="create-the-virtual-network"></a>创建虚拟网络
在本部分，你将创建虚拟网络和子网来托管用于访问专用链接资源的 VM。

1. 在屏幕的左上角，选择 **"创建资源** > **网络** > **虚拟网络**"。
2. 在“创建虚拟网络”**** 中，输入或选择以下信息：

    | 设置 | “值” |
    | ------- | ----- |
    | “属性” | 输入 *MyVirtualNetwork*。 |
    | 地址空间 | 输入 10.1.0.0/16**。 |
    | 订阅 | 选择订阅。|
    | 资源组 | 选择“新建”，输入 myResourceGroup，然后选择“确定”**********。 |
    | 位置 | 选择“西欧”****。|
    | 子网 - 名称 | 输入 *mySubnet*。 |
    | 子网 - 地址范围 | 输入 10.1.0.0/24**。 |
    |||
3. 将其余的设置保留默认值，然后选择“创建”****。

### <a name="create-virtual-machine"></a>创建虚拟机

1. 在 Azure 门户中屏幕的左上角，选择 **"创建资源** > **计算** > **虚拟机**"。

2. 在“创建虚拟机 - 基本信息”**** 中，输入或选择以下信息：

    | 设置 | “值” |
    | ------- | ----- |
    | **项目详情** | |
    | 订阅 | 选择订阅。 |
    | 资源组 | 选择**我的资源组**。 已在上一部分创建此内容。  |
    | **实例详细信息** |  |
    | 虚拟机名称 | 输入 *myVm*。 |
    | 区域 | 选择“西欧”****。 |
    | 可用性选项 | 保留默认值“不需要基础结构冗余”****。 |
    | 图像 | 选择“Windows Server 2019 Datacenter”。**** |
    | 大小 | 保留默认值“标准 DS1 v2”****。 |
    | **管理员帐户** |  |
    | 用户名 | 输入所选用户名。 |
    | 密码 | 输入所选密码。 密码必须至少 12 个字符长，且符合[定义的复杂性要求](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm)。|
    | 确认密码 | 重新输入密码。 |
    | **入站端口规则** |  |
    | 公共入站端口 | 保留默认值“无”****。 |
    | **节省资金** |  |
    | 已有 Windows 许可证？ | 保留默认值“否”****。 |
    |||

1. 选择 **"下一步"：磁盘**。

1. 在 **"创建虚拟机磁盘"中**，保留默认值并选择 **"下一步：网络**"。

1. 在“创建虚拟机 - 基本信息”**** 中，选择以下信息：

    | 设置 | “值” |
    | ------- | ----- |
    | 虚拟网络 | 保留默认值“MyVirtualNetwork”****。  |
    | 地址空间 | 保留默认值“10.1.0.0/24”。****|
    | 子网 | 保留默认值“mySubnet (10.1.0.0/24)”。****|
    | 公共 IP | 保留默认值“(new) myVm-ip”****。 |
    | 公共入站端口 | 选择“允许所选端口”****。 |
    | 选择入站端口 | 选择“HTTP”和“RDP”。********|
    |||


1. 选择“查看 + 创建”****。 随后你会转到“查看 + 创建”页，Azure 将在此页面验证配置****。

1. 当您看到**验证传递**的消息时，选择 **"创建**"。

## <a name="create-an-azure-database-for-mariadb"></a>创建 Azure Database for MariaDB

在本节中，您将在 Azure 中为 MariaDB 服务器创建 Azure 数据库。 

1. 在 Azure 门户中屏幕的左上角，选择为**MariaDB****创建资源** > **数据库** > Azure 数据库 。

1. 在**MariaDB 的 Azure 数据库中**提供了以下信息：

    | 设置 | “值” |
    | ------- | ----- |
    | **项目详情** | |
    | 订阅 | 选择订阅。 |
    | 资源组 | 选择**我的资源组**。 已在上一部分创建此内容。|
    | **服务器详细信息** |  |
    |服务器名称  | 输入 *myserver*。 如果此名称已被使用，请创建唯一的名称。|
    | 管理员用户名| 输入所选的管理员名称。 |
    | 密码 | 输入所选密码。 密码长度必须至少为 8 个字符，且符合定义的要求。 |
    | 位置 | 选择要希望 MariaDB 服务器驻留的 Azure 区域。 |
    |版本  | 选择所需的 MariaDB 服务器的数据库版本。|
    | 计算 + 存储| 根据工作负荷选择服务器所需的定价层。 |
    |||
 
7. 选择“确定”。 
8. 选择“查看 + 创建”****。 随后你会转到“查看 + 创建”页，Azure 将在此页面验证配置****。 
9. 当您看到验证传递的消息时，选择 **"创建**"。 
10. 看到“验证通过”消息时选择“创建”。 

## <a name="create-a-private-endpoint"></a>创建专用终结点

在本节中，您将创建到 MariaDB 服务器的专用终结点。 

1. 在 Azure 门户中屏幕的左上角，选择 **"创建资源** > **网络** > **专用链接**"。
2. 在“专用链接中心 - 概述”中的“与服务建立专用连接”选项的旁边，选择“启动”。************

    ![专用链接概述](media/concepts-data-access-and-security-private-link/privatelink-overview.png)

1. 在 **"创建专用终结点 - 基础知识"中**，输入或选择此信息：

    | 设置 | “值” |
    | ------- | ----- |
    | **项目详情** | |
    | 订阅 | 选择订阅。 |
    | 资源组 | 选择**我的资源组**。 已在上一部分创建此内容。|
    | **实例详细信息** |  |
    | “属性” | 输入“myPrivateEndpoint”**。 如果此名称已被使用，请创建唯一的名称。 |
    |区域|选择“西欧”****。|
    |||
5. 选择 **"下一步"：资源**。
6. 在“创建专用终结点 - 资源”中，输入或选择以下信息：****

    | 设置 | “值” |
    | ------- | ----- |
    |连接方法  | 选择“连接到我的目录中的 Azure 资源”。|
    | 订阅| 选择订阅。 |
    | 资源类型 | 选择**微软.DBforMariaDB/服务器**。 |
    | 资源 |选择“myServer”**|
    |目标子资源 |选择*玛丽亚德银行服务器*|
    |||
7. 选择 **"下一步"：配置**。
8. 在 **"创建专用终结点 - 配置**"中，输入或选择此信息：

    | 设置 | “值” |
    | ------- | ----- |
    |**网络**| |
    | 虚拟网络| 选择*我的虚拟网络*。 |
    | 子网 | 选择“mySubnet”。 ** |
    |**专用 DNS 集成**||
    |与专用 DNS 区域集成 |选择 **“是”**。 |
    |专用 DNS 区域 |选择 *（新）私人链接.mariadb.数据库.azure.com* |
    |||

1. 选择“查看 + 创建”****。 随后你会转到“查看 + 创建”页，Azure 将在此页面验证配置****。 
2. 当您看到**验证传递**的消息时，选择 **"创建**"。 

    ![已创建专用链接](media/concepts-data-access-and-security-private-link/show-mariadb-private-link.png)

    > [!NOTE] 
    > 客户 DNS 设置中的 FQDN 不会解析为配置的专用 IP。 您必须为配置的 FQDN 设置 DNS 区域，[如下所示](../dns/dns-operations-recordsets-portal.md)。

## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>使用远程桌面 (RDP) 连接到 VM


创建 **myVm** 后，按如下所述从 Internet 连接到该 VM： 

1. 在门户的搜索栏中，输入 *myVm*。

1. 选择“连接”按钮。**** 选择“连接”按钮后，“连接到虚拟机”随即打开********。

1. 选择**下载 RDP 文件**。 Azure 会创建远程桌面协议 (*.rdp*) 文件，并将其下载到计算机。

1. 打开 downloaded.rdp** 文件。

    1. 出现提示时，选择“连接”****。

    1. 输入在创建 VM 时指定的用户名和密码。

        > [!NOTE]
        > 您可能需要选择**更多选项** > **"使用其他帐户**"来指定创建 VM 时输入的凭据。

1. 选择“确定”。

1. 你可能会在登录过程中收到证书警告。 如果收到证书警告，请选择“确定”或“继续”********。

1. VM 桌面出现后，将其最小化以返回到本地桌面。

## <a name="access-the-mariadb-server-privately-from-the-vm"></a>从 VM 私下访问 MariaDB 服务器

1. 在  *myVM* 的远程桌面中打开 PowerShell。

2. 输入  `nslookup mydemomserver.privatelink.mariadb.database.azure.com`。 

    将收到类似于下面的消息：
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mydemoMariaDBserver.privatelink.mariadb.database.azure.com
    Address:  10.1.3.4
    ```

3. 使用任何可用客户端测试 MariaDB 服务器的专用链路连接。 在下面的示例中，我使用[MySQL 工作台](https://dev.mysql.com/doc/workbench/en/wb-installing-windows.html)执行操作。


4. 在 **"新建连接**"中，输入或选择此信息：

    | 设置 | “值” |
    | ------- | ----- |
    | 服务器类型| 选择**MariaDB**。|
    | 服务器名称| 选择*mydemoserver.privatelink.mariadb.database.azure.com* |
    | 用户名 | 输入在username@servernameMariaDB 服务器创建期间提供的用户名。 |
    |密码 |输入在 MariaDB 服务器创建期间提供的密码。 |
    |SSL|选择 **"必需**"。|
    ||

5. 选择 **"测试连接**"**或"确定**"。

6. （可选）从左侧菜单浏览数据库，并从 MariaDB 数据库中创建或查询信息

7. 关闭远程桌面连接到 myVm。

## <a name="clean-up-resources"></a>清理资源
使用专用终结点 MariaDB 服务器和 VM 完成后，请删除资源组及其包含的所有资源：

1. 在门户顶部的 **"搜索"** 框中输入 *"我的资源组* "，然后从搜索结果中选择 *"我的资源组* "。
2. 选择“删除资源组”****。
3. 输入"资源组 **"以键入资源组名称**，然后选择 **"删除**"。

## <a name="next-steps"></a>后续步骤

在此执行中，您可以在虚拟网络上创建了 VM、MariaDB 的 Azure 数据库以及用于专用访问的专用终结点。 您从 Internet 连接到一个 VM，并使用专用链路安全地连接到 MariaDB 服务器。 要了解有关私有终结点的更多内容，请参阅[什么是 Azure 专用终结点](https://docs.microsoft.com/azure/private-link/private-endpoint-overview)。
