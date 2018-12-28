---
title: 在 Azure Active Directory 中使用 SCIM 自动预配应用 | Microsoft Docs
description: Azure Active Directory 可以使用 SCIM 协议规范中定义的接口，自动将用户和组预配到以 Web 服务为前端的任何应用程序或标识存储
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: barbkess
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;seohack1
ms.openlocfilehash: 87f5153ef71f74a0fa1a6be3c527fba03b65bf83
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2018
ms.locfileid: "53095561"
---
# <a name="using-system-for-cross-domain-identity-management-scim-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a>使用跨域标识管理系统 (SCIM) 将用户和组从 Azure Active Directory 自动预配到应用程序

## <a name="overview"></a>概述
Azure Active Directory (Azure AD) 可以使用[跨域标识管理系统 (SCIM) 2.0 协议规范](https://tools.ietf.org/html/draft-ietf-scim-api-19)中定义的接口，将用户和组自动预配到以 Web 服务为前端的任何应用程序或标识存储。 Azure Active Directory 可将创建、修改或删除分配用户和组的请求发送到 web 服务。 然后，Web 服务可将这些请求转换为针对目标标识存储的操作。 

![][0]
*图 1：通过 Web 服务从 Azure Active Directory 预配到标识存储*

此功能可与 Azure AD 中的“携带自己的应用“功能结合使用。 此功能允许对由 SCIM Web 服务前置的应用程序启用单一登录和自动用户预配。

在 Azure Active Directory 中使用 SCIM 有两种用例：

* 将用户和组预配到支持 SCIM 的应用程序 - 支持 SCIM 2.0 并使用 OAuth 持有者令牌进行身份验证的应用程序可与 Azure AD 配合工作，而无需配置。
  
* 为支持其他基于 API 的预配的应用程序构建自己的预配解决方案 - 对于非 SCIM 应用程序，可以创建一个 SCIM 终结点用于在 Azure AD SCIM 终结点与应用程序为用户预配支持的任何 API 之间进行转换。 为了帮助开发 SCIM 终结点，我们提供了公共语言基础结构 (CLI) 库以及代码示例，这些代码示例说明如何提供 SCIM 终结点和转换 SCIM 消息。  

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>将用户和组预配到支持 SCIM 的应用程序
Azure AD 可配置为自动将已分配的用户和组预配到实现[跨域标识管理系统 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) Web 服务、并接受使用 OAuth 持有者令牌进行身份验证的应用程序。 在 SCIM 2.0 规范中，应用程序必须符合以下要求：

* 支持根据 SCIM 协议第 3.3 部分创建用户和/或组。  
* 支持根据 SCIM 协议第 3.5.2 部分修改具有修补请求的用户和/或组。  
* 支持根据 SCIM 协议第 3.4.1 部分检索已知资源。  
* 支持根据 SCIM 协议第 3.4.2 部分查询用户和/或组。  默认情况下，用户是根据 externalId 查询的，组是根据 displayName 查询的。  
* 支持根据 SCIM 协议第 3.4.2 部分，按 ID 和管理员查询用户。  
* 支持根据 SCIM 协议第 3.4.2 部分，按 ID 和成员查询组。  
* 接受根据 SCIM 协议第 2.1 部分使用 OAuth 持有者令牌进行授权。

请咨询应用程序提供者，或参阅应用程序提供者文档中的说明，以了解是否符合这些要求。

### <a name="getting-started"></a>入门
支持本文所述 SCIM 配置文件的应用程序可以使用 Azure AD 应用程序库中的“非库应用程序”功能连接到 Azure Active Directory。 连接后，Azure AD 将每隔 40 分钟运行同步过程，此过程将为分配的用户和组查询应用程序的 SCIM 终结点，并根据分配详细信息创建或修改这些用户和组。

**连接到支持 SCIM 的应用程序：**

1. 登录 [Azure 门户](https://portal.azure.com)。 
2. 浏览到“Azure Active Directory”>“企业应用程序”，然后选择“新建应用程序”>“所有”>“非库应用程序”。
3. 为应用程序输入一个名称，然后单击“添加”图标创建应用对象。
    
   ![][1]
   *图 2：Azure AD 应用程序库*
    
4. 在生成的屏幕的左侧列中选择“预配”选项卡。
5. 在“预配模式”菜单中，选择“自动”。
    
   ![][2]
   *图 3：在 Azure 门户中配置预配*
    
6. 在“租户 URL”字段中，输入应用程序的 SCIM 终结点的 URL。 示例： https://api.contoso.com/scim/v2/
7. 如果 SCIM 终结点需要来自非 Azure AD 颁发者的 OAuth 持有者令牌，可将所需的 OAuth 持有者令牌复制到可选的“密钥令牌”字段。 如果此字段留空，则 Azure AD 会在每个请求中包含从 Azure AD 颁发的 OAuth 持有者令牌。 将 Azure AD 用作标识提供程序的应用可以验证 Azure AD 颁发的此令牌。
8. 单击“测试连接”按钮，使 Azure Active Directory 尝试连接到 SCIM 终结点。 如果尝试失败，则显示错误信息。  
9. 如果尝试连接应用程序成功，请单击“保存”来保存管理员凭据。
10. 在“映射”部分中有两个可选的属性映射集：一个用于用户对象，一个用于组对象。 分别选择它们，查看从 Azure Active Directory 同步到应用的属性。 选为“匹配”属性的特性用于匹配应用中的用户和组，以执行更新操作。 选择“保存”按钮以提交任何更改。

    >[!NOTE]
    >也可通过禁用“组”映射来选择性禁用组对象的同步。 

11. “设置”下的“作用域”字段定义同步的用户或组。 若选择“仅同步分配的用户和组”（推荐），将仅同步“用户和组”选项卡中分配的用户和组。
12. 一旦配置完成，请将“预配状态”更改为“启用”。
13. 单击“保存”以启用 Azure AD 预配服务。 
14. 如果仅同步分配的用户和组（推荐），请确保选择“用户和组”选项卡，并分配要同步的用户和/或组。

一旦启动初始同步，即可使用“审核日志”选项卡来监视进程，它将显示由应用中预配服务所执行的所有操作。 若要详细了解如何读取 Azure AD 预配日志，请参阅[有关自动用户帐户预配的报告](check-status-user-account-provisioning.md)。

> [!NOTE]
> 初始同步执行的时间比后续同步长，只要服务正在运行，大约每隔 40 分钟就会进行一次同步。 


## <a name="building-your-own-provisioning-solution-for-any-application"></a>为任何应用程序生成自己的预配解决方案
创建可与 Azure Active Directory 交互的 SCIM Web 服务后，可为提供 REST 或 SOAP 用户预配 API 的几乎所有应用程序启用单一登录和自动用户预配。

工作方式如下：

1. Azure AD 提供名为 [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/) 的公共语言基础结构库。 系统集成商和开发商可以使用此库来创建与部署能够将 Azure AD 连接到任何应用程序的标识存储的、基于 SCIM 的 Web 服务终结点。
2. 将在 Web 服务中实现映射，以将标准化用户架构映射到用户架构和应用程序所需的协议。
3. 终结点 URL 在 Azure AD 中注册为应用程序库中自定义应用程序的一部分。
4. 用户和组在 Azure AD 中分配到此应用程序。 分配后，它们会被放入队列，以同步到目标应用程序。 处理队列的同步过程每隔 40 分钟运行一次。

### <a name="code-samples"></a>代码示例
为简化此过程，我们提供了[代码示例](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)，用于创建 SCIM Web 服务终结点并演示自动预配。 其中一个示例是维护包含逗号分隔值行（代表用户和组的）的文件的提供程序。  另一个是在 Amazon Web 服务标识与访问管理服务上运行的提供程序。  

**先决条件**

* Visual Studio 2013 或更高版本
* [用于 .NET 的 Azure SDK](https://azure.microsoft.com/downloads/)
* 支持将 ASP.NET Framework 4.5 用作 SCIM 终结点的 Windows 计算机。 必须能够从云访问此计算机
* [具有 Azure AD Premium 试用版或许可版的 Azure 订阅](https://azure.microsoft.com/services/active-directory/)

### <a name="getting-started"></a>入门
实现可以接受来自 Azure AD 的预配请求的 SCIM 终结点的最简单方法是构建和部署将预配的用户输出逗号分隔值 (CSV) 文件的代码示例。

**创建示例 SCIM 终结点：**

1. 从 [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) 下载代码示例包
2. 将包解压缩，并将其放在 Windows 计算机上的某个位置，例如 C:\AzureAD-BYOA-Provisioning-Samples\。
3. 在此文件夹中，启动 Visual Studio 中的 FileProvisioning\Host\FileProvisioningService.csproj 项目。
4. 选择“工具”>“NuGet 包管理器”>“包管理器控制台”并执行以下命令，使 FileProvisioningService 项目解析解决方案引用：

   ```
    Update-Package -Reinstall
   ```

5. 构建 FileProvisioningService 项目。
6. 在 Windows 中启动命令提示符应用程序（以管理员身分），并使用 cd 命令将目录切换到 \AzureAD-BYOA-Provisioning-Samples\FileProvisioning\Host\bin\Debug 文件夹。
7. 运行以下命令，并将 <ip-address> 替换为 Windows 计算机的 IP 地址或域名：

   ```
    FileSvc.exe http://<ip-address>:9000 TargetFile.csv
   ```

8. 在 Windows 中，于“Windows 设置”>“网络和 Internet 设置”下，选择“Windows 防火墙”>“高级设置”，并创建允许对端口 9000 进行入站访问的“入站规则”。
9. 如果 Windows 计算机位于路由器后面，则需要将路由器配置为在面向 Internet 的端口 9000 与 Windows 计算机上的端口 9000 之间执行网络访问转换。 Azure AD 要在云中访问此终结点，需要此配置。

**在 Azure AD 中注册示例 SCIM 终结点：**

1. 登录 [Azure 门户](https://portal.azure.com)。 
2. 浏览到“Azure Active Directory”>“企业应用程序”，然后选择“新建应用程序”>“所有”>“非库应用程序”。
3. 为应用程序输入一个名称，然后单击“添加”图标创建应用对象。 创建的应用程序对象代表要预配和实现单一登录的目标应用，而不仅是 SCIM 终结点。
4. 在生成的屏幕的左侧列中选择“预配”选项卡。
5. 在“预配模式”菜单中，选择“自动”。
    
   ![][2]
   *图 4：在 Azure 门户中配置预配*
    
6. 在“租户 URL”字段中，输入面向 Internet 的 URL 和 SCIM 终结点的端口。 该条目类似于 http://testmachine.contoso.com:9000 或 http://<ip-address>:9000/，其中 <ip-address> 是 Internet 公开的 IP 地址。  
7. 如果 SCIM 终结点需要来自非 Azure AD 颁发者的 OAuth 持有者令牌，可将所需的 OAuth 持有者令牌复制到可选的“密钥令牌”字段。 如果此字段留空，则 Azure AD 会在每个请求中包含从 Azure AD 颁发的 OAuth 持有者令牌。 将 Azure AD 用作标识提供程序的应用可以验证 Azure AD 颁发的此令牌。
8. 单击“测试连接”按钮，使 Azure Active Directory 尝试连接到 SCIM 终结点。 如果尝试失败，则显示错误信息。  
9. 如果尝试连接应用程序成功，请单击“保存”来保存管理员凭据。
10. 在“映射”部分中有两个可选的属性映射集：一个用于用户对象，一个用于组对象。 分别选择它们，查看从 Azure Active Directory 同步到应用的属性。 选为“匹配”属性的特性用于匹配应用中的用户和组，以执行更新操作。 选择“保存”按钮以提交任何更改。
11. “设置”下的“作用域”字段定义同步的用户或组。 若选择“仅同步分配的用户和组”（推荐），将仅同步“用户和组”选项卡中分配的用户和组。
12. 一旦配置完成，请将“预配状态”更改为“启用”。
13. 单击“保存”以启用 Azure AD 预配服务。 
14. 如果仅同步分配的用户和组（推荐），请确保选择“用户和组”选项卡，并分配要同步的用户和/或组。

一旦启动初始同步，即可使用“审核日志”选项卡来监视进程，它将显示由应用中预配服务所执行的所有操作。 若要详细了解如何读取 Azure AD 预配日志，请参阅[有关自动用户帐户预配的报告](check-status-user-account-provisioning.md)。

验证此示例的最后一步是打开 Windows 计算机上 \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug 文件夹中的 TargetFile.csv 文件。 运行预配过程后，此文件会显示所有已分配和预配的用户与组的详细信息。

### <a name="development-libraries"></a>开发库
若要开发自己的符合 SCIM 规范的 Web 服务，请先熟悉 Microsoft 提供的、有助于加速开发过程的以下库： 

1. 提供公共语言基础结构 (CLI) 库以配合基于该基础结构的语言，例如 C#。 其中一个库 [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/) 声明接口 Microsoft.SystemForCrossDomainIdentityManagement.IProvider，如下图所示：使用这些库的开发人员将对某个类（一般称为提供程序）实现该接口。 通过库，开发者可部署符合 SCIM 规范的 Web 服务。 Web 服务可以托管在 Internet Information Services 中，也可托管在任何可执行的公共语言基础结构程序集中。 请求将转换为对提供程序方法的调用，这些方法由开发者编程，以便对某些标识存储执行操作。
  
   ![][3]
  
2. [快速路由处理程序](https://expressjs.com/guide/routing.html)用于分析代表对 node.js Web 服务的调用（由 SCIM 规范定义）的 node.js 请求对象。   

### <a name="building-a-custom-scim-endpoint"></a>构建自定义 SCIM 终结点
通过 CLI 库，使用这些库的开发者将其服务托管在任何可执行的公共语言基础结构程序集或 Internet Information Services 中。 以下代码示例用于将服务托管在地址为 http://localhost:9000: 的可执行程序集中： 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

此服务必须具有 HTTP 地址，其服务器身份验证证书的根证书颁发机构是下列名称之一： 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

可以使用网络 shell 实用程序将服务器身份验证证书绑定到 Windows 主机上的某个端口： 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

此处，为 certhash 参数提供的值为证书指纹，为 appid 参数提供的值为任意全局唯一标识符。  

要将服务托管在 Internet Information Services 中，开发人员需构建一个 CLA 代码库程序集，并在该程序集的默认命名空间中使用名为 Startup 的类。  以下是这种类的示例： 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a>处理终结点身份验证
来自 Azure Active Directory 的请求包括 OAuth 2.0 持有者令牌。   接收请求的任何服务应该代表所需的 Azure Active Directory 租户将颁发者作为 Azure Active Directory 进行身份验证，以访问 Azure Active Directory Graph Web 服务。  在令牌中，颁发者是由一个 iss 声明标识的，例如 "iss":"https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/"。  在此示例中，声明值的基址 https://sts.windows.net 将 Azure Active Directory 标识为颁发者，而相对地址段 cbb1a5ac-f33b-45fa-9bf5-f37db0fed422 代表颁发令牌时 Azure Active Directory 租户的唯一标识符。  如果颁发的令牌用于访问 Azure Active Directory Graph Web 服务，该服务的标识符 00000002-0000-0000-c000-000000000000 应在令牌的 aud 声明值中。  

使用 Microsoft 提供的 CLA 库构建 SCIM 服务的开发者可以按照以下步骤使用 Microsoft.Owin.Security.ActiveDirectory 包对 Azure Active Directory 的请求进行身份验证： 

1. 在提供程序中，通过每次启动服务时让服务返回要调用的方法来实现 Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior 属性： 

   ```
     public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
     {
       get
       {
         return this.OnServiceStartup;
       }
     }

     private void OnServiceStartup(
       Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
       System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
     {
     }
   ```

2. 将以下代码添加到该方法，以代表指定的租户对所有服务终结点的所有请求进行身份验证，以确定它们是否持有 Azure Active Directory 颁发的、用于访问 Azure AD Graph Web 服务的令牌： 

   ```
     private void OnServiceStartup(
       Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
       System.Web.Http.HttpConfiguration HttpConfiguration configuration)
     {
       // IFilter is defined in System.Web.Http.dll.  
       System.Web.Http.Filters.IFilter authorizationFilter = 
         new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

       // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
       // System.IdentityModel.Token.Jwt.dll.
       SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
         new TokenValidationParameters()
         {
           ValidAudience = "00000002-0000-0000-c000-000000000000"
         };

       // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
       // Microsoft.Owin.Security.ActiveDirectory.dll
       Microsoft.Owin.Security.ActiveDirectory.
       WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
         new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
         TokenValidationParameters = tokenValidationParameters,
         Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                       // identifier for this one.  
       };

       applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
     }
   ```


## <a name="user-and-group-schema"></a>用户和组架构
Azure Active Directory 可将两种类型的资源预配到 SCIM Web 服务。  这些类型的资源是用户和组。  

用户资源由协议规范 http://tools.ietf.org/html/draft-ietf-scim-core-schema 中包含的架构标识符“urn:ietf:params:scim:schemas:extension:enterprise:2.0:User”予以标识。  以下表 1 提供了 Azure Active Directory 中用户属性与“urn:ietf:params:scim:schemas:extension:enterprise:2.0:User”资源属性之间的默认映射。  

组资源由架构标识符 http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group 予以标识。  下表 2 显示了 Azure Active Directory 中的组属性与 http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group 资源的属性之间的默认映射。  

### <a name="table-1-default-user-attribute-mapping"></a>表 1：默认用户属性映射

| Azure Active Directory 用户 | “urn:ietf:params:scim:schemas:extension:enterprise:2.0:User” |
| --- | --- |
| IsSoftDeleted |活动 |
| displayName |displayName |
| Facsimile-TelephoneNumber |phoneNumbers[type eq "fax"].value |
| givenName |name.givenName |
| jobTitle |title |
| mail |emails[type eq "work"].value |
| mailNickname |externalId |
| manager |manager |
| mobile |phoneNumbers[type eq "mobile"].value |
| objectId |ID |
| postalCode |addresses[type eq "work"].postalCode |
| proxy-Addresses |emails[type eq "other"].Value |
| physical-Delivery-OfficeName |addresses[type eq "other"].Formatted |
| streetAddress |addresses[type eq "work"].streetAddress |
| surname |name.familyName |
| telephone-Number |phoneNumbers[type eq "work"].value |
| user-PrincipalName |userName |

### <a name="table-2-default-group-attribute-mapping"></a>表 2：默认组属性映射

| Azure Active Directory 组 | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| --- | --- |
| displayName |externalId |
| mail |emails[type eq "work"].value |
| mailNickname |displayName |
| members |members |
| objectId |ID |
| proxyAddresses |emails[type eq "other"].Value |

## <a name="user-provisioning-and-de-provisioning"></a>用户预配和取消预配
下图显示了 Azure Active Directory 发送到 SCIM 服务以管理用户在其他标识存储中的生命周期的消息。 该图还显示了使用 Microsoft 提供的、用于构建此类服务的 CLI 库所实现的 SCIM 服务如何将这些请求转换为对提供程序的方法调用。  

![][4]
*图 5：用户预配和取消预配顺序*

1. Azure Active Directory 会在服务中查询是否有某个用户的 externalId 属性值与 Azure AD 中用户的 mailNickname 属性值匹配。 查询以类似此例的超文本传输协议 (HTTP) 请求形式表示，其中，jyoung 是 Azure Active Directory 中某个用户的 mailNickname 示例： 

   ```
     GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
     Authorization: Bearer ...
   ```

   如果使用 Microsoft 提供的、用于实现 SCIM 服务的公共语言基础结构库构建了服务，则将请求转换为对服务提供者的 Query 方法调用。  以下是该方法的签名： 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  
 
     System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]>  Query(
       Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
       string correlationIdentifier);
   ````

   以下是 Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters 接口的定义： 

   ```
     public interface IQueryParameters: 
       Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
     {
         System.Collections.Generic.IReadOnlyCollection  <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
         { get; }
     }

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
     {
       system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
       { get; }
       System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
       { get; }
       string SchemaIdentifier 
       { get; }
     }

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
     {
         Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
           { get; set; }
         string AttributePath 
           { get; } 
         Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
           { get; }
         string ComparisonValue 
           { get; }
     }

     public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
     {
         Equals
     }
   ```

   在查询具有给定 externalId 属性值的用户的下列示例中，传递给 Query 方法的参数值是： 
   * parameters.AlternateFilters.Count:1
   * parameters.AlternateFilters.ElementAt(0).AttributePath: "externalId"
   * parameters.AlternateFilters.ElementAt(0).ComparisonOperator:ComparisonOperator.Equals
   * parameters.AlternateFilter.ElementAt(0).ComparisonValue: "jyoung"
   * correlationIdentifier:System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"] 

2. 如果在 Web 服务中查询是否有某个用户的 externalId 属性值与用户的 mailNickname 属性值匹配时，该查询的响应未返回任何用户，Azure Active Directory 将请求服务预配一个与 Azure Active Directory 中的用户相对应的用户。  以下是此类请求的示例： 

   ```
     POST https://.../scim/Users HTTP/1.1
     Authorization: Bearer ...
     Content-type: application/scim+json
     {
       "schemas":
       [
         "urn:ietf:params:scim:schemas:core:2.0:User",
         "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
       "externalId":"jyoung",
       "userName":"jyoung",
       "active":true,
       "addresses":null,
       "displayName":"Joy Young",
       "emails": [
         {
           "type":"work",
           "value":"jyoung@Contoso.com",
           "primary":true}],
       "meta": {
         "resourceType":"User"},
        "name":{
         "familyName":"Young",
         "givenName":"Joy"},
       "phoneNumbers":null,
       "preferredLanguage":null,
       "title":null,
       "department":null,
       "manager":null}
   ```

   Microsoft 提供的、用于实现 SCIM 服务的通用语言基础结构库将请求转换为对服务提供者的 Create 方法调用。  Create 方法具有此签名： 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
     // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
 
     System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create (
       Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
       string correlationIdentifier);
   ```

   如果请求预配用户，资源参数的值将是 Microsoft.SystemForCrossDomainIdentityManagement 的实例。 Core2EnterpriseUser 类，在 Microsoft.SystemForCrossDomainIdentityManagement.Schemas 库中定义。  如果预配用户的请求成功，则方法的实现应返回 Microsoft.SystemForCrossDomainIdentityManagement 的实例。 Core2EnterpriseUser 类，其 Identifier 属性值设置为新预配用户的唯一标识符。  

3. 为了更新存在于前端为 SCIM 的标识存储中的已知用户，Azure Active Directory 将通过类似于下面的请求向服务请求该用户的当前状态来继续处理： 

   ```
     GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
     Authorization: Bearer ...
   ```

   如果使用 Microsoft 提供的、用于实现 SCIM 服务的公共语言基础结构库生成了服务，则将请求转换为对服务提供者的 Retrieve 方法调用。  以下是 Retrieve 方法的签名： 

    ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
     // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
     // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
     System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
        Retrieve(
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
            parameters, 
            string correlationIdentifier);
 
     public interface 
       Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
         IRetrievalParameters
         {
           Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
             ResourceIdentifier 
               { get; }
     }
     public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
     {
         string Identifier 
           { get; set; }
         string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
           { get; set; }
     }
   ```

   在检索用户当前状态的请求示例中，作为参数自变量值提供的对象属性值如下所示： 
  
   * 标识符："54D382A4-2050-4C03-94D1-E769F1D15682"
   * SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

4. 要更新引用属性，Azure Active Directory 将查询服务以判断以该服务为前端的标识存储中的引用属性的当前值是否已经与 Azure Active Directory 中该属性的值相匹配。 对于用户，以这种方式查询当前值的唯一属性是 manager 属性。 确定特定用户对象的 manager 属性当前是否具有特定值的请求示例如下： 

   ```
     GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq  2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
     Authorization: Bearer ...
   ```

   属性查询参数“ID”的值，表示如果满足提供为筛选器查询参数值的表达式的用户对象存在，则服务应以“urn:ietf:params:scim:schemas:core:2.0:User”或“urn:ietf:params:scim:schemas:extension:enterprise:2.0:User”资源做出响应（仅包括该资源的“ID”属性值）。  对请求者而言，ID 属性值是已知的。 它包含在筛选器查询参数的值中；请求它的目的实际上是请求满足筛选表达式的资源的精简表示形式（指示是否存在任何此类对象）。   

   如果使用 Microsoft 提供的、用于实现 SCIM 服务的公共语言基础结构库构建了服务，则将请求转换为对服务提供者的 Query 方法调用。 作为参数自变量值提供的对像属性值如下： 
  
   * parameters.AlternateFilters.Count:2
   * parameters.AlternateFilters.ElementAt(x).AttributePath:“ID”
   * parameters.AlternateFilters.ElementAt(x).ComparisonOperator:ComparisonOperator.Equals
   * parameters.AlternateFilter.ElementAt(x).ComparisonValue:"54D382A4-2050-4C03-94D1-E769F1D15682"
   * parameters.AlternateFilters.ElementAt(y).AttributePath: "manager"
   * parameters.AlternateFilters.ElementAt(y).ComparisonOperator:ComparisonOperator.Equals
   * parameters.AlternateFilter.ElementAt(y).ComparisonValue:"2819c223-7f76-453a-919d-413861904646"
   * parameters.RequestedAttributePaths.ElementAt(0):“ID”
   * parameters.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

   此处，索引 x 的值可以是 0 并且索引 y 的值可以是 1，或者，x 值可以是 1 并且 y 的值可以是 0，具体根据筛选器查询参数表达式的顺序而定。   

5. 以下是从 Azure Active Directory 向 SCIM 服务发出更新用户请求的示例： 

   ```
     PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
     Authorization: Bearer ...
     Content-type: application/scim+json
     {
       "schemas": 
       [
         "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
       "Operations":
       [
         {
           "op":"Add",
           "path":"manager",
           "value":
             [
               {
                 "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                 "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
   ```

   用于实现 SCIM 服务的 Microsoft 通用语言基础结构库将请求转换为对服务提供者的 Update 方法调用。 以下是 Update 方法的签名： 

   ```
     // System.Threading.Tasks.Tasks and 
     // System.Collections.Generic.IReadOnlyCollection<T>
     // are defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
     // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
     // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
     // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
     // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
     // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
     // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
     // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

     System.Threading.Tasks.Task Update(
       Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
       string correlationIdentifier);

     public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
     {
     Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
       PatchRequest 
         { get; set; }
     Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
       ResourceIdentifier 
         { get; set; }        
     }

     public class PatchRequest2: 
       Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
     {
     public System.Collections.Generic.IReadOnlyCollection
       <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
         Operations
         { get;}

     public void AddOperation(
       Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
     }

     public class PatchOperation
     {
     public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
       Name
       { get; set; }

     public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
       Path
       { get; set; }

     public System.Collections.Generic.IReadOnlyCollection
       <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
       { get; }

     public void AddValue(
       Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
     }

     public enum OperationName
     {
       Add,
       Remove,
       Replace
     }

     public interface IPath
     {
       string AttributePath { get; }
       System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
       Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
     }

     public class OperationValue
     {
       public string Reference
       { get; set; }

       public string Value
       { get; set; }
     }
   ```

    在更新用户的请求示例中，作为修补参数值提供的对象具有这些属性值： 
  
   * ResourceIdentifier.Identifier:"54D382A4-2050-4C03-94D1-E769F1D15682"
   * ResourceIdentifier.SchemaIdentifier:  "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"
   * (PatchRequest as PatchRequest2).Operations.Count:1
   * (PatchRequest as PatchRequest2).Operations.ElementAt(0).OperationName:OperationName.Add
   * (PatchRequest as PatchRequest2).Operations.ElementAt(0).Path.AttributePath: "manager"
   * (PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.Count:1
   * (PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Reference: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
   * (PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Value:2819c223-7f76-453a-919d-413861904646

6. 若要从 SCIM 服务面对的标识存储中取消预配用户，Azure AD 会发送如下请求： 

   ```
     DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
     Authorization: Bearer ...
   ```

   如果使用 Microsoft 提供的、用于实现 SCIM 服务的公共语言基础结构库构建了服务，则将请求转换为对服务提供者的 Delete 方法调用。   该方法具有以下签名： 

   ```
     // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
     // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
     // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
     System.Threading.Tasks.Task Delete(
       Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
         resourceIdentifier, 
       string correlationIdentifier);
   ```

   在取消预配用户的请求示例中，作为 resourceIdentifier 参数值提供的对象具有以下属性值： 

   * ResourceIdentifier.Identifier:"54D382A4-2050-4C03-94D1-E769F1D15682"
   * ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

## <a name="group-provisioning-and-de-provisioning"></a>组预配和取消预配
下图显示了 Azure AD 发送到 SCIM 服务以管理其他标识存储中的组生命周期的消息。  这些消息在以下三个方面与用户相关的消息不同： 

* 组资源的架构标识为 `http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group`。  
* 检索组的请求规定将成员属性从请求响应中提供的任何资源中排除。  
* 确定引用属性是否具有特定值的请求是有关成员属性的请求。  

![][5]
*图 6：组预配和取消预配顺序*

## <a name="related-articles"></a>相关文章
* [在 SaaS 应用中自动预配和取消预配用户](user-provisioning.md)
* [为用户预配自定义属性映射](customize-application-attributes.md)
* [为属性映射编写表达式](functions-for-customizing-application-data.md)
* [用于用户预配的作用域筛选器](define-conditional-rules-for-provisioning-user-accounts.md)
* [帐户预配通知](user-provisioning.md)
* [有关如何集成 SaaS 应用的教程列表](../saas-apps/tutorial-list.md)

<!--Image references-->
[0]: ./media/use-scim-to-provision-users-and-groups/scim-figure-1.png
[1]: ./media/use-scim-to-provision-users-and-groups/scim-figure-2a.png
[2]: ./media/use-scim-to-provision-users-and-groups/scim-figure-2b.png
[3]: ./media/use-scim-to-provision-users-and-groups/scim-figure-3.png
[4]: ./media/use-scim-to-provision-users-and-groups/scim-figure-4.png
[5]: ./media/use-scim-to-provision-users-and-groups/scim-figure-5.png
