---
title: 对 Azure Cosmos DB 使用 Robomongo
description: 了解如何配合使用 Robomongo 与 Azure Cosmos DB:API for MongoDB 帐户
keywords: robomongo
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: sngun
ms.openlocfilehash: 78f0158c9a80a60717b81b4788531c7efd979111
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2018
ms.locfileid: "52863799"
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a>配合使用 Robomongo 与 Azure Cosmos DB:API for MongoDB 帐户
若要使用 Robomongo 连接到 Azure Cosmos DB:API for MongoDB 帐户，必须：

* 下载并安装 [Robomongo](https://robomongo.org/)
* 具有 Azure Cosmos DB:API for MongoDB 帐户的[连接字符串](connect-mongodb-account.md)信息

## <a name="connect-using-robomongo"></a>使用 Robomongo 进行连接
若要将 Azure Cosmos DB:API for MongoDB 帐户添加到 Robomongo MongoDB 连接，请执行以下步骤。

1. 使用[此处](connect-mongodb-account.md)的说明检索 Azure Cosmos DB: API for MongoDB 帐户连接信息。

    ![连接字符串边栏选项卡的屏幕截图](./media/mongodb-robomongo/connectionstringblade.png)
2. 运行 Robomongo.exe

3. 单击“文件”下的“连接”按钮以管理连接。 然后，在“MongoDB 连接”窗口中单击“创建”，这会打开“连接设置”窗口。

4. 在“连接设置”窗口中，选择名称。 然后，从步骤 1 的连接信息中找到**主机**和**端口**，并将其分别输入到“地址”和“端口”中。

    ![Robomongo 管理连接的屏幕截图](./media/mongodb-robomongo/manageconnections.png)
5. 在“身份验证”选项卡上，单击“执行身份验证”。 然后，输入数据库（默认值为 Admin）、用户名和密码。
**用户名**和**密码**可以在步骤 1 的连接信息中找到。

    ![Robomongo 身份验证选项卡的屏幕截图](./media/mongodb-robomongo/authentication.png)
6. 在“SSL”选项卡上，选中“使用 SSL 协议”，然后将“身份验证方法”更改为“自签名证书”。

    ![Robomongo SSL 选项卡的屏幕截图](./media/mongodb-robomongo/SSL.png)
7. 最后，单击“测试”验证是否能够连接，并单击“保存”。

## <a name="next-steps"></a>后续步骤
* 探索 Azure Cosmos DB:API for MongoDB [示例](mongodb-samples.md)。
