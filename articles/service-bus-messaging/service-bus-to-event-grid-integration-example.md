---
title: 教程：Azure 服务总线到事件网格的集成示例
description: 教程：本文提供的示例涉及服务总线消息传送和事件网格集成。
documentationcenter: .net
author: spelluru
ms.topic: tutorial
ms.date: 06/23/2020
ms.author: spelluru
ms.openlocfilehash: 8f947489c2298e580ae455763709df1734687130
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85337065"
---
# <a name="tutorial-respond-to-azure-service-bus-events-received-via-azure-event-grid-by-using-azure-functions-and-azure-logic-apps"></a>教程：使用 Azure Functions 和 Azure 逻辑应用对通过 Azure 事件网格收到的 Azure 服务总线事件做出响应
本教程介绍如何使用 Azure Functions 和 Azure 逻辑应用对通过 Azure 事件网格收到的 Azure 服务总线事件做出响应。 

在本教程中，你将了解如何执行以下操作：
> [!div class="checklist"]
> * 创建服务总线命名空间
> * 准备用于发送消息的示例应用程序
> * 向服务总线主题发送消息
> * 使用逻辑应用接收消息
> * 在 Azure 上设置测试函数
> * 通过事件网格连接函数和命名空间
> * 使用 Azure Functions 接收消息

## <a name="prerequisites"></a>先决条件

若要完成本教程，请确保已安装：

- [Visual Studio 2017 Update 3（版本 15.3 (26730.01)）](https://www.visualstudio.com/vs)或更高版本。
- [NET Core SDK](https://www.microsoft.com/net/download/windows) 2.0 或更高版本。

## <a name="create-a-service-bus-namespace"></a>创建服务总线命名空间
请遵照以下教程中的说明：[快速入门：使用 Azure 门户创建服务总线主题和主题的订阅](service-bus-quickstart-topics-subscriptions-portal.md)来执行以下任务：

- 创建一个**高级**服务总线命名空间。 
- 获取连接字符串。 
- 创建服务总线主题。
- 创建主题的两个订阅。 

## <a name="prepare-a-sample-application-to-send-messages"></a>准备用于发送消息的示例应用程序
可以使用任何方法向服务总线主题发送消息。 此过程末尾的示例代码假设使用的是 Visual Studio 2017。

1. 克隆 [GitHub azure-service-bus 存储库](https://github.com/Azure/azure-service-bus/)。
2. 在 Visual Studio 中转到 *\samples\DotNet\Microsoft.ServiceBus.Messaging\ServiceBusEventGridIntegration* 文件夹，然后打开 *SBEventGridIntegration.sln* 文件。
3. 转到 **MessageSender** 项目，然后选择 **Program.cs**。
4. 填写服务总线主题名称，以及在上一步骤中获取的连接字符串：

    ```csharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    const string TopicName = "YOUR TOPIC NAME";
    ```
5. 将 `numberOfMessages` 值更新为 5。 
5. 生成并运行程序，以将测试消息发送到服务总线主题。 

## <a name="receive-messages-by-using-logic-apps"></a>使用逻辑应用接收消息
执行以下步骤，将逻辑应用与 Azure 服务总线和 Azure 事件网格连接到一起：

1. 在 Azure 门户中创建逻辑应用。
    1. 依次选择“+ 创建资源”、“集成”、“逻辑应用”。   
    2. 在“逻辑应用 - 创建”页上，输入逻辑应用的**名称**。
    3. 选择 **Azure 订阅**。 
    4. 为“资源组”选择“使用现有项”，然后选择以前创建的、用于其他资源（例如 Azure 函数、服务总线命名空间）的资源组。  
    5. 选择逻辑应用的**位置**。 
    6. 选择“创建”以创建逻辑应用。 
2. 在“逻辑应用设计器”页上，选择“模板”下的“空白逻辑应用”。   
3. 在设计器中执行以下步骤：
    1. 搜索“事件网格”。 
    2. 选择“发生资源事件时 - Azure 事件网格”。 

        ![逻辑应用设计器 - 选择事件网格触发器](./media/service-bus-to-event-grid-integration-example/logic-apps-event-grid-trigger.png)
4. 选择“登录”，输入 Azure 凭据，然后选择“允许访问”。  
5. 在“当资源事件发生时”页上执行以下步骤：
    1. 选择 Azure 订阅。 
    2. 对于“资源类型”，请选择“Microsoft.ServiceBus.Namespaces”。  
    3. 对于“资源名称”，请选择你的服务总线命名空间。 
    4. 选择“添加新参数”，然后选择“后缀筛选器”。  
    5. 对于“后缀筛选器”，请输入第二个服务总线主题订阅的名称。 
        ![逻辑应用设计器 - 配置事件](./media/service-bus-to-event-grid-integration-example/logic-app-configure-event.png)
6. 在设计器中选择“+ 新建步骤”，然后执行以下步骤：
    1. 搜索“服务总线”。
    2. 在列表中选择“服务总线”。 
    3. 在“操作”列表中选择“获取消息”。  
    4. 选择“从主题订阅中获取消息(扫视锁定)”。 

        ![逻辑应用设计器 - 获取消息操作](./media/service-bus-to-event-grid-integration-example/service-bus-get-messages-step.png)
    5. 输入**连接的名称**。 例如：**从主题订阅中获取消息**，并选择服务总线命名空间。 

        ![逻辑应用设计器 - 选择服务总线命名空间](./media/service-bus-to-event-grid-integration-example/logic-apps-select-namespace.png) 
    6. 选择“RootManageSharedAccessKey”，然后选择“创建” 。

        ![逻辑应用设计器 - 选择共享访问密钥](./media/service-bus-to-event-grid-integration-example/logic-app-shared-access-key.png) 
    8. 选择你的主题和订阅 。 
    
        ![逻辑应用设计器 - 选择你的服务总线主题和订阅](./media/service-bus-to-event-grid-integration-example/logic-app-select-topic-subscription.png)
7. 选择“+ 新建步骤”，然后执行以下步骤： 
    1. 选择“服务总线”。
    2. 在操作列表中选择“完成主题订阅中的消息”。 
    3. 选择你的服务总线**主题**。
    4. 选择主题的第二个**订阅**。
    5. 对于“消息的锁定标记”，请从“动态内容”中选择“锁定标记”。   

        ![逻辑应用设计器 - 选择你的服务总线主题和订阅](./media/service-bus-to-event-grid-integration-example/logic-app-complete-message.png)
8. 在逻辑应用设计器的工具栏上选择“保存”以保存逻辑应用。 
9. 遵照[向服务总线主题发送消息](#send-messages-to-the-service-bus-topic)部分的说明向主题发送消息。 
10. 切换到逻辑应用的“概述”页。 “运行历史记录”中会显示已发送的消息的逻辑应用运行。

    ![逻辑应用设计器 - 逻辑应用运行](./media/service-bus-to-event-grid-integration-example/logic-app-runs.png)

## <a name="set-up-a-test-function-on-azure"></a>在 Azure 上设置测试函数 
在处理整个方案之前，请至少设置一个小型测试函数，用于调试和观察流动的事件。 遵照[在 Azure 门户中创建第一个函数](../azure-functions/functions-create-first-azure-function.md)一文中的说明执行以下任务： 

1. 创建函数应用。
2. 创建 HTTP 触发的函数。 

然后执行以下步骤： 


# <a name="azure-functions-v2"></a>[Azure Functions V2](#tab/v2)

1. 在树视图中展开“函数”，并选择你的函数。 将函数代码替换为以下代码： 

    ```csharp
    #r "Newtonsoft.Json"
    
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Primitives;
    using Newtonsoft.Json;
    
    public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");
        var content = req.Body;
        string jsonContent = await new StreamReader(content).ReadToEndAsync();
        log.LogInformation($"Received Event with payload: {jsonContent}");
    
        IEnumerable<string> headerValues;
        headerValues = req.Headers.GetCommaSeparatedValues("Aeg-Event-Type");
    
        if (headerValues.Count() != 0)
        {
            var validationHeaderValue = headerValues.FirstOrDefault();
            if(validationHeaderValue == "SubscriptionValidation")
            {
                log.LogInformation("Validating the subscription");            
                var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
                var code = events[0].Data["validationCode"];
                log.LogInformation($"Validation code: {code}");
                return (ActionResult) new OkObjectResult(new { validationResponse = code });
            }
        }
    
        return jsonContent == null
            ? new BadRequestObjectResult("Please pass a name on the query string or in the request body")
            : (ActionResult)new OkObjectResult($"Hello, {jsonContent}");
    }
    
    public class GridEvent
    {
        public string Id { get; set; }
        public string EventType { get; set; }
        public string Subject { get; set; }
        public DateTime EventTime { get; set; }
        public Dictionary<string, string> Data { get; set; }
        public string Topic { get; set; }
    }    
    ```
2. 选择工具栏上的“保存”以保存函数的代码。

    ![保存函数代码](./media/service-bus-to-event-grid-integration-example/save-function-code.png)
3. 在工具栏上选择“测试/运行”，然后执行以下步骤： 
    1. 在正文中输入以下 JSON。

        ```json
        [{
          "id": "64ba80ae-9f8e-425f-8bd7-d88d2c0ba3e3",
          "topic": "/subscriptions/0000000000-0000-0000-0000-0000000000000/resourceGroups/spegridsbusrg/providers/Microsoft.ServiceBus/namespaces/spegridsbusns",
          "subject": "",
          "data": {
            "validationCode": "D7D825D4-BD04-4F73-BDE3-70666B149857",
            "validationUrl": "https://rp-eastus.eventgrid.azure.net:553/eventsubscriptions/spsbusegridsubscription/validate?id=D7D825D4-BD04-4F73-BDE3-70666B149857&t=2020-06-09T18:28:51.5724615Z&apiVersion=2020-04-01-preview&[Hidden Credential]"
          },
          "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
          "eventTime": "2020-06-09T18:28:51.5724615Z",
          "metadataVersion": "1",
          "dataVersion": "2"
        }]
        ```    
    2. 单击“添加标头”，然后添加名称为 `aeg-event-type` 且值为 `SubscriptionValidation` 的标头。 
    3. 选择“运行”。 

        ![测试运行](./media/service-bus-to-event-grid-integration-example/test-run-function.png)
    4. 请确认在响应正文中看到返回状态代码“OK”以及验证代码。 另请参阅函数记录的信息。 

        ![测试运行 - 响应](./media/service-bus-to-event-grid-integration-example/test-function-response.png)        
3. 选择“获取函数 URL”并记下 URL。 

    ![获取函数 URL](./media/service-bus-to-event-grid-integration-example/get-function-url.png)
5. 选择 URL 文本旁边的“复制”按钮。    
    ![复制函数 URL](./media/service-bus-to-event-grid-integration-example/get-function-url-copy.png)

# <a name="azure-functions-v1"></a>[Azure Functions V1](#tab/v1)

1. 将函数配置为使用 **V1** 版本： 
    1. 在树视图中选择你的函数应用，然后选择“函数应用设置”。 
    2. 为“运行时版本”选择“~1”。  
2. 在树视图中展开“函数”，并选择你的函数。 将函数代码替换为以下代码： 

    ```csharp
    #r "Newtonsoft.Json"
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info("C# HTTP trigger function processed a request.");
        // parse query parameter
        var content = req.Content;
    
        string jsonContent = await content.ReadAsStringAsync(); 
        log.Info($"Received Event with payload: {jsonContent}");
    
        IEnumerable<string> headerValues;
        if (req.Headers.TryGetValues("Aeg-Event-Type", out headerValues))
        {
            var validationHeaderValue = headerValues.FirstOrDefault();
            if(validationHeaderValue == "SubscriptionValidation")
            {
            var events = JsonConvert.DeserializeObject<GridEvent[]>(jsonContent);
                 var code = events[0].Data["validationCode"];
                 return req.CreateResponse(HttpStatusCode.OK,
                 new { validationResponse = code });
            }
        }
    
        return jsonContent == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + jsonContent);
    }
    
    public class GridEvent
    {
        public string Id { get; set; }
        public string EventType { get; set; }
        public string Subject { get; set; }
        public DateTime EventTime { get; set; }
        public Dictionary<string, string> Data { get; set; }
        public string Topic { get; set; }
    }
    ```
4. 选择“保存并运行”。

    ![函数应用输出](./media/service-bus-to-event-grid-integration-example/function-run-output.png)
4. 选择工具栏上的“获取函数 URL”。 

    ![获取函数 URL](./media/service-bus-to-event-grid-integration-example/get-function-url.png)
5. 选择 URL 文本旁边的“复制”按钮。    
    ![复制函数 URL](./media/service-bus-to-event-grid-integration-example/get-function-url-copy.png)

---

## <a name="connect-the-function-and-namespace-via-event-grid"></a>通过事件网格连接函数和命名空间
在本部分，你要使用 Azure 门户将函数和服务总线命名空间绑定在一起。 

若要创建 Azure 事件网格订阅，请执行以下操作：

1. 在 Azure 门户中转到你的命名空间，然后在左窗格中选择“事件”。 此时会打开命名空间窗口，两个事件网格订阅显示在右窗格中。 
    
    ![服务总线 - 事件页](./media/service-bus-to-event-grid-integration-example/service-bus-events-page.png)
2. 在工具栏上选择“+ 事件订阅”。 
3. 在“创建事件订阅”页中执行以下步骤：
    1. 输入订阅的**名称**。 
    2. 输入系统主题的名称 。 系统主题是为 Azure 资源（如 Azure 存储帐户和 Azure 服务总线）创建的主题。 若要详细了解系统主题，请参阅[系统主题概述](../event-grid/system-topics.md)。
    2. 为“终结点类型”选择“Web Hook”。  

        ![服务总线 - 事件网格订阅](./media/service-bus-to-event-grid-integration-example/event-grid-subscription-page.png)
    3. 选中“选择终结点”，粘贴函数 URL，然后选择“确认选择” 。 

        ![函数 - 选择终结点](./media/service-bus-to-event-grid-integration-example/function-select-endpoint.png)
    4. 切换到“筛选器”选项卡，然后执行以下任务：
        1. 选择“启用主题筛选”
        2. 输入先前创建的服务总线主题的“第一个订阅”的名称。
        3. 选择“创建”按钮。 

            ![事件订阅筛选器](./media/service-bus-to-event-grid-integration-example/event-subscription-filter.png)
4. 切换到“事件”页面的“事件订阅”选项卡，并确认列表中显示了事件订阅 。

    ![列表中的事件订阅](./media/service-bus-to-event-grid-integration-example/event-subscription-in-list.png)

## <a name="send-messages-to-the-service-bus-topic"></a>向服务总线主题发送消息
1. 运行向服务总线主题发送消息的 .NET C# 应用程序。 

    ![控制台应用输出](./media/service-bus-to-event-grid-integration-example/console-app-output.png)
1. 在 Azure 函数应用的页面上，由“代码 + 测试”选项卡切换到“监视”选项卡 。应会看到发布到服务总线主题的每个消息的条目。 如果没有看到这些条目，请在等待几分钟后刷新页面。 

    ![监视函数](./media/service-bus-to-event-grid-integration-example/function-monitor.png)

    还可以使用“监视”页的“日志”选项卡在发送消息时查看日志信息 。 可能会有一些延迟，因此请在几分钟后查看记录的消息。 

## <a name="receive-messages-by-using-azure-functions"></a>使用 Azure Functions 接收消息
上一部分观察了一个简单的测试和调试方案，确保了事件在流动。 

此部分将介绍如何在收到事件后接收和处理消息。

### <a name="publish-a-function-from-visual-studio"></a>从 Visual Studio 发布函数
1. 在打开的同一个 Visual Studio 解决方案 (**SBEventGridIntegration**) 中，选择 **SBEventGridIntegration** 项目中的 **ReceiveMessagesOnEvent.cs**。 
2. 在以下代码中输入服务总线连接字符串：

    ```Csharp
    const string ServiceBusConnectionString = "YOUR CONNECTION STRING";
    ```
3. 下载函数的**发布配置文件**：
    1. 选择你的函数应用。 
    2. 选择“概述”选项卡（如果尚未选择）。 
    3. 在工具栏上选择“获取发布配置文件”。 

        ![获取函数的发布配置文件](./media/service-bus-to-event-grid-integration-example/function-download-publish-profile.png)
    4. 将文件保存到项目的文件夹中。 
4. 在 Visual Studio 中右键单击“SBEventGridIntegration”，然后选择“发布”。  
5. 在“发布”上，执行以下步骤： 
    1. 在“发布”页面上选择“启动”  
    2. 对于“目标”，请选择“导入配置文件” 。 
    3. 选择“**下一页**”。 

        ![Visual Studio -“导入配置文件”按钮](./media/service-bus-to-event-grid-integration-example/visual-studio-import-profile-button.png)
7. 选择前面下载的“发布配置文件”，然后选择“完成” 。

    ![选择发布配置文件](./media/service-bus-to-event-grid-integration-example/select-publish-profile.png)
8. 在“发布”页上选择“发布”。  

    ![Visual Studio - 发布](./media/service-bus-to-event-grid-integration-example/select-publish.png)
9. 确认看到了新的 Azure 函数 **ReceiveMessagesOnEvent**。 根据需要刷新页面。 

    ![确认已创建新函数](./media/service-bus-to-event-grid-integration-example/function-receive-messages.png)
10. 获取并记下新函数的 URL。 

### <a name="event-grid-subscription"></a>事件网格订阅

1. 删除现有的事件网格订阅：
    1. 在“服务总线命名空间”页上，选择左侧菜单中的“事件”。  
    2. 切换到“事件订阅”选项卡。 
    2. 选择现有的事件订阅。 

        ![选择事件订阅](./media/service-bus-to-event-grid-integration-example/select-event-subscription.png)
    3. 在“事件订阅”页上选择“删除”。  选择“是”以确认删除。 
        ![删除“事件订阅”按钮](./media/service-bus-to-event-grid-integration-example/delete-subscription-button.png)
2. 遵照[通过事件网格连接函数和命名空间](#connect-the-function-and-namespace-via-event-grid)部分的说明，使用新函数 URL 创建事件网格订阅。
3. 遵照[向服务总线主题发送消息](#send-messages-to-the-service-bus-topic)部分的说明，向主题发送消息并监视函数。 


## <a name="next-steps"></a>后续步骤

* 详细了解 [Azure 事件网格](https://docs.microsoft.com/azure/event-grid/)。
* 详细了解 [Azure Functions](https://docs.microsoft.com/azure/azure-functions/)。
* 详细了解 [Azure 应用服务的逻辑应用功能](https://docs.microsoft.com/azure/logic-apps/)。
* 详细了解 [Azure 服务总线](https://docs.microsoft.com/azure/service-bus/)。


[2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid2.png
[3]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid3.png
[7]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid7.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[10]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid10.png
[11]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid11.png
[12]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12.png
[12-1]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-1.png
[12-2]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid12-2.png
[13]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid13.png
[14]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid14.png
[15]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid15.png
[16]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid16.png
[17]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid17.png
[18]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid18.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png
