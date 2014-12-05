<properties pageTitle="Get started with authentication (Windows Store) | Mobile Dev Center" metaKeywords="authentication, Facebook, Google, Twitter, Microsoft Account, login" description="Learn how to use Mobile Services to authenticate users of your Windows Store app through a variety of identity providers, including Google, Facebook, Twitter, and Microsoft." metaCanonical="" services="mobile" documentationCenter="Mobile" title="Get started with authentication in Mobile Services" authors="Glenn Gailey" solutions="" manager="" editor="" />

# 移动服务中的身份验证入门

<div class="dev-center-tutorial-selector sublanding"><a href="/zh-cn/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-users" title="Windows Store C#">Windows 应用商店 C#</a><a href="/zh-cn/documentation/articles/mobile-services-dotnet-backend-windows-store-javascript-get-started-users" title="Windows Store JavaScript">Windows 应用商店 JavaScript</a><a href="/zh-cn/documentation/articles/mobile-services-dotnet-backend-windows-phone-get-started-users" title="Windows Phone">Windows Phone</a><a href="/zh-cn/documentation/articles/mobile-services-dotnet-backend-ios-get-started-users" title="iOS" class="current">iOS</a><!--<a href="/zh-cn/documentation/articles/mobile-services-dotnet-backend-android-get-started-users" title="Android">Android</a>-->
</div>

<div class="dev-center-tutorial-subselector"><a href="/zh-cn/documentation/articles/mobile-services-dotnet-backend-ios-get-started-users/" title=".NET backend" class="current">.NET 后端</a> | <a href="/zh-cn/documentation/articles/mobile-services-ios-get-started-users/"  title="JavaScript backend">JavaScript 后端</a></div>

本主题说明如何通过应用程序对 Azure 移动服务中的用户进行身份验证。在本教程中，你将要使用移动服务支持的标识提供者向快速入门项目添加身份验证。在移动服务成功完成身份验证和授权后，将显示用户 ID 值。

本教程将指导你完成在应用程序中启用身份验证的以下基本步骤：

1.  [注册应用程序以进行身份验证并配置移动服务][注册应用程序以进行身份验证并配置移动服务]
2.  [将表权限限制给已经过身份验证的用户][将表权限限制给已经过身份验证的用户]
3.  [向应用程序添加身份验证][向应用程序添加身份验证]

本教程基于移动服务快速入门。因此，你还必须先完成[移动服务入门][移动服务入门]教程。

<a name="register"></a>
## 注册应用程序以进行身份验证并配置移动服务

[WACOM.INCLUDE [mobile-services-register-authentication](../includes/mobile-services-register-authentication.md)]

<ol start="5">
<li><p>在 Visual Studio 中，打开移动服务项目的 Web.config 文件，然后在 appSettings 节中，设置从标识提供者获取的应用程序标识符和共享密钥值。</p>
<p>在本地开发期间将使用这些设置。将移动服务项目发布到 Azure 后，这些设置将被门户中设置的值重写。</p></li>
</ol>

<a name="permissions"></a>
## 将权限限制给已经过身份验证的用户

[WACOM.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

<ol start="6">
<li><p>在 Xcode 中，打开你在完成[移动服务入门][1]教程后创建的项目。</p></li>
<li><p>在 iPhone 模拟器中按“运行”按钮以生成项目并启动应用程序；验证启动该应用程序后，是否会引发状态代码为 401（“未授权”）的未处理异常 。</p>
   
   	<p>发生此异常的原因是应用程序尝试以未经身份验证的用户身份访问移动服务，但 <em>TodoItem</em> 表现在要求身份验证。</p></li>
</ol>

接下来，你需要更新应用程序，以便在从移动服务请求资源之前对用户进行身份验证。

<a name="add-authentication"></a>
## 向应用程序添加身份验证

[WACOM.INCLUDE [mobile-services-ios-authenticate-app](../includes/mobile-services-ios-authenticate-app.md)]

<a name="next-steps"></a>
## 后续步骤

在下一教程[移动服务用户的服务端授权][移动服务用户的服务端授权]中，你将使用移动服务基于已进行身份验证的用户提供的用户 ID 值来筛选移动服务返回的数据。

  [注册应用程序以进行身份验证并配置移动服务]: #register
  [将表权限限制给已经过身份验证的用户]: #permissions
  [向应用程序添加身份验证]: #add-authentication
  [移动服务入门]: /zh-cn/documentation/articles/mobile-services-dotnet-backend-ios-get-started/
  [1]: /zh-cn/documentation/articles/mobile-services-ios-get-started
  [移动服务用户的服务端授权]: /zh-cn/documentation/articles/mobile-services-dotnet-backend-ios-authorize-users-in-scripts
