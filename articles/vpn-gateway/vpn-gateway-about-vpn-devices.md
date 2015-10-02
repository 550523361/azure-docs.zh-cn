<properties 
   pageTitle="关于用于站点到站点 Azure 虚拟网络连接的 VPN 设备 | Microsoft Azure"
   description="了解用于 Azure 虚拟网络站点到站点 VPN 网关连接的 VPN 设备和 IPsec 参数。"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carolz"
   editor="tysonn" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/07/2015"
   ms.author="cherylmc" />

# 关于用于站点到站点虚拟网络连接的 VPN 设备

配置站点到站点 VPN 连接需要用到 VPN 设备。在创建混合云解决方案时，或者每当你想要在本地网络与虚拟网络之间建立安全连接时，可以使用站点到站点连接。本文介绍兼容的 VPN 设备和配置参数。

请务必知道，除了兼容的 VPN 设备，站点到站点连接还需要有公开的 IPv4 IP 地址。此外，你需要选择支持你的解决方案的网关类型。并非所有 VPN 设备都支持所有网关类型。请参阅 [VPN 网关](vpn-gateway-about-vpngateways.md)，以确认创建解决方案所需的网关类型。



## VPN 设备

我们在与设备供应商合作的过程中验证了一系列的标准站点到站点 (S2S) VPN 设备。有关已知与虚拟网络兼容的 VPN 设备的列表，请参阅下面的[兼容的 VPN 设备](#compatible-vpn-devices)。此列表中包含的设备系列中的所有设备都应适用于 Azure VPN 网关。若要获取配置 VPN 设备的帮助，请参考对应于各设备系列的设备配置模板。

除非另有说明，否则高性能 VPN 网关和动态路由 VPN 网关的规范是相同的。例如，与 Azure 动态路由 VPN 网关兼容的已验证 VPN 设备也与新的 Azure 高性能 VPN 网关兼容。


### 兼容的 VPN 设备

我们已与 VPN 设备供应商合作共同验证了特定 VPN 设备系列是否适用。以下部分提供了已知适用于我们的 VPN 网关的所有设备系列。除非另外特别说明，否则所列设备系列中的所有设备都已知适用于我们的网关。如果你的设备未列在该列表中，请参阅[不在兼容列表中的设备](#devices-not-on-the-compatible-list)。



如需 VPN 设备支持，请联系你的设备制造商。



| **供应商** | **设备系列** | **最低操作系统版本** | **静态路由** | **动态路由** |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Allied Telesis | AR 系列 VPN 路由器 | 2\.9.2 | 即将支持 | 不兼容 |
| Barracuda Networks, Inc. | Barracuda NG Firewall | Barracuda NG Firewall 5.4.3 | [Barracuda NG Firewall](https://techlib.barracuda.com/display/BNGV54/How%20to%20Configure%20an%20IPsec%20Site-to-Site%20VPN%20to%20a%20Windows%20Azure%20VPN%20Gateway)| 不兼容 |
| Barracuda Networks, Inc. | Barracuda Firewall | Barracuda Firewall 6.5 | [Barracuda Firewall](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | 不兼容 |
| Brocade | Vyatta 5400 vRouter | Virtual Router 6.6R3 GA | [配置说明](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) | 不兼容 |
| 检查点 | 安全网关 | R75.40、R75.40VS | [配置说明](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) | [配置说明](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco | ASA | 8\.3 | [Cisco ASA 示例](https://msdn.microsoft.com/library/azure/dn133793.aspx) | 不兼容 |
| Cisco | ASR | IOS 15.1（静态）、IOS 15.2（动态） | [Cisco ASR 示例](https://msdn.microsoft.com/library/azure/dn133802.aspx) | [Cisco ASR 示例](https://msdn.microsoft.com/library/azure/dn133802.aspx) |
| Cisco | ISR | IOS 15.0（静态）、IOS 15.1（动态） | [Cisco ISR 示例](https://msdn.microsoft.com/library/azure/dn133800.aspx) | [Cisco ISR 示例](https://msdn.microsoft.com/library/azure/dn133800.aspx) |
| Citrix | CloudBridge MPX 设备或 VPX 虚拟设备 | 不适用 | [集成说明](https://www.citrix.com/welcome.html?resource=%2Fdownloads%2Fcloudbridge%2Fbetas-and-tech-previews%2Fcloudbridge-azure-integration) | 不兼容 |
| Dell SonicWALL | TZ 系列、NSA 系列、SuperMassive 系列、E 类 NSA 系列 | SonicOS 5.8.x、SonicOS 5.9.x、SonicOS 6.x | [配置说明](https://www.sonicwall.com/app/projects/file_downloader/document_lib.php?t=TN&id=348) | 不兼容 |
| F5 | BIG-IP 系列 | 不适用 | [配置说明](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) | 不兼容 |
| Fortinet | FortiGate | FortiOS 5.0.7 | [配置说明](http://docs.fortinet.com/fortigate/admin-guides) | [配置说明](http://docs.fortinet.com/fortigate/admin-guides) |
| Internet Initiative Japan (IIJ) | SEIL 系列 | SEIL/X 4.60、SEIL/B1 4.60、SEIL/x86 3.20 | [配置说明](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) | 不兼容 |
| Juniper | SRX | JunOS 10.2（静态）、JunOS 11.4（动态） | [Juniper SRX 示例](https://msdn.microsoft.com/library/azure/dn133794.aspx) | [Juniper SRX 示例](https://msdn.microsoft.com/library/azure/dn133794.aspx) |
| Juniper | J 系列 | JunOS 10.4r9（静态）、JunOS 11.4（动态） | [Juniper J 系列示例](https://msdn.microsoft.com/library/azure/dn133799.aspx) | [Juniper J 系列示例](https://msdn.microsoft.com/library/azure/dn133799.aspx) |
| Juniper | ISG | ScreenOS 6.3（静态和动态） | [Juniper ISG 示例](https://msdn.microsoft.com/library/azure/dn133797.aspx) | [Juniper ISG 示例](https://msdn.microsoft.com/library/azure/dn133797.aspx) |
| Juniper | SSG | ScreenOS 6.2（静态和动态） | [Juniper SSG 示例](https://msdn.microsoft.com/library/azure/dn133796.aspx) | [Juniper SSG 示例](https://msdn.microsoft.com/library/azure/dn133796.aspx) |
| Microsoft | 路由和远程访问服务 | Windows Server 2012 | 不兼容 | [路由和远程访问服务 (RRAS) 示例](https://msdn.microsoft.com/library/azure/dn133801.aspx) |
| Openswan | Openswan | 2\.6.32 | （即将支持） | 不兼容 |
| Palo Alto Networks | 运行 PAN-OS 5.0 或更高版本的所有设备 | PAN-OS 5x 或更高版本 | [Palo Alto Networks](https://support.paloaltonetworks.com/) | 不兼容 |
| Watchguard | 全部 | Fireware XTM v11.x | [配置说明](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/) | 不兼容 |


### 不在兼容列表中的设备


如果已知兼容 VPN 设备列表中未列出你的设备，但你想要使用该设备建立 VPN 连接，那么，你需要确认该设备是否符合[网关要求](vpn-gateway-about-vpngateways.md#gateway-requirements)表中列出的最低要求。满足最低要求的设备也应该能顺利用于虚拟网络。请联系你的设备制造商了解更多支持和配置说明。


## 编辑设备配置示例

在下载提供的 VPN 设备配置示例后，你需要替换一些值来反映你环境的设置。

**编辑示例：**

1. 使用记事本打开示例。 
1. 搜索所有 <*text*> 字符串并将其替换为与你的环境相关的值。请确保包含 < and >。指定名称时，你选择的名称应是唯一的。如果命令无效，请查看你的设备制造商文档。

| **示例文本** | **更改为** |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP\_OnPremisesNetwork&gt; | 你为此对象选择的名称。示例：myOnPremisesNetwork |
| &lt;RP\_AzureNetwork&gt; | 你为此对象选择的名称。示例：myAzureNetwork |
| &lt;RP\_AccessList&gt; | 你为此对象选择的名称。示例：myAzureAccessList |
| &lt;RP\_IPSecTransformSet&gt; | 你为此对象选择的名称。示例：myIPSecTransformSet |
| &lt;RP\_IPSecCryptoMap&gt; | 你为此对象选择的名称。示例：myIPSecCryptoMap |
| &lt;SP\_AzureNetworkIpRange&gt; | 指定范围。示例：192.168.0.0 |
| &lt;SP\_AzureNetworkSubnetMask&gt; | 指定子网掩码。示例：255.255.0.0 |
| &lt;SP\_OnPremisesNetworkIpRange&gt; | 指定本地范围。示例：10.2.1.0 |
| &lt;SP\_OnPremisesNetworkSubnetMask&gt; | 指定本地子网掩码。示例：255.255.255.0 |
| &lt;SP\_AzureGatewayIpAddress&gt; | 此信息特定于你的虚拟网络，位于管理门户的“网关 IP 地址”中。 |
| &lt;SP\_PresharedKey&gt; | 此信息特定于你的虚拟网络，位于管理门户的“管理密钥”中。 |



## IPsec 参数

### IKE 阶段 1 设置

| **属性** | **静态路由 VPN 网关** | **动态路由 VPN 网关和标准或高性能 VPN 网关** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| SDK 版本 | IKEv1 | IKEv2 |
| Diffie-Hellman 组 | 组 2（1024 位） | 组 2（1024 位） |
| 身份验证方法 | 预共享密钥 | 预共享密钥 |
| 加密算法 | AES256 AES128 3DES | AES256 3DES |
| 哈希算法 | SHA1(SHA128) | SHA1(SHA128) |
| 阶段 1 安全关联 (SA) 生命周期（时间） | 28,800 秒 | 28,800 秒 |


### IKE 阶段 2 设置

| **属性** | **静态路由 VPN 网关** | **动态路由 VPN 网关和标准或高性能 VPN 网关** |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| SDK 版本 | IKEv1 | IKEv2 |
| 哈希算法 | SHA1(SHA128) | SHA1(SHA128) |
| 阶段 2 安全关联 (SA) 生命周期（时间）| 3,600 秒 | - | 阶段 2 安全关联 (SA) 生命周期（吞吐量）| 102,400,000 KB | - | IPsec SA 加密和身份验证产品（按偏好顺序列出）| 1.ESP-AES256 2.ESP-AES128 3.ESP-3DES 4.N/A | 请参阅动态路由网关 IPsec 安全关联 (SA) 产品 | | 完全向前保密 (PFS) | 否 | 是 (DH Group1) | | 对等体存活检测 | 不支持 | 支持 |

### 动态路由网关 IPsec 安全关联 (SA) 产品

下表列出b IPsec SA 加密和身份验证产品。这些产品按提供或接受产品的偏好顺序列出。

| **IPsec SA 加密和身份验证产品** | **Azure 网关作为发起方** | Azure 网关作为响应方 |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1 | ESP AES\_256 SHA | ESP AES\_128 SHA |
| 2 | ESP AES\_128 SHA | ESP 3\_DES MD5 |
| 3 | ESP 3\_DES MD5 | ESP 3\_DES SHA |
| 4 | ESP 3\_DES SHA | AH SHA1（具有含 null HMAC 的 ESP AES\_128） |
| 5 | AH SHA1（具有含 null HMAC 的 ESP AES\_256） | AH SHA1（具有含 null HMAC 的 ESP 3\_DES） |
| 6 | AH SHA1（具有含 null HMAC 的 ESP AES\_128） | AH MD5（具有含 null HMAC 的 ESP 3\_DES），不建议生命周期 |
| 7 | AH SHA1（具有含 null HMAC 的 ESP 3\_DES） | AH SHA1（具有 ESP 3\_DES SHA1），无生命周期 |
| 8 | AH MD5（具有含 null HMAC 的 ESP 3\_DES），不建议生命周期 | AH MD5（具有 ESP 3\_DES MD5），无生命周期 |
| 9 | AH SHA1（具有 ESP 3\_DES SHA1），无生命周期 | ESP DES MD5 |
| 10 | AH MD5（具有 ESP 3\_DES MD5），无生命周期 | ESP DES SHA1，无生命周期 |
| 11 | ESP DES MD5 | AH SHA1（具有 ESP DES null HMAC），不建议生命周期 |
| 12 | ESP DES SHA1，无生命周期 | AH MD5（具有 ESP DES null HMAC），不建议生命周期 |
| 13 | AH SHA1（具有 ESP DES null HMAC），不建议生命周期 | AH SHA1（具有 ESP DES SHA1），无生命周期 |
| 14 | AH MD5（具有 ESP DES null HMAC），不建议生命周期 | AH MD5（具有 ESP DES MD5），无生命周期 |
| 15 | AH SHA1（具有 ESP DES SHA1），无生命周期 | ESP SHA，无生命周期 |
| 16 | AH MD5（具有 ESP DES MD5），无生命周期 | ESP MD5，无生命周期 |
| 17 | | - | AH SHA，无生命周期| | 18 | - | AH MD5，无生命周期 |




- 你可以使用动态路由和高性能 VPN 网关指定 IPsec ESP NULL 加密，以便在 Azure 网络中建立 VNet 到 VNet 的连接。 

- adk 要通过 Internet 建立跨界连接，请使用默认的 Azure VPN 网关设置以及上表中列出的加密和哈希算法，以确保关键通信的安全性。

## 后续步骤


若要了解有关 VPN 网关的详细信息，请参阅[关于 VPN 网关](vpn-gateway-about-vpngateways.md)。

若要配置站点到站点 VPN，请参阅[使用站点到站点 VPN 连接创建虚拟网络](vpn-gateway-site-to-site-create.md)。

<!---HONumber=71-->