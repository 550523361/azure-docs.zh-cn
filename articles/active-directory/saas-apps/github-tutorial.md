---
title: 教程：Azure Active Directory 与 GitHub 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 GitHub 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.author: jeedes
ms.openlocfilehash: b2a90a4599e5d07baba721d5649b72422dc5cb4d
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818739"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>教程：Azure Active Directory 与 GitHub 集成

本教程介绍如何将 GitHub 与 Azure Active Directory (Azure AD) 集成。

将 GitHub 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 GitHub。
- 可以让用户使用其 Azure AD 帐户自动登录到 GitHub（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 GitHub 的集成，需要以下项：

- Azure AD 订阅
- 启用了 GitHub 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述

在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 GitHub
2. 配置和测试 Azure AD 单一登录

## <a name="adding-github-from-the-gallery"></a>从库中添加 GitHub

要配置 GitHub 与 Azure AD 的集成，需要从库中将 GitHub 添加到托管 SaaS 应用列表。

**若要从库中添加 GitHub，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]

3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

4. 在搜索框中，键入“GitHub”，在结果面板中选择“GitHub”，然后单击“添加”按钮添加该应用程序。

    ![结果列表中的 GitHub](./media/github-tutorial/tutorial_github_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 GitHub 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 GitHub 用户。 换句话说，需要在 Azure AD 用户与 GitHub 中的相关用户之间建立链接关系。

若要配置和测试 GitHub 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
2. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 GitHub 测试用户](#create-a-github-test-user)** - 在 GitHub 中创建 Britta Simon 的对应用户，并将其链接到用户的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
5. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，会在 Azure 门户中启用 Azure AD 单一登录并在 GitHub 应用程序中配置单一登录。

**若要配置 GitHub 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 GitHub 应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

2. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。

    ![“单一登录”对话框](./media/github-tutorial/tutorial_github_samlbase.png)

3. 在“GitHub 域和 URL”部分中，执行以下步骤：

    ![GitHub 域和 URL 单一登录信息](./media/github-tutorial/tutorial_github_url.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL：`https://github.com/orgs/<entity-id>/sso`

    b. 在“标识符(实体 ID)”文本框中，使用以下模式键入 URL： `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > 请注意，这些不是实际值。 必须使用实际登录 URL 和标识符更新这些值。 此处我们建议在“标识符”中使用字符串的唯一值。 转到“GitHub 管理”部分检索这些值。

4. 在“用户属性”部分中，为“用户标识符”选择“user.mail”。

    ![配置单一登录](./media/github-tutorial/tutorial_github_attribute_new01.png)

5. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![证书下载链接](./media/github-tutorial/tutorial_github_certificate.png) 

6. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/github-tutorial/tutorial_general_400.png)

7. 在“GitHub 配置”部分中，单击“配置 GitHub”打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”。

    ![GitHub 配置](./media/github-tutorial/tutorial_github_configure.png) 

8. 在另一个 Web 浏览器窗口中，以管理员身份登录到 GitHub 组织站点。

9. 导航“设置”并单击“安全性”。

    ![设置](./media/github-tutorial/tutorial_github_config_github_03.png)

10. 选中“启用 SAML 身份验证”框，显示“单一登录”配置字段。 然后，使用单一登录 URL 值更新 Azure AD 配置中的单一登录 URL。

    ![设置](./media/github-tutorial/tutorial_github_config_github_13.png)

11. 配置以下字段：

    a. 在“登录 URL”文本框中，粘贴从 Azure 门户复制的“SAML 单一登录服务 URL”值。

    b. 将从 Azure 门户复制的“SAML 实体 ID”值粘贴到“颁发者”文本框中。

    c. 在记事本中打开从 Azure 门户下载的证书，将内容粘贴到“公共证书”文本框中。

    ![设置](./media/github-tutorial/tutorial_github_config_github_051.png)

12. 单击“测试 SAML 配置”，确认在 SSO 期间未发生验证失败错误。

    ![设置](./media/github-tutorial/tutorial_github_config_github_06.png)

13. 单击“保存”

> [!NOTE]
> GitHub 中的单一登录向 GitHub 中的特定组织进行身份验证，并不替换 GitHub 本身的身份验证。 因此，如果用户的 GitHub.com 会话已过期，可能会要求你在单一登录过程中使用 GitHub 的 ID/密码进行身份验证。

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/github-tutorial/create_aaduser_01.png)

2. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/github-tutorial/create_aaduser_02.png)

3. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/github-tutorial/create_aaduser_03.png)

4. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/github-tutorial/create_aaduser_04.png)

    a. 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。

### <a name="create-a-github-test-user"></a>创建 GitHub 测试用户

本部分的目的是在 GitHub 中创建名为“Britta Simon”的用户。 GitHub 支持在默认情况下启用的自动用户预配。 有关如何配置自动用户预配的更多详细信息，请参见[此处](github-provisioning-tutorial.md)。

如果需要手动创建用户，请执行以下步骤：

1. 以管理员身份登录到 GitHub 公司站点。

2. 单击“人员”。

    ![人员](./media/github-tutorial/tutorial_github_config_github_08.png "人员")

3. 单击“邀请成员”。

    ![邀请用户](./media/github-tutorial/tutorial_github_config_github_09.png "邀请用户")

4. 在“邀请成员”对话框页上，执行以下步骤：

    a. 在“电子邮件”文本框中，键入 Britta Simon 帐户的电子邮件地址。

    ![邀请人员](./media/github-tutorial/tutorial_github_config_github_10.png "邀请人员")

    b. 单击“发送邀请”。

    ![邀请人员](./media/github-tutorial/tutorial_github_config_github_11.png "邀请人员")

    > [!NOTE]
    > Azure Active Directory 帐户持有者将收到一封电子邮件，并且将单击其中的链接以在激活帐户前确认帐户。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

本部分通过授予 Britta Simon 访问 GitHub 的权限，允许其使用 Azure 单一登录。

![分配用户角色][200] 

**要将 Britta Simon 分配到 GitHub，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201]

2. 在应用程序列表中，选择“GitHub”。

    ![应用程序列表中的 GitHub 链接](./media/github-tutorial/tutorial_github_app.png)  

3. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

5. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

6. 在“用户和组”对话框中单击“选择”按钮。

7. 在“添加分配”对话框中单击“分配”按钮。

### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 GitHub 磁贴时，应自动登录到 GitHub 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/github-tutorial/tutorial_general_01.png
[2]: ./media/github-tutorial/tutorial_general_02.png
[3]: ./media/github-tutorial/tutorial_general_03.png
[4]: ./media/github-tutorial/tutorial_general_04.png

[100]: ./media/github-tutorial/tutorial_general_100.png

[200]: ./media/github-tutorial/tutorial_general_200.png
[201]: ./media/github-tutorial/tutorial_general_201.png
[202]: ./media/github-tutorial/tutorial_general_202.png
[203]: ./media/github-tutorial/tutorial_general_203.png