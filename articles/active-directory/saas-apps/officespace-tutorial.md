---
title: 教程：Azure Active Directory 与 OfficeSpace Software 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 OfficeSpace Software 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d1c48c10d2c58e5cb2ffd7df296390bfaf765bd
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56206389"
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>教程：Azure Active Directory 与 OfficeSpace Software 集成

在本教程中，了解如何将 OfficeSpace Software 与 Azure Active Directory (Azure AD) 集成。

将 OfficeSpace Software 与 Azure AD 集成具有以下优势：

- 可在 Azure AD 中控制谁有权访问 OfficeSpace Software。
- 可以让用户使用其 Azure AD 帐户自动登录到 OfficeSpace Software（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 OfficeSpace Software 的集成，需要具有以下项：

- Azure AD 订阅
- 已启用 OfficeSpace Software 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 OfficeSpace Software
1. 配置和测试 Azure AD 单一登录

## <a name="adding-officespace-software-from-the-gallery"></a>从库中添加 OfficeSpace Software
要配置 OfficeSpace Software 与 Azure AD 的集成，需要从库中将 OfficeSpace Software 添加到托管 SaaS 应用列表。

**若要从库中添加 OfficeSpace Software，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

1. 在搜索框中，键入“OfficeSpace Software”，在结果面板中选择“OfficeSpace Software”，然后单击“添加”按钮添加该应用程序。

    ![结果列表中的 OfficeSpace Software](./media/officespace-tutorial/tutorial_officespace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，根据名为“Britta Simon”的测试用户的指示配置和测试 OfficeSpace Software 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 OfficeSpace Software 用户。 换句话说，需要在 Azure AD 用户与 OfficeSpace Software 中的相关用户之间建立关联关系。

在 OfficeSpace Software 中，将 Azure AD 中“用户名”的值指定为“用户名”的值来建立此链接关系。

若要配置并测试 OfficeSpace Software 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 OfficeSpace Software 测试用户](#create-a-officespace-software-test-user)** - 在 OfficeSpace Software 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 OfficeSpace Software 应用程序中配置单一登录。

**若要配置 OfficeSpace Software 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户的“OfficeSpace Software”应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/officespace-tutorial/tutorial_officespace_samlbase.png)

1. 在“OfficeSpace Software 域和 URL”部分中，执行以下步骤：

    ![OfficeSpace Software 域和 URL 单一登录信息](./media/officespace-tutorial/tutorial_officespace_url.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL： `https://<company name>.officespacesoftware.com/users/sign_in/saml`

    b. 在“标识符”文本框中，使用以下模式键入 URL：`<company name>.officespacesoftware.com`

    > [!NOTE] 
    > 这些不是实际值。 必须使用实际登录 URL 和标识符更新这些值。 请联系 [OfficeSpace Software 客户端支持团队](mailto:support@officespacesoftware.com)获取这些值。 

1. OfficeSpace Software 应用程序需要采用特定格式的 SAML 断言。 请为此应用程序配置以下声明。 可以在应用程序集成页的“用户属性”部分管理这些属性的值。 以下屏幕截图显示一个示例。
    
    ![配置属性](./media/officespace-tutorial/tutorial_officespace_attribute.png)

1. 在“单一登录”对话框上的“用户属性”部分中，选择“user.mail”作为**用户标识符**，并针对下表中所示的每一行，执行以下步骤：
    
    | 属性名称 | 属性值 |
    | --- | --- |    
    | 电子邮件 | user.mail |
    | 名称 | user.displayname |
    | first_name | user.givenname |
    | last_name | user.surname |

    a. 单击“添加属性”，打开“添加属性”对话框。

    ![配置“添加” ](./media/officespace-tutorial/tutorial_attribute_04.png)

    ![配置属性](./media/officespace-tutorial/tutorial_attribute_05.png)
    
    b. 在“名称”文本框中，键入为该行显示的属性名称。
    
    c. 在“值”列表中，选择为该行显示的属性值。
    
    d. 单击“确定”
 
1. 在“SAML 签名证书”部分中，复制证书的“指纹”值。

    ![证书下载链接](./media/officespace-tutorial/tutorial_officespace_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/officespace-tutorial/tutorial_general_400.png)

1. 在“OfficeSpace Software 配置”部分中，单击“配置 OfficeSpace Software”以打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL 和 SAML 单一登录服务 URL”。

    ![OfficeSpace Software 配置](./media/officespace-tutorial/tutorial_officespace_configure.png) 

1. 在另一 Web 浏览器窗口中，以管理员身份登录到 OfficeSpace Software 租户。

1. 转到“设置”，并单击“连接器”。

    ![在应用端配置单一登录](./media/officespace-tutorial/tutorial_officespace_002.png)

1. 单击“SAML 身份验证”。

    ![在应用端配置单一登录](./media/officespace-tutorial/tutorial_officespace_003.png)

1. 在“SAML 身份验证”部分中，执行以下步骤：

    ![在应用端配置单一登录](./media/officespace-tutorial/tutorial_officespace_004.png)

    a. 在“注销提供程序 URL”文本框中，粘贴从 Azure 门户复制的“注销 URL”值。

    b. 在“客户端 IDP 目标 URL”文本框中，粘贴从 Azure 门户复制的“SAML 单一登录服务 URL”值。

    c. 在“客户端 IDP 证书指纹”文本框中，粘贴从 Azure 门户复制的“指纹”值。 

    d. 单击“保存设置”。


> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/officespace-tutorial/create_aaduser_01.png)

1. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/officespace-tutorial/create_aaduser_02.png)

1. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/officespace-tutorial/create_aaduser_03.png)

1. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/officespace-tutorial/create_aaduser_04.png)

    a. 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。
 
### <a name="create-a-officespace-software-test-user"></a>创建 OfficeSpace Software 测试用户

本部分的目的是在 OfficeSpace Software 中创建名为“Britta Simon”的用户。 OfficeSpace Software 支持在默认情况下启用的实时预配。

此部分不存在任何操作项。 尝试访问 OfficeSpace Software 期间，如果该用户尚不存在，则将创建一个新用户。

> [!NOTE]
> 如果需要手动创建用户，则需联系 [OfficeSpace Software 支持团队](mailto:support@officespacesoftware.com)。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 OfficeSpace Software 的权限，以支持其使用 Azure 单一登录。

![分配用户角色][200] 

**要将 Britta Simon 分配到 OfficeSpace Software，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“OfficeSpace Software”。

    ![应用程序列表中的 OfficeSpace Software 链接](./media/officespace-tutorial/tutorial_officespace_app.png)  

1. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

当在访问面板中单击 OfficeSpace Software 磁贴时，应当会自动登录到 OfficeSpace Software 应用程序。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/officespace-tutorial/tutorial_general_01.png
[2]: ./media/officespace-tutorial/tutorial_general_02.png
[3]: ./media/officespace-tutorial/tutorial_general_03.png
[4]: ./media/officespace-tutorial/tutorial_general_04.png

[100]: ./media/officespace-tutorial/tutorial_general_100.png

[200]: ./media/officespace-tutorial/tutorial_general_200.png
[201]: ./media/officespace-tutorial/tutorial_general_201.png
[202]: ./media/officespace-tutorial/tutorial_general_202.png
[203]: ./media/officespace-tutorial/tutorial_general_203.png

