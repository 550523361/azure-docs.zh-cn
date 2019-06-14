---
title: Azure Active Directory 域服务：将 RHEL VM 加入托管域 | Microsoft Docs
description: 将 Red Hat Enterprise Linux 虚拟机加入 Azure AD 域服务
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: d76ae997-2279-46dd-bfc5-c0ee29718096
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mstephen
ms.openlocfilehash: 5231968f0818d26bb313bd62f45cb5ad2a86b0bb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "66245979"
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>将 Red Hat Enterprise Linux 7 虚拟机加入托管域
本文说明如何将 Red Hat Enterprise Linux (RHEL) 7 虚拟机加入 Azure AD 域服务托管域。

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>开始之前
若要执行本文中所列的任务，需要：  
1. 一个有效的 **Azure 订阅**。
2. 一个 **Azure AD 目录** - 已与本地目录或仅限云的目录同步。
3. 必须为 Azure AD 目录启用 **Azure AD 域服务**。 如果未启用，请遵循[入门指南](create-instance.md)中所述的所有任务。
4. 请确保已将托管域的 IP 地址配置为虚拟网络的 DNS 服务器。 有关详细信息，请参阅[如何更新 Azure 虚拟网络的 DNS 设置](active-directory-ds-getting-started-dns.md)
5. 完成[将密码同步到 Azure AD 域服务托管域](active-directory-ds-getting-started-password-sync.md)所要执行的步骤。


## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>预配 Red Hat Enterprise Linux 虚拟机
使用以下任一方法在 Azure 中预配 RHEL 7 虚拟机：
* [Azure 门户](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * 将虚拟机部署到**已在其中启用了 Azure AD 域服务的同一个虚拟网络**。
> * 选取与已启用 Azure AD 域服务的子网**不同的子网**。
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>远程连接到新预配的 Linux 虚拟机
RHEL 7.2 虚拟机已在 Azure 中预配。 下一个任务是使用在预配 VM 时创建的本地管理员帐户远程连接到虚拟机。

请遵循[如何登录到运行 Linux 的虚拟机](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)一文中的说明。


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>配置 Linux 虚拟机上的 hosts 文件
在 SSH 终端中编辑 /etc/hosts 文件，并更新计算机的 IP 地址和主机名。

```
sudo vi /etc/hosts
```

在 hosts 文件中输入以下值：

```
127.0.0.1 contoso-rhel.contoso100.com contoso-rhel
```
在这里，“contoso100.com”是托管域的 DNS 域名。 “contoso-rhel”是要加入到托管域的 RHEL 虚拟机的主机名。


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>在 Linux 虚拟机上安装所需的包
接下来，在虚拟机上安装加入域所需的包。 在 SSH 终端中，键入以下命令以安装所需的包：

    ```
    sudo yum install realmd sssd krb5-workstation krb5-libs samba-common-tools
    ```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>将 Linux 虚拟机加入托管域
在 Linux 虚拟机上安装所需的包后，下一个任务是将虚拟机加入托管域。

1. 发现 AAD 域服务托管域。 在 SSH 终端中键入以下命令：

    ```
    sudo realm discover CONTOSO100.COM
    ```

   > [!NOTE]
   > **故障排除：** 如果“领域发现”找不到托管域： 
   >   * 确保域可从虚拟机（请尝试 ping）进行访问。
   >   * 检查虚拟机是否已确实部署到提供托管域的同一个虚拟网络。
   >   * 检查是否已将虚拟网络的 DNS 服务器设置更新为指向托管域的域控制器。

2. 初始化 Kerberos。 在 SSH 终端中键入以下命令：

    > [!TIP]
    > * 请确保指定属于“AAD DC 管理员”组的用户。
    > * 以大写字母指定域名，否则 kinit 会失败。
    >

    ```
    kinit bob@CONTOSO100.COM
    ```

3. 将计算机加入域。 在 SSH 终端中键入以下命令：

    > [!TIP]
    > 使用在前一步骤中指定的同一用户帐户（“kinit”）。
    >

    ```
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'
    ```

计算机成功加入托管域后，应会显示一条消息（“已在领域中成功注册计算机”）。


## <a name="verify-domain-join"></a>验证域加入
验证计算机是否已成功加入托管域。 使用不同 SSH 连接连接到已加入域的 RHEL VM。 使用域用户帐户连接，并检查该用户帐户是否正确解析。

1. 在 SSH 终端中键入以下命令，使用 SSH 连接到已加入域的 RHEL 虚拟机。 使用属于托管域的域帐户（例如，在本例中为“bob@CONTOSO100.COM”）。
    ```
    ssh -l bob@CONTOSO100.COM contoso-rhel.contoso100.com
    ```

2. 在 SSH 终端中键入以下命令，查看是否已正确初始化主目录。
    ```
    pwd
    ```

3. 在 SSH 终端中键入以下命令，查看组成员身份是否正确解析。
    ```
    id
    ```


## <a name="troubleshooting-domain-join"></a>排查域加入问题
请参阅[排查域加入问题](join-windows-vm.md#troubleshoot-joining-a-domain)一文。

## <a name="related-content"></a>相关内容
* [Azure AD 域服务 - 入门指南](create-instance.md)
* [将 Windows Server 虚拟机加入 Azure AD 域服务托管域](active-directory-ds-admin-guide-join-windows-vm.md)
* [如何登录到运行 Linux 的虚拟机](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。
* [安装 Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7 - Windows 集成指南](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
