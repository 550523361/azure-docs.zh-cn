---
title: Azure Functions 的 PowerShell 开发人员参考
description: 了解如何使用 PowerShell 开发函数。
author: eamonoreilly
ms.topic: conceptual
ms.date: 04/22/2019
ms.openlocfilehash: 41f977e7e7c23c2f49fd656461b7a3920802997e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "79276733"
---
# <a name="azure-functions-powershell-developer-guide"></a>Azure Functions PowerShell 开发人员指南

本文详细介绍如何使用 PowerShell 编写 Azure Functions。

PowerShell Azure function （函数）表示为触发时执行的 PowerShell 脚本。 每个函数脚本都有`function.json`一个相关文件，用于定义该函数的行为方式，如其触发方式及其输入和输出参数。 若要了解详细信息，请参阅[触发器和绑定一文](functions-triggers-bindings.md)。 

与其他类型的函数一样，PowerShell 脚本函数采用与在`function.json`文件中定义的所有输入绑定的名称相匹配的参数。 还`TriggerMetadata`传递了一个参数，该参数包含有关启动函数的触发器的附加信息。

本文假定你已阅读 [Azure Functions 开发人员参考](functions-reference.md)。 还应已完成 PowerShell 的[函数快速入门](functions-create-first-function-powershell.md)，以创建第一个 powershell 函数。

## <a name="folder-structure"></a>文件夹结构

PowerShell 项目的所需文件夹结构如下所示。 可更改此默认值。 有关详细信息，请参阅下面的 [scriptFile](#configure-function-scriptfile) 部分。

```
PSFunctionApp
 | - MyFirstFunction
 | | - run.ps1
 | | - function.json
 | - MySecondFunction
 | | - run.ps1
 | | - function.json
 | - Modules
 | | - myFirstHelperModule
 | | | - myFirstHelperModule.psd1
 | | | - myFirstHelperModule.psm1
 | | - mySecondHelperModule
 | | | - mySecondHelperModule.psd1
 | | | - mySecondHelperModule.psm1
 | - local.settings.json
 | - host.json
 | - requirements.psd1
 | - profile.ps1
 | - extensions.csproj
 | - bin
```

在项目的根目录中，有一个可用于配置[`host.json`](functions-host-json.md) function app 的共享文件。 每个函数都有一个具有其自己的代码文件（ps1）和绑定配置文件`function.json`（）的文件夹。 函数的父目录名称始终是函数的名称。

某些绑定需要`extensions.csproj`文件存在。 [版本2.x 和更高版本](functions-versions.md)的函数运行时中所需的绑定扩展在`extensions.csproj`文件中定义，实际库文件位于`bin`文件夹中。 本地开发时，必须[注册绑定扩展](functions-bindings-register.md#extension-bundles)。 在 Azure 门户中开发函数时，系统将为你完成此注册。

在 PowerShell Function Apps 中，你可以选择在`profile.ps1`函数应用开始运行时运行它（也称为*[冷启动](#cold-start)*）。 有关详细信息，请参阅[PowerShell 配置文件](#powershell-profile)。

## <a name="defining-a-powershell-script-as-a-function"></a>将 PowerShell 脚本定义为函数

默认情况下，Functions 运行时会在 `run.ps1` 中查找你的函数，其中，`run.ps1` 与其相应的 `function.json` 共享同一个父目录。

您的脚本在执行时传递了多个参数。 若要处理这些参数，请`param`将块添加到脚本顶部，如以下示例中所示：

```powershell
# $TriggerMetadata is optional here. If you don't need it, you can safely remove it from the param block
param($MyFirstInputBinding, $MySecondInputBinding, $TriggerMetadata)
```

### <a name="triggermetadata-parameter"></a>TriggerMetadata 参数

`TriggerMetadata`参数用于提供有关触发器的附加信息。 其他元数据的不同之处在于绑定到绑定，但是`sys`它们都包含一个属性，该属性包含以下数据：

```powershell
$TriggerMetadata.sys
```

| properties   | 说明                                     | 类型     |
|------------|-------------------------------------------------|----------|
| UtcNow     | 当触发函数时，采用 UTC 格式        | DateTime |
| MethodName | 触发的函数的名称     | 字符串   |
| RandGuid   | 此函数执行的唯一 guid | 字符串   |

每个触发器类型都有一组不同的元数据。 `$TriggerMetadata`例如，的`QueueTrigger`包含`InsertionTime`、 `Id` `DequeueCount`、等。 有关队列触发器的元数据的详细信息，请参阅[队列触发器的官方文档](functions-bindings-storage-queue-trigger.md#message-metadata)。 查看正在处理的[触发器](functions-triggers-bindings.md)的相关文档，了解触发器元数据内部的内容。

## <a name="bindings"></a>绑定

在 PowerShell 中，在函数的函数 json 中配置和定义[绑定](functions-triggers-bindings.md)。 函数通过多种方式来与绑定交互。

### <a name="reading-trigger-and-input-data"></a>读取触发器和输入数据

触发器和输入绑定作为传递到函数的参数读取。 输入绑定在函数`direction` json 中`in`的设置为。 在`name`中`function.json`定义的属性是`param`块中参数的名称。 由于 PowerShell 使用命名参数进行绑定，因此参数的顺序并不重要。 但是，最佳做法是遵循中定义的绑定的顺序`function.json`。

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)
```

### <a name="writing-output-data"></a>写入输出数据

在函数中，输出绑定在函数`direction` json 中`out`的设置为。 您可以使用`Push-OutputBinding` cmdlet 将其写入到输出绑定，该 cmdlet 可用于函数运行时。 在所有情况下， `name`中`function.json`定义的绑定属性都与`Name` `Push-OutputBinding` cmdlet 的参数相对应。

下面演示了如何在函数`Push-OutputBinding`脚本中调用：

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Push-OutputBinding -Name myQueue -Value $myValue
```

还可以通过管道传递特定绑定的值。

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Produce-MyOutputValue | Push-OutputBinding -Name myQueue
```

`Push-OutputBinding`的行为方式与为指定的`-Name`值不同：

* 当指定的名称无法解析为有效的输出绑定时，将引发错误。

* 当输出绑定接受值的集合时，可以重复调用`Push-OutputBinding`来推送多个值。

* 当输出绑定只接受单一值时，第二`Push-OutputBinding`次调用会引发错误。

#### <a name="push-outputbinding-syntax"></a>`Push-OutputBinding` 语法

下面是调用`Push-OutputBinding`的有效参数：

| 名称 | 类型 | 位置 | 说明 |
| ---- | ---- |  -------- | ----------- |
| **`-Name`** | 字符串 | 1 | 要设置的输出绑定的名称。 |
| **`-Value`** | 对象 | 2 | 要设置的输出绑定的值，它从管道 ByValue 接受。 |
| **`-Clobber`** | SwitchParameter | 已命名 | 可有可无指定时，将强制为指定的输出绑定设置值。 | 

还支持以下常见参数： 
* `Verbose`
* `Debug`
* `ErrorAction`
* `ErrorVariable`
* `WarningAction`
* `WarningVariable`
* `OutBuffer`
* `PipelineVariable`
* `OutVariable` 

有关详细信息，请参阅[About CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)。

#### <a name="push-outputbinding-example-http-responses"></a>OutputBinding 示例： HTTP 响应

HTTP 触发器使用名为`response`的输出绑定返回响应。 在下面的示例中，的`response`输出绑定的值为 "output #1"：

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #1"
})
```

由于输出是 HTTP （仅接受单一实例值），因此，在第二次调用`Push-OutputBinding`时会引发错误。

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #2"
})
```

对于仅接受单独值的输出，可以使用`-Clobber`参数重写旧值，而不是尝试添加到集合。 下面的示例假定您已经添加了一个值。 通过使用`-Clobber`，以下示例中的响应将重写现有值以返回 "output #3" 值：

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #3"
}) -Clobber
```

#### <a name="push-outputbinding-example-queue-output-binding"></a>OutputBinding 示例：队列输出绑定

`Push-OutputBinding`用于将数据发送到输出绑定，如[Azure 队列存储输出绑定](functions-bindings-storage-queue-output.md)。 在下面的示例中，写入队列的消息的值为 "output #1"：

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #1"
```

存储队列的输出绑定接受多个输出值。 在这种情况下，在第一次写入队列后调用下面的示例： "output #1" 和 "output #2"。

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #2"
```

下面的示例（在上一两个调用后调用）将另外两个值添加到输出集合：

```powershell
PS >Push-OutputBinding -Name outQueue -Value @("output #3", "output #4")
```

写入队列时，消息包含以下四个值： "output #1"、"output #2"、"output #3" 和 "output #4"。

#### <a name="get-outputbinding-cmdlet"></a>`Get-OutputBinding` cmdlet

可以使用`Get-OutputBinding` cmdlet 检索当前为输出绑定设置的值。 此 cmdlet 将检索包含输出绑定名称及其各自值的哈希表。 

下面的示例使用`Get-OutputBinding`返回当前绑定值：

```powershell
Get-OutputBinding
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
MyOtherQueue                   myData
```

`Get-OutputBinding`还包含一个名`-Name`为的参数，该参数可用于筛选返回的绑定，如以下示例中所示：

```powershell
Get-OutputBinding -Name MyQ*
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
```

中`Get-OutputBinding`支持通配符（*）。

## <a name="logging"></a>日志记录

PowerShell 函数中的日志记录类似于常规的 PowerShell 日志记录。 您可以使用日志记录 cmdlet 来写入每个输出流。 每个 cmdlet 都映射到函数使用的日志级别。

| 函数日志记录级别 | 日志记录 cmdlet |
| ------------- | -------------- |
| 错误 | **`Write-Error`** |
| 警告 | **`Write-Warning`**  | 
| 信息 | **`Write-Information`** <br/> **`Write-Host`** <br /> **`Write-Output`**      | 信息 | 写入_信息_级别日志记录。 |
| 调试 | **`Write-Debug`** |
| 跟踪 | **`Write-Progress`** <br /> **`Write-Verbose`** |

除了这些 cmdlet 外，写入管道的任何内容都将重定向到`Information`日志级别并以默认的 PowerShell 格式显示。

> [!IMPORTANT]
> 使用`Write-Verbose`或`Write-Debug` cmdlet 不足以查看详细和调试级别日志记录。 你还必须配置日志级别阈值，该阈值声明你实际关注的日志级别。 若要了解详细信息，请参阅[配置函数应用日志级别](#configure-the-function-app-log-level)。

### <a name="configure-the-function-app-log-level"></a>配置 function app 日志级别

Azure Functions 允许您定义阈值级别，以便轻松控制函数写入日志的方式。 若要为写入控制台的所有跟踪设置阈值，请使用`logging.logLevel.default` [ `host.json`文件][host json 引用]中的属性。 此设置应用于 Function App 中的所有函数。

下面的示例将阈值设置为对所有函数启用详细日志记录，但将阈值设置为对名`MyFunction`为的函数启用调试日志记录：

```json
{
    "logging": {
        "logLevel": {
            "Function.MyFunction": "Debug",
            "default": "Trace"
        }
    }
}  
```

有关详细信息，请参阅 [host.json 参考]。

### <a name="viewing-the-logs"></a>查看日志

如果 Function App 在 Azure 中运行，则可以使用 Application Insights 进行监视。 若要了解有关查看和查询函数日志的详细信息，请阅读[监视 Azure Functions](functions-monitoring.md)。

如果要在本地运行 Function App 进行开发，则日志将默认保存到文件系统。 若要在控制台中查看日志，请在`AZURE_FUNCTIONS_ENVIRONMENT`启动 Function App 之前`Development`将环境变量设置为。

## <a name="triggers-and-bindings-types"></a>触发器和绑定类型

你可以使用许多触发器和绑定来与函数应用一起使用。 [可在此处找到](functions-triggers-bindings.md#supported-bindings)触发器和绑定的完整列表。

所有触发器和绑定在代码中表示为一些真实的数据类型：

* Hashtable
* 字符串
* byte[]
* int
* double
* HttpRequestContext
* HttpResponseContext

此列表中的前五个类型是标准 .NET 类型。 最后两个仅由[HttpTrigger 触发器](#http-triggers-and-bindings)使用。

函数中的每个绑定参数都必须是这些类型之一。

### <a name="http-triggers-and-bindings"></a>HTTP 触发器和绑定

HTTP 和 webhook 触发器以及 HTTP 输出绑定使用请求和响应对象来表示 HTTP 消息。

#### <a name="request-object"></a>请求对象

传递到脚本的请求对象的类型`HttpRequestContext`为，它具有以下属性：

| properties  | 说明                                                    | 类型                      |
|-----------|----------------------------------------------------------------|---------------------------|
| **`Body`**    | 一个包含请求正文的对象。 `Body`基于数据序列化为最佳类型。 例如，如果数据是 JSON，则以哈希表形式传递。 如果数据是字符串，则以字符串的形式传递。 | 对象 (object) |
| **`Headers`** | 包含请求标头的字典。                | Dictionary<string，string><sup>*</sup> |
| **`Method`** | 请求的 HTTP 方法。                                | 字符串                    |
| **`Params`**  | 一个包含请求的路由参数的对象。 | Dictionary<string，string><sup>*</sup> |
| **`Query`** | 一个包含查询参数的对象。                  | Dictionary<string，string><sup>*</sup> |
| **`Url`** | 请求的 URL。                                        | 字符串                    |

<sup>*</sup>所有`Dictionary<string,string>`键都不区分大小写。

#### <a name="response-object"></a>响应对象

应发送回的响应对象的类型`HttpResponseContext`为，它具有以下属性：

| properties      | 说明                                                 | 类型                      |
|---------------|-------------------------------------------------------------|---------------------------|
| **`Body`**  | 一个包含响应正文的对象。           | 对象 (object)                    |
| **`ContentType`** | 用于设置响应的内容类型的简短内容。 | 字符串                    |
| **`Headers`** | 一个包含响应标头的对象。               | 字典或哈希表   |
| **`StatusCode`**  | 响应的 HTTP 状态代码。                       | 字符串或整数             |

#### <a name="accessing-the-request-and-response"></a>访问请求和响应

当使用 HTTP 触发器时，可以使用与任何其他输入绑定相同的方式访问 HTTP 请求。 它在`param`块中。

使用`HttpResponseContext`对象返回响应，如下所示：

`function.json`

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "anonymous"
    },
    {
      "type": "http",
      "direction": "out"
    }
  ]
}
```

`run.ps1`

```powershell
param($req, $TriggerMetadata)

$name = $req.Query.Name

Push-OutputBinding -Name res -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "Hello $name!"
})
```

调用此函数的结果为：

```
PS > irm http://localhost:5001?Name=Functions
Hello Functions!
```

### <a name="type-casting-for-triggers-and-bindings"></a>触发器和绑定的类型强制转换

对于某些绑定，如 blob 绑定，可以指定参数的类型。

例如，若要将 Blob 存储中的数据作为字符串提供，请将以下类型强制转换添加`param`到我的块：

```powershell
param([string] $myBlob)
```

## <a name="powershell-profile"></a>PowerShell 配置文件

在 PowerShell 中，有一个 PowerShell 配置文件的概念。 如果你不熟悉 PowerShell 配置文件，请参阅[关于配置文件](/powershell/module/microsoft.powershell.core/about/about_profiles)。

在 PowerShell 函数中，配置文件脚本在函数应用启动时执行。 函数应用在首次部署时开始，在空闲后启动（[冷启动](#cold-start)）。

使用工具（如 Visual Studio Code 和 Azure Functions Core Tools）创建函数应用时，将为您创建`profile.ps1`一个默认值。 默认配置文件将保留[在核心工具 GitHub 存储库中](https://github.com/Azure/azure-functions-core-tools/blob/dev/src/Azure.Functions.Cli/StaticResources/profile.ps1)，并包含：

* 自动对 Azure 进行 MSI 身份验证。
* 如果需要，可以打开 Azure PowerShell `AzureRM` PowerShell 别名。

## <a name="powershell-version"></a>PowerShell 版本

下表显示了每个主要版本的函数运行时所使用的 PowerShell 版本：

| Functions 版本 | PowerShell 版本                             |
|-------------------|------------------------------------------------|
| 1.x               | Windows PowerShell 5.1 （由运行时锁定） |
| 2.x               | PowerShell Core 6                              |

可以通过从任何函数进行打印`$PSVersionTable`来查看当前版本。

## <a name="dependency-management"></a>依赖项管理

函数可让你利用[PowerShell 库](https://www.powershellgallery.com)来管理依赖项。 启用依赖关系管理后，psd1 文件将用于自动下载所需的模块。 您可以通过将`managedDependency`属性设置为，并`true`将属性设置为，[并将属性设置为，如下](functions-host-json.md)例所示：

```json
{
  "managedDependency": {
          "enabled": true
       }
}
```

创建新的 PowerShell 函数项目时，默认情况下会启用依赖项管理，其中包含 Azure [ `Az`模块](/powershell/azure/new-azureps-module-az)。 当前支持的最大模块数为10。 支持的语法为_`MajorNumber`_ `.*`或确切的模块版本，如以下要求中所示。 psd1 示例：

```powershell
@{
    Az = '1.*'
    SqlServer = '21.1.18147'
}
```

更新 psd1 文件时，将在重新启动后安装更新的模块。

> [!NOTE]
> 托管依赖项需要对 www.powershellgallery.com 的访问权限才能下载模块。 在本地运行时，请确保运行时可以通过添加任何所需的防火墙规则来访问此 URL。 

以下应用程序设置可用于更改如何下载和安装托管依赖项。 应用升级在中`MDMaxBackgroundUpgradePeriod`启动，升级过程将在大约内完成。 `MDNewSnapshotCheckPeriod`

| Function App 设置              | 默认值             | 说明                                         |
|   -----------------------------   |   -------------------     |  -----------------------------------------------    |
| **`MDMaxBackgroundUpgradePeriod`**      | `7.00:00:00`（7天）     | 每个 PowerShell 工作进程启动时对 PowerShell 库上的模块升级进行检查，每`MDMaxBackgroundUpgradePeriod`隔一次。 当 PowerShell 库中有新的模块版本时，它将安装到文件系统，并提供给 PowerShell 辅助角色。 减小此值后，函数应用可以更快地获取较新的模块版本，但它还会增加应用资源使用情况（网络 i/o、CPU、存储）。 增加此值会减少应用的资源使用情况，但也可能会延迟将新的模块版本传递给你的应用。 | 
| **`MDNewSnapshotCheckPeriod`**         | `01:00:00`（1小时）       | 将新模块版本安装到文件系统后，必须重新启动每个 PowerShell 工作进程。 重新启动 PowerShell 辅助角色会影响应用可用性，因为它可以中断当前函数执行。 在重新启动所有 PowerShell 工作进程之前，函数调用可能会使用旧的或新的模块版本。 在内`MDNewSnapshotCheckPeriod`重新启动所有 PowerShell 辅助角色。 增大此值可降低中断的频率，但也可能增加函数调用使用旧模块版本或新模块版本不确定的时间段。 |
| **`MDMinBackgroundUpgradePeriod`**      | `1.00:00:00`（1天）     | 若要避免频繁的工作线程重新启动时进行过多的模块升级，则当任何工作线程已启动最后一个`MDMinBackgroundUpgradePeriod`检查时，不会执行模块升级检查。 |

利用您自己的自定义模块与通常的方式不同。

在本地计算机上，该模块将安装在你`$env:PSModulePath`的中的一个全局可用文件夹中。 在 Azure 中运行时，无法访问计算机上安装的模块。 这意味着 PowerShell 函数`$env:PSModulePath`应用的与常规 PowerShell 脚本`$env:PSModulePath`中的不同。

在函数中`PSModulePath` ，包含两个路径：

* 位于`Modules` function app 的根目录中的文件夹。
* 由 PowerShell 语言辅助`Modules`角色控制的文件夹的路径。

### <a name="function-app-level-modules-folder"></a>函数应用级`Modules`文件夹

若要使用自定义模块，可以将函数所依赖的模块置于`Modules`文件夹中。 在此文件夹中，模块自动提供给函数运行时。 函数应用中的任何函数都可以使用这些模块。 

> [!NOTE]
> Psd1 文件中指定的模块会自动下载并包含在路径中，因此不需要将它们包含在模块文件夹中。 当在云中运行时， `$env:LOCALAPPDATA/AzureFunctions`它们存储在本地`/data/ManagedDependencies`的文件夹和文件夹中。

若要利用自定义模块功能，请在函数`Modules`应用程序的根目录中创建一个文件夹。 将要在函数中使用的模块复制到此位置。

```powershell
mkdir ./Modules
Copy-Item -Path /mymodules/mycustommodule -Destination ./Modules -Recurse
```

使用`Modules`文件夹时，函数应用应具有以下文件夹结构：

```
PSFunctionApp
 | - MyFunction
 | | - run.ps1
 | | - function.json
 | - Modules
 | | - MyCustomModule
 | | - MyOtherCustomModule
 | | - MySpecialModule.psm1
 | - local.settings.json
 | - host.json
 | - requirements.psd1
```

启动函数应用时，PowerShell 语言辅助角色会将此`Modules`文件夹添加到中`$env:PSModulePath` ，以便可以依赖于模块自动加载，就像在常规 PowerShell 脚本中一样。

### <a name="language-worker-level-modules-folder"></a>语言辅助角色`Modules`级别文件夹

PowerShell 语言辅助角色通常使用几个模块。 这些模块在的最后位置定义`PSModulePath`。 

模块的当前列表如下所示：

* 用于处理存档的[Microsoft PowerShell. Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive)： module，如`.zip`、 `.nupkg`和其他。
* **ThreadJob**：一种基于线程的 PowerShell 作业 api 实现。

默认情况下，函数使用这些模块的最新版本。 若要使用特定模块版本，请将该特定版本放置`Modules`在 function app 的文件夹中。

## <a name="environment-variables"></a>环境变量

在 Functions 中，服务连接字符串等[应用设置](functions-app-settings.md)在执行过程中将公开为环境变量。 你可以使用`$env:NAME_OF_ENV_VAR`来访问这些设置，如以下示例中所示：

```powershell
param($myTimer)

Write-Host "PowerShell timer trigger function ran! $(Get-Date)"
Write-Host $env:AzureWebJobsStorage
Write-Host $env:WEBSITE_SITE_NAME
```

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

在本地运行时，可从 [local.settings.json](functions-run-local.md#local-settings-file) 项目文件读取应用设置。

## <a name="concurrency"></a>并发

默认情况下，PowerShell 运行时一次只能处理一个函数调用。 但是，在以下情况下，此并发级别可能不足：

* 当你尝试同时处理大量调用时。
* 当您具有在同一 function app 内调用其他函数的函数时。

可以通过将以下环境变量设置为整数值来更改此行为：

```
PSWorkerInProcConcurrencyUpperBound
```

在 Function App 的 "[应用设置](functions-app-settings.md)" 中设置此环境变量。

### <a name="considerations-for-using-concurrency"></a>使用并发的注意事项

默认情况下，PowerShell 是_单线程_脚本语言。 但是，可以在同一进程中使用多个 PowerShell 运行空间来添加并发。 创建的运行空间量将与 PSWorkerInProcConcurrencyUpperBound 应用程序设置匹配。 吞吐量将受到选定计划中可用的 CPU 和内存量的影响。

Azure PowerShell 使用某些_进程级_上下文和状态，以帮助节省额外的键入内容。 但是，如果在函数应用中打开并发，并调用更改状态的操作，则可能会出现争用条件。 这些争用条件很难调试，因为一个调用依赖于某种状态，另一个调用更改了状态。

Azure PowerShell 的并发性存在巨大的价值，因为某些操作可能需要相当长的时间。 但是，必须谨慎操作。 如果怀疑遇到争用条件，请将 PSWorkerInProcConcurrencyUpperBound 应用设置设置为`1` ，并改为对并发使用[语言工作进程级隔离](functions-app-settings.md#functions_worker_process_count)。

## <a name="configure-function-scriptfile"></a>配置函数`scriptFile`

默认情况下，从`run.ps1`中执行 PowerShell 函数，该文件与对应`function.json`的父目录共享相同的父目录。

中`scriptFile`的属性`function.json`可用于获取类似于以下示例的文件夹结构：

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.ps1
```

在这种情况下`function.json` ， `myFunction`的包括`scriptFile`一个属性，该属性引用包含要运行的导出函数的文件。

```json
{
  "scriptFile": "../lib/PSFunction.ps1",
  "bindings": [
    // ...
  ]
}
```

## <a name="use-powershell-modules-by-configuring-an-entrypoint"></a>通过配置入口点来使用 PowerShell 模块

本文显示了模板生成的默认`run.ps1`脚本文件中的 PowerShell 函数。
但是，还可以在 PowerShell 模块中包含函数。 您可以使用函数 json "配置文件中的`scriptFile`和`entryPoint`字段引用模块中的特定函数代码。

在这种情况`entryPoint`下，是中`scriptFile`引用的 PowerShell 模块中的函数或 cmdlet 的名称。

请考虑以下文件夹结构：

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.psm1
```

其中`PSFunction.psm1`包含：

```powershell
function Invoke-PSTestFunc {
    param($InputBinding, $TriggerMetadata)

    Push-OutputBinding -Name OutputBinding -Value "output"
}

Export-ModuleMember -Function "Invoke-PSTestFunc"
```

在此示例中，的配置`myFunction`包括引用`scriptFile` `PSFunction.psm1`的属性，该属性是另一个文件夹中的 PowerShell 模块。  `entryPoint`属性引用`Invoke-PSTestFunc`函数，该函数是模块中的入口点。

```json
{
  "scriptFile": "../lib/PSFunction.psm1",
  "entryPoint": "Invoke-PSTestFunc",
  "bindings": [
    // ...
  ]
}
```

使用此配置时， `Invoke-PSTestFunc`将完全按原样`run.ps1`执行。

## <a name="considerations-for-powershell-functions"></a>PowerShell 函数的注意事项

使用 PowerShell 函数时，请注意以下部分中的注意事项。

### <a name="cold-start"></a>冷启动

在[无服务器托管模型](functions-scale.md#consumption-plan)中开发 Azure Functions 时，冷启动就会成为现实。 *冷启动*是指函数应用开始运行以处理请求所用的时间段。 在消耗计划中，冷启动会更频繁，因为函数应用在非活动期间关闭。

### <a name="bundle-modules-instead-of-using-install-module"></a>绑定模块，而不是使用`Install-Module`

脚本将在每次调用时运行。 避免在`Install-Module`脚本中使用。 改为`Save-Module`在发布前使用，以便您的函数无需浪费时间来下载模块。 如果冷启动会影响你的功能，请考虑将 function app 部署到[应用服务计划](functions-scale.md#app-service-plan)，将其设置为*Always on*或[高级计划](functions-scale.md#premium-plan)。

## <a name="next-steps"></a>后续步骤

有关更多信息，请参见以下资源：

* [Azure Functions 最佳做法](functions-best-practices.md)
* [Azure Functions developer reference（Azure Functions 开发人员参考）](functions-reference.md)
* [Azure Functions 触发器和绑定](functions-triggers-bindings.md)

[host.json 引用]: functions-host-json.md
