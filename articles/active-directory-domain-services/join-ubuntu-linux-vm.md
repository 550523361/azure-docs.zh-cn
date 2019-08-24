---
title: Azure Active Directory 域服务：将 Ubuntu VM 加入托管域 | Microsoft Docs
description: 将 Ubuntu Linux 虚拟机加入 Azure AD 域服务
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 804438c4-51a1-497d-8ccc-5be775980203
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: 80dbb4f3d0c8b993beab5f6344d6034d6c2b6895
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69990576"
---
# <a name="join-an-ubuntu-virtual-machine-in-azure-to-a-managed-domain"></a>将 Azure 中的 Ubuntu 虚拟机加入托管域
本文介绍如何将 Ubuntu Linux 虚拟机加入 Azure AD 域服务托管域。

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>开始之前
若要执行本文中所列的任务，需要：  
1. 一个有效的 **Azure 订阅**。
2. 一个 **Azure AD 目录** - 已与本地目录或仅限云的目录同步。
3. 必须为 Azure AD 目录启用 **Azure AD 域服务**。 如果未启用，请遵循[入门指南](tutorial-create-instance.md)中所述的所有任务。
4. 请确保已将托管域的 IP 地址配置为虚拟网络的 DNS 服务器。 有关详细信息，请参阅[如何更新 Azure 虚拟网络的 DNS 设置](tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network)
5. 完成[将密码同步到 Azure AD 域服务托管域](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)所要执行的步骤。


## <a name="provision-an-ubuntu-linux-virtual-machine"></a>预配 Ubuntu Linux 虚拟机
使用以下任一方法在 Azure 中预配 Ubuntu Linux 虚拟机：
* [Azure 门户](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * 将虚拟机部署到**已在其中启用了 Azure AD 域服务的同一个虚拟网络**。
> * 选取与已启用 Azure AD 域服务的子网**不同的子网**。
>


## <a name="connect-remotely-to-the-ubuntu-linux-virtual-machine"></a>远程连接到 Ubuntu Linux 虚拟机
Ubuntu 虚拟机已在 Azure 中预配。 下一个任务是使用在预配 VM 时创建的本地管理员帐户远程连接到虚拟机。

按照[如何登录到运行 Linux 的虚拟机一](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)文中的说明进行操作。


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>配置 Linux 虚拟机上的 hosts 文件
在 SSH 终端中编辑 /etc/hosts 文件，并更新计算机的 IP 地址和主机名。

```console
sudo vi /etc/hosts
```

在 hosts 文件中输入以下值：

```console
127.0.0.1 contoso-ubuntu.contoso.com contoso-ubuntu
```

此处, "contoso.com" 是托管域的 DNS 域名。 “contoso-ubuntu”是要加入托管域的 Ubuntu 虚拟机的主机名。


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>在 Linux 虚拟机上安装所需的包
接下来，在虚拟机上安装加入域所需的包。 执行以下步骤：

1.  在 SSH 终端中键入以下命令，从存储库下载包列表。 此命令会更新包列表，以获取有关最新版的包及其依赖项的信息。

    ```console
    sudo apt-get update
    ```

2. 键入以下命令安装所需的包。

    ```console
      sudo apt-get install krb5-user samba sssd sssd-tools libnss-sss libpam-sss ntp ntpdate realmd adcli
    ```

3. 在 Kerberos 安装期间，会出现一个粉红色的屏幕。 安装“krb5-user”包时会出现输入领域名的提示（以全大写的形式输入）。 安装过程会在 /etc/krb5.conf 中写入 [realm] 和 [domain_realm] 节。

    > [!TIP]
    > 如果托管域的名称为 contoso.com, 请输入 CONTOSO.COM 作为领域。 请记住，必须以大写形式指定领域名。


## <a name="configure-the-ntp-network-time-protocol-settings-on-the-linux-virtual-machine"></a>在 Linux 虚拟机上配置的 NTP（网络时间协议）设置
Ubuntu VM 的日期和时间必须与托管域同步。 在 /etc/ntp.conf 文件中添加托管域的 NTP 主机名。

```console
sudo vi /etc/ntp.conf
```

在 ntp.conf 文件中，输入以下值并保存文件：

```console
server contoso.com
```

此处, "contoso.com" 是托管域的 DNS 域名。

现在，请将 Ubuntu VM 的日期和时间与 NTP 服务器同步，然后启动 NTP 服务：

```console
sudo systemctl stop ntp
sudo ntpdate contoso.com
sudo systemctl start ntp
```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>将 Linux 虚拟机加入托管域
在 Linux 虚拟机上安装所需的包后，下一个任务是将虚拟机加入托管域。

1. 发现 AAD 域服务托管域。 在 SSH 终端中键入以下命令：

    ```console
    sudo realm discover CONTOSO.COM
    ```

   > [!NOTE]
   > **故障排除：** 如果“领域发现”找不到托管域：
   >   * 确保域可从虚拟机（请尝试 ping）进行访问。
   >   * 检查虚拟机是否已确实部署到提供托管域的同一个虚拟网络。
   >   * 检查是否已将虚拟网络的 DNS 服务器设置更新为指向托管域的域控制器。

2. 初始化 Kerberos。 在 SSH 终端中键入以下命令：

    > [!TIP]
    > * 请确保指定属于“AAD DC 管理员”组的用户。 如果需要, 请[在 Azure AD 中将用户帐户添加到组中](../active-directory/fundamentals/active-directory-groups-members-azure-portal.md)。
    > * 以大写字母指定域名，否则 kinit 会失败。
    >

    ```console
    kinit bob@CONTOSO.COM
    ```

3. 将计算机加入域。 在 SSH 终端中键入以下命令：

    > [!TIP]
    > 使用在前一步骤中指定的同一用户帐户（“kinit”）。
    >
    > 如果 VM 无法加入域, 请确保 VM 的网络安全组允许 TCP + UDP 端口464上的出站 Kerberos 流量到 Azure AD DS 托管域的虚拟网络子网。

    ```console
    sudo realm join --verbose CONTOSO.COM -U 'bob@CONTOSO.COM' --install=/
    ```

计算机成功加入托管域后，应会显示一条消息（“已在领域中成功注册计算机”）。


## <a name="update-the-sssd-configuration-and-restart-the-service"></a>更新 SSSD 配置并重启服务
1. 在 SSH 终端中键入以下命令。 打开 sssd.conf 文件并进行以下更改
    
    ```console
    sudo vi /etc/sssd/sssd.conf
    ```

2. 注释禁止 **use_fully_qualified_names = True** 行并保存文件。
    
    ```console
    # use_fully_qualified_names = True
    ```

3. 重启 SSSD 服务。
    
    ```console
    sudo service sssd restart
    ```


## <a name="configure-automatic-home-directory-creation"></a>配置自动主目录创建
若要在用户登录后启用主目录的自动创建, 请在 PuTTY 终端中键入以下命令:

```console
sudo vi /etc/pam.d/common-session
```

在此文件中的“session optional pam_sss.so”行下面添加以下行，并保存此文件：

```console
session required pam_mkhomedir.so skel=/etc/skel/ umask=0077
```


## <a name="verify-domain-join"></a>验证域加入
验证计算机是否已成功加入托管域。 使用不同的 SSH 连接连接到已加入域的 Ubuntu VM。 使用域用户帐户连接，并检查该用户帐户是否正确解析。

1. 在 SSH 终端中键入以下命令，使用 SSH 连接到已加入域的 Ubuntu 虚拟机。 使用属于托管域的域帐户（例如，在本例中为“bob@CONTOSO.COM”）。
    
    ```console
    ssh -l bob@CONTOSO.COM contoso-ubuntu.contoso.com
    ```

2. 在 SSH 终端中键入以下命令，查看是否已正确初始化主目录。
    
    ```console
    pwd
    ```

3. 在 SSH 终端中键入以下命令，查看组成员身份是否正确解析。
    
    ```console
    id
    ```


## <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>为“AAD DC 管理员”组授予 sudo 特权
可为“AAD DC 管理员”组的成员授予 Ubuntu VM 上的管理特权。 sudo 文件位于 /etc/sudoers 中。 在 sudoers 中添加的 AD 组成员可以执行 sudo。

1. 在 SSH 终端中, 确保以超级用户权限登录。 可以使用创建 VM 时指定的本地管理员帐户。 请执行以下命令：
    
    ```console
    sudo vi /etc/sudoers
    ```

2. 将以下条目添加到 /etc/sudoers 文件并保存该文件：
    
    ```console
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators ALL=(ALL) NOPASSWD:ALL
    ```

3. 你现在可以作为 "AAD DC 管理员" 组的成员登录, 并且应具有 VM 的管理权限。


## <a name="troubleshooting-domain-join"></a>排查域加入问题
请参阅[排查域加入问题](join-windows-vm.md#troubleshoot-domain-join-issues)一文。


## <a name="related-content"></a>相关内容
* [Azure AD 域服务 - 入门指南](tutorial-create-instance.md)
* [将 Windows Server 虚拟机加入 Azure AD 域服务托管域](active-directory-ds-admin-guide-join-windows-vm.md)
* [如何登录到运行 Linux 的虚拟机](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。
