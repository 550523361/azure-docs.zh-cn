---
title: 使用托管标识确保 Azure SQL 数据库连接的安全 - Azure 应用服务 | Microsoft Docs
description: 了解如何使用托管标识让数据库连接更安全，以及如何将此应用到其他 Azure 服务。
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: syntaxc4
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/30/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: b7d8a9b0ef48f7daed74fb15263e516d820a6a38
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "53259063"
---
# <a name="tutorial-secure-azure-sql-database-connection-from-app-service-using-a-managed-identity"></a>教程：使用托管标识确保从应用服务进行的 Azure SQL 数据库连接的安全

[应用服务](app-service-web-overview.md)在 Azure 中提供高度可缩放、自修补的 Web 托管服务。 它还为应用提供[托管标识](app-service-managed-service-identity.md)，这是一项统包解决方案，可以确保安全地访问 [Azure SQL 数据库](/azure/sql-database/)和其他 Azure 服务。 应用服务中的托管标识可以让应用更安全，因为不需在应用中存储机密，例如连接字符串中的凭据。 在本教程中，请将托管标识添加到在[教程：使用 SQL 数据库在 Azure 中构建 ASP.NET 应用](app-service-web-tutorial-dotnet-sqldatabase.md)。 完成后，示例应用就可以安全地连接到 SQL 数据库，不需用户名和密码。

> [!NOTE]
> 此方案目前受 .NET Framework 4.6 及更高版本的支持，但不受 [.NET Core 2.1](https://www.microsoft.com/net/learn/get-started/windows) 的支持。 [.NET Core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2) 支持此方案，但它尚未包括在应用服务的默认映像中。 
>

学习如何：

> [!div class="checklist"]
> * 启用托管标识
> * 授予 SQL 数据库访问托管标识的权限
> * 配置应用程序代码，以便使用 Azure Active Directory 身份验证通过 SQL 数据库进行身份验证
> * 在 SQL 数据库中向托管标识授予最低特权

> [!NOTE]
>在本地 Active Directory (AD DS) 中，Azure Active Directory 身份验证不同于[集成式 Windows 身份验证](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10))。 AD DS 和 Azure Active Directory 使用的身份验证协议完全不相同。 有关详细信息，请参阅 [Azure AD 域服务文档](https://docs.microsoft.com/azure/active-directory-domain-services/)。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>先决条件

本文从你在[教程：使用 SQL 数据库在 Azure 中构建 ASP.NET 应用](app-service-web-tutorial-dotnet-sqldatabase.md)。 如果尚未学习该教程，请先学习该教程。 也可调整这些步骤，使用 SQL 数据库来构建自己的 ASP.NET 应用。

<!-- ![app running in App Service](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png) -->

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-managed-identities"></a>启用托管标识

若要为 Azure 应用启用托管标识，请在 Cloud Shell 中使用 [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) 命令。 在以下命令中，替换 *\<app name>*。

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app name>
```

以下示例演示了在 Azure Active Directory 中创建标识后的输出：

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

下一步将用到 `principalId` 的值。 若要在 Azure Active Directory 中查看新标识的详细信息，请使用 `principalId` 的值运行以下可选命令：

```azurecli-interactive
az ad sp show --id <principalid>
```

## <a name="grant-database-access-to-identity"></a>授予数据库访问标识的权限

接下来，请在 Cloud Shell 中使用 [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az-sql-server-ad-admin_create) 命令授予数据库访问应用的托管标识的权限。 在以下命令中，替换 *\<server_name>* 和 <principalid_from_last_step>。 键入 *\<admin_user>* 的管理员名称。

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server_name> --display-name <admin_user> --object-id <principalid_from_last_step>
```

托管标识现在可以访问 Azure SQL 数据库服务器了。

## <a name="modify-connection-string"></a>修改连接字符串

在 Cloud Shell 中使用 [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) 命令修改以前为应用设置的连接。 在以下命令中，将 *\<app name>* 替换为应用的名称，将 *\<server_name>* 和 *\<db_name>* 替换为 SQL 数据库的相应名称。

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='Server=tcp:<server_name>.database.windows.net,1433;Database=<db_name>;' --connection-string-type SQLAzure
```

## <a name="modify-aspnet-code"></a>修改 ASP.NET 代码

在 Visual Studio 中，打开包管理器控制台，并添加 NuGet 包 [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)：

```PowerShell
Install-Package Microsoft.Azure.Services.AppAuthentication -Version 1.1.0-preview
```

打开 _Models\MyDatabaseContext.cs_，将以下 `using` 语句添加到文件顶部：

```csharp
using System.Data.SqlClient;
using Microsoft.Azure.Services.AppAuthentication;
using System.Web.Configuration;
```

在 `MyDatabaseContext` 类中，添加以下构造函数：

```csharp
public MyDatabaseContext(SqlConnection conn) : base(conn, true)
{
    conn.ConnectionString = WebConfigurationManager.ConnectionStrings["MyDbConnection"].ConnectionString;
    // DataSource != LocalDB means app is running in Azure with the SQLDB connection string you configured
    if(conn.DataSource != "(localdb)\\MSSQLLocalDB")
        conn.AccessToken = (new AzureServiceTokenProvider()).GetAccessTokenAsync("https://database.windows.net/").Result;

    Database.SetInitializer<MyDatabaseContext>(null);
}
```

此构造函数将自定义 SqlConnection 对象配置为使用应用服务提供的 Azure SQL 数据库的访问令牌。 有了访问令牌，应用服务应用就可以使用其托管标识通过 Azure SQL 数据库进行身份验证。 有关详细信息，请参阅[获取 Azure 资源的令牌](app-service-managed-service-identity.md#obtaining-tokens-for-azure-resources)。 可以使用 `if` 语句，通过 LocalDB 继续在本地测试应用。

> [!NOTE]
> `SqlConnection.AccessToken` 目前仅在 .NET Framework 4.6 及更高版本中受支持，以及在 [.NET Core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2) 中受支持，但在 [.NET Core 2.1](https://www.microsoft.com/net/learn/get-started/windows) 中不受支持。
>

若要使用这个新的构造函数，请打开 `Controllers\TodosController.cs` 并找到 `private MyDatabaseContext db = new MyDatabaseContext();` 行。 现有的代码使用默认的 `MyDatabaseContext` 控制器通过标准的连接字符串来创建数据库，该字符串在未经[更改](#modify-connection-string)的情况下以明文形式保存用户名和密码。

请将整行替换为以下代码：

```csharp
private MyDatabaseContext db = new MyDatabaseContext(new System.Data.SqlClient.SqlConnection());
```

### <a name="publish-your-changes"></a>发布更改

现在，剩下的操作是将更改发布到 Azure。

在“解决方案资源管理器”中，右键单击 “DotNetAppSqlDb”项目，并选择“发布”。

![从解决方案资源管理器发布](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

在发布页中单击“发布”。 当新网页显示待办事项列表时，表明应用使用了托管标识连接到数据库。

![Code First 迁移后的 Azure Web 应用](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

现在应该可以像以前一样编辑待办事项列表了。

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="grant-minimal-privileges-to-identity"></a>向标识授予最低特权

在此前的步骤中，你可能已注意到托管标识是以 Azure AD 管理员身份连接到 SQL Server。 为了向托管标识授予最低特权，需以 Azure AD 管理员身份登录到 Azure SQL 数据库服务器，然后添加包含托管标识的 Azure Active Directory 组。 

### <a name="add-managed-identity-to-an-azure-active-directory-group"></a>向 Azure Active Directory 组添加托管标识

在 Cloud Shell 中，将应用的托管标识添加到名为 _myAzureSQLDBAccessGroup_ 的新 Azure Active Directory 组，如以下脚本所示：

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group <group_name> --name <app_name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiobjectid
az ad group member list -g $groupid
```

若要查看每个命令的完整 JSON 输出，请删除参数 `--query objectId --output tsv`。

### <a name="reconfigure-azure-ad-administrator"></a>重新配置 Azure AD 管理员

你此前已经以 Azure AD 管理员身份为 SQL 数据库分配托管标识。 不能使用此标识进行交互式登录（以添加数据库用户），因此需使用实际的 Azure AD 用户。 若要添加 Azure AD 用户，请执行[为 Azure SQL 数据库服务器预配 Azure Active Directory 管理员](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)中的步骤。 

> [!IMPORTANT]
> 添加之后，除非你想完全禁用 Azure AD 对 SQL 数据库的访问（从所有 Azure AD 帐户），否则不要删除 SQL 数据库的 Azure AD 管理员。
> 

### <a name="grant-permissions-to-azure-active-directory-group"></a>向 Azure Active Directory 组授予权限

在 Cloud Shell 中，使用 SQLCMD 命令登录到 SQL 数据库。 将 _\<server\_name>_ 替换为 SQL 数据库服务器名称，将 _\<db\_name>_ 替换为应用使用的数据库名称，将 _\<AADuser\_name>_ 和 _\<AADpassword>_ 替换为 Azure AD 用户的凭据。

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -d <db_name> -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

在所需数据库的 SQL 提示符窗口中运行以下命令，添加以前创建的 Azure Active Directory 组并授予应用所需的权限。 例如， 

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_ddladmin ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

键入 `EXIT`，返回到 Cloud Shell 提示符窗口。 

## <a name="next-steps"></a>后续步骤

你已了解：

> [!div class="checklist"]
> * 启用托管标识
> * 授予 SQL 数据库访问托管标识的权限
> * 配置应用程序代码，以便使用 Azure Active Directory 身份验证通过 SQL 数据库进行身份验证
> * 在 SQL 数据库中向托管标识授予最低特权

转到下一教程，了解如何向 Web 应用映射自定义 DNS 名称。

> [!div class="nextstepaction"]
> [将现有的自定义 DNS 名称映射到 Azure Web 应用](app-service-web-tutorial-custom-domain.md)
