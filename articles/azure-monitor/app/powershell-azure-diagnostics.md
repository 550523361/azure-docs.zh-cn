---
title: 使用 PowerShell 在 Azure 中设置 Application Insights | Microsoft Docs
description: 自动配置 Azure 诊断，通过管道将数据发送到 Application Insights。
ms.topic: conceptual
ms.date: 08/06/2019
ms.openlocfilehash: da1796c8af5b9463d8223615f4b0629ba65eb3e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669797"
---
# <a name="using-powershell-to-set-up-application-insights-for-azure-cloud-services"></a>使用 PowerShell 为 Azure 云服务设置 Application Insights

可以将 [Microsoft Azure](https://azure.com) 配置为向 [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md)[发送 Azure 诊断](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md)。 该诊断与 Azure Cloud Service 和 Azure VM 有关。 它们是对使用 Application Insights SDK 从应用内发送的遥测的补充。 作为在 Azure 中自动处理新建资源过程的一部分，可以使用 PowerShell 配置诊断。

## <a name="azure-template"></a>Azure 模板
如果 Web 应用在 Azure 中，并且使用 Azure 资源管理器模板创建资源，可以通过将以下内容添加到资源节点来配置 Application Insights：

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource` - Application Insights 资源的名称
* `myWebAppName` - Web 应用的 ID

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>在部署云服务过程中启用诊断扩展
`New-AzureDeployment` cmdlet 具有参数 `ExtensionConfiguration`，它会接收一组诊断配置。 可使用 `New-AzureServiceDiagnosticsExtensionConfig` cmdlet 创建这些诊断配置。 例如：

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>在现有的云服务上启用诊断扩展
在现有服务上，使用 `Set-AzureServiceDiagnosticsExtension`。

```ps

    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>获取当前诊断扩展配置
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>删除诊断扩展
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

如果已使用不带 Role 参数的 `Set-AzureServiceDiagnosticsExtension` 或 `New-AzureServiceDiagnosticsExtensionConfig` 启用诊断扩展，则可以使用不带 Role 参数的 `Remove-AzureServiceDiagnosticsExtension` 删除该扩展。 如果启用扩展时使用了 Role 参数，则删除扩展时也必须使用该参数。

若要从单个角色中删除诊断扩展，请使用以下命令：

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>请参阅
* [使用 Application Insights 监视 Azure Cloud Service 应用](../../azure-monitor/app/cloudservices.md)
* [将 Azure 诊断发送到应用程序见解](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [自动配置警报](powershell-alerts.md)

