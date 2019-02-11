---
title: Azure 服务总线管理库 | Microsoft Docs
description: 在 .NET 中管理服务总线命名空间和消息实体。
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/05/2018
ms.author: aschhab
ms.openlocfilehash: d6c2cd813e96b40fc9f95785a1fd28e324a0437b
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2019
ms.locfileid: "54850943"
---
# <a name="service-bus-management-libraries"></a>服务总线管理库

Azure 服务总线管理库可以动态预配服务总线命名空间和实体。 这样可以实现复杂的部署和消息方案，并能以编程方式确定要预配的实体。 这些库目前可用于 .NET。

## <a name="supported-functionality"></a>支持的功能

* 命名空间创建、更新、删除
* 队列创建、更新、删除
* 主题创建、更新、删除
* 订阅创建、更新、删除

## <a name="prerequisites"></a>先决条件

若要开始使用服务总线管理库，必须使用 Azure Active Directory (Azure AD) 服务进行身份验证。 Azure AD 要求身份验证为服务主体，并且该主体提供对 Azure 资源的访问权限。 有关创建服务主体的信息，请参阅以下文章之一：  

* [使用 Azure 门户创建可访问资源的 Active Directory 应用程序和服务主体](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [使用 Azure PowerShell 创建服务主体来访问资源](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [使用 Azure CLI 创建服务主体来访问资源](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

这些教程提供 `AppId`（客户端 ID）、`TenantId` 和 `ClientSecret`（身份验证密钥），这些都用于管理库进行的身份验证。 若要对资源组运行命令，必须拥有“所有者”权限。

## <a name="programming-pattern"></a>编程模式

操作任何服务总线资源的模式都遵循通用协议：

1. 使用 **Microsoft.IdentityModel.Clients.ActiveDirectory** 库从 Azure AD 获取令牌：
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```
2. 创建 `ServiceBusManagementClient` 对象：

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. 将 `CreateOrUpdate` 参数设置为指定值：

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. 执行调用：

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a>后续步骤

* [.NET 管理示例](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Microsoft.Azure.Management.ServiceBus API 参考](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
