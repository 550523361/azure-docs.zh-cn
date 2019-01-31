---
title: include 文件
description: include 文件
services: active-directory
author: daveba
ms.service: active-directory
ms.subservice: msi
ms.topic: include
ms.date: 05/31/2018
ms.author: daveba
ms.custom: include file
ms.openlocfilehash: 7cdd2ce44cfa24b2b6bad2bb45260299bc8eda5f
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2019
ms.locfileid: "55252364"
---
| 类别 | 限制 |
| --- | --- |
| 用户分配的托管标识 | <ul><li>创建用户分配的托管标识时，只能使用字母数字字符（0-9、a-z、A-Z）和连字符 (-)。 另外，为了确保能够正常分配给 VM/VMSS，名称长度不得超过 24 个字符。</li><li>如果使用的是托管标识虚拟机扩展，支持的限制为 32 个用户分配的托管标识。  如果不使用托管标识虚拟机扩展，支持的限制为 512 个用户分配的标识。</li>|

