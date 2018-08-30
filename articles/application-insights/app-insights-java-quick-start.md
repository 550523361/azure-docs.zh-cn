---
title: Azure Application Insights 快速入门 | Microsoft docs
description: 提供有关快速安装 Java Web 应用以使用 Application Insights 进行监视的说明
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.reviewer: lagayhar
ms.date: 07/11/2018
ms.service: application-insights
ms.custom: mvc
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: b36e4598f5ff20b921c5cd150ae19be233cc2d14
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2018
ms.locfileid: "43246507"
---
# <a name="start-monitoring-your-java-web-application"></a>开始监视 Java Web 应用程序

使用 Azure Application Insights，可轻松监视 Web 应用程序的可用性、性能和使用情况。 还可以快速确定并诊断应用程序中的错误，而无需等待用户报告这些错误。 使用 Application Insights Java SDK，可以监视常见的第三方包，包括 MongoDB、MySQL 和 Redis。

本快速入门介绍如何将 Application Insights SDK 添加到现有 Java 动态 Web 项目。

## <a name="prerequisites"></a>先决条件

完成本快速入门教程：

- 安装 JRE 1.7 或 1.8
- 安装[免费 Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/)。 本快速入门使用 Eclipse Oxygen (4.7)
- 将需要 Azure 订阅和现有 Java 动态 Web 项目
 
如果没有 Java 动态 Web 项目，可以使用[创建 Java Web 应用快速入门](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-java)创建一个。

如果没有 Azure 订阅，请在开始之前创建一个[免费](https://azure.microsoft.com/free/)帐户。

如果你更喜欢 Spring 框架，请尝试[配置 Spring Boot 初始值设定程序以使用 Application Insights 指南](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights)

## <a name="log-in-to-the-azure-portal"></a>登录到 Azure 门户

登录到 [Azure 门户](https://portal.azure.com/)。

## <a name="enable-application-insights"></a>启用 Application Insights

Application Insights 可以从任何连接 Internet 的应用程序收集遥测数据，而不考虑它是在本地运行还是在云中运行。 按照以下步骤开始查看此数据。

1. 选择“创建资源” > “监视 + 管理” > “Application Insights”。

   ![添加 Application Insights 资源](./media/app-insights-java-quick-start/001-j.png)

   此时会显示配置对话框，请使用下表填写输入字段。

    | 设置        | 值           | 说明  |
   | ------------- |:-------------|:-----|
   | **Name**      | 全局唯一值 | 标识所监视的应用的名称 |
   | **应用程序类型** | Java Web 应用程序 | 所监视的应用的类型 |
   | **资源组**     | myResourceGroup      | 用于托管 App Insights 数据的新资源组的名称 |
   | **位置** | 美国东部 | 选择离你近的位置或离托管应用的位置近的位置 |

2. 单击“创建”。

## <a name="install-app-insights-plugin"></a>安装 App Insights 插件

1. 启动 **Eclipse** > 单击“帮助”> 选择“安装新软件”。

   ![“新建 App Insights 资源”窗体](./media/app-insights-java-quick-start/000-j.png)

2. 将 ```http://dl.microsoft.com/eclipse``` 复制到“使用”字段中 > 选中“用于 Java 的 Azure 工具包”> 选择“用于 Java 的 Application Insights 插件” > **取消选中**“安装期间联系所有更新站点以查找所需的软件”。

3. 安装完成后，系统会提示“重启 Eclipse”。

## <a name="configure-app-insights-plugin"></a>配置 App Insights 插件

1. 启动 **Eclipse** > 打开“项目”> 在“项目资源管理器”中右键单击项目名称 > 选择 **Azure** > 单击“登录”。

2. 选择身份验证方法“交互式”> 单击“登录”> 出现提示时输入 **Azure 凭据** > 选择 **Azure订阅**。

3. 在**项目资源管理器**中右键单击项目名称 > 选择 **Azure** > 单击“配置 Application Insights”。

4. 选中“使用 Application Insights 启用遥测”> 选择要链接到 Java 应用的 App Insights 资源和关联的**检测密钥**。

   ![Eclipse Azure 配置菜单](./media/app-insights-java-quick-start/0007-j.png)

> [!NOTE]
> 用于 Java 的 Application Insights SDK 能够捕获并直观显示实时指标，但首次启用遥测数据收集时，可能需要几分钟，然后数据才开始显示在门户中。 如果此应用是一个低流量测试应用，请记住，仅当存在活动请求或操作时，才会捕获大多数指标。

## <a name="start-monitoring-in-the-azure-portal"></a>开始在 Azure 门户中监视

1. 现在可以在 Azure 门户中重新打开 Application Insights“概述”页（已在其中检索到检测密钥），查看有关当前正在运行的应用程序的详细信息。

   ![Application Insights 概述菜单](./media/app-insights-java-quick-start/overview-001.png)

2. 单击“应用程序映射”以获取应用程序组件之间依赖关系的可视布局。 每个组件均显示 KPI，如负载、性能、失败和警报。

   ![应用程序地图](./media/app-insights-java-quick-start/application-map-001.png)

3. 单击“应用分析”图标 ![“应用程序映射”图标](./media/app-insights-java-quick-start/006.png)。 这将打开“Application Insights Analytics”，该软件提供丰富的查询语言，可用于分析 Application Insights 收集的所有数据。 在本示例中，将生成以图表形式呈现请求计数的查询。 可以编写自己的查询来分析其他数据。

   ![一段时间内用户请求的分析图](./media/app-insights-java-quick-start/0010-j.png)

4. 返回到“概述”页并检查 KPI 图形。  此仪表板提供有关应用程序运行状况的统计信息，包括传入请求数、这些请求的持续时间，以及发生的任何故障。

   ![“运行状况概述时间线”图](./media/app-insights-java-quick-start/overview-perf.png)

   若要启用“页面视图加载时间”图表以填充“客户端遥测”数据，请将此脚本添加到要跟踪的每一页：

   ```HTML
   <!-- 
   To collect user behavior analytics about your application, 
   insert the following script into each page you want to track.
   Place this code immediately before the closing </head> tag,
   and before any other scripts. Your first data will appear 
   automatically in just a few seconds.
   -->
   <script type="text/javascript">
     var appInsights=window.appInsights||function(config){
     function i(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s="AuthenticatedUserContext",h="start",c="stop",l="Track",a=l+"Event",v=l+"Page",y=u.createElement(o),r,f;y.src=config.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(y);try{t.cookie=u.cookie}catch(p){}for(t.queue=[],t.version="1.0",r=["Event","Exception","Metric","PageView","Trace","Dependency"];r.length;)i("track"+r.pop());return i("set"+s),i("clear"+s),i(h+a),i(c+a),i(h+v),i(c+v),i("flush"),config.disableExceptionTracking||(r="onerror",i("_"+r),f=e[r],e[r]=function(config,i,u,e,o){var s=f&&f(config,i,u,e,o);return s!==!0&&t["_"+r](config,i,u,e,o),s}),t
    }({
        instrumentationKey:"<instrumentation key>"
    });

    window.appInsights=appInsights;
    appInsights.trackPageView();
   </script>
    ```

5. 单击“实时流”。 在此处可找到与 Java Web 应用性能相关的实时指标。 **实时指标流**包括与以下项相关的数据：传入请求数、这些请求的持续时间和发生的任何故障。 还可以实时监视处理器和内存等关键性能指标。

   ![“服务器指标”图](./media/app-insights-java-quick-start/livemetricsjava.png)

若要了解有关监视 Java 的详细信息，请查看[其他 App Insights Java 文档](.\app-insights-java-get-started.md)。

## <a name="clean-up-resources"></a>清理资源

如果计划继续使用后续的快速入门或相关教程，请勿清除在本快速入门中创建的资源。 如果不打算继续，请在 Azure 门户中执行以下步骤，删除通过此快速入门创建的所有资源。

1. 在 Azure 门户的左侧菜单中，单击“资源组”，然后单击“myResourceGroup”。
2. 在资源组页上单击“删除”，在文本框中键入 **myResourceGroup**，然后单击“删除”。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [查找和诊断性能问题](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)