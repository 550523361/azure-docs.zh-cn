---
title: "使用适用于 Azure 上的 Linux 的 Docker VM 扩展"
description: "介绍 Docker 以及 Azure 虚拟机扩展，并说明如何在 Azure 上，使用 Azure CLI 通过命令行以编程方式创建用作 Docker 主机的虚拟机。"
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: eaff75e3-d929-4931-a4a1-8c377a8e7302
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/29/2016
ms.author: rasquill
ms.openlocfilehash: b276911ecbbf161cb6068c1af7a035850035b98d
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/09/2017
---
# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>从 Azure 命令行界面 (Azure CLI) 使用 Docker VM 扩展
> [!IMPORTANT] 
> Azure 提供两个不同的部署模型用于创建和处理资源：[Resource Manager 和经典模型](../../../resource-manager-deployment-model.md)。 本文介绍如何使用经典部署模型。 Microsoft 建议大多数新部署使用 Resource Manager 模型。 有关将 Docker VM 扩展与 Resource Manager 模型配合使用的信息，请参阅[此处](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。

本主题说明如何通过 Azure CLI 中的服务管理 (asm) 模式，在任何平台上创建包含 Docker VM 扩展的 VM。 [Docker](https://www.docker.com/) 是最常用的虚拟化技术之一，它使用 [Linux 容器](http://en.wikipedia.org/wiki/LXC)而不是虚拟机作为在共享资源上隔离数据和执行计算的方法。 可以在 [Azure Linux 代理](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)中使用 Docker VM 扩展，创建可在 Azure 上为应用程序托管任意数量的容器的 Docker VM。 若要查看容器及其优点的综合讨论，请参阅 [Docker 高级白板](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard)。

## <a name="how-to-use-the-docker-vm-extension-with-azure"></a>如何对 Azure 使用 Docker VM 扩展
若要对 Azure 使用 Docker VM 扩展，必须安装 0.8.6 版本以上的 [Azure 命令行接口 ](https://github.com/Azure/azure-sdk-tools-xplat)(Azure CLI) （截至本文编写时的最新版本为 0.10.0）。 可以在 Mac、Linux 和 Windows 上安装 Azure CLI。

在 Azure 上使用 Docker 的整个过程相当简单：

* 在要从中控制 Azure 的计算机上安装 Azure CLI 及其依赖项（在 Windows 上，这是作为虚拟机运行的 Linux 分发版）
* 使用 Azure CLI Docker 命令在 Azure 中创建 VM Docker 主机
* 使用本地 Docker 命令在 Azure 中的 Docker VM 内管理 Docker 容器。

### <a name="install-the-azure-command-line-interface-azure-cli"></a>安装 Azure 命令行界面 (Azure CLI)
若要安装和配置 Azure CLI，请参阅[如何安装 Azure 命令行接口](../../../cli-install-nodejs.md)。 要确认安装，请在命令提示符下键入 `azure`，片刻之后，就会看到 Azure CLI ASCII 图文，其中列出了可用的基本命令。 如果安装正常，应该可以键入 `azure help vm`，并看到列出的其中一个命令是“docker”。

> [!NOTE]
> Docker 提供适用于 Windows [Docker 计算机](https://docs.docker.com/installation/windows/)的工具，也可以使用这些工具自动创建可以配合用作 docker 主机的 Azure VM 使用的 docker 客户端。
> 
> 

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>将 Azure CLI 连接到 Azure 帐户
在使用 Azure CLI 之前，必须将 Azure 帐户凭据与平台上的 Azure CLI 相关联。 [如何连接到 Azure 订阅](/cli/azure/authenticate-azure-cli)部分说明了如何下载和导入 **.publishsettings** 文件，或者将 Azure CLI 与组织 ID 相关联。

> [!NOTE]
> 由于使用一个或其他多个身份验证方法的行为有一些差异，因此请务必阅读上述文档，以了解不同的功能。
> 
> 

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>安装 Docker 并对 Azure 使用 Docker VM 扩展
遵循 [Docker 安装说明](https://docs.docker.com/installation/#installation)在计算机本地安装 Docker。

要将 Docker 与 Azure 虚拟机配合使用，VM 使用的 Linux 映像上必须已安装 [Azure Linux VM 代理](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。 目前，只有两种类型的映像提供此代理：

* Azure 映像库中的 Ubuntu 映像，或
* 使用已安装并配置的 Azure Linux VM 代理创建的自定义 Linux 映像。 有关如何使用 Azure VM 代理构建自定义 Linux VM 的详细信息，请参阅 [Azure Linux VM 代理](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。

### <a name="using-the-azure-image-gallery"></a>使用 Azure 映像库
从 Bash 或终端会话键入以下 Azure CLI 命令找到 VM 映像库中要使用的最新 Ubuntu 映像

`azure vm image list | grep Ubuntu-14_04`

选择其中一个映像名称（例如 `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`），并使用以下命令创建使用该映像的新 VM。

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

其中：

* &lt;vm-cloudservice 名称&gt;是将成为 Azure 中 Docker 容器主计算机的 VM 的名称
* &lt;用户名&gt;是 VM 的默认根用户的用户名
* &lt;密码&gt;是符合 Azure 的复杂性标准的用户名帐户的密码

> [!NOTE]
> 目前，密码必须至少有 8 个字符，包含一个小写字母和一个大写字母、一个数字以及下列特殊字符之一：`!@#$%^&+=`。 不，上一个句子末尾的句号并不是特殊字符。
> 
> 

如果命令成功，根据使用的确切参数和选项，应会可以看到如下所示的内容：

![](media/cli-use-docker/dockercreateresults.png)

> [!NOTE]
> 创建虚拟机可能需要几分钟的时间，但当其设置完成后（状态值为 `ReadyRole`），Docker 守护程序（Docker 服务）便会启动，可以连接到 Docker 容器主机。
> 
> 

要测试你在 Azure 中创建的 Docker VM，请键入

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

其中，*&lt;vm-name-you-used&gt;* 是在 `azure vm docker create` 调用中使用的虚拟机名称。 应会看到如下所示的内容，这表示 Docker 主机 VM 已在 Azure 中启动和运行，并且正等待命令。 

现在，可以尝试使用 Docker 客户端进行连接以获取信息（在某些 Docker 客户端设置（如 Mac 上的 Docker 客户端设置）中可能需要使用 `sudo`）：

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

为了确认它一直在工作，可以检查 Docker 扩展的 VM：

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Docker 主机 VM 身份验证
除了创建 Docker VM 以外，`azure vm docker create` 命令还会自动创建所需的证书，使 Docker 客户端计算机能够使用 HTTPS 连接到 Azure 容器主机；这些证书将适当地存储在客户端和主计算机上。 在后续尝试时，现有证书会被重复使用并与新主机共享。

默认情况下，证书放置在 `~/.docker` 中，而 Docker 将配置为在端口 **2376** 上运行。 如果要使用不同的端口或目录，则可以使用下列其中一个 `azure vm docker create` 命令行选项将 Docker 容器主机 VM 配置为使用不同端口或不同证书来连接客户端：

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

主机上的 Docker 守护程序配置为使用由 `azure vm docker create` 命令生成的证书侦听和验证指定端口上的客户端连接。 客户端计算机必须使用这些证书来获取对 Docker 主机的访问权限。

> [!NOTE]
> 不使用这些证书运行的联网主机很容易受到任何可连接到该计算机的用户的攻击。 在修改默认配置之前，请确保了解这样做会对计算机和应用程序造成的风险。
> 
> 

## <a name="next-steps"></a>后续步骤
* 现在，可以转到 [Docker 用户指南]，开始使用该 Docker VM。 若要在新门户中创建启用 Docker 的 VM，请参阅[如何在门户中使用 Docker VM 扩展]。
* Azure Docker VM 扩展还支持 Docker 撰写，它使用声明性 YAML 文件在任何环境中获取由开发人员建模的应用程序，并生成一致的部署。 请参阅[开始使用 Docker 和 Compose，在 Azure 虚拟机上定义和运行多容器应用程序]。  

<!--Anchors-->
[Subheading 1]:#subheading-1
[Subheading 2]:#subheading-2
[Subheading 3]:#subheading-3
[Next steps]:#next-steps

[How to use the Docker VM Extension with Azure]:#How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]:#Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]:#Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]:../../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]:../../../app-service-web/web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]:../storage-whatis-account.md
[如何在门户中使用 Docker VM 扩展]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker 用户指南]:https://docs.docker.com/userguide/

[开始使用 Docker 和 Compose，在 Azure 虚拟机上定义和运行多容器应用程序]:../docker-compose-quickstart.md
