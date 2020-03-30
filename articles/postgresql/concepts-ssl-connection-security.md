---
title: SSL - Azure Database for PostgreSQL（单一服务器）
description: 有关如何为 Azure Database for PostgreSQL（单一服务器）配置 SSL 连接的说明和信息。
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 303da4dcb68a79e69254f6610afc0003bf0aa22c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476994"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql---single-server"></a>在 Azure Database for PostgreSQL - 单一服务器中配置 SSL 连接

Azure Database for PostgreSQL 倾向于使用安全套接字层 (SSL) 将客户端应用程序连接到 PostgreSQL 服务。 通过在数据库服务器与客户端应用程序之间强制实施 SSL 连接，可以加密服务器与应用程序之间的数据流，有助于防止“中间人”攻击。

默认情况下，PostgreSQL 数据库服务配置为需要 SSL 连接。 如果客户端应用程序不支持 SSL 连接，则可以选择禁用 SSL。

## <a name="enforcing-ssl-connections"></a>强制实施 SSL 连接

对于通过 Azure 门户或 CLI 预配的所有 Azure Database for PostgreSQL 服务器，强制实施 SSL 连接是默认启用的。 

同样，在 Azure 门户中，用户服务器的“连接字符串”设置中预定义了连接字符串，该字符串中包含以通用语言使用 SSL 连接到数据库服务器所需的参数。 SSL 参数因连接器而异，例如“ssl=true”、“sslmode=require”或“sslmode=required”，以及其他变体。

## <a name="configure-enforcement-of-ssl"></a>配置强制实施 SSL

（可选）可以禁用强制实施 SSL 连接。 Microsoft Azure 建议始终启用“强制实施 SSL 连接”**** 设置，以增强安全性。

### <a name="using-the-azure-portal"></a>使用 Azure 门户

访问 Azure Database for PostgreSQL 服务器，并单击“连接安全性”****。 使用切换按钮来启用或禁用“强制实施 SSL 连接”**** 设置。 然后，单击 **“保存”**。

![连接安全性 - 禁用强制实施 SSL](./media/concepts-ssl-connection-security/1-disable-ssl.png)

可以通过在“概述”**** 页中查看“SSL 强制实施状态”**** 指示器来确认设置。

### <a name="using-azure-cli"></a>使用 Azure CLI

可以通过在 Azure CLI 中分别使用 `Enabled` 或 `Disabled` 值来启用或禁用“ssl-enforcement”**** 参数。

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>确保应用程序或框架支持 SSL 连接

某些使用 PostgreSQL 作为其数据库服务的应用程序框架在安装期间默认不启用 SSL。 如果 PostgreSQL 服务器强制实施 SSL 连接，但应用程序未配置 SSL，则应用程序可能无法连接到数据库服务器。 请查阅应用程序文档，了解如何启用 SSL 连接。

## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>需要证书验证才可启用 SSL 连接性的应用程序

在某些情况下，应用程序需要具备从受信任的证书颁发机构 (CA) 证书文件 (.cer) 生成的本地证书文件才能实现安全连接。 连接到 PostgreSQL 服务器的 Azure 数据库的证书位于https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem。 下载证书文件并将其保存到您的首选位置。

### <a name="connect-using-psql"></a>使用 psql 进行连接

下面的示例演示如何使用 psql 命令行实用程序连接到 PostgreSQL 服务器。 使用`sslmode=verify-full`连接字符串设置强制实施 SSL 证书验证。 将本地证书文件路径传递给参数`sslrootcert`。

以下命令是 psql 连接字符串的示例：

```shell
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> 确认传递给 `sslrootcert` 的值与你保存的证书的文件路径匹配。

## <a name="next-steps"></a>后续步骤

在 [Azure Database for PostgreSQL 的连接库](concepts-connection-libraries.md)中查看各种应用程序连接选项。
