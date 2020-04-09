---
title: Azure 安全中心中的实时虚拟机访问 | Microsoft Docs
description: 本文档说明 Azure 安全中心中的实时 VM 访问如何帮助你控制对 Azure 虚拟机的访问。
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: cc4e267c6912b8938db1ba5497a27f9c0026bd79
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887327"
---
# <a name="secure-your-management-ports-with-just-in-time-access"></a>通过及时访问保护管理端口

如果您位于安全中心的标准定价层（请参阅[定价](/azure/security-center/security-center-pricing)），则可以使用及时 （JIT） 虚拟机 （VM） 访问权限锁定 Azure VM 的入站流量。 这减少了受到攻击的风险，同时提供了在需要时轻松连接到 VM 的访问。

> [!NOTE]
> 安全中心实时 VM 访问当前仅支持通过 Azure 资源管理器部署的 VM。 若要了解有关经典部署模型和资源管理器部署模型的详细信息，请参阅 [Azure 资源管理器与经典部署](../azure-resource-manager/management/deployment-models.md)。

[!INCLUDE [security-center-jit-description](../../includes/security-center-jit-description.md)]

## <a name="configure-jit-on-a-vm"></a>在 VM 上配置 JIT

可通过三种方式在 VM 上配置 JIT 策略：

- [在 Azure 安全中心配置 JIT 访问](#jit-asc)
- [在 Azure VM 页中配置 JIT 访问](#jit-vm)
- [以编程方式在 VM 上配置 JIT 策略](#jit-program)

## <a name="configure-jit-in-azure-security-center"></a>在 Azure 安全中心配置 JIT

在安全中心，可以配置 JIT 策略并使用 JIT 策略请求访问 VM

### <a name="configure-jit-access-on-a-vm-in-security-center"></a>在安全中心的 VM 上配置 JIT 访问 <a name="jit-asc"></a>

1. 打开“安全中心”**** 仪表板。

1. 在左侧窗格中，选择“实时 VM 访问”****。

    ![实时 VM 访问磁贴](./media/security-center-just-in-time/just-in-time.png)

    "**实时 VM 访问**"窗口将打开并显示有关 VM 状态的信息：

    - **已配置** - 已配置为支持实时 VM 访问的 VM。 提供的数据是针对过去一周的，并且针对每个 VM 包括了已批准的请求数、上次访问日期和时间以及上一个用户。
    - **推荐** - 可以支持实时 VM 访问但尚未配置此功能的 VM。 建议为这些 VM 启用实时 VM 访问控制。
    - **不推荐** - 导致不推荐某个 VM 的可能原因有：
      - 缺少 NSG - 实时解决方案需要 NSG 准备就绪。
      - 经典 VM - 安全中心实时 VM 访问当前仅支持通过 Azure 资源管理器部署的 VM。 实时解决方案不支持经典部署。 
      - 其他 - 如果在订阅或资源组的安全策略中未开启实时解决方案，或者 VM 缺少公共 IP 且没有已准备就绪的 NSG，则该 VM 将位于此类别中。

1. 选择“建议”选项卡。****

1. 在“虚拟机”**** 下，单击要启用的 VM。 这会在 VM 旁边放置一个复选标记。

      ![启用实时访问](./media/security-center-just-in-time/enable-just-in-time.png)

1. 单击“在 VM 上启用 JIT”。**** 将打开一个窗格，显示 Azure 安全中心建议的默认端口：
    - 22 - SSH
    - 3389 - RDP
    - 5985 - WinRM 
    - 5986 - WinRM
1. 或者，您可以将自定义端口添加到列表中：

      1. 单击 **添加**。 此时会打开“添加端口配置”窗口****。
      1. 对于选择配置的每个端口，无论是默认端口还是自定义端口，都可以自定义下列设置：
            - **协议类型** - 批准某个请求时，此端口允许的协议。
            - **允许的源 IP 地址** - 批准某个请求时，此端口允许的 IP 范围。
            - **最大请求时间** - 可以打开特定端口的最大时间范围。

     1. 单击 **“确定”** 。

1. 单击“ **保存**”。

> [!NOTE]
>如果为 VM 启用 JIT VM 访问，Azure 安全中心将在与所选端口关联的网络安全组和 Azure 防火墙中为该端口创建“拒绝所有入站流量”规则。 如果为所选端口创建了其他规则，则现有规则优先于新的"拒绝所有入站流量"规则。 如果所选端口上没有现有规则，则新的"拒绝所有入站流量"规则将在网络安全组和 Azure 防火墙中占据最高优先级。


## <a name="request-jit-access-via-security-center"></a>通过安全中心请求 JIT 访问

若要通过安全中心请求访问 VM，请执行以下操作：

1. 在“实时 VM 访问”下，选择“已配置”选项卡********。

1. 在“虚拟机”**** 下，单击要请求访问的 VM。 这会勾选该 VM。

    - “连接详细信息”列中的图标指示是在 NSG 还是 FW 中启用了 JIT****。 如果两者都启用了，则仅显示防火墙图标。

    - “连接详细信息”列提供连接 VM 所需的信息，及其打开的端口****。

      ![请求实时访问](./media/security-center-just-in-time/request-just-in-time-access.png)

1. 单击“请求访问”。**** 此时会打开“请求访问”窗口。****

      ![JIT 详细信息](./media/security-center-just-in-time/just-in-time-details.png)

1. 在“请求访问”下，为每个 VM 配置要打开的端口、要为其打开该端口的源 IP 地址以及将打开该端口的时间范围****。 只能请求访问在实时策略中配置的端口。 每个端口都有一个从实时策略派生的最大允许时间。

1. 单击“打开端口”。****

> [!NOTE]
> 如果请求访问的用户使用代理，则“我的 IP”选项可能无法使用****。 可能需要定义组织的完整 IP 地址范围。



## <a name="edit-a-jit-access-policy-via-security-center"></a>通过安全中心编辑 JIT 访问策略

可以对 VM 的现有实时策略进行以下更改：添加并配置要保护该 VM 的新端口，或更改与已保护的端口相关的任何其他设置。

编辑现有的 VM 实时策略：

1. 在“已配置”选项卡中，在“VM”下，通过单击 VM 对应的行内的三个点来选择要为其添加端口的 VM********。 

1. 选择 **"编辑**"。

1. 在“JIT VM 访问配置”下，可以编辑已保护的端口的现有设置，也可以添加新的自定义端口****。 
  ![jit vm 访问](./media/security-center-just-in-time/edit-policy.png)



## <a name="audit-jit-access-activity-in-security-center"></a>审核安全中心的 JIT 访问活动

可以使用日志搜索深入了解 VM 活动。 若要查看日志，请执行以下操作：

1. 在“实时 VM 访问”下，选择“已配置”选项卡********。
2. 在**VM**下，通过单击该 VM 行中的三个点来查看有关的信息，并从菜单中选择 **"活动日志**"。 此时会打开“活动日志”。****

   ![选择“活动日志”](./media/security-center-just-in-time/select-activity-log.png)

   “活动日志”**** 提供了该 VM 的以前操作的经筛选视图以及时间、日期和订阅。

可以通过选择“单击此处将所有项下载为 CSV”来下载日志信息。****

修改筛选器并单击“应用”来创建搜索和日志。****



## <a name="configure-jit-access-from-an-azure-vms-page"></a>在 Azure VM 页配置 JIT 访问 <a name="jit-vm"></a>

为方便起见，可以使用 JIT 直接从安全中心的 VM 页连接到 VM。

### <a name="configure-jit-access-on-a-vm-via-the-azure-vm-page"></a>通过 Azure VM 页在 VM 上配置 JIT 访问

若要轻松地在 VM 间进行实时访问，可以将 VM 设置为仅允许从 VM 内直接进行实时访问。

1. 在 [Azure 门户](https://ms.portal.azure.com)中，搜索并选择“虚拟机”****。 
2. 选择要实行实时访问限制的虚拟机。
3. 在菜单中选择“配置”****。
4. 在"及时**访问**"下，选择 **"及时启用**"。 

此操作可为使用以下设置的 VM 启用实时访问：

- Windows 服务器：
    - RDP 端口 3389
    - 允许的最长访问时间为三小时
    - 允许的源 IP 地址设置为“任何”
- Linux 服务器：
    - SSH 端口 22
    - 允许的最长访问时间为三小时
    - 允许的源 IP 地址设置为“任何”
     
如果 VM 已启用实时访问，则在转到其配置页时，将看到实时访问已启用。可使用链接打开 Azure 安全中心中的策略，以便查看和更改设置。

![VM 中的 JIT 配置](./media/security-center-just-in-time/jit-vm-config.png)

### <a name="request-jit-access-to-a-vm-via-an-azure-vms-page"></a>通过 Azure VM 的页面请求对 VM 的 JIT 访问权限

在 Azure 门户中，尝试连接到 VM 时，Azure 会检查是否在该 VM 上配置实时访问策略。

- 如果在 VM 上配置了 JIT 策略，则可以单击"**请求访问权限**"，根据为 VM 设置的 JIT 策略授予访问权限。 

  >![jit 请求](./media/security-center-just-in-time/jit-request.png)

  使用以下默认参数请求访问：

  - **源 IP**： "任何"（*） （无法更改）
  - **时间范围**：三小时（无法更改） <!--Isn't this set in the policy-->
  - **端口号**：在 Windows 中为 RDP 端口 3389/在 Linux 中为端口 22（可更改）

    > [!NOTE]
    > 批准对受 Azure 防火墙保护的 VM 的请求后，安全中心将为用户提供正确的连接详细信息（来自 DNAT 表的端口映射）用于连接 VM。

- 如果未在 VM 上配置 JIT，系统将提示您配置其上的 JIT 策略。

  ![jit 提示](./media/security-center-just-in-time/jit-prompt.png)

## <a name="configure-a-jit-policy-on-a-vm-programmatically"></a>以编程方式在 VM 上配置 JIT 策略 <a name="jit-program"></a>

可通过 REST API 和 PowerShell 设置和使用实时功能。

### <a name="jit-vm-access-via-rest-apis"></a>通过 REST API 对 VM 进行 JIT 访问

通过 Azure 安全中心 API 可使用实时 VM 访问功能。 可以通过此 API 获取有关配置 VM 的信息、添加新的 VM、请求访问 VM 等。 要详细了解实时 REST API，请参阅 [Jit Network Access Policies](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies)（JIT网络访问策略）。

### <a name="jit-vm-access-via-powershell"></a>通过 PowerShell 对 VM 进行 JIT 访问

要通过 PowerShell 使用实时 VM 访问解决方案，请使用正式的 Azure 安全中心 PowerShell cmdlet，具体为 `Set-AzJitNetworkAccessPolicy`。

下面的示例可对特定 VM 设置实时 VM 访问策略，并设置以下各项：

1.    关闭端口 22 和 3389。

2.    将每个的最大时间窗口设置为 3 小时，使它们能够按每个批准的请求打开。
3.    允许请求访问的用户控制源 IP 地址，允许用户对批准的实时访问请求建立成功会话。

在 PowerShell 中运行以下命令实现此目的：

1.    分配变量，保存 VM 的实时 VM 访问策略：

        $JitPolicy = （*id="/订阅/订阅 ID/资源组/资源组/资源组/提供商/微软.计算/虚拟机/VMNAME"端口=（{编号=22;       协议\";\*       允许的Source地址前缀\（""）;\*       最大请求访问持续时间="PT3H"*，[数字=3389;       协议\";\*       允许的Source地址前缀\（""）;\*       最大请求访问持续时间="PT3H"*））

2.    将 VM 实时 VM 访问策略插入数组：
    
        $JitPolicyArr_（$JitPolicy）

3.    对所选 VM 配置实时 VM 访问策略：
    
        设置-AzJitNetwork 访问策略 - 金德"基本" - 位置"位置" - 名称"默认" - 资源组名称"资源组" -虚拟机$JitPolicyArr 

### <a name="request-access-to-a-vm-via-powershell"></a>通过 PowerShell 请求访问 VM

在以下示例中，可以看到对特定 VM 的实时 VM 访问请求，其中请求端口 22 为特定 IP 地址打开，并持续特定时间：

在 PowerShell 中运行以下命令：
1.    配置 VM 请求访问属性

        $JitPolicyVm1 = （*id="/订阅 ID/资源组/资源组/资源组/提供商/微软.计算/虚拟机/VMNAME"端口=（*编号=22;     endTimeUtc="2018-09-17T17：00.3658798Z";     允许来源地址前缀*（"IPV4ADDRESS"）*）
2.    在数组中插入 VM 访问请求参数：

        $JitPolicyArr=（$JitPolicyVm1）
3.    发送请求访问权限（使用步骤 1 中获取的资源 ID）

        启动-AzJit网络访问策略 - 资源 Id"/订阅/订阅 ID/资源组/资源组/资源组/提供商/微软.安全/位置/位置/jitNetworkAccess策略/默认" -虚拟机$JitPolicyArr

有关详细信息，请参阅[PowerShell cmdlet 文档](https://docs.microsoft.com/powershell/scripting/developer/cmdlet/cmdlet-overview)。


## <a name="automatic-cleanup-of-redundant-jit-rules"></a>自动清理冗余 JIT 规则 

每当更新 JIT 策略时，都会自动运行清理工具以检查整个规则集的有效性。 该工具查找策略中的规则与 NSG 中的规则不匹配。 如果清理工具发现不匹配，它会确定原因，并在安全的情况下删除不再需要的内置规则。 清理程序永远不会删除您创建的规则。

当清理程序可能删除内置规则时，示例：

- 当存在两个定义相同的规则，并且一个规则具有高于另一个规则（这意味着，将永远不会使用低优先级规则）
- 当规则描述包含与规则中的目标 IP 不匹配的 VM 的名称时 


## <a name="next-steps"></a>后续步骤

在本文中，你已了解了安全中心中的实时 VM 访问如何帮助你控制对 Azure 虚拟机的访问。

若要了解有关安全中心的详细信息，请参阅以下文章：

- Microsoft 学习模块[使用 Azure 安全中心保护服务器和 VM 免受暴力攻击和恶意软件攻击](https://docs.microsoft.com/learn/modules/secure-vms-with-azure-security-center/)
- [设置安全策略](tutorial-security-policy.md)– 了解如何为 Azure 订阅和资源组配置安全策略。
- [管理安全建议](security-center-recommendations.md)– 了解建议如何帮助保护 Azure 资源。
- [安全运行状况监视](security-center-monitoring.md) — 了解如何监视 Azure 资源的运行状况。
