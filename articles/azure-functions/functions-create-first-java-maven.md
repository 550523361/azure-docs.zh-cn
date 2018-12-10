---
title: 在 Azure 中通过 Java 和 Maven 创建你的第一个函数 | Microsoft Docs
description: 通过 Java 和 Maven 创建一个简单的 HTTP 触发函数，并将其发布到 Azure。
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: azure functions, functions, 事件处理, 计算, 无服务器体系结构
ms.service: azure-functions
ms.devlang: java
ms.topic: quickstart
ms.date: 08/10/2018
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: fdd29bbfaf36619fd823220e5d32a48a1619679b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842061"
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>通过 Java 和 Maven 创建你的第一个函数（预览版）

> [!NOTE] 
> 用于 Azure Functions 的 Java 当前为预览版。

本快速入门可指导通过 Maven 创建[无服务器](https://azure.microsoft.com/solutions/serverless/)函数项目，在本地对其进行测试，并将其部署到 Azure。 完成后，你的 Java 函数代码将在云中运行，并可以通过 HTTP 请求触发。

![通过 cURL 从命令行中访问 Hello World 函数](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>先决条件
若要通过 Java 开发函数应用，必须安装以下软件：

-  [Java 开发人员工具包](https://www.azul.com/downloads/zulu/)，版本 8。
-  [Apache Maven](https://maven.apache.org) 3.0 或更高版本。
-  [Azure CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> JAVA_HOME 环境变量必须设置为 JDK 的安装位置，以完成本快速入门。

## <a name="install-the-azure-functions-core-tools"></a>安装 Azure Functions Core Tools

[Azure Functions 核心工具 2.0](https://www.npmjs.com/package/azure-functions-core-tools) 为编写、运行和调试 Azure Functions 提供了本地开发环境。 

若要进行安装，请访问 Azure Functions Core Tools 项目的[安装](https://github.com/azure/azure-functions-core-tools#installing)部分，找到操作系统的具体说明。

也可以在安装以下必备组件后，使用 [Node.js](https://nodejs.org/) 随附的 [npm](https://www.npmjs.com/) 手动安装此工具：

-  最新版本的 [.NET Core](https://www.microsoft.com/net/core)。
-  [Node.js](https://nodejs.org/download/) 8.6 或更高版本。

若要继续进行基于 npm 的安装，请运行：

```
npm install -g azure-functions-core-tools@core
```

> [!NOTE]
> 如果在安装 Azure Functions 核心工具版本 2.0 时遇到问题，请参阅[版本 2.x 运行时](/azure/azure-functions/functions-run-local#version-2x-runtime)。

## <a name="generate-a-new-functions-project"></a>生成新的 Functions 项目

在空的文件夹中，运行以下命令以从 [Maven archetype](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) 生成 Functions 项目。

### <a name="linuxmacos"></a>Linux/MacOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

### <a name="windows-cmd"></a>Windows (CMD)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-archetype
```

Maven 会请求你提供所需的值以完成项目的生成。 有关 groupId、artifactId 和 version 值，请参阅 [Maven 命名约定](https://maven.apache.org/guides/mini/guide-naming-conventions.html)参考。 AppName 值在 Azure 中必须唯一，以便 Maven 基于以前输入的 artifactId 生成默认应用名称。 PackageName 值确定所生成函数代码的 Java 包。

下面的 `com.fabrikam.functions` 和 `fabrikam-functions` 标识符用作示例，目的是使本快速入门中后面的步骤更易读。 建议你在此步骤中向 Maven 提供你自己的值。

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Confirm properties configuration: Y
```

在此示例 `fabrikam-functions` 中，Maven 在新文件夹中创建名为 artifactId 的项目文件 项目中生成的可以运行的代码是一个简单的回显请求正文的 [HTTP 触发](/azure/azure-functions/functions-bindings-http-webhook)函数：

```java
public class Function {
    /**
     * This function listens at endpoint "/api/hello". Two ways to invoke it using "curl" command in bash:
     * 1. curl -d "HTTP Body" {your host}/api/hello
     * 2. curl {your host}/api/hello?name=HTTP%20Query
     */
    @FunctionName("hello")
    public HttpResponseMessage<String> hello(
            @HttpTrigger(name = "req", methods = {"get", "post"}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponse(400, "Please pass a name on the query string or in the request body");
        } else {
            return request.createResponse(200, "Hello, " + name);
        }
    }
}

```

## <a name="run-the-function-locally"></a>在本地运行函数

将目录更改为新创建的项目文件夹，并通过 Maven 生成和运行此函数：

```
cd fabrikam-function
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> 如果在使用 Java 9 时遇到 `javax.xml.bind.JAXBException` 异常，请参阅 [GitHub](https://github.com/jOOQ/jOOQ/issues/6477) 上的解决方法。

当函数在本地系统上运行并且做好响应 HTTP 请求的准备时，将显示以下输出：

```Output
Listening on http://localhost:7071
Hit CTRL-C to exit...

Http Functions:

   hello: http://localhost:7071/api/hello
```

使用 curl 在新的终端窗口中从命令行触发函数：

```
curl -w '\n' -d LocalFunction http://localhost:7071/api/hello
```

```Output
Hello LocalFunction!
```

在终端中使用 `Ctrl-C` 停止函数代码。

## <a name="deploy-the-function-to-azure"></a>将函数部署到 Azure

部署到 Azure Functions 的过程中会使用 Azure CLI 中的帐户凭据。 在继续操作之前[使用 Azure CLI 登录](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)。

```azurecli
az login
```

使用 `azure-functions:deploy` Maven 目标将代码部署到新的函数应用。

```
mvn azure-functions:deploy
```

部署完成后，将显示可用于访问你的 Azure 函数应用的 URL：

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

使用 `cURL` 测试在 Azure 上运行的函数应用。 需更改以下示例中的 URL，使之与前一步骤中你自己的函数应用的已部署 URL 匹配。

```
curl -w '\n' https://fabrikam-function-20170920120101928.azurewebsites.net/api/hello -d AzureFunctions
```

```Output
Hello AzureFunctions!
```

## <a name="make-changes-and-redeploy"></a>进行更改并重新部署

编辑生成的项目中的 `src/main.../Function.java` 源文件来更改你的函数应用返回的文本。 更改以下行：

```java
return request.createResponse(200, "Hello, " + name);
```

更改为以下内容：

```java
return request.createResponse(200, "Hi, " + name);
```

保存更改，如以前一样通过从终端运行 `azure-functions:deploy` 进行重新部署。 函数应用将更新，并且以下请求：

```bash
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

将具有更新的输出：

```Output
Hi, AzureFunctionsTest
```

## <a name="next-steps"></a>后续步骤

你已使用简单的 HTTP 触发器创建 Java 函数应用，并将其部署到 Azure Functions。

- 有关开发 Java 函数的详细信息，请查看 [Java 函数开发人员指南](functions-reference-java.md)。
- 使用 `azure-functions:add` Maven 目标将具有不同触发器的其他函数添加到你的项目。
- 使用 [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions)、[IntelliJ](functions-create-maven-intellij.md) 和 [Eclipse](functions-create-maven-eclipse.md) 在本地编写并调试函数。 
- 使用 Visual Studio Code 调试在 Azure 中部署的函数。 有关说明，请参阅 Visual Studio Code [无服务器 Java 应用程序](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)文档。
