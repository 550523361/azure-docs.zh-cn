---
title: 访问 Azure 实验室服务中的教室实验室 | Microsoft Docs
description: 在本教程中，会访问课堂实验室中由教授设置的虚拟机。
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 1328835714086dcec71b0e9dd4d1916794f557a6
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2019
ms.locfileid: "54390172"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>教程：访问 Azure 实验室服务中的课堂实验室
在本教程中，你会作为一名学生连接到教室实验室中的虚拟机 (VM)。 

在本教程中，将执行以下操作：

> [!div class="checklist"]
> * 使用注册链接 
> * 连接到虚拟机

## <a name="use-the-registration-link"></a>使用注册链接

1. 导航到从教授/教师处收到的注册 URL。 
2. 使用学校帐户登录服务以完成注册。 
3. 注册后，请确认可看到你有权访问的实验室的虚拟机。 
2. 等待虚拟机准备就绪，然后**启动** VM。 此过程需要一些时间。  

    ![启动 VM](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)

## <a name="connect-to-the-virtual-machine"></a>连接到虚拟机

1. 在要访问的实验室虚拟机的磁贴上，选择“连接”。 

    ![连接到 VM](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. 将 RDP 文件保存到硬盘，然后将其打开（假定 VM 为 Windows VM）
3. 使用从教师/教授处获取的用户名和密码登录到计算机。 

## <a name="next-steps"></a>后续步骤
本教程使用了从教师/教授处获取的注册链接来访问教室实验室。

作为实验室所有者，你希望查看已注册到实验室的用户并跟踪 VM 的使用情况。 转到下一教程，了解如何跟踪实验室的使用情况：

> [!div class="nextstepaction"]
> [跟踪实验室的使用情况](tutorial-track-usage.md) 