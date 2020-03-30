---
title: VPN 网关：修改网关 IP 地址设置：Azure 门户
description: 本文介绍了如何使用 Azure 门户更改本地网络网关的 IP 地址前缀。
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: fdb98242cede36f818604a7a5d713f5f7c75daad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864020"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>使用 Azure 门户修改本地网络网关设置

有时本地网络网关 AddressPrefix 或 GatewayIPAddress 的设置会变更。 本文演示如何修改本地网络网关设置。 还可以选择以下列表中的其他选项，使用另一种方法来修改这些设置：

在删除连接之前，可能需要下载用于连接设备的配置，以便获取已定义的 PSK。 这样，便无需在另一端重新定义它。

> [!div class="op_single_selector"]
> * [Azure 门户](vpn-gateway-modify-local-network-gateway-portal.md)
> * [电源外壳](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>修改 IP 地址前缀

修改 IP 地址前缀时，执行的步骤取决于你的本地网络网关是否具有连接。

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>修改网关 IP 地址

如果要连接的 VPN 设备已更改其公共 IP 地址，则需根据该更改修改本地网关。 更改公共 IP 地址时，执行的步骤取决于你的本地网络网关是否具有连接。

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>后续步骤

可验证网关连接。 请参阅[验证网关连接](vpn-gateway-verify-connection-resource-manager.md)。
