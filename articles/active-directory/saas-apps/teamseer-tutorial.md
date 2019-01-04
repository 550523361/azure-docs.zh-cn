---
title: 教程：Azure Active Directory 与 TeamSeer 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 TeamSeer 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 2b9f3c7905fbb301c74a040a259b2f8666c75377
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834480"
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>教程：Azure Active Directory 与 TeamSeer 集成

本教程介绍如何将 TeamSeer 与 Azure Active Directory (Azure AD) 集成。

将 TeamSeer 与 Azure AD 集成提供以下优势：

- 可在 Azure AD 中控制谁有权访问 TeamSeer
- 可以让用户使用其 Azure AD 帐户自动登录到 TeamSeer（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 TeamSeer 的集成，需要以下项：

- Azure AD 订阅
- 已启用 TeamSeer 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 TeamSeer
1. 配置和测试 Azure AD 单一登录

## <a name="adding-teamseer-from-the-gallery"></a>从库中添加 TeamSeer
若要配置 TeamSeer 与 Azure AD 的集成，需要从库中将 TeamSeer 添加到托管 SaaS 应用列表。

**若要从库中添加 TeamSeer，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“TeamSeer”。

    ![创建 Azure AD 测试用户](./media/teamseer-tutorial/tutorial_teamseer_search.png)

1. 在结果面板中，选择“TeamSeer”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 TeamSeer 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 TeamSeer 用户。 换句话说，需要在 Azure AD 用户与 TeamSeer 中的相关用户之间建立链接关系。

可通过将 Azure AD 中“用户名”的值指定为 TeamSeer 中“用户名”的值来建立此链接关系。

若要配置和测试 TeamSeer 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 TeamSeer 测试用户](#creating-a-teamseer-test-user)** - 在 TeamSeer 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 TeamSeer 应用程序中配置单一登录。

**若要配置 TeamSeer 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 **TeamSeer** 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/teamseer-tutorial/tutorial_teamseer_samlbase.png)

1. 在“TeamSeer 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/teamseer-tutorial/tutorial_teamseer_url.png)

     在“登录 URL”文本框中，使用以下模式键入 URL： `https://www.teamseer.com/<companyid>`

    > [!NOTE] 
    > 此值不是真实值。 使用实际登录 URL 更新该值。 若要获取该值，请与 [TeamSeer 客户端支持团队](https://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html)联系。 
 
1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![配置单一登录](./media/teamseer-tutorial/tutorial_teamseer_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录](./media/teamseer-tutorial/tutorial_general_400.png)

1. 在“TeamSeer 配置”部分中，单击“配置 TeamSeer”打开“配置登录”窗口。 从“快速参考”部分中复制“SAML 单一登录服务 URL”

    ![配置单一登录](./media/teamseer-tutorial/tutorial_teamseer_configure.png)

1. 在另一个 Web 浏览器窗口中，以管理员身份登录 TeamSeer 公司站点。

1. 转到“HR 管理员”。
   
    ![HR 管理员](./media/teamseer-tutorial/ic789634.png "HR 管理员")

1. 单击“设置”。
   
    ![设置](./media/teamseer-tutorial/ic789635.png "设置")

1. 单击“设置 SAML 提供程序详细信息”。
   
    ![SAML 设置](./media/teamseer-tutorial/ic789636.png "SAML 设置")

1. 在“SAML 提供程序详细信息”部分中，执行以下步骤：
   
    ![SAML 设置](./media/teamseer-tutorial/ic789637.png "SAML 设置")   

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 将“单一登录服务 URL”值粘贴到“URL”文本框中。
          
    b. 在记事本中打开 base-64 编码证书，将其内容复制到剪贴板，然后将其粘贴到“IdP 公共证书”文本框中。

1. 若要完成 SAML 提供程序配置，请执行以下步骤：
    
    ![SAML 设置](./media/teamseer-tutorial/ic789638.png "SAML 设置") 

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“测试电子邮件地址”中，键入测试用户的电子邮件地址。 
  
    b. 在“颁发者”文本框中，键入服务提供商的颁发者 URL。 
  
    c. 单击“ **保存**”。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/teamseer-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/teamseer-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/teamseer-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/teamseer-tutorial/create_aaduser_04.png) 

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-teamseer-test-user"></a>创建 TeamSeer 测试用户

要使 Azure AD 用户能够登录 TeamSeer，必须将这些用户预配到 TeamSeer 中。 对于 TeamSeer，预配是一项手动任务。

**若要预配用户帐户，请执行以下步骤：**

1. 以管理员身份登录 **TeamSeer** 公司站点。

1. 执行以下步骤：
   
    ![HR 管理员](./media/teamseer-tutorial/ic789640.png "HR 管理员")  
 
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 转到“HR 管理员”\>“用户”。
  
    b. 单击“运行新建用户向导”。

1. 在“用户详细信息”部分中，执行以下步骤：
   
    ![用户详细信息](./media/teamseer-tutorial/ic789641.png "用户详细信息")

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在相关文本框中键入要预配的有效 AAD 帐户的“名字”、“姓氏”和“用户名(电子邮件地址)”。
  
    b. 单击“下一步”。

1. 按照屏幕上的说明添加新用户，然后单击“完成”。

>[!NOTE]
>可以使用任何其他 TeamSeer 用户帐户创建工具或 TeamSeer 提供的 API 来预配 Azure AD 用户帐户。 

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 TeamSeer 的权限，允许她使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 TeamSeer，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“TeamSeer”。

    ![配置单一登录](./media/teamseer-tutorial/tutorial_teamseer_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

如果要测试单一登录设置，请打开访问面板。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/teamseer-tutorial/tutorial_general_01.png
[2]: ./media/teamseer-tutorial/tutorial_general_02.png
[3]: ./media/teamseer-tutorial/tutorial_general_03.png
[4]: ./media/teamseer-tutorial/tutorial_general_04.png

[100]: ./media/teamseer-tutorial/tutorial_general_100.png

[200]: ./media/teamseer-tutorial/tutorial_general_200.png
[201]: ./media/teamseer-tutorial/tutorial_general_201.png
[202]: ./media/teamseer-tutorial/tutorial_general_202.png
[203]: ./media/teamseer-tutorial/tutorial_general_203.png

