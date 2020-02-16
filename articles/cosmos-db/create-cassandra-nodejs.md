---
title: 快速入门：将 Cassandra API 与 Node.js 配合使用 - Azure Cosmos DB
description: 本快速入门介绍如何配合 Node.js 使用 Azure Cosmos DB Cassandra API 创建配置文件应用程序
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: 429b8845e49158c906c02773f654c9487ff98d1e
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/11/2020
ms.locfileid: "77134771"
---
# <a name="quickstart-build-a-cassandra-app-with-nodejs-sdk-and-azure-cosmos-db"></a>快速入门：使用 Node.js SDK 和 Azure Cosmos DB 构建 Cassandra 应用

> [!div class="op_single_selector"]
> * [.NET](create-cassandra-dotnet.md)
> * [Java](create-cassandra-java.md)
> * [Node.js](create-cassandra-nodejs.md)
> * [Python](create-cassandra-python.md)
>  

在本快速入门中，你将创建一个 Azure Cosmos DB Cassandra API 帐户，并使用从 GitHub 克隆的 Cassandra Node.js 应用创建一个 Cassandra 数据库和一个容器。 Azure Cosmos DB 是一种多模型数据库服务，你可以借助其全球分布和水平缩放功能快速创建和查询文档、表、键/值和图数据库。

## <a name="prerequisites"></a>必备条件

- 具有活动订阅的 Azure 帐户。 [免费创建一个](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。 或者[免费试用 Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) 而无需 Azure 订阅。
- [Node.js 0.10.29+](https://nodejs.org/)。
- [Git](https://www.git-scm.com/downloads)。

## <a name="create-a-database-account"></a>创建数据库帐户

在创建文档数据库之前，需通过 Azure Cosmos DB 创建 Cassandra 帐户。

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>克隆示例应用程序

现在从 GitHub 克隆 Cassandra API 应用，设置连接字符串，并运行应用。 会看到以编程方式处理数据是多么容易。 

1. 打开命令提示符。 创建名为 `git-samples` 的新文件夹。 然后，关闭命令提示符。

    ```bash
    md "C:\git-samples"
    ```

2. 打开 git 终端窗口（例如 git bash）。 使用 `cd` 命令更改为要安装示例应用的新文件夹。

    ```bash
    cd "C:\git-samples"
    ```

3. 运行下列命令以克隆示例存储库。 此命令在计算机上创建示例应用程序的副本。

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-nodejs-getting-started.git
    ```

## <a name="review-the-code"></a>查看代码

此步骤是可选的。 如果有意了解如何通过代码创建数据库资源，可以查看以下代码片段。 这些代码片段全部摘自 `C:\git-samples\azure-cosmos-db-cassandra-nodejs-getting-started` 文件夹中 `uprofile.js` 文件。 否则，可以直接跳转到[更新连接字符串](#update-your-connection-string)。 

* 用户名和密码值是使用 Azure 门户中的连接字符串页设置的。 `path\to\cert` 提供 X509 证书的路径。 

   ```javascript
   var ssl_option = {
        cert : fs.readFileSync("path\to\cert"),
        rejectUnauthorized : true,
        secureProtocol: 'TLSv1_2_method'
        };
   const authProviderLocalCassandra = new cassandra.auth.PlainTextAuthProvider(config.username, config.password);
   ```

* 使用 contactPoint 信息初始化 `client`。 从 Azure 门户中检索 contactPoint。

    ```javascript
    const client = new cassandra.Client({contactPoints: [config.contactPoint], authProvider: authProviderLocalCassandra, sslOptions:ssl_option});
    ```

* `client` 连接到 Azure Cosmos DB Cassandra API。

    ```javascript
    client.connect(next);
    ```

* 创建新的键空间。

    ```javascript
    function createKeyspace(next) {
        var query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter1\' : \'1\' }";
        client.execute(query, next);
        console.log("created keyspace");    
  }
    ```

* 创建新表。

   ```javascript
   function createTable(next) {
    var query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)";
        client.execute(query, next);
        console.log("created table");
   },
   ```

* 插入键/值实体。

    ```javascript
    ...
       {
          query: 'INSERT INTO  uprofile.user  (user_id, user_name , user_bcity) VALUES (?,?,?)',
          params: [5, 'IvanaV', 'Belgaum']
        }
    ];
    client.batch(queries, { prepare: true}, next);
    ```

* 用于获取所有键值的查询。

    ```javascript
   var query = 'SELECT * FROM uprofile.user';
    client.execute(query, { prepare: true}, function (err, result) {
      if (err) return next(err);
      result.rows.forEach(function(row) {
        console.log('Obtained row: %d | %s | %s ',row.user_id, row.user_name, row.user_bcity);
      }, this);
      next();
    });
    ```  
    
* 用于获取键-值的查询。

    ```javascript
    function selectById(next) {
        console.log("\Getting by id");
        var query = 'SELECT * FROM uprofile.user where user_id=1';
        client.execute(query, { prepare: true}, function (err, result) {
        if (err) return next(err);
            result.rows.forEach(function(row) {
            console.log('Obtained row: %d | %s | %s ',row.user_id, row.user_name, row.user_bcity);
        }, this);
        next();
        });
    }
    ```  

## <a name="update-your-connection-string"></a>更新连接字符串

现在返回到 Azure 门户，获取连接字符串信息，并将其复制到应用。 连接字符串使应用能与托管数据库进行通信。

1. 在 [Azure 门户](https://portal.azure.com/)的 Azure Cosmos DB 帐户中，选择“连接字符串”  。 

    使用 ![“复制”按钮](./media/create-cassandra-nodejs/copy.png) 复制最上面的值“联系点”。

    ![在 Azure 门户的连接字符串页中查看并复制“联系点”、“用户名”和“密码”](./media/create-cassandra-nodejs/keys.png)

2. 打开 `config.js` 文件。 

3. 粘贴门户中的“联系点”值，并覆盖第 4 行中的 `<FillMEIN>`。

    第 4 行现在应如下所示 

    `config.contactPoint = "cosmos-db-quickstarts.cassandra.cosmosdb.azure.com:10350"`

4. 复制并粘贴门户中的“用户名”值，并覆盖第 2 行中的 `<FillMEIN>`。

    第 2 行现在应如下所示 

    `config.username = 'cosmos-db-quickstart';`
    
5. 复制并粘贴门户中的“密码”值，并覆盖第 3 行中的 `<FillMEIN>`。

    第 3 行现在应如下所示

    `config.password = '2Ggkr662ifxz2Mg==';`

6. 保存 `config.js` 文件。
    
## <a name="use-the-x509-certificate"></a>使用 X509 证书

1. 从 [https://cacert.omniroot.com/bc2025.crt](https://cacert.omniroot.com/bc2025.crt) 在本地下载 Baltimore CyberTrust 根证书。 使用文件扩展名 `.cer` 重命名该文件。

   证书的序列号为 `02:00:00:b9`，SHA1 指纹为 `d4🇩🇪20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74`。

2. 打开 `uprofile.js` 并更改 `path\to\cert` 以指向新证书。

3. 保存 `uprofile.js`。

## <a name="run-the-nodejs-app"></a>运行 Node.js 应用

1. 在 git 终端窗口中，运行 `npm install` 安装所需的 npm 模块。

2. 运行 `node uprofile.js` 启动 node 应用程序。

3. 通过命令行验证结果是否符合预期。

    ![查看并验证输出](./media/create-cassandra-nodejs/output.png)

    按 CTRL+C 停止执行程序并关闭控制台窗口。 

4. 在 Azure 门户中，打开数据资源管理器  ，以查询、修改和处理这些新数据。 

    ![在数据资源管理器中查看数据](./media/create-cassandra-nodejs/data-explorer.png) 

## <a name="review-slas-in-the-azure-portal"></a>在 Azure 门户中查看 SLA

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>清理资源

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>后续步骤

本快速入门介绍了如何使用 Cassandra API 创建 Azure Cosmos DB 帐户，以及如何运行用于创建 Cassandra 数据库和容器的 Cassandra Node.js 应用。 现在可以将其他数据导入 Azure Cosmos DB 帐户了。 

> [!div class="nextstepaction"]
> [将 Cassandra 数据导入 Azure Cosmos DB](cassandra-import-data.md)


