---
title: 使用 Ruby 连接到 Azure Database for PostgreSQL
description: 本快速入门提供了一个 Ruby 代码示例，你可以使用它来连接到 Azure Database for PostgreSQL 并查询其中的数据。
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 02/28/2018
ms.openlocfilehash: 6748f168624a20e17491a2f84b63b966ce5ad4c6
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2018
ms.locfileid: "53539277"
---
# <a name="azure-database-for-postgresql-use-ruby-to-connect-and-query-data"></a>Azure Database for PostgreSQL：使用 Ruby 连接和查询数据
本快速入门演示了如何使用 [Ruby](https://www.ruby-lang.org) 应用程序连接到 Azure Database for PostgreSQL。 同时还介绍了如何使用 SQL 语句在数据库中查询、插入、更新和删除数据。 本文中的步骤假定你熟悉如何使用 Ruby 进行开发，但不熟悉如何使用 Azure Database for PostgreSQL。

## <a name="prerequisites"></a>先决条件
此快速入门使用以下任意指南中创建的资源作为起点：
- [创建 DB - 门户](quickstart-create-server-database-portal.md)
- [创建 DB - Azure CLI](quickstart-create-server-database-azure-cli.md)

## <a name="install-ruby"></a>安装 Ruby
在自己的计算机上安装 Ruby。 

### <a name="windows"></a>Windows
- 下载并安装最新版本的 [Ruby](https://rubyinstaller.org/downloads/)。
- 在 MSI 安装程序的完成屏幕上，选中“运行 'ridk install' 以安装 MSYS2 和开发工具链”框。 然后单击“完成”，启动下一安装程序。
- 此时会启动 RubyInstaller2 for Windows 安装程序。 键入 2，安装 MSYS2 存储库更新。 完成并返回到安装提示符处以后，关闭命令窗口。
- 从“开始”菜单启动新的命令提示符 (cmd)。
- 测试 Ruby 安装 `ruby -v`，查看已安装的版本。
- 测试 Gem 安装 `gem -v`，查看已安装的版本。
- 运行命令 `gem install pg`，使用 Gem 生成适用于 Ruby 的 PostgreSQL 模块。

### <a name="macos"></a>MacOS
- 运行命令 `brew install ruby`，使用 Homebrew 安装 Ruby。 如需更多安装选项，请参阅 Ruby [安装文档](https://www.ruby-lang.org/en/documentation/installation/#homebrew)
- 测试 Ruby 安装 `ruby -v`，查看已安装的版本。
- 测试 Gem 安装 `gem -v`，查看已安装的版本。
- 运行命令 `gem install pg`，使用 Gem 生成适用于 Ruby 的 PostgreSQL 模块。

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- 通过运行命令 `sudo apt-get install ruby-full` 安装 Ruby。 如需更多安装选项，请参阅 Ruby [安装文档](https://www.ruby-lang.org/en/documentation/installation/)。
- 测试 Ruby 安装 `ruby -v`，查看已安装的版本。
- 通过运行命令 `sudo gem update --system` 安装 Gem 的最新更新。
- 测试 Gem 安装 `gem -v`，查看已安装的版本。
- 通过运行命令 `sudo apt-get install build-essential` 安装 gcc、make 和其他生成工具。
- 通过运行命令 `sudo apt-get install libpq-dev` 安装 PostgreSQL 库。
- 运行命令 `sudo gem install pg`，使用 Gem 生成 Ruby pg 模块。

## <a name="run-ruby-code"></a>运行 Ruby 代码 
- 将代码保存到文件扩展名为 .rb 的文本文件中，再将该文件保存到项目文件夹（例如 `C:\rubypostgres\read.rb` 或 `/home/username/rubypostgres/read.rb`）中
- 若要运行此代码，请启动命令提示符或 bash shell。 将目录更改为项目文件夹 `cd rubypostgres`，然后键入命令 `ruby read.rb` 来运行应用程序。

## <a name="get-connection-information"></a>获取连接信息
获取连接到 Azure Database for PostgreSQL 所需的连接信息。 需要完全限定的服务器名称和登录凭据。

1. 登录到 [Azure 门户](https://portal.azure.com/)。
2. 在 Azure 门户的左侧菜单中，单击“所有资源”，然后搜索已创建的服务器（例如 mydemoserver）。
3. 单击服务器名称。
4. 从服务器的“概览”面板中记下“服务器名称”和“服务器管理员登录名”。 如果忘记了密码，也可通过此面板来重置密码。
 ![Azure Database for PostgreSQL 服务器名称](./media/connect-ruby/1-connection-string.png)

## <a name="connect-and-create-a-table"></a>进行连接并创建表
使用以下代码进行连接，使用 **CREATE TABLE** SQL 语句创建表，然后使用 **INSERT INTO** SQL 语句将行添加到表中。

该代码使用 [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) 对象和构造函数 [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) 来连接到 Azure Database for PostgreSQL。 然后调用 [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) 方法，以便运行 DROP、CREATE TABLE 和 INSERT INTO 命令。 代码使用 [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error) 类来检查是否存在错误。 然后，它会调用方法 [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)，在终止之前关闭连接。

将 `host`、`database`、`user` 和 `password` 字符串替换为你自己的值。 
```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database'

    # Drop previous table of same name if one exists
    connection.exec('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    connection.exec('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    connection.exec("INSERT INTO inventory VALUES(1, 'banana', 150)")
    connection.exec("INSERT INTO inventory VALUES(2, 'orange', 154)")
    connection.exec("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="read-data"></a>读取数据
使用以下代码进行连接，并使用 **SELECT** SQL 语句来读取数据。 

该代码使用 [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) 对象和构造函数 [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) 来连接到 Azure Database for PostgreSQL。 然后，它会调用方法 [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) 来运行 SELECT 命令，将结果保存在结果集中。 结果集集合使用 `resultSet.each do` 循环进行循环访问，将最新的行值保存在 `row` 变量中。 代码使用 [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error) 类来检查是否存在错误。 然后，它会调用方法 [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)，在终止之前关闭连接。

将 `host`、`database`、`user` 和 `password` 字符串替换为你自己的值。 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :database => dbname, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    resultSet = connection.exec('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="update-data"></a>更新数据
使用以下代码进行连接，并使用 **UPDATE** SQL 语句更新数据。

该代码使用 [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) 对象和构造函数 [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) 来连接到 Azure Database for PostgreSQL。 然后，它会调用方法 [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) 来运行 UPDATE 命令。 代码使用 [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error) 类来检查是否存在错误。 然后，它会调用方法 [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)，在终止之前关闭连接。

将 `host`、`database`、`user` 和 `password` 字符串替换为你自己的值。 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


## <a name="delete-data"></a>删除数据
使用以下代码进行连接，并使用 **DELETE** SQL 语句读取数据。 

该代码使用 [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) 对象和构造函数 [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) 来连接到 Azure Database for PostgreSQL。 然后，它会调用方法 [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) 来运行 UPDATE 命令。 代码使用 [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error) 类来检查是否存在错误。 然后，它会调用方法 [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method)，在终止之前关闭连接。

将 `host`、`database`、`user` 和 `password` 字符串替换为你自己的值。 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [使用导出和导入功能迁移数据库](./howto-migrate-using-export-and-import.md)
