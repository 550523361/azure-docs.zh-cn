---
title: 教程：Azure Active Directory 与 SD Elements 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 SD Elements 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c09947e6d34c5314e8ed4bc2744f07b199278a6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188097"
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>教程：Azure Active Directory 与 SD Elements 集成

本教程介绍如何将 SD Elements 与 Azure Active Directory (Azure AD) 集成。

将 SD Elements 与 Azure AD 集成提供以下优势：

- 可在 Azure AD 中控制谁有权访问 SD Elements
- 可以让用户使用其 Azure AD 帐户自动登录到 SD Elements（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 SD Elements 的集成，需备齐以下项目：

- Azure AD 订阅
- 已启用 SD Elements 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 SD Elements
1. 配置和测试 Azure AD 单一登录

## <a name="adding-sd-elements-from-the-gallery"></a>从库中添加 SD Elements
要配置 SD Elements 与 Azure AD 的集成，需要从库中将 SD Elements 添加到托管 SaaS 应用列表。

**若要从库中添加 SD Elements，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“SD Elements”。

    ![创建 Azure AD 测试用户](./media/sd-elements-tutorial/tutorial_sdelements_search.png)

1. 在结果面板中，选择“SD Elements”，并单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，基于一个名为“Britta Simon”的测试用户使用 SD Elements 配置和测试 Azure AD 单一登录。

为使单一登录能正常工作，Azure AD 需要知道 SD Elements 中与 Azure AD 用户对应的用户。 换句话说，需要在 Azure AD 用户与 SD Elements 中相关用户之间建立链接关系。

可通过将 Azure AD 中“用户名”的值指定为 SD Elements 中“用户名”的值来建立此链接关系。

若要配置和测试 SD Elements 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. [创建 SD Elements 测试用户](#creating-a-sd-elements-test-user) - 在 SD Elements 中创建 Britta Simon 的对应用户，并将其链接到用户的 Azure AD 身份。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

本部分将在 Azure 门户中启用 Azure AD 单一登录并在 SD Elements 应用程序中配置单一登录。

**若要配置 SD Elements 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 SD Elements 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/sd-elements-tutorial/tutorial_sdelements_samlbase.png)

1. 在“SD Elements 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/sd-elements-tutorial/tutorial_sdelements_url.png)

    a. 在“标识符”文本框中，使用以下模式键入 URL：`https://<tenantname>.sdelements.com/sso/saml2/metadata`

    b. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://<tenantname>.sdelements.com/sso/saml2/acs/`

    > [!NOTE] 
    > 这些不是实际值。 请使用实际标识符和回复 URL 更新这些值。 请联系 [SD Elements 支持团队](mailto:support@sdelements.com)获取这些值。

1. SD Elements 应用程序需要采用特定格式的 SAML 断言。 请为此应用程序配置以下声明。 可从应用程序的“用户属性”选项卡管理这些属性的值。 以下屏幕截图显示一个示例。

    ![配置单一登录](./media/sd-elements-tutorial/tutorial_sdelements_attribute.png)

1. 在“单一登录”对话框的“用户属性”部分，按图中所示配置 SAML 令牌属性，然后执行以下步骤： 

    | 属性名称 | 属性值 |
    | --- | --- |
    | 电子邮件 |user.mail |
    | 名 |user.givenname |
    | 姓 |user.surname |

    a. 单击“添加属性”，打开“添加属性”对话框。

    ![配置单一登录](./media/sd-elements-tutorial/tutorial_officespace_04.png)

    ![配置单一登录](./media/sd-elements-tutorial/tutorial_officespace_05.png)

    b. 在“名称”文本框中，键入为该行显示的属性名称。

    c. 在“值”列表中，选择为该行显示的属性值。

    d. 单击“确定” 。
 
1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![配置单一登录](./media/sd-elements-tutorial/tutorial_sdelements_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录](./media/sd-elements-tutorial/tutorial_general_400.png)

1. 在“SD Elements 配置”部分，单击“配置 SD Elements”，打开“配置登录”窗口。 从“快速参考”部分中复制“SAML 实体 ID 和 SAML 单一登录服务 URL”。

    ![配置单一登录](./media/sd-elements-tutorial/tutorial_sdelements_configure.png)

1. 若要启用单一登录，请联系 [SD Elements 支持团队](mailto:support@sdelements.com)并向其提供下载的证书文件。 

1. 在另一个浏览器窗口中，以管理员身份登录 SD Elements 租户。

1. 在顶部菜单中，单击“系统”，然后单击“单一登录”。 
   
    ![配置单一登录](./media/sd-elements-tutorial/tutorial_sd-elements_09.png) 

1. 在“单一登录设置”对话框上，执行以下步骤：
   
    ![配置单一登录](./media/sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    a. 对于“SSO 类型”，选择“SAML”。
   
    b. 在“标识提供者实体 ID”文本框中，粘贴从 Azure 门户复制的“SAML 实体 ID”值。 
   
    c. 在“标识提供者单一登录服务”文本框中，粘贴从 Azure 门户复制的“ SAML 单一登录服务 URL”值。 
   
    d. 单击“ **保存**”。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/sd-elements-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/sd-elements-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/sd-elements-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/sd-elements-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-sd-elements-test-user"></a>创建 SD Elements 测试用户

本部分的目的是在 SD Elements 中创建名为“Britta Simon”的用户。 对于 SD Elements，创建 SD Elements 用户是一项手动任务。

**若要在 SD Elements 中创建 Britta Simon，请执行以下步骤：**

1. 在 Web 浏览器窗口中，以管理员身份登录 SD Elements 公司站点。

1. 在顶部菜单中，单击“用户管理”，然后单击“用户”。
   
    ![创建 SD Elements 测试用户](./media/sd-elements-tutorial/tutorial_sd-elements_11.png) 

1. 单击“添加新用户”。
   
    ![创建 SD Elements 测试用户](./media/sd-elements-tutorial/tutorial_sd-elements_12.png)
 
1. 在“添加新用户”对话框中，执行以下步骤：
   
    ![创建 SD Elements 测试用户](./media/sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    a. 在“电子邮件”文本框中，输入用户的电子邮件地址（例如 brittasimon@contoso.com）。
   
    b. 在“名字”文本框中，输入用户的名字（如“Britta”）。
   
    c. 在“姓氏”文本框中，输入用户的姓氏（如“Simon”）。
   
    d. 选择“用户”作为“角色”。 
   
    e. 单击“创建用户”。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 SD Elements 的权限，以支持其使用 Azure 单一登录。

![分配用户][200] 

**要将 Britta Simon 分配到 SD Elements，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“SD Elements”。

    ![配置单一登录](./media/sd-elements-tutorial/tutorial_sdelements_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

本部分旨在使用“访问面板”测试 Azure AD 单一登录配置。
  
单击访问面板中的“SD Elements”磁贴时，用户应自动登录到 SD Elements 应用程序。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/sd-elements-tutorial/tutorial_general_203.png

