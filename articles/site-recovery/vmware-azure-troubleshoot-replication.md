---
title: 使用 Azure Site Recovery 排查将 VMware VM 和物理服务器灾难恢复到 Azure 时的复制问题 | Microsoft Docs
description: 本文针对使用 Azure Site Recovery 将 VMware VM 和物理服务器灾难恢复到 Azure 期间遇到的常见复制问题提供故障排除信息。
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: ae2f32a02005bc015d2521e576ea5625bef2d377
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846005"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>解决 VMware VM 和物理服务器的复制问题

使用 Azure Site Recovery 保护 VMware 虚拟机或物理服务器时，可能会收到特定错误消息。 本文介绍在使用 [Azure Site Recovery](site-recovery-overview.md) 将本地 VMware VM 和物理服务器复制到 Azure 时可能遇到的一些常见问题。

## <a name="initial-replication-issues"></a>初始复制问题。

对于在支持方面遇到的初始复制故障，大多数都由源服务器到进程服务器或进程服务器到 Azure 之间的连接问题引起。 大多数情况下，用户可按照下列步骤解决这些问题。

### <a name="verify-the-source-machine"></a>验证源计算机
* 在源服务器计算机命令行中，使用 Telnet 通过 https 端口（默认为 9443）对进程服务器执行 ping 操作（如下所示），查看是否存在任何网络连接问题或防火墙端口阻止问题。

    `telnet <PS IP address> <port>`
> [!NOTE]
    > 请使用 Telnet，不要使用 PING 测试连接。  如果未安装 Telnet，请按照[此处](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)的步骤列表进行操作

如果无法连接，则在进程服务器上允许入站端口 9443，然后检查问题是否仍然存在。 有时，当进程服务器受 DMZ 保护时，也会导致此问题。

* 查看 `InMage Scout VX Agent – Sentinel/OutpostStart` 服务的状态，检查其是否未运行，然后检查问题是否仍然存在。   

## <a name="verify-the-process-server"></a>验证进程服务器

* **检查进程服务器是否主动将数据推送到 Azure**

在进程服务器计算机中，打开任务管理器（按 Ctrl-Shift-Esc）。 转到“性能”选项卡，然后单击“打开资源监视器”链接。 在资源管理器中，转到“网络”选项卡。检查“网络活动的进程”中的 cbengine.exe 是否正在主动发送大量数据（以 MB 为单位）。

![启用复制](./media/vmware-azure-troubleshoot-replication/cbengine.png)

如果不是，请执行下列步骤：

* **检查进程服务器是否能够连接 Azure Blob**：选择并勾选 cbengine.exe，查看“TCP 连接”，检查是否存在从进程服务器到 Azure 存储 Blob URL 的连接。

![启用复制](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

如果没有连接，则转到“控制面板”>“服务”，检查以下服务是否启动且正在运行：

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     *
（重新）启动所有未运行的服务，然后检查问题是否仍然存在。

* **检查进程服务器是否能通过端口 443 连接到 Azure 公共 IP 地址**

从 `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` 打开最新的 CBEngineCurr.errlog，然后搜索：443 和失败的连接尝试。

![启用复制](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

如果存在问题，则在进程服务器命令行中，使用 telnet 通过端口 443 对在 CBEngineCurr.currLog 中找到的 Azure 公共 IP 地址（上图中已屏蔽）执行 ping 操作。

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
如果无法连接，则检查访问问题是否由防火墙或代理导致（在下一步提供说明）。


* **检查进程服务器上基于 IP 地址的防火墙是否未阻止访问**：如果在服务器上使用基于 IP 地址的防火墙规则，请从[此处](https://www.microsoft.com/download/details.aspx?id=41653)下载 Microsoft Azure 数据中心 IP 范围的完整列表，然后将其添加到防火墙配置，确保它们允许到 Azure（和 HTTPS (443) 端口）的通信。  允许订阅的 Azure 区域的 IP 地址范围以及美国西部的 IP 地址范围（用于访问控制和标识管理）。

* **检查进程服务器上基于 URL 的防火墙是否未阻止访问**：如果在服务器上使用基于 URL 的防火墙规则，请确保将以下 URL 添加到防火墙配置。

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

* **检查进程服务器上的代理设置是否未阻止访问**。  如果使用代理服务器，请确保代理服务器名称由 DNS 服务器解析。
若要查看在配置服务器安装期间提供的信息， 请转到注册表项

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

现在请确保 Azure Site Recovery 代理正在使用相同的设置发送数据。
搜索 Microsoft Azure 备份

将其打开，再单击“操作”>“更改属性”。 “代理配置”选项卡下应显示代理地址，其应与注册表设置中显示的代理地址相同。 如果不同，请将其更改为相同的地址。


* **检查进程服务器上的限制带宽是否不受约束**：增加带宽，然后检查问题是否仍然存在。

## <a name="next-steps"></a>后续步骤
如需更多帮助，请在 [Azure Site Recovery 论坛](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)提出疑问。 我们的社区非常活跃，我们的工程师将为你提供帮助。
