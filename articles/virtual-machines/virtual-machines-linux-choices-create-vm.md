<properties
	pageTitle="创建 Linux VM 的不同方式 | Microsoft Azure"
	description="列出在 Azure 上创建 Linux 虚拟机的不同方式，并提供进一步说明链接。"
	services="virtual-machines"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor=""
	tags="azure-service-management,azure-resource-manager"/>

<tags
	ms.service="virtual-machines"
	ms.date="08/12/2015"
	wacn.date=""/>

# 创建 Linux 虚拟机的不同方式

[AZURE.INCLUDE [了解部署模型](../includes/learn-about-deployment-models-both-include.md)]

Azure 提供不同方式来创建 VM，因为 VM 适合于不同用户和目的。这意味着你将需要对 VM 以及如何创建它进行一些选择。本文提供了这些选项的摘要和说明链接。

最近引入了 Azure 资源管理器模板作为一种创建和管理虚拟机的方法，而将其不同资源作为一个逻辑部署单元。此方法的说明包含如下（如果可用）。若要了解有关 Azure 资源管理器以及如何将资源作为一个单元管理的详细信息，请参阅[概述][]。

## 工具选项

### GUI：Azure 门户或预览门户

Azure 门户的图形用户界面是一种试用虚拟机的简便方式，尤其是在你刚开始摸索 Azure 时。使用 [Azure 门户](http://manage.windowsazure.cn)或 [Azure 门户](https://manage.windowsazure.cn)创建 VM。如需一般性说明，请参阅[创建自定义虚拟机][]，然后从“库”中选择任意 Linux 映像。请注意，[Azure 门户](http://manage.windowsazure.cn)只使用经典部署模型创建虚拟机。

### 命令 Shell：Azure CLI 或 Azure PowerShell

如果你喜欢使用命令行界面，请选择适用于 Mac 和 Linux 用户的 Azure 命令行界面 (CLI) 或 Azure PowerShell，后者提供 Windows PowerShell cmdlets for Azure 和自定义控制台。

有关 Azure CLI 的信息，请参阅[创建运行 Linux 的虚拟机][]。若要使用模板，请参阅[使用 Azure 资源管理器模板与 Azure CLI 来部署和管理虚拟机][]。

有关 Azure PowerShell，请参阅[使用 Azure PowerShell 创建和预配置基于 Linux 的虚拟机][]。若要使用模板，请参阅[使用 Azure 资源管理器模板与 PowerShell 来部署和管理虚拟机][]。

### 开发环境：Visual Studio

[使用 Visual Studio 创建用于网站的虚拟机][]

[使用计算、网络和存储 .NET 库部署 Azure 资源][]

## 操作系统和映像选项

根据要运行的操作系统选择映像。Azure 及其合作伙伴提供了许多映像，其中一些映像包括应用程序和工具。或者，使用你自己的某一映像。

### Azure 映像

这些说明显示如何使用 Azure 映像来创建已使用网络、负载平衡等选项自定义的虚拟机。请参阅[如何在 Azure 中创建运行 Linux 的自定义虚拟机][]。

### 使用你自己的映像

通过*捕获*现有 Azure 虚拟机来使用基于该 VM 的映像，或者上载你自己的存储在虚拟硬盘 (VHD) 中的映像：

- [如何使用 CLI 捕获 Linux 虚拟机以用作模板][]
- [创建并上载包含 Linux 操作系统的虚拟硬盘][]

## 后续步骤

[登录到虚拟机][]

[附加数据磁盘][]

## 其他资源

[基本配置测试环境][]

[Azure 混合云测试环境][]

[适合使用针对 Mac、Linux 和 Windows 的 Azure CLI 进行 VM 操作的等效资源管理器和服务管理命令][]

<!-- LINKS -->
[概述]: ../resource-group-overview.md

[Create a Virtual Machine Running Windows]: virtual-machines-windows-tutorial.md
[Create a Virtual Machine Running Linux]: virtual-machines-linux-tutorial.md

[适合使用针对 Mac、Linux 和 Windows 的 Azure CLI 进行 VM 操作的等效资源管理器和服务管理命令]: xplat-cli-azure-manage-vm-asm-arm.md
[使用 Azure 资源管理器模板与 Azure CLI 来部署和管理虚拟机]: virtual-machines-deploy-rmtemplates-azure-cli.md
[使用 Azure 资源管理器模板与 PowerShell 来部署和管理虚拟机]: virtual-machines-deploy-rmtemplates-powershell.md
[使用 Azure PowerShell 创建和预配置基于 Linux 的虚拟机]: virtual-machines-ps-create-preconfigure-linux-vms.md

[如何在 Azure 中创建运行 Linux 的自定义虚拟机]: virtual-machines-linux-create-custom.md
[如何使用 CLI 捕获 Linux 虚拟机以用作模板]: virtual-machines-linux-capture-image.md

[创建并上载包含 Linux 操作系统的虚拟硬盘]: virtual-machines-linux-create-upload-vhd.md

[使用 Visual Studio 创建用于网站的虚拟机]: virtual-machines-dotnet-create-visual-studio-powershell.md
[使用计算、网络和存储 .NET 库部署 Azure 资源]: virtual-machines-arm-deployment.md

[登录到虚拟机]: virtual-machines-linux-how-to-log-on.md

[附加数据磁盘]: virtual-machines-linux-how-to-attach-disk.md

[基本配置测试环境]: virtual-machines-base-configuration-test-environment.md
[Azure 混合云测试环境]: virtual-machines-hybrid-cloud-test-environments.md

[创建运行 Linux 的虚拟机]: virtual-machines-linux-tutorial.md
[创建自定义虚拟机]: virtual-machines-create-custom.md

<!---HONumber=Mooncake_1207_2015-->