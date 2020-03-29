---
title: Azure CDN 的标准规则引擎中的匹配条件 |微软文档
description: Azure 内容交付网络 （Azure CDN） 的标准规则引擎中匹配条件的参考文档。
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: 425266e2a7ca42bb17ca598ddfc2f2b86591f32e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "74900179"
---
# <a name="match-conditions-in-the-standard-rules-engine-for-azure-cdn"></a>Azure CDN 的标准规则引擎中的匹配条件

在 Azure 内容交付网络 （Azure CDN）[的标准规则引擎](cdn-standard-rules-engine.md)中，规则由一个或多个匹配条件和操作组成。 本文详细介绍了可在 Azure CDN 的标准规则引擎中使用匹配条件的详细说明。

规则的第一部分是匹配条件或匹配条件集。 在 Azure CDN 的标准规则引擎中，每个规则最多可以有四个匹配条件。 匹配条件标识为其执行已定义操作的特定类型的请求。 如果使用多个匹配条件，则匹配条件将使用 AND 逻辑组合在一起。

例如，可以使用匹配条件：

- 基于特定 IP 地址、国家或地区筛选请求。
- 按标头信息筛选请求。
- 筛选来自移动设备或桌面设备的请求。

## <a name="match-conditions"></a>匹配条件

以下匹配条件可用于 Azure CDN 的标准规则引擎。 

### <a name="device-type"></a>设备类型 

标识从移动设备或桌面设备发出的请求。  

#### <a name="required-fields"></a>Required fields

运算符 | 支持的值
---------|----------------
等于，不等于 | 移动式、桌面式

### <a name="http-version"></a>HTTP 版本

根据请求的 HTTP 版本标识请求。

#### <a name="required-fields"></a>Required fields

运算符 | 支持的值
---------|----------------
等于，不等于 | 2.0， 1.1， 1.0， 0.9， 全部

### <a name="request-cookies"></a>请求 Cookie

根据传入请求中的 Cookie 信息标识请求。

#### <a name="required-fields"></a>Required fields

Cookie 名称 | 运算符 | 曲奇值 | 案例转换
------------|----------|--------------|---------------
String | [标准操作员列表](#standard-operator-list) | 字符串， int | 没有转换为大写到小写

#### <a name="key-information"></a>重要信息

- 指定 Cookie 名称时，不能使用通配符值（\*包括星号 （ ）;您必须使用确切的 Cookie 名称。
- 每个匹配条件的实例只能指定一个 Cookie 名称。
- Cookie 名称比较不区分大小写。
- 要指定多个 Cookie 值，请使用每个 Cookie 值之间的单个空格。 
- Cookie 值可以利用通配符值。
- 如果未指定通配符值，则只有完全匹配满足此匹配条件。 例如，"值"将匹配"价值"，而不是"值 1"。 

### <a name="post-argument"></a>后参数

根据为请求中使用的 POST 请求方法定义的参数标识请求。 

#### <a name="required-fields"></a>Required fields

参数名称 | 运算符 | 参数值 | 案例转换
--------------|----------|----------------|---------------
String | [标准操作员列表](#standard-operator-list) | 字符串， int | 没有转换为大写到小写

### <a name="query-string"></a>查询字符串

标识包含特定查询字符串参数的请求。 此参数设置为与特定模式匹配的值。 请求 URL 中的查询字符串参数（例如参数 **=值**）确定是否满足此条件。 此匹配条件按名称标识查询字符串参数，并接受一个或多个值作为参数值。

#### <a name="required-fields"></a>Required fields

运算符 | 查询字符串 | 案例转换
---------|--------------|---------------
[标准操作员列表](#standard-operator-list) | 字符串， int | 没有转换为大写到小写

### <a name="remote-address"></a>远程地址

根据请求者的位置或 IP 地址标识请求。

#### <a name="required-fields"></a>Required fields

运算符 | 支持的值
---------|-----------------
Any | 空值
地理匹配 | 国家/地区代码
IP 匹配 | IP 地址（空间分隔）
没有任何 | 空值
不是地理匹配 | 国家/地区代码
不是 IP 匹配 | IP 地址（空间分隔）

#### <a name="key-information"></a>重要信息

- 使用 CIDR 表示法。
- 要指定多个 IP 地址和 IP 地址块，请在值之间使用单个空格：
  - **IPv4 示例**： *1.2.3.4 10.20.30.40*匹配从地址 1.2.3.4 或 10.20.30.40 到达的任何请求。
  - **IPv6**示例 *：1：2：3：4：4：5：6：8 10：20：30：40：60：70：80*匹配从任意一个地址 1：2：3：4：5：5：6：8 或 10：20：30：40：60：70：80 到达的任何请求。
- IP 地址块的语法为 IP 基址后跟正斜杠和前缀大小。 例如：
  - **IPv4 示例**： *5.5.5.64/26*匹配从地址 5.5.5.64 到 5.5.5.127 到达的任何请求。
  - **IPv6 示例**： *1：2：3：/48*匹配从地址 1：2：3：0：0：0：0 到 1：2：3：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ffff：ff

### <a name="request-body"></a>请求正文

根据请求正文中显示的特定文本标识请求。

#### <a name="required-fields"></a>Required fields

运算符 | 请求正文 | 案例转换
---------|--------------|---------------
[标准操作员列表](#standard-operator-list) | 字符串， int | 没有转换为大写到小写

### <a name="request-header"></a>请求标头

标识在请求中使用特定标头的请求。

#### <a name="required-fields"></a>Required fields

标头名称 | 运算符 | 标头值 | 案例转换
------------|----------|--------------|---------------
String | [标准操作员列表](#standard-operator-list) | 字符串， int | 没有转换为大写到小写

### <a name="request-method"></a>请求方法

标识使用指定请求方法的请求。

#### <a name="required-fields"></a>Required fields

运算符 | 支持的值
---------|----------------
等于，不等于 | 获取， 发布， 放， 删除， 头， 选项， 跟踪

#### <a name="key-information"></a>重要信息

- 只有 GET 请求方法才能在 Azure CDN 中生成缓存的内容。 其他所有请求方法均通过网络代理。 

### <a name="request-protocol"></a>请求协议

标识使用所用指定协议的请求。

#### <a name="required-fields"></a>Required fields

运算符 | 支持的值
---------|----------------
等于，不等于 | HTTP、HTTPS

### <a name="request-url"></a>请求 URL

标识与指定 URL 匹配的请求。

#### <a name="required-fields"></a>Required fields

运算符 | 请求 URL | 案例转换
---------|-------------|---------------
[标准操作员列表](#standard-operator-list) | 字符串， int | 没有转换为大写到小写

#### <a name="key-information"></a>重要信息

- 使用此规则条件时，请确保包含协议信息。 例如： *https://www.\<yourdomain\>.com*.

### <a name="url-file-extension"></a>URL 文件扩展名

标识在请求 URL 中的文件名中包含指定文件扩展名的请求。

#### <a name="required-fields"></a>Required fields

运算符 | 分机 | 案例转换
---------|-----------|---------------
[标准操作员列表](#standard-operator-list) | 字符串， int | 没有转换为大写到小写

#### <a name="key-information"></a>重要信息

- 对于扩展，不包括前导期;例如，使用*html*而不是 *.html*。

### <a name="url-file-name"></a>URL 文件名

标识在请求 URL 中包含指定文件名的请求。

#### <a name="required-fields"></a>Required fields

运算符 | 文件名 | 案例转换
---------|-----------|---------------
[标准操作员列表](#standard-operator-list) | 字符串， int | 没有转换为大写到小写

#### <a name="key-information"></a>重要信息

- 要指定多个文件名，请用单个空格分隔每个文件名。 

### <a name="url-path"></a>URL 路径

标识在请求 URL 中包含指定路径的请求。

#### <a name="required-fields"></a>Required fields

运算符 | “值” | 案例转换
---------|-------|---------------
[标准操作员列表](#standard-operator-list) | 字符串， int | 没有转换为大写到小写

#### <a name="key-information"></a>重要信息

- 文件名值可以利用通配符值。 例如，每个文件名模式可以包含一个或多个星号 (*)，其中每个星号与包含一个或多个字符的序列匹配。

## <a name="reference-for-rules-engine-match-conditions"></a>规则引擎匹配条件参考

### <a name="standard-operator-list"></a>标准操作员列表

对于接受标准运算符列表中的值的规则，以下运算符有效：

- Any
- 等于 
- 包含 
- 开头为 
- 结尾为 
- 小于
- 小于或等于
- 大于
- 大于或等于
- 没有任何
- 不包含
- 不以 
- 不以 
- 不小于
- 不小于或等于
- 不大于
- 不大于或等于

对于数字运算符（如*小于*和*大于或等于*），使用的比较基于长度。 在这种情况下，匹配条件中的值应为等于要比较的长度的整数。 

## <a name="next-steps"></a>后续步骤

- [Azure CDN 概述](cdn-overview.md)
- [标准规则引擎参考](cdn-standard-rules-engine-reference.md)
- [标准规则引擎中的操作](cdn-standard-rules-engine-actions.md)
- [使用标准规则引擎强制执行 HTTPS](cdn-standard-rules-engine.md)
