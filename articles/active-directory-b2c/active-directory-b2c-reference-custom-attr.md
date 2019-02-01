---
title: 在 Azure Active Directory B2C 中定义自定义属性 | Microsoft Docs
description: 在 Azure Active Directory B2C 中定义应用程序的自定义属性以收集有关客户的信息。
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 53d9aec5a689babb637d2eff6b36ea3b8bd8d97f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173932"
---
# <a name="define-custom-attributes-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中定义自定义属性

 每个面向客户的应用程序对需要收集的信息都有独特的要求。 Azure Active Directory (Azure AD) B2C 租户附带了一组存储在属性中的内置信息：名、姓、市、邮编。 在 Azure AD B2C 中，可以扩展存储在每个客户帐户上的属性集。 
 
 可以在 [Azure 门户](https://portal.azure.com/) 中创建自定义属性并将其用于注册用户流、注册或登录用户流或配置文件编辑用户流。 还可以使用 [Azure AD 图形 API](active-directory-b2c-devquickstarts-graph-dotnet.md) 读取和写入这些属性。 Azure AD B2C 中的自定义属性使用 [Azure AD 图形 API 目录架构扩展](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions)。

## <a name="create-a-custom-attribute"></a>创建自定义属性

1. 以 Azure AD B2C 租户的全局管理员身份登录 [Azure 门户](https://portal.azure.com/)。
2. 通过在 Azure 门户的右上角切换到包含 Azure AD B2C 租户的目录，确保你正在使用该目录。 选择订阅信息，然后选择“切换目录”。 

    ![切换到 Azure AD B2C 租户](./media/active-directory-b2c-reference-custom-attr/switch-directories.png)

    选择包含租户的目录。

    ![选择目录](./media/active-directory-b2c-reference-custom-attr/select-directory.png)

3. 选择 Azure 门户左上角的“所有服务”，搜索并选择 **Azure AD B2C**。
4. 选择“用户属性”，然后选择“添加”。
5. 提供自定义属性的“名称”（例如，“ShoeSize”）
6. 选择“数据类型”。 仅“String”、“Boolean”和“Int”可供选择。
7. （可选）输入“说明”以供参考。 
8. 单击“创建”。

现在自定义属性在“用户属性”列表中显示，以便在用户流中使用。 自定义属性只有在任何用户流中第一次使用时才创建，而不是将其添加到“用户属性”列表时创建。

## <a name="use-a-custom-attribute-in-your-user-flow"></a>在用户流中使用自定义属性

1. 在 Azure AD B2C 租户中，选择“用户流”。
2. 选择策略（例如，“B2C_1_SignupSignin”）将其打开。 
4. 选择“用户属性”，然后选择自定义属性（例如，“ShoeSize”）。 单击“ **保存**”。
5. 选择“应用程序声明”，然后选择自定义属性。 
6. 单击“ **保存**”。

可以使用用户流上的“运行用户流”功能来验证客户体验。 现在可以在注册期间收集的属性的列表中看到“ShoeSize”，并在发送回应用程序的令牌中看到它。

