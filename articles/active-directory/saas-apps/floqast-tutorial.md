---
title: 教程：Azure Active Directory 与 FloQast 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 FloQast 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 013cb57d-567c-44d0-a119-e6ba6e607153
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeedes
ms.openlocfilehash: f46714d2d4860abd1857e6ae16f98848678336aa
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55184762"
---
# <a name="tutorial-azure-active-directory-integration-with-floqast"></a>教程：Azure Active Directory 与 FloQast 集成

在本教程中，了解如何将 FloQast 与 Azure Active Directory (Azure AD) 集成。

将 FloQast 与 Azure AD 集成提供以下优势：

- 可以在 Azure AD 中控制谁有权访问 FloQast。
- 可以让用户使用其 Azure AD 帐户自动登录到 FloQast（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 FloQast 的集成，需要以下项：

- Azure AD 订阅
- 启用了 FloQast 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 FloQast
1. 配置和测试 Azure AD 单一登录

## <a name="adding-floqast-from-the-gallery"></a>从库中添加 FloQast
若要配置 FloQast 与 Azure AD 的集成，需要从库中将 FloQast 添加到托管 SaaS 应用列表。

**若要从库中添加 FloQast，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

1. 在搜索框中，键入“FloQast”，在结果面板中选择“FloQast”，然后单击“添加”按钮添加该应用程序。

    ![结果列表中的 FloQast](./media/floqast-tutorial/tutorial_floqast_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，基于一个名为“Britta Simon”的测试用户使用 FloQast 配置和测试 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 FloQast 用户。 换句话说，需要建立 Azure AD 用户与 FloQast 中相关用户之间的链接关系。

若要配置和测试 FloQast 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. [创建 FloQast 测试用户](#create-a-floqast-test-user) - 在 FloQast 中创建 Britta Simon 的对应用户，并将其链接到用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 FloQast 应用程序中配置单一登录。

**若要配置 FloQast 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中，在 FloQast 应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/floqast-tutorial/tutorial_floqast_samlbase.png)

1. 在“FloQast 域和 URL”部分，如果要在 IDP 发起的模式下配置应用程序，请执行以下步骤：

    ![FloQast 域和 URL 单一登录信息](./media/floqast-tutorial/tutorial_floqast_url.png)

     在“标识符”文本框中，键入一个 URL：`https://go.floqast.com/`

1. 如果要在 SP 发起的模式下配置应用程序，请选中“显示高级 URL 设置”，并执行以下步骤：

    ![FloQast 域和 URL 单一登录信息](./media/floqast-tutorial/tutorial_floqast_url1.png)

     在“登录 URL”文本框中，键入 URL `https://go.floqast.com/login/sso`
     
1. FloQast 应用程序需要特定格式的 SAML 断言。 请为此应用程序配置以下声明。 可以在应用程序集成页的“用户属性”部分管理这些属性的值。 以下屏幕截图显示一个示例。
    
    ![配置单一登录属性](./media/floqast-tutorial/tutorial_floqast_attribute.png)
    
1. 在“单一登录”对话框的“用户属性”部分，按图中所示配置 SAML 令牌属性，然后执行以下步骤：
    
    | 属性名称 | 属性值 |
    | ------------------- | -------------------- |    
    | FirstName           | user.givenname |
    | LastName        | user.surname |
    | 电子邮件       | user.mail    |

    a. 单击“添加属性”，打开“添加属性”对话框。

    ![配置单一登录 Add](./media/floqast-tutorial/tutorial_attribute_04.png)

    ![配置单一登录 Addattb](./media/floqast-tutorial/tutorial_attribute_05.png)

    b. 在“名称”文本框中，键入为该行显示的属性名称。

    c. 在“值”列表中，选择为该行显示的属性值。

    d. 将“命名空间”留空。
    
    e. 单击“确定” 。

1. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存元数据文件并执行以下步骤：

    ![证书下载链接](./media/floqast-tutorial/tutorial_floqast_certificate.png)

    a. 选中“显示高级证书签名设置”。

    ![证书断言](./media/floqast-tutorial/tutorial_floqast_certificateassertion.png)

    b. 选择“签名选项”作为“签名 SAML 响应和断言”。

1. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/floqast-tutorial/tutorial_general_400.png)
    
1. 若要在 FloQast 端配置单一登录，需要将下载的元数据 XML 发送给 [FloQast 支持团队](mailto:support@floqast.com)。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/floqast-tutorial/create_aaduser_01.png)

1. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/floqast-tutorial/create_aaduser_02.png)

1. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/floqast-tutorial/create_aaduser_03.png)

1. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/floqast-tutorial/create_aaduser_04.png)

    a. 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。
 
### <a name="create-a-floqast-test-user"></a>创建一个 FloQast 测试用户

在本部分中，将在 FloQast 中创建一个名为“Britta Simon”的用户。 与  [FloQast 支持团队](mailto:support@floqast.com) 协作，将用户添加到 FloQast 平台中。 使用单一登录前，必须先创建并激活用户。 

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过向 Britta Simon 授予 FloQast 的访问权限支持她使用 Azure 单一登录。

![分配用户角色][200] 

**若要将 Britta Simon 分配到 FloQast，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“FloQast”。

    ![应用程序列表中的 FloQast 链接](./media/floqast-tutorial/tutorial_floqast_app.png)  

1. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

当在访问面板中单击 FloQast 磁贴时，应当会自动登录到 FloQast 应用程序。
有关访问面板的详细信息，请参阅[访问面板简介](../user-help/active-directory-saas-access-panel-introduction.md)。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/floqast-tutorial/tutorial_general_01.png
[2]: ./media/floqast-tutorial/tutorial_general_02.png
[3]: ./media/floqast-tutorial/tutorial_general_03.png
[4]: ./media/floqast-tutorial/tutorial_general_04.png

[100]: ./media/floqast-tutorial/tutorial_general_100.png

[200]: ./media/floqast-tutorial/tutorial_general_200.png
[201]: ./media/floqast-tutorial/tutorial_general_201.png
[202]: ./media/floqast-tutorial/tutorial_general_202.png
[203]: ./media/floqast-tutorial/tutorial_general_203.png

