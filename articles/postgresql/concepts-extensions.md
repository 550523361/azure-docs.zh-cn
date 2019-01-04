---
title: 使用 Azure Database for PostgreSQL 中的 PostgreSQL 扩展
description: 介绍有关使用 Azure Database for PostgreSQL 中的扩展来扩展数据库功能的功能。
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/12/2018
ms.openlocfilehash: d6d5a8500435a540f091a082e7dc0e0d6d455716
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540837"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>Azure Database for PostgreSQL 中的 PostgreSQL 扩展
PostgreSQL 支持使用扩展来扩展数据的功能。 扩展允许在单个包中将多个相关 SQL 对象捆绑在一起，可以使用单个命令在数据库中加载或删除该包。 在数据库中加载之后，扩展可以如同内置功能一样运行。 有关 PostgreSQL 扩展的详细信息，请参阅  [Packaging Related Objects into an Extension](https://www.postgresql.org/docs/9.6/static/extend-extensions.html)（将相关对象打包到扩展中）。

## <a name="how-to-use-postgresql-extensions"></a>如何使用 PostgreSQL 扩展
必须先在数据库中安装 PostgreSQL 扩展，然后才能使用它们。 若要安装特定扩展，请通过 psql 工具运行  [CREATE EXTENSION](https://www.postgresql.org/docs/9.6/static/sql-createextension.html)  命令，将打包的对象加载到数据库中。

Azure Database for PostgreSQL 目前支持部分关键扩展（已在下面列出）。 未列出的扩展不受支持；无法使用 Azure Database for PostgreSQL 服务创建自己的扩展。

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Azure Database for PostgreSQL 支持的扩展
下表列出了用于 PostgreSQL 的 Azure 数据库目前支持的标准 PostgreSQL 扩展。 还可以通过运行 `SELECT * FROM pg_available_extensions;` 获取此信息。

### <a name="data-types-extensions"></a>数据类型扩展

> [!div class="mx-tableFixed"]
| **扩展** | **说明** |
|---|---|
| [chkpass](https://www.postgresql.org/docs/9.6/static/chkpass.html) | 提供用于自动加密密码的数据类型。 |
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | 提供不区分大小写的字符串类型。 |
| [cube](https://www.postgresql.org/docs/9.6/static/cube.html) | 提供用于多维数据集的数据类型。 |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | 提供用于存储键/值对集的数据类型。 |
| [isn](https://www.postgresql.org/docs/9.6/static/isn.html) | 提供用于国际产品编号标准的数据类型。 |
| [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | 提供用于分层树形结构的数据类型。 |

### <a name="functions-extensions"></a>函数扩展

> [!div class="mx-tableFixed"]
| **扩展** | **说明** |
|---|---|
| [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | 提供一种计算地球表面上的大圆距离的方法。 |
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | 提供多个函数，用于确定字符串间的相似性和差异。 |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | 提供用于操作无 null 整数数组的函数和运算符。 |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | 提供加密函数。 |
| [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | 按时间或 ID 管理已分区表。 |
| [pg\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | 提供函数和运算符，用于基于三元匹配确定字母数字文本的相似性。 |
| [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | 提供可操作整个表（包括交叉表）的函数。 |
| [uuid ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | 生成全局唯一标识符 (UUID)。 |

### <a name="full-text-search-extensions"></a>全文搜索扩展

> [!div class="mx-tableFixed"]
| **扩展** | **说明** |
|---|---|
| [dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | 提供用于整数的文本搜索字典模板。 |
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | 删除了词素中重音（附加符号）的文本搜索字典。 |

### <a name="index-types-extensions"></a>索引类型扩展

> [!div class="mx-tableFixed"]
| **扩展** | **说明** |
|---|---|
| [btree\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | 提供示例 GIN 运算符类，该类对特定数据类型实现类似 B-tree 的行为。 |
| [btree\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | 提供实施 B-tree 的 GiST 索引运算符类。 |

### <a name="language-extensions"></a>语言扩展

> [!div class="mx-tableFixed"]
| **扩展** | **说明** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL 可加载过程语言。 |
| [plv8](https://plv8.github.io/) | 可用于存储过程、触发器等的 PostgreSQL 的 Javascript 语言扩展。 |

### <a name="miscellaneous-extensions"></a>其他扩展

> [!div class="mx-tableFixed"]
| **扩展** | **说明** |
|---|---|
| [pg\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | 提供一种方法用于实时检查共享缓冲区缓存的当前状况。 |
| [pg\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | 提供一种方法用于将相关数据加载到缓冲区缓存中。 |
| [pg\_stat\_statements](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | 提供一种方法用于跟踪服务器执行的所有 SQL 语句的执行统计信息。 （请参阅下文了解此扩展的说明）。 |
| [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | 提供一种显示行级锁定信息的方法。 |
| [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | 提供一种显示元组级别统计信息的方法。 |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | 外部数据包装器，用于访问外部 PostgreSQL 服务器中存储的数据。 |
| [hypopg](https://hypopg.readthedocs.io/en/latest/) | 提供了一种创建不耗费 CPU 或磁盘的假设索引的方法。 |
| [dblink](https://www.postgresql.org/docs/current/dblink.html) | 一个支持从数据库会话中连接到其他 PostgreSQL 数据库的模块。 |


### <a name="postgis-extensions"></a>PostGIS 扩展

> [!div class="mx-tableFixed"]
| **扩展** | **说明** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topology, postgis\_tiger\_geocoder, postgis\_sfcgal | PostgreSQL 的空间和地理对象。 |
| address\_standardizer, address\_standardizer\_data\_us | 用于将地址分析成构成元素。 用于支持地理编码地址规范化步骤。 |
| [pgrouting](https://pgrouting.org/) | 扩展 PostGIS / PostgreSQL 地理空间数据库，以提供地理空间路由功能。 |


### <a name="using-pgstatstatements"></a>使用 pg_stat_statements
[pg\_stat\_ 扩展](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html)预加载在每个 Azure Database for PostgreSQL 服务器上，以便跟踪 SQL 语句的执行统计信息。
设置 `pg_stat_statements.track`，它可以控制哪些语句由扩展计数，默认为 `top`，这意味着跟踪所有由客户端直接发布的语句。 另外两个跟踪级别为 `none` 和 `all`。 此设置可通过 [Azure 门户](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal)或 [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli) 作为服务器参数进行配置。

查询执行信息 pg_stat_statements 提供的权限与记录每个 SQL 语句时对服务器性能的影响之间存在权衡。 如果不经常使用 pg_stat_statements 扩展，则建议将 `pg_stat_statements.track` 设置为 `none`。 请注意，某些第三方监视服务可能依赖 pg_stat_statements 来提供查询性能见解，因此，请确认这是否适合你。


## <a name="next-steps"></a>后续步骤
如果未看到要使用的扩展，请告诉我们。 若要支持现有请求或提出新反馈和请求，请访问我们的[客户反馈论坛](https://feedback.azure.com/forums/597976-azure-database-for-postgresql)。
