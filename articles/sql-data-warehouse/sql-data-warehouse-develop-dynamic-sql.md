<properties
   pageTitle="SQL 数据仓库中的动态 SQL | Microsoft Azure"
   description="有关在开发解决方案时使用 Azure SQL 数据仓库中的动态 SQL 的技巧。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.date="09/22/2015"
   wacn.date=""/>

# SQL 数据仓库中的动态 SQL
在为 SQL 数据仓库开发应用程序代码时，你可能需要使用动态 SQL 来帮助提供灵活、泛型、模块化的解决方案。SQL 数据仓库目前不支持 Blob 数据类型。这可能会限制字符串的大小，因为 Blob 类型包括 varchar(max) 和 nvarchar(max) 类型。如果你在构建极大型字符串时在应用程序代码中使用了这些类型，则需要将代码分解成块，并改用 EXEC 语句。

一个简单的示例：

```
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

如果字符串较短，则可以像平时一样使用 [sp\_executesql][]。


## 后续步骤
有关更多开发技巧，请参阅[开发概述][]。

<!--Image references-->

<!--Article references-->
[开发概述]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp\_executesql]: https://msdn.microsoft.com/zh-CN/library/ms188001.aspx

<!--Other Web references-->

<!---HONumber=Mooncake_1207_2015-->