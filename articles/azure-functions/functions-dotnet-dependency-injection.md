---
title: 在 .NET Azure Functions 中使用依赖项注入
description: 了解如何使用依赖项注入在 .NET 函数中注册和使用服务
author: craigshoemaker
ms.topic: reference
ms.date: 09/05/2019
ms.author: cshoe
ms.reviewer: jehollan
ms.openlocfilehash: a1ff8e0aedce5d3a6acc9a39084cf0839efdd88e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678444"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>在 .NET Azure Functions 中使用依赖项注入

Azure Functions 支持依赖项注入 (DI) 软件设计模式，这是在类及其依赖项之间实现[控制反转 (IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) 的一种技术。

- Azure Functions 中的依赖项注入基于 .NET Core 依赖项注入功能。 建议熟悉[.NET 核心依赖项注入](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)。 There are differences in how you override dependencies and how configuration values are read with Azure Functions on the Consumption plan.

- 对依赖项注入的支持始于 Azure Functions 2.x。

## <a name="prerequisites"></a>先决条件

必须先安装以下 NuGet 包，然后才能使用依赖项注入：

- [Microsoft.Azure.Functions.Extensions](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Microsoft.NET.Sdk.Functions 包](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/)版本 1.0.28 或更高版本

## <a name="register-services"></a>注册服务

若要注册服务，请创建一个方法来配置组件并将组件添加到 `IFunctionsHostBuilder` 实例。  Azure Functions 主机创建 `IFunctionsHostBuilder` 的实例并将其直接传递到你的方法。

若要注册方法，请添加 `FunctionsStartup` 程序集属性来指定在启动期间使用的类型名称。

```csharp
using System;
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Http;
using Microsoft.Extensions.Logging;

[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();

            builder.Services.AddSingleton((s) => {
                return new MyService();
            });

            builder.Services.AddSingleton<ILoggerProvider, MyLoggerProvider>();
        }
    }
}
```

### <a name="caveats"></a>注意事项

在运行时处理启动类之前和之后，会运行一系列注册步骤。 因此，请注意以下事项：

- 启动类只能用于设置和注册。  避免使用在启动时注册的服务。 例如，请勿尝试在记录器中记录在启动过程中注册的消息。 此注册过程点对你的服务来说太早，因此无法使用。 在运行 `Configure` 方法以后，Functions 运行时会继续注册其他依赖项，这可能影响服务的运行。

- 依赖项注入容器仅存储显式注册的类型  。 能够用作可注入类型的服务仅限在 `Configure` 方法中设置的服务。 因此，特定于 Functions 的类型（例如 `BindingContext` 和 `ExecutionContext`）在设置中不可用，也不能用作可注入类型。

## <a name="use-injected-dependencies"></a>使用注入的依赖项

使用构造函数注入以后，依赖项即可在函数中使用。 使用构造函数注入要求你不得使用静态类。

以下示例演示了 `IMyService` 和 `HttpClient` 依赖项是如何注入到 HTTP 触发的函数中的。 此示例使用在启动时注册 `HttpClient` 所需的 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) 包。

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using System.Net.Http;

namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly IMyService _service;
        private readonly HttpClient _client;

        public HttpTrigger(IMyService service, HttpClient httpClient)
        {
            _service = service;
            _client = httpClient;
        }

        [FunctionName("GetPosts")]
        public async Task<IActionResult> Get(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = "posts")] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            var res = await _client.GetAsync("https://microsoft.com");
            await _service.AddResponse(res);

            return new OkResult();
        }
    }
}
```

## <a name="service-lifetimes"></a>服务生存期

Azure Functions 应用提供与 [ASP.NET 依赖项注入](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes)相同的服务生存期。 就 Functions 应用来说，不同的服务生存期表现如下：

- **暂时性**：每次请求此服务时，都会创建暂时性服务。
- **限定范围**：限定范围的服务的生存期与函数执行生存期相匹配。 具有作用域的服务针对每次执行创建一次。 在执行期间针对该服务的后续请求将重用现有的服务实例。
- **单一实例**：单一实例服务生存期与主机生存期相匹配，并且在该实例上的各个函数执行之间重用。 对于连接和客户端（例如 `SqlConnection` 或 `HttpClient` 实例），建议使用单一实例生存期服务。

在 GitHub 上查看或下载[不同服务生存期的示例](https://aka.ms/functions/di-sample)。

## <a name="logging-services"></a>日志记录服务

如果需要自己的日志记录提供程序，请将自定义类型注册为`ILoggerProvider`实例。 应用程序见解由 Azure 函数自动添加。

> [!WARNING]
> - 不要`AddApplicationInsightsTelemetry()`添加到服务集合，因为它注册的服务与环境提供的服务冲突。
> - 不要注册您自己的`TelemetryConfiguration`应用程序，或者`TelemetryClient`如果您正在使用内置的应用程序见解功能。 如果需要配置自己的`TelemetryClient`实例，请通过注入`TelemetryConfiguration`的实例创建一个实例，如监视器 Azure[函数](./functions-monitoring.md#version-2x-and-later-2)所示。

### <a name="iloggert-and-iloggerfactory"></a>ILogger<T>和 ILoggerFactory

主机将注入`ILogger<T>`和`ILoggerFactory`服务到构造函数中。  但是，默认情况下，这些新日志记录筛选器将从函数日志中筛选出来。  您需要修改`host.json`文件才能选择加入其他筛选器和类别。  下面的示例演示添加主机将`ILogger<HttpTrigger>`公开的日志。

```csharp
namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly ILogger<HttpTrigger> _log;

        public HttpTrigger(ILogger<HttpTrigger> log)
        {
            _log = log;
        }

        [FunctionName("HttpTrigger")]
        public async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req)
        {
            _log.LogInformation("C# HTTP trigger function processed a request.");

            // ...
    }
}
```

以及添加`host.json`日志筛选器的文件。

```json
{
    "version": "2.0",
    "logging": {
        "applicationInsights": {
            "samplingExcludedTypes": "Request",
            "samplingSettings": {
                "isEnabled": true
            }
        },
        "logLevel": {
            "MyNamespace.HttpTrigger": "Information"
        }
    }
}
```

## <a name="function-app-provided-services"></a>函数应用提供的服务

函数主机注册许多服务。 以下服务可以安全地用作应用程序中的依赖项：

|服务类型|生存期|说明|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|单一实例|运行时配置|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|单一实例|负责提供主机实例的 ID|

如果存在你要依赖的其他服务，请[在 GitHub 上创建一个问题并提出它们](https://github.com/azure/azure-functions-host)。

### <a name="overriding-host-services"></a>替代主机服务

目前不支持替代由主机提供的服务。  如果存在你要替代的服务，请[在 GitHub 上创建一个问题并提出它们](https://github.com/azure/azure-functions-host)。

## <a name="working-with-options-and-settings"></a>使用选项和设置

在[应用设置](./functions-how-to-use-azure-function-app-settings.md#settings)中定义的值可以在 `IConfiguration` 实例中使用，这使得你可以读取启动类中的应用设置值。

可以将 `IConfiguration` 实例中的值提取为自定义类型。 将应用设置值复制为自定义类型以后，即可使这些值变得可注入，方便测试服务。 读取到配置实例的设置必须是简单的键/值对。

请考虑以下类，该类包含名为与应用设置一致的属性：

```csharp
public class MyOptions
{
    public string MyCustomSetting { get; set; }
}
```

`local.settings.json`文件可能按照如下方式构造自定义设置：
```json
{
  "IsEncrypted": false,
  "Values": {
    "MyOptions:MyCustomSetting": "Foobar"
  }
}
```

在 `Startup.Configure` 方法中，可以使用以下代码将 `IConfiguration` 实例的值提取为自定义类型：

```csharp
builder.Services.AddOptions<MyOptions>()
                .Configure<IConfiguration>((settings, configuration) =>
                                           {
                                                configuration.GetSection("MyOptions").Bind(settings);
                                           });
```

调用 `Bind` 可以将那些与属性名匹配的值从配置复制到自定义实例中。 IoC 容器中现在提供可以注入到函数中的选项实例。

选项对象作为泛型 `IOptions` 接口的实例注入函数中。 使用 `Value` 属性访问在配置中发现的值。

```csharp
using System;
using Microsoft.Extensions.Options;

public class HttpTrigger
{
    private readonly MyOptions _settings;

    public HttpTrigger(IOptions<MyOptions> options)
    {
        _settings = options.Value;
    }
}
```

请参阅 [ASP.NET Core 中的选项模式](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options)，更详细地了解如何使用选项。

> [!WARNING]
> 请避免尝试从有关消耗计划的 local.settings.json** 或 appsettings.{environment}.json** 等文件中读取值。 从与触发器连接相关的这些文件中读取的值在应用缩放时不可用，因为托管基础结构无法访问配置信息。

## <a name="next-steps"></a>后续步骤

有关更多信息，请参见以下资源：

- [如何监视函数应用](functions-monitoring.md)
- [适用于函数的最佳做法](functions-best-practices.md)
