---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 623f95eac029e808746d5d08fa088eba134592dd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "60459028"
---
单击“创建资源”按钮。 在“在市场中搜索”字段中，输入 `Azure Active Directory B2C`。

![添加突出显示的按钮，并在“在市场中搜索”字段中添加文本“Azure Active Directory B2C”](./media/active-directory-b2c-create-tenant/find-azure-ad-b2c.png)

在结果列表中，选择“Azure Active Directory B2C”。

![在结果列表中选择的 Azure Active Directory B2C](./media/active-directory-b2c-create-tenant/find-azure-ad-b2c-result.png)

所示为 Azure Active Directory B2C 的详细信息。 若要开始配置新的 Azure Active Directory B2C 租户，请单击“创建”按钮。

选择“创建新的 Azure AD B2C 租户”。 下表中指定的设置使用公司名称 Contoso 作为示例。 创建租户时，需提供自己的组织名称和唯一租户名称。  

![Azure AD B2C 使用可用字段中的示例文本创建租户](./media/active-directory-b2c-create-tenant/create-new-b2c-tenant.png)

| 设置      | 示例值  | 描述                                        |
| ------------ | ------- | -------------------------------------------------- |
| 组织名称 | Contoso | 组织的名称。 | 
| 初始域名 |  ContosoB2CTenant | B2C 租户的域名。 默认情况下，初始域名包括 .onmicrosoft.com。 如果创建的是测试租户，请选择非生产名称，例如 ContosoB2CTesting。 |
| 国家或地区 | 美国 | 选择目录所对应的国家或地区。 目录将在此位置创建，以后不能更改。  |

单击“创建”按钮以创建租户。 创建租户可能需要几分钟的时间。 完成后，系统会通过通知提醒你。