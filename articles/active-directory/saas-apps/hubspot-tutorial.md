---
title: 教程：Azure Active Directory 与 HubSpot 的集成 | Microsoft 文档
description: 了解如何在 Azure Active Directory 与 HubSpot 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 57343ccd-53ea-4e62-9e54-dee2a9562ed5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8bbb307654d4aaf753a4a3284875dee4f5707f2a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/18/2019
ms.locfileid: "57901708"
---
# <a name="tutorial-azure-active-directory-integration-with-hubspot"></a>教程：Azure Active Directory 与 HubSpot 的集成

在本教程中，了解如何将 HubSpot 与 Azure Active Directory (Azure AD) 集成。

将 HubSpot 与 Azure AD 集成提供以下优势：

- 可在 Azure AD 中控制谁有权访问 HubSpot
- 可以让用户使用其 Azure AD 帐户自动登录到 HubSpot（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>必备组件

若要配置 Azure AD 与 HubSpot 的集成，需要以下项：

- Azure AD 订阅
- 启用了 HubSpot 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述

在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 HubSpot
2. 配置和测试 Azure AD 单一登录

## <a name="adding-hubspot-from-the-gallery"></a>从库中添加 HubSpot

要配置 HubSpot 与 Azure AD 的集成，需要从库中将 HubSpot 添加到托管 SaaS 应用列表。

若要从库中添加 HubSpot，请执行以下步骤：

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。

    ![Active Directory][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]

3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

4. 在搜索框中，键入“HubSpot”。 从结果面板中选择“HubSpot”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/hubspot-tutorial/tutorial_hubspot_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，基于一个名为“Britta Simon”的测试用户使用 HubSpot 配置和测试 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 HubSpot 用户。 换句话说，需要建立 Azure AD 用户与 HubSpot 中相关用户之间的链接关系。

若要配置和测试 HubSpot 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **创建 HubSpot 测试用户** - 在 HubSpot 中创建 Britta Simon 的对应用户，将其链接到用户的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 HubSpot 应用程序中配置单一登录。

若要配置 HubSpot 的 Azure AD 单一登录，请执行以下步骤：

1. 在 Azure 门户中，在 HubSpot 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

2. 在“选择单一登录方法”对话框中，单击“SAML”模式对应的“选择”，以启用单一登录。

    ![配置单一登录](./media/hubspot-tutorial/tutorial_general_301.png)

3. 如果你需要从任何其他模式更改为 SAML 模式，请单击屏幕顶部的“更改单一登录模式”。

    ![配置单一登录](./media/hubspot-tutorial/tutorial_general_300.png)

4. 在“使用 SAML 设置单一登录”页上，单击“编辑”图标以打开“基本 SAML 配置”对话框。

    ![配置单一登录](./media/hubspot-tutorial/tutorial_general_302.png)

5. 如果要在 IDP 发起的模式下配置应用程序，请在“基本 SAML 配置”部分中执行以下步骤：

    ![HubSpot 域和 URL 单一登录信息](./media/hubspot-tutorial/tutorial_hubspot_url.png)

    a. 在“标识符”文本框中，使用以下模式键入 URL：`https://api.hubspot.com/login-api/v1/saml/login?portalId=<CUSTOMER ID>`

    b. 在“回复 URL”文本框中，键入使用以下模式的 URL：`https://api.hubspot.com/login-api/v1/saml/acs?portalId=<CUSTOMER ID>`

    > [!NOTE]
    > 这些不是实际值。 本教程稍后将介绍如何使用实际的标识符和回复 URL 来更新该值。 请联系 [HubSpot 客户端支持团队](https://help.hubspot.com/)获取这些值。

    c. 如果要在 SP 发起的模式下配置应用程序，请单击“设置其他 URL”，并执行以下步骤：

    ![HubSpot 域和 URL 单一登录信息](./media/hubspot-tutorial/tutorial_hubspot_url1.png)

    在“登录 URL”文本框中，键入 URL：`https://app.hubspot.com/login`

6. 在“SAML 签名证书”页的“SAML 签名证书”部分中，单击“下载”以下载“证书(Base64)”并将证书文件保存在计算机上。

    ![配置单一登录](./media/hubspot-tutorial/tutorial_hubspot_certificate.png)

7. 在“设置 HubSpot”部分中，单击“复制”按钮以复制“登录 URL”和“Azure AD 标识符”值。

    ![配置单一登录](./media/hubspot-tutorial/tutorial_hubspot_configure.png)

8. 在浏览器中打开新选项卡并登录到 HubSpot 管理员帐户。

9. 单击页面右上角的“设置”图标。

    ![配置单一登录](./media/hubspot-tutorial/config1.png)

10. 单击“帐户默认值”。

    ![配置单一登录](./media/hubspot-tutorial/config2.png)

11. 向下滚动到“安全”部分，并单击“设置”。

    ![配置单一登录](./media/hubspot-tutorial/config3.png)

12. 在“设置单一登录”部分中，执行以下步骤：

    ![配置单一登录](./media/hubspot-tutorial/config4.png)

    a. 单击“复制”按钮以复制“受众 URI (服务提供商实体 ID)”值，并将其粘贴到 Azure 门户中“基本 SAML 配置”部分的“标识符”文本框中。

    b. 单击“复制”按钮以复制“登录 URI、ACS、收件人或重定向”值，并将其粘贴到 Azure 门户中“基本 SAML 配置”部分的“回复 URL”文本框中。

    c. 在“标识提供者标识符或证书颁发者 URL”文本框中，粘贴从 Azure 门户复制的“Azure AD 标识符”值。

    d. 在“标识提供者单一登录 URL”文本框中，粘贴从 Azure 门户复制的“登录 URL”值。

    e. 在记事本中打开已下载的  **Certificate(Base64)**  文件。 将其内容复制到剪贴板，然后粘贴到“X.509 证书”框 ****。

    f. 单击“验证”。

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”。

    ![创建 Azure AD 用户][100]

2. 选择屏幕顶部的“新建用户”。

    ![创建 Azure AD 测试用户](./media/hubspot-tutorial/create_aaduser_01.png) 

3. 在“用户属性”中，按照以下步骤操作。

    ![创建 Azure AD 测试用户](./media/hubspot-tutorial/create_aaduser_02.png)

    a. 在“名称”字段中，输入 BrittaSimon。
  
    b. 在中**用户名**字段中，键入**brittasimon\@yourcompanydomain.extension**  
    例如： BrittaSimon@contoso.com

    c. 选择“属性”，再选择“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 选择“创建”。

### <a name="creating-a-hubspot-test-user"></a>创建 HubSpot 测试用户

为了使 Azure AD 用户能够登录到 HubSpot，必须将其预配到 HubSpot 中。
就 HubSpot 来说，需要手动执行预配。

**若要预配用户帐户，请执行以下步骤：**

1. 以管理员身份登录到 HubSpot 公司站点。

2. 单击页面右上角的“设置”图标。

    ![配置单一登录](./media/hubspot-tutorial/config1.png)

3. 单击“用户和团队”。

    ![配置单一登录](./media/hubspot-tutorial/user1.png)

4. 单击“创建用户”。

    ![配置单一登录](./media/hubspot-tutorial/user2.png)

5. 输入类似用户的电子邮件地址**brittasimon\@contoso.com**中**添加电子邮件 addess(es)** 文本框中，单击**下一步**。

    ![配置单一登录](./media/hubspot-tutorial/user3.png)

6. 在“创建用户”部分中，请浏览各个选项卡并为用户选择相应的选项和权限，然后单击“下一步”。

    ![配置单一登录](./media/hubspot-tutorial/user4.png)

7. 单击“发送”以向用户发送邀请。

    ![配置单一登录](./media/hubspot-tutorial/user5.png)

    > [!NOTE]
    > 接受邀请后，将激活用户。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过向 Britta Simon 授予 HubSpot 的访问权限支持她使用 Azure 单一登录。

1. 在 Azure 门户中，选择“企业应用程序”，然后选择“所有应用程序”。

    ![分配用户][201]

2. 在应用程序列表中，选择“HubSpot”。

    ![配置单一登录](./media/hubspot-tutorial/tutorial_hubspot_app.png) 

3. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202]

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

5. 在“用户和组”对话框中，选择“用户”列表中的 Britta Simon，然后单击屏幕底部的“选择”按钮。

6. 在“添加分配”对话框中，选择“分配”按钮。

### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击 HubSpot 磁贴时，应自动登录到 HubSpot 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/hubspot-tutorial/tutorial_general_01.png
[2]: ./media/hubspot-tutorial/tutorial_general_02.png
[3]: ./media/hubspot-tutorial/tutorial_general_03.png
[4]: ./media/hubspot-tutorial/tutorial_general_04.png
[100]: ./media/hubspot-tutorial/tutorial_general_100.png
[200]: ./media/hubspot-tutorial/tutorial_general_200.png
[201]: ./media/hubspot-tutorial/tutorial_general_201.png
[202]: ./media/hubspot-tutorial/tutorial_general_202.png
[203]: ./media/hubspot-tutorial/tutorial_general_203.png
