---
title: 教程：Azure Active Directory 与 ThousandEyes 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 ThousandEyes 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 06de55fd2bd1064e93e38fe0ff0cfb13fdc1463a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175428"
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>教程：Azure Active Directory 与 ThousandEyes 集成

本教程介绍如何将 ThousandEyes 与 Azure Active Directory (Azure AD) 集成。

将 ThousandEyes 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 ThousandEyes
- 可以让用户使用其 Azure AD 帐户自动登录到 ThousandEyes（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 ThousandEyes 的集成，需要以下项：

- Azure AD 订阅
- 已启用 ThousandEyes 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在此处获取一个月的试用版：[试用版产品/服务](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库添加 ThousandEyes
1. 配置和测试 Azure AD 单一登录

## <a name="adding-thousandeyes-from-the-gallery"></a>从库添加 ThousandEyes
要配置 ThousandEyes 与 Azure AD 的集成，需从库中将 ThousandEyes 添加到托管 SaaS 应用列表。

**若要从库中添加 ThousandEyes，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“ThousandEyes”。

    ![创建 Azure AD 测试用户](./media/thousandeyes-tutorial/tutorial_thousandeyes_search.png)

1. 在结果面板中，选择“ThousandEyes”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分中，将基于名为“Britta Simon”的测试用户配置并测试 ThousandEyes 的 Azure AD 单一登录。

为使单一登录能正常工作，Azure AD 需要知道 ThousandEyes 中与 Azure AD 用户相对应的用户。 换句话说，需要建立起 Azure AD 用户与 ThousandEyes 中的相关用户之间的关联。

通过将 ThousandEyes 中“用户名”的值指定为 Azure AD 中“用户名”的值来建立此关联。

若要配置并测试 ThousandEyes 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 ThousandEyes 测试用户](#creating-a-thousandeyes-test-user)** - 在 ThousandEyes 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 ThousandEyes 应用程序中配置单一登录。

**若要配置 ThousandEyes 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 ThousandEyes 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。

    ![配置单一登录](./media/thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

1. 在“ThousandEyes 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    在“登录 URL”文本框中，键入 URL `https://app.thousandeyes.com/login/sso`

1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![配置单一登录](./media/thousandeyes-tutorial/tutorial_thousandeyes_certificate.png)

1. 单击“保存”按钮。

    ![配置单一登录](./media/thousandeyes-tutorial/tutorial_general_400.png)

1. 在“ThousandEyes 配置”部分中，单击“配置 ThousandEyes”打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”。

    ![配置单一登录](./media/thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

1. 在另一个 Web 浏览器窗口中，以管理员身份登录 **ThousandEyes** 公司站点。

1. 在顶部菜单中，单击“设置”。

    ![设置](./media/thousandeyes-tutorial/ic790066.png "设置")

1. 单击“帐户”

    ![帐户](./media/thousandeyes-tutorial/ic790067.png "Account")

1. 单击“安全性和身份验证”选项卡。

    ![安全性和身份验证](./media/thousandeyes-tutorial/ic790068.png "安全性和身份验证")

1. 在“设置单一登录”部分中，执行以下步骤：

    ![设置单一登录](./media/thousandeyes-tutorial/ic790069.png "设置单一登录")

    a. 选择“启用单一登录”。

    b. 在“登录页 URL”文本框中，粘贴从 Azure 门户复制的“SAML 单一登录服务 URL”。

    c. 在“注销页面 URL”文本框中，粘贴从 Azure 门户复制的“注销 URL”。

    d. 在“标识提供程序颁发者”文本框中，粘贴从 Azure 门户复制的“SAML 实体 ID”。

    e. 在“验证证书”中，单击“选择文件”，然后上传从 Azure 门户下载的证书。

    f. 单击“ **保存**”。

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/thousandeyes-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/thousandeyes-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。

    ![创建 Azure AD 测试用户](./media/thousandeyes-tutorial/create_aaduser_03.png)

1. 在“用户”对话框页上，执行以下步骤：

    ![创建 Azure AD 测试用户](./media/thousandeyes-tutorial/create_aaduser_04.png)

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。

### <a name="creating-a-thousandeyes-test-user"></a>创建 ThousandEyes 测试用户

本部分的目的是在 ThousandEyes 中创建名为“Britta Simon”的用户。 ThousandEyes 支持在默认情况下启用的自动用户预配。 有关如何配置自动用户预配的更多详细信息，请参见[此处](thousandeyes-provisioning-tutorial.md)。

如果需要手动创建用户，请执行以下步骤：

1. 以管理员身份登录 ThousandEyes 公司站点。

1. 单击“设置”。

    ![设置](./media/thousandeyes-tutorial/IC790066.png "设置")

1. 单击“帐户”。

    ![帐户](./media/thousandeyes-tutorial/IC790067.png "Account")

1. 单击“帐户和用户”选项卡。

    ![帐户和用户](./media/thousandeyes-tutorial/IC790073.png "帐户和用户")

1. 在“添加用户和帐户”部分中，执行以下步骤：

    ![添加用户帐户](./media/thousandeyes-tutorial/IC790074.png "添加用户帐户")

    a. 在“名称”文本框中，键入用户名（如 Britta Simon）。

    b. 在“电子邮件地址”文本框中，键入用户的电子邮件地址（如 brittasimon@contoso.com）。

    b. 单击“将新用户添加到帐户”。

    > [!NOTE]
    > Azure Active Directory 帐户持有者会收到一封电子邮件，其中包含用于确认和激活帐户的链接。

> [!NOTE]
> 可使用任何其他 ThousandEyes 用户帐户创建工具或使用 ThousandEyes 提供的 API 来预配 Azure Active Directory 用户帐户。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 ThousandEyes 的权限，使她能够使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 ThousandEyes，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“ThousandEyes”。

    ![配置单一登录](./media/thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击 ThousandEyes 磁贴时，应会自动登录到 ThousandEyes 应用程序。

有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)
* [配置用户预配](thousandeyes-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/thousandeyes-tutorial/tutorial_general_203.png
