---
title: 教程：Azure Active Directory 与 T&E Express 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 T&E Express 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97e294a8b0e34e1fac24d0cbe01791e2eb290a43
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162572"
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a>教程：Azure Active Directory 与 T&E Express 集成

在本教程中，了解如何将 T&E Express 与 Azure Active Directory (Azure AD) 集成。

将 T&E Express 与 Azure AD 集成提供以下优势：

- 可在 Azure AD 中控制谁有权访问 T&E Express
- 可以让用户使用其 Azure AD 帐户自动登录到 T&E Express（单一登录）
- 可在一个中心位置（即 Azure 管理门户）管理帐户

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 T&E Express 的集成，需要以下项：

- Azure AD 订阅
- 已启用 T&E Express 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 T&E Express
1. 配置和测试 Azure AD 单一登录

## <a name="adding-te-express-from-the-gallery"></a>从库中添加 T&E Express
若要配置 T&E Express 与 Azure AD 的集成，需要从库中将 T&E Express 添加到托管 SaaS 应用列表。

**若要从库中添加 T&E Express，请执行以下步骤：**

1. 在 **[Azure 管理门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 单击对话框顶部的“添加”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“T&E Express”。

    ![创建 Azure AD 测试用户](./media/tyeexpress-tutorial/tutorial_tyeexpress_search.png)

1. 在结果窗格中，选择“T&E Express”，并单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/tyeexpress-tutorial/tutorial_tyeexpress_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 T&E Express 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 T&E Express 用户。 换句话说，需要建立 Azure AD 用户与 T&E Express 中相关用户之间的链接关系。

通过将 Azure AD 中“用户名”的值指定为 T&E Express 中“用户名”的值来建立此链接关系。

若要配置和测试 T&E Express 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 T&E Express 测试用户](#creating-a-te-express-test-user)** - 在 T&E Express 中创建 Britta Simon 的对应用户，并将其链接到她的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 管理门户中启用 Azure AD 单一登录并在 T&E Express 应用程序中配置单一登录。

**若要配置 T&E Express 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 管理门户的“T&E Express”应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/tyeexpress-tutorial/tutorial_tyeexpress_samlbase.png)

1. 在“T&E Express 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/tyeexpress-tutorial/tutorial_tyeexpress_url.png)

    a. 在“标识符”文本框中，键入 `https://<domain>.tyeexpress.com` 值

    b. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`

    > [!NOTE] 
    > 请注意，这些不是实际值。 必须使用实际标识符和回复 URL 更新这些值。 此处我们建议在“标识符”中使用字符串的唯一值。 若要获取这些值，请联系 [ 支持团队](http://www.tyeexpress.com/contacto.aspx)。

1. 在“SAML 签名证书”部分中，单击“元数据 XML”，并在计算机上保存 XML 文件。

    ![配置单一登录](./media/tyeexpress-tutorial/tutorial_tyeexpress_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录](./media/tyeexpress-tutorial/tutorial_general_400.png)

1. 要在 **T&E Express** 中配置单一登录，请使用管理员凭据登录 T&E Express 应用程序，而不使用 SAML 单一登录。

1. 在“管理员”选项卡下，单击“SAML 域”打开 SAML 设置页。

    ![配置单一登录](./media/tyeexpress-tutorial/tye-SAML.png)

1. 将“Activar(Activate)”选项从“No”更改为“SI(Yes)”。 在“标识提供程序元数据”文本框中，粘贴从 Azure 门户下载的元数据 XML。

    ![配置单一登录](./media/tyeexpress-tutorial/tyeAdmin.png)

1. 单击“Guardar(Save)”按钮保存设置。  


### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 管理门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 管理门户的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/tyeexpress-tutorial/create_aaduser_01.png) 

1. 转到“用户和组”，单击“所有用户”显示用户列表。
    
    ![创建 Azure AD 测试用户](./media/tyeexpress-tutorial/create_aaduser_02.png) 

1. 在对话框顶部单击“添加”，打开“用户”对话框。
 
    ![创建 Azure AD 测试用户](./media/tyeexpress-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/tyeexpress-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-te-express-test-user"></a>创建 T&E Express 测试用户

要使 Azure AD 用户能够登录 T&E Express，必须将这些用户预配到 T&E Express 中。  
就 T&E Express 来说，预配任务需要手动完成。

**若要预配用户帐户，请执行以下步骤：**

1. 以管理员身份登录 T&E Express 公司站点。

1. 在管理员标签下，单击“用户”打开用户母版页。

    ![添加员工](./media/tyeexpress-tutorial/tye-adminusers.png)

1. 在主页上单击“+”添加用户。

    ![添加员工](./media/tyeexpress-tutorial/tye-usershome.png)

1. 输入表单中所有必须填写的详细信息，并单击保存按钮进行保存。

    ![添加员工](./media/tyeexpress-tutorial/tye-usersadd.png)

    ![添加员工](./media/tyeexpress-tutorial/tye-userssave.png)


### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 T&E Express 的权限，允许她使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 T&E Express，请执行以下步骤：**

1. 在 Azure 管理门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“T&E Express”。

    ![配置单一登录](./media/tyeexpress-tutorial/tutorial_tyeexpress_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的“T&E Express”磁贴时，用户应自动登录到 T&E Express 应用程序。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tyeexpress-tutorial/tutorial_general_01.png
[2]: ./media/tyeexpress-tutorial/tutorial_general_02.png
[3]: ./media/tyeexpress-tutorial/tutorial_general_03.png
[4]: ./media/tyeexpress-tutorial/tutorial_general_04.png

[100]: ./media/tyeexpress-tutorial/tutorial_general_100.png

[200]: ./media/tyeexpress-tutorial/tutorial_general_200.png
[201]: ./media/tyeexpress-tutorial/tutorial_general_201.png
[202]: ./media/tyeexpress-tutorial/tutorial_general_202.png
[203]: ./media/tyeexpress-tutorial/tutorial_general_203.png

