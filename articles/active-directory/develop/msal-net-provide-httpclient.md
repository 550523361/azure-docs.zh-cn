---
title: 提供 httpClient &代理 （MSAL.NET） |蔚蓝
titleSuffix: Microsoft identity platform
description: 了解如何使用适用于 .NET 的 Microsoft 身份验证库 (MSAL.NET) 提供你自己的 HttpClient 和代理来连接到 Azure AD。
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/23/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: dbf08e23b2bc1f657363f69df55763437e6c8a90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "76695036"
---
# <a name="providing-your-own-httpclient-and-proxy-using-msalnet"></a>使用 MSAL.NET 提供你自己的 HttpClient 和代理
[初始化公共客户端应用程序](msal-net-initializing-client-applications.md)时，可以使用`.WithHttpClientFactory method`提供您自己的 HttpClient。  提供你自己的 HttpClient 可以实现高级方案，例如对 HTTP 代理的精细控制、自定义用户代理标头，或者强制 MSAL 使用特定的 HttpClient（例如在 ASP.NET Core Web 应用/API 中）。

## <a name="initialize-with-httpclientfactory"></a>使用 HttpClientFactory 进行初始化
下面的示例展示了如何创建 `HttpClientFactory`，然后通过它来初始化公共客户端应用程序：

```csharp
IMsalHttpClientFactory httpClientFactory = new MyHttpClientFactory();

var pca = PublicClientApplicationBuilder.Create(MsalTestConstants.ClientId) 
                                        .WithHttpClientFactory(httpClientFactory)
                                        .Build();
```

## <a name="httpclient-and-xamarin-ios"></a>HttpClient 和 Xamarin iOS
使用 Xamarin iOS 时，建议创建一个 `HttpClient` 并让其针对 iOS 7 和更高版本显式使用基于 `NSURLSession` 的处理程序。 MSAL.NET 会自动创建一个 `HttpClient`，它针对 iOS 7 和更高版本使用 `NSURLSessionHandler`。 有关详细信息，请阅读[适用于 HttpClient 的 Xamarin iOS 文档](/xamarin/cross-platform/macios/http-stack)。