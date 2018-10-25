---
title: 教程：Azure Active Directory 与 Jamf Pro 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 Jamf Pro 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: d28e28a2c4f8144da16c4838f07c9b8bb5ce67f0
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268151"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>教程：Azure Active Directory 与 Jamf Pro 集成

在本教程中，了解如何将 Jamf Pro 与 Azure Active Directory (Azure AD) 集成。

将 Jamf Pro 与 Azure AD 集成可以提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Jamf Pro。
- 可使用户通过其 Azure AD 帐户自动登录 Jamf Pro（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Jamf Pro 的集成，需要准备好以下各项：

- Azure AD 订阅
- 启用了 Jamf Pro 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述

在本教程中，将在测试环境中测试 Azure AD 单一登录。
本教程中概述的方案包括两个主要构建基块：

1. 从库添加 Jamf Pro
2. 配置和测试 Azure AD 单一登录

## <a name="adding-jamf-pro-from-the-gallery"></a>从库添加 Jamf Pro

要配置 Jamf Pro 与 Azure AD 的集成，需要从库中将 Jamf Pro 添加到托管 SaaS 应用列表。

若要从库中添加 Jamf Pro，请执行以下步骤：

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![图像](./media/jamfprosamlconnector-tutorial/selectazuread.png)

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![图像](./media/jamfprosamlconnector-tutorial/a_select_app.png)
    
3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![图像](./media/jamfprosamlconnector-tutorial/a_new_app.png)

4. 在搜索框中，键入“Jamf Pro”，在结果面板中选择“Jamf Pro”，然后单击“添加”按钮添加该应用程序。

     ![图像](./media/jamfprosamlconnector-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，基于一个名为“Britta Simon”的测试用户使用 Jamf Pro 配置和测试 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Jamf Pro 用户。 换句话说，需要建立 Azure AD 用户与 Jamf Pro 中相关用户之间的链接关系。

若要配置和测试 Jamf Pro 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
2. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 Jamf Pro 测试用户](#create-a-jamf-pro-test-user)** - 在 Jamf Pro 中创建 Britta Simon 的对应用户，并将其链接到用户的 Azure AD 身份。
4. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
5. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 Jamf Pro 应用程序中配置单一登录。

若要配置 Jamf Pro 的 Azure AD 单一登录，请执行以下步骤：

1. 在 [Azure 门户](https://portal.azure.com/)中的“Jamf Pro”应用程序集成页上，选择“单一登录”。

    ![图像](./media/jamfprosamlconnector-tutorial/b1_b2_select_sso.png)

2. 单击屏幕顶部的“更改单一登录模式”，以选择“SAML”模式。

      ![图像](./media/jamfprosamlconnector-tutorial/b1_b2_saml_ssso.png)

3. 在“选择单一登录方法”对话框中，单击“SAML”模式对应的“选择”，以启用单一登录。

    ![图像](./media/jamfprosamlconnector-tutorial/b1_b2_saml_sso.png)

4. 在“设置 SAML 单一登录”页上，单击“编辑”按钮，以打开“基本 SAML 配置”对话框。

    ![图像](./media/jamfprosamlconnector-tutorial/b1-domains_and_urlsedit.png)

5. 在“基本 SAML 配置”部分中，按照以下步骤操作：

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“标识符”文本框中，使用以下模式键入 URL：`https://<subdomain>.jamfcloud.com/saml/metadata`。

    b. 在“回复 URL”文本框中，使用以下模式键入 URL：`https://<subdomain>.jamfcloud.com/saml/SSO`。

    ![图像](./media/jamfprosamlconnector-tutorial//b2-domains_and_urls.png)

    c. 单击“设置其他 URL”。

    d. 在“登录 URL”文本框中，使用以下模式键入 URL：`https://<subdomain>.jamfcloud.com`。

    ![图像](./media/jamfprosamlconnector-tutorial//b4-domains_and_urls.png)

    > [!NOTE]
    > 这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 将从 Jamf Pro 门户中的“单一登录”部分获取实际的标识符值（本教程稍后会介绍）。 可以从标识符值提取实际的子域值，并在登录 URL 和答复 URL 中使用该子域信息。

6. 在“设置 SAML 单一登录”页的“SAML 签名证书”部分中，单击“复制”按钮，以复制“应用联合元数据 URL”，并将它保存在计算机上。

    ![图像](./media/jamfprosamlconnector-tutorial/C2_certificate.png)

7. 必须通过单击“安装扩展”来安装“我的应用安全登录浏览器扩展”，才能在 Jamf Pro 中自动执行配置。

    ![图像](./media/jamfprosamlconnector-tutorial/install_extension.png)
 
8. 将扩展添加到浏览器后，单击“安装 Jamf Pro”会定向到 Jamf Pro 应用程序。 随后，提供管理员凭据，以登录 Jamf Pro。 浏览器扩展会自动为你配置应用程序，并自动执行第 9-12 步。

    ![图像](./media/jamfprosamlconnector-tutorial/d1_saml.png)

9. 若要手动安装 Jamf Pro，请打开新的 Web 浏览器窗口，以管理员身份登录 Jamf Pro 公司网站，并按照以下步骤操作：

10. 单击页面右上角的“设置”图标。

    ![Jamf Pro 配置](./media/jamfprosamlconnector-tutorial/configure1.png)

11. 单击“单一登录”。

    ![Jamf Pro 配置](./media/jamfprosamlconnector-tutorial/configure2.png)

12. 在“单一登录”页上，执行以下步骤：

    ![Jamf Pro 单一登录](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 选择“Jamf Pro Server”以启用单一登录访问。

    b. 如果选择“允许所有用户绕过”，则用户不会重定向到用于身份验证的标识提供者登录页，但可以直接登录到 Jamf Pro。 当用户尝试通过标识提供者访问 Jamf Pro 时，会发生 IdP 发起的 SSO 身份验证和授权。

    c. 为“用户映射: SAML”选择“NameID”选项。 默认情况下，此设置指定为“NameID”，但可以定义自定义的属性。

    d. 为“用户映射: JAMF PRO”选择“电子邮件”。 Jamf Pro 按以下方式映射 IdP 发送的 SAML 属性：按用户和按组。 当用户尝试访问 Jamf Pro 时，Jamf Pro 默认会从标识提供者获取有关该用户的信息，并且其与 Jamf Pro 用户帐户相匹配。 如果传入的用户帐户在 Jamf Pro 中不存在，则会发生组名称匹配。

    e. 在“组属性名称”文本框中粘贴值 `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups`。

13. 在同一页上，一直向下滚动到“单一登录”部分下的“标识提供者”，并执行以下步骤：

    ![Jamf Pro 配置](./media/jamfprosamlconnector-tutorial/configure3.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 从“标识提供者”下拉菜单中选择选项“其他”。

    b. 在“其他提供程序”文本框中输入“Azure AD”。

    c. 选择“标识提供者元数据源”下拉菜单中的选项“元数据 URL”，并将你从 Azure 门户复制的“应用联合元数据 URL”值粘贴到下面的文本框中。

    d. 复制“实体 ID”值并将其粘贴到 Azure 门户上“Jamf Pro 域和 URL”部分的“标识符(实体 ID)”文本框中。

    >[!NOTE]
    > 此处已模糊化的值是子域部分。使用此值填写 Azure 门户上的“Jamf Pro 域和 URL”部分中的“登录 URL”和“回复 URL”。

    e. 单击“ **保存**”。

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户 

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”。

    ![图像](./media/jamfprosamlconnector-tutorial/d_users_and_groups.png)

2. 选择屏幕顶部的“新建用户”。

    ![图像](./media/jamfprosamlconnector-tutorial/d_adduser.png)

3. 在“用户属性”中，按照以下步骤操作。

    ![图像](./media/jamfprosamlconnector-tutorial/d_userproperties.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“名称”字段中，输入“BrittaSimon”。
  
    b. 在“用户名”字段中，键入“brittasimon@yourcompanydomain.extension”  
    例如： BrittaSimon@contoso.com

    c. 选择“属性”，选中“显示密码”复选框，再记下“密码”框中显示的值。

    d. 选择“创建”。

### <a name="create-a-jamf-pro-test-user"></a>创建一个 Jamf Pro 测试用户

若要使 Azure AD 用户能够登录 Jamf Pro，必须将其预配到 Jamf Pro 中。 就 Jamf Pro 来说，需要手动执行预配。

**若要预配用户帐户，请执行以下步骤：**

1. 以管理员身份登录到 Jamf Pro 公司站点。

2. 单击页面右上角的“设置”图标。

    ![添加员工](./media/jamfprosamlconnector-tutorial/configure1.png)

3. 单击“Jamf Pro 用户帐户和组”。

    ![添加员工](./media/jamfprosamlconnector-tutorial/user1.png)

4. 单击“新建” 。

    ![添加员工](./media/jamfprosamlconnector-tutorial/user2.png)

5. 选择“创建标准帐户”。

    ![添加员工](./media/jamfprosamlconnector-tutorial/user3.png)

6. 在“新建帐户”对话框执行以下步骤：

    ![添加员工](./media/jamfprosamlconnector-tutorial/user4.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“用户名”文本框中键入“BrittaSimon”的全名。

    b. 根据你的组织为“访问级别”、“特权设置”和“访问状态”选择合适的选项。

    c. 在“全名”文本框中，键入 Britta Simon 的全名。

    d. 在“电子邮件地址”文本框中，键入 Britta Simon 帐户的电子邮件地址。

    e. 在“密码”文本框中，键入用户的密码。

    f. 在“验证密码”文本框中，键入用户的密码。

    g. 单击“ **保存**”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Jamf Pro 的权限，支持其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”和“Jamf Pro”。

    ![图像](./media/jamfprosamlconnector-tutorial/d_all_applications.png)

2. 在应用程序列表中，选择“Jamf Pro”。

    ![图像](./media/jamfprosamlconnector-tutorial/d_all_proapplications.png)

3. 在左侧菜单中，选择“用户和组”。

    ![图像](./media/jamfprosamlconnector-tutorial/d_leftpaneusers.png)

4. 依次选择“添加”按钮和“添加分配”对话框中的“用户和组”。

    ![图像](./media/jamfprosamlconnector-tutorial/d_assign_user.png)

4. 在“用户和组”对话框中，选择“用户”列表中的“Britta Simon”，再单击屏幕底部的“选择”按钮。

5. 在“添加分配”对话框中，选择“分配”按钮。

### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击 Jamf Pro 磁贴时，应当会自动登录到 Jamf Pro 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)
