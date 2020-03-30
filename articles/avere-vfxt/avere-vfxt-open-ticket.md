---
title: 如何获取对 Avere vFXT for Azure 的支持
description: 创建有关 Avere vFXT for Azure 的支持票证的说明
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: b3c604003f78a150067875fb2064ab5eca0600db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252553"
---
# <a name="get-help-with-your-system"></a>获得有关系统的帮助

有关 Azure 系统的 Avere vFXT 的帮助，以下是获得支持的方法：

* **Avere vFXT 问题** - 使用 Azure 门户为 Avere vFXT 创建支持票证，[如下](#open-a-support-ticket-for-your-avere-vfxt)所述。
* **配额** - 如果遇到配额相关问题，可[请求增加配额 ](#request-a-quota-increase)
* **文档和示例** - 如果发现该文档或示例存在问题，请滚动到存在问题的页面底部，然后使用“反馈”部分搜索现有问题并提交新文档（如有需要）****。

## <a name="open-a-support-ticket-for-your-avere-vfxt"></a>为 Avere vFXT 创建支持票证

如果在部署或使用 Avere vFXT 时遇到问题，请通过 Azure 门户请求帮助。

按照以下步骤操作，确保已使用群集中的资源标记支持票证。 标记票证有助于我们将其路由到正确的支持资源。

1. 从[https://portal.azure.com](https://portal.azure.com)中选择**资源组**。 浏览到出现问题的 vFXT 群集的资源组，然后单击其中一个 Avere 群集虚拟机。

    ![圈出特定 VM 的 Azure 门户资源组“概述”面板的屏幕截图](media/avere-vfxt-ticket-vm.png)

1. 在 VM 页面中，向下滚动到左侧面板的底部，然后单击“新建支持请求”****。

    ![上一屏幕截图中 VM 的 Azure 门户 VM 页面的屏幕截图。 左侧菜单滚动到底部，"新支持请求"圈。](media/avere-vfxt-ticket-request.png)

1. 在支持请求的第一页上，选择问题类型并确保选择了正确的订阅。

   在 **"服务**"下，单击 **"所有服务**"，并在 **"存储"** 下查看以选择**Avere vFXT**。

   添加简短摘要，然后选择问题类型。

    ![Azure 门户中新支持请求屏幕的屏幕截图。 选择了"基本"选项卡。 屏幕项目包括问题类型、订阅、服务、摘要和问题类型。](media/ticket-basics.png)

   单击“下一步”继续。

1. 支持表单的第二页包含基于摘要描述修复问题的建议。 如果仍需要创建支持票证，请单击底部的 **"下一步**"按钮。

   ![选中"解决方案"选项卡的新支持请求屏幕的屏幕截图。 中间的文本字段标题为"建议的解决方案"，并解释了可能的补救措施。](media/ticket-solutions.png)

1. 在第三页中，提供详细信息 - 这包括有关群集、出现问题的时间、严重性以及如何与您联系的信息。 填写信息，然后单击底部的 **"下一步**"按钮。

   ![选中"详细信息"选项卡的新支持请求屏幕的屏幕截图。 信息字段分为"问题详细信息"、"支持方法"和"联系信息"类别。](media/ticket-details.png)

1. 查看最终页面上的信息，然后单击"**创建**"。 确认信息和票证号码将发送到你的电子邮件地址，支持人员成员将与你取得联系。

## <a name="request-a-quota-increase"></a>请求增加配额

请参阅 [vFXT 群集的配额](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)，了解部署 Avere vFXT for Azure 所需的组件。 可以从 Azure 门户[请求增加配额](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request)。
