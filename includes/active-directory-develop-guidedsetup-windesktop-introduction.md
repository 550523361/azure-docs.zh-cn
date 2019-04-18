---
title: include 文件
description: include 文件
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: f0cc888eaf3724737e9c868c69a641094a19348c
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/11/2019
ms.locfileid: "59498357"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>从 Windows 桌面应用调用 Microsoft Graph API

本指南演示本机 Windows 桌面 .NET (XAML) 应用程序如何从面向开发人员的 Microsoft 标识平台（以前称为 Azure AD）v2.0 终结点获取访问令牌并调用 Microsoft Graph API 或需要访问令牌的其他 API。

完成本指南后，你的应用程序将能够调用使用个人帐户（包括 outlook.com、live.com 等）的受保护 API。 应用程序还将使用任何使用 Azure Active Directory 的公司或组织提供的工作和学校帐户。  

> [!NOTE]
> 本指南需要 Visual Studio 2015 Update 3 或 Visual Studio 2017。 没有这些版本？ [免费下载 Visual Studio 2017](https://www.visualstudio.com/downloads/)。

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>本指南生成的示例应用的工作原理

![显示本教程生成的示例应用的工作原理](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.svg)

本指南创建的示例应用程序使 Windows 桌面应用程序能够查询接受 Microsoft 标识平台终结点令牌的 Microsoft Graph API 或 Web API。 对于此方案，通过 Authorization 标头将令牌添加到 HTTP 请求。 Microsoft 身份验证库 (MSAL) 处理令牌获取和续订。

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>负责获得用于访问受保护 Web API 的令牌

对用户进行身份验证后，示例应用程序会收到一个令牌，该令牌可用于查询受面向开发人员的 Microsoft 标识平台保护的 Microsoft Graph API 或 Web API。

API（如 Microsoft Graph）需要令牌以允许访问特定资源。 例如，需要使用令牌读取用户的配置文件、访问用户的日历或发送电子邮件。 应用程序可通过指定 API 作用域来使用 MSAL 请求访问令牌，从而访问这些资源。 然后对于针对受保护资源发出的每个调用，将此访问令牌添加到 HTTP 授权标头。

MSAL 负责管理缓存和刷新访问令牌，因此应用程序无需执行这些任务。

## <a name="nuget-packages"></a>NuGet 包

本指南使用以下 NuGet 包：

|库|说明|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft 身份验证库 (MSAL.NET)|
