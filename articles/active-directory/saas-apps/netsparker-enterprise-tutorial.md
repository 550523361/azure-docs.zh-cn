---
title: 教程：Azure Active Directory 单一登录 (SSO) 与 Netsparker Enterprise 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Netsparker Enterprise 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/13/2020
ms.author: jeedes
ms.openlocfilehash: 629b5f0b4f4d8b4f63e278802d2a36aea77792c4
ms.sourcegitcommit: f311f112c9ca711d88a096bed43040fcdad24433
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94981223"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-netsparker-enterprise"></a>教程：Azure Active Directory 单一登录 (SSO) 与 Netsparker Enterprise 集成

本教程介绍如何将 Netsparker Enterprise 与 Azure Active Directory (Azure AD) 进行集成。 将 Netsparker Enterprise 与 Azure AD 集成后，可以：

* 在 Azure AD 中控制谁有权访问 Netsparker Enterprise。
* 让用户使用其 Azure AD 帐户自动登录到 Netsparker Enterprise。
* 在一个中心位置（Azure 门户）管理帐户。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 启用了 Netsparker Enterprise 单一登录 (SSO) 的订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。

* Netsparker Enterprise 支持 SP 和 IDP 发起的 SSO
* Netsparker Enterprise 支持实时用户预配

> [!NOTE]
> 此应用程序的标识符是一个固定字符串值，因此只能在一个租户中配置一个实例。


## <a name="adding-netsparker-enterprise-from-the-gallery"></a>从库中添加 Netsparker Enterprise

若要配置 Netsparker Enterprise 与 Azure AD 的集成，需要从库中将 Netsparker Enterprise 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 Azure 门户。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  。
1. 导航到“企业应用程序”，选择“所有应用程序” 。
1. 若要添加新的应用程序，请选择“新建应用程序”。
1. 在“从库中添加”部分的搜索框中，键入 Netsparker Enterprise 。
1. 从结果面板中选择 Netsparker Enterprise，然后添加该应用。 在该应用添加到租户时等待几秒钟。


## <a name="configure-and-test-azure-ad-sso-for-netsparker-enterprise"></a>配置并测试 Netsparker Enterprise 的 Azure AD SSO

使用名为 B.Simon 的测试用户配置和测试 Netsparker Enterprise 的 Azure AD SSO。 若要使 SSO 正常工作，需要在 Azure AD 用户与 Netsparker Enterprise 相应用户之间建立关联。

若要配置并测试 Netsparker Enterprise 的 Azure AD SSO，请执行以下步骤：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. **[配置 Netsparker Enterprise SSO](#configure-netsparker-enterprise-sso)** - 在应用程序端配置单一登录设置。
    1. **[创建 Netsparker Enterprise 测试用户](#create-netsparker-enterprise-test-user)** - 在 Netsparker Enterprise 中创建 B.Simon 的对应用户，并将其关联到该用户的 Azure AD 表示形式。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

## <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 Azure 门户的 Netsparker Enterprise 应用程序集成页上，找到“管理”部分，然后选择“单一登录”  。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“使用 SAML 设置单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 如果要在“IDP”发起的模式下配置应用程序，请在“基本 SAML 配置”部分中输入以下字段的值   ：

    在“回复 URL”文本框中，使用以下模式键入 URL：`https://www.netsparkercloud.com/account/assertionconsumerservice/?spId=<SPID>`

1. 如果要在 SP  发起的模式下配置应用程序，请单击“设置其他 URL”  ，并执行以下步骤：

    在“登录 URL”文本框中，键入 URL：`https://www.netsparkercloud.com/account/ssosignin/`

    > [!NOTE]
    > 答复 URL 值不是真实值。 请使用实际回复 URL 更新此值。 请联系 [Netsparker Enterprise 客户端支持团队](mailto:support@netsparker.com)获取这些值。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。 

1. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分中，找到“证书(Base64)”，选择“下载”以下载该证书并将其保存到计算机上   。

    ![证书下载链接](common/certificatebase64.png)

1. 在“设置 Netsparker Enterprise”部分中，根据要求复制相应 URL。

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

在本部分中，通过授予 B.Simon 访问 Netsparker Enterprise 的权限，允许其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。 
1. 在应用程序列表中，选择“Netsparker Enterprise”。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。
1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。
1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。
1. 如果你希望将某角色分配给用户，可以从“选择角色”下拉列表中选择该角色。 如果尚未为此应用设置任何角色，你将看到选择了“默认访问权限”角色。
1. 在“添加分配”对话框中，单击“分配”按钮。

## <a name="configure-netsparker-enterprise-sso"></a>配置 Netsparker Enterprise SSO

1.  以管理员身份登录到 Netsparker Enterprise。

1. 转到“设置”>“单一登录”。

1. 在“单一登录”窗口中，选择“Azure Active Directory”选项卡 。 

1. 在下面的页中执行以下步骤。

    ![Azure Active Directory 选项卡](./media/netsparker-enterprise-tutorial/configure-sso.png)

    a. 复制“标识符”值，并将此值粘贴到 Azure 门户上“基本 SAML 配置”部分的“标识符”文本框中  。

    b. 复制“SAML 2.0 服务 URL”值，并将此值粘贴到 Azure 门户上“基本 SAML 配置”部分的“回复 URL”文本框中  。

    c. 在“IdP 标识符”字段中，粘贴从 Azure 门户复制的“标识符”值 。

    e. 在“SAML 2.0 终结点”字段中，粘贴从 Azure 门户复制的“回复 URL”值 。

    f. 在记事本中打开从 Azure 门户下载的“证书(Base64)”，将内容粘贴到“x.509 证书”文本框中 。

    g. 根据要求勾选“启用自动预配”和“要求对 SAML 断言加密” 。

    h. 单击 **“保存更改”** 。

### <a name="create-netsparker-enterprise-test-user"></a>创建 Netsparker Enterprise 测试用户

本部分将在 Netsparker Enterprise 中创建一个名为 Britta Simon 的用户。 Netsparker Enterprise 支持默认启用的实时用户预配。 此部分不存在任何操作项。 如果 Netsparker Enterprise 中尚不存在用户，则会在身份验证后创建一个新用户。

## <a name="test-sso"></a>测试 SSO 

在本部分，你将使用以下选项测试 Azure AD 单一登录配置。 

#### <a name="sp-initiated"></a>SP 启动的：

* 在 Azure 门户中单击“测试此应用程序”。 这会重定向到 Netsparker Enterprise 登录 URL，你可在这里启动登录流。  

* 直接转到 Netsparker Enterprise 登录 URL，并在其中启动登录流。

#### <a name="idp-initiated"></a>IDP 启动的：

* 在 Azure 门户中单击“测试此应用程序”后，你应自动登录到为其设置了 SSO 的 Netsparker Enterprise 

还可以使用 Microsoft 访问面板在任何模式下测试此应用程序。 在单击访问面板中的 Netsparker Enterprise 磁贴时，如果是在 SP 模式下配置的，会重定向到应用程序登录页来启动登录流；如果是在 IDP 模式下配置的，则应会自动登录到为其设置了 SSO 的 Netsparker Enterprise。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。


## <a name="next-steps"></a>后续步骤

配置 Netsparker Enterprise 后，可强制实施会话控制，实时防止组织的敏感数据外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。


