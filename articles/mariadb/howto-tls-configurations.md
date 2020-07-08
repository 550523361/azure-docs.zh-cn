---
title: TLS 配置-Azure 门户-Azure Database for MariaDB
description: 了解如何使用 Azure 门户为 Azure Database for MariaDB 设置 TLS 配置
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: 9ebaca8720eb923c2373c45ac605146bbcdbc4a8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85847994"
---
# <a name="configuring-tls-settings-in-azure-database-for-mariadb-using-azure-portal"></a>使用 Azure 门户在 Azure Database for MariaDB 中配置 TLS 设置

本文介绍如何将 Azure Database for MariaDB 服务器配置为强制执行连接所使用的最低 TLS 版本，并拒绝与已配置的最低 TLS 版本相比，使网络安全性更低的所有连接。

可以通过为其数据库服务器设置最小 TLS 版本来强制 TLS 版本连接到其 Azure Database for MariaDB。 例如，如果将最小 TLS 设置版本设置为 TLS 1.0，则意味着服务器将允许使用 TLS 1.0、1.1 和 1.2 + 的客户端连接。 或者，将此选项设置为1.2 意味着仅允许使用 TLS 1.2 + 的客户端进行连接，并且将拒绝 TLS 1.0 和 TLS 1.1 的所有连接。

## <a name="prerequisites"></a>先决条件

若要完成本操作指南，需要：

* [Azure Database for MariaDB](quickstart-create-mariaDB-server-database-using-azure-portal.md)

## <a name="set-tls-configurations-for-azure-database-for-mariadb"></a>为 Azure Database for MariaDB 设置 TLS 配置

请按照以下步骤设置 MariaDB server 最低 TLS 版本：

1. 在 [Azure 门户](https://portal.azure.com/)中，选择现有 Azure Database for MariaDB 服务器。

1. 在 "MariaDB 服务器" 页上的 "**设置**" 下，单击 "**连接安全性**" 以打开 "连接安全配置" 页。

1. 在 "**最低 TLS 版本**" 中，选择 " **1.2** " 以拒绝 MariaDB 服务器的 tls 版本低于 tls 1.2 的连接。

    ![Azure Database for MariaDB TLS 配置](./media/howto-tls-configurations/tls-configurations.png)

1. 单击“保存”以保存更改。

1. 通知会确认已成功启用连接安全设置。

    ![Azure Database for MariaDB TLS 配置成功](./media/howto-tls-configurations/tls-configurations-success.png)

## <a name="next-steps"></a>后续步骤

了解[如何创建有关指标的警报](howto-alert-metric.md)