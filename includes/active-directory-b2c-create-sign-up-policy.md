---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 511b05e6cae769a5b39ae81a3e67efd05d374511
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50132857"
---
如果仅希望在应用程序上启用注册，请使用**注册**策略。 此策略描述了客户在注册过程中将经历的体验以及应用程序在成功注册之后会接收到的令牌内容。

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

单击“注册策略”。

单击边栏选项卡顶部的“+ 添加”。

“名称”确定应用程序使用的注册策略名称。 例如，输入 **SiUp**。

单击“标识提供者”，并选择“电子邮件注册”。 或者，也可以选择社交标识提供者（如果已配置）。 单击“确定”。

单击“注册属性”。 在这里，可以选择要在注册期间从使用者收集的属性。 例如，选择“国家/地区”、“显示名称”和“邮政编码”。 单击“确定”。

单击“应用程序声明”。 在这里，可以选择希望在成功注册体验后发回到应用程序的令牌中返回的声明。 例如，选择“显示名称”、“标识提供者”、“邮政编码”、“用户是新用户”和“用户的对象 ID”。

单击“创建”。 已创建的策略在“注册策略”边栏选项卡中显示为“B2C_1_SiUp”（自动添加 **B2C\_1\_** 片段）。

通过单击“B2C_1_SiUp”打开策略。

在“应用程序”下拉列表中选择“Contoso B2C 应用”，并在“回复 URL/重定向 URI”下拉列表中选择 `https://localhost:44321/`。

单击“立即运行”。 将打开一个新的浏览器选项卡，可以运行注册应用程序的使用者体验。

> [!NOTE]
> 策略创建和更新最多需要一分钟才能生效。
>