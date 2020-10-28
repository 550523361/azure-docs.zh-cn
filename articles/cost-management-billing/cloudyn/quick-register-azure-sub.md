---
title: 在 Cloudyn 中注册 Azure 订阅
description: 了解如何在 Cloudyn 中注册 Azure 订阅并登录到 Cloudyn 门户。
author: bandersmsft
ms.author: banders
ms.date: 03/12/2020
ms.topic: quickstart
ms.custom: seodec18
ms.service: cost-management-billing
ms.subservice: cloudyn
ms.reviewer: benshy
ROBOTS: NOINDEX
ms.openlocfilehash: bb7add199186ab54eb28131070e717e4298bedf1
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2020
ms.locfileid: "92526403"
---
# <a name="register-an-individual-azure-subscription-and-view-cost-data"></a>注册一个单独的 Azure 订阅并查看成本数据

使用 Azure 订阅在 Cloudyn 中注册。 注册后可以访问 Cloudyn 门户。 本快速入门将详细介绍创建 Cloudyn 试用订阅和登录到 Cloudyn 门户所需的注册过程。 此外，还将演示如何立即开始查看成本数据。

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]

## <a name="sign-in-to-azure"></a>登录 Azure

- 在 [https://portal.azure.com](https://portal.azure.com) 中登录 Azure 门户。

## <a name="register-with-cloudyn"></a>注册 Cloudyn

1. 在 Azure 门户中，单击服务列表中的“成本管理 + 计费”  。
2. 在“概览”  下，单击“Cloudyn”   
    ![显示在 Azure 门户中的 Cloudyn 页](./media/quick-register-azure-sub/cost-mgt-billing-service.png)
3. 在“成本管理”  页上，单击“转到 Cloudyn”  ，以在新窗口中打开 Cloudyn 注册页。
4. 在 Cloudyn 门户试用注册页上，键入公司名称，然后选择“Azure 个人订阅所有者”  ，然后单击“下一步”  。 你的帐户名称和租户 ID 被自动添加到窗体。  
    ![试用注册页，可在此输入注册信息](./media/quick-register-azure-sub/trial-reg-ind.png)
5. 选择与你的订阅相关联的“套餐 ID - 名称”  。 如果不确定你的订阅的费率 ID，可以查看 Azure 账单或查找“套餐 ID”  。
6. 同意“使用条款”，并验证相关信息，然后单击“下一步”  。
7. 在“获取其他数据”  页，单击“下一步”  ，以授权 Cloudyn 收集 Azure 资源数据。 收集的数据包括订阅的使用情况、性能、计费和标记数据。  
    ![收集在其中授权 Cloudyn 的其他数据页](./media/quick-register-azure-sub/gather-additional.png)
8. 你的浏览器会将你转到 Cloudyn 的登录页。 使用 Azure 订阅凭据登录。
9. 单击“转到 Cloudyn”  ，以打开 Cloudyn 门户，然后在“帐户管理”  页，应该可以看到你的 Azure 订阅帐户信息。  
    ![显示 Azure 订阅信息的帐户管理页](./media/quick-register-azure-sub/accounts-mgt.png)

若要观看有关如何注册 Azure 订阅的教程视频，请参阅[在 Cloudyn 中查找要使用的目录 GUID 和费率 ID](https://youtu.be/PaRjnyaNGMI)。

[!INCLUDE [cost-management-create-account-view-data](../../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>后续步骤

在快速入门中，使用你的 Azure 订阅信息注册 Cloudyn。 此外，还会登录到 Cloudyn 门户并开始查看成本数据。 若要了解有关 Cloudyn 的详细信息，请继续学习 Cloudyn 的教程。

> [!div class="nextstepaction"]
> [查看使用情况和成本](tutorial-review-usage.md)
