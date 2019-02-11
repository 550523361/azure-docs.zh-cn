---
title: 教程：Azure Active Directory 与 Predictix Price Reporting 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Predictix Price Reporting 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 691d0c43-3aa1-4220-9e46-e7a88db234ad
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: f52827351fbb90805b1d00f08071740543da3d81
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190881"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-price-reporting"></a>教程：Azure Active Directory 与 Predictix Price Reporting 的集成

本教程介绍如何将 Predictix Price Reporting 与 Azure Active Directory (Azure AD) 集成。

将 Predictix Price Reporting 与 Azure AD 集成具有以下优势：

- 可在 Azure AD 中控制谁有权访问 Predictix Price Reporting。
- 可以让用户通过其 Azure AD 帐户自动登录到 Predictix Price Reporting（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Predictix Price Reporting 的集成，需备齐以下项目：

- Azure AD 订阅
- 启用了 Predictix Price Reporting 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Predictix Price Reporting
1. 配置和测试 Azure AD 单一登录

## <a name="adding-predictix-price-reporting-from-the-gallery"></a>从库中添加 Predictix Price Reporting
要通过配置将 Predictix Price Reporting 集成到 Azure AD 中，需从库将 Predictix Price Reporting 添加到托管式 SaaS 应用的列表中。

**若要从库添加 Predictix Price Reporting，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

1. 在搜索框中，键入“Predictix Price Reporting”，从结果面板中选择“Predictix Price Reporting”，然后单击“添加”按钮添加该应用程序。

    ![结果列表中的 Predictix Price Reporting](./media/predictixpricereporting-tutorial/tutorial_predictixpricereporting_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

本部分需根据名为“Britta Simon”的测试用户的情况，配置和测试 Predictix Price Reporting 的 Azure AD 单一登录。

若要使用单一登录，Azure AD 需要了解与 Azure AD 中的用户相对应的 Predictix Price Reporting 中的用户是谁。 换句话说，需要建立 Azure AD 用户与 Predictix Price Reporting 中相关用户之间的关联关系。

可通过将 Azure AD 中“用户名”的值指定为 Predictix Price Reporting 中“用户名”的值，建立此链接关系。

若要使用 Predictix Price Reporting 配置和测试 Azure AD 单一登录，需完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 Predictix Price Reporting 测试用户](#create-a-predictix-price-reporting-test-user)** - 在 Predictix Price Reporting 中创建 Britta Simon 的对应用户，并将其链接到用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 Predictix Price Reporting 应用程序中配置单一登录。

**若要通过 Predictix Price Reporting 配置 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的“Predictix Price Reporting”应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/predictixpricereporting-tutorial/tutorial_predictixpricereporting_samlbase.png)

1. 在“Predictix Price Reporting 域和 URL”部分中，执行以下步骤：

    ![Predictix Price Reporting 域和 URL 单一登录信息](./media/predictixpricereporting-tutorial/tutorial_predictixpricereporting_url.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL： `https://<companyname-pricing>.predictix.com/sso/request`

    b. 在“标识符”文本框中，使用以下模式键入 URL：
    
    | |
    |--|
    | `https://<companyname-pricing>.predictix.com` |
    | `https://<companyname-pricing>.dev.predictix.com` |

    > [!NOTE] 
    > 这些不是实际值。 必须使用实际登录 URL 和标识符更新这些值。 请联系 [Predictix Price Reporting 客户端支持团队](https://www.infor.com/company/customer-center/)获取这些值。 
 
1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![证书下载链接](./media/predictixpricereporting-tutorial/tutorial_predictixpricereporting_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/predictixpricereporting-tutorial/tutorial_general_400.png)

1. 在“Predictix Price Reporting 配置”部分，单击“配置 Predictix Price Reporting”打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”。

    ![Predictix Price Reporting 配置](./media/predictixpricereporting-tutorial/tutorial_predictixpricereporting_configure.png) 

1. 要在 Predictix Price Reporting 端配置单一登录，需要将下载的证书 (Base64)、注销 URL、SAML 实体 ID 和 SAML 单一登录服务 URL 发送给 [Predictix Price Reporting 支持团队](https://www.infor.com/company/customer-center/)。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/predictixpricereporting-tutorial/create_aaduser_01.png)

1. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/predictixpricereporting-tutorial/create_aaduser_02.png)

1. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/predictixpricereporting-tutorial/create_aaduser_03.png)

1. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/predictixpricereporting-tutorial/create_aaduser_04.png)

    a. 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。
 
### <a name="create-a-predictix-price-reporting-test-user"></a>创建 Predictix Price Reporting 测试用户

本部分需在 Predictix Price Reporting 中创建名为“Britta Simon”的用户。 与 [Predictix Price Reporting 支持团队](https://www.infor.com/company/customer-center/)合作，在 Predictix Price Reporting 平台中添加用户。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，将通过向 Britta Simon 授予 Predictix Price Reporting 的访问权限使之能够使用 Azure 单一登录。

![分配用户角色][200] 

**要将 Britta Simon 分配到 Predictix Price Reporting，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“Predictix Price Reporting”。

    ![应用程序列表中的 Predictix Price Reporting 链接](./media/predictixpricereporting-tutorial/tutorial_predictixpricereporting_app.png)  

1. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的“Predictix Price Reporting”磁贴时，用户就会自动登录到 Predictix Price Reporting 应用程序。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/predictixpricereporting-tutorial/tutorial_general_01.png
[2]: ./media/predictixpricereporting-tutorial/tutorial_general_02.png
[3]: ./media/predictixpricereporting-tutorial/tutorial_general_03.png
[4]: ./media/predictixpricereporting-tutorial/tutorial_general_04.png

[100]: ./media/predictixpricereporting-tutorial/tutorial_general_100.png

[200]: ./media/predictixpricereporting-tutorial/tutorial_general_200.png
[201]: ./media/predictixpricereporting-tutorial/tutorial_general_201.png
[202]: ./media/predictixpricereporting-tutorial/tutorial_general_202.png
[203]: ./media/predictixpricereporting-tutorial/tutorial_general_203.png

