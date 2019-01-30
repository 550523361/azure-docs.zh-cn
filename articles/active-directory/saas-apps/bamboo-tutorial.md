---
title: 教程：Azure Active Directory 与 SAML SSO for Bamboo by resolution GmbH 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 SAML SSO for Bamboo by resolution GmbH 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: f00160c7-f4cc-43bf-af18-f04168d3767c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.openlocfilehash: 6fb5b67c5df54fc5edfb14e0392e14fc1be239a6
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811190"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bamboo-by-resolution-gmbh"></a>教程：Azure Active Directory 与 SAML SSO for Bamboo by resolution GmbH 的集成

本教程介绍如何将 SAML SSO for Bamboo by resolution GmbH 与 Azure Active Directory (Azure AD) 集成。

将 SAML SSO for Bamboo by resolution GmbH 与 Azure AD 集成具有以下优势：

- 可以在 Azure AD 中控制谁有权访问 SAML SSO for Bamboo by resolution GmbH。
- 可以让用户通过其 Azure AD 帐户自动登录到 SAML SSO for Bamboo by resolution GmbH（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 SAML SSO for Bamboo by resolution GmbH 的集成，需要以下各项：

- Azure AD 订阅
- 一个启用了 SAML SSO for Bamboo by resolution GmbH 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库添加 SAML SSO for Bamboo by resolution GmbH
1. 配置和测试 Azure AD 单一登录

## <a name="adding-saml-sso-for-bamboo-by-resolution-gmbh-from-the-gallery"></a>从库添加 SAML SSO for Bamboo by resolution GmbH
要配置 SAML SSO for Bamboo by resolution GmbH 的集成（集成到 Azure AD 中），需从库中将 SAML SSO for Bamboo by resolution GmbH 添加到托管 SaaS 应用的列表中。

**若要从库中添加 SAML SSO for Bamboo by resolution GmbH，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

1. 在搜索框中，键入 SAML SSO for Bamboo by resolution GmbH，在结果面板中选择 SAML SSO for Bamboo by resolution GmbH，然后单击“添加”按钮以添加该应用程序。

    ![结果列表中的 SAML SSO for Bamboo by resolution GmbH](./media/bamboo-tutorial/tutorial_bamboo_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 SAML SSO for Bamboo by resolution GmbH 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 SAML SSO for Bamboo by resolution GmbH 用户。 换句话说，需要在 Azure AD 用户与 SAML SSO for Bamboo by resolution GmbH 中相关用户之间建立链接关系。

在 SAML SSO for Bamboo by resolution GmbH 中，将 Azure AD 中“用户名”的值指定为“用户名”值来建立此链接关系。

若要配置并测试 SAML SSO for Jira by resolution GmbH 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 SAML SSO for Bamboo by resolution GmbH 测试用户](#create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user)** - 在 SAML SSO for Bamboo by resolution GmbH 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

本部分介绍了在 Azure 门户中启用 Azure AD 单一登录，并在 SAML SSO for Bamboo by resolution GmbH 应用程序中配置单一登录。

**若要配置 SAML SSO for Bamboo by resolution GmbH 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中，在 SAML SSO for Bamboo by resolution GmbH 应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/bamboo-tutorial/tutorial_bamboo_samlbase.png)

1. 如果想在 IDP 启动模式下配置应用程序，请在“SAML SSO for Bamboo by resolution GmbH 域和 URL”部分执行下列步骤：

    ![SAML SSO for Bamboo by resolution GmbH 域和 URL 单一登录信息](./media/bamboo-tutorial/tutorial_bamboo_url.png)

    a. 在“标识符”文本框中，使用以下模式键入 URL：`https://<server-base-url>/plugins/servlet/samlsso`

    b. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://<server-base-url>/plugins/servlet/samlsso`

1. 如果要在 SP 发起的模式下配置应用程序，请选中“显示高级 URL 设置”，并执行以下步骤：

    ![SAML SSO for Bamboo by resolution GmbH 域和 URL 单一登录信息](./media/bamboo-tutorial/tutorial_bamboo_url1.png)

    在“登录 URL”文本框中，使用以下模式键入 URL： `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > 这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 若要获取这些值，请联系 [SAML SSO for Bamboo by resolution GmbH 客户端支持团队](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support)。 

1. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存元数据文件。

    ![证书下载链接](./media/bamboo-tutorial/tutorial_bamboo_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/bamboo-tutorial/tutorial_general_400.png)

1. 以管理员身份登录到 SAML SSO for Bamboo by resolution GmbH 公司站点。

1. 在主工具栏的右侧，单击“设置” > “加载项”。

    ![设置](./media/bamboo-tutorial/tutorial_bamboo_setings.png)

1. 转到“安全性”部分，在菜单栏上单击“SAML 单一登录”。

    ![Samlsingle](./media/bamboo-tutorial/tutorial_bamboo_samlsingle.png)

1. 在“SAML 单一登录插件配置”页上，单击“添加 idp”。 

    ![添加 idp](./media/bamboo-tutorial/tutorial_bamboo_addidp.png)

1. 在“选择 SAML 标识提供者”页上，执行以下步骤：

    ![标识提供者](./media/bamboo-tutorial/tutorial_bamboo_identityprovider.png)

    a. 选择“Idp 类型”作为“AZURE AD”。

    b. 在“名称”文本框中，键入名称。

    c. 在“说明”文本框中，键入说明。

    d. 单击“下一步”。

1. 在“标识提供者配置”页上，单击“下一步”。

    ![标识配置](./media/bamboo-tutorial/tutorial_bamboo_identityconfig.png)

1.  在“导入 SAML Idp 元数据”页上，单击“加载文件”以上传从 Azure 门户下载的“元数据 XML”文件。

    ![Idpmetadata](./media/bamboo-tutorial/tutorial_bamboo_idpmetadata.png)

1. 单击“下一步”。

1. 单击“保存设置”。

    ![保存](./media/bamboo-tutorial/tutorial_bamboo_save.png)
    
> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/bamboo-tutorial/create_aaduser_01.png)

1. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/bamboo-tutorial/create_aaduser_02.png)

1. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/bamboo-tutorial/create_aaduser_03.png)

1. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/bamboo-tutorial/create_aaduser_04.png)

    a. 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。
 
### <a name="create-a-saml-sso-for-bamboo-by-resolution-gmbh-test-user"></a>创建 SAML SSO for Bamboo by resolution GmbH 测试用户

本部分的目的是在 SAML SSO for Bamboo by resolution GmbH 中创建名为“Britta Simon”的用户。 SAML SSO for Bamboo by resolution GmbH 支持实时预配，也可以手动创建用户，具体请根据要求联系 [SAML SSO for Bamboo by resolution GmbH 支持团队](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bamboo/server/support)。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 SAML SSO for Bamboo by resolution GmbH 的权限，允许其使用 Azure 单一登录。

![分配用户角色][200] 

**要将 Britta Simon 分配到 SAML SSO for Bamboo by resolution GmbH，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择 SAML SSO for Bamboo by resolution GmbH。

    ![应用程序列表中的 SAML SSO for Bamboo by resolution GmbH 链接](./media/bamboo-tutorial/tutorial_bamboo_app.png)  

1. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击 SAML SSO for Bamboo by resolution GmbH ，便会自动登录到 SAML SSO for Bamboo by resolution GmbH 应用程序。
有关访问面板的详细信息，请参阅[访问面板简介](../user-help/active-directory-saas-access-panel-introduction.md)。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bamboo-tutorial/tutorial_general_01.png
[2]: ./media/bamboo-tutorial/tutorial_general_02.png
[3]: ./media/bamboo-tutorial/tutorial_general_03.png
[4]: ./media/bamboo-tutorial/tutorial_general_04.png

[100]: ./media/bamboo-tutorial/tutorial_general_100.png

[200]: ./media/bamboo-tutorial/tutorial_general_200.png
[201]: ./media/bamboo-tutorial/tutorial_general_201.png
[202]: ./media/bamboo-tutorial/tutorial_general_202.png
[203]: ./media/bamboo-tutorial/tutorial_general_203.png

