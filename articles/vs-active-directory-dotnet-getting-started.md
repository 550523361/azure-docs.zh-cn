<properties title="Getting Started with Active Directory Authentication" pageTitle="" metaKeywords="Azure, Getting Started, Active Directory" description="" services="active-directory" documentationCenter="" authors="ghogen, kempb" />
  
<tags ms.service="active-directory" ms.workload="web" ms.tgt_pltfrm="vs-getting-started" ms.devlang="na" ms.topic="article" ms.date="10/8/2014" ms.author="ghogen, kempb" />

> [AZURE.SELECTOR]
> - [入门](/documentation/articles/vs-active-directory-dotnet-getting-started/)
> - [发生了什么情况](/documentation/articles/vs-active-directory-dotnet-what-happened/)

## Azure Active Directory 入门（.NET 项目）
 
#####访问控制器需要身份验证 

你项目中的所有控制器均带有 **Authorize** 属性。此属性要求用户先进行身份验证，然后才能访问这些控制器。若要允许匿名访问控制器，请从控制器删除此属性。如果你想要更详细地设置这些权限，请将该属性应用到需要身份验证的每个方法，而不是将它应用到控制器类。
 
#####添加 SignIn/SignOut 控件 

若要将 SignIn/SignOut 控件添加到你的视图，你可以使用 **_LoginPartial.cshtml** 分部视图将该功能添加到你的某个视图。下面是已添加到标准 **_Layout.cshtml** 视图的功能的示例。（注意带有 navbar-collapse 类的 div 中的最后一个元素）：

<PRE class="prettyprint">
    &lt;!DOCTYPE html&gt; 
     &lt;html&gt; 
     &lt;head&gt; 
         &lt;meta charset="utf-8" /&gt; 
        &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
        &lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
        @Styles.Render("~/Content/css") 
        @Scripts.Render("~/bundles/modernizr") 
    &lt;/head&gt; 
    &lt;body&gt; 
        &lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
            &lt;div class="container"&gt; 
                &lt;div class="navbar-header"&gt; 
                    &lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                    &lt;/button&gt; 
                    @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) 
                &lt;/div&gt; 
                &lt;div class="navbar-collapse collapse"&gt; 
                    &lt;ul class="nav navbar-nav"&gt; 
                        &lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
                        &lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
                        &lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
                    &lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/div&gt; 
            &lt;/div&gt; 
        &lt;/div&gt; 
        &lt;div class="container body-content"&gt; 
            @RenderBody() 
            &lt;hr /&gt; 
            &lt;footer&gt; 
                &lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
            &lt;/footer&gt; 
        &lt;/div&gt; 
        @Scripts.Render("~/bundles/jquery") 
        @Scripts.Render("~/bundles/bootstrap") 
        @RenderSection("scripts", required: false) 
    &lt;/body&gt; 
    &lt;/html&gt;
</PRE>

[详细了解 Azure Active Directory](http://azure.microsoft.com/services/active-directory/)
