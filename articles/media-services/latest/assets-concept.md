---
title: 媒体服务中的资产 - Azure | Microsoft Docs
description: 本文介绍何为资产以及 Azure 媒体服务如何使用这些资产。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/08/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: f9a6f0963ce8f45da567bb4f6326e9fcc8f435ef
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140126"
---
# <a name="assets"></a>资产

**资产**包含数字文件（包括视频、音频、图像、缩略图集合、文本轨道和隐藏式字幕文件）以及这些文件的相关元数据。 数字文件在上传到资产中后，即可用于媒体服务编码和流式处理工作流。

资产会被映射到 [Azure 存储帐户](storage-account-concept.md)中的 blob 容器，而资产中的文件会作为块 blob 存储在该容器中。 可以使用存储 SDK 客户端与容器中的资产文件进行交互。

当帐户使用常规用途 v2 (GPv2) 存储时，Azure 媒体服务支持 Blob 层。 借助 GPv2，可以将文件移动到冷存储。 冷存储适合存档不再需要的源文件（例如，编码后的源文件）。

在媒体服务 v3 中，可以从资产或 HTTP URL 创建作业输入。 若要创建可用作作业输入的资产，请参阅[从本地文件创建作业输入](job-input-from-local-file-how-to.md)。

此外，请参阅[媒体服务中的存储帐户](storage-account-concept.md)和[转换和作业](transform-concept.md)。

## <a name="asset-definition"></a>资产定义

下表显示了资产的属性并给出了它们的定义。

|名称|Description|
|---|---|
|id|资源的完全限定的资源 ID。|
|名称|资源的名称。|
|properties.alternateId |资产的备用 ID。|
|properties.assetId |资产 ID。|
|properties.container |资产 blob 容器的名称。|
|properties.created |资产的创建日期。|
|properties.description|资产说明。|
|properties.lastModified |上次修改资产的日期。|
|properties.storageAccountName |存储帐户的名称。|
|properties.storageEncryptionFormat |资产加密格式。 无格式或 MediaStorageEncryption。|
|type|资源的类型。|

有关完整定义，请参阅[资产](https://docs.microsoft.com/rest/api/media/assets)。

## <a name="filtering-ordering-paging"></a>筛选、排序、分页

媒体服务支持对资产使用以下 OData 查询选项： 

* $filter 
* $orderby 
* $top 
* $skiptoken 

运算符说明：

* Eq = 等于
* Ne = 不等于
* Ge = 大于或等于
* Le = 小于或等于
* Gt = 大于
* Lt = 小于

### <a name="filteringordering"></a>筛选/排序

下表显示了这些选项如何应用于资产属性： 

|名称|筛选器|顺序|
|---|---|---|
|id|||
|名称|支持：Eq、Gt、Lt|支持：升序和降序|
|properties.alternateId |支持：Eq||
|properties.assetId |支持：Eq||
|properties.container |||
|properties.created|支持：Eq、Gt、Lt| 支持：升序和降序|
|properties.description |||
|properties.lastModified |||
|properties.storageAccountName |||
|properties.storageEncryptionFormat | ||
|type|||

以下 C# 示例按创建日期筛选：

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

### <a name="pagination"></a>分页

已启用的四个排序顺序均支持分页。 页面大小当前为 1000。

> [!TIP]
> 应始终使用下一个链接来枚举集合，而不依赖特定的页面大小。

如果查询响应包含许多项，则服务将返回一个“\@odata.nextLink”属性来获取下一页结果。 这可用于逐页浏览整个结果集。 无法配置页面大小。 

如果在逐页浏览集合时创建或删除资产，则会在返回的结果中反映此更改（如果这些更改位于集合中尚未下载的部分）。 

以下 C# 示例显示如何枚举帐户中的所有资产。

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

有关 REST 示例，请参阅 [Assets - List](https://docs.microsoft.com/rest/api/media/assets/list)（资产 - 列出）

## <a name="storage-side-encryption"></a>存储端加密

若要保护静态资产，应通过存储端加密对资产进行加密。 下表显示了存储端加密在媒体服务中的工作方式：

|加密选项|Description|媒体服务 v2|媒体服务 v3|
|---|---|---|---|
|媒体服务存储加密|AES-256 加密，媒体服务管理的密钥|支持<sup>(1)</sup>|不支持<sup>(2)</sup>|
|[静态数据的存储服务加密](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|由 Azure 存储提供的服务器端加密，由 Azure 或客户管理的密钥|支持|支持|
|[存储客户端加密](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|由 Azure 存储提供的客户端加密，由 Key Vault 中的客户管理的密钥|不支持|不支持|

<sup>1</sup> 虽然媒体服务确实支持处理明文形式（未经过任何形式的加密）的内容，但不建议这样做。

<sup>2</sup> 在媒体服务 v3 中，仅当资产是使用媒体服务 v2 创建的时才支持存储加密（AES-256 加密）以实现向后兼容性。 这意味着 v3 会处理现有的存储加密资产，但不会允许创建新资产。

## <a name="next-steps"></a>后续步骤

[流式传输文件](stream-files-dotnet-quickstart.md)
