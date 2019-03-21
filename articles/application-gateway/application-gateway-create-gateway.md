---
title: 创建、启动或删除应用程序网关 | Microsoft Docs
description: 此页提供有关创建、配置、启动和删除 Azure 应用程序网关的说明
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.assetid: 577054ca-8368-4fbf-8d53-a813f29dc3bc
ms.service: application-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: victorh
ms.openlocfilehash: 60ea054f6a3305e523e0843690bf58f7998cf0bb
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2019
ms.locfileid: "57783205"
---
# <a name="create-start-or-delete-an-application-gateway-with-powershell"></a>使用 PowerShell 创建、启动或删除应用程序网关 

> [!div class="op_single_selector"]
> * [Azure 门户](application-gateway-create-gateway-portal.md)
> * [Azure 资源管理器 PowerShell](application-gateway-create-gateway-arm.md)
> * [Azure 经典 PowerShell](application-gateway-create-gateway.md)
> * [Azure 资源管理器模板](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Azure 应用程序网关是第 7 层负载均衡器。 它在不同服务器之间提供故障转移和性能路由 HTTP 请求，而不管它们是在云中还是本地。 应用程序网关提供许多应用程序传送控制器 (ADC) 功能，包括 HTTP 负载均衡、基于 Cookie 的会话相关性、安全套接字层 (SSL) 卸载、自定义运行状况探测、多站点支持，以及许多其他功能。 若要查找支持功能的完整列表，请参阅[应用程序网关概述](application-gateway-introduction.md)

本文指导完成创建、配置、启动和删除应用程序网关的步骤。

## <a name="before-you-begin"></a>开始之前

1. 使用 Web 平台安装程序安装最新版本的 Azure PowerShell cmdlet。 可以从[下载页](https://azure.microsoft.com/downloads/)的“Windows PowerShell”部分下载并安装最新版本。
2. 如果有现有的虚拟网络，请选择现有一个空子网，或者在现有虚拟网络中创建一个新子网，专门供应用程序网关使用。 除非使用 vnet 对等互连，否则应用程序网关部署到的虚拟网络必须与要部署在应用程序网关后面的资源相同。 若要了解详细信息，请访问 [Vnet 对等互连](../virtual-network/virtual-network-peering-overview.md)
3. 请确认已创建包含有效子网、可正常运行的虚拟网络。 请确保没有虚拟机或云部署正在使用子网。 应用程序网关必须单独位于虚拟网络子网中。
4. 必须存在配置为使用应用程序网关的服务器，或者必须在虚拟网络中为其创建终结点，或者必须为其分配公共 IP/VIP。

## <a name="what-is-required-to-create-an-application-gateway"></a>创建应用程序网关需要什么？

使用 `New-AzureApplicationGateway` 命令创建应用程序网关时，此时无需设置配置；配置新创建的资源时，必须使用 XML 或配置对象进行配置。

有效值为：

* **后端服务器池：** 后端服务器的 IP 地址列表。 列出的 IP 地址应属于虚拟网络子网，或者是公共 IP/VIP。
* **后端服务器池设置：** 每个池均具有端口、协议和基于 Cookie 的相关性等设置。 这些设置绑定到池，并会应用到池中的所有服务器。
* **前端端口：** 此端口是应用程序网关上打开的公共端口。 流量将抵达此端口，并重定向到后端服务器之一。
* **侦听器：** 侦听器具有前端端口、协议（Http 或 Https，这些值区分大小写）和 SSL 证书名称（如果要配置 SSL 卸载）。
* **规则：** 规则会绑定侦听器和后端服务器池，并定义当流量抵达特定侦听器时应定向到哪个后端服务器池。

## <a name="create-an-application-gateway"></a>创建应用程序网关

创建应用程序网关：

1. 创建应用程序网关资源。
2. 创建配置 XML 文件或配置对象。
3. 将配置提交到新建的应用程序网关资源。

> [!NOTE]
> 如果需要为应用程序网关配置自定义探测，请参阅 [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-classic-ps.md)（使用 PowerShell 创建带自定义探测的应用程序网关）。 有关详细信息，请查看 [custom probes and health monitoring](application-gateway-probe-overview.md) （自定义探测和运行状况监视）。

![方案示例][scenario]

### <a name="create-an-application-gateway-resource"></a>创建应用程序网关资源

要创建网关，请使用 `New-AzureApplicationGateway` cmdlet，并将值替换为你自己的值。 此时不会开始计收网关的费用。 计费会在后面已成功启动网关时开始。

以下示例使用名为“testvnet1”的虚拟网络和名为“subnet-1”的子网创建应用程序网关：

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

*Description*、*InstanceCount* 和 *GatewaySize* 是可选参数。

若要验证是否已创建网关，可以使用 `Get-AzureApplicationGateway` cmdlet。

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Stopped
VirtualIPs    : {}
DnsName       :
```

> [!NOTE]
> *InstanceCount* 的默认值为 2，最大值为 10。 *GatewaySize* 的默认值为 Medium。 可以选择 Small、Medium 或 Large。

VirtualIPs 和 DnsName 显示为空白，因为网关尚未启动。 这些值在网关进入运行状态后立即创建。

## <a name="configure-the-application-gateway"></a>配置应用程序网关

可以使用 XML 或配置对象配置应用程序网关。

### <a name="configure-the-application-gateway-by-using-xml"></a>使用 XML 配置应用程序网关

在以下示例中，使用 XML 文件配置所有应用程序网关设置，并将这些设置提交到应用程序网关资源。  

#### <a name="step-1"></a>步骤 1

将以下文本复制到记事本中。

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>(name-of-your-frontend-port)</Name>
            <Port>(port number)</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>(name-of-your-backend-pool)</Name>
            <IPAddresses>
                <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>(backend-setting-name-to-configure-rule)</Name>
            <Port>80</Port>
            <Protocol>[Http|Https]</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>(name-of-the-listener)</Name>
            <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
            <Protocol>[Http|Https]</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>(name-of-load-balancing-rule)</Name>
            <Type>basic</Type>
            <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
            <Listener>(name-of-the-listener)</Listener>
            <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

编辑配置项的括号之间的值。 使用扩展名 .xml 保存文件。

> [!IMPORTANT]
> 协议项 Http 或 Https 区分大小写。

以下示例演示如何使用配置文件设置应用程序网关。 该示例负载均衡公共端口 80 上的 HTTP 流量，并将网络流量发送到两个 IP 地址之间的后端端口 80。

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>BackendPool1</Name>
            <IPAddresses>
                <IPAddress>10.0.0.1</IPAddress>
                <IPAddress>10.0.0.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>BackendSetting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>HTTPListener1</Name>
            <FrontendPort>FrontendPort1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>HttpLBRule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
            <Listener>HTTPListener1</Listener>
            <BackendAddressPool>BackendPool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

#### <a name="step-2"></a>步骤 2

下一步，设置应用程序网关。 将 `Set-AzureApplicationGatewayConfig` cmdlet 与配置 XML 文件配合使用。

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

### <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>使用配置对象配置应用程序网关

以下示例演示如何使用配置对象配置应用程序网关。 必须单独配置所有配置项，然后将其添加到应用程序网关配置对象。 创建配置对象之后，使用 `Set-AzureApplicationGateway` 命令将配置提交到之前创建的应用程序网关资源。

> [!NOTE]
> 在为每个配置对象分配值之前，需要声明 PowerShell 用于存储的对象类型。 创建各项的第一行定义了所使用的 `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)`。

#### <a name="step-1"></a>步骤 1

创建每个配置项。

按以下示例中所示创建前端 IP。

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

按以下示例中所示创建前端端口。

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

创建后端服务器池。

定义要添加到后端服务器池的 IP 地址（如下个示例所示）。

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

使用 $server 对象将值添加到后端池对象 ($pool)。

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

创建后端服务器池设置。

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

创建侦听器。

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

创建规则。

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

#### <a name="step-2"></a>步骤 2

将每个配置项分配给应用程序网关配置对象 ($appgwconfig)。

将前端 IP 添加到配置。

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

将前端端口添加到配置。

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
将后端服务器池添加到配置。

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

将后端池设置添加到配置。

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

将侦听器添加到配置。

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

将规则添加到配置。

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a>步骤 3
使用 `Set-AzureApplicationGatewayConfig` 将配置对象提交到应用程序网关资源。

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-the-gateway"></a>启动网关

配置网关后，使用 `Start-AzureApplicationGateway` cmdlet 来启动网关。 成功启动网关后，将开始计收应用程序网关的费用。

> [!NOTE]
> `Start-AzureApplicationGateway` cmdlet 最多可能需要 15 到 20 分钟才能完成。

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-the-gateway-status"></a>验证网关状态

使用 `Get-AzureApplicationGateway` cmdlet 检查网关状态。 如果前一步骤中的 `Start-AzureApplicationGateway` 成功，则 *State* 应为 Running，*Vip* 和 *DnsName* 应包含有效的条目。

以下示例演示了一个正常运行并已准备好将流量定向到 `http://<generated-dns-name>.cloudapp.net`的应用程序网关。

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
Vip           : 138.91.170.26
DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net
```

## <a name="delete-the-application-gateway"></a>删除应用程序网关

若要删除应用程序网关，请执行以下步骤：

1. 使用 `Stop-AzureApplicationGateway` cmdlet 停止该网关。
2. 使用 `Remove-AzureApplicationGateway` cmdlet 删除该网关。
3. 使用 `Get-AzureApplicationGateway` cmdlet 验证是否已删除该网关。

以下示例在第一行显示 `Stop-AzureApplicationGateway` cmdlet，接着显示输出。

```powershell
Stop-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

在应用程序网关进入停止状态后，使用 `Remove-AzureApplicationGateway` cmdlet 删除该服务。

```powershell
Remove-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

若要验证是否已删除服务，可以使用 `Get-AzureApplicationGateway` cmdlet。 此步骤不是必需的。

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
.....
```

## <a name="next-steps"></a>后续步骤

如果要配置 SSL 卸载，请参阅 [Configure an application gateway for SSL offload](application-gateway-ssl.md)（配置应用程序网关以进行 SSL 卸载）。

如果要将应用程序网关配置为与内部负载均衡器配合使用，请参阅[创建具有内部负载均衡器 (ILB) 的应用程序网关](application-gateway-ilb.md)。

如需负载均衡选项的其他常规信息，请参阅：

* [Azure 负载均衡器](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure 流量管理器](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png
