---
title: 教程：Azure Active Directory 与 ITRP 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 ITRP 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: e09716a3-4200-4853-9414-2390e6c10d98
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 66318557af8f4ad5ad85efb19b6ab9066aa3c5ed
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55163460"
---
# <a name="tutorial-azure-active-directory-integration-with-itrp"></a>教程：Azure Active Directory 与 ITRP 集成

本教程介绍如何将 ITRP 与 Azure Active Directory (Azure AD) 集成。

将 ITRP 与 Azure AD 集成具有以下优势：

- 可在 Azure AD 中控制谁有权访问 ITRP
- 可使用户通过其 Azure AD 帐户自动登录到 ITRP（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 ITRP 的集成，需要以下项：

- Azure AD 订阅
- 已启用 ITRP 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 ITRP
1. 配置和测试 Azure AD 单一登录

## <a name="adding-itrp-from-the-gallery"></a>从库中添加 ITRP
若要配置 ITRP 与 Azure AD 的集成，需从库中将 ITRP 添加到托管 SaaS 应用列表。

**若要从库中添加 ITRP，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“ITRP”。

    ![创建 Azure AD 测试用户](./media/itrp-tutorial/tutorial_itrp_search.png)

1. 在结果面板中，选择“ITRP”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/itrp-tutorial/tutorial_itrp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，基于名为“Britta Simon”的测试用户配置和测试 ITRP 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 ITRP 用户。 换句话说，需要建立 Azure AD 用户与 ITRP 中相关用户之间的链接关系。

可通过将 Azure AD 中“用户名”的值指定为 ITRP 中“用户名”的值来建立此链接关系。

若要配置和测试 ITRP 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 ITRP 测试用户](#creating-an-itrp-test-user)** - 在 ITRP 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 ITRP 应用程序中配置单一登录。

**若要配置 ITRP 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的“ITRP”应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/itrp-tutorial/tutorial_itrp_samlbase.png)

1. 在“ITRP 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/itrp-tutorial/tutorial_itrp_url.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL： `https://<tenant-name>.itrp.com`

    b. 在“标识符”文本框中，使用以下模式键入 URL：`https://<tenant-name>.itrp.com`

    > [!NOTE] 
    > 这些不是实际值。 必须使用实际登录 URL 和标识符更新这些值。 请联系 [ITRP 客户端支持团队](https://www.itrp.com/support)获取这些值。 
 
1. 在“SAML 签名证书”部分中，复制证书的指纹值。

    ![配置单一登录](./media/itrp-tutorial/tutorial_itrp_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录](./media/itrp-tutorial/tutorial_general_400.png)

1. 在“ITRP 配置”部分，单击“配置 ITRP”，打开“配置登录”窗口。 从“快速参考”部分中复制“SAML 单一登录服务 URL”和“注销 URL”。

    ![配置单一登录](./media/itrp-tutorial/tutorial_itrp_configure.png) 

1. 在另一个 Web 浏览器窗口中，以管理员身份登录到 ITRP 公司站点。

1. 在顶部工具栏中，单击“设置”。
   
    ![ITRP](./media/itrp-tutorial/ic775570.png "ITRP")

1. 在左侧导航窗格中，选择“单一登录”。
   
    ![单一登录](./media/itrp-tutorial/ic775571.png "单一登录")

1. 在“单一登录配置”部分中，执行以下步骤：
   
    ![单一登录](./media/itrp-tutorial/ic775572.png "单一登录")
    
    ![单一登录](./media/itrp-tutorial/ic775573.png "单一登录")   

    a. 单击“启用”。

    b. 在“远程注销 URL”文本框中，粘贴从 Azure 门户复制的“注销 URL”值 ****。

    c. 在“SAML SSO URL”文本框中，粘贴从 Azure 门户复制的“SAML 单一登录服务 URL”值 ****。

    d.在“证书指纹”文本框中，粘贴从 Azure 门户复制的证书“指纹”值。 
      
1. 单击“ **保存**”。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/itrp-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/itrp-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/itrp-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/itrp-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-an-itrp-test-user"></a>创建 ITRP 测试用户

为了使 Azure AD 用户能够登录到 ITRP，必须将其预配到 ITRP 中。  

对于 ITRP，预配是一项手动任务。

**若要预配用户帐户，请执行以下步骤：**

1. 登录到 **ITRP** 租户。

1. 在顶部工具栏中，单击“记录”。
   
    ![管理员](./media/itrp-tutorial/ic775575.png "管理员")

1. 在弹出菜单中，选择“人员”。
   
    ![人员](./media/itrp-tutorial/ic775587.png "人员")

1. 单击“添加新人员”(“+”)。
   
    ![管理员](./media/itrp-tutorial/ic775576.png "管理员")

1. 在“添加新人员”对话框中，执行以下步骤：
   
    ![用户](./media/itrp-tutorial/ic775577.png "用户") 
      
    a. 键入要预配的有效 AAD 帐户的“名称”、“电子邮件”。

    b. 单击“ **保存**”。

>[!NOTE]
>可以使用 ITRP 提供的任何其他 ITRP 用户帐户创建工具或 API 来预配 AAD 用户帐户。 
> 

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 对 ITRP 的访问权限，使其能够使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 ITRP，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“ITRP”。

    ![配置单一登录](./media/itrp-tutorial/tutorial_itrp_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 ITRP 磁贴时，应自动登录到 ITRP 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/itrp-tutorial/tutorial_general_01.png
[2]: ./media/itrp-tutorial/tutorial_general_02.png
[3]: ./media/itrp-tutorial/tutorial_general_03.png
[4]: ./media/itrp-tutorial/tutorial_general_04.png

[100]: ./media/itrp-tutorial/tutorial_general_100.png

[200]: ./media/itrp-tutorial/tutorial_general_200.png
[201]: ./media/itrp-tutorial/tutorial_general_201.png
[202]: ./media/itrp-tutorial/tutorial_general_202.png
[203]: ./media/itrp-tutorial/tutorial_general_203.png

