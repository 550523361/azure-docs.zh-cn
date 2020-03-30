---
title: 诊断 Linux 混合 Runbook 工作 - Azure 更新管理
description: 了解如何在 Linux 上解决和支持更新管理的 Azure 自动化混合 Runbook 工作线程的问题。
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 12/03/2019
ms.topic: conceptual
ms.service: automation
ms.subservice: update-management
manager: carmonm
ms.openlocfilehash: e60ba71607b99f0ea97e0725ffdd0740f3e9c579
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278293"
---
# <a name="understand-and-resolve-linux-hybrid-runbook-worker-health-for-update-management"></a>了解并解决 Linux 混合 Runbook 辅助角色运行状况，用于更新管理

可能会有许多原因导致计算机在更新管理中不显示“就绪”****。 在更新管理中，可以检查混合 Runbook 工作线程代理的运行状况以确定基础问题。 本文讨论如何在[脱机方案中](#troubleshoot-offline)从 Azure 门户和非 Azure 计算机运行 Azure 计算机的疑难解答。

下表列出计算机可能处于的三个就绪状态：

* **就绪**- 部署了混合 Runbook 工作线程，最后一次出现不到 1 小时。
* **已断开连接**- 已部署混合 Runbook 工作线程，最后一次出现是在 1 小时前。
* **未配置**- 找不到混合 Runbook 工作线程或尚未完成载入。

> [!NOTE]
> Azure 门户显示的内容与计算机的当前状态之间可能会有轻微的延迟。

## <a name="start-the-troubleshooter"></a>启动“故障排除”

对于 Azure 计算机，通过单击门户中“更新代理准备”**** 列下的“故障排除”**** 链接，可以启动“更新代理故障排除”**** 页。 对于非 Azure 计算机，该链接将带您到本文。 请参阅脱机说明以排除非 Azure 计算机故障。

![VM 列表页](../media/update-agent-issues-linux/vm-list.png)

> [!NOTE]
> 检查要求 VM 处于运行状态。 如果 VM 未运行，会出现一个用于链接到“启动 VM”**** 的按钮。

在“更新代理故障排除”**** 页上，单击“运行检查”****，启动故障排除。 疑难解答使用[Run 命令](../../virtual-machines/linux/run-command.md)在计算机上运行脚本以验证依赖项。 完成故障排除时，它会返回检查的结果。

![故障排除页](../media/update-agent-issues-linux/troubleshoot-page.png)

完成后，窗口中将返回结果。 检查部分提供了每项检查所要查找的内容相关信息。

![更新代理检查页](../media/update-agent-issues-linux/update-agent-checks.png)

## <a name="prerequisite-checks"></a>先决条件检查

### <a name="operating-system"></a>操作系统

操作系统检查验证混合 Runbook 辅助角色是否正在运行以下操作系统之一：

|操作系统  |说明  |
|---------|---------|
|CentOS 6 (x86/x64) 和 7 (x64)      | Linux 代理必须具有访问更新存储库的权限。 基于分类的修补需要借助“yum”来返回 CentOS 当前没有的安全数据。         |
|Red Hat Enterprise 6 (x86/x64) 和 7 (x64)     | Linux 代理必须具有访问更新存储库的权限。        |
|SUSE Linux Enterprise Server 11 (x86/x64) 和 12 (x64)     | Linux 代理必须具有访问更新存储库的权限。        |
|Ubuntu 14.04 LTS、16.04 LTS 和 18.04 LTS (x86/x64)      |Linux 代理必须具有访问更新存储库的权限。         |

## <a name="monitoring-agent-service-health-checks"></a>监视代理服务运行状况检查

### <a name="log-analytics-agent"></a>Log Analytics 代理

此检查可确保安装 Linux 的日志分析代理。 有关如何安装的说明，请参阅[安装适用于 Linux 的代理](../../azure-monitor/learn/quick-collect-linux-computer.md#install-the-agent-for-linux
)。

### <a name="log-analytics-agent-status"></a>日志分析代理状态

此检查可确保 Linux 的日志分析代理正在运行。 如果该代理未在运行，则可以运行以下命令尝试重启该代理。 有关对该代理进行故障排除的详细信息，请参阅[对 Linux 混合 Runbook 辅助角色进行故障排除](hybrid-runbook-worker.md#linux)

```bash
sudo /opt/microsoft/omsagent/bin/service_control restart
```

### <a name="multihoming"></a>多宿主

此检查可确定代理是否向多个工作区报告。 更新管理不支持多宿主。

### <a name="hybrid-runbook-worker"></a>混合 Runbook 辅助角色

此检查验证 Linux 的日志分析代理是否具有混合 Runbook 辅助程序包。 更新管理需要此包才能工作。

### <a name="hybrid-runbook-worker-status"></a>混合 Runbook 辅助角色状态

此检查可确保混合 Runbook 辅助角色在计算机上运行。 如果混合 Runbook 辅助角色正常运行，则应存在以下进程。 若要了解详细信息，请参阅[对适用于 Linux 的 Log Analytics 代理进行故障排除](hybrid-runbook-worker.md#oms-agent-not-running)。

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

## <a name="connectivity-checks"></a>连接性检查

### <a name="general-internet-connectivity"></a>一般 Internet 连接

此检查可确保计算机拥有 Internet 访问权限。

### <a name="registration-endpoint"></a>注册终结点

此检查确定混合 Runbook 辅助角色是否可以与 Azure 自动化正确通信日志分析工作区。

代理和防火墙配置必须允许混合 Runbook 辅助角色代理与注册终结点通信。 有关要打开的地址和端口的列表，请参阅[混合辅助角色的网络规划](../automation-hybrid-runbook-worker.md#network-planning)

### <a name="operations-endpoint"></a>操作终结点

此检查用于确定代理是否可以与作业运行时数据服务正确通信。

代理和防火墙配置必须允许混合 Runbook 辅助角色代理与作业运行时数据服务通信。 有关要打开的地址和端口的列表，请参阅[混合辅助角色的网络规划](../automation-hybrid-runbook-worker.md#network-planning)

### <a name="log-analytics-endpoint-1"></a>Log Analytics 终结点 1

此检查会验证计算机是否可以访问 Log Analytics 代理所需的终结点。

### <a name="log-analytics-endpoint-2"></a>Log Analytics 终结点 2

此检查会验证计算机是否可以访问 Log Analytics 代理所需的终结点。

### <a name="log-analytics-endpoint-3"></a>Log Analytics 终结点 3

此检查会验证计算机是否可以访问 Log Analytics 代理所需的终结点。

## <a name="troubleshoot-offline"></a><a name="troubleshoot-offline"></a>脱机使用故障排除

可以通过在本地运行脚本，在混合 Runbook 辅助角色上脱机使用故障排除。 Python 脚本 [update_mgmt_health_check.py](https://gallery.technet.microsoft.com/scriptcenter/Troubleshooting-utility-3bcbefe6) 可在脚本中心内找到。 以下示例显示了此脚本的输出示例：

```output
Debug: Machine Information:   Static hostname: LinuxVM2
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 00000000000000000000000000000000
           Boot ID: 00000000000000000000000000000000
    Virtualization: microsoft
  Operating System: Ubuntu 16.04.5 LTS
            Kernel: Linux 4.15.0-1025-azure
      Architecture: x86-64


Passed: Operating system version is supported

Passed: Microsoft Monitoring agent is installed

Debug: omsadmin.conf file contents:
        WORKSPACE_ID=00000000-0000-0000-0000-000000000000
        AGENT_GUID=00000000-0000-0000-0000-000000000000
        LOG_FACILITY=local0
        CERTIFICATE_UPDATE_ENDPOINT=https://00000000-0000-0000-0000-000000000000.oms.opinsights.azure.com/ConfigurationService.Svc/RenewCertificate
        URL_TLD=opinsights.azure.com
        DSC_ENDPOINT=https://scus-agentservice-prod-1.azure-automation.net/Accou            nts/00000000-0000-0000-0000-000000000000/Nodes\(AgentId='00000000-0000-0000-0000-000000000000'\)
        OMS_ENDPOINT=https://00000000-0000-0000-0000-000000000000.ods.opinsights            .azure.com/OperationalData.svc/PostJsonDataItems
        AZURE_RESOURCE_ID=/subscriptions/00000000-0000-0000-0000-000000000000/re            sourcegroups/myresourcegroup/providers/microsoft.compute/virtualmachines/linuxvm            2
        OMSCLOUD_ID=0000-0000-0000-0000-0000-0000-00
        UUID=00000000-0000-0000-0000-000000000000


Passed: Microsoft Monitoring agent is running

Passed: Machine registered with log analytics workspace:['00000000-0000-0000-0000-000000000000']

Passed: Hybrid worker package is present

Passed: Hybrid worker is running

Passed: Machine is connected to internet

Passed: TCP test for {scus-agentservice-prod-1.azure-automation.net} (port 443)             succeeded

Passed: TCP test for {eus2-jobruntimedata-prod-su1.azure-automation.net} (port 4            43) succeeded

Passed: TCP test for {00000000-0000-0000-0000-000000000000.ods.opinsights.azure.            com} (port 443) succeeded

Passed: TCP test for {00000000-0000-0000-0000-000000000000.oms.opinsights.azure.            com} (port 443) succeeded

Passed: TCP test for {ods.systemcenteradvisor.com} (port 443) succeeded

```

## <a name="next-steps"></a>后续步骤

要解决混合 Runbook 辅助手册的其他问题，请参阅[疑难解答 - 混合 Runbook 工作人员](hybrid-runbook-worker.md)。
