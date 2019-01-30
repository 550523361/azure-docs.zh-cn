---
title: 教程：Azure Active Directory 与 Chromeriver 的集成 | Microsoft 文档
description: 了解如何在 Azure Active Directory 和 Chromeriver 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 445c5600-e340-4724-a9cb-3cfaf5770b70
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeedes
ms.openlocfilehash: c097372ea3107e84b348fd6ab526524e515865c4
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817004"
---
# <a name="tutorial-azure-active-directory-integration-with-chromeriver"></a>教程：Azure Active Directory 与 Chromeriver 的集成

在本教程中，了解如何将 Chromeriver 与 Azure Active Directory (Azure AD) 集成。

将 Chromeriver 与 Azure AD 集成提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Chromeriver
- 可以让用户使用其 Azure AD 帐户自动登录到 Chromeriver（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Chromeriver 的集成，需要具有以下项：

- Azure AD 订阅
- 已启用 Chromeriver 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Chromeriver
1. 配置和测试 Azure AD 单一登录

## <a name="adding-chromeriver-from-the-gallery"></a>从库中添加 Chromeriver
若要配置 Chromeriver 与 Azure AD 的集成，需要从库中将 Chromeriver 添加到托管 SaaS 应用列表。

**若要从库中添加 Chromeriver，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“Chromeriver”。

    ![创建 Azure AD 测试用户](./media/chromeriver-tutorial/tutorial_chromeriver_search.png)

1. 在结果窗格中，选择“Chromeriver”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/chromeriver-tutorial/tutorial_chromeriver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 Chromeriver 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Chromeriver 用户。 换句话说，需要建立 Azure AD 用户与 Chromeriver 中相关用户之间的关联关系。

可通过将 Azure AD 中“用户名”的值指定为 Chromeriver 中“用户名”的值来建立此关联关系。

若要配置和测试 Chromeriver 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 Chromeriver 测试用户](#creating-a-chromeriver-test-user)** - 在 Chromeriver 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 Chromeriver 应用程序中配置单一登录。

**若要配置 Chromeriver 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 Chromeriver 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/chromeriver-tutorial/tutorial_chromeriver_samlbase.png)

1. 在“Chromeriver 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/chromeriver-tutorial/tutorial_chromeriver_url.png)

    a. 在“标识符”文本框中，使用以下模式键入 URL：`https://<subdomain>.chromeriver.com`

    b. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://<subdomain>.chromeriver.com/login/sso/saml/consume?customerId=<uniqueid>`

    > [!NOTE] 
    > 这些不是实际值。 请使用实际标识符和回复 URL 更新这些值。 若要获取这些值，请联系 [Chromeriver 支持团队](https://www.chromeriver.com/services/support)。
 


1. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存元数据文件。

    ![配置单一登录](./media/chromeriver-tutorial/tutorial_chromeriver_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录](./media/chromeriver-tutorial/tutorial_general_400.png)

1. 若要在 Chromeriver 端配置单一登录，需要将下载的元数据 XML 发送给 [Chromeriver 支持团队](https://www.chromeriver.com/services/support)。 在为订阅启用了 SSO 后，将收到通知。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/chromeriver-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/chromeriver-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/chromeriver-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/chromeriver-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-chromeriver-test-user"></a>创建 Chromeriver 测试用户

若要使 Azure AD 用户能够登录到 Chromeriver，必须将其预配到 Chromeriver 中。  

对于 Chromeriver，需要由 [Chromeriver 支持团队](https://www.chromeriver.com/services/support)创建用户帐户。

>[!NOTE]
>可以使用 Chromeriver 提供的任何其他 Chromeriver 用户帐户创建工具或 API 来预配 Azure Active Directory 用户帐户。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Chromeriver 的权限，允许其使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 Chromeriver，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“Chromeriver”。

    ![配置单一登录](./media/chromeriver-tutorial/tutorial_chromeriver_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

本部分旨在使用“访问面板”测试 Azure AD 单一登录配置。

在访问面板中单击 Chromeriver 磁贴时，应当会自动登录到 Chromeriver 应用程序。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/chromeriver-tutorial/tutorial_general_01.png
[2]: ./media/chromeriver-tutorial/tutorial_general_02.png
[3]: ./media/chromeriver-tutorial/tutorial_general_03.png
[4]: ./media/chromeriver-tutorial/tutorial_general_04.png

[100]: ./media/chromeriver-tutorial/tutorial_general_100.png

[200]: ./media/chromeriver-tutorial/tutorial_general_200.png
[201]: ./media/chromeriver-tutorial/tutorial_general_201.png
[202]: ./media/chromeriver-tutorial/tutorial_general_202.png
[203]: ./media/chromeriver-tutorial/tutorial_general_203.png

