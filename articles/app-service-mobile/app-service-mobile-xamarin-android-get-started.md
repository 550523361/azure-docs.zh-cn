---
title: 适用于 Xamarin.Android 应用的 Azure 移动应用入门
description: 按照本教程进行操作，开始使用 Azure 移动应用进行 Xamarin Android 开发
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: crdun
ms.openlocfilehash: 29efa963a254913e3d4744ade1d161c5c8ce42e4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "58893344"
---
# <a name="create-a-xamarinandroid-app"></a>创建 Xamarin.Android 应用
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>概述
本教程说明如何向 Xamarin.Android 应用添加基于云的后端服务。 有关详细信息，请参阅 [什么是移动应用](app-service-mobile-value-prop.md)。

以下是完成的应用程序的屏幕快照：

![][0]

只有在完成本教程后，才可以学习有关 Xamarin Android 应用的所有其他移动应用教程。

## <a name="prerequisites"></a>必备组件
若要完成本教程，需要具备以下先决条件：

* 有效的 Azure 帐户。 如果没有帐户，可以注册 Azure 试用版并获取多达 10 个免费的移动应用。 有关详细信息，请参阅 [Azure 免费试用](https://azure.microsoft.com/pricing/free-trial/)。
* Visual Studio with Xamarin。 有关说明，请参阅 [设置和安装 Visual Studio 和 Xamarin](/visualstudio/cross-platform/setup-and-install) 。

## <a name="create-an-azure-mobile-app-backend"></a>创建 Azure 移动应用后端
按照这些步骤创建移动应用后端。

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

现已预配可供移动客户端应用程序使用的 Azure 移动应用后端。 接下来，为简单的“待办事项列表”后端下载服务器项目并将其发布到 Azure。

## <a name="configure-the-server-project"></a>配置服务器项目
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>下载并运行 Xamarin.Android 应用
1. 在“下载并运行 Xamarin.Android 项目”下，单击“下载”按钮。

      将压缩的项目文件保存到本地计算机，并记下保存位置。
2. 按 **F5** 键生成项目并启动该应用。
3. 在应用中键入有意义的文本（例如“完成教程”），并单击“添加”图标。

    ![][10]

    来自请求的数据插入到 TodoItem 表。 移动应用后端返回存储在表中的项，数据显示在列表中。

   > [!NOTE]
   > 可以查看访问移动应用后端以查询和插入数据的代码，这些代码在 ToDoActivity.cs C# 文件中。
   >
   >

## <a name="troubleshooting"></a>故障排除
如果在生成解决方案时遇到问题，请运行 NuGet 包管理器并更新 `Xamarin.Android` 支持包。 快速入门项目并非始终包含最新版本。

请注意，在项目中引用的所有支持包必须都具有相同的版本。 对于 Android 平台，[Azure 移动应用 NuGet 包](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)具有 `Xamarin.Android.Support.CustomTabs` 依赖项，因此，如果你的项目使用较新的支持包，则你需要直接安装具有所需版本的此包以避免冲突。

## <a name="next-steps"></a>后续步骤
* [向应用添加脱机同步](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [向应用添加身份验证](app-service-mobile-xamarin-android-get-started-users.md)
* [向 Xamarin.Android 应用添加推送通知](app-service-mobile-xamarin-android-get-started-push.md)
* [如何使用 Azure 移动应用的托管客户端](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
