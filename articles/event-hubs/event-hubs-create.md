---
title: 创建 Azure 事件中心 | Microsoft Docs
description: 使用 Azure 门户创建 Azure 事件中心命名空间和事件中心
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 5b468e1758d752cd3001c85b328d064369429499
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/17/2018
ms.locfileid: "42141895"
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>使用 Azure 门户创建事件中心命名空间和事件中心

## <a name="create-an-event-hubs-namespace"></a>创建事件中心命名空间

1. 登录到 [Azure 门户][Azure portal]，单击屏幕左上角的“创建资源”。
2. 依次单击“物联网”、“事件中心”。
   
    ![](./media/event-hubs-create/create-event-hub9.png)

3. 在“创建命名空间”中输入命名空间名称。 系统会立即检查该名称是否可用。  

4. 在确保命名空间名称可用后，选择定价层（基础版或标准版）。 另外，请选择一个 Azure 订阅、资源组以及要创建该资源的位置。
 
5. 单击“创建”以创建命名空间。 可能需要等待几分钟让系统完全预配资源。

    ![](./media/event-hubs-create/create-event-hub1.png)

6. 在命名空间的门户列表中，单击新创建的命名空间。

7. 依次“共享访问策略”、“RootManageSharedAccessKey”。
    
    ![](./media/event-hubs-create/create-event-hub7.png)

8. 单击复制按钮，将 **RootManageSharedAccessKey** 连接字符串复制到剪贴板。 将此连接字符串保存在临时位置（如记事本），供稍后使用。
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>创建事件中心

1. 在“事件中心”命名空间列表中，单击新创建的命名空间。      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. 在命名空间边栏选项卡中，单击“事件中心” 。
   
    ![](./media/event-hubs-create/create-event-hub3.png)

3. 在边栏选项卡顶部，单击“+ 事件中心”。
   
    ![](./media/event-hubs-create/create-event-hub4.png)
4. 为事件中心键入名称，并单击“创建”。 

现在已创建事件中心，已具有收发事件所需的连接字符串。

## <a name="next-steps"></a>后续步骤

若要了解有关事件中心的详细信息，请访问以下链接：

* [事件中心概述](event-hubs-what-is-event-hubs.md)
* [事件中心 API 概述](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/