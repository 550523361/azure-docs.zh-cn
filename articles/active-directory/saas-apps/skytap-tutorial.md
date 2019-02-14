---
title: 教程：Azure Active Directory 与 Skytap 集成 | Microsoft Docs
description: 了解如何配置 Azure Active Directory 和 Skytap 之间的单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6cb7ab2-da1a-4015-8e6f-c0c47bb6210f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec6d34ec82d5239af653e69532703a931f9cb12a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56182742"
---
# <a name="tutorial-azure-active-directory-integration-with-skytap"></a>教程：Azure Active Directory 与 Skytap 集成

本教程介绍了如何将 Skytap 与 Azure Active Directory (Azure AD) 集成。

将 Skytap 与 Azure AD 集成带来以下优势：

- 可以在 Azure AD 中控制谁有权访问 Skytap。
- 可以让用户使用其 Azure AD 帐户自动登录到 Skytap（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Skytap 集成，需要有以下各项：

- Azure AD 订阅
- 启用了 Skytap 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Skytap
1. 配置和测试 Azure AD 单一登录

## <a name="adding-skytap-from-the-gallery"></a>从库中添加 Skytap
若要配置将 Skytap 集成到 Azure AD 中，需要从库中将 Skytap 添加到托管 SaaS 应用列表。

**若要从库中添加 Skytap，请按照以下步骤操作：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

1. 在搜索框中，键入“Skytap”，选择结果面板中的“Skytap”，再单击“添加”按钮以添加应用程序。

    ![结果列表中的 Skytap](./media/skytap-tutorial/tutorial_skytap_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，将基于名为“Britta Simon”的测试用户使用 Skytap 配置和测试 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Skytap 用户。 换句话说，需要在 Azure AD 用户和 Skytap 中的相关用户之间建立链接关系。

若要配置和测试 Skytap 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 Skytap 测试用户](#create-a-skytap-test-user)** - 在 Skytap 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在此部分中，在 Azure 门户中启用 Azure AD 单一登录，并在 Skytap 应用程序中配置单一登录。

**若要配置 Skytap 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 Skytap 应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/skytap-tutorial/tutorial_skytap_samlbase.png)

1. 在“Skytap 域和 URL”部分中，若要在 IDP 发起模式下配置应用程序，请按照以下步骤操作：

    ![“Skytap 域和 URL”单一登录信息](./media/skytap-tutorial/tutorial_skytap_url.png)

    a. 在“标识符”文本框中，使用以下模式键入 URL：`http://pingone.com/<custom EntityID>`

    b. 在“回复 URL”文本框中，键入 URL：`https://sso.connect.pingidentity.com/sso/sp/ACS.saml2`

1. 如果要在 SP 发起的模式下配置应用程序，请选中“显示高级 URL 设置”，并执行以下步骤：

    ![“Skytap 域和 URL”单一登录信息](./media/skytap-tutorial/tutorial_skytap_url1.png)

    c. 在“登录 URL”文本框中，使用以下模式键入 URL： `https://sso.connect.pingidentity.com/sso/sp/initsso?saasid=<saasid>&idpid=<idpid>`
     
    d. 在“中继状态”文本框中，使用以下格式键入 URL：`https://pingone.com/1.0/<custom ID>`

    > [!NOTE] 
    > 这些不是实际值。 使用实际标识符、登录 URL 和中继状态更新这些值。 若要获取这些值，请联系 [Skytap 客户端支持团队](mailto:support@skytap.com)。 

1. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存元数据文件。

    ![证书下载链接](./media/skytap-tutorial/tutorial_skytap_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/skytap-tutorial/tutorial_general_400.png)
    
1. 若要在 Skytap 端配置单一登录，需要将下载的元数据 XML 发送给 [Skytap 支持团队](mailto:support@skytap.com)。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/skytap-tutorial/create_aaduser_01.png)

1. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/skytap-tutorial/create_aaduser_02.png)

1. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/skytap-tutorial/create_aaduser_03.png)

1. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/skytap-tutorial/create_aaduser_04.png)

    a. 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。
 
### <a name="create-a-skytap-test-user"></a>创建 Skytap 测试用户

在此部分中，在 Skytap 中创建用户“Britta Simon”。 请与  [Skytap 支持团队](mailto:support@skytap.com)协作，将用户添加到 Skytap 平台中。 使用单一登录前，必须先创建并激活用户。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在此部分中，通过向 Britta Simon 授予对 Skytap 的访问权限，让其能够使用 Azure 单一登录。

![分配用户角色][200] 

**若要将 Britta Simon 分配到 Skytap，请按照以下步骤操作：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“Skytap”。

    ![应用程序列表中的“Skytap”链接](./media/skytap-tutorial/tutorial_skytap_app.png)  

1. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击“访问权限”面板中的“Skytap”磁贴时，应该会自动登录 Skytap 应用程序。
有关访问面板的详细信息，请参阅[访问面板简介](../user-help/active-directory-saas-access-panel-introduction.md)。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skytap-tutorial/tutorial_general_01.png
[2]: ./media/skytap-tutorial/tutorial_general_02.png
[3]: ./media/skytap-tutorial/tutorial_general_03.png
[4]: ./media/skytap-tutorial/tutorial_general_04.png

[100]: ./media/skytap-tutorial/tutorial_general_100.png

[200]: ./media/skytap-tutorial/tutorial_general_200.png
[201]: ./media/skytap-tutorial/tutorial_general_201.png
[202]: ./media/skytap-tutorial/tutorial_general_202.png
[203]: ./media/skytap-tutorial/tutorial_general_203.png

