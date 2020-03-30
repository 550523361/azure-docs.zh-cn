---
title: 在 Microsoft Power BI 中使用 Azure Blockchain Workbench 数据
description: 了解如何在 Microsoft Power BI 中加载和查看 Azure Blockchain Workbench SQL DB 数据。
ms.date: 05/09/2019
ms.topic: article
ms.reviewer: mmercuri
ms.openlocfilehash: 6e1f160c3563a280548c74ebe84f30bf08945c3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "74324799"
---
# <a name="using-azure-blockchain-workbench-data-with-microsoft-power-bi"></a>通过 Microsoft Power BI 使用 Azure Blockchain Workbench 数据

Microsoft Power BI 提供了使用 Power BI 桌面轻松地从 SQL DB 数据库生成强大[https://www.powerbi.com](https://www.powerbi.com)报告，然后将它们发布到 的功能。

本文包含了有关以下操作的分步演练：从 PowerBI Desktop 中连接到 Azure Blockchain Workbench 的 SQL 数据库，创建报表，以及将报表部署到 powerbi.com。

## <a name="prerequisites"></a>先决条件

* 下载[电源 BI 桌面](https://aka.ms/pbidesktopstore)。

## <a name="connecting-power-bi-to-data-in-azure-blockchain-workbench"></a>将 Power BI 连接到 Azure Blockchain Workbench 中的数据

1.  打开 Power BI Desktop。
2.  选择“获取数据”****。

    ![获取数据](./media/data-powerbi/get-data.png)
3.  从数据源类型列表中选择“SQL Server”。****

4.  在对话框中提供服务器和数据库名称。 指定是要导入数据还是要执行 **DirectQuery**。 选择“确定”。

    ![选择 SQL Server](./media/data-powerbi/select-sql.png)

5.  提供数据库凭据来访问 Azure Blockchain Workbench。 选择“数据库”**** 并输入凭据。

    如果使用由 Azure Blockchain Workbench 部署过程创建的凭据，则用户名是 **dbadmin** 并且密码是在部署过程中提供的密码。

    ![SQL DB 设置](./media/data-powerbi/db-settings.png)

6.  在连接到数据库后，“导航器”对话框将显示数据库中可用的表和视图。**** 视图设计用来提供报告并且全部以 **vw** 作为前缀。

    ![导航器](./media/data-powerbi/navigator.png)

7.  选择要包括的视图。 为了演示，我们包括了 **vwContractAction**，该视图提供了已对某个合同执行的操作的相关详细信息。

    ![选择视图](./media/data-powerbi/select-views.png)

现在可以像往常那样使用 Power BI 创建和发布报表了。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench 中的数据库视图](database-views.md)