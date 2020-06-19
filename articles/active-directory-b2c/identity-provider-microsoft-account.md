---
title: 设置通过 Microsoft 帐户注册与登录
titleSuffix: Azure AD B2C
description: 使用 Azure Active Directory B2C，为应用程序中的客户提供通过 Microsoft 帐户注册与登录的功能。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/12/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 375c83445bb559efe5c797e583129cb1b0c2fb65
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83636911"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>使用 Azure Active Directory B2C 设置通过 Microsoft 帐户注册与登录

## <a name="create-a-microsoft-account-application"></a>创建 Microsoft 帐户应用程序

若要在 Azure Active Directory B2C (Azure AD B2C) 中将 Microsoft 帐户用作[标识提供者](openid-connect.md)，则需要在 Azure AD 租户中创建应用程序。 Azure AD 租户与 Azure AD B2C 租户不同。 如果还没有 Microsoft 帐户，可以在 [https://www.live.com/](https://www.live.com/) 获取。

1. 登录 [Azure 门户](https://portal.azure.com)。
1. 请确保使用包含 Azure AD 租户的目录，方法是选择顶部菜单中的“目录 + 订阅”筛选器，然后选择包含 Azure AD 租户的目录。
1. 选择 Azure 门户左上角的“所有服务”，然后搜索并选择“应用注册” 。
1. 选择“新注册”。
1. 输入应用程序的**名称**。 例如，MSAapp1。
1. 在“受支持的帐户类型”下，选择“任何组织目录（任何 Azure AD 目录 - 多租户）中的帐户和个人 Microsoft 帐户（例如，Skype、Xbox）”。

   有关不同帐户类型选择的详细信息，请参阅[快速入门：将应用程序注册到 Microsoft 标识平台](../active-directory/develop/quickstart-register-app.md)。
1. 在“重定向 URI（可选）”下，选择“Web”，然后在文本框中输入 `https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/oauth2/authresp`。 将 `<tenant-name>` 替换为 Azure AD B2C 租户的名称。
1. 选择“注册”
1. 记录应用程序概述页上显示的“应用程序（客户端）ID”。 在下一部分中配置标识提供者时，需要用到此项。
1. 选择“证书和密码”
1. 单击“新客户端密码”
1. 为密码输入“说明”，例如“应用程序密码 1”，然后单击“添加”。
1. 记录“值”列中显示的应用程序密码。 在下一部分中配置标识提供者时，需要用到此项。

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>将 Microsoft 帐户配置为标识提供者

1. 以 Azure AD B2C 租户的全局管理员身份登录 [Azure 门户](https://portal.azure.com/)。
1. 请确保使用包含 Azure AD B2C 租户的目录，方法是选择顶部菜单中的“目录 + 订阅”筛选器，然后选择包含租户的目录。
1. 选择 Azure 门户左上角的“所有服务”，搜索并选择 **Azure AD B2C**。
1. 选择“标识提供者”，然后选择“Microsoft 帐户”。
1. 输入“名称”。 例如，MSA。
1. 对于“客户端 ID”，输入之前创建的 Azure AD 应用程序的应用程序（客户端）ID。
1. 对于“客户端密码”，输入已记录的客户端密码。
1. 选择“保存”。
