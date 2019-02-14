---
title: 教程：Azure Active Directory 与 Marketo 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Marketo 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: abab9f6e38fcf69dcb04bfea0f84d883dc5267b7
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56199657"
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>教程：Azure Active Directory 与 Marketo 的集成

本教程介绍了如何将 Marketo 与 Azure Active Directory (Azure AD) 进行集成。

将 Marketo 与 Azure AD 集成可提供以下优势：

- 可以在 Azure AD 中控制谁有权访问 Marketo
- 可以让用户使用其 Azure AD 帐户自动登录到 Marketo（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Marketo 的集成，需要具有以下项：

- Azure AD 订阅
- 启用了 Marketo 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Marketo
1. 配置和测试 Azure AD 单一登录

## <a name="adding-marketo-from-the-gallery"></a>从库中添加 Marketo
要配置 Marketo 与 Azure AD 的集成，需要从库中将 Marketo 添加到托管 SaaS 应用列表。

**若要从库中添加 Marketo，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“Marketo”。

    ![创建 Azure AD 测试用户](./media/marketo-tutorial/tutorial_marketo_search.png)

1. 在结果窗格中，选择“Marketo”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于名为“Britta Simon”的测试用户使用 Marketo 配置和测试 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Marketo 用户。 换句话说，需要在 Azure AD 用户与 Marketo 中的相关用户之间建立链接关系。

可通过将 Azure AD 中“用户名”的值指定为 Marketo 中“用户名”的值来建立此链接关系。

若要配置并测试 Marketo 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 Marketo 测试用户](#creating-a-marketo-test-user)** - 在 Marketo 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 Marketo 应用程序中配置单一登录。

**若要配置 Marketo 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的“Marketo”应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_samlbase.png)

1. 在“Marketo 域和 URL”部分，执行以下步骤：

    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_url.png)

    a. 在“标识符”文本框中，使用以下模式键入 URL：`https://saml.marketo.com/sp`

    b. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE] 
    > 这些不是实际值。 请使用实际标识符和回复 URL 更新这些值。 请联系 [Marketo 支持团队](http://investors.marketo.com/contactus.cfm)获取这些值。
 
1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录](./media/marketo-tutorial/tutorial_general_400.png)

1. 在“Marketo 配置”部分中，单击“配置 Marketo”以打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”。

    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_configure.png) 

1. 若要获取应用程序的 Munchkin Id，请使用管理员凭据登录到 Marketo 并执行以下操作：
   
    a. 使用管理员凭据登录到 Marketo 应用。
   
    b. 单击顶部导航窗格上的“管理员”按钮。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. 导航到“集成”菜单，单击“Munchkin 链接”。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_11.png)
   
    d. 复制屏幕上显示的 Munchkin Id，并在 Azure AD 配置向导中填写回复 URL。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_12.png) 

1. 若要在应用程序中配置 SSO，请执行以下步骤：
   
    a. 使用管理员凭据登录到 Marketo 应用。
   
    b. 单击顶部导航窗格上的“管理员”按钮。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. 导航到“集成”菜单，然后单击“单一登录”。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_07.png) 
   
    d. 若要启用 SAML 设置，请单击“编辑”按钮。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_08.png) 
   
    e. 已启用的单一登录设置。
   
    f. 将“SAML 实体 ID”粘贴到“颁发者 ID”文本框中。
   
    g. 在“实体 ID”文本框中，输入 `http://saml.marketo.com/sp` 作为URL。
   
    h. 对于“名称标识符元素”，选择“用户 ID 位置”。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > 如果用户标识符不是 UPN 值，则在“属性”选项卡中更改该值。
   
    i. 上传从 Azure AD 配置向导下载的证书。 保存设置。
   
    j. 编辑“重定向页面”设置。
   
    k. 将“SAML 单一登录服务 URL”粘贴到“登录 URL”文本框。
   
    l. 将“注销 URL”粘贴到“注销 URL”文本框中。
   
    m. 在“错误 URL”中，复制 Marketo 实例 URL 并单击“保存”按钮以保存设置。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_10.png)

1. 若要为用户启用 SSO，请完成以下操作：
   
    a. 使用管理员凭据登录到 Marketo 应用。
   
    b. 单击顶部导航窗格上的“管理员”按钮。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. 导航到“安全性”菜单，并单击“登录设置”。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_13.png)
   
    d. 选中“需要 SSO”选项并保存设置。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/marketo-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/marketo-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/marketo-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/marketo-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-marketo-test-user"></a>创建一个 Marketo 测试用户

在本部分中，会在 Marketo 中创建一个名为 Britta Simon 的用户。 请按照以下步骤在 Marketo 平台中创建用户。

1. 使用管理员凭据登录到 Marketo 应用。

1. 单击顶部导航窗格上的“管理员”按钮。
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_06.png) 

1. 导航到“安全性”菜单，单击“用户和角色”
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_19.png)  

1. 在“用户”选项卡上单击“邀请新用户”链接
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_15.png) 

1. 在“邀请新用户”向导中，填写以下信息
   
    a. 在文本框中输入用户**电子邮件**地址
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_16.png)
   
    b. 在文本框中输入**名字**
   
    c. 在文本框中输入**姓氏**
   
    d. 点击“下一步”

1. 在“权限”选项卡中，选择“用户角色”并单击“下一步”
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_17.png)
1. 单击“发送”按钮以发送用户邀请
   
    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_18.png)

1. 用户将收到电子邮件通知，且必须单击链接并更改密码以激活帐户。 

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，将通过向 Britta Simon 授予对 Marketo 的访问权限，使其能够使用 Azure 单一登录。

![分配用户][200] 

**要将 Britta Simon 分配到 Marketo，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“Marketo”。

    ![配置单一登录](./media/marketo-tutorial/tutorial_marketo_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

当在访问面板中单击 Marketo 磁贴时，应当会自动登录到 Marketo 应用程序。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/marketo-tutorial/tutorial_general_01.png
[2]: ./media/marketo-tutorial/tutorial_general_02.png
[3]: ./media/marketo-tutorial/tutorial_general_03.png
[4]: ./media/marketo-tutorial/tutorial_general_04.png

[100]: ./media/marketo-tutorial/tutorial_general_100.png

[200]: ./media/marketo-tutorial/tutorial_general_200.png
[201]: ./media/marketo-tutorial/tutorial_general_201.png
[202]: ./media/marketo-tutorial/tutorial_general_202.png
[203]: ./media/marketo-tutorial/tutorial_general_203.png

