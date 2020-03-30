---
title: 编码或解码平面文件
description: 使用 Azure 逻辑应用和 Enterprise Integration Pack 对企业集成的平面文件进行编码或解码
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 07/08/2016
ms.openlocfilehash: 2d6182ba01507c2fb361628e01bb52e1ea821f44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152646"
---
# <a name="encode-or-decode-flat-files-with-azure-logic-apps-and-enterprise-integration-pack"></a>使用 Azure 逻辑应用和 Enterprise Integration Pack 对平面文件进行编码或解码

在企业到企业 (B2B) 方案中将 XML 内容发送给业务合作伙伴之前，可能要对该内容进行编码。 在逻辑应用中，可以使用平面文件编码连接器进行此操作。 创建的逻辑应用可以从各种源获取其 XML 内容，包括从 HTTP 请求触发器、从其他应用程序、甚至是从许多[连接器](../connectors/apis-list.md)中的一个。 有关逻辑应用的详细信息，请参阅[逻辑应用文档](logic-apps-overview.md "了解有关逻辑应用的更多信息")。  

## <a name="create-the-flat-file-encoding-connector"></a>创建平面文件编码连接器
按照以下步骤可将平面文件编码连接器添加到逻辑应用。

1. 创建逻辑应用并将其[链接到您的集成帐户](logic-apps-enterprise-integration-accounts.md "了解如何将集成帐户链接到逻辑应用")。 此帐户包含用于对 XML 数据进行编码的架构。  

1. 在逻辑应用设计器中，将**何时收到 HTTP 请求**触发器添加到逻辑应用。

1. 添加平面文件编码操作，如下所示：

   a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 选择**加号**。

   b.保留“数据库类型”设置，即设置为“共享”。 选择“添加操作”**** 链接（在选择了加号之后出现）。

   c. 在搜索框中输入“平面”** 一词，以便在所有操作中筛选出要使用的操作。

   d.单击“下一步”。 从列表中选择“平面文件编码”**** 选项。   

      ![“平面文件编码”选项的屏幕截图](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   

1. 在“平面文件编码”**** 对话框中，选择“内容”**** 文本框。  

   ![“内容”文本框的屏幕截图](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)  

1. 选择正文标记作为要编码的内容。 正文标记回填充内容字段。     

   ![正文标记的屏幕截图](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  

1. 选择“架构名称”**** 列表框，并选择要用于对输入内容进行编码的架构。    

   ![“架构名称”列表框的屏幕截图](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)  

1. 保存工作。

   ![“保存”图标的屏幕截图](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

此时，已完成平面文件编码连接器设置。 在实际应用程序中，可能要将编码数据存储在业务线应用程序中，如 Salesforce。 或者，可以将该编码数据发送给贸易合作伙伴。 可以使用提供的其他连接器中的任何一个，轻松地添加操作以将编码操作的输出发送给 Salesforce 或贸易合作伙伴。

现在可以通过向 HTTP 终结点发出请求并在请求正文中包含 XML 内容，来测试连接器。  

## <a name="create-the-flat-file-decoding-connector"></a>创建平面文件解码连接器

> [!NOTE]
> 要完成这些步骤，需要已将架构文件上传到集成帐户。

1. 在逻辑应用设计器中，将**何时收到 HTTP 请求**触发器添加到逻辑应用。

1. 添加平面文件解码操作，如下所示：

   a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 选择**加号**。

   b.保留“数据库类型”设置，即设置为“共享”。 选择“添加操作”**** 链接（在选择了加号之后出现）。

   c. 在搜索框中输入“平面”** 一词，以便在所有操作中筛选出要使用的操作。

   d.单击“下一步”。 从列表中选择“平面文件解码”**** 选项。   

      ![“平面文件解码”选项的屏幕截图](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   

1. 选择“内容”**** 控件。 这会生成来自前面步骤的内容的列表，这些内容可以用作要解码的内容。 请注意，传入 HTTP 请求的正文** 可供用作要解码的内容。 还可以直接在“内容”**** 控件中输入要解码的内容。     

1. 选择正文** 标记。 请注意，正文标记现在处于“内容”**** 控件中。

1. 选择要用于对内容进行解码的架构的名称。 以下屏幕截图显示 OrderFile** 是所选架构名称。 此架构名称以前已上传到集成帐户中。

   ![“平面文件解码”对话框的屏幕截图](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png) 

1. 保存工作。  

   ![“保存”图标的屏幕截图](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

此时，已完成平面文件解码连接器设置。 在实际应用程序中，可能要将解码数据存储在业务线应用程序中，如 Salesforce。 可以轻松地添加操作以将解码操作的输出发送给 Salesforce。

现在可以通过向 HTTP 终结点发出请求并在请求正文中包含要解码的 XML 内容，来测试连接器。  

## <a name="next-steps"></a>后续步骤
* [详细了解企业集成包](logic-apps-enterprise-integration-overview.md "了解企业集成包")。  

