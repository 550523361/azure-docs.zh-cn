---
title: Azure AD Xamarin 入门 | Microsoft Docs
description: 构建一个与 Azure AD 集成以方便登录，并使用 OAuth 调用受 Azure AD 保护的 API 的 Xamarin 应用程序。
services: active-directory
documentationcenter: xamarin
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/22/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e3470d9220ed471a05792ed5b3bb259e0dcbe0a6
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121913"
---
# <a name="quickstart-build-a-xamarin-app-that-integrates-microsoft-sign-in"></a>快速入门：构建集成 Microsoft 登录的 Xamarin 应用

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

借助 Xamarin，可以使用 C# 编写可在 iOS、Android 和 Windows（移动设备和电脑）上运行的移动应用。 如果使用 Xamarin 构建应用，可以通过 Azure Active Directory (Azure AD) 轻松使用用户的 Active Directory 帐户对其进行身份验证。 该应用还能安全使用 Azure AD 保护的任何 Web API，例如 Office 365 API 或 Azure API。

对于需要访问受保护资源的 Xamarin 应用程序，Azure AD 提供 Active Directory 身份验证库 (ADAL)。 在本质上，ADAL 的唯一用途就是方便应用获取访问令牌。 为了演示操作的简单性，本文介绍如何构建 DirectorySearcher 应用，该应用可以：

* 在 iOS、Android、Windows 桌面、Windows Phone 和 Windows 应用商店上运行。
* 使用单个可移植类库 (PCL) 对用户进行身份验证，并获取 Azure AD 图形 API 的令牌。
* 在目录中搜索具有给定 UPN 的用户。

## <a name="prerequisites"></a>先决条件

* 下载[框架项目](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip)，或下载[已完成的示例](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip)。 每个下载内容都是 Visual Studio 2013 解决方案。
* 还需要一个可在其中创建用户和注册应用的 Azure AD 租户。 如果还没有租户，请[了解如何获取租户](quickstart-create-new-tenant.md)。

准备好后，请按照以下 4 个部分中的步骤操作。

## <a name="step-1-set-up-your-xamarin-development-environment"></a>步骤 1：设置 Xamarin 开发环境

由于本教程包含 iOS、Android 和 Windows 的项目，因此需要 Visual Studio 和 Xamarin。 若要创建所需的环境，请完成 MSDN 上[设置和安装 Visual Studio 与 Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) 中的过程。 这些说明包含的材料可供你在等待安装完成时查看，以深入了解 Xamarin。

完成设置后，在 Visual Studio 中打开解决方案。 可以看到六个项目：五个特定于平台的项目，一个要在所有平台之间共享的 PCL，即 DirectorySearcher.cs。

## <a name="step-2-register-the-directorysearcher-app"></a>步骤 2：注册 DirectorySearcher 应用

若要让应用获取令牌，首先需要在 Azure AD 租户中注册该应用，并授予其访问 Azure AD 图形 API 的权限。 方法如下：

1. 登录到 [Azure 门户](https://portal.azure.com)。
2. 在顶部栏上，单击帐户。 然后，在“目录”列表下选择要注册应用的 Active Directory 租户。 
3. 在左窗格中，单击“所有服务”，并选择“Azure Active Directory”。  
4. 单击“应用注册”，然后选择“新建注册”   。
5. 若要创建新的客户端应用程序，请遵照提示操作。
   * **名称** - 向用户描述应用。
   * 在“支持的帐户类型”下，选择“任何组织目录中的帐户和个人 Microsoft 帐户”。  
   * **重定向 URI** 是 Azure AD 用来返回令牌响应的方案与字符串组合。 输入值（例如 `http://DirectorySearcher`）。
6. 完成注册后，Azure AD 将为应用分配唯一的应用程序 ID。 复制“应用程序”选项卡中的值，因为稍后需用到此值。 
7. 在“API 权限”  页面上，选择“添加权限”  。 在“选择 API”中选择“Microsoft Graph”  。
8. 在“委托的权限”  下，选择 **User.Read** 权限，然后点击“添加”  以保存。 此权限将应用程序设置为在 Azure AD Graph API 中查询用户。

## <a name="step-3-install-and-configure-adal"></a>步骤 3：安装并配置 ADAL

将应用注册到 Azure AD 后，可以安装 ADAL 并编写标识相关的代码。 若要允许 ADAL 与 Azure AD 通信，请向其提供一些有关应用注册的信息。

1. 使用包管理器控制台将 ADAL 添加到 DirectorySearcher 项目。

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
    ```

    注意每个项目中添加了两个库：ADAL 的 PCL 部分，和特定于平台的部分。
2. 在 DirectorySearcherLib 项目中，打开 DirectorySearcher.cs。
3. 将类成员值替换为在 Azure 门户中输入的值。 只要使用 ADAL，代码就会引用这些值。

   * *tenant* 是 Azure AD 租户的域（例如 contoso.onmicrosoft.com）。
   * *clientId* 是从门户中复制的应用的客户端 ID。
   *  returnUri 是在门户中输入的重定向 URI （例如 `http://DirectorySearcher`）。

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a>步骤 4：使用 ADAL 从 Azure AD 获取令牌

几乎所有的应用的身份验证逻辑都位于 `DirectorySearcher.SearchByAlias(...)`。 在特定于平台的项目中，所要做的一切就是将上下文参数传递到 `DirectorySearcher` PCL。

1. 打开 DirectorySearcher.cs，然后将一个新参数添加到 `SearchByAlias(...)` 方法。 `IPlatformParameters` 是上下文参数，用于封装 ADAL 需要对其执行身份验证的特定于平台的对象。

    ```csharp
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. 初始化 `AuthenticationContext`，这是 ADAL 的主类。 此操作将传递 ADAL 与 Azure AD 通信时所需的坐标。
3. 调用 `AcquireTokenAsync(...)`，该类接受 `IPlatformParameters` 对象，并调用所需的身份验证流来向应用程序返回令牌。

    ```csharp
    ...
        AuthenticationResult authResult = null;
        try
        {
            AuthenticationContext authContext = new AuthenticationContext(authority);
            authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
        }
        catch (Exception ee)
        {
            results.Add(new User { error = ee.Message });
            return results;
        }
    ...
    ```

    `AcquireTokenAsync(...)` 首先会尝试返回请求资源（在本例中为图形 API）的令牌，而不提示用户输入其凭据（通过缓存或刷新旧令牌）。 必要时，它会在获取请求的令牌之前，向用户显示 Azure AD 登录页。
4. 在 **Authorization** 标头中将访问令牌附加到图形 API 请求：

    ```csharp
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

这就是需要针对 `DirectorySearcher` PCL 和应用的标识相关代码执行的所有操作。 余下的操作是在每个平台的视图中调用 `SearchByAlias(...)` 方法，并根据需要添加代码来正确处理 UI 生命周期。

### <a name="android"></a>Android
1. 在 MainActivity.cs 中，在按钮单击处理程序中添加对 `SearchByAlias(...)` 的调用：

    ```csharp
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. 重写 `OnActivityResult` 生命周期方法，将所有身份验证重定向转发回到相应的方法。 ADAL 在 Android 中为此提供了帮助器方法：

    ```csharp
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a>Windows 桌面

在 MainWindow.xaml.cs 中，只需调用 `SearchByAlias(...)`，并在桌面的 `PlatformParameters` 对象中传递 `WindowInteropHelper`：

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS
在 DirSearchClient_iOSViewController.cs 中，iOS `PlatformParameters` 对象将引用视图控制器：

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a>Windows Universal
在 Windows Universal 中打开 MainPage.xaml.cs，并实现 `Search` 方法。 此方法根据需要使用共享项目中的帮助器方法来更新 UI。

```csharp
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

现已创建一个正常运行的 Xamarin 应用，它可以对用户进行身份验证，并使用 OAuth 2.0 在五个不同的平台上安全调用 Web API。

## <a name="step-5-populate-your-tenant"></a>步骤 5：填充租户 

如果尚未在租户中填充用户，现在便可执行此操作。

1. 运行 DirectorySearcher 应用，并使用其中一个用户进行登录。
2. 根据用户的 UPN 搜索其他用户。

## <a name="next-steps"></a>后续步骤

使用 ADAL 可以方便地将常见标识功能合并到应用中。 它会负责所有的繁琐工作，例如缓存管理、OAuth 协议支持、向用户显示登录 UI，以及刷新过期令牌。 只需知道一个 API 调用，即 `authContext.AcquireToken*(…)`。

* 下载[已完成的示例](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip)（无配置值）。
* 了解如何[使用 Azure AD 保护 .NET Web API](quickstart-v1-dotnet-webapi.md)。
