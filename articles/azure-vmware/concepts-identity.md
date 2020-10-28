---
title: 概念-标识和访问
description: 了解 Azure VMware 解决方案的标识和访问概念
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 7127109801d92d2177f6edac3efcaf76ddf217e6
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92674641"
---
# <a name="azure-vmware-solution-identity-concepts"></a>Azure VMware 解决方案标识概念

部署私有云时，将设置 vCenter 服务器和 NSX-T 管理器。 你使用 vCenter 来管理虚拟机 (VM) 工作负荷。 你使用 NSX-T 管理器来扩展私有云软件定义的网络。

访问和身份管理使用 CloudAdmin 组权限，以便对 NSX-T 管理器使用 vCenter 和受限的管理员权限。 它确保私有云平台会自动升级到最新的功能和修补程序。  有关详细信息，请参阅 [私有云升级概念一文][concepts-upgrades]。

## <a name="vcenter-access-and-identity"></a>vCenter 访问和标识

VCenter 中的权限通过 CloudAdmin 组提供。 该组可以在 vCenter 本地管理，也可以通过与 Azure Active Directory 集成 vCenter LDAP 单一登录。 你能够在部署私有云后启用该集成。

下表显示了 CloudAdmin 和 CloudGlobalAdmin 权限。

|  权限集           | CloudAdmin | CloudGlobalAdmin | 注释 |
| :---                     |    :---:   |       :---:      |   :--:  |
|  警报                  | CloudAdmin 用户对 Compute-ResourcePool 和 Vm 中的警报具有所有告警特权。     |          --        |  -- |
|  自动部署             |  --  |        --        |  Microsoft 进行主机管理。  |
|  证书            |  --  |        --       |  Microsoft 进行证书管理。  |
|  内容库         | CloudAdmin 用户有权创建和使用内容库中的文件。    |         已通过 SSO 启用。         |  Microsoft 会将内容库中的文件分发到 ESXi 的主机。  |
|  数据中心              |  --  |        --          |  Microsoft 执行所有数据中心操作。  |
|  数据存储               | AllocateSpace、Datastore.Config、DeleteFile、FileManagement、、、UpdateVirtualMachineMetadata     |    --    |   -- |
|  ESX 代理程序管理器       |  --  |         --       |  Microsoft 执行所有操作。  |
|  Folder                  |  CloudAdmin 用户具有所有文件夹特权。     |  --  |  --  |
|  全球                  |  CancelTask、GlobalTag、global. LogEvent、global. ManageCustomFields、ServiceManagers、SetCustomField、temTag、Global.Sys         |                  |    |
|  主机                    |  Cdb-ik-hbr. HbrManagement      |        --          |  Microsoft 执行所有其他主机操作。  |
|  InventoryService        |  InventoryService 标记      |        --          |  --  |
|  网络                 |  Network.Assign    |                  |  Microsoft 执行所有其他网络操作。  |
|  权限             |  --  |        --       |  Microsoft 执行所有权限操作。  |
|  配置文件驱动的存储  |  --  |        --       |  Microsoft 执行所有配置文件操作。  |
|  资源                |  CloudAdmin 用户具有所有资源权限。        |      --       | --   |
|  计划任务          |  CloudAdmin 用户具有所有 ScheduleTask 权限。   |   --   | -- |
|  会话                |  GlobalMessage、ValidateSession      |   --   |  Microsoft 执行所有其他会话操作。  |
|  存储视图           |  StorageViews   |        --          |  Microsoft 执行所有其他存储视图操作 (配置服务) 。  |
|  任务                   |  --  |  --   |  Microsoft 管理管理任务的扩展。  |
|  vApp                    |  CloudAdmin 用户具有所有 vApp 权限。  |  --  |  --  |
|  虚拟机         |  CloudAdmin 用户具有所有 VirtualMachine 权限。  |  --  |  --  |
|  vService                |  CloudAdmin 用户具有所有 vService 权限。  |  --  |  --  |

## <a name="nsx-t-manager-access-and-identity"></a>NSX-T 管理器访问和标识

使用 "管理员" 帐户访问 NSX-T 管理器。 它具有完全权限，可让你创建和管理 T1 路由器、逻辑交换机和所有服务。 权限使你可以访问 NSX-T T0 路由器。 更改 T0 路由器可能导致网络性能下降或没有私有云访问。 在 Azure 门户中打开支持请求，请求对 NSX-T T0 路由器进行任何更改。
  
## <a name="next-steps"></a>后续步骤

下一步是了解 [私有云升级概念][concepts-upgrades]。

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-upgrades]: ./concepts-upgrades.md