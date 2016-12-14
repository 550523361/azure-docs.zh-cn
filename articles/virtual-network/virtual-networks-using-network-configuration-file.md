---
title: "使用网络配置文件配置虚拟网络"
description: "有关通过 Azure 管理门户导出和导入网络配置文件以创建或修改虚拟网络的说明。 "
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8a29f94cf0b6f8da70a5a18dde1dcbdca68bf284


---
# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>使用网络配置文件配置虚拟网络
可以使用 Azure 管理门户或使用网络配置文件配置虚拟网络 (VNet)。

## <a name="creating-and-modifying-a-network-configuration-file"></a>创建和修改网络配置文件
编写网络配置文件的最简单方法是将网络设置从现有虚拟网络配置中导出，然后修改该文件，使之包含需要为你的虚拟网络配置的设置。

若要编辑网络配置文件，只需直接打开该文件，对其进行相应的更改，然后保存该文件即可。 可以使用任何 *xml* 编辑器来更改网络配置文件。 

应严格遵循[网络配置文件架构设置](https://msdn.microsoft.com/library/azure/jj157100.aspx)指南。 

Azure 会将部署有项目的子网视为“**使用中**”。 当某个子网处于“使用中”状态时，不能对其进行修改。 在修改之前，请将已部署到子网的任何内容移动到不会进行修改的其他子网。   请参阅[将 VM 或角色实例移到其他子网](virtual-networks-move-vm-role-to-subnet.md)。

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>使用管理门户导出和导入虚拟网络设置
你可以使用 PowerShell 或管理门户导入和导出包含在网络配置文件中的网络配置设置。 下面的说明将帮助你使用管理门户进行导出和导入。 

### <a name="to-export-your-network-settings"></a>导出网络设置
导出时，订阅中的所有虚拟网络设置将写入一个 .xml 文件中。 

1. 登录到**管理门户**。
2. 在管理门户的“**网络**”页底部，单击“**导出**”。 
3. 在“**导出网络配置**”窗口中，验证是否选择了想要导出网络设置的订阅。 然后，单击右下角的复选标记。 
4. 出现提示时，将 *NetworkConfig.xml* 文件保存到所选位置。

### <a name="to-import-your-network-settings"></a>导入网络设置
1. 在“**管理门户**”左下的导航窗格中，单击“**新建**”。
2. 单击“**网络服务**” -> “**虚拟网络**” -> “**导入配置**”。
3. 在“**导入网络配置文件**”页上，浏览到你的网络配置文件，然后单击“**下一步**”箭头。
4. 在“**构建网络**”页上，会在屏幕上看到相关信息，显示将更改或创建网络配置的哪些部分。 如果所做的更改在你看来是正确的，请单击复选标记以继续更新或创建虚拟网络。 




<!--HONumber=Nov16_HO3-->


