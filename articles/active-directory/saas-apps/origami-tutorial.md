---
title: 教程：Azure Active Directory 与 Origami 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Origami 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b3587acd0232920e812512384af3158a1e0afbe
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191361"
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a>教程：Azure Active Directory 与 Origami 集成

本教程介绍如何将 Origami 与 Azure Active Directory (Azure AD) 集成。

将 Origami 与 Azure AD 集成具有以下优势：

- 可在 Azure AD 中控制谁有权访问 Origami
- 可以让用户通过其 Azure AD 帐户自动登录到 Origami（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Origami 的集成，需备齐以下项目：

- Azure AD 订阅
- 启用 Origami 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Origami
1. 配置和测试 Azure AD 单一登录

## <a name="adding-origami-from-the-gallery"></a>从库中添加 Origami
要通过配置将 Origami 集成到 Azure AD 中，需从库将 Origami 添加到托管式 SaaS 应用的列表中。

**若要从库添加 Origami，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“Origami”。

    ![创建 Azure AD 测试用户](./media/origami-tutorial/tutorial_origami_search.png)

1. 在结果窗格中，选择“Origami”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/origami-tutorial/tutorial_origami_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分需根据名为“Britta Simon”的测试用户的情况，配置和测试 Origami 的 Azure AD 单一登录。

若要使用单一登录，Azure AD 需要了解与 Azure AD 中的用户相对应的 Origami 中的用户是谁。 换句话说，需要建立 Azure AD 用户与 Origami 中相关用户之间的关联关系。

可通过将 Azure AD 中“用户名”的值指定为 Origami 中“用户名”的值来建立此链接关系。

若要使用 Origami 配置和测试 Azure AD 单一登录，需完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. [创建 Origami 测试用户](#creating-an-origami-test-user) - 目的是在 Origami 中有一个与 Azure AD 中的 Britta Simon 相对应的关联用户。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

本部分需在 Azure 门户中启用 Azure AD 单一登录，并在 Origami 应用程序中配置单一登录。

**若要通过 Origami 配置 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 Origami 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/origami-tutorial/tutorial_origami_samlbase.png)

1. 在“Origami 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/origami-tutorial/tutorial_origami_url.png)

    在“登录 URL”文本框中，使用以下模式键入 URL： `https://live.origamirisk.com/origami/account/login?account=<companyname>`

    > [!NOTE] 
    > 此值不是真实值。 请使用实际登录 URL 更新此值。 请联系 [Origami 客户端支持团队](https://wordpress.org/support/theme/origami)获取此值。 
 
1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![配置单一登录](./media/origami-tutorial/tutorial_origami_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录](./media/origami-tutorial/tutorial_general_400.png)

1. 在“Origami 配置”部分，单击“配置 Origami”打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL 和 SAML 单一登录服务 URL”。

    ![配置单一登录](./media/origami-tutorial/tutorial_origami_configure.png) 

1. 使用管理员权限登录到 Origami 帐户。

1. 在顶部菜单中，单击“管理员”。
   
    ![配置单一登录](./media/origami-tutorial/tutorial_origami_51.png)

1. 在“单一登录设置”对话框页上，执行以下步骤：
   
    ![配置单一登录](./media/origami-tutorial/tutorial_origami_531.png)

    a. 选择“启用单一登录”。

    b. 在“标识提供者登录页 URL”文本框中，粘贴从 Azure 门户复制的 SAML 单一登录服务 URL 值。

    c. 在“标识提供者注销页 URL”文本框中，粘贴从 Azure 门户复制的注销 URL 值。

    d. 单击“浏览”上传从 Azure 门户下载的证书。

    e. 单击“保存更改”。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/origami-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/origami-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/origami-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/origami-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-an-origami-test-user"></a>创建 Origami 测试用户

本部分需在 Origami 中创建名为“Britta Simon”的用户。 

1. 使用管理员权限登录到 Origami 帐户。

1. 在顶部菜单中，单击“管理员”。
   
    ![配置单一登录](./media/origami-tutorial/tutorial_origami_51.png)

1. 在“用户和安全”对话框中，单击“用户”。
   
    ![配置单一登录](./media/origami-tutorial/tutorial_origami_54.png)

1. 单击“添加新用户”。
   
    ![配置单一登录](./media/origami-tutorial/tutorial_origami_55.png)

1. 在“添加新用户”对话框中，执行以下步骤：
   
    ![配置单一登录](./media/origami-tutorial/tutorial_origami_56.png)

    a. 在“用户名”文本框中，键入用户的电子邮件地址（例如 brittasimon@contoso.com）。

    b. 在“密码”文本框中，键入密码。

    c. 在“确认密码”文本框中，再次键入该密码。

    d. 在“名字”文本框中，输入用户的名字（如“Britta”）。

    e. 在“姓氏”文本框中，输入用户的姓氏（如“Simon”）。

    f. 单击“ **保存**”。
   
    ![配置单一登录](./media/origami-tutorial/tutorial_origami_57.png)

1. 向用户分配“用户角色”和“客户端访问权限”。 
   
    ![配置单一登录](./media/origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

本部分需授予 Britta Simon 访问 Origami 的权限，使之能够使用 Azure 单一登录。

![分配用户][200] 

**要将 Britta Simon 分配到 Origami，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“Origami”。

    ![配置单一登录](./media/origami-tutorial/tutorial_origami_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的“Origami”磁贴时，用户就会自动登录到 Origami 应用程序。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/origami-tutorial/tutorial_general_01.png
[2]: ./media/origami-tutorial/tutorial_general_02.png
[3]: ./media/origami-tutorial/tutorial_general_03.png
[4]: ./media/origami-tutorial/tutorial_general_04.png

[100]: ./media/origami-tutorial/tutorial_general_100.png

[200]: ./media/origami-tutorial/tutorial_general_200.png
[201]: ./media/origami-tutorial/tutorial_general_201.png
[202]: ./media/origami-tutorial/tutorial_general_202.png
[203]: ./media/origami-tutorial/tutorial_general_203.png

