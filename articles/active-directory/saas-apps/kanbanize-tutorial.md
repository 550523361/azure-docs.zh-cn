---
title: 教程：Azure Active Directory 与 Kanbanize 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 Kanbanize 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b436d2f6-bfa5-43fd-a8f9-d2144dc25669
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22c136225e5a8526afd482e5ef8400198947422f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "60264234"
---
# <a name="tutorial-azure-active-directory-integration-with-kanbanize"></a>教程：Azure Active Directory 与 Kanbanize 集成

本教程介绍如何将 Kanbanize 与 Azure Active Directory (Azure AD) 集成。

将 Kanbanize 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Kanbanize。
- 可以让用户使用其 Azure AD 帐户自动登录到 Kanbanize（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>必备组件

若要配置 Azure AD 与 Kanbanize 的集成，需要以下项：

- Azure AD 订阅
- 已启用 Kanbanize 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Kanbanize
2. 配置和测试 Azure AD 单一登录

## <a name="adding-kanbanize-from-the-gallery"></a>从库中添加 Kanbanize
若要配置 Kanbanize 与 Azure AD 的集成，需要从库中将 Kanbanize 添加到托管 SaaS 应用列表。

**若要从库中添加 Kanbanize，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”  图标。 

    ![“Azure Active Directory”按钮][1]

2. 导航到“企业应用程序”。  然后转到“所有应用程序”  。

    ![“企业应用程序”边栏选项卡][2]
    
3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”  按钮。

    ![“新增应用程序”按钮][3]

4. 在搜索框中，键入“Kanbanize”，在结果面板中选择“Kanbanize”，然后单击“添加”按钮添加该应用程序    。

    ![结果列表中的 Kanbanize](./media/kanbanize-tutorial/tutorial_kanbanize_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，将基于名为“Britta Simon”的测试用户使用 Kanbanize 配置和测试 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Kanbanize 用户。 换句话说，需要在 Azure AD 用户与 Kanbanize 中的相关用户之间建立链接关系。

若要配置和测试 Kanbanize 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
2. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 Kanbanize 测试用户](#create-a-kanbanize-test-user)** - 在 Kanbanize 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
5. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 Kanbanize 应用程序中配置单一登录。

**若要配置 Kanbanize 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 **Kanbanize** 应用程序集成页上，单击“单一登录”。 

    ![配置单一登录链接][4]

2. 在“单一登录”  对话框中，选择“基于 SAML 的单一登录”  作为“模式”  以启用单一登录。
 
    ![“单一登录”对话框](./media/kanbanize-tutorial/tutorial_kanbanize_samlbase.png)

3. 在“Kanbanize 域和 URL”部分中，如果要在“IDP”  发起的模式下配置应用程序，请执行以下步骤  ：

    ![Kanbanize 域和 URL 单一登录信息](./media/kanbanize-tutorial/tutorial_kanbanize_url.png)

    a. 在“标识符”  文本框中，使用以下模式键入 URL：`https://<subdomain>.kanbanize.com/`

    b. 在“回复 URL”  文本框中，使用以下模式键入 URL：`https://<subdomain>.kanbanize.com/saml/acs`

    c. 选中“显示高级 URL 设置”。 

    d.  在“中继状态”  文本框中，键入 URL：`/ctrl_login/saml_login`

    e. 如果要在 **SP** 发起的模式下配置应用程序，请在“登录 URL”  文本框中使用以下模式键入一个 URL：`https://<subdomain>.kanbanize.com`
     
    > [!NOTE] 
    > 这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 请联系 [Kanbanize 客户端支持团队](mailto:support@ms.kanbanize.com)获取这些值。 

5. Kanbanize 应用程序需要特定格式的 SAML 断言，这要求向“SAML 令牌属性”配置添加自定义属性映射。 以下屏幕截图显示一个示例。 “用户标识符”的默认值是“user.userprincipalname”，但 Kanbanize 要求通过用户的电子邮件地址映射此项   。 为此，可以使用列表中的 user.mail 属性，或使用基于组织配置的相应属性值 
    
    ![配置单一登录](./media/kanbanize-tutorial/tutorial_Kanbanize_attribute.png)

6. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。  

    ![证书下载链接](./media/kanbanize-tutorial/tutorial_kanbanize_certificate.png) 

7. 单击“保存”按钮  。

    ![配置单一登录“保存”按钮](./media/kanbanize-tutorial/tutorial_general_400.png)
    
8. 在“Kanbanize 配置”  部分中，单击“配置 Kanbanize”  打开“配置登录”  窗口。 从“快速参考”  部分中复制“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”  。

    ![Kanbanize 配置](./media/kanbanize-tutorial/tutorial_kanbanize_configure.png)

9. 在另一个 Web 浏览器窗口中，以安全管理员身份登录到 Kanbanize。 

10. 转到页面右上角，单击“设置”  徽标。

    ![Kanbanize 设置](./media/kanbanize-tutorial/tutorial_kanbanize_set.png)

11. 在“管理面板”页上的菜单左侧，单击“集成”  ，然后启用“单一登录”  。 

    ![Kanbanize 集成](./media/kanbanize-tutorial/tutorial_kanbanize_admin.png)

12. 在“集成”部分下，单击“配置”  打开“单一登录集成”  页。

    ![Kanbanize 配置](./media/kanbanize-tutorial/tutorial_kanbanize_config.png)

13. 在“单一登录集成”  页上的“配置”  下，执行以下步骤：

    ![Kanbanize 集成](./media/kanbanize-tutorial/tutorial_kanbanize_save.png)

    a. 在“Idp 实体 ID”文本框中，粘贴从 Azure 门户复制的“SAML 实体 ID”值   。

    b. 在“Idp 登录终结点”文本框中，粘贴从 Azure 门户复制的“SAML 单一登录服务 URL”值   。

    c. 在“Idp 注销终结点”文本框中，粘贴从 Azure 门户复制的“注销 URL”值   。

    d. 在“电子邮件的属性名称”  文本框中，输入此值 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    e. 在“名字的属性名称”  文本框中，输入此值 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    f. 在“姓氏的属性名称”  文本框中，输入此值 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` 
    > [!Note]
    > 可以通过组合 Azure 门户的“用户属性”部分中相应属性的命名空间和名称值来获取这些值。

    g. 在记事本中，打开从 Azure 门户下载的 base-64 编码证书，复制其内容（不带开始和结束标记），然后将其粘贴到“Idp X.509 证书”框中 **** 。

    h. 选中“启用同时使用 SSO 和 Kanbanize 登录”  。
    
    i. 单击“保存设置”。 

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。 

    ![“Azure Active Directory”按钮](./media/kanbanize-tutorial/create_aaduser_01.png)

2. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”   。

    ![“用户和组”以及“所有用户”链接](./media/kanbanize-tutorial/create_aaduser_02.png)

3. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”    。

    ![“添加”按钮](./media/kanbanize-tutorial/create_aaduser_03.png)

4. 在“用户”  对话框中，执行以下步骤：

    ![“用户”对话框](./media/kanbanize-tutorial/create_aaduser_04.png)

    a. 在“姓名”  框中，键入“BrittaSimon”  。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。 

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值   。

    d. 单击**创建**。
 
### <a name="create-a-kanbanize-test-user"></a>创建 Kanbanize 测试用户

本部分的目的是在 Kanbanize 中创建名为 Britta Simon 的用户。 Kanbanize 支持在默认情况下启用的实时预配。 此部分不存在任何操作项。 尝试访问 Kanbanize 期间，如果该用户尚不存在，则将创建一个新用户。

>[!Note]
>如果需要手动创建用户，请联系 [Kanbanize 客户端支持团队](mailto:support@ms.kanbanize.com)。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Kanbanize 的权限，允许其使用 Azure 单一登录。

![分配用户角色][200] 

**若要将 Britta Simon 分配到 Kanbanize，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”  ，并单击“所有应用程序”  。

    ![分配用户][201] 

2. 在应用程序列表中，选择“Kanbanize”  。

    ![应用程序列表中的 Kanbanize 链接](./media/kanbanize-tutorial/tutorial_kanbanize_app.png)  

3. 在左侧菜单中，单击“用户和组”  。

    ![“用户和组”链接][202]

4. 单击“添加”按钮。  然后在“添加分配”对话框中选择“用户和组”。  

    ![“添加分配”窗格][203]

5. 在“用户和组”  对话框的“用户”列表中，选择“Britta Simon”。 

6. 在“用户和组”对话框中单击“选择”按钮。  

7. 在“添加分配”对话框中单击“分配”按钮。  
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 Kanbanize 磁贴时，应自动登录到 Kanbanize 应用程序。
有关访问面板的详细信息，请参阅[访问面板简介](../active-directory-saas-access-panel-introduction.md)。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kanbanize-tutorial/tutorial_general_01.png
[2]: ./media/kanbanize-tutorial/tutorial_general_02.png
[3]: ./media/kanbanize-tutorial/tutorial_general_03.png
[4]: ./media/kanbanize-tutorial/tutorial_general_04.png

[100]: ./media/kanbanize-tutorial/tutorial_general_100.png

[200]: ./media/kanbanize-tutorial/tutorial_general_200.png
[201]: ./media/kanbanize-tutorial/tutorial_general_201.png
[202]: ./media/kanbanize-tutorial/tutorial_general_202.png
[203]: ./media/kanbanize-tutorial/tutorial_general_203.png

