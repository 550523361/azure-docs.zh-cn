---
title: 教程：将 Azure Active Directory 与 MOVEit Transfer - Azure AD 集成集成 | Microsoft 文档
description: 了解如何配置 Azure Active Directory 和 MOVEit Transfer - Azure AD 集成之间的单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b50bf623046094509170b5b5efc091013499b51b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56169830"
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>教程：将 Azure Active Directory 与 MOVEit Transfer - Azure AD 集成

此教程介绍如何将 MOVEit Transfer - Azure AD 集成与 Azure Active Directory (Azure AD) 集成。

将 MOVEit Transfer - Azure AD 集成与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 MOVEit Transfer - Azure AD 集成。
- 可以让用户使用其 Azure AD 帐户自动登录到 MOVEit Transfer - Azure AD 集成（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 MOVEit Transfer - Azure AD 集成之间的集成，需要具有以下项：

- Azure AD 订阅
- 已启用 MOVEit Transfer - Azure AD 集成单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 MOVEit Transfer - Azure AD 集成
1. 配置和测试 Azure AD 单一登录

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a>从库中添加 MOVEit Transfer - Azure AD 集成
要配置 MOVEit Transfer - Azure AD 集成与 Azure AD 的集成，需要从库中将 MOVEit Transfer - Azure AD 集成添加到托管 SaaS 应用列表。

若要从库中添加 MOVEit Transfer - Azure AD 集成，请执行以下步骤：

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

1. 在搜索框中，键入“MOVEit Transfer - Azure AD 集成”，从结果面板中选择“MOVEit Transfer - Azure AD 集成”，然后单击“添加”按钮添加该应用程序。

    ![结果列表中的 MOVEit Transfer - Azure AD 集成](./media/moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，基于名为“Britta Simon”的测试用户配置和测试对 MOVEit Transfer - Azure AD 集成的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 MOVEit Transfer - Azure AD 集成用户。 换句话说，需要在 Azure AD 用户与 MOVEit Transfer - Azure AD 集成中的相关用户之间建立链接关系。

在 MOVEit Transfer - Azure AD 集成中，将 Azure AD 中“用户名”的值指定为“用户名”的值来建立此链接关系。

若要配置并测试 MOVEit Transfer - Azure AD 集成的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 MOVEit Transfer - Azure AD 集成测试用户](#create-a-moveit-transfer---azure-ad-integration-test-user)** - 在 MOVEit Transfer - Azure AD 集成中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 MOVEit Transfer - Azure AD 集成应用程序中配置单一登录。

若要配置 MOVEit Transfer - Azure AD 集成的 Azure AD 单一登录，请执行以下步骤：

1. 在 Azure 门户中的“MOVEit Transfer - Azure AD 集成”应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

1. 在“MOVEit Transfer - Azure AD 集成域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL： `https://contoso.com`

    b. 在“标识符”文本框中，使用以下模式键入 URL：`https://contoso.com/<tenatid>`

    c. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`    
     
    > [!NOTE] 
    > 这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 可以稍后在“服务提供程序元数据 URL”部分查看这些值或联系 [MOVEit Transfer - Azure AD 集成客户端支持团队](https://community.ipswitch.com/s/support)获取这些值。

1. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存元数据文件。

    ![证书下载链接](./media/moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/moveittransfer-tutorial/tutorial_general_400.png)
    
1. 以管理员身份登录到 MOVEit Transfer 租户。

1. 在左侧导航窗格上，单击“设置”。

    ![应用端上的“设置”部分](./media/moveittransfer-tutorial/tutorial_moveittransfer_000.png)

1. 单击位于“安全策略”->“用户身份验证”下的“单一登录”链接。

    ![应用端的安全策略](./media/moveittransfer-tutorial/tutorial_moveittransfer_001.png)

1. 单击元数据 URL 链接以下载元数据文档。

    ![服务提供程序元数据 URL](./media/moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * 验证“entityID”是否与“MOVEit Transfer - Azure AD 集成域和 URL”部分中的“标识符”匹配。
    * 验证“AssertionConsumerService”位置 URL 是否与“MOVEit Transfer - Azure AD 集成域和 URL”部分中的“回复 URL”匹配。
    
    ![在应用端配置单一登录](./media/moveittransfer-tutorial/tutorial_moveittransfer_007.png)

1. 单击“添加标识提供程序”按钮以添加新的联合身份提供程序。

    ![添加标识提供者](./media/moveittransfer-tutorial/tutorial_moveittransfer_003.png)

1. 单击“浏览...”以选择从 Azure 门户中下载的元数据文件，并单击“添加标识提供者”以上传所下载的文件。

    ![SAML 标识提供程序](./media/moveittransfer-tutorial/tutorial_moveittransfer_004.png)

1. 在“编辑联合身份提供程序设置...”页上，对于“启用”选择“是”，并单击“保存”。

    ![联合标识提供程序设置](./media/moveittransfer-tutorial/tutorial_moveittransfer_005.png)

1. 在“编辑联合标识提供者用户设置”页上，执行以下操作：
    
    ![编辑联合标识提供程序设置](./media/moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. 对于“登录名”，选择“SAML NameID”。
    
    b. 选择“其他”作为“全名”并在“属性名称”文本框中输入以下值：`http://schemas.microsoft.com/identity/claims/displayname`。
    
    c. 选择“其他”作为“电子邮件”并在“属性名称”文本框中输入以下值：`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`。
    
    d. 对于“登录时自动创建帐户”，选择“是”。
    
    e. 单击“保存”按钮。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/moveittransfer-tutorial/create_aaduser_01.png)

1. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/moveittransfer-tutorial/create_aaduser_02.png)

1. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/moveittransfer-tutorial/create_aaduser_03.png)

1. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/moveittransfer-tutorial/create_aaduser_04.png)

    a. 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a>创建 MOVEit Transfer - Azure AD 集成测试用户

本部分的目的是在 MOVEit Transfer - Azure AD 集成中创建名为 Britta Simon 的用户。 MOVEit Transfer - Azure AD 集成支持在默认情况下启用的实时预配。 此部分不存在任何操作项。 尝试访问 MOVEit Transfer - Azure AD 集成期间，如果尚不存在用户，则会创建一个新用户。

>[!NOTE]
>如果需要手动创建用户，则需联系 [MOVEit Transfer - Azure AD 集成客户端支持团队](https://community.ipswitch.com/s/support)。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 MOVEit Transfer - Azure AD 集成的权限，支持其使用 Azure 单一登录。

![分配用户角色][200] 

若要将 Britta Simon 分配到 MOVEit Transfer - Azure AD 集成，请执行以下步骤：

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“MOVEit Transfer - Azure AD 集成”。

    ![应用程序列表中的 MOVEit Transfer - Azure AD 集成链接](./media/moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

1. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

本部分旨在使用“访问面板”测试 Azure AD SSO 配置。

当在访问面板中单击 MOVEit Transfer - Azure AD 集成磁贴时，应当会自动登录到 MOVEit Transfer - Azure AD 集成应用程序。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/moveittransfer-tutorial/tutorial_general_203.png

