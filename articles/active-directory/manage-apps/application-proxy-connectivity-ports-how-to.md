---
title: 如何打开应用程序代理应用程序所需的防火墙端口 | Microsoft Docs
description: 了解要使 Azure AD 应用程序代理正常工作需要打开哪些端口
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 710abfd8dd56d3f8276accec397b21d8d60fd3c5
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203311"
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>如何打开应用程序代理应用程序所需的防火墙端口

若要查看所需端口的完整列表及每个端口的功能，请参阅[应用程序代理文档](application-proxy-add-on-premises-application.md)的先决条件部分。 请注意，应用程序代理仅使用出站端口。

还可以通过从本地网络打开[连接器端口测试工具](https://aadap-portcheck.connectorporttest.msappproxy.net/)，检查是否已打开所需的全部端口。 绿色复选标记越多表示复原能力越强。 

## <a name="app-proxy-regions"></a>应用代理区域

我们会提供一种方法，告知哪些区域对你来说需要为绿色。 目前，请确保这些区域全部为绿色。 不管处在哪个区域，美国中部都不可或缺。

为了确保工具提供正确的结果，请务必：

-   在安装有连接器的服务器的浏览器上打开该工具。

-   确保适用于连接器的任何代理或防火墙也应用到此页。 可以在 Internet Explorer 上通过转到“设置” -&gt;“Internet 选择” -&gt;“连接” -&gt;“LAN 设置”完成此操作。 在此页上，会看到“为 LAN 使用代理服务器”字段。 选中此框，将代理地址输入“地址”字段。

## <a name="next-steps"></a>后续步骤
[了解 Azure AD 应用程序代理连接器](application-proxy-connectors.md)
