---
title: 在 Azure 门户中配置和访问 Azure Database for MySQL 的服务器日志
description: 本文介绍如何从 Azure 门户配置和访问 Azure Database for PostgreSQL 中的服务器日志。
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 030c9bf32da7b635066a744270739251b9bf3d03
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "49984700"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>使用 Azure 门户中配置和访问服务器日志

可以从 Azure 门户配置、列出和下载 [Azure Database for MySQL 服务器日志](concepts-server-logs.md)。

## <a name="prerequisites"></a>先决条件
若要逐步执行本操作方法指南，需要：
- [Azure Database for MySQL 服务器](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>配置日志记录
配置 MySQL 慢查询日志的访问权限。 

1. 登录到 [Azure 门户](https://portal.azure.com/)。

2. 选择 Azure Database for MySQL 服务器。

3. 在侧栏“监视”部分下，选择“服务器日志”。 
   ![选择“服务器日志”，单击“配置”](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. 若要查看服务器参数，请选择标题“单击此处以启用日志并配置日志参数”。

5. 更改需要调整的参数。 在此会话中所做的更改都突出显示为紫色。 

   更改参数之后，可以单击“保存”。 或者也可以放弃所做的更改。

   ![只需单击“保存”或“放弃”](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

6. 单击服务器参数页上的“关闭”按钮（X 图标）返回到日志列表。

## <a name="view-list-and-download-logs"></a>查看列表并下载日志
日志记录开始后，可以在“服务器日志”面板上查看可用日志列表并下载各个日志文件。 

1. 打开 Azure 门户。

2. 选择 Azure Database for MySQL 服务器。

3. 在侧栏中的“监视”部分下，选择“服务器日志”。 此页面将显示日志文件列表，如图所示：

   ![日志列表](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > 日志命名约定是 mysql-slow-< your server name>-yyyymmddhh.log。 文件名称中的日期和时间是发布日志的时间。 日志文件每 24 小时或每 7.5 GB 旋转一次，两个条件先达到任何一个就会开始旋转。

4. 如果需要，使用“搜索框”可快速缩小范围，找到基于日期/时间的特定日志。 按日志名称进行搜索。

5. 使用表行中日志文件旁边的“下载”按钮（向下箭头图标）下载单个日志文件，如图所示：

   ![单击“下载”图标](./media/howto-configure-server-logs-in-portal/5-download.png)


## <a name="next-steps"></a>后续步骤
- 若要了解如何以编程方式下载日志，请参阅[使用 CLI 访问服务器日志](howto-configure-server-logs-in-cli.md)。
- 详细了解 Azure Database for MySQL 中的[服务器日志](concepts-server-logs.md)。 
- 有关参数定义和 MySQL 日志记录的详细信息，请参阅[日志](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)上的 MySQL 文档。

