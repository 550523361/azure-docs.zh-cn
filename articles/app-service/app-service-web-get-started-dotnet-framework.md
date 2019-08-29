---
title: 创建 C# ASP.NET Framework Web 应用 - Azure 应用服务 | Microsoft Docs
description: 了解如何通过部署默认的 C# ASP.NET Web 应用，在 Azure 应用服务中运行 Web 应用。
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 04a1becf-7756-4d4e-92d8-d9471c263d23
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 09/05/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 6c32415e750964e94129a4a6f9cf3812fe9117b5
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70067280"
---
# <a name="create-an-aspnet-framework-web-app-in-azure"></a>在 Azure 中创建 ASP.NET Framework Web 应用

[Azure 应用服务](overview.md)提供高度可缩放、自修补的 Web 托管服务。  本快速入门演示如何将第一个 ASP.NET Web 应用部署到 Azure 应用服务中。 完成后，将拥有一个资源组，该资源组包含一个应用服务计划和一个部署了 Web 应用程序的应用服务应用。

![](./media/app-service-web-get-started-dotnet-framework/published-azure-web-app.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>先决条件

为完成此教程，请安装支持 **ASP.NET 和 Web 开发**工作负荷的 <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017</a>。

如果已安装 Visual Studio 2017：

- 通过单击“帮助”   >   “检查更新”，在 Visual Studio 中安装最新的更新。
- 通过单击“工具”   >   “获取工具和功能”，添加工作负荷。

## <a name="create-an-aspnet-web-app"></a>创建 ASP.NET Web 应用

在 Visual Studio 中，通过依次选择“文件”>“新建”>“项目”  创建项目。 

在“新建项目”  对话框中，选择“Visual C#”>“Web”>“ASP.NET Web 应用程序 (.NET Framework)”  。

将应用程序命名为 _myFirstAzureWebApp_，然后选择“确定”  。
   
![“新建项目”对话框](./media/app-service-web-get-started-dotnet-framework/new-project.png)

可将任何类型的 ASP.NET Web 应用部署到 Azure。 在本快速入门教程中，请选择“MVC”模板，并确保将身份验证设置为“不进行身份验证”。  
      
选择“确定”  。

![“新建 ASP.NET 项目”对话框](./media/app-service-web-get-started-dotnet-framework/select-mvc-template.png)

在菜单中，选择“调试>启动但不调试”  以在本地运行 Web 应用。

![在本地运行应用](./media/app-service-web-get-started-dotnet-framework/local-web-app.png)

## <a name="launch-the-publish-wizard"></a>启动发布向导

在“解决方案资源管理器”  中右键单击“myFirstAzureWebApp”  项目，然后选择“发布”  。

![从解决方案资源管理器发布](./media/app-service-web-get-started-dotnet-framework/solution-explorer-publish.png)

发布向导是自动启动的。 选择“应用服务”   >   “发布”以打开“创建应用服务”  对话框。

![从项目概述页发布](./media/app-service-web-get-started-dotnet-framework/publish-to-app-service.png)

## <a name="sign-in-to-azure"></a>登录 Azure

在“创建应用服务”  对话框中，选择“添加帐户”  ，然后登录到你的 Azure 订阅。 如果已登录，请从下拉列表中选择包含所需订阅的帐户。

> [!NOTE]
> 如果已经登录，请先不要选择“创建”  。
>
>
   
![登录 Azure](./media/app-service-web-get-started-dotnet-framework/sign-in-azure.png)

## <a name="create-a-resource-group"></a>创建资源组

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

在“资源组”  旁边，选择“新建”  。

将资源组命名为 **myResourceGroup**，然后选择“确定”  。

## <a name="create-an-app-service-plan"></a>创建应用服务计划

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

在“托管计划”旁边  ，选择“新建”  。 

在“配置托管计划”对话框中，使用该屏幕截图下面的表中的设置。 

![创建应用服务计划](./media/app-service-web-get-started-dotnet-framework/configure-app-service-plan.png)

| 设置 | 建议的值 | 描述 |
|-|-|-|
|应用服务计划| myAppServicePlan | 应用服务计划的名称。 |
| 位置 | 西欧 | 托管 Web 应用的数据中心。 |
| 大小 | 免费 | [定价层](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)确定托管功能。 |

选择“确定”  。

## <a name="create-and-publish-the-web-app"></a>创建并发布 Web 应用

在“应用名称”中，键入唯一的应用名称（有效字符为 `a-z`、`0-9` 和 `-`），或接受自动生成的唯一名称。  Web 应用的 URL 为 `http://<app_name>.azurewebsites.net`，其中 `<app_name>` 是应用名称。

选择“创建”  开始创建 Azure 资源。

![配置应用名称](./media/app-service-web-get-started-dotnet-framework/web-app-name.png)

向导完成后，它会将 ASP.NET Web 应用发布到 Azure，然后在默认浏览器中启动该应用。

![已在 Azure 中发布的 ASP.NET Web 应用](./media/app-service-web-get-started-dotnet-framework/published-azure-web-app.png)

在[创建和发布步骤](#create-and-publish-the-web-app)中指定的应用名称用作 `http://<app_name>.azurewebsites.net` 格式的 URL 前缀。

恭喜，你的 ASP.NET Web 应用已在 Azure 应用服务中实时运行！

## <a name="update-the-app-and-redeploy"></a>更新应用并重新部署

在“解决方案资源管理器”中打开“Views\Home\Index.cshtml”。  

在顶部附近找到 `<div class="jumbotron">` HTML 标记，将整个元素替换为以下代码：

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

若要重新部署到 Azure，请在“解决方案资源管理器”  中右键单击“myFirstAzureWebApp”  项目，然后选择“发布”  。

在发布页上选择“发布”  。
![Visual Studio“发布摘要”页](./media/app-service-web-get-started-dotnet-framework/publish-summary-page.png)

发布完成后，Visual Studio 将启动浏览器并转到 Web 应用的 URL。

![已在 Azure 中更新的 ASP.NET Web 应用](./media/app-service-web-get-started-dotnet-framework/updated-azure-web-app.png)

## <a name="manage-the-azure-app"></a>管理 Azure 应用

转到 <a href="https://portal.azure.com" target="_blank">Azure 门户</a>管理 Web 应用。

从左侧菜单中选择“应用程序服务”，并选择 Azure 应用的名称。 

![在门户中导航到 Azure 应用](./media/app-service-web-get-started-dotnet-framework/access-portal.png)

这里我们可以看到 Web 应用的概述页。 并可以执行基本的管理任务，例如浏览、停止、启动、重新启动和删除。 

![Azure 门户中的应用服务边栏选项卡](./media/app-service-web-get-started-dotnet-framework/web-app-blade.png)

左侧菜单提供了用于配置应用的不同页面。 

## <a name="video"></a>视频

观看视频，动态了解此快速入门教程，然后自行按步骤将你的第一个 .NET 应用发布到 Azure。

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-NET-Developers/Create-a-NET-app-in-Azure-Quickstart/player]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [将 ASP.NET 与 SQL 数据库配合使用](app-service-web-tutorial-dotnet-sqldatabase.md)
