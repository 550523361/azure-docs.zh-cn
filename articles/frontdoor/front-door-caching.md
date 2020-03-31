---
title: Azure 前门 - 缓存 |微软文档
description: 本文可帮助您了解 Azure 前门如何监视后端的运行状况
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: d4fed878e2c0b1430e963f43743fd772493d3270
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471738"
---
# <a name="caching-with-azure-front-door"></a>使用 Azure 前门进行缓存
下列文档详细说明了在具有已启用缓存的路由规则时 Front Door 的行为。 前门是一个现代的内容交付网络 （CDN），因此，随着动态站点加速和负载平衡，它还支持缓存行为，就像任何其他 CDN 一样。

## <a name="delivery-of-large-files"></a>大型文件交付
Azure 前门提供大文件，文件大小上没有上限。 Front Door 使用一种被称作对象区块的技术。 请求大型文件时，Front Door 会从后端检索文件的较小部分。 收到完整或字节范围的文件请求后，Front Door 环境以 8 MB 的区块为单位从后端请求文件。

</br>区块到达 Front Door 环境后，会进行缓存并立即提供给用户。 然后，Front Door 会并行预提取下一个区块。 此预提取可确保内容始终先于用户一个区块，从而减少延迟。 此过程将继续，直到整个文件下载完成（如果已请求），所有字节范围都可用（如果已请求），或客户端终止连接。

</br>有关字节范围请求的详细信息，请阅读 [RFC 7233](https://web.archive.org/web/20171009165003/http://www.rfc-base.org/rfc-7233.html)。
Front Door 会在收到任何区块时将其缓存，因此整个文件无需在 Front Door 缓存中进行缓存。 后续的文件或字节范围请求将从该缓存提供。 如果未缓存部分区块，将通过预提取从后端请求区块。 该项优化取决于后端能否支持字节范围请求；如果后端不支持字节范围请求，则此优化无效。

## <a name="file-compression"></a>文件压缩
Front Door 可动态压缩边缘内容，从而更快响应客户端。 所有文件都可进行压缩。 但是，文件必须是适合压缩列表的 MIME 类型。 Front Door 当前禁止更改此列表。 当前列表为：</br>
- "application/eot"
- "application/font"
- "application/font-sfnt"
- "application/javascript"
- "application/json"
- "application/opentype"
- "application/otf"
- "application/pkcs7-mime"
- "application/truetype"
- "application/ttf",
- "application/vnd.ms-fontobject"
- "application/xhtml+xml"
- "application/xml"
- "application/xml+rss"
- "application/x-font-opentype"
- "application/x-font-truetype"
- "application/x-font-ttf"
- "application/x-httpd-cgi"
- "application/x-mpegurl"
- "application/x-opentype"
- "application/x-otf"
- "application/x-perl"
- "application/x-ttf"
- "application/x-javascript"
- "font/eot"
- "font/ttf"
- "font/otf"
- "font/opentype"
- "image/svg+xml"
- "text/css"
- "text/csv"
- "text/html"
- "text/javascript"
- "text/js", "text/plain"
- "text/richtext"
- "text/tab-separated-values"
- "text/xml"
- "text/x-script"
- "text/x-component"
- "text/x-java-source"

此外，文件大小必须介于 1 KB 与 8 MB 之间（不含首尾）。

这些配置文件支持以下压缩编码：
- [Gzip （GNU zip）](https://en.wikipedia.org/wiki/Gzip)
- [布罗特利](https://en.wikipedia.org/wiki/Brotli)

如果请求同时支持 gzip 和 Brotli 压缩，则 Brotli 压缩优先。</br>
如果资产请求指定进行压缩，但该请求导致缓存缺失，则 Front Door 将直接在 POP 服务器上压缩资产。 此后，将从缓存提供压缩的文件。 通过 transfer-encoding: chunked 返回所生成的项。

## <a name="query-string-behavior"></a>查询字符串行为
借助 Front Door，可控制如何对包含查询字符串的 Web 请求缓存文件。 在包含查询字符串的 Web 请求中，查询字符串是问号 (?) 后出现的请求部分。 查询字符串可以包含一个或多个键值对，其中字段名称和其值由等号 (=) 分隔。 每个键值对由与号 (&) 分隔。 例如，`http://www.contoso.com/content.mov?field1=value1&field2=value2` 。 如果请求的查询字符串中有多个键值对，其顺序并不重要。
- **忽略查询字符串**：默认模式。 在此模式下，Front Door 将来自请求者的查询字符串传递到第一个请求上的后端并缓存该资产。 由 Front Door 环境处理的资产的所有后续请求都将忽略查询字符串，直到所缓存的资产过期。

- **缓存每个唯一的 URL**：在此模式下，包含唯一 URL 的每个请求（包括查询字符串）将视为具有其自己的缓存的唯一资产。 例如，后端对 `www.example.ashx?q=test1` 的请求做出的响应将缓存在 Front Door 环境中，并为具有同一查询字符串的后续缓存返回该响应。 `www.example.ashx?q=test2` 的请求将作为具有其自己的生存时间设置的单独资产来缓存。

## <a name="cache-purge"></a>缓存清除
Front Door 将缓存资产，直到资产的生存时间 (TTL) 过期。 资产的 TTL 到期后，如果客户端请求资产，Front Door 环境将检索具有最新更新的资产副本，以满足客户端请求并存储刷新缓存。
</br>确保用户始终获取资产的最新副本的最佳做法是针对每次更新将资产版本化，并将其发布为新 URL。 Front Door 将立即检索用于下一个客户端请求的新资产。 有时候可能希望从所有边缘节点清除缓存的内容，并强制其全部检索新的已更新资产。 这可能是由于 Web 应用程序获得了更新，或快速更新的资产包含不正确的信息。

</br>选择要从边缘节点清除的资产。 若要清除所有资产，单击“全部清除”复选框。 否则，请在“路径”文本框中键入要清除的每个资产的路径。 路径支持以下格式。
1. **单路径清除**：通过指定具有文件扩展名的资产的完整路径（没有协议和域）清除单个资产，例如/图片/斯特拉斯堡.png;
2. **通配符清除**：星号 (\*) 可用作通配符。 清除具有 /\*的终结点下的所有文件夹、子文件夹和文件，或通过指定文件夹后跟 /（\*例如/图片/）\*来清除特定文件夹下的所有子文件夹和文件。
3. **根域清除**：清除路径中具有“/”的终结点的根。

Front Door 的缓存清除不区分大小写。 此外，它们不区分查询字符串，这表示清除 URL 时将一并清除其查询字符串的所有变体。 

## <a name="cache-expiration"></a>缓存到期
按下列标题顺序来确定项目在缓存中的存储时间：</br>
1. 缓存控制：s-maxage=\<秒>
2. 缓存控制：最大年龄=\<秒>
3. 过期：http-date \<>

缓存控制响应标头指示不会缓存响应，例如缓存控制：专用、缓存控制：无缓存和缓存控制：不受存储。 但是，如果 POP 中多个正在进行的请求针对的是同一 URL，则它们可共享响应。 如果不存在缓存控制，则默认行为是 AFD 将缓存资源的 X 时间，其中 X 在 1 到 3 天之间随机选取。

## <a name="request-headers"></a>请求标头

使用缓存时，以下请求标头不转发到后端。
- Content-Length
- Transfer-Encoding

## <a name="next-steps"></a>后续步骤

- 了解如何[创建 Front Door](quickstart-create-front-door.md)。
- 了解 [Front Door 的工作原理](front-door-routing-architecture.md)。
