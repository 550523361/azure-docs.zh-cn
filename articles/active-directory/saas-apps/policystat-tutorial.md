---
title: 教程：Azure Active Directory 与 PolicyStat 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 PolicyStat 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f8dad5e3b5bfd908e8a3b3c1f1bcc78dd689757a
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823872"
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>教程：Azure Active Directory 与 PolicyStat 的集成

本教程介绍如何将 PolicyStat 与 Azure Active Directory (Azure AD) 集成。

将 PolicyStat 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 PolicyStat
- 可让用户通过其 Azure AD 帐户自动登录到 PolicyStat（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 PolicyStat 的集成，需要以下项：

- Azure AD 订阅
- 已启用 PolicyStat 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 PolicyStat
1. 配置和测试 Azure AD 单一登录

## <a name="adding-policystat-from-the-gallery"></a>从库中添加 PolicyStat
若要配置 PolicyStat 与 Azure AD 的集成，需要从库中将 PolicyStat 添加到托管 SaaS 应用列表。

若要从库中添加 PolicyStat，请执行以下步骤：

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“PolicyStat”。

    ![创建 Azure AD 测试用户](./media/policystat-tutorial/tutorial_policystat_search.png)

1. 在结果面板中，选择“PolicyStat”，并单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，基于一个名为“Britta Simon”的测试用户使用 PolicyStat 配置和测试 Azure AD 单一登录。

为使单一登录能正常工作，Azure AD 需要知道 PolicyStat 中与 Azure AD 用户对应的用户。 换句话说，需要建立 Azure AD 用户与 PolicyStat 中相关用户之间的链接关系。

可通过将 Azure AD 中“用户名”的值指定为 PolicyStat 中“用户名”的值来建立此链接关系。

若要配置和测试 PolicyStat 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. [创建 PolicyStat 测试用户](#creating-a-policystat-test-user) - 在 PolicyStat 中创建 Britta Simon 的对应用户，并将其链接到用户的 Azure AD 身份。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

本部分将介绍如何在 Azure 门户中启用 Azure AD 单一登录并在 PolicyStat 应用程序中配置单一登录。

若要配置 PolicyStat 的 Azure AD 单一登录，请执行以下步骤：

1. 在 Azure 门户中的 PolicyStat 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/policystat-tutorial/tutorial_policystat_samlbase.png)

1. 在“PolicyStat 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/policystat-tutorial/tutorial_policystat_url.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL： `https://<companyname>.policystat.com`

    b. 在“标识符”文本框中，使用以下模式键入 URL：`https://<companyname>.policystat.com/saml2/metadata/`

    > [!NOTE] 
    > 这些不是实际值。 必须使用实际登录 URL 和标识符更新这些值。 请联系 [PolicyStat 客户端支持团队](http://www.policystat.com/support/)获取这些值。 
 
1. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存元数据文件。

    ![配置单一登录](./media/policystat-tutorial/tutorial_policystat_certificate.png) 

1. 此部分的目的是概述如何使用户使用基于 SAML 协议的联合身份验证通过他们在 Azure AD 中的帐户向 PolicyStat 进行身份验证。

    PolicyStat 应用程序需要特定格式的 SAML 断言，这要求将自定义属性映射添加到 SAML 令牌属性配置。  

     以下屏幕截图显示了相关示例。

     ![属性](./media/policystat-tutorial/tutorial_policystat_attribute.png "属性")

1. 若要添加所需的属性映射，请执行以下步骤：

    | 属性名称    |   属性值 |
    |------------------- | -------------------- |
    | uid | ExtractMailPrefix([mail]) |
    
    a. 单击“添加属性”，打开“添加属性”对话框。

    ![配置单一登录](./media/policystat-tutorial/tutorial_policystat_04.png)

    ![配置单一登录](./media/policystat-tutorial/tutorial_policystat_addatribute.png)
    
    b. 在“属性名称”文本框中，键入“uid”。

    c. 在“属性值”文本框中，选择“ExtractMailPrefix()”。    
   
    d. 从“邮件”列表中，选择“User.mail”。
    
    e. 单击“确定”

1. 单击“保存”按钮。

    ![配置单一登录](./media/policystat-tutorial/tutorial_general_400.png)

1. 在另一 Web 浏览器窗口中，以管理员身份登录到 PolicyStat 公司站点。

1. 单击“管理员”选项卡，并单击左侧导航窗格中的“单一登录配置”。
   
    ![管理员菜单](./media/policystat-tutorial/ic808633.png "管理员菜单")

1. 在“设置”部分，选择“启用单一登录集成”。
   
    ![单一登录配置](./media/policystat-tutorial/ic808634.png "单一登录配置")

1. 单击“配置属性”，并在“配置属性”部分执行以下步骤：
   
    ![单一登录配置](./media/policystat-tutorial/ic808635.png "单一登录配置")
   
    a. 在“用户名属性”文本框中，键入“uid”。

    b. 在“名字属性”文本框中，键入用户的名字，如 Britta。

    c. 在“姓氏属性”文本框中，键入用户的姓氏，如 Simon。

    d. 在“电子邮件属性”文本框中，键入用户的电子邮件地址，如 BrittaSimon@contoso.com。

    e. 单击“保存更改”。

1. 单击“IDP 元数据”，并在“IDP 元数据”部分执行以下步骤：
   
    ![单一登录配置](./media/policystat-tutorial/ic808636.png "单一登录配置")
   
    a. 打开下载的元数据文件，复制其内容，然后将其粘贴到“标识提供者元数据”文本框中。

    b. 单击“保存更改”。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/policystat-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/policystat-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/policystat-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/policystat-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-policystat-test-user"></a>创建 PolicyStat 测试用户

要让 Azure AD 用户登录 PolicyStat，必须将其预配到 PolicyStat 中。  

PolicyStat 支持实时用户预配。 这意味着你不需手动将用户添加到 PolicyStat。 系统会在用户首次通过 SSO 登录时自动将其添加到其中。

>[!NOTE]
>可使用任何其他 PolicyStat 用户帐户创建工具或 PolicyStat 提供的 API 来预配 Azure AD 用户帐户。
> 

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 PolicyStat 的权限，允许她使用 Azure 单一登录。

![分配用户][200] 

若要将 Britta Simon 分配到 PolicyStat，请执行以下步骤：

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“PolicyStat”.

    ![配置单一登录](./media/policystat-tutorial/tutorial_policystat_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击“PolicyStat”磁贴就会自动登录到 PolicyStat 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/policystat-tutorial/tutorial_general_01.png
[2]: ./media/policystat-tutorial/tutorial_general_02.png
[3]: ./media/policystat-tutorial/tutorial_general_03.png
[4]: ./media/policystat-tutorial/tutorial_general_04.png

[100]: ./media/policystat-tutorial/tutorial_general_100.png

[200]: ./media/policystat-tutorial/tutorial_general_200.png
[201]: ./media/policystat-tutorial/tutorial_general_201.png
[202]: ./media/policystat-tutorial/tutorial_general_202.png
[203]: ./media/policystat-tutorial/tutorial_general_203.png

