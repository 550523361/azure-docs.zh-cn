---
title: 从 MySQL Workbench 连接到 Azure Database for MariaDB
description: 本快速入门介绍如何使用 MySQL Workbench 连接到 Azure Database for MariaDB 并查询其中的数据。
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.custom: mvc
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: b3a4d00381361b5299e86b959d9775318ae81e88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998217"
---
# <a name="azure-database-for-mariadb-use-mysql-workbench-to-connect-and-query-data"></a>Azure Database for MariaDB：使用 MySQL Workbench 进行连接并查询数据
本快速入门演示如何使用 MySQL Workbench 应用程序连接到 Azure Database for MariaDB。 

## <a name="prerequisites"></a>先决条件
此快速入门使用以下任意指南中创建的资源作为起点：
- [使用 Azure 门户创建 Azure Database for MariaDB 服务器](./quickstart-create-mariadb-server-database-using-azure-portal.md)
- [使用 Azure CLI 创建 Azure Database for MariaDB 服务器](./quickstart-create-mariadb-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a>安装 MySQL Workbench
在计算机上从 [MySQL 网站](https://dev.mysql.com/downloads/workbench/)下载并安装 MySQL Workbench。

## <a name="get-connection-information"></a>获取连接信息
获取连接到 Azure Database for MariaDB 所需的连接信息。 需要完全限定的服务器名称和登录凭据。

1. 登录到 [Azure 门户](https://portal.azure.com/)。

2. 在 Azure 门户的左侧菜单中，单击“所有资源”，然后搜索已创建的服务器（例如 mydemoserver）。

3. 单击服务器名称。

4. 从服务器的“概览”面板中记下“服务器名称”和“服务器管理员登录名”。 如果忘记了密码，也可通过此面板来重置密码。
 ![Azure Database for MariaDB 服务器名称](./media/connect-workbench/1_server-overview-name-login.png)

## <a name="connect-to-server-using-mysql-workbench"></a>使用 MySQL Workbench 连接到服务器 
若要使用 MySQL Workbench 连接到 Azure Database for MariaDB 服务器，请使用以下步骤：

1.  启动计算机上的 MySQL Workbench 应用程序。 

2.  在“设置新连接”对话框的“参数”选项卡上，输入以下信息：

    ![设置新连接](./media/connect-workbench/2-setup-new-connection.png)

    | **设置** | 建议的值 | 字段说明 |
    |---|---|---|
    |   连接名称 | 演示连接 | 指定此连接的标签。 |
    | 连接方法 | 标准 (TCP/IP) | 标准 (TCP/IP) 就足够了。 |
    | 主机名 | 服务器名称 | 指定此前在创建 Azure Database for MariaDB 时使用过的服务器名称值。 所显示的示例服务器为 mydemoserver.mariadb.database.azure.com。 请使用完全限定的域名 (\*.mariadb.database.azure.com)，如示例中所示。 如果记不起服务器名称，请按上一部分的步骤操作，以便获取连接信息。  |
    | 端口 | 3306 | 在连接到 Azure Database for MariaDB 时，始终使用端口 3306。 |
    | 用户名 |  服务器管理员登录名 | 键入此前在创建 Azure Database for MariaDB 时提供的服务器管理员登录用户名。 示例用户名为 myadmin@mydemoserver。 如果记不起用户名，请按上一部分的步骤操作，以便获取连接信息。 格式为 username@servername。
    | 密码 | 你的密码 | 单击“在保管库中存储...”按钮来保存密码。 |

3.   单击“测试连接”以测试是否所有参数均已正确配置。 

4.   单击“确定”保存连接。 

5.   在“MySQL 连接”列表中，单击与服务器对应的磁贴并等待建立连接。

        将打开一个新的 SQL 选项卡，该选项卡包含一个可在其中键入查询的空白编辑器。
    
        > [!NOTE]
        > 默认情况下，SSL 连接安全性是必需的，并且在 Azure Database for MariaDB 服务器上强制执行。 虽然通常不需要对 SSL 证书进行额外的配置，MySQL Workbench 即可连接到服务器，但建议将 SSL CA 证书与 MySQL Workbench 绑定。 如果需要禁用 SSL，请访问 Azure 门户，然后单击“连接安全性”页来禁用“强制实施 SSL 连接”切换按钮。

## <a name="create-table-insert-read-update-and-delete-data"></a>创建表，插入、读取、更新和删除数据
1. 将示例 SQL 代码复制并粘贴到一个空白 SQL 选项卡中，以阐释一些示例数据。

    此代码将创建名为 quickstartdb 的空数据库，然后创建名为清单的示例表。 它会插入一些行，然后读取这些行。 它通过更新语句更改数据，并再次读取这些行。 最后，它删除一个行，并再次读取这些行。
    
    ```sql
    -- Create a database
    -- DROP DATABASE IF EXISTS quickstartdb;
    CREATE DATABASE quickstartdb;
    USE quickstartdb;
    
    -- Create a table and insert rows
    DROP TABLE IF EXISTS inventory;
    CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
    INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
    INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
    INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    
    -- Read
    SELECT * FROM inventory;
    
    -- Update
    UPDATE inventory SET quantity = 200 WHERE id = 1;
    SELECT * FROM inventory;
    
    -- Delete
    DELETE FROM inventory WHERE id = 2;
    SELECT * FROM inventory;
    ```

    此屏幕快照显示 SQL Workbench 中的一个 SQL 代码示例以及运行该示例代码后的输出。
    
    ![运行示例 SQL 代码的 MySQL Workbench SQL 选项卡](media/connect-workbench/3-workbench-sql-tab.png)

2. 若要运行示例 SQL 代码，请单击“SQL 文件”选项卡工具栏中的闪电图标。
3. 请注意页面中间“结果网格”部分中的三个选项卡式结果。 
4. 请注意页面底部的“输出”列表。 显示有每个命令的状态。 

现已使用 MySQL Workbench 连接到 Azure Database for MariaDB，并已使用 SQL 语言查询数据。

<!--
## Next steps
> [!div class="nextstepaction"]
> [Migrate your database using Export and Import](./concepts-migrate-import-export.md)
-->