---
title: 教程：Azure Active Directory 与 Springer Link 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Springer Link 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 58cdf029-bdc0-43c4-a469-b921c2a669bd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: jeedes
ms.openlocfilehash: d4d8d61f5e8834b679eeb68cb416bb6358a5f53a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180137"
---
# <a name="tutorial-azure-active-directory-integration-with-springer-link"></a>教程：Azure Active Directory 与 Springer Link 集成

在本教程中，了解如何将 Springer Link 与 Azure Active Directory (Azure AD) 集成。

将 Springer Link 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Springer Link。
- 可以让用户使用其 Azure AD 帐户自动登录到 Springer Link（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Springer Link 的集成，需要以下项：

- Azure AD 订阅
- 已启用 Springer Link 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Springer Link
1. 配置和测试 Azure AD 单一登录

## <a name="adding-springer-link-from-the-gallery"></a>从库中添加 Springer Link
要配置 Springer Link 与 Azure AD 的集成，需要从库中将 Springer Link 添加到托管 SaaS 应用列表。

**若要从库中添加 Springer Link，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

1. 在搜索框中，键入“Springer Link”，从结果面板中选择“Springer Link”，然后单击“添加”按钮添加应用程序。

    ![结果列表中的 Springer Link](./media/springerlink-tutorial/tutorial_springerlink_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，基于一个名为“Britta Simon”的测试用户配置和测试 Springer Link 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Springer Link 用户。 换句话说，需要建立 Azure AD 用户与 Springer Link 中相关用户之间的关联关系。

将 Azure AD 中“用户名”的值指定为 Springer Link 中“用户名”的值，从而建立此关联关系。

若要配置和测试 Springer Link 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将介绍如何在 Azure 门户中启用 Azure AD 单一登录并在 Springer Link 应用程序中配置单一登录。

**若要配置 Springer Link 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中，在“Springer Link”应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/springerlink-tutorial/tutorial_springerlink_samlbase.png)

1. 在“Springer Link 域和 URL”部分中，如果要在“IDP”发起的模式下配置应用程序，请执行以下步骤：

    ![Springer Link 域和 URL 单一登录信息](./media/springerlink-tutorial/tutorial_springerlink_url1.png)

    a. 在“标识符”文本框中，键入 URL：`https://fsso.springer.com`

    b. 在“回复 URL”文本框中，键入 URL：`https://fsso-qa1.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`    

1. 选中“显示高级 URL 设置”。 如果要在“SP”发起的模式下配置应用程序：

    ![Springer Link 域和 URL 单一登录信息](./media/springerlink-tutorial/tutorial_springerlink_url.png)

    在“登录 URL”文本框中，键入 URL：`https://fsso.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`

1. 在“SAML 签名证书”部分上，单击”复制”按钮来复制**应用联合元数据 URL**，并将其粘贴到记事本。 

    ![证书下载链接](./media/springerlink-tutorial/tutorial_springerlink_certificate.png)    

1. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/springerlink-tutorial/tutorial_general_400.png)

1. 若要在 **Springer Link** 端配置单一登录，需要将**应用联合元数据 URL**发送给 [Springer Link 支持团队](mailto:identity@springernature.com)。

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/springerlink-tutorial/create_aaduser_01.png)

1. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/springerlink-tutorial/create_aaduser_02.png)

1. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/springerlink-tutorial/create_aaduser_03.png)

1. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/springerlink-tutorial/create_aaduser_04.png)

    a. 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。
 
### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Springer Link 的权限，允许她使用 Azure 单一登录。

![分配用户角色][200] 

**若要将 Britta Simon 分配到 Springer Link，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“Springer Link”。

    ![应用程序列表中的 Springer Link 链接](./media/springerlink-tutorial/tutorial_springerlink_app.png)  

1. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的“Springer Link”磁贴时，会自动登录到 Springer Link 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/springerlink-tutorial/tutorial_general_01.png
[2]: ./media/springerlink-tutorial/tutorial_general_02.png
[3]: ./media/springerlink-tutorial/tutorial_general_03.png
[4]: ./media/springerlink-tutorial/tutorial_general_04.png

[100]: ./media/springerlink-tutorial/tutorial_general_100.png

[200]: ./media/springerlink-tutorial/tutorial_general_200.png
[201]: ./media/springerlink-tutorial/tutorial_general_201.png
[202]: ./media/springerlink-tutorial/tutorial_general_202.png
[203]: ./media/springerlink-tutorial/tutorial_general_203.png

