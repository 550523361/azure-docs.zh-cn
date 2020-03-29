---
title: 使用 REST 将文件上载到 Azure 媒体服务 v3 帐户 |微软文档
description: 了解如何通过创建和上传资产将媒体内容加入媒体服务。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 38d46978e37ead59deb17a86f643df041452e497
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705762"
---
# <a name="upload-files-into-a-media-services-v3-account-using-rest"></a>使用 REST 将文件上载到媒体服务 v3 帐户

在媒体服务中，可以将数字文件上传到与资产关联的 blob容器中。 [资产](https://docs.microsoft.com/rest/api/media/operations/asset)实体可以包含视频、音频、图像、缩略图集合、文本轨道和隐藏字幕文件（以及有关这些文件的元数据）。 将文件上传到资产的容器中后，相关内容即安全地存储在云中供后续处理和流式传输。

本文介绍如何使用 REST 上传本地文件。

## <a name="prerequisites"></a>先决条件

若要完成本主题中所述的步骤，必须：

- 查看[资产概念](assets-concept.md)。
- [为 Azure 媒体服务 REST API 调用配置 Postman。](media-rest-apis-with-postman.md)
    
    确保遵循[获取 Azure AD 令牌](media-rest-apis-with-postman.md#get-azure-ad-token)主题中的最后一步。 

## <a name="create-an-asset"></a>创建资产

本部分介绍如何创建新的资产。

1. 选择 **"资产** -> **创建或更新资产**"。
2. 按“发送”。****

    ![创建资产](./media/upload-files/postman-create-asset.png)

可看到包含有关新创建资产的信息的**响应**。

## <a name="get-a-sas-url-with-read-write-permissions"></a>获取具有读写权限的 SAS URL 

本部分介绍如何获取通过所创建资产生成的 SAS URL。 所创建的 SAS URL 具有读写权限，可用于将数字文件上传到资产容器。

1. 选择 **"资产** -> **列表"资产 URL。**
2. 按“发送”。****

    ![上传文件](./media/upload-files/postman-create-sas-locator.png)

可看到包含有关资产 URL 的信息的**响应**。 复制第一个 URL 并使用它上传文件。

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>使用上传 URL 将文件上传到 Blob 存储

使用 Azure 存储 API 或 SDK，例如，[存储 REST API](../../storage/common/storage-rest-api-auth.md) 或 [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)。

## <a name="next-steps"></a>后续步骤

[教程：基于 URL 对远程文件进行编码并流式传输视频 - REST](stream-files-tutorial-with-rest.md)
