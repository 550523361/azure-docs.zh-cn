---
title: 教程：Azure Active Directory 与 Workrite 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Workrite 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 2a5c2956-a011-4d5c-877b-80679b6587b5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb64debca10cf7be6e2e328a1f401f125b67d940
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57447160"
---
# <a name="tutorial-azure-active-directory-integration-with-workrite"></a>教程：Azure Active Directory 与 Workrite 的集成

本教程介绍如何将 Workrite 与 Azure Active Directory (Azure AD) 集成。

将 Workrite 与 Azure AD 集成提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Workrite。
- 可让用户使用其 Azure AD 帐户自动登录到 Workrite 单一登录。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>必备组件

若要配置 Azure AD 与 Workrite 的集成，需要以下项：

- Azure AD 订阅
- 已启用 Workrite 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Workrite
1. 配置和测试 Azure AD 单一登录

## <a name="adding-workrite-from-the-gallery"></a>从库中添加 Workrite
要配置 Workrite 与 Azure AD 的集成，需将库中的 Workrite 添加到托管的 SaaS 应用列表。

**若要从库中添加 Workrite，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

1. 在搜索框中键入“Workrite”，在结果面板中选择“Workrite”，然后单击“添加”按钮添加该应用程序。

    ![结果列表中的 Workrite](./media/workrite-tutorial/tutorial_workrite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，基于名为“Britta Simon”的测试用户配置和测试 Workrite 的 Azure AD 单一登录。

为使单一登录能正常工作，Azure AD 需要知道 Workrite 中与 Azure AD 用户对应的用户。 换言之，需要建立 Azure AD 用户与 Workrite 中相关用户之间的链接关系。

可以通过将 Azure AD 中“用户名”的值指定为 Workrite 中“用户名”的值来建立此关联。

若要配置和测试 Workrite 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 Workrite 测试用户](#create-a-workrite-test-user)** - 在 Workrite 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 Workrite 应用程序中配置单一登录。

**若要配置 Workrite 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的“Workrite”应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/workrite-tutorial/tutorial_workrite_samlbase.png)

1. 在“Workrite 域和 URL”部分中，执行以下步骤：

    ![Workrite 域和 URL 单一登录信息](./media/workrite-tutorial/tutorial_workrite_url.png)

    在“登录 URL”文本框中，使用以下模式键入 URL： `https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`

    > [!NOTE] 
    > 此值不是真实值。 使用实际登录 URL 更新此值。 请联系 [Workrite 客户端支持团队](mailto:support@workrite.co.uk)获取此值。

1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![证书下载链接](./media/workrite-tutorial/tutorial_workrite_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/workrite-tutorial/tutorial_general_400.png)

1. 在“Workrite 配置”部分，单击“配置 Workrite”打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”。

    ![Workrite 配置](./media/workrite-tutorial/tutorial_workrite_configure.png) 

1. 若要在 Workrite 端配置单一登录，需要将下载的证书 (Base64)、注销 URL、SAML 实体 ID 和 SAML 单一登录服务 URL 发送给 [ 支持团队](mailto:support@workrite.co.uk)。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/workrite-tutorial/create_aaduser_01.png)

1. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/workrite-tutorial/create_aaduser_02.png)

1. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/workrite-tutorial/create_aaduser_03.png)

1. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/workrite-tutorial/create_aaduser_04.png)

    a. 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。
 
### <a name="create-a-workrite-test-user"></a>创建 Workrite 测试用户

本部分的目的是在 Workrite 中创建名为 Britta Simon 的用户。

**若要在 Workrite 中创建名为 Britta Simon 的用户，请执行以下步骤：**

1. 以管理员身份登录到 Workrite 公司站点。

1. 在导航窗格中，单击“管理员”。
   
    ![管理员控制][400]

1. 转到“快速链接”，并单击“创建用户”。
   
    ![“创建用户”部分][401]

1. 在“创建用户”对话框中，执行以下步骤：
   
    ![创建用户对话框][402]
    
    a. 在“电子邮件”文本框中，键入用户的电子邮件地址（例如 Brittasimon@contoso.com）。

    b. 在“名字”文本框中，键入用户的名字（如 Britta）。

    c. 在“姓氏”文本框中，键入用户的姓氏（如 Simon）。
    
    d. 对于“选择角色”选项，请选择“客户端管理员”。
    
    e. 单击“ **保存**”。   

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Workrite 的权限，允许她使用 Azure 单一登录。

![分配用户角色][200] 

**要将 Britta Simon 分配到 Workrite，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“Workrite”。

    ![应用程序列表中的 Workrite 链接](./media/workrite-tutorial/tutorial_workrite_app.png)  

1. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

本部分旨在使用“访问面板”测试 Azure AD SSO 配置。

在访问面板中单击“Workrite”磁贴，即可自动登录到 Workrite 应用程序。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/workrite-tutorial/tutorial_general_01.png
[2]: ./media/workrite-tutorial/tutorial_general_02.png
[3]: ./media/workrite-tutorial/tutorial_general_03.png
[4]: ./media/workrite-tutorial/tutorial_general_04.png

[100]: ./media/workrite-tutorial/tutorial_general_100.png

[200]: ./media/workrite-tutorial/tutorial_general_200.png
[201]: ./media/workrite-tutorial/tutorial_general_201.png
[202]: ./media/workrite-tutorial/tutorial_general_202.png
[203]: ./media/workrite-tutorial/tutorial_general_203.png
[400]: ./media/workrite-tutorial/tutorial_workrite_400.png
[401]: ./media/workrite-tutorial/tutorial_workrite_401.png
[402]: ./media/workrite-tutorial/tutorial_workrite_402.png

