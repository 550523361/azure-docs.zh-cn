---
title: 教程：Azure Active Directory 单一登录 (SSO) 与 Appian 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 Appian 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/17/2020
ms.author: jeedes
ms.openlocfilehash: fc80ae9478f8dfc265602668931a21abe346c5e4
ms.sourcegitcommit: f311f112c9ca711d88a096bed43040fcdad24433
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94981216"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-appian"></a>教程：Azure Active Directory 单一登录 (SSO) 与 Appian 的集成

在本教程中，你将了解如何将 Appian 与 Azure Active Directory (Azure AD) 集成。 将 Appian 与 Azure AD 集成后，可以执行以下操作：

* 在 Azure AD 中控制谁有权访问 Appian。
* 让用户使用其 Azure AD 帐户自动登录到 Appian。
* 在一个中心位置（Azure 门户）管理帐户。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 已启用 Appian 单一登录 (SSO) 的订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。

* Appian 支持 SP 和 IDP 发起的 SSO
* Appian 支持实时用户预配

## <a name="adding-appian-from-the-gallery"></a>从库中添加 Appian

若要配置 Appian 与 Azure AD 的集成，需要将 Appian 从库中添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 Azure 门户。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  。
1. 导航到“企业应用程序”，选择“所有应用程序” 。
1. 若要添加新的应用程序，请选择“新建应用程序”。
1. 在“从库中添加”部分的搜索框中，键入“Appian” 。
1. 在结果面板中选择 Appian，然后添加该应用。 在该应用添加到租户时等待几秒钟。


## <a name="configure-and-test-azure-ad-sso-for-appian"></a>配置并测试 Appian 的 Azure AD SSO

使用名为 B.Simon 的测试用户配置和测试 Appian 的 Azure AD SSO。 若要使 SSO 有效，需要在 Azure AD 用户与 Appian 相关用户之间建立链接关系。

若要配置并测试 Appian 的 Azure AD SSO，请执行以下步骤：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. **[配置 Appian SSO](#configure-appian-sso)** - 在应用程序端配置单一登录设置。
    1. **[创建 Appian 测试用户](#create-appian-test-user)** - 在 Appian 中创建 B.Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

## <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 Azure 门户的 Appian 应用程序集成页上，找到“管理”部分并选择“单一登录”  。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“使用 SAML 设置单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 如果要在“IDP”发起的模式下配置应用程序，请在“基本 SAML 配置”部分中输入以下字段的值   ：

    a. 在“标识符”  文本框中，使用以下模式键入 URL：`https://<SUBDOMAIN>.appiancloud.com`

    b. 在“回复 URL”  文本框中，使用以下模式键入 URL：`https://<SUBDOMAIN>.appiancloud.com/suite/saml/AssertionConsumer`

1. 如果要在 SP  发起的模式下配置应用程序，请单击“设置其他 URL”  ，并执行以下步骤：

    在“登录 URL”文本框中，使用以下模式键入 URL：`https://<SUBDOMAIN>.appiancloud.com/suite`

    > [!NOTE]
    > 这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 请联系 [Appian 客户端支持团队](mailto:support@appian.com)获取这些值。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

1. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分中找到“联合元数据 XML”，选择“下载”以下载该证书并将其保存在计算机上     。

    ![证书下载链接](common/metadataxml.png)

1. 在“设置 Appian”部分中，根据要求复制相应 URL。

    ![复制配置 URL](common/copy-configuration-urls.png)
### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

在本部分，我们将在 Azure 门户中创建名为 B.Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”  。
1. 选择屏幕顶部的“新建用户”。
1. 在“用户”属性中执行以下步骤：
   1. 在“名称”字段中，输入 `B.Simon`。  
   1. 在“用户名”字段中输入 username@companydomain.extension。 例如，`B.Simon@contoso.com`。
   1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。
   1. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，将通过授予 B.Simon 访问 Appian 的权限，允许其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。 
1. 在应用程序列表中，选择“Appian”。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。
1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。
1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。
1. 如果你希望将某角色分配给用户，可以从“选择角色”下拉列表中选择该角色。 如果尚未为此应用设置任何角色，你将看到选择了“默认访问权限”角色。
1. 在“添加分配”对话框中，单击“分配”按钮。

## <a name="configure-appian-sso"></a>配置 Appian SSO

1. 在另一个 Web 浏览器窗口中，以管理员身份登录到 Appian 网站。

1. 转到“管理控制台”并选择“启用 SAML” 。

1. 单击“添加 SAML 标识提供者”。

1.  为 IdP 输入描述。

1.  输入“标识提供者名称”，它应该是服务和标识提供者的唯一名称。

1.  将 IdP 元数据上传到“标识提供者元数据”。 

1.  选择与 IdP 匹配的相应“签名哈希算法”。

1.  单击对话框右上方的“测试此配置”，验证配置是否有效。

1. 成功登录后，单击“完成”，以关闭配置对话框。

1. 如果希望将新的 IdP 登录页面设置为默认页，请相应地设置“默认登录页面”。

1.  关闭对话框后，单击“验证我的访问权限”，确保仍然可以登录 Appian。 这次，请务必以当前用户身份登录。

1.  成功验证你仍可以登录后，单击“保存更改”。


### <a name="create-appian-test-user"></a>创建 Appian 测试用户

在本部分，我们将在 Appian 中创建名为 Britta Simon 的用户。 Appian 支持默认启用的实时用户预配。 此部分不存在任何操作项。 如果 Appian 中尚不存在用户，身份验证后系统会创建一个新用户。

## <a name="test-sso"></a>测试 SSO 

在本部分，你将使用以下选项测试 Azure AD 单一登录配置。 

#### <a name="sp-initiated"></a>SP 启动的：

* 在 Azure 门户中单击“测试此应用程序”。 此操作会重定向到 Appian 登录 URL，你可在其中启动登录流。  

* 直接转到 Appian 登录 URL，并从那里启动登录流。

#### <a name="idp-initiated"></a>IDP 启动的：

* 在 Azure 门户中单击“测试此应用程序”后，你应该会自动登录到为其设置了 SSO 的 Appian 

还可以使用 Microsoft 访问面板在任何模式下测试此应用程序。 在访问面板中单击 Appian 磁贴时，如果是在 SP 模式下配置的，则系统会将你重定向到应用程序登录页，以便启动登录流；而如果是在 IDP 模式下配置的，你应该会自动登录到为其设置了 SSO 的 Appian。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。


## <a name="next-steps"></a>后续步骤

配置 Appian 后，可以强制实施会话控制，实时防止组织的敏感数据外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。


