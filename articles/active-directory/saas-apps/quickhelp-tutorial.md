---
title: 教程：Azure Active Directory 与 QuickHelp 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 QuickHelp 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.openlocfilehash: cbc25218079f8e8529777dd8e169a2e689eabc6b
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54824212"
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>教程：Azure Active Directory 与 QuickHelp 的集成

本教程介绍如何将 QuickHelp 与 Azure Active Directory (Azure AD) 集成。

将 QuickHelp 与 Azure AD 集成提供以下优势：

- 可在 Azure AD 中控制谁有权访问 QuickHelp
- 可以让用户使用其 Azure AD 帐户自动登录到 QuickHelp（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 QuickHelp 的集成，需备齐以下项目：

- Azure AD 订阅
- 已启用 QuickHelp 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 QuickHelp
1. 配置和测试 Azure AD 单一登录

## <a name="adding-quickhelp-from-the-gallery"></a>从库中添加 QuickHelp
要配置 QuickHelp 与 Azure AD 的集成，需要从库中将 QuickHelp 添加到托管 SaaS 应用列表。

**若要从库中添加 QuickHelp，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“QuickHelp”。

    ![创建 Azure AD 测试用户](./media/quickhelp-tutorial/tutorial_quickhelp_search.png)

1. 在结果面板中，选择“QuickHelp”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于一个名为“Britta Simon”的测试用户配置并测试 QuickHelp 的 Azure AD 单一登录。

为使单一登录能正常工作，Azure AD 需要知道与 Azure AD 用户相对应的 QuickHelp 用户。 换句话说，需要在 Azure AD 用户与 QuickHelp 中相关用户之间建立链接关系。

可通过将 Azure AD 中“用户名”的值指定为 QuickHelp 中“用户名”的值来建立此关联关系。

若要配置和测试 QuickHelp 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. [创建 QuickHelp 测试用户](#creating-a-quickhelp-test-user) - 在 QuickHelp 中有一个与 Azure AD 中的 Britta Simon 相对应的关联用户。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 QuickHelp 应用程序中配置单一登录。

**若要使用 QuickHelp 配置 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 QuickHelp 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

1. 在“QuickHelp 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/quickhelp-tutorial/tutorial_quickhelp_url.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL： `https://quickhelp.com/<ROUTEURL>`

    b. 在“标识符”文本框中，键入一个 URL：`https://auth.quickhelp.com`

    > [!NOTE] 
    > 登录 URL 值不是实际值。 请使用实际登录 URL 更新此值。 联系组织的 QuickHelp 管理员或 BrainStorm 客户成功经理以获取此值。
 
1. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存元数据文件。

    ![配置单一登录](./media/quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录](./media/quickhelp-tutorial/tutorial_general_400.png) 

1. 以管理员身份登录 QuickHelp 公司站点。

1. 在顶部菜单中，单击“管理员”。
   
    ![配置单一登录][21]

1. 在“QuickHelp 管理员”菜单上，单击“设置”。
   
    ![配置单一登录][22]

1. 单击“身份验证设置”。

1. 在“身份验证设置”页上，执行以下步骤
   
    ![配置单一登录][23]
   
    a. 对于“SSO 类型”，选择“WSFederation”。
   
    b. 要上载已下载的 Azure 元数据文件，请单击“浏览”，导航到该文件，并单击“上载元数据”。
   
    c. 在“电子邮件”文本框中，键入 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`。
   
    d. 在“名字”文本框中，`type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`。
   
    e. 在“姓氏”文本框中，`type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`。
   
    f. 在“操作栏”中，单击“保存”。

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/quickhelp-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/quickhelp-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/quickhelp-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/quickhelp-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-quickhelp-test-user"></a>创建 QuickHelp 测试用户

本部分的目的是在 QuickHelp 中创建名为“Britta Simon”的用户。
为使单一登录能正常工作，Azure AD 需要知道与 Azure AD 用户相对应的 QuickHelp 用户。 换句话说，需要在 Azure AD 用户与 QuickHelp 中相关用户之间建立链接关系。

QuickHelp 支持实时预配。 这意味着，如有必要，会在 QuickHelp 中自动创建用户帐户，并且该帐户与 Azure AD 帐户关联。

此部分不存在任何操作项。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 QuickHelp 的权限，允许她使用 Azure 单一登录。

![分配用户][200] 

**要将 Britta Simon 分配到 QuickHelp，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“QuickHelp”。

    ![配置单一登录](./media/quickhelp-tutorial/tutorial_quickhelp_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

本部分旨在使用“访问面板”测试 Azure AD 单一登录配置。  

单击访问面板中的“QuickHelp”磁贴时，用户应自动登录到 QuickHelp 应用程序。


## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/quickhelp-tutorial/tutorial_quickhelp_07.png
