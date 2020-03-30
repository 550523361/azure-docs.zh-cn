---
title: 使用 v3 API 进行开发
titleSuffix: Azure Media Services
description: 了解在使用媒体服务 v3 进行开发时适用于实体和 API 的规则。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/21/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: eacdfe8211c97e75b6609f5e11b681f84ae55846
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79472078"
---
# <a name="develop-with-media-services-v3-apis"></a>使用媒体服务 v3 API 进行开发

作为开发者，可以利用媒体服务 [REST API](https://docs.microsoft.com/rest/api/media/) 或客户端库，与 REST API 交互，以轻松创建、管理和维护自定义媒体工作流。 [媒体服务 v3](https://aka.ms/ams-v3-rest-sdk) API 基于 OpenAPI 规范（以前称为 Swagger）。

本文介绍在使用媒体服务 v3 进行开发时适用于实体和 API 的规则。

## <a name="accessing-the-azure-media-services-api"></a>访问 Azure 媒体服务 API

若要有权访问媒体服务资源和媒体服务 API，必须先进行身份验证。 媒体服务支持[基于 Azure 活动目录 （Azure AD） 的](../../active-directory/fundamentals/active-directory-whatis.md)身份验证。 下面是两个常用的身份验证选项：
 
* **服务主体身份验证**：用于对服务进行身份验证（例如：Web 应用、函数应用、逻辑应用、API 和微服务）。 常常使用这种身份验证方法的应用程序是运行守护程序服务、中间层服务或计划作业的应用程序。 例如，此方法适用于应始终位于通过服务主体连接到媒体服务的中间层中的 Web 应用。
* **用户身份验证**：用于验证使用应用与媒体服务资源交互的人员。 交互式应用应先提示用户输入用户凭据。 例如，授权用户用来监视编码作业或实时传送视频流的管理控制台应用程序。

媒体服务 API 要求发出 REST API 请求的用户或应用有权访问媒体服务帐户资源，并有权使用“参与者”或“所有者”角色。******** 可以使用“读者”角色访问 API，但只有“获取”或“列出”操作可用。************有关详细信息，请参阅[媒体服务帐户的基于角色的访问控制](rbac-overview.md)。

如果不创建服务主体，可以考虑使用 Azure 资源的托管标识通过 Azure 资源管理器来访问媒体服务 API。 若要详细了解 Azure 资源的托管标识，请参阅[什么是 Azure 资源的托管标识？](../../active-directory/managed-identities-azure-resources/overview.md)。

### <a name="azure-ad-service-principal"></a>Azure AD 服务主体

如果创建 Azure AD 应用和服务主体，该应用必须位于其自身的租户中。 创建应用后，向应用授予对媒体服务帐户的“参与者”或“所有者”角色访问权限。********

如果你不确定自己是否有权创建 Azure AD 应用，请参阅[所需的权限](../../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)。

在下图中，数字表示按时间顺序的请求流：

![在 Web API 中使用 AAD 进行中层应用身份验证](./media/use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

1. 中间层应用请求获取包含以下参数的 Azure AD 访问令牌：  

   * Azure AD 租户终结点。
   * 媒体服务资源 URI。
   * REST 媒体服务的资源 URI。
   * Azure AD 应用值：客户端 ID和客户端机密。

   若要获取所有所需值，请参阅[使用 Azure CLI 访问 Azure 媒体服务 API](access-api-cli-how-to.md)。

2. Azure AD 访问令牌发送到中间层。
4. 中间层使用 Azure AD 令牌向 Azure 媒体 REST API 发送请求。
5. 中间层获取媒体服务返回的数据。

### <a name="samples"></a>示例

参阅以下示例，其中演示了如何连接 Azure AD 服务主体：

* [通过 REST 进行连接](media-rest-apis-with-postman.md)  
* [通过 Java 进行连接](configure-connect-java-howto.md)
* [使用 .NET 进行连接](configure-connect-dotnet-howto.md)
* [使用 Node.js 进行连接](configure-connect-nodejs-howto.md)
* [通过 Python 进行连接](configure-connect-python-howto.md)

## <a name="naming-conventions"></a>命名约定

Azure 媒体服务 v3 资源名称（例如，资产、作业、转换）需遵循 Azure 资源管理器命名约束。 根据 Azure 资源管理器的要求，资源名称始终必须唯一。 因此，可以为资源名称使用任何唯一的标识符字符串（例如，GUID）。

媒体服务资源名称不能包含“<”、“>”、“%”、“&”、“:”、“&#92;”、“?”、“/”、“*”、“+”、“.”、单引号或任何控制字符。 允许其他所有字符。 资源名称的最大长度为 260 个字符。

有关 Azure 资源管理器命名的详细信息，请参阅[命名要求](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource)和[命名约定](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging)。

### <a name="names-of-filesblobs-within-an-asset"></a>资产中的文件/Blob 的名称

资产中的文件/blob 名称必须符合 [Blob 名称要求](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata)和 [NTFS 名称要求](https://docs.microsoft.com/windows/win32/fileio/naming-a-file)。 符合这些要求的原因是可以将文件从 Blob 存储复制到本地 NTFS 磁盘进行处理。

## <a name="long-running-operations"></a>长时间运行的操作

在 Azure 媒体服务 [swagger 文件](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/streamingservice.json)中标有 `x-ms-long-running-operation` 的操作是长时间运行的操作。 

有关如何跟踪 Azure 异步操作的详细信息，请参阅[异步操作](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations#monitor-status-of-operation)。

媒体服务具有以下长时间运行的操作：

* [创建实时事件](https://docs.microsoft.com/rest/api/media/liveevents/create)
* [更新实时事件](https://docs.microsoft.com/rest/api/media/liveevents/update)
* [删除实时事件](https://docs.microsoft.com/rest/api/media/liveevents/delete)
* [启动实时事件](https://docs.microsoft.com/rest/api/media/liveevents/start)
* [停止实时事件](https://docs.microsoft.com/rest/api/media/liveevents/stop)

  使用 `removeOutputsOnStop` 参数可以在停止事件时删除所有关联的实时输出。  
* [重置实时事件](https://docs.microsoft.com/rest/api/media/liveevents/reset)
* [创建实时输出](https://docs.microsoft.com/rest/api/media/liveevents/create)
* [删除实时输出](https://docs.microsoft.com/rest/api/media/liveevents/delete)
* [创建流式处理终结点](https://docs.microsoft.com/rest/api/media/streamingendpoints/create)
* [更新流式处理终结点](https://docs.microsoft.com/rest/api/media/streamingendpoints/update)
* [删除流式处理终结点](https://docs.microsoft.com/rest/api/media/streamingendpoints/delete)
* [启动流式处理终结点](https://docs.microsoft.com/rest/api/media/streamingendpoints/start)
* [停止流式处理终结点](https://docs.microsoft.com/rest/api/media/streamingendpoints/stop)
* [缩放流式处理终结点](https://docs.microsoft.com/rest/api/media/streamingendpoints/scale)

成功提交长时间运行的操作后，你会收到“202 已接受”；必须使用返回的操作 ID 轮询操作的完成状态。

[跟踪异步 Azure 操作](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations)一文深入说明了如何通过响应中返回的值跟踪异步 Azure 操作的状态。

一个给定的实时事件或其任何关联的实时输出仅支持一个长时间运行的操作。 启动某个长时间运行的操作后，必须先完成该操作，才能针对同一个实时事件或任何关联的实时输出启动后续的长时间运行的操作。 对于包含多个实时输出的实时事件，必须等待针对一个实时输出的长时间运行的操作完成，然后才能对另一个实时输出触发长时间运行的操作。 

## <a name="sdks"></a>SDK

> [!NOTE]
> Azure 媒体服务 v3 SDK 不保证是线程安全的。 在开发多线程应用时，应添加自己的线程同步逻辑以保护客户端，或对每个线程使用新的 AzureMediaServicesClient 对象。 你还应该注意由代码提供给客户端的可选对象引入的多线程问题（如 .NET 中的 HttpClient 实例）。

|SDK 中 IsInRole 中的声明|参考|
|---|---|
|[.NET SDK](https://aka.ms/ams-v3-dotnet-sdk)|[.NET 参考](https://aka.ms/ams-v3-dotnet-ref)|
|[Java SDK](https://aka.ms/ams-v3-java-sdk)|[Java 参考](https://aka.ms/ams-v3-java-ref)|
|[Python SDK](https://aka.ms/ams-v3-python-sdk)|[Python 参考](https://aka.ms/ams-v3-python-ref)|
|[Node.js SDK](https://aka.ms/ams-v3-nodejs-sdk) |[Node.js 参考](/javascript/api/overview/azure/mediaservices/management)| 
|[Go SDK](https://aka.ms/ams-v3-go-sdk) |[Go 参考](https://aka.ms/ams-v3-go-ref)|
|[Ruby SDK](https://aka.ms/ams-v3-ruby-sdk)||

### <a name="see-also"></a>请参阅

- [包含媒体服务事件的 EventGrid .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/)
- [媒体服务事件的定义](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/eventgrid/data-plane/Microsoft.Media/stable/2018-01-01/MediaServices.json)

## <a name="azure-media-services-explorer"></a>Azure 媒体服务浏览器

[Azure 媒体服务浏览器](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) 是可供希望了解媒体服务的 Windows 客户使用的工具。 AMSE 是一个 Winforms/C# 应用程序，用于通过媒体服务对 VOD 和实时内容进行上传、下载、编码和流式传输。 AMSE 工具适用于希望在不编写任何代码的情况下测试媒体服务的客户。 对于希望使用媒体服务进行开发的客户，可以为其提供 AMSE 代码作为资源。

AMSE 是一个开源项目，由社区提供支持（可以将问题报告给 https://github.com/Azure/Azure-Media-Services-Explorer/issues)）。 此项目采用了 [Microsoft 开放源代码行为准则](https://opensource.microsoft.com/codeofconduct/)。 有关详细信息，请参阅[行为准则常见问题解答](https://opensource.microsoft.com/codeofconduct/faq/)；若有其他任何问题或意见，请联系 opencode@microsoft.com。

## <a name="filtering-ordering-paging-of-media-services-entities"></a>媒体服务实体的筛选、排序和分页

请参阅 [Azure 媒体服务实体的筛选、排序和分页](entities-overview.md)。

## <a name="ask-questions-give-feedback-get-updates"></a>提出问题、提供反馈、获取更新

查看 [Azure 媒体服务社区](media-services-community.md)文章，了解可以提出问题、提供反馈和获取有关媒体服务的更新的不同方法。

## <a name="see-also"></a>请参阅

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

## <a name="next-steps"></a>后续步骤

* [使用 Java 连接到媒体服务](configure-connect-java-howto.md)
* [使用 .NET 连接到媒体服务](configure-connect-dotnet-howto.md)
* [使用 Node.js 连接到媒体服务](configure-connect-nodejs-howto.md)
* [使用 Python 连接到媒体服务](configure-connect-python-howto.md)
