---
title: Azure 数据工厂实体的命名规则-版本1
description: 描述数据工厂实体的命名规则。
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 2b6dc5b89b8c5d691b19e9e3368d805eb59b1db1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87082854"
---
# <a name="rules-for-naming-azure-data-factory-entities"></a>Azure 数据工厂实体的命名规则
> [!NOTE]
> 本文适用于数据工厂版本 1。 如果使用当前版本的数据工厂服务，请参阅[数据工厂中的命名规则](../naming-rules.md)。

下表提供了数据工厂项目的命名规则。

| 名称 | 名称唯一性 | 验证检查 |
|:--- |:--- |:--- |
| Data Factory |在 Microsoft Azure 内唯一。 名称不区分大小写，即 `MyDF` 和 `mydf` 指的是同一个数据工厂。 |<ul><li>一个数据工厂绑定到一个 Azure 订阅。</li><li>对象名称必须以字母或数字开头，并且只能包含字母、数字和短划线 (-) 字符。</li><li>每个短划线 (-) 字符的前后必须紧接字母或数字。 容器名称中不允许使用连续短划线。</li><li>名称长度为 3-63 个字符。</li></ul> |
| 链接服务/表/管道 |在数据工厂中唯一。 名称不区分大小写。 |<ul><li>表名称的最大字符数：260。</li><li>对象名称必须以字母、数字或下划线 (_) 开头。</li><li>不允许使用以下字符：“.”、“+”、“?”、“/”、“<”、“>”、“ * ”、“%”、“&”、“:”、“\\”</li></ul> |
| 资源组 |在 Microsoft Azure 内唯一。 名称不区分大小写。 |<ul><li>最大字符数：1000。</li><li>名称可包含字母、数字和以下字符：“-”、“_”、“,”和“.”</li></ul> |

