---
title: 教程：Azure Active Directory 与 ScaleX Enterprise 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 ScaleX Enterprise 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64edf2aa47211c1d2a598417a7b2edc00f260075
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "60321055"
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>教程：Azure Active Directory 与 ScaleX Enterprise 的集成

本教程介绍了如何将 ScaleX Enterprise 与 Azure Active Directory (Azure AD) 进行集成。

将 ScaleX Enterprise 与 Azure AD 集成可提供以下优势：

- 可以在 Azure AD 中控制谁有权访问 ScaleX Enterprise
- 可以让用户使用其 Azure AD 帐户自动登录到 ScaleX Enterprise（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

若要了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅。 [Azure Active Directory](../manage-apps/what-is-single-sign-on.md) 的应用程序访问与单一登录是什么。

## <a name="prerequisites"></a>必备组件

若要配置 Azure AD 与 ScaleX Enterprise 的集成，需要具有以下项：

- Azure AD 订阅
- 启用了 ScaleX Enterprise 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 ScaleX Enterprise
1. 配置和测试 Azure AD 单一登录

## <a name="adding-scalex-enterprise-from-the-gallery"></a>从库中添加 ScaleX Enterprise
要配置 ScaleX Enterprise 与 Azure AD 的集成，需要从库中将 ScaleX Enterprise 添加到托管 SaaS 应用列表。

**若要从库中添加 ScaleX Enterprise，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”  图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。  然后转到“所有应用程序”  。

    ![应用程序][2]
    
1. 单击  对话框顶部的“添加”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“ScaleX Enterprise”。 

    ![创建 Azure AD 测试用户](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

1. 在结果窗格中，选择“ScaleX Enterprise”  ，并单击“添加”  按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于名为“Britta Simon”的测试用户配置并测试 ScaleX Enterprise 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 ScaleX Enterprise 用户。 换句话说，需要在 Azure AD 用户与 ScaleX Enterprise 中的相关用户之间建立链接关系。

可以通过将 Azure AD 中“用户名”的值分配为 ScaleX Enterprise 中“用户名”的值来建立此链接关系。  

若要配置并测试 ScaleX Enterprise 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 ScaleX Enterprise 测试用户](#creating-a-scalex-enterprise-test-user)** - 在 ScaleX Enterprise 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，会在 Azure 门户中启用 Azure AD 单一登录并在 ScaleX Enterprise 应用程序中配置单一登录。

**若要配置 ScaleX Enterprise 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中，在 **ScaleX Enterprise** 应用程序集成页上，单击“单一登录”。 

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的登录”作为“模式”以启用单一登录。   
 
    ![配置单一登录](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

1. 在“ScaleX Enterprise 域和 URL”  部分中，如果要在 **IDP** 启动的模式下配置应用程序，请执行以下步骤：

    ![配置单一登录](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. 在“标识符”  文本框中，使用以下模式键入值：`https://platform.rescale.com/saml2/<company id>/`

    b. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://platform.rescale.com/saml2/<company id>/acs/`

1. 如果要在“SP”  发起的模式下配置应用程序，请选中“显示高级 URL 设置”  ：

    ![配置单一登录](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    在“登录 URL”  文本框中，使用以下模式键入值：`https://platform.rescale.com/saml2/<company id>/sso/`
     
    > [!NOTE] 
    > 这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 请联系 [ScaleX Enterprise 客户端支持团队](https://info.rescale.com/contact_sales)来获取这些值。 

1. ScaleX 应用程序需要特定格式的 SAML 断言，这要求修改与 SAML 令牌属性配置之间的自定义属性映射。 单击“查看和编辑所有其他用户属性”复选框以打开自定义属性设置。 

    ![配置单一登录](./media/scalexenterprise-tutorial/scalex_attributes.png)
    
    a. 右键单击属性**名称**并单击“删除”。

    ![配置单一登录](./media/scalexenterprise-tutorial/delete_attribute_name.png)

    b. 单击“emailaddress”属性以打开“编辑属性”窗口。  将其值从“user.mail”更改为“user.userprincipalname”并单击“确定”。  

    ![配置单一登录](./media/scalexenterprise-tutorial/edit_email_attribute.png) 
    
1. 在“SAML 签名证书”部分中，单击“证书(Base64)”，并在计算机上保存证书文件。  

    ![配置单一登录](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

1. 单击“保存”按钮  。

    ![配置单一登录](./media/scalexenterprise-tutorial/tutorial_general_400.png)
    
1. 在“ScaleX Enterprise 配置”部分中，单击“配置 ScaleX Enterprise”以打开“配置登录”窗口。    从“快速参考”部分中复制“SAML 实体 ID 和 SAML 单一登录服务 URL”。   

    ![配置单一登录](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

1. 若要在 **ScaleX Enterprise** 端配置单一登录，请以管理员身份登录到 ScaleX Enterprise 公司网站。

1. 单击右上角的菜单并选择“Contoso 管理”。 

    > [!NOTE] 
    > Contoso 只是一个示例。 这应当是实际公司名称。 

    ![配置单一登录](./media/scalexenterprise-tutorial/Test_Admin.png) 

1. 从顶部的菜单中选择“集成”并选择“单一登录”。  

    ![配置单一登录](./media/scalexenterprise-tutorial/admin_sso.png) 

1. 如下所示填写表单：

    ![配置单一登录](./media/scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. 选择“创建可以通过 SSO 进行身份验证的任何用户”。 

    b. **服务提供商 saml**：粘贴 urn:oasis:names:tc:SAML:2.0:nameid-format:persistent 值

    c. **ACS 响应中的标识提供者名称电子邮件字段**：粘贴 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` 值

    d. **标识提供者 EntityDescriptor 实体 ID：** 粘贴从 Azure 门户复制的 SAML 实体 ID  。

    e. **标识提供者 SingleSignOnService URL：** 粘贴 Azure 门户中的 SAML 单一登录服务 URL  。

    f. **标识提供者公共 X509 证书：** 在记事本中打开从 Azure 下载的 X509 证书并将内容粘贴到此框中。 请确保证书内容的中间没有换行符。
    
    g. 选中以下复选框：“已启用”、“加密 NameID”和“对 AuthnRequest 进行签名”。 

    h. 单击“更新 SSO 设置”以保存设置。 

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”  部分添加此应用后，只需单击“单一登录”  选项卡，即可通过底部的“配置”  部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”  图标。

    ![创建 Azure AD 测试用户](./media/scalexenterprise-tutorial/create_aaduser_01.png) 

1. 转到“用户和组”  ，单击“所有用户”  显示用户列表。
    
    ![创建 Azure AD 测试用户](./media/scalexenterprise-tutorial/create_aaduser_02.png) 

1. 在对话框顶部，单击“添加”以打开“用户”对话框。  
 
    ![创建 Azure AD 测试用户](./media/scalexenterprise-tutorial/create_aaduser_03.png) 

1. 在“用户”  对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/scalexenterprise-tutorial/create_aaduser_04.png) 

    a. 在“名称”  文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。  

    c. 选择“显示密码”  并记下“密码”的值  。

    d. 单击**创建**。
 
### <a name="creating-a-scalex-enterprise-test-user"></a>创建 ScaleX Enterprise 测试用户

为了使 Azure AD 用户能够登录到 ScaleX Enterprise，必须将其预配到 ScaleX Enterprise 中。 对于 ScaleX Enterprise，预配是自动执行的任务，不需要手动执行任何步骤。 在 ScaleX 端会自动预配可以通过 SSO 凭据成功进行身份验证的任何用户。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，将通过向 Britta Simon 授予对 ScaleX Enterprise 的访问权限使她能够使用 Azure 单一登录。

![分配用户][200] 

**要将 Britta Simon 分配到 ScaleX Enterprise，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”  ，并单击“所有应用程序”  。

    ![分配用户][201] 

1. 在应用程序列表中，选择“ScaleX Enterprise”  。

    ![配置单一登录](./media/scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

1. 在左侧菜单中，单击“用户和组”  。

    ![分配用户][202] 

1. 单击“添加”按钮。  然后在“添加分配”对话框中选择“用户和组”。  

    ![分配用户][203]

1. 在“用户和组”  对话框的“用户”列表中，选择“Britta Simon”。 

1. 在“用户和组”对话框中单击“选择”按钮。  

1. 在“添加分配”对话框中单击“分配”按钮。  

### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 ScaleX Enterprise 磁贴时，会自动登录到 ScaleX Enterprise 应用程序。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。


## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/scalexenterprise-tutorial/tutorial_general_203.png

