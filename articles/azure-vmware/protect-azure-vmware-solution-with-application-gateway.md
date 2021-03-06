---
title: 使用 Azure 应用程序网关保护 Azure VMware 解决方案上的 web 应用
description: 配置 Azure 应用程序网关，以安全地公开在 Azure VMware 解决方案上运行的 web 应用。
ms.topic: how-to
ms.date: 11/13/2020
ms.openlocfilehash: 02e439989c985354dbe06fa3e231d5daf7099d70
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2020
ms.locfileid: "94628957"
---
# <a name="use-azure-application-gateway-to-protect-your-web-apps-on-azure-vmware-solution"></a>使用 Azure 应用程序网关保护 Azure VMware 解决方案上的 web 应用

[Azure 应用程序网关](https://azure.microsoft.com/services/application-gateway/) 是第7层 web 流量负载均衡器，可用于管理 web 应用程序的流量。 它在 Azure VMware 解决方案1.0 版和 v2.0 版中提供。 这两个版本都是通过在 Azure VMware 解决方案上运行的 web 应用进行测试的。

功能包括： 
- 基于 Cookie 的会话关联
- 基于 URL 的路由
- Web 应用程序防火墙 (WAF)

有关功能的完整列表，请参阅 [Azure 应用程序网关功能](../application-gateway/features.md)。 

本文介绍如何在 web 服务器场前面使用应用程序网关来保护在 Azure VMware 解决方案上运行的 web 应用。 

## <a name="topology"></a>拓扑
此图显示了如何使用应用程序网关来保护 (Vm) 、Azure 虚拟机规模集或本地服务器的 Azure IaaS 虚拟机。 应用程序网关将 Azure VMware 解决方案 Vm 视为本地服务器。 

![此图显示了应用程序网关如何保护 Azure IaaS 虚拟机 (Vm) 、Azure 虚拟机规模集或本地服务器。](media/protect-azure-vmware-solution-with-application-gateway/app-gateway-protects.png)

> [!IMPORTANT]
> Azure 应用程序网关是目前唯一受支持的方法来公开在 Azure VMware 解决方案 Vm 上运行的 web 应用。

此图显示了用于通过 Azure VMware 解决方案 web 应用程序验证应用程序网关的测试方案。

:::image type="content" source="media/hub-spoke/azure-vmware-solution-second-level-traffic-segmentation.png" alt-text="显示用于通过 Azure VMware 解决方案 web 应用程序验证应用程序网关的测试方案的关系图。" border="false":::

应用程序网关实例部署在中心的专用子网中。 它具有 Azure 公共 IP 地址。 建议为虚拟网络激活标准 DDoS 保护。 Web 服务器托管在 NSX T0 和 T1 路由器后面的 Azure VMware 解决方案私有云上。 Azure VMware 解决方案使用 [ExpressRoute Global Reach](../expressroute/expressroute-global-reach.md) 来实现与集线器和本地系统的通信。

## <a name="prerequisites"></a>先决条件

- 具有活动订阅的 Azure 帐户。
- 部署并运行的 Azure VMware 解决方案私有云。

## <a name="deployment-and-configuration"></a>部署和配置

1. 在 Azure 门户中，搜索 " **应用程序网关** "，然后选择 " **创建应用程序网关** "。

2. 提供基本详细信息，如下图所示：然后选择 " **下一步：前端>** 。 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/create-app-gateway.png" alt-text="显示 Azure 门户中的 &quot;创建应用程序网关&quot; 页的屏幕截图。":::

3. 选择前端 IP 地址类型。 对于 "公用"，请选择现有的公共 IP 地址，或创建一个新的 IP 地址。 选择 **下一步：后端>** 。

    > [!NOTE]
    > 专用前端仅支持标准和 Web 应用程序防火墙 (WAF) Sku。

4. 添加在 Azure VMware 解决方案基础结构上运行的 Vm 的后端池。 提供在 Azure VMware 解决方案私有云上运行的 web 服务器的详细信息，并选择 " **添加** "。  然后选择 " **下一步：配置>** 。

1. 在 " **配置** " 选项卡上，选择 " **添加路由规则** "。

6. 在 " **侦听器** " 选项卡上，提供侦听器的详细信息。 如果选择了 HTTPS，则必须从 PFX 文件或现有 Azure Key Vault 证书提供证书。 

7. 选择 " **后端目标** " 选项卡，然后选择以前创建的后端池。 对于 " **HTTP 设置** " 字段，请选择 " **新增** "。

8. 配置 HTTP 设置的参数。 选择 **添加** 。

9. 如果要配置基于路径的规则，请选择 " **添加多个目标" 以创建基于路径的规则** 。 

10. 添加基于路径的规则，然后选择 " **添加** "。 重复此步骤以添加其他基于路径的规则。 

11. 添加完基于路径的规则后，请再次选择 " **添加** ";然后选择 " **下一步：标记">** 。 

12. 添加标记，然后选择 " **下一步"：查看 + 创建>** 。

13. 验证将在应用程序网关上运行;如果成功，请选择 " **创建** " 以进行部署。

## <a name="configuration-examples"></a>配置示例

在本部分中，你将了解如何将应用程序网关配置为 Azure VMware 解决方案 Vm，作为这些用例的后端池： 

- [托管多个站点](#hosting-multiple-sites)
- [按 URL 路由](#routing-by-url)

### <a name="hosting-multiple-sites"></a>托管多个站点

此过程说明如何使用现有应用程序网关上的 Azure VMware 解决方案私有云上运行的 Vm 定义后端地址池。 

>[!NOTE]
>此过程假定你有多个域，因此我们将使用 www.contoso.com 和 www.fabrikam.com 的示例。


1. 在私有云中，创建两个不同的 Vm 池。 一个代表 Contoso 和第二个 Fabrikam。 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool.png" alt-text="显示 VSphere 客户端中 web 服务器详细信息摘要的屏幕截图。":::

    我们使用了 Windows Server 2016，其中安装了 Internet Information Services (IIS) 角色来说明本教程。 安装 Vm 后，运行以下 PowerShell 命令，在每个 Vm 上配置 IIS。 

    ```powershell
    Install-WindowsFeature -Name Web-Server
    Add-Content -Path C:\inetpub\wwwroot\Default.htm -Value $($env:computername)
    ```

2. 在现有应用程序网关实例中，从左侧菜单中选择 " **后端池** "，选择 "  **添加** "，然后输入新的池的详细信息。 在右窗格中选择 " **添加** "。

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-02.png" alt-text="用于添加后端池的 &quot;后端池&quot; 页的屏幕截图。" lightbox="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-02.png":::

3. 在 " **侦听器** " 部分中，为每个网站创建一个新的侦听器。 输入每个侦听器的详细信息，然后选择 " **添加** "。

4. 在左侧，选择 " **HTTP 设置** "，并在左窗格中选择 " **添加** "。 填写详细信息以创建新的 HTTP 设置，然后选择 " **保存** "。

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-03.png" alt-text="&quot;HTTP 设置&quot; 页的屏幕截图，用于创建新的 HTTP 设置。" lightbox="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-03.png":::

5. 在左侧菜单的 " **规则** " 部分中创建规则。 将每个规则与相应的侦听器相关联。 选择 **添加** 。

6. 配置相应的后端池和 HTTP 设置。 选择 **添加** 。

7. 测试连接。 打开首选浏览器并导航到 Azure VMware 解决方案环境中托管的不同网站，例如 http://www.fabrikam.com 。

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-multi-backend-pool-07.png" alt-text="显示成功测试连接的浏览器页的屏幕截图。":::

### <a name="routing-by-url"></a>按 URL 路由

此过程说明如何使用现有应用程序网关上的 Azure VMware 解决方案私有云上运行的 Vm 定义后端地址池。 然后创建路由规则，以确保 Web 流量到达池中的相应服务器。

1. 在私有云中，创建虚拟机池来表示 web 场。 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool.png" alt-text="VMSphere 客户端中页的屏幕截图，显示了另一个 VM 的摘要。":::

    已使用安装了 IIS 的 Windows Server 2016 角色来说明此教程。 安装 Vm 后，运行以下 PowerShell 命令，为每个 VM 配置 IIS 教程。 

    第一个虚拟机 contoso-01，将承载主网站。

    ```powershell
    Install-WindowsFeature -Name Web-Server
    Add-Content -Path C:\inetpub\wwwroot\Default.htm -Value $($env:computername)
    ```

    第二个虚拟机 contoso-web-02 将托管映像站点。
 
    ```powershell
    Install-WindowsFeature -Name Web-Server
    New-Item -Path "C:\inetpub\wwwroot\" -Name "images" -ItemType "directory"
    Add-Content -Path C:\inetpub\wwwroot\images\test.htm -Value $($env:computername)
    ```

    第三个虚拟机 contoso-03，将托管视频站点。

    ```powershell
    Install-WindowsFeature -Name Web-Server
    New-Item -Path "C:\inetpub\wwwroot\" -Name "video" -ItemType "directory"
    Add-Content -Path C:\inetpub\wwwroot\video\test.htm -Value $($env:computername)
    ```

2. 在现有应用程序网关实例中添加三个新的后端池。 

   1. 从左侧菜单中选择“后端池”。 
   1. 选择 " **添加** "，然后输入第一个池（ **contoso-web** ）的详细信息。 
   1. 将一个 VM 添加为目标。 
   1. 选择 **添加** 。 
   1. 为 **contoso-images** 和 **contoso-视频** 重复此过程，并将一个唯一的 VM 添加为目标。 

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-02.png" alt-text="后端池页的屏幕截图，显示添加了三个新的后端池。" lightbox="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-02.png":::

3. 在 " **侦听器** " 部分中，使用端口8080创建一个类型为 "基本" 的新侦听器。

4. 在左侧导航栏中，选择 " **HTTP 设置** "，并在左窗格中选择 " **添加** "。 填写详细信息以创建新的 HTTP 设置，然后选择 " **保存** "。

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-04.png" alt-text="显示 HTTP 设置配置的 &quot;添加 HTTP 设置&quot; 页的屏幕截图。":::

5. 在左侧菜单的 " **规则** " 部分中创建规则。 将每个规则与以前创建的侦听器相关联。 然后配置主后端池和 HTTP 设置。 选择 **添加** 。

    :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-07.png" alt-text="&quot;添加路由规则&quot; 页的屏幕截图，用于配置到后端目标的路由规则。":::

6. 测试配置。 访问 Azure 门户上的应用程序网关，并复制 " **概述** " 部分中的 "公共 IP 地址"。 

   1. 打开新的浏览器窗口并输入 URL `http://<app-gw-ip-address>:8080` 。 

      :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-08.png" alt-text="显示成功测试配置的浏览器页的屏幕截图。":::

   1. 将 URL 更改为 `http://<app-gw-ip-address>:8080/images/test.htm`。

      :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-09.png" alt-text="具有新 URL 的另一个成功测试的屏幕截图。":::

   1. 再次将 URL 更改为 `http://<app-gw-ip-address>:8080/video/test.htm` 。

      :::image type="content" source="media/protect-azure-vmware-solution-with-application-gateway/app-gateway-url-route-backend-pool-10.png" alt-text="带有最终 URL 的成功测试的屏幕截图。":::

## <a name="next-steps"></a>后续步骤

有关更多配置示例，请查看 [Azure 应用程序网关文档](../application-gateway/index.yml) 。