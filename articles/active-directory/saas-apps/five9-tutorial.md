---
title: 教程：将 Azure Active Directory 与 Five9 Plus Adapter (CTI, Contact Center Agents) 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 Five9 Plus Adapter (CTI, Contact Center Agents) 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 124bc858e9950962d3ae0f69db8fb962a3725f87
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2019
ms.locfileid: "56206508"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>教程：将 Azure Active Directory 与 Five9 Plus Adapter (CTI, Contact Center Agents) 集成

本教程介绍如何将 Five9 Plus Adapter (CTI, Contact Center Agents) 与 Azure Active Directory (Azure AD) 集成。

将 Five9 Plus Adapter (CTI, Contact Center Agents) 与 Azure AD 集成具有以下好处：

- 可以在 Azure AD 中控制谁有权访问 Five9 Plus Adapter (CTI, Contact Center Agents)
- 可以让用户使用其 Azure AD 帐户自动登录到 Five9 Plus Adapter (CTI, Contact Center Agents)（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Five9 Plus Adapter (CTI, Contact Center Agents) 的集成，需要以下项：

- Azure AD 订阅
- 已启用 Five9 Plus Adapter (CTI, Contact Center Agents) 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在此处获取一个月的试用版：[试用版产品/服务](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库添加 Five9 Plus Adapter (CTI, Contact Center Agents)
1. 配置和测试 Azure AD 单一登录

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>从库添加 Five9 Plus Adapter (CTI, Contact Center Agents)
若要配置 Five9 Plus Adapter (CTI, Contact Center Agents) 与 Azure AD 的集成，请从库将 Five9 Plus Adapter (CTI, Contact Center Agents) 添加到托管 SaaS 应用列表中。

若要从库添加 Five9 Plus Adapter (CTI, Contact Center Agents)，请执行以下步骤：

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“Five9 Plus Adapter (CTI, Contact Center Agents)”。

    ![创建 Azure AD 测试用户](./media/five9-tutorial/tutorial_five9_search.png)

1. 在结果面板中，选择“Five9 Plus Adapter (CTI, Contact Center Agents)”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，基于一个名为“Britta Simon”的测试用户使用 Five9 Plus Adapter (CTI, Contact Center Agents) 配置和测试 Azure AD 单一登录。

为使单一登录能正常工作，Azure AD 需要知道 Five9 Plus Adapter (CTI, Contact Center Agents) 中与 Azure AD 中的用户相对应的用户。 换句话说，需要在 Azure AD 用户与 Five9 Plus Adapter (CTI, Contact Center Agents) 中相关用户之间建立链接关系。

在 Five9 Plus Adapter (CTI, Contact Center Agents) 中，将 Azure AD 中“用户名”的值指定为“用户名”的值来建立此链接关系。

若要配置和测试 Five9 Plus Adapter (CTI, Contact Center Agents) 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 Five9 Plus Adapter (CTI, Contact Center Agents) 测试用户](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)** - 在 Five9 Plus Adapter (CTI, Contact Center Agents) 创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录，并在 Five9 Plus Adapter (CTI, Contact Center Agents) 应用程序中配置单一登录。

**若要使用 Five9 Plus Adapter (CTI, Contact Center Agents) 配置 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户的“Five9 Plus Adapter (CTI, Contact Center Agents)”应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/five9-tutorial/tutorial_five9_samlbase.png)

1. 在“Five9 Plus Adapter (CTI, Contact Center Agents) 域和 URL”部分，执行以下步骤：

    ![配置单一登录](./media/five9-tutorial/tutorial_five9_url.png)
    
    a. 在“标识符”文本框中，使用以下模式键入 URL：

    |    环境      |       代码      |
    | :-- | :-- |
    | 适用于 Five9 Plus Adapter for Microsoft Dynamics CRM | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | 适用于 Five9 Plus Adapter for Zendesk | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | 适用于 Five9 Plus Adapter for Agent Desktop Toolkit | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. 在“回复 URL”文本框中，使用以下模式键入 URL：

    |      环境     |      代码      |
    | :--                  | :--           |
    | 适用于 Five9 Plus Adapter for Microsoft Dynamics CRM | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | 适用于 Five9 Plus Adapter for Zendesk | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | 适用于 Five9 Plus Adapter for Agent Desktop Toolkit | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![配置单一登录](./media/five9-tutorial/tutorial_five9_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录](./media/five9-tutorial/tutorial_general_400.png)

1. 在“Five9 Plus Adapter (CTI, Contact Center Agents) 配置”部分，单击“配置 Five9 Plus Adapter (CTI, Contact Center Agents)”，打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”。

    ![配置单一登录](./media/five9-tutorial/tutorial_five9_configure.png) 

1. 若要在“Five9 Plus Adapter (CTI, Contact Center Agents)”端配置单一登录，需要将下载的“证书 (Base64)”、“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”发送到 [Five9 Plus Adapter (CTI, Contact Center Agents) 支持团队](https://www.five9.com/about/contact)。 此外，若要进一步配置 SSO，请根据适配器遵循以下步骤：

    a. “Five9 Plus Adapter for Agent Desktop Toolkit”管理员指南：[https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. “Five9 Plus Adapter for Microsoft Dynamics CRM”管理员指南：[https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. “Five9 Plus Adapter for Zendesk”管理员指南：[https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/five9-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/five9-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/five9-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/five9-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>创建 Five9 Plus Adapter (CTI, Contact Center Agents) 测试用户

本部分在 Five9 Plus Adapter (CTI, Contact Center Agents) 中创建一个名为 Britta Simon 的用户。 通过 [Five9 Plus Adapter (CTI, Contact Center Agents) 支持团队](https://www.five9.com/about/contact)在 Five9 Plus Adapter (CTI, Contact Center Agents) 平台添加用户。 使用单一登录前，必须先创建并激活用户。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Five9 Plus Adapter (CTI, Contact Center Agents) 的权限，允许她使用 Azure 单一登录。

![分配用户][200] 

若要将 Britta Simon 分配到 Five9 Plus Adapter (CTI, Contact Center Agents)，请执行以下步骤：

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“Five9 Plus Adapter (CTI, Contact Center Agents)”。

    ![配置单一登录](./media/five9-tutorial/tutorial_five9_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在“访问面板”中单击“Five9 Plus Adapter (CTI, Contact Center Agents)”磁贴时，应自动登录到 Five9 Plus Adapter (CTI, Contact Center Agents) 应用程序。
有关访问面板的详细信息，请参阅[访问面板简介](../user-help/active-directory-saas-access-panel-introduction.md)。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/five9-tutorial/tutorial_general_01.png
[2]: ./media/five9-tutorial/tutorial_general_02.png
[3]: ./media/five9-tutorial/tutorial_general_03.png
[4]: ./media/five9-tutorial/tutorial_general_04.png

[100]: ./media/five9-tutorial/tutorial_general_100.png

[200]: ./media/five9-tutorial/tutorial_general_200.png
[201]: ./media/five9-tutorial/tutorial_general_201.png
[202]: ./media/five9-tutorial/tutorial_general_202.png
[203]: ./media/five9-tutorial/tutorial_general_203.png

