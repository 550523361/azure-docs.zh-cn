---
title: “基于角色的访问控制”预览版 - Azure 事件中心 | Microsoft Docs
description: 本文提供有关 Azure 事件中心基于角色的访问控制的信息。
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 05/21/2019
ms.author: shvija
ms.openlocfilehash: ae970b9612154a6463c4bf44a65da71a20c81635
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "65978314"
---
# <a name="active-directory-role-based-access-control-preview"></a>Active Directory 基于角色的访问控制（预览版）

Microsoft Azure 基于 Azure Active Directory (Azure AD) 针对资源和应用程序提供了集成的访问控制管理功能。 使用 Azure AD，可以专门为基于 Azure 的应用程序管理用户帐户和应用程序，还可以将现有的 Active Directory 基础结构与 Azure AD 联合起来以实现公司范围内的单一登录，该单一登录还涵盖了 Azure 资源和 Azure 托管的应用程序。 然后，可将那些 Azure AD 用户和应用程序标识分配给全局和特定于服务的角色以授予对 Azure 资源的访问权限。

对于 Azure 事件中心，通过 Azure 门户和 Azure 资源管理 API 对命名空间和所有相关资源的管理已使用*基于角色的访问控制* (RBAC) 模型进行了保护。 针对运行时操作的 RBAC 目前为公共预览版。 

使用 Azure AD RBAC 的应用程序不需要处理 SAS 规则和密钥，也不需要处理特定于事件中心的任何其他访问令牌。 客户端应用与 Azure AD 进行交互来建立身份验证上下文，并获取事件中心的访问令牌。 使用需要交互式登录的域用户帐户，应用程序从不直接处理任何凭据。

## <a name="event-hubs-roles-and-permissions"></a>事件中心角色和权限
Azure 授权访问的事件中心命名空间提供了以下内置 RBAC 角色：

[事件中心数据所有者 （预览）](../role-based-access-control/built-in-roles.md#service-bus-data-owner)角色可让数据访问的事件中心命名空间和其实体 （队列、 主题、 订阅和筛选器）

>[!IMPORTANT]
> 我们之前已支持向“所有者”  或“参与者”  角色添加托管标识。 但是，不再授予“所有者”  和“参与者”  角色的数据访问权限。 如果使用是“所有者”  或“参与者”  角色，请切换到使用“事件中心数据所有者”  角色。


## <a name="use-event-hubs-with-an-azure-ad-domain-user-account"></a>将事件中心与 Azure AD 域用户帐户一起使用

以下部分介绍创建和运行一个示例应用程序（该应用程序提示交互式 Azure AD 用户进行登录）所需的步骤，如何向该用户帐户授予事件中心访问权限，以及如何使用该标识来访问事件中心。 

此介绍描述了一个简单的控制台应用程序，[其代码位于 GitHub 上](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Rbac/EventHubsSenderReceiverRbac/)

### <a name="create-an-active-directory-user-account"></a>创建 Active Directory 用户帐户

这第一个步骤是可选的。 每个 Azure 订阅都会自动与一个 Azure Active Directory 租户进行配对，如果你具有某个 Azure 订阅的访问权限，则你的用户帐户已经注册。 这意味着你只能使用自己的帐户。 

如果仍然希望为此方案创建特定帐户，请[执行这些步骤](../automation/automation-create-aduser-account.md)。 必须拥有所需的权限才能在 Azure Active Directory 租户中创建帐户，对于较大的企业方案，这可能不现实。

### <a name="create-an-event-hubs-namespace"></a>创建事件中心命名空间

接下来，[创建事件中心命名空间](event-hubs-create.md)。 

在创建命名空间后，在门户上导航到其“访问控制(IAM)”  页面，然后单击“添加角色分配”  将 Azure AD 用户帐户添加到“所有者”角色。 如果你使用自己的用户帐户并且已创建了命名空间，则已获得“所有者”角色。 若要向角色添加一个不同的帐户，请在“添加权限”  面板的“选择”  字段中搜索 Web 应用程序的名称，然后单击该条目。 然后单击“保存”  。 用户帐户现在已具有对事件中心命名空间和对之前创建的事件中心的访问权限。
 
### <a name="register-the-application"></a>注册应用程序

在可以运行示例应用程序之前，将它注册到 Azure AD 中并批准允许应用程序以其身份访问事件中心的许可提示。 

由于示例应用程序是一个控制台应用程序，因此必须注册一个本机应用程序并将 **Microsoft.EventHub** 的 API 权限添加到“必需的权限”集。 本机应用程序在 Azure AD 中还需要有一个充当标识符的 redirect-URI  ，该 URI 不需要是网络目的地。 对于此示例请使用 `https://eventhubs.microsoft.com`，因为示例代码已使用了该 URI。

[此教程](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md)中介绍了详细的注册步骤。 请按照那些步骤注册一个**本机**应用，然后按照更新说明将 **Microsoft.EventHub** API 添加到必需的权限。 执行那些步骤时，请记下 **TenantId** 和 **ApplicationId**，因为到时要使用这些值来运行应用程序。

### <a name="run-the-app"></a>运行应用

在可以运行示例前，请编辑 App.config 文件并根据方案设置以下值：

- `tenantId`：设置为 **TenantId** 值。
- `clientId`：设置为 **ApplicationId** 值。 
- `clientSecret`：如果希望使用客户端机密进行登录，请在 Azure AD 中创建它。 此外，请使用 Web 应用或 API 而非本机应用。 另外，请在之前创建的命名空间中将该应用添加到“访问控制(IAM)”  下。
- `eventHubNamespaceFQDN`：设置为新创建的事件中心命名空间的完全限定 DNS 名称，例如 `example.servicebus.windows.net`。
- `eventHubName`：设置为已创建的事件中心的名称。
- 执行前面的步骤时在应用中指定的重定向 URI。
 
运行该控制台应用程序时，系统会提示选择一个方案，请通过键入相应的编号并按 ENTER 来选择“交互式用户登录”  。 应用程序会显示一个登录窗口，要求同意访问事件中心，然后使用登录标识通过该服务来运行整个发送/接收方案。

此应用使用 `ServiceAudience.EventHubsAudience` 作为令牌受众。 使用受众无法作为常量使用的其他语言或 SDK 时，要使用的正确值为 `https://eventhubs.azure.net/`。

## <a name="next-steps"></a>后续步骤

有关事件中心的详细信息，请访问以下链接：

* 使用 [事件中心教程](event-hubs-dotnet-standard-getstarted-send.md)
* [事件中心常见问题解答](event-hubs-faq.md)
* [事件中心定价详细信息](https://azure.microsoft.com/pricing/details/event-hubs/)
* [使用事件中心的示例应用程序](https://github.com/Azure/azure-event-hubs/tree/master/samples)
