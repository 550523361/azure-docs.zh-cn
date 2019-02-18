---
title: 快速入门 - 使用 Azure PowerShell 创建 Azure DNS 区域和记录
description: 了解如何在 Azure DNS 中创建 DNS 区域和记录。 这是有关使用 Azure PowerShell 创建和管理你的第一个 DNS 区域和记录的分步快速入门。
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 12/4/2018
ms.author: victorh
ms.openlocfilehash: 839c97ccccbc1ce2cf646afcd27894a190eda1b0
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000877"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-powershell"></a>快速入门：使用 Azure PowerShell 创建 Azure DNS 区域和记录

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

在本快速入门中，你将使用 Azure PowerShell 创建你的第一个 DNS 区域和记录。 也可以使用 [Azure 门户](dns-getstarted-portal.md)或 [Azure CLI](dns-getstarted-cli.md) 执行这些步骤。 

DNS 区域用来托管某个特定域的 DNS 记录。 若要开始在 Azure DNS 中托管域，需要为该域名创建 DNS 区域。 随后会在此 DNS 区域内为每个 DNS 记录创建域。 最后，要将 DNS 区域发布到 Internet，需要为域配置名称服务器。 以下描述了上述每一个步骤。

Azure DNS 还支持创建专用域。 有关如何创建第一个专用 DNS 区域和记录的分步说明，请参阅 [Azure DNS 专用区域入门（使用 PowerShell）](private-dns-getstarted-powershell.md)。

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="create-the-resource-group"></a>创建资源组

在创建 DNS 区域之前，创建一个资源组来包含 DNS 区域：

```powershell
New-AzResourceGroup -name MyResourceGroup -location "eastus"
```

## <a name="create-a-dns-zone"></a>创建 DNS 区域

通过使用 `New-AzDnsZone` cmdlet 创建 DNS 区域。 以下示例在名为 *MyResourceGroup* 的资源组中创建名为 *contoso.com* 的 DNS 区域。 使用该示例创建 DNS 区域，将相应的值替换成自己的值。

```powershell
New-AzDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>创建 DNS 记录

可以使用 `New-AzDnsRecordSet` cmdlet 创建记录集。 下面的示例在资源组“MyResourceGroup”中在 DNS 区域“contoso.com”中创建相对名称为“www”的一个记录集。 记录集的完全限定名称为“www.contoso.com”。 记录类型为“A”，IP 地址为“1.2.3.4”，TTL 为 3600 秒。

```powershell
New-AzDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "1.2.3.4")
```

## <a name="view-records"></a>查看记录

若要列出区域中的 DNS 记录，请使用：

```powershell
Get-AzDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="update-name-servers"></a>更新名称服务器

正确设置 DNS 区域和记录后，需要将域名配置为使用 Azure DNS 名称服务器。 这样，Internet 上的其他用户便可以找到 DNS 记录。

区域的名称服务器是通过 `Get-AzDnsZone` cmdlet 指定的：

```powershell
Get-AzDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

这些名称服务器应当配置有域名注册机构（向其购买域名的机构）。 域名注册机构将提供选项来为域设置名称服务器。 有关详细信息，请参见[教程：在 Azure DNS 中托管域](dns-delegate-domain-azure-dns.md#delegate-the-domain)。

## <a name="delete-all-resources"></a>删除所有资源

当不再需要时，可以通过删除资源组来删除本快速入门中创建的所有资源：

```powershell
Remove-AzResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>后续步骤

现在，你已使用 Azure PowerShell 创建了你的第一个 DNS 区域和记录，可以在自定义域中为 Web 应用创建记录了。

> [!div class="nextstepaction"]
> [在自定义域中为 web 应用创建 DNS 记录](./dns-web-sites-custom-domain.md)

