---
title: 在独立 Service Fabric 群集中添加或删除节点
description: 了解如何向运行 Windows Server 的本地或任意云中物理计算机或虚拟机上的 Azure Service Fabric 群集添加节点。
author: dkkapur
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: dekapur
ms.openlocfilehash: f9bee35ee8e82070b4cf601139b471562ba5e10b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934213"
---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>向在 Windows Server 上运行的独立 Service Fabric 群集添加或删除节点
[在 Windows Server 计算机上创建独立 Service Fabric 群集](service-fabric-cluster-creation-for-windows-server.md)之后，（业务）需求可能发生变化，因此需要向群集添加或删除节点。 本文提供了实现此目标的详细步骤。 请注意，在本地开发群集中不支持添加/删除节点的功能。

## <a name="add-nodes-to-your-cluster"></a>向群集添加节点

1. 按照"计划"中概述的步骤准备要添加到群集的 VM/计算机[，并准备 Service Fabric 群集部署](service-fabric-cluster-standalone-deployment-preparation.md)。

2. 标识要将此 VM/计算机添加到的容错域和升级域。

   如果使用证书来保护群集，则证书应安装在本地证书存储中，为节点加入群集做准备。 使用其他形式的安全时，模拟应用。

3. 通过远程桌面 (RDP) 方式进入需要向群集添加的 VM/计算机。

4. 将[Windows 服务器服务结构的独立包](https://go.microsoft.com/fwlink/?LinkId=730690)复制到 VM/计算机，然后解压缩包。

5. 使用提升的权限运行 PowerShell，然后转到解压缩包的位置。

6. 使用描述要添加的新节点的参数运行 AddNode.ps1** 脚本。 下面的示例将一个名为 VM5 的新节点添加到 UD1 和 fd：/dc1/r0 中，该节点的类型为 NodeType0 和 IP 地址 182.17.34.52。 `ExistingClusterConnectionEndPoint`是现有群集中已有节点的连接终结点，可以是群集中*任何*节点的 IP 地址。 

   不安全（原型设计）：

   ```
   .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
   ```

   安全（基于证书）：

   ```  
   $CertThumbprint= "***********************"
    
   .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -X509Credential -ServerCertThumbprint $CertThumbprint  -AcceptEULA

   ```

   当脚本完成运行时，可以通过运行[获取服务FabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet来检查新节点是否已添加。

7. 要确保群集中不同节点的一致性，请务必启动配置升级。 运行[获取服务FabricCluster配置](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps)以获取最新的配置文件，并将新添加的节点添加到"节点"部分。 还建议在需要重新部署具有相同配置的群集时始终提供最新的群集配置。

   ```
    {
        "nodeName": "vm5",
        "iPAddress": "182.17.34.52",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD1"
    }
   ```

8. 请运行 [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) 开始升级。

   ```
   Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
   ```

   可在 Service Fabric Explorer 中监视升级进度。 或者，您可以运行[获取服务Fabric集群升级](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)。

### <a name="add-nodes-to-clusters-configured-with-windows-security-using-gmsa"></a>将节点添加到使用 gMSA 配置 Windows 安全性的群集中。
对于使用组托管服务帐户 (gMSA)(https://technet.microsoft.com/library/hh831782.aspx) 配置的群集，可以使用配置升级添加新的节点：
1. 在现有的任意节点上运行 [Get ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps)，获取最新的配置文件，并在“节点”部分添加想要添加的新节点相关详细信息。 请确保新节点属于同一组合托管帐户。 此帐户应当是所有计算机的管理员。

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. 请运行 [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) 开始升级。

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
    ```
    可在 Service Fabric Explorer 中监视升级进度。 或者，也可运行 [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-node-types-to-your-cluster"></a>向群集添加节点类型
为了添加新的节点类型，请修改配置，将新节点类型包含在“属性”下的“节点类型”部分，并使用 [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) 开始配置升级。 升级完成后，可以使用此节点类型将新节点添加到群集。

## <a name="remove-nodes-from-your-cluster"></a>从群集中删除节点
可以采用以下方式通过配置升级将节点从群集中删除：

1. 运行 [Get ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) 获取最新配置文件，并将节点从“节点”部分删除**。
将 "NodesToBeRemoved" 参数添加到 "FabricSettings" 中的“设置”部分。 “值”应当是一个逗号分隔列表，其中包含需要删除的节点的节点名称。

    ```
         "fabricSettings": [
            {
            "name": "Setup",
            "parameters": [
                {
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
                },
                {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
                },
                {
                "name": "NodesToBeRemoved",
                "value": "vm0, vm1"
                }
            ]
            }
        ]
    ```
2. 请运行 [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) 开始升级。

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    可在 Service Fabric Explorer 中监视升级进度。 或者，您可以运行[获取服务Fabric集群升级](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)。

> [!NOTE]
> 删除可能启动多个升级的节点。 某些节点标有 `IsSeedNode=”true”` 标记，可使用 `Get-ServiceFabricClusterManifest` 通过查询群集清单进行标识。 在此类方案中，因为种子节点需要移动，因此删除此类节点相比其他节点需要较长时间。 群集必须至少维护 3 个主节点类型的节点。
> 
> 

### <a name="remove-node-types-from-your-cluster"></a>从群集中删除节点类型
删除节点类型前，请仔细检查是否存在任何引用该节点类型的节点。 请在删除相应节点类型之前删除这些节点。 删除所有相应节点后，可从群集配置中删除节点类型并使用 [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) 开始配置升级。


### <a name="replace-primary-nodes-of-your-cluster"></a>替换群集的主节点
应逐个节点地替换主节点，而不是批量删除再添加。


## <a name="next-steps"></a>后续步骤
* [Windows 独立群集的配置设置](service-fabric-cluster-manifest.md)
* [使用 X509 证书保护 Windows 上的独立群集](service-fabric-windows-cluster-x509-security.md)
* [使用运行 Windows 的 Azure VM 创建独立 Service Fabric 群集](service-fabric-cluster-creation-with-windows-azure-vms.md)

