---
title: BizTalk 服务中的颁发者名称和颁发者密钥 | Microsoft Docs
description: 了解如何为 BizTalk 服务中的 Service Bus 或 Access Control (ACS) 检索颁发者名称和颁发者密钥。 MABS，WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 5eac98ec88b960956c9a0931673e67f530aef8da
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2019
ms.locfileid: "57542176"
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk 服务：颁发者名称和颁发者密钥

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk 服务使用 Service Bus 颁发者名称和颁发者密钥以及 Access Control 颁发者名称和颁发者密钥。 具体而言：

| 任务 | 哪个颁发者名称和颁发者密钥 |
| --- | --- |
| 从 Visual Studio 部署应用程序 |Access Control 颁发者名称和颁发者密钥 |
| 配置 Azure BizTalk 服务门户 |Access Control 颁发者名称和颁发者密钥 |
| 在 Visual Studio 中使用 BizTalk 适配器服务创建 LOB 中继 |Service Bus 颁发者名称和颁发者密钥 |

本主题列出了检索颁发者名称和颁发者密钥的步骤。 

## <a name="access-control-issuer-name-and-issuer-key"></a>Access Control 颁发者名称和颁发者密钥
Access Control 颁发者名称和颁发者密钥由以下各项使用：

* 在 Visual Studio 中创建 Azure BizTalk 服务应用程序：若要成功部署到 Azure BizTalk 服务应用程序在 Visual Studio 中的，输入 Access Control 颁发者名称和颁发者密钥。 
* Azure BizTalk 服务门户：当你创建 BizTalk 服务，并打开 BizTalk 服务门户时，Access Control 颁发者名称和颁发者密钥自动注册为使用相同的访问控制值部署。

### <a name="get-the-access-control-issuer-name-and-issuer-key"></a>获取访问控制颁发者名称和颁发者密钥

若要使用 ACS 进行身份验证，并获取颁发者名称和颁发者密钥值，总体步骤包括：

1. 安装 [Azure PowerShell cmdlet](https://azure.microsoft.com/documentation/articles/powershell-install-configure/)。
2. 添加 Azure 帐户：`Add-AzureAccount`
3. 返回订阅名称：`get-azuresubscription`
4. 选择订阅：`select-azuresubscription <name of your subscription>` 
5. 创建新的命名空间：`new-azuresbnamespace <name for the service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`

    示例：`new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`
      
5. 创建新 ACS 命名空间时（这可能需要几分钟），会在连接字符串中列出颁发者名称和颁发者密钥值： 

    ```
    Name                  : biztalksbnamespace
    Region                : South Central US
    DefaultKey            : abcdefghijklmnopqrstuvwxyz
    Status                : Active
    CreatedAt             : 10/18/2016 9:36:30 PM
    AcsManagementEndpoint : https://biztalksbnamespace-sb.accesscontrol.windows.net/
    ServiceBusEndpoint    : https://biztalksbnamespace.servicebus.windows.net/
    ConnectionString      : Endpoint=sb://biztalksbnamespace.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue=abcdefghijklmnopqrstuvwxyz
    NamespaceType         : Messaging
    ```

总结：  
颁发者名称 = SharedSecretIssuer  
颁发者密钥 = SharedSecretKey

在 [New-azuresbnamespace](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azuresbnamespace) cmdlet 中了解详细信息。 

## <a name="service-bus-issuer-name-and-issuer-key"></a>Service Bus 颁发者名称和颁发者密钥
Service Bus 颁发者名称和颁发者密钥由 BizTalk 适配器服务使用。 在 Visual Studio 中的 BizTalk 服务项目中，使用 BizTalk 适配器服务连接到本地业务线 (LOB) 系统。 若要连接，请创建 LOB 中继并输入 LOB 系统的详细信息。 在执行此操作时，还可以输入 Service Bus 颁发者名称和颁发者密钥。

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>检索 Service Bus 颁发者名称和颁发者密钥
1. 登录到 [Azure 门户](https://portal.azure.com)。
2. 搜索“服务总线”，并选择命名空间。 
3. 打开“共享访问策略”属性，选择策略，并查看“连接字符串”以获取名称和密钥值。  

## <a name="next"></a>下一步
其他 Azure BizTalk 服务主题：

* [安装 Azure BizTalk 服务 SDK](https://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [教程：Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [如何开始使用 Azure BizTalk 服务 SDK](https://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Azure BizTalk 服务](https://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>另请参阅
* [如何：使用 ACS 管理服务配置服务标识](https://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [BizTalk 服务：开发人员、 基本、 标准和高级版图表](https://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk 服务：预配](https://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk 服务：设置状态图表](https://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk 服务：仪表板、 监视和缩放选项卡](https://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk 服务：备份和还原](https://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk 服务：限制](https://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

