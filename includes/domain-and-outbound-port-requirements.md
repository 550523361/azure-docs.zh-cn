---
title: include 文件
description: include 文件
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: abnarain
ms.openlocfilehash: 6af6795fefb41f1d8f2b56e0aa1fb367fc18cee2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "74559301"
---
| 域名                  | 出站端口 | 说明                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | 自承载集成运行时连接到 Azure 数据工厂中的数据移动服务时需要此端口。 |
| `*.frontend.clouddatahub.net` | 443            | 自承载集成运行时连接到数据工厂服务时需要此端口。 |
| `download.microsoft.com`    | 443            | 自承载集成运行时下载更新时需要此端口。 如果已禁用自动更新，则可以跳过对此域的配置。 |
| `*.core.windows.net`          | 443            | 使用[分阶段复制](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy)功能时，由自承载集成运行时用来连接到 Azure 存储帐户。 |
| `*.database.windows.net`      | 1433           | 仅当从/向 Azure SQL 数据库或 Azure SQL 数据仓库复制时才为必需项，否则为可选项。 在不打开端口 1433 的情况下，使用暂存复制功能将数据复制到 SQL 数据库或 SQL 数据仓库。 |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | 仅当从或复制到 Azure Data Lake Store 时是必需的，否则为可选。 |
