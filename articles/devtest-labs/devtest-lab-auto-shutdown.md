---
title: 在 Azure 开发测试实验室和计算 Vm 中管理自动关闭策略 |Microsoft Docs
description: 了解如何为实验室设置自动关闭策略，以便在虚拟机不使用时自动关闭虚拟机。
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: cd7974580ea30c9d0591c88380a4e626711bad1e
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93318976"
---
# <a name="configure-autoshutdown-for-lab-and-compute-virtual-machines-in-azure-devtest-labs"></a>为 Azure 开发测试实验室中的实验室和计算虚拟机配置自动关闭

本文介绍如何为开发测试实验室中的实验室 Vm 和计算 Vm 配置自动关闭设置。

## <a name="configure-autoshutdown-for-lab-vms-devtest-labs"></a>将自动关闭配置为实验室 Vm (开发测试实验室) 

借助 Azure 开发测试实验室，可通过管理每个实验室的策略（设置）来控制成本并尽量减少实验中的浪费。 本文介绍如何为实验室配置自动关闭策略。  它还演示如何为实验室 VM 配置自动关闭设置。 若要了解如何设置每个实验室策略，请参阅[在 Azure 开发测试实验室中定义实验室策略](devtest-lab-set-lab-policy.md)。  

### <a name="set-autoshutdown-policy-for-a-lab"></a>为实验室设置自动关闭策略

实验室所有者可针对实验室中的所有 VM 配置关机计划。 这样可以节省成本，因为未使用（空闲）的计算机不会运行。 可以集中在所有实验室 Vm 上强制实施关闭策略，还可以保存实验室用户为其各个计算机设置计划的工作量。 此功能可让你根据实验室计划设置策略，范围为允许实验室用户对其 VM 的关闭计划拥有完全控制权，使其无法控制其 VM 关闭。 实验室所有者可通过以下步骤配置此策略：

1. 在实验室的主页上，选择“配置和策略”。
2. 在左侧菜单的“计划”部分选择“自动关机策略”。
3. 选择一个选项。 以下各节介绍了有关这些选项的更多详细信息：

    ![自动关闭策略选项](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

> [!IMPORTANT]
> 对关闭策略的更改仅适用于在实验室中创建的新 Vm，而不适用于现有的虚拟机。

### <a name="configure-autoshutdown-settings"></a>配置自动关闭设置

自动关闭策略允许你指定此实验室的 Vm 的关闭时间，从而有助于最大程度地减少实验室浪费。

若要查看或更改实验室的策略，请执行以下步骤：

1. 在实验室的主页上，选择“配置和策略”。
2. 选择左侧菜单的 " **计划** " 部分中的 " **自动关闭** "。
3. 选择“开启”启用此策略，选择“关闭”禁用此策略。
     ![自动关闭详细信息](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
4. 如果启用此策略，请指定要关闭当前实验室中所有 VM 的时间（和时区）。
5. 指定 **"是"** 或 " **否** "，以在指定的自动关闭时间之前30分钟发送通知。 如果选择“是”，请输入 Webhook URL 终结点或电子邮件地址，指定要将通知发布或发送到的位置。 用户会收到通知，其中提供了延迟关闭的选项。 有关详细信息，请参阅 " [通知](#notifications) " 部分。
6. 选择“保存”。

    默认情况下，一旦启用，此策略会应用到当前实验室中所有 VM。 要从特定 VM 中删除此设置，请打开 VM 的管理窗格，并更改其“自动关闭”设置。

> [!NOTE]
> 如果在当前计划时间的30分钟内更新实验室或特定实验室虚拟机的自动关闭计划，则更新的关闭时间将适用于下一天的计划。

### <a name="user-sets-a-schedule-and-can-opt-out"></a>用户设置计划，并可选择禁用

如果在此策略中设置了你的实验室，则实验室用户可以替代或选择禁用实验室计划。 此选项授予实验室用户对其 VM 的自动关机计划的完全控制权。 实验室用户在其 VM 自动关机计划页中看不到任何变化。

![自动关闭策略选项-1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>用户设置计划，但无法选择禁用

如果在此策略中设置了你的实验室，则实验室用户可以替代实验室计划。 但是，他们不能选择退出自动关闭策略。 此选项可确保实验室中的每台计算机都处于自动关闭计划下。 实验室用户可更新其 Vm 的自动关闭计划，并设置关机通知。

![自动关闭策略选项-2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>用户无法控制实验室管理员设置的计划

如果在此策略中设置了你的实验室，则实验室用户无法替代或选择禁用实验室计划。 此选项可让实验室管理员全权控制实验室中每台计算机的计划。 实验室用户只能针对其 VM 设置自动关机通知。

![自动关闭策略选项-3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="configure-autoshutdown-for-compute-vms"></a>为计算 Vm 配置自动关闭

1. 在 " **虚拟机** " 页上，在 " **操作** " 部分的左侧菜单中选择 " **自动关闭** "。
2. 在 " **自动关闭** " 页上，选择 **"打开** " 以启用此策略，并选择 " **关闭** " 以禁用它。
3. 如果启用此策略，请指定应关闭 VM 的 **时间** **(和时区** ) 。
4. 选择 **"是"** 或 " **否** "，以在指定的自动关闭时间之前30分钟发送通知。 如果选择“是”，请输入 Webhook URL 终结点或电子邮件地址，指定要将通知发布或发送到的位置。 用户会收到通知，其中提供了延迟关闭的选项。 有关详细信息，请参阅 " [通知](#notifications) " 部分。
5. 选择“保存”。

    ![为计算 VM 配置自动关闭](./media/devtest-lab-auto-shutdown/comnpute-auto-shutdown.png)

### <a name="view-activity-logs-for-auto-shutdown-updates"></a>查看自动关闭更新的活动日志

当你更新自动关闭设置时，你将看到在 VM 的活动日志中记录的活动。

1. 在 [Azure 门户](https://portal.azure.com)中，导航到 VM 的主页。
2. 从左侧菜单中选择 " **活动日志** "。
3. 从筛选器中删除 **资源： mycomputevm** 。
4. 确认在活动日志中看到 " **添加或修改计划** " 操作。 如果看不到，请等待一段时间，并刷新活动日志。

    ![活动日志条目](./media/devtest-lab-auto-shutdown/activity-log-entry.png)
5. 选择 " **添加或修改计划** " 操作，在 " **摘要** " 页上查看以下信息：

    - 操作名称 (添加或修改计划) 
    - 更新自动关闭设置的日期和时间。
    - 更新设置的用户的电子邮件地址。

        ![活动日志条目摘要](./media/devtest-lab-auto-shutdown/activity-log-entry-summary.png)
6. 切换到 " **添加或修改计划** " 页中的 " **更改历史记录** " 选项卡，可以看到设置的更改历史记录。 在下面的示例中，关闭时间已从2020年4月10日下午7更改为下午6： 15:18:47 EST。 而且，在 15:25:09 EST 禁用了该设置。

    ![活动日志-更改历史记录](./media/devtest-lab-auto-shutdown/activity-log-entry-change-history.png)
7. 若要查看有关操作的更多详细信息，请在 " **添加或修改计划** " 页中切换到 " **JSON** " 选项卡。

## <a name="notifications"></a>通知

配置自动关闭后，如果任何 Vm 受影响，通知将在自动关闭触发之前30分钟发送到实验室用户。 使用此选项，实验室用户可以在关机前保存其工作。 此通知还为每个 VM 提供了以下操作的链接，以防有人需要在其 VM 上工作。

- 暂时跳过自动关闭
- 暂停自动关闭一小时
- 将自动关闭推迟2小时

如果指定了 webhook，通知将发送到 webhook url。  如果在自动关闭设置中指定了电子邮件地址，则会向该电子邮件地址发送一封电子邮件。 Webhook 允许生成或设置订阅特定事件的集成。 触发其中一个事件时，开发测试实验室会将 HTTP POST 有效负载发送到 webhook 的已配置 URL。 有关响应 webhook 的详细信息，请参阅 [AZURE FUNCTIONS http 触发器和绑定概述](../azure-functions/functions-bindings-http-webhook.md) 或 [添加适用于 Azure 逻辑应用的 http 触发器](../connectors/connectors-native-http.md#add-an-http-trigger)。

建议你使用 web 挂钩，因为 Azure 逻辑应用和时差等各种应用都广泛支持它们。  Webhook 允许你实现自己的发送通知的方式。 例如，本文逐步讲解如何配置自动关闭通知，以便使用 Azure 逻辑应用向 VM 所有者发送电子邮件。 首先，让我们快速完成在实验室中启用自动关闭通知的基本步骤。

### <a name="create-a-logic-app-that-receives-email-notifications"></a>创建一个逻辑应用，用于接收电子邮件通知

[Azure 逻辑应用](../logic-apps/logic-apps-overview.md) 提供了许多连接器，使你可以轻松地将服务与其他客户端（如 Office 365 和 Twitter）相集成。 在高级别上，为电子邮件通知设置逻辑应用的步骤可以分为四个阶段：

- 创建逻辑应用。
- 配置内置模板。
- 与你的电子邮件客户端集成
- 获取 Webhook URL。

### <a name="create-a-logic-app"></a>创建逻辑应用

若要开始，请使用以下步骤在 Azure 订阅中创建逻辑应用：

1. 在左侧菜单中选择 " **+ 创建资源** "，选择 " **集成** "，然后选择 " **逻辑应用** "。

    ![新建逻辑应用菜单](./media/devtest-lab-auto-shutdown/new-logic-app.png)
2. 在 " **逻辑应用-创建** " 页上，执行以下步骤：
    1. 输入逻辑应用的 **名称** 。
    2. 选择 **Azure 订阅** 。
    3. 创建新的资源组，或选择现有资源组。
    4. 选择逻辑应用的 **位置** 。

        ![新建逻辑应用-设置](./media/devtest-lab-auto-shutdown/new-logic-app-page.png)
3. 在 **通知** 中，选择 "前往通知上的 **资源** "。

    ![转到资源](./media/devtest-lab-auto-shutdown/go-to-resource.png)
4. 在 " **部署工具** " 类别下选择 **逻辑应用设计器** 。

    ![选择 HTTP 请求/响应](./media/devtest-lab-auto-shutdown/select-http-request-response-option.png)
5. 在 " **HTTP 请求-响应** " 页上，选择 " **使用此模板** "。

    ![选择 "使用此模板" 选项](./media/devtest-lab-auto-shutdown/select-use-this-template.png)
6. 将以下 JSON 复制到 **请求正文 JSON 架构** 部分：

    ```json
    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "properties": {
            "delayUrl120": {
                "type": "string"
            },
            "delayUrl60": {
                "type": "string"
            },
            "eventType": {
                "type": "string"
            },
            "guid": {
                "type": "string"
            },
            "labName": {
                "type": "string"
            },
            "owner": {
                "type": "string"
            },
            "resourceGroupName": {
                "type": "string"
            },
            "skipUrl": {
                "type": "string"
            },
            "subscriptionId": {
                "type": "string"
            },
            "text": {
                "type": "string"
            },
            "vmName": {
                "type": "string"
            },
            "vmUrl": {
                "type": "string"
            },
            "minutesUntilShutdown": {
                "type": "string"
            }
        },
        "required": [
            "skipUrl",
            "delayUrl60",
            "delayUrl120",
            "vmName",
            "guid",
            "owner",
            "eventType",
            "text",
            "subscriptionId",
            "resourceGroupName",
            "labName",
            "vmUrl",
            "minutesUntilShutdown"
        ],
        "type": "object"
    }
    ```

    ![显示 "请求正文 JSON 架构" 的屏幕截图。](./media/devtest-lab-auto-shutdown/request-json.png)
7. 在设计器中选择 " **+ 新步骤** "，然后执行以下步骤：
    1. 搜索 **Office 365 Outlook-发送电子邮件** 。
    2. 选择 "从 **操作****发送电子邮件** "。

        ![发送电子邮件选项](./media/devtest-lab-auto-shutdown/select-send-email.png)
    3. 选择 **"登录"** 以登录到你的电子邮件帐户。
    4. 选择 **"字段"，然后选择 "** 所有者"。
    5. 选择 " **使用者** "，并输入电子邮件通知的主题。 例如： "Shutdown machine vmName for Lab： labName"。
    6. 选择 " **正文** "，并定义电子邮件通知的正文内容。 例如： "vmName 计划在15分钟后关闭。 单击 "URL" 跳过此关闭操作。 延迟关机一小时： delayUrl60。 延迟关机2小时： delayUrl120。 "

        ![请求正文 JSON 架构](./media/devtest-lab-auto-shutdown/email-options.png)
8. 在工具栏上选择“保存”。 现在，你可以复制 **HTTP POST URL** 。 选择 "复制" 按钮，将 URL 复制到剪贴板。

    ![WebHook URL](./media/devtest-lab-auto-shutdown/webhook-url.png)

## <a name="next-steps"></a>后续步骤

若要了解如何设置所有策略，请参阅 [在 Azure 开发测试实验室中定义实验室策略](devtest-lab-set-lab-policy.md)。
