---
author: rothja
ms.service: azure-resource-manager
ms.topic: include
ms.date: 2/14/2020
ms.author: rohink
ms.openlocfilehash: 0f7187300ec96ce417866c4fb8fa02783c1da63a
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86515833"
---
**公共 DNS 区域**

| 资源 | 限制 |
| --- | --- |
| 每个订阅的公共 DNS 区域数 |250 <sup>1</sup> |
| 每个公共 DNS 区域的记录集数 |10,000 <sup>1</sup> |
| 公共 DNS 区域中每个记录集的记录数 |20 |
| 单个 Azure 资源的别名记录数 |20|

<sup>1</sup>如果需要增加这些限制，请与 Azure 支持部门联系。

**专用 DNS 区域**

| 资源 | 限制 |
| --- | --- |
| 每个订阅的专用 DNS 区域数 |1000|
| 每个专用 DNS 区域的记录集 |25000|
| 针对专用 DNS 区域的每个记录集的记录 |20|
| 每个专用 DNS 区域的虚拟网络链接 |1000|
| 启用了自动注册的每个专用 DNS 区域的虚拟网络链接 |100|
| 虚拟网络可以链接到并且已启用自动注册的专用 DNS 区域数 |1|
| 虚拟网络可以链接的专用 DNS 区域数 |1000|
| 虚拟机每秒可发送到 Azure DNS 解析程序的 DNS 查询数 |500 <sup>1</sup> |
| 每个虚拟机排队的最大 DNS 查询数（等待响应） |200 <sup>1</sup> |

<sup>1</sup>这些限制适用于每个单独的虚拟机，而不是在虚拟网络级别。 将删除超出这些限制的 DNS 查询。
