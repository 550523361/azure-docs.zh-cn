<properties
	pageTitle="安装 Azure 命令行界面 | Microsoft Azure"
	description="安装适用于 Mac、Linux 和 Windows 的 Azure CLI 即可使用 Azure 服务"
	editor=""
	manager="timlt"
	documentationCenter=""
	authors="dlepow"
	services=""
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="multiple"
	ms.date="01/08/2016"
	wacn.date=""/>

# 安装 Azure CLI

快速安装 Azure 命令行界面 (Azure CLI)，以便使用一组基于 shell 的开源命令在 Microsoft Azure 中创建和管理资源。使用提供的安装程序包之一在操作系统上安装 Azure CLI，安装使用 Node.js 和 **npm** 的 CLI，或者安装 Azure CLI 作为 Docker 主机中的容器。有关更多选项和背景信息，请参阅 [GitHub](https://github.com/azure/azure-xplat-cli) 上的项目存储库。

[AZURE.INCLUDE [了解部署模型](../includes/learn-about-deployment-models-both-include.md)]

安装了 Azure CLI 之后，你将能够[将它与 Azure 订阅关联](/documentation/articles/xplat-cli-connect)，并从命令行界面（Bash、终端、命令提示符等）运行 **azure** 命令，以使用 Azure 资源。




## 使用安装程序

提供以下安装程序包：

* [Windows 安装程序][windows-installer]

* [OS X 安装程序](http://go.microsoft.com/fwlink/?LinkId=252249)

* [Linux 安装程序][linux-installer]


## 安装和使用 Node.js 与 npm

如果 Node.js 已安装在你的系统上，则使用以下命令安装 Azure CLI：

	npm install azure-cli -g

> [AZURE.NOTE] 在 Linux 分发中，你可能需要使用 `sudo` 才能成功运行 __npm__ 命令。

### 在使用 [dpkg](http://zh.wikipedia.org/wiki/Dpkg) 包管理的 Linux 分发上安装 node.js 和 npm
在这些分发中，最常使用[高级打包工具 (apt)](http://zh.wikipedia.org/wiki/Advanced_Packaging_Tool) 或基于 `.deb` 包格式的其他工具。示例包括 Ubuntu 和 Debian。

这些分发的较新版本大多要求安装 **nodejs-legacy**，目的是对 **npm** 工具进行适当的配置以安装 Azure CLI。以下代码所示命令可将 **npm** 正确安装在 Ubuntu 14.04 上。

	sudo apt-get install nodejs-legacy
	sudo apt-get install npm
	sudo npm install -g azure-cli

某些较旧的分发（如 Ubuntu 12.04）要求安装 Node.js 的最新二进制分发。以下代码显示了如何通过安装和使用 **curl** 来实现这一点。

>[AZURE.NOTE] 命令摘自安装说明，详情请访问[此链接](https://github.com/joyent/node/wiki/installing-node.js-via-package-manager)。不过，在使用 **sudo** 作为管道目标时，应始终检查所要安装的脚本，确保其执行的功能与你预期的一致，然后再通过 **sudo** 运行它们。强大的功能意味着重大的责任。

	sudo apt-get install curl
	curl -sL https://deb.nodesource.com/setup | sudo bash -
	sudo apt-get install -y nodejs
	sudo npm install -g azure-cli

### 在使用 [rpm](http://zh.wikipedia.org/wiki/RPM_Package_Manager) 包管理的 Linux 分发上安装 node.js 和 npm

在基于 RPM 的分发上安装 Node.js 需要启用 EPEL 存储库。以下代码显示了在 CentOS 7 上进行安装所需遵循的最佳实践。（请注意，在下面的第一行中，“-”（连字符）很重要！）

	su -
	yum update [enter]
	yum upgrade –y [enter]
	yum install epel-release [enter]
	yum install nodejs [enter]
	yum install npm [enter]
	npm install -g azure-cli  [enter]

### 在 Windows 和 Mac OS X 上安装 Node.js 和 npm

你可以使用 [Nodejs.org](https://nodejs.org/en/download/) 中的安装程序在 Windows 和 OS X 上安装 Node.js 和 npm。你可能需要重新启动计算机来完成安装。打开命令或终端窗口并键入相应命令，查看 node 和 npm 是否已正确安装

	npm -v

如果显示 npm 版本已安装，你可以继续操作并安装 Azure CLI

	npm install -g azure-cli

在安装结束时，你应该会看到如下内容：

	azure-cli@0.8.0 ..\node_modules\azure-cli
	|-- easy-table@0.0.1
	|-- eyes@0.1.8
	|-- xmlbuilder@0.4.2
	|-- colors@0.6.1
	|-- node-uuid@1.2.0
	|-- async@0.2.7
	|-- underscore@1.4.4
	|-- tunnel@0.0.2
	|-- omelette@0.1.0
	|-- github@0.1.6
	|-- commander@1.0.4 (keypress@0.1.0)
	|-- xml2js@0.1.14 (sax@0.5.4)
	|-- streamline@0.4.5
	|-- winston@0.6.2 (cycle@1.0.2, stack-trace@0.0.7, async@0.1.22, pkginfo@0.2.3, request@2.9.203)
	|-- kuduscript@0.1.2 (commander@1.1.1, streamline@0.4.11)
	|-- azure@0.7.13 (dateformat@1.0.2-1.2.3, envconf@0.0.4, mpns@2.0.1, mime@1.2.10, validator@1.4.0, xml2js@0.2.8, wns@0.5.3, request@2.25.0)

>[AZURE.NOTE] 对于 Linux 系统，你还可以通过从[源](http://go.microsoft.com/fwlink/?linkid=253472)进行构建的方式来安装 Azure CLI。有关从源代码生成的详细信息，请参阅存档中随附的 INSTALL 文件。

## 使用 Docker 容器

在 Docker 主机中，运行：

```
docker run -it microsoft/azure-cli
```

## 运行 Azure CLI 命令
安装了 Azure CLI 之后，你将可以从命令行用户界面（Bash、终端、命令提示符等）运行 **azure** 命令。例如，若要运行帮助命令，请键入以下命令：

```
azure help
```

若要查看安装的 Azure CLI 版本，请键入以下命令：

```
azure --version
```

你现在已准备就绪！ 若要访问所有 CLI 命令以使用自己的资源，请[从 Azure CLI 连接到你的 Azure 订阅](xplat-cli-connect.md)。

## 更新 CLI

Microsoft 会频繁发布 Azure CLI 的更新版本。使用适用于你的操作系统的安装程序重新安装 CLI，或者如果已安装 Node.js 和 npm，请键入以下命令（在 Linux 分发上可能需要使用 **sudo**）进行更新。

```
npm upgrade -g azure-cli
```

## 其他资源

* [将 Azure CLI 与 Azure 资源管理器配合使用][cliarm]

* [将 Azure CLI 与 Azure 服务管理配合使用][cliasm]

* 若要了解有关 Azure CLI、下载源代码、报告问题或贡献项目的详细信息，请访问[适用于 Azure CLI 的 GitHub 存储库](https://github.com/azure/azure-xplat-cli)。

* 如果你遇到有关使用 Azure CLI 或 Azure 的问题，请访问 [Azure 论坛](http://social.msdn.microsoft.com/Forums/windowsazure/home)。



[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/?linkid=9828653&clcid=0x409
[linux-installer]: http://go.microsoft.com/fwlink/?linkid=253472
[cliasm]: virtual-machines/virtual-machines-command-line-tools.md
[cliarm]: virtual-machines/xplat-cli-azure-resource-manager.md

<!---HONumber=Mooncake_0215_2016-->