---
title: 管理 Azure 媒体服务中的资产
titleSuffix: Azure Media Services
description: 可以在资产中输入媒体（例如，通过上传或实时引入）、输出媒体（从作业输出）以及从中发布媒体（用于流式处理）。 本主题概述如何创建新资产和上传文件。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/26/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 9136fd702fad5c12a8ec97a68ff8a592a203d7d2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "80582200"
---
# <a name="manage-assets"></a>管理资产

在 Azure 媒体服务的[资产](https://docs.microsoft.com/rest/api/media/assets)中可执行以下操作： 

* 将媒体文件上传到资产；
* 将实时传送流引入和存档到资产；
* 将分析作业编码的结果输出到资产，
* 发布媒体以进行流式处理； 
* 从资产中下载文件。

本主题概述如何将文件上传到资产，以及如何执行其他一些常见操作。 此外，提供了代码示例和相关主题的链接。

## <a name="prerequisite"></a>先决条件 

在开始开发之前，请查看：

* [概念](concepts-overview.md)
* [使用媒体服务 v3 API 进行开发](media-services-apis-overview.md)（包括有关访问 API、命名约定等的信息） 

## <a name="upload-media-files-into-an-asset"></a>将媒体文件上传到资产

将数字文件上传到存储中并与资产关联后，即可将其用于媒体服务编码、流式传输和分析内容工作流。 一个常见的媒体服务工作流是上传、编码和流式传输文件。 本部分概述常规步骤。

1. 使用媒体服务 v3 API 创建新的“输入”资产。 此操作在与媒体服务帐户关联的存储帐户中创建一个容器。 API 返回容器名称（例如 `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`）。

    如果已有一个要与资产关联的 Blob 容器，则可以在创建资产时指定容器名称。 媒体服务目前仅支持容器根中的 Blob，而不支持在文件名中输入路径。 因此，使用文件名“input.mp4”的容器是有效的。 但是，使用文件名“videos/inputs/input.mp4”的容器是无效的。

    可以使用 Azure CLI 直接上传到订阅中你有权访问的任何存储帐户和容器。

    容器名称必须唯一，并遵循存储命名准则。 该名称不一定要遵循媒体服务资产容器名称（资产 GUID）的格式。

    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. 获取具有读写权限的 SAS URL，用于将数字文件上传到资产容器。

    可以使用媒体服务 API [列出资产容器 URL](https://docs.microsoft.com/rest/api/media/assets/listcontainersas)。

    AssetContainerSas.listContainerSas 采用 [ListContainerSasInput](https://docs.microsoft.com/rest/api/media/assets/listcontainersas#listcontainersasinput) 参数，在此参数中可以设置 `expiryTime`。 时间应设置为小于 24 小时。

    [ListContainerSasInput](https://docs.microsoft.com/rest/api/media/assets/listcontainersas#listcontainersasinput) 返回多个 SAS URL，因为每个存储帐户有两个存储帐户密钥。 存储帐户有两个密钥，因为它有助于进行故障转移并无缝轮换存储帐户密钥。 第一个 SAS URL 表示第一个存储帐户密钥，第二个 SAS URL 表示第二个密钥。
3. 使用 Azure 存储 API 或 SDK（例如，[存储 REST API](../../storage/common/storage-rest-api-auth.md) 或 [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)）将文件上传到资产容器。
4. 使用媒体服务 v3 API 创建用于处理“输入”资产的转换和作业。 有关详细信息，请参阅[转换和作业](transform-concept.md)。
5. 从“输出”资产流式传输内容。

### <a name="create-a-new-asset"></a>创建新资产

> [!NOTE]
> 属于日期/时间类型的资产的属性始终采用 UTC 格式。

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

有关 REST 示例，请参阅[使用 REST 创建资产](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples)示例。

该示例演示如何创建**请求正文**，在该正文中可以指定说明、容器名称、存储帐户和其他有用信息。

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

### <a name="see-also"></a>另请参阅

* [从本地文件创建作业输入](job-input-from-local-file-how-to.md)
* [从 HTTPS URL 创建作业输入](job-input-from-http-how-to.md)

## <a name="ingest-and-archive-live-streams-into-an-asset"></a>将实时传送流引入和存档到资产

在媒体服务中，[实时输出](https://docs.microsoft.com/rest/api/media/liveoutputs)对象类似于数字视频录制器，它会捕获实时流并将其记录到媒体服务帐户中的某个资产内。 记录的内容保存在[资产](https://docs.microsoft.com/rest/api/media/assets)资源定义的容器中。

有关详细信息，请参阅：

* [使用云 DVR](live-event-cloud-dvr.md)
* [流式传输实时流的教程](stream-live-tutorial-with-api.md)

## <a name="output-the-results-of-a-job-to-an-asset"></a>将作业结果输出到资产

在媒体服务中，在处理视频（例如，编码或分析）时，需要创建一个输出[资产](assets-concept.md)来存储[作业](transforms-jobs-concept.md)的结果。

有关详细信息，请参阅：

* [对视频进行编码](encoding-concept.md)
* [从本地文件创建作业输入](job-input-from-local-file-how-to.md)

## <a name="publish-an-asset-for-streaming"></a>发布资产以进行流式处理

若要发布资产以进行流式处理，需要创建一个[流定位符](streaming-locators-concept.md)。 流定位符需要知道所要发布的资产名称。 

有关详细信息，请参阅：

[教程：使用媒体服务 v3 上传、编码和流式传输视频](stream-files-tutorial-with-api.md)

## <a name="download-results-of-a-job-from-an-output-asset"></a>从输出资产中下载作业结果

然后，可以使用媒体服务和存储 API 将作业结果下载到本地文件夹。 

请参阅[下载文件](download-results-howto.md)示例。

## <a name="filtering-ordering-paging"></a>筛选、排序、分页

请参阅[媒体服务实体的筛选、排序、分页](entities-overview.md)。

## <a name="next-steps"></a>后续步骤

查看演示如何上传、编码、分析、流式传输和按需流式处理的完整代码示例： 

* [Java](https://docs.microsoft.com/samples/azure-samples/media-services-v3-java/azure-media-services-v3-samples-using-java/)； 
* [.NET](https://docs.microsoft.com/samples/azure-samples/media-services-v3-dotnet/azure-media-services-v3-samples-using-net/)； 
* [REST](https://docs.microsoft.com/samples/azure-samples/media-services-v3-rest-postman/azure-media-services-postman-collection/)。
