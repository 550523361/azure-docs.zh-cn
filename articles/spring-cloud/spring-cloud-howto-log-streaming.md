---
title: 实时流式传输 Azure Spring Cloud 应用日志
description: 如何使用日志流式处理即时查看应用程序日志
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/14/2019
ms.custom: devx-track-java
ms.openlocfilehash: fb76f7897b9647a688e21993002f9c96fe9487f8
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89046742"
---
# <a name="stream-azure-spring-cloud-app-logs-in-real-time"></a>实时流式传输 Azure Spring Cloud 应用日志
Azure 春季 Cloud 在 Azure CLI 中启用日志流式处理，以获取实时应用程序控制台日志以进行故障排除。 还可以 [通过诊断设置分析日志和指标](./diagnostic-services.md)。

## <a name="prerequisites"></a>先决条件

* 安装适用于春季 Cloud 的 [Azure CLI 扩展](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli#install-the-azure-cli-extension) ，最低版本为0.2.0。
* 带有正在运行的应用程序的 **Azure 春季 cloud** 的实例，例如 " [春季 cloud app](./spring-cloud-quickstart-launch-app-cli.md)"。

> [!NOTE]
>  ASC CLI 扩展已从版本为0.2.0 更新为0.2.1。 此更改影响日志流式处理的命令的语法： `az spring-cloud app log tail` ，它被替换为： `az spring-cloud app logs` 。 命令： `az spring-cloud app log tail` 将在未来版本中弃用。 如果使用的是版本为0.2.0，则可以升级到0.2.1。 首先，删除旧版本的命令： `az extension remove -n spring-cloud` 。  然后，使用以下命令安装0.2.1： `az extension add -n spring-cloud` 。

## <a name="use-cli-to-tail-logs"></a>使用 CLI 来结尾日志

若要避免重复指定资源组和服务实例的名称，请设置默认资源组名称和群集名称。
```
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
```
在以下示例中，将在命令中省略资源组和服务名称。

### <a name="tail-log-for-app-with-single-instance"></a>带有单个实例的应用程序的尾日志
如果名为 "身份验证服务" 的应用只有一个实例，则可以使用以下命令查看应用实例的日志：
```
az spring-cloud app logs -n auth-service
```
这将返回日志：
```
...
2020-01-15 01:54:40.481  INFO [auth-service,,,] 1 --- [main] o.apache.catalina.core.StandardService  : Starting service [Tomcat]
2020-01-15 01:54:40.482  INFO [auth-service,,,] 1 --- [main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.22]
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.a.c.c.C.[Tomcat].[localhost].[/uaa]  : Initializing Spring embedded WebApplicationContext
2020-01-15 01:54:40.760  INFO [auth-service,,,] 1 --- [main] o.s.web.context.ContextLoader  : Root WebApplicationContext: initialization completed in 7203 ms

...
```

### <a name="tail-log-for-app-with-multiple-instances"></a>具有多个实例的应用的结尾日志
如果有多个名为的应用程序实例 `auth-service` ，则可以使用选项查看实例日志 `-i/--instance` 。 

首先，可以通过以下命令获取应用程序实例的名称。

```
az spring-cloud app show -n auth-service --query properties.activeDeployment.properties.instances -o table
```
具有结果：

```
Name                                         Status    DiscoveryStatus
-------------------------------------------  --------  -----------------
auth-service-default-12-75cc4577fc-pw7hb  Running   UP
auth-service-default-12-75cc4577fc-8nt4m  Running   UP
auth-service-default-12-75cc4577fc-n25mh  Running   UP
``` 
然后，可以使用选项选项流式传输应用程序实例的日志 `-i/--instance` ：

```
az spring-cloud app logs -n auth-service -i auth-service-default-12-75cc4577fc-pw7hb
```

还可以从 Azure 门户获取应用实例的详细信息。  选择 Azure 春季云服务左侧导航窗格中的 " **应用** " 后，选择 " **应用实例**"。

### <a name="continuously-stream-new-logs"></a>连续流式传输新日志
默认情况下， `az spring-cloud ap log tail` 仅打印流式传输到应用控制台的现有日志，然后退出。 如果要流式传输新日志，请添加-f (--遵循) ：  

```
az spring-cloud app logs -n auth-service -f
``` 
检查支持的所有日志记录选项：
``` 
az spring-cloud app logs -h 
```

## <a name="next-steps"></a>后续步骤
* [快速入门：通过日志、指标和跟踪来监视 Azure 春季云应用](spring-cloud-quickstart-logs-metrics-tracing.md)
* [通过诊断设置分析日志和指标](./diagnostic-services.md)

 





