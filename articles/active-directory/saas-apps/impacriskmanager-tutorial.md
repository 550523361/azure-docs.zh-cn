---
title: 教程：Azure Active Directory 与 IMPAC Risk Manager 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 IMPAC Risk Manager 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 4d77390e-898c-4258-a562-a1181dfe2880
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f987c6803f6ca538f4ae7470caaff597c9596c2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2019
ms.locfileid: "57900737"
---
# <a name="tutorial-azure-active-directory-integration-with-impac-risk-manager"></a>教程：Azure Active Directory 与 IMPAC Risk Manager 集成

本教程介绍了如何将 IMPAC Risk Manager 与 Azure Active Directory (Azure AD) 进行集成。

IMPAC Risk Manager 与 Azure AD 的集成可提供以下优势：

- 可在 Azure AD 中控制哪些用户有权访问 IMPAC Risk Manager。
- 可让用户使用 Azure AD 帐户自动登录到 IMPAC Risk Manager（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>必备组件

若要配置 Azure AD 与 IMPAC Risk Manager 的集成，需要以下各项：

- Azure AD 订阅
- 已启用 IMPAC Risk Manager 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 IMPAC Risk Manager
1. 配置和测试 Azure AD 单一登录

## <a name="adding-impac-risk-manager-from-the-gallery"></a>从库中添加 IMPAC Risk Manager
若要配置 IMPAC Risk Manager 与 Azure AD 的集成，需要将库中的 IMPAC Risk Manager 添加到托管 SaaS 应用列表。

若要从库中添加 IMPAC Risk Manager，请执行以下步骤：

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

1. 在搜索框中，键入“IMPAC Risk Manager”，在结果面板中选择“IMPAC Risk Manager”，然后单击“添加”按钮添加该应用程序。

    ![结果列表中的 IMPAC Risk Manager](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，基于名为“Britta Simon”的测试用户配置和测试 IMPAC Risk Manager 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 IMPAC Risk Manager 用户。 也就是说，需要在 Azure AD 用户与 IMPAC Risk Manager 中的相关用户之间建立链接关系。

通过将 Azure AD 中“用户名”的值指定为 IMPAC Risk Manager 中“用户名”的值来建立此链接关系。

若要配置和测试 IMPAC Risk Manager 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. [创建 IMPAC Risk Manager 测试用户](#create-a-impac-risk-manager-test-user) - 在 IMPAC Risk Manager 中创建 Britta Simon 的对应用户，并将其关联到用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 IMPAC Risk Manager 应用程序中配置单一登录。

若要配置 IMPAC Risk Manager 的 Azure AD 单一登录，请执行以下步骤：

1. 在 Azure 门户的“IMPAC Risk Manager”应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_samlbase.png)

1. 若要在启用 IDP 的模式下配置应用程序，请在“IMPAC Risk Manager 域和 URL”部分执行以下步骤：

    ![IMPAC Risk Manager 域和 URL 单一登录信息](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_url_new.png)

    a. 在“标识符”文本框中，键入 IMPAC 提供的值

    b. 在“回复 URL”文本框中，使用以下模式键入 URL：

    | 环境 | URL 模式 |
    | ---------------|--------------- |    
    | 生产 |`https://www.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | 过渡和培训  |`https://staging.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | 开发  |`https://dev.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | QA |`https://QA.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|
    | 测试 |`https://test.riskmanager.co.nz/DotNet/SSOv2/AssertionConsumerService.aspx?client=<ClientSuffix>`|

1. 如果要在 SP 发起的模式下配置应用程序，请选中“显示高级 URL 设置”，并执行以下步骤：

    ![IMPAC Risk Manager 域和 URL 单一登录信息](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_url1_new.png)

    在“登录 URL”文本框中，使用以下模式键入 URL：
    
    | 环境 | URL 模式 |
    | ---------------|--------------- |    
    | 生产 |`https://www.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | 过渡和培训  |`https://staging.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | 开发  |`https://dev.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | QA |`https://QA.riskmanager.co.nz/SSOv2/<ClientSuffix>`|
    | 测试 |`https://test.riskmanager.co.nz/SSOv2/<ClientSuffix>`|

    > [!NOTE] 
    > 这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 请联系 [IMPAC Risk Manager 客户端支持团队](mailto:rmsupport@Impac.co.nz)获取这些值。

1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![证书下载链接](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/impacriskmanager-tutorial/tutorial_general_400.png)
    
1. 在“IMPAC Risk Manager 配置”部分中，单击“配置 IMPAC Risk Manager”来打开“配置登录”窗口。 复制“快速参考”部分中的“SAML 单一登录服务 URL”、“SAML 实体 ID”和“注销 URL”

    ![配置单一登录](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_configure.png)

1. 若要在“IMPAC Risk Manager”端配置单一登录，需要将下载的“证书(Base64)”、“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”发送给 [IMPAC Risk Manager 支持团队”](mailto:rmsupport@Impac.co.nz)。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/impacriskmanager-tutorial/create_aaduser_01.png)

1. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/impacriskmanager-tutorial/create_aaduser_02.png)

1. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/impacriskmanager-tutorial/create_aaduser_03.png)

1. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/impacriskmanager-tutorial/create_aaduser_04.png)

    a. 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。
 
### <a name="create-a-impac-risk-manager-test-user"></a>创建 IMPAC Risk Manager 测试用户

在本部分中，将在 IMPAC Risk Manager 中创建名为 Britta Simon 的用户。 请与  [IMPAC Risk Manager 支持团队](mailto:rmsupport@Impac.co.nz) 协作，将用户添加到 IMPAC Risk Manager 平台中。 使用单一登录前，必须先创建并激活用户。 

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 IMPAC Risk Manager 的权限，允许她使用 Azure 单一登录。

![分配用户角色][200] 

若要将 Britta Simon 分配到 IMPAC Risk Manager，请执行以下步骤：

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“IMPAC Risk Manager”。

    ![应用程序列表中的 IMPAC Risk Manager 链接](./media/impacriskmanager-tutorial/tutorial_impacriskmanager_app.png)  

1. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击 IMPAC Risk Manager 磁贴时，会自动登录到 IMPAC Risk Manager 应用程序。
有关访问面板的详细信息，请参阅[访问面板简介](../user-help/active-directory-saas-access-panel-introduction.md)。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/impacriskmanager-tutorial/tutorial_general_01.png
[2]: ./media/impacriskmanager-tutorial/tutorial_general_02.png
[3]: ./media/impacriskmanager-tutorial/tutorial_general_03.png
[4]: ./media/impacriskmanager-tutorial/tutorial_general_04.png

[100]: ./media/impacriskmanager-tutorial/tutorial_general_100.png

[200]: ./media/impacriskmanager-tutorial/tutorial_general_200.png
[201]: ./media/impacriskmanager-tutorial/tutorial_general_201.png
[202]: ./media/impacriskmanager-tutorial/tutorial_general_202.png
[203]: ./media/impacriskmanager-tutorial/tutorial_general_203.png

