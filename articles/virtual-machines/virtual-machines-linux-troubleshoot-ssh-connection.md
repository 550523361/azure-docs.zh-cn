<properties
	pageTitle="对 Azure VM 的 SSH 连接进行故障排除 | Azure"
	description="为运行 Linux 的 Azure 虚拟机排查并修复“SSH 连接失败”或“SSH 连接被拒绝”等 SSH 错误。"
	keywords="ssh 连接被拒绝,ssh 错误,azure ssh,SSH 连接失败"
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.date="04/12/2016"
	wacn.date=""/>

# 对于基于 Linux 的 Azure 虚拟机的 Secure Shell (SSH) 连接进行故障排除

有许多原因可能会导致在尝试连接到基于 Linux 的 Azure 虚拟机时出现 SSH 错误。本文将帮助你找出原因并更正它们。

[AZURE.INCLUDE [了解部署模型](../includes/learn-about-deployment-models-both-include.md)]


如果你对本文中的任何观点存在疑问，可以联系 [MSDN Azure 和 CSDN 论坛](/support/forums/)上的 Azure 专家。或者，你也可以提出 Azure 支持事件。请转到 [Azure 支持站点](/support/contact/)并单击“获取支持”。有关使用 Azure 支持的信息，请阅读 [Azure 支持常见问题](/support/faq/)。


## 修复常见的 SSH 错误

本部分列出了常见的 SSH 连接问题的快速修复步骤。

### 使用经典部署模型创建的虚拟机

尝试以下步骤来解决最常见的 SSH 连接失败：

1. 从 [Azure 门户](https://portal.azure.cn)<br>“重置远程访问” 单击“浏览”>“虚拟机(经典)”> 你的 Linux 虚拟机 >“重置远程...”。

2. 重启虚拟机。<br> 在 [Azure 门户](https://portal.azure.cn)中，单击“浏览”>“虚拟机(经典)”> 你的 Linux 虚拟机 >“重新启动”。<br> 在 [Azure 经典门户](https://manage.windowsazure.cn)中，打开“虚拟机”>“实例”>“重新启动”。

3. [调整虚拟机的大小](https://msdn.microsoft.com/zh-cn/library/dn168976.aspx)。

4. 按照[如何为基于 Linux 的虚拟机重置密码或 SSH](/documentation/articles/virtual-machines-linux-classic-reset-access) 中的说明，在虚拟机上执行以下操作：

	- 重置密码或 SSH 密钥。
	- 创建新的 “sudo” 用户帐户。
	- 重置 SSH 配置。

5. 检查 VM 的资源运行状况以了解是否有任何平台问题。<br> 单击“浏览”>“虚拟机(经典)”> 你的 Linux 虚拟机 >“设置”>“检查运行状况”。


### 使用资源管理器部署模型创建的虚拟机

若要解决使用资源管理器部署模型创建的虚拟机的常见 SSH 问题，请尝试以下步骤。

#### 重置 SSH 连接
通过使用 Azure CLI 确保已安装版本 2.0.5 或更高版本的 [Microsoft Azure Linux 代理](/documentation/articles/virtual-machines-linux-agent-user-guide)。

如果尚未安装 Azure CLI，请[安装 Azure CLI 并连接到 Azure 订阅](/documentation/articles/xplat-cli-install)，然后使用 `azure login` 命令登录。请确保你在资源管理器模式下：

	azure config mode arm

使用以下方法之一重置 SSH 连接：

* 按以下示例所示使用 `vm reset-access` 命令。

		azure vm reset-access -g YourResourceGroupName -n YourVirtualMachineName -r

这将在虚拟机上安装 `VMAccessForLinux` 扩展。

* 或者，使用以下内容创建名为 PrivateConf.json 的文件：

		{  
			"reset_ssh":"True"
		}

然后手动运行 `VMAccessForLinux` 扩展以重置 SSH 连接。

	azure vm extension set "YourResourceGroupName" "YourVirtualMachineName" VMAccessForLinux Microsoft.OSTCExtensions "1.2" --private-config-path PrivateConf.json

#### 重置 SSH 凭据

* 运行 `vm reset-access` 命令以设置任何 SSH 凭据。

		azure vm reset-access TestRgV2 TestVmV2 -u NewUser -p NewPassword

在命令行上键入 `azure vm reset-access -h` 可以查看有关此命令的详细信息。

* 或者，使用以下内容创建名为 PrivateConf.json 的文件。

		{
			"username":"NewUsername", "password":"NewPassword", "expiration":"2016-01-01", "ssh_key":"", "reset_ssh":false, "remove_user":""
		}

然后使用上述文件运行 Linux 扩展。

	$azure vm extension set "testRG" "testVM" VMAccessForLinux Microsoft.OSTCExtensions "1.2" --private-config-path PrivateConf.json

请注意，你可以遵循[如何为基于 Linux 的虚拟机重置密码或 SSH](/documentation/articles/virtual-machines-linux-classic-reset-access) 中的类似步骤来尝试其他不同的做法。请记得修改资源管理器模式的 Azure CLI 指令。


## SSH 错误的详细故障排除

如果 SSH 客户端仍然无法连接到虚拟机上的 SSH 服务，原因可能是多方面的。下面是这种失败所涉及到的组件。

![显示 SSH 服务组件的图表](./media/virtual-machines-linux-troubleshoot-ssh-connection/ssh-tshoot1.png)

以下部分将帮助你查明失败的原因，并得出解决方法或应对措施。

### 预备步骤

首先，在门户中检查虚拟机的状态。

在 [Azure 经典门户](https://manage.windowsazure.cn)中，针对采用经典部署模型的虚拟机：

1. 单击“虚拟机”>“VM 名称”。
2. 单击 VM 的“仪表板”以查看 VM 的状态。
3. 单击“监视器”，以查看计算、存储和网络资源的最近活动。
4. 单击“终结点”以确保 SSH 流量有终结点。

在 [Azure 门户](https://portal.azure.cn)中：

1. 如需查找使用经典部署模型创建的虚拟机，请单击“浏览”>“虚拟机(经典)”>“VM 名称”。如需查找使用资源管理器创建的虚拟机，请单击“浏览”>“虚拟机”>“VM 名称”。该虚拟机的状态窗格中应显示“正在运行”。向下滚动以显示计算、存储和网络资源的最近活动。
2. 单击“设置”以检查终结点、IP 地址和其他设置。若要确定使用资源管理器创建的虚拟机中的终结点，请检查是否定义了[网络安全组](/documentation/articles/virtual-networks-nsg)、规则是否应用于该组，以及在子网中是否引用了这些终结点。

若要验证网络连接，请检查所配置的终结点，并了解是否可通过其他协议（例如 HTTP 或其他服务）连接到该 VM。

在执行这些步骤之后，重新尝试 SSH 连接。


### 查明问题的根源

如果计算机上的 SSH 客户端无法连接到 Azure 虚拟机上的 SSH 服务，则可能是以下来源存在问题或配置错误所造成的：

- SSH 客户端计算机
- 组织边缘设备
- 云服务终结点和访问控制列表 (ACL)
- 网络安全组
- 基于 Linux 的 Azure 虚拟机

#### 来源 1：SSH 客户端计算机

若要将你的计算机从失败原因中排除，请检查你的计算机是否能够与其他基于 Linux 的本地计算机建立 SSH 连接。

![突出显示 SSH 客户端计算机组件的图表](./media/virtual-machines-linux-troubleshoot-ssh-connection/ssh-tshoot2.png)

如果不能，请检查你的计算机上是否存在以下项：

- 本地防火墙设置阻止了入站或出站 SSH 流量 (TCP 22)
- 本地安装的客户端代理软件阻止了 SSH 连接
- 本地安装的网络监视软件阻止了 SSH 连接
- 监视流量或允许/禁止特定类型流量的其他类型的安全软件

对于所有这些情况，请暂时禁用可疑软件，然后尝试与本地计算机建立 SSH 连接以找出原因。然后，与网络管理员合作以更正软件的设置，从而允许 SSH 连接。

如果使用的是证书身份验证，则需验证你是否具有这些权限以访问主目录中的 .ssh 文件夹：

- Chmod 700 ~/.ssh
- Chmod 644 ~/.ssh/*.pub
- Chmod 600 ~/.ssh/id\_rsa（或存储私钥的其他任何文件）
- Chmod 644 ~/.ssh/known\_hosts（包含已通过 SSH 连接到的主机）

#### 来源 2：组织边缘设备

若要将你的组织边缘设备从失败原因中排除，请检查直接连接到 Internet 的计算机是否可以与 Azure VM 建立 SSH 连接。如果是通过站点到站点 VPN 或 ExpressRoute 连接来访问 VM，请跳转到[来源 4：网络安全组](#nsg)。

![突出显示组织边缘设备的图表](./media/virtual-machines-linux-troubleshoot-ssh-connection/ssh-tshoot3.png)

如果没有直接连接到 Internet 的计算机，可以轻松地在其自己的资源组或云服务中创建新的 Azure 虚拟机，然后进行使用。有关详细信息，请参阅[在 Azure 中创建运行 Linux 的虚拟机](/documentation/articles/virtual-machines-linux-quick-create-cli)。测试完成后，请删除资源组或虚拟机以及云服务。

如果可以创建与直接连接到 Internet 的计算机之间的 SSH 连接，则检查你的组织边缘设备中是否存在以下问题：

- 内部防火墙阻止了与 Internet 的 SSH 连接
- 代理服务器阻止了 SSH 连接
- 边界网络中的设备上运行的入侵检测或网络监视软件阻止了 SSH 连接

与网络管理员合作以更正组织边缘设备的设置，从而允许与 Internet 建立 SSH 流量连接。

#### 来源 3：云服务终结点和 ACL

> [AZURE.NOTE] 此来源仅适用于使用经典部署模型创建的虚拟机。对于使用资源管理器创建的虚拟机，请跳转到[来源 4：网络安全组](#nsg)。

若要将云服务终结点和 ACL 从失败原因中排除，请检查同一虚拟网络中的其他 Azure VM 是否可与使用[经典部署模型](/documentation/articles/resource-manager-deployment-model)创建的 VM 建立 SSH 连接。

![突出显示云服务终结点和 ACL 的图表](./media/virtual-machines-linux-troubleshoot-ssh-connection/ssh-tshoot4.png)

如果同一虚拟网络中没有其他 VM，你可以轻松创建一个新 VM。有关详细信息，请参阅[在 Azure 中创建运行 Linux 的虚拟机](/documentation/articles/virtual-machines-linux-quick-create-cli)。测试完成后，请删除多余的 VM。

如果可以与同一虚拟网络中的某个 VM 建立 SSH 连接，请检查：

- 目标 VM 上 SSH 流量的终结点配置。终结点的专用 TCP 端口应该与 VM 上的 SSH 服务正在侦听的 TCP 端口（默认为 22）匹配。对于在资源管理器部署模型中使用模板创建的 VM，请通过“浏览”>“虚拟机(v2)”>“VM 名称”>“设置”>“终结点”，验证 Azure 门户中的 SSH TCP 端口号。
- 目标虚拟机上的 SSH 流量终结点的 ACL。ACL 允许你指定基于源 IP 地址允许或拒绝的从 Internet 传入的流量。错误配置的 ACL 可能会阻止 SSH 流量传入终结点。检查你的 ACL 以确保允许从你的代理服务器或其他边缘服务器的公共 IP 地址传入的流量。有关详细信息，请参阅[关于网络访问控制列表 (ACL)](/documentation/articles/virtual-networks-acl)。

若要将终结点从问题原因中排除，请删除当前终结点，然后创建一个新的终结点并指定 **SSH** 名称（公用和专用端口号为 TCP 端口 22）。有关详细信息，请参阅[在 Azure 中的虚拟机上设置终结点](/documentation/articles/virtual-machines-windows-classic-setup-endpoints)。

<a id="nsg"></a>
#### 来源 4：网络安全组

通过使用网络安全组，可以对允许的入站和出站流量进行更精细的控制。你可以创建跨 Azure 虚拟网络中的子网和云服务的规则。检查你的网络安全组规则，以确保允许来自和去往 Internet 的 SSH 流量。有关详细信息，请参阅[关于网络安全组](/documentation/articles/virtual-networks-nsg)。

#### 来源 5：基于 Linux 的 Azure 虚拟机

最后一个可能出现问题的来源是 Azure 虚拟机本身。

![突出显示基于 Linux 的 Azure 虚拟机的图表](./media/virtual-machines-linux-troubleshoot-ssh-connection/ssh-tshoot5.png)

如果尚未这样做，请按照[如何为基于 Linux 的虚拟机重置密码或 SSH](/documentation/articles/virtual-machines-linux-classic-reset-access) 中的说明，在虚拟机上执行操作。

再次尝试从你的计算机建立连接。如果仍然失败，则可能存在以下问题：

- 目标虚拟机上未运行 SSH 服务。
- TCP 端口 22 上未侦听 SSH 服务。若要测试这一点，可在本地计算机上安装一个远程登录客户端，然后运行“telnet *cloudServiceName*.chinacloudapp.cn 22”。这将确定虚拟机是否允许与 SSH 终结点进行入站和出站通信。
- 目标虚拟机上的本地防火墙具有阻止入站或出站 SSH 流量的规则。
- Azure 虚拟机上运行的入侵检测或网络监视软件阻止了 SSH 连接。


## 其他资源

对于采用经典部署模型的虚拟机，请参阅[如何为基于 Linux 的虚拟机重置密码或 SSH](/documentation/articles/virtual-machines-linux-classic-reset-access)

[对与基于 Windows 的 Azure 虚拟机的 Windows 远程桌面连接进行故障排除](/documentation/articles/virtual-machines-windows-troubleshoot-rdp-connection)

[对在 Azure 虚拟机上运行的应用程序的访问进行故障排除](/documentation/articles/virtual-machines-linux-troubleshoot-app-connection)

<!---HONumber=Mooncake_0509_2016-->