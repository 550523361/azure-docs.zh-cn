---
title: 管理 Azure 实验室服务中的实验室帐户 | Microsoft Docs
description: 了解如何在 Azure 订阅中创建实验室帐户、查看所有实验室帐户，或者删除实验室帐户。
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2018
ms.author: spelluru
ms.openlocfilehash: 20412efac553458f3028f873bcc6d918a673f261
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2018
ms.locfileid: "52838803"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>管理 Azure 实验室服务中的实验室帐户 
在 Azure 实验室服务中，实验室帐户是托管实验室（如教室实验室）的容器。 管理员可以设置一个具有 Azure 实验室服务的实验室帐户，并为能够在帐户中创建实验室的实验室所有者提供访问权限。 本文介绍如何创建实验室帐户、查看所有实验室帐户，或者删除实验室帐户。

## <a name="create-a-lab-account"></a>创建实验室帐户
1. 登录到 [Azure 门户](https://portal.azure.com)。
2. 在左侧的主菜单中，选择“创建资源”。
3. 在 Azure 市场中搜索“实验室服务”，然后从下拉列表中选择“实验室服务”。 
4. 在筛选出的服务列表中，选择“实验室服务(预览版)”。 
5. 在“创建实验室帐户”窗口，选择“创建”。
7. 在“实验室帐户”窗口，执行以下操作： 
    1. 在“实验室帐户名称”中输入名称。 
    2. 选择要在其中创建实验室帐户的“Azure 订阅”。
    3. 在“资源组”中选择“新建”，然后输入资源组的名称。
    4. 为“位置”选择要在其中创建实验室帐户的位置/区域。 
    5. 选择“创建”。 

        ![创建实验室帐户窗口](../media/how-to-manage-lab-accounts/lab-account-settings.png)
5. 如果没有看到实验室帐户页面，请选择“通知”按钮，然后单击通知中的“转到资源”按钮。 

    ![创建实验室帐户窗口](../media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. 会看到以下“实验室帐户”页面：

    ![“实验室帐户”页面](../media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>将用户添加为“实验室创建者”角色
若要在实验室帐户中设置课堂实验室，用户必须是实验室帐户中“实验室创建者”角色的成员。 用来创建实验室帐户的帐户会自动添加到此角色。 如果打算使用同一用户帐户创建课堂实验室，可以跳过此步骤。 若要使用其他用户帐户创建教室实验室，请执行以下步骤： 

1. 在“实验室帐户”页上，选择“访问控制(IAM)”，然后单击工具栏中的“+ 添加角色分配”。 
2. 在“添加权限”页，为“角色”选择“实验室创建者”，选择想要添加为实验室创建者角色的用户，然后选择“保存”。

## <a name="specify-marketplace-images-available-to-lab-owners"></a>指定可供实验室所有者使用的市场映像
作为实验室帐户所有者，你可以指定可供实验室创建者用来在实验室帐户中创建实验室的市场映像。 

1. 在左侧的菜单上选择“市场映像”。 默认情况下，可以看到映像的完整列表（包括启用的和禁用的）。 可以通过从顶部的下拉列表中选择“仅启用的/仅禁用的”选项对列表进行筛选来仅查看启用的/禁用的映像。 
    
    ![“市场映像”页](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    列表中显示的市场映像只是满足以下条件的映像：
        
    - 创建单个 VM。
    - 使用 Azure 资源管理器预配 VM
    - 不需要购买额外的许可计划
2. 若要**禁用**已启用的市场映像，请执行下列操作之一： 
    1. 选择最后一列中的“...”（省略号）并选择“禁用映像”。 

        ![禁用一个映像](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. 通过选中列表中映像名称前面的复选框从列表中选择一个或多个映像，然后选择“禁用所选映像”。 

        ![禁用多个映像](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. 类似地，若要**启用**市场映像，请执行下列操作之一： 
    1. 选择最后一列中的“...”（省略号）并选择“启用映像”。 
    2. 通过选中列表中映像名称前面的复选框从列表中选择一个或多个映像，然后选择“启用所选映像”。 

## <a name="view-lab-accounts"></a>查看实验室帐户
1. 登录到 [Azure 门户](https://portal.azure.com)。
2. 从菜单中选择“所有资源”。 
3. 选择“实验室服务”作为“类型”。 
    也可按订阅、资源组、位置和标记进行筛选。 

## <a name="delete-a-lab-account"></a>删除实验室帐户
按照上一节中的说明来显示列表中的实验室帐户。 根据以下说明删除实验室帐户： 

1. 选择要删除的**实验室帐户**。 
2. 从工具栏选择“删除”。 
3. 键入“是”进行确认。
4. 选择“删除”。 

## <a name="view-and-manage-labs-in-the-lab-account"></a>在实验室帐户中查看和管理实验室

1. 在“实验室帐户”页上，选择左侧菜单中的“实验室”。

    ![帐户中的实验室](../media/how-to-manage-lab-accounts/labs-in-account.png)
1. 你会看到帐户中的**实验室列表**，其中包含以下信息： 
    1. 实验室的名称。
    2. 创建实验室的日期。 
    3. 创建实验室的用户的电子邮件地址。 
    4. 允许加入实验室的最大用户数。 
    5. 实验室的状态。 

## <a name="delete-a-lab-in-the-lab-account"></a>删除实验室帐户中的实验室
按照上一部分中的说明进行操作，以查看实验室帐户中的实验列表。

1. 选择“...(省略号)”，然后选择“删除”。 

    ![删除实验室 - 按钮](../media/how-to-manage-lab-accounts/delete-lab-button.png)
2. 在警告消息上选择“是”。 



## <a name="next-steps"></a>后续步骤
请参阅以下文章：

- [以实验室所有者身份创建并管理实验室](how-to-manage-classroom-labs.md)
- [以实验室所有者身份设置并发布模板](how-to-create-manage-template.md)
- [以实验室所有者身份配置并控制实验室的使用](how-to-configure-student-usage.md)
- [以实验室用户身份访问教室实验室](how-to-use-classroom-lab.md)