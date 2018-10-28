---
title: include 文件
description: include 文件
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 15db2192703971a8056df34343c427db11c8411a
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988492"
---
## <a name="register-your-application"></a>注册应用程序

若要注册应用程序并将应用程序注册信息添加到解决方案，有两个选项：

### <a name="option-1-express-mode"></a>选项 1：快速模式

可以通过执行以下操作快速注册应用程序：

1. 通过 [Microsoft 应用程序注册门户](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)注册应用程序。
2. 输入应用程序的名称和电子邮件。
3. 确保选中“指导式设置”选项。
4. 按照说明向应用程序添加重定向 URL。

### <a name="option-2-advanced-mode"></a>选项 2：高级模式

若要注册应用程序并将应用程序注册信息添加到解决方案，请执行以下操作：

1. 转到 [Microsoft 应用程序注册门户](https://apps.dev.microsoft.com/portal/register-app)注册应用程序。
2. 输入应用程序的名称和电子邮件。
3. 确保取消选中“指导式设置”选项
4. 依次选择“`Add Platform`”、“`Web`”。
5. 返回 Visual Studio，在解决方案资源管理器中选择项目并查看“属性”窗口（如果看不到“属性”窗口，请按 F4）
6. 将“已启用 SSL”更改为 `True`。
7. 在 Visual Studio 中右键单击该项目，然后选择“属性”和“Web”选项卡。在“服务器”部分中，将项目 URL 更改为 SSL URL。
8. 复制 SSL URL 并将此 URL 添加到注册门户重定向列表中的重定向 URL 列表：<br/><br/>![项目属性](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
9. 在根文件夹内 `web.config` 中的 `configuration\appSettings` 部分之下添加以下内容：

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

10. 将 `ClientId` 替换为刚注册的应用程序 ID。
11. 将 `redirectUri` 替换为项目的 SSL URL。
