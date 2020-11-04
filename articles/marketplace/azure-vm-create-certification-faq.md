---
title: Azure Marketplace 的 VM 认证故障排除
description: 本文介绍了测试和验证 Azure Marketplace 的 VM 映像时常见的故障排除主题。
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: troubleshooting
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 10/19/2020
ms.openlocfilehash: f065b1bc98eab86542ecff73e1471e4d90cd4182
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/04/2020
ms.locfileid: "93339527"
---
# <a name="vm-certification-troubleshooting"></a>VM 认证故障排除

将虚拟机 (VM) 映像发布到 Azure Marketplace 时，Azure 团队会对其进行验证，以确保其 bootability、安全性和 Azure 兼容性。 如果任何高质量测试失败，则发布将失败，并且你将收到一条描述问题的错误消息。

本文介绍了 VM 映像发布期间的常见错误消息，以及相关解决方案。

> [!NOTE]
> 如果你有任何问题或反馈，请联系合作伙伴中心 [支持](https://aka.ms/marketplacepublishersupport)部门。

## <a name="approved-base-image"></a>批准的基本映像

提交请求以重新发布包含更新的映像时，部件号验证测试用例可能会失败。 如果失败，则不会批准映像。

当使用属于另一发布服务器的基本映像并且已更新该映像时，将发生此错误。 在这种情况下，你将不能发布映像。

若要解决此问题，请从 Azure Marketplace 检索映像，并对其进行更改。 有关详细信息，请参阅以下文章：

- [Linux 映像](../virtual-machines/linux/endorsed-distros.md?toc=/azure/virtual-machines/linux/toc.json)
- [Windows 映像](azure-vm-create-using-approved-base.md)

> [!Note]
> 如果使用的是不是从 Azure Marketplace 获取的 Linux 基础映像，则可以将第一个分区偏移 2048 KB。 这允许使用未格式化的空间来添加新的计费信息，并允许 Azure 将 VM 发布到 Azure Marketplace。  

> [!Note]
> 如果使用的是不是从 Marketplace 获取的 Linux 基础映像，则可以将第一个分区偏移 2048 KB。 这允许使用未格式化的空间来添加新的计费信息，并允许 Azure 将 VM 发布到 Marketplace。  

## <a name="vm-extension-failure"></a>VM 扩展失败

查看映像是否支持 VM 扩展。

若要启用 VM 扩展，请执行以下操作：

1. 选择你的 Linux VM。
1. 请参阅 " **诊断设置** "。
1. 通过更新 **存储帐户** 启用基本矩阵。
1. 选择“保存”。

   ![启用来宾级监视](./media/create-vm/vm-certification-issues-solutions-1.png)

若要验证是否已正确激活 VM 扩展，请执行以下操作：

1. 在 VM 中，选择 " **vm 扩展** " 选项卡，然后验证 **Linux 诊断扩展** 的状态。
1. 
    * 如果 "状态" *设置* 为 "已成功"，则已通过扩展测试用例。  
    * 如果状态为 " *预配失败* "，则扩展测试用例失败，需要设置强化标志。

      ![显示预配已成功的屏幕截图](./media/create-vm/vm-certification-issues-solutions-2.png)

      如果 VM 扩展失败，请参阅 [使用 Linux 诊断扩展监视指标和日志](../virtual-machines/extensions/diagnostics-linux.md) 以启用它。 如果你不希望启用 VM 扩展，请联系支持团队，并要求他们禁用该扩展。

## <a name="vm-provisioning-issue"></a>VM 设置问题

在提交产品/服务之前，请检查以确保已遵循 VM 预配过程。 若要查看用于预配 VM 的 JSON 格式，请参阅 [测试虚拟机映像](azure-vm-image-test.md)。

预配问题可能包括以下故障方案：

|场景|错误|Reason|解决方案|
|---|---|---|---|
|1|虚拟硬盘 (VHD 无效) |如果 VHD 页脚中的指定 cookie 值不正确，则 VHD 将被视为无效。|重新创建映像并提交请求。|
|2|无效的 blob 类型|VM 预配失败，因为使用的块是 blob 类型而不是页类型。|重新创建映像并提交请求。|
|3|预配超时或未正确通用化|VM 通用化出现问题。|用通用化重新创建映像并提交请求。|
|

> [!NOTE]
> 有关 VM 通用化的详细信息，请参阅：
> - [Linux 文档](azure-vm-create-using-approved-base.md#generalize-the-image)
> - [Windows 文档](../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep)


## <a name="vhd-specifications"></a>VHD 规范

### <a name="conectix-cookie-and-other-vhd-specifications"></a>Conectix cookie 和其他 VHD 规范
"Conectix" 字符串是 VHD 规范的一部分，在下面的 VHD 页脚中定义为8字节 "cookie"，用于标识文件创建者。 Microsoft 创建的所有 vhd 文件都有此 cookie。 

VHD 格式的 blob 应具有512字节的页脚;这是 VHD 页脚格式：

|硬盘页脚字段|大小（字节）|
|---|---|
Cookie|8
功能|4
文件格式版本|4
数据偏移量|8
时间戳|4
Creator 应用程序|4
Creator 版本|4
Creator 主机操作系统|4
原始大小|8
当前大小|8
磁盘几何|4
磁盘类型|4
校验和|4
唯一 ID|16
已保存状态|1
保留|427


### <a name="vhd-specifications"></a>VHD 规范
若要确保无缝发布体验，请确保 **VHD 满足以下条件：**
* Cookie 必须包含字符串 "conectix"
* 必须修复磁盘类型
* VHD 的虚拟大小至少为20MB
* VHD (对齐，即虚拟大小必须是 1 MB 的倍数) 
* VHD blob 长度 = 虚拟大小 + VHD 脚注长度 (512) 

可在此处下载 VHD 规范 [。](https://www.microsoft.com/download/details.aspx?id=23850)


## <a name="software-compliance-for-windows"></a>适用于 Windows 的软件符合性

如果由于软件合规性问题而拒绝 Windows 映像请求，则可能是使用已安装的 SQL server 实例创建了 Windows 映像，而不是从 Azure Marketplace 获取相关的 SQL 版本基本映像。

请勿创建你自己的 Windows 映像，并在其中安装 SQL server。 请改用 Azure Marketplace (的企业/标准/web) 中已批准的 SQL 基本映像。

如果你正在尝试安装 Visual Studio 或任何 Office 许可产品，请联系支持团队以获得预先批准。

有关选择已批准基准的详细信息，请参阅 [从批准的基础创建虚拟机](azure-vm-create-using-approved-base.md)。

## <a name="tool-kit-test-case-execution-failed"></a>工具包测试用例执行失败

Microsoft 认证工具包可帮助你运行测试用例，并验证你的 VHD 或映像是否与 Azure 环境兼容。

下载 [Microsoft 认证工具包](azure-vm-image-test.md)。

## <a name="linux-test-cases"></a>Linux 测试用例

下表列出了工具包将运行的 Linux 测试用例。 说明中说明了测试验证。

|场景|测试用例|描述|
|---|---|---|
|1|Bash 历史记录|在创建 VM 映像之前，应清除 Bash 历史记录文件。|
|2|Linux 代理版本|应安装 Azure Linux 代理2.2.41 或更高版本。|
|3|必需的内核参数|验证是否已设置以下内核参数： <br>控制台 = ttyS0<br>earlyprintk = ttyS0<br>rootdelay=300|
|4|在 OS 磁盘上交换分区|验证是否未在 OS 磁盘上创建交换分区。|
|5|OS 磁盘上的根分区|为 OS 磁盘创建一个根分区。|
|6|OpenSSL 版本|OpenSSL 版本应为 v 0.9.8 或更高版本。|
|7|Python 版本|强烈建议使用 Python 版本2.6 或更高版本。|
|8|客户端活动间隔|将 ClientAliveInterval 设置为180。 在应用程序需要时，可以将其从30设置为235。 如果要为最终用户启用 SSH，则必须按照说明设置此值。|
|9|OS 体系结构|仅支持 64 位操作系统。|
|10|自动更新|确定是否启用 Linux 代理自动更新。|

### <a name="common-errors-found-while-executing-previous-test-cases"></a>执行前面的测试用例时发现常见错误

下表列出了在执行前面的测试用例时发现的常见错误：
 
|场景|测试用例|错误|解决方案|
|---|---|---|---|
|1|Linux 代理版本测试用例|最低 Linux 代理版本为2.2.41 或更高版本。 此要求是必需的，因为2020年5月1日。|请更新 Linux 代理版本，该版本应为2.241 或更高版本。 有关详细信息，可以访问 [Linux 代理版本更新页](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)。|
|2|Bash 历史记录测试用例|如果提交图像的 bash 历史记录大小超过 1 kb (KB) ，将会出现错误。 大小限制为 1 KB，以确保不会在 bash 历史记录文件中捕获任何潜在的敏感信息。|若要解决此问题，请将 VHD 装载到任何其他工作 VM，并进行所需的任何更改 (例如，删除 *bash* history 文件) 将大小减小到小于或等于 1 KB。|
|3|必需的内核参数测试用例|如果 **console** 的值未设置为 **ttyS0** ，则会收到此错误。 通过运行以下命令进行检查：<br>`cat /proc/cmdline`|将 **console** 的值设置为 **ttyS0** ，然后重新提交请求。|
|4|ClientAlive 间隔测试用例|如果该测试用例的工具包结果提供了失败的结果，则 **ClientAliveInterval** 的值不正确。|将 **ClientAliveInterval** 的值设置为小于或等于235，然后重新提交请求。|

### <a name="windows-test-cases"></a>Windows 测试用例

下表列出了工具包将运行的 Windows 测试用例，以及测试验证的说明：

|场景 |测试事例|描述|
|---|---|---|---|
|1|OS 体系结构|Azure 仅支持64位操作系统。|
|2|用户帐户依赖项|应用程序的执行不应依赖于管理员帐户。|
|3|故障转移群集|目前尚不支持 Windows Server 故障转移群集功能。 应用程序不应依赖于此功能。|
|4|IPV6|Azure 环境目前尚不支持 IPv6。 应用程序不应依赖于此功能。|
|5|DHCP|目前尚不支持动态主机配置协议服务器角色。 应用程序不应依赖于此功能。|
|6|Hyper-V|目前尚不支持 hyper-v 服务器角色。 应用程序不应依赖于此功能。|
|7|远程访问|尚不支持远程访问 (直接访问) 服务器角色。 应用程序不应依赖于此功能。|
|8|权限管理服务|Rights Management 服务。 尚不支持该服务器角色。 应用程序不应依赖于此功能。|
|9|Windows 部署服务|Windows 部署服务。 尚不支持该服务器角色。 应用程序不应依赖于此功能。|
|10|BitLocker 驱动器加密|操作系统不受操作系统硬盘支持，但它可以在数据磁盘上使用。 BitLocker 驱动器加密|
|11|Internet 存储名称服务器|目前尚不支持 Internet 存储名称服务器功能。 应用程序不应依赖于此功能。|
|12|多路径 I/O|多路径 i/o。 此服务器功能尚不受支持。 应用程序不应依赖于此功能。|
|13|Network Load Balancing|网络负载平衡。 此服务器功能尚不受支持。 应用程序不应依赖于此功能。|
|14|对等名称解析协议|对等名称解析协议。 此服务器功能尚不受支持。 应用程序不应依赖于此功能。|
|15|SNMP 服务| (SNMP) Services 功能的简单网络管理协议目前尚不受支持。 应用程序不应依赖于此功能。|
|16|Windows Internet 名称服务|Windows Internet 名称服务。 此服务器功能尚不受支持。 应用程序不应依赖于此功能。|
|17|无线 LAN 服务|无线 LAN 服务。 此服务器功能尚不受支持。 应用程序不应依赖于此功能。|
|

如果前面的测试用例遇到任何故障，请参阅该解决方案的表中的 " **说明** " 列。 如果需要详细信息，请与支持团队联系。 

## <a name="data-disk-size-verification"></a>数据磁盘大小验证

如果随数据磁盘提交的请求的大小大于 1023 gb (GB) ，则不会批准该请求。 此规则适用于 Linux 和 Windows。

重新提交大小小于或等于 1023 GB 的请求。

## <a name="os-disk-size-validation"></a>OS 磁盘大小验证

有关操作系统磁盘大小的限制，请参阅以下规则。 提交任何请求时，验证 OS 磁盘大小是否在 Linux 或 Windows 的限制范围内。

|(OS)|推荐的 VHD 大小|
|---|---|
|Linux|30 GB 到 1023 GB|
|Windows|30 GB 到 250 GB|
|

虚拟机允许访问基础操作系统时，请确保 VHD 大小对于 VHD 而言足够大。 由于磁盘无法在无停机时间的情况下扩展，因此请使用磁盘大小从 30 GB 到 50 GB。

|VHD 大小|实际占用的大小|解决方案|
|---|---|---|
|>500 tib (TiB) |n/a|请联系支持团队以获取异常批准。|
|250-500 TiB|>200 gb (GiB) 与 blob 大小的差异|请联系支持团队以获取异常批准。|
|

> [!NOTE]
> 较大的磁盘大小会产生较高的成本，并将在安装和复制过程中产生延迟。 由于此延迟和成本，支持团队可能会寻找异常批准的理由。

## <a name="wannacry-patch-verification-test-for-windows"></a>适用于 Windows 的 WannaCry 修补程序验证测试

若要防止与 WannaCry 病毒相关的潜在攻击，请确保使用最新修补程序更新所有 Windows 映像请求。

若要查看 Windows Server 修补版本的操作系统详细信息及其支持的最低版本，请参阅下表： 

可以通过或验证图像文件版本 `C:\windows\system32\drivers\srv.sys` `srv2.sys` 。

> [!NOTE]
> Windows Server 2019 没有任何必需的版本要求。

|(OS)|Version|
|---|---|
|Windows 服务 2008 R2|6.1.7601.23689|
|Windows Server 2012|6.2.9200.22099|
|Windows Server 2012 R2|6.3.9600.18604|
|Windows Server 2016|10.0.14393.953|
|Windows Server 2019|NA|
|

## <a name="sack-vulnerability-patch-verification"></a>SACK 漏洞修补程序验证

提交 Linux 映像时，可能会因为内核版本问题而拒绝请求。

使用已批准的版本更新内核，并重新提交请求。 可以在下表中找到已批准的内核版本。 版本号应等于或大于此处列出的编号。

如果映像未与以下某个内核版本一起安装，请将其更新为正确的修补程序。 在用以下必需的修补程序更新映像后，请求支持团队提供必要的批准：

- CVE-2019-11477 
- CVE-2019-11478 
- CVE-2019-11479

|OS 系列|版本|内核|
|---|---|---|
|Ubuntu|14.04 LTS|4.4.0-151| 
||14.04 LTS|4.15.0-1049-*-azure|
||16.04 LTS|4.15.0-1049|
||18.04 LTS|4.18.0-1023|
||18.04 LTS|5.0.0-1025|
||18.10|4.18.0-1023|
||19.04|5.0.0-1010|
||19.04|5.3.0-1004|
|RHEL 和美分 OS|6.10|2.6.32-754.15。3|
||7.2|3.10.0-327.79。2|
||7.3|3.10.0-514.66。2|
||7.4|3.10.0-693.50。3|
||7.5|3.10.0-862.34。2|
||7.6|3.10.0-957.21。3|
||7.7|3.10.0-1062.1。1|
||8.0|4.18.0-80.4.2|
||8.1|4.18.0-147|
||"7-RAW" (7.6) ||
||"7-LVM" (7.6) |3.10.0-957.21。3|
||RHEL-SAP 7。4|待定|
||RHEL-SAP 7。5|待定|
|SLES|SLES11SP4 (包括 SAP) |3.0.101-108.95.2|
||适用于 SAP 的 SLES12SP1|3.12.74-60.64.115.1|
||适用于 SAP 的 SLES12SP2|4.4.121-92.114.1|
||SLES12SP3|4.4180-4.31.1 (内核-azure) |
||适用于 SAP 的 SLES12SP3|4.4.180-94.97.1|
||SLES12SP4|4.12.14-6.15.2 (内核-azure) |
||适用于 SAP 的 SLES12SP4|4.12.14-95.19.1|
||SLES15|4.12.14-5.30.1 (内核-azure) |
||适用于 SAP 的 SLES15|4.12.14-5.30.1 (内核-azure) |
||SLES15SP1|4.12.14-5.30.1 (内核-azure) |
|Oracle|6.10|UEK2 2.6.39-400.312。2<br>UEK3 3.8.13-118.35。2<br>RHCK 2.6.32-754.15。3 
||7.0-7。5|UEK3 3.8.13-118.35。2<br>UEK4 4.1.12-124.28。3<br>RHCK 遵循 RHEL|
||7.6|RHCK 3.10.0-957.21。3<br>UEK5 4.14.35-1902.2.0|
|CoreOS 稳定2079.6。0|4.19.43*|
||Beta 2135.3。1|4.19.50*|
||Alpha 2163.2。1|4.19.50*|
|Debian|jessie (安全) |3.16.68-2|
||jessie precise-backports|4.9.168 + deb9u3|
||stretch (安全性) |4.9.168 + deb9u3|
||Debian GNU/Linux 10 (buster) |Debian 6.3.0-18 + deb9u1|
||buster、sid (stretch precise-backports) |4.19.37-5|
|

## <a name="image-size-should-be-in-multiples-of-megabytes"></a>图像大小应为兆字节的倍数

Azure 上的所有 Vhd 必须将虚拟大小调整为 1 mb 的倍数 (MB) 。 如果 VHD 不符合建议的虚拟大小，则可能会拒绝你的请求。

在将原始磁盘转换为 VHD 时遵循指导原则，并确保原始磁盘大小为 1 MB 的倍数。 有关详细信息，请参阅有关 [非认可分发的信息](../virtual-machines/linux/create-upload-generic.md)。

## <a name="vm-access-denied"></a>拒绝访问 VM

如果在 VM 上运行测试用例时遇到访问被拒绝问题，可能是因为没有足够的权限来运行测试用例。

检查是否为运行自测事例的帐户启用了正确的访问权限。 如果未启用访问权限，请启用它以运行测试用例。 如果你不想启用访问，则可能会与支持团队共享自测用例结果。

如果你想要提交针对认证过程的 SSH 禁用映像请求，请遵循以下步骤

1. 在映像中执行 Azure 工具包。  (请下载 [最新工具包](https://aka.ms/AzureCertificationTestTool)

2. 提起 [支持票证](https://aka.ms/marketplacepublishersupport)，附加工具包报告并提供产品/服务详细信息-产品/服务名称、发布者名称、计划 ID/SKU 和版本。

3. 请重新提交证书申请。


## <a name="download-failure"></a>下载失败
    
请参阅下表，了解当你使用共享访问签名 (SAS) URL 下载 VM 映像时出现的任何问题。

|场景|错误|Reason|解决方案|
|---|---|---|---|
|1|找不到 Blob|VHD 可以从指定位置删除或移动。|| 
|2|Blob 正在使用中|VHD 由其他内部进程使用。|使用 SAS URL 下载 VHD 时，VHD 应处于已使用状态。|
|3|无效 SAS URL|VHD 的关联 SAS URL 不正确。|获取正确的 SAS URL。|
|4|签名无效|VHD 的关联 SAS URL 不正确。|获取正确的 SAS URL。|
|6|HTTP 条件头|SAS URL 无效。|获取正确的 SAS URL。|
|7|VHD 名称无效|检查 VHD 名称中是否存在任何特殊字符，如百分号 (% ) 或引号 ( ") 。|通过删除特殊字符来重命名 VHD 文件。|
|

## <a name="first-mb-2048-kb-partition-only-for-linux"></a>第一 MB (2048 KB) 分区 (仅适用于 Linux) 

提交 VHD 时，请确保 VHD 的第一个 2048 KB 为空。 否则，你的请求将失败。

>[!NOTE]
>* 对于某些特殊的映像，例如从 Azure Marketplace 获取的 Azure Windows 基准映像的基础，我们将检查计费标记，如果计费标记存在并且与我们的内部可用值匹配，则忽略 MB 分区。


## <a name="steps-for-creating-first-mb-2048-kb-partition-only-for-linux-on-an-empty-vhd"></a> (2048 KB) 分区 (仅用于在空 VHD 上的 Linux) 中创建第一个 MB 的步骤

步骤1：创建任意类型的 VM (例如： Ubuntu、分币 OS 等) 。 填写必填字段，然后单击 "下一步：磁盘>" \
![下一步：磁盘命令](./media/create-vm/vm-certification-issues-solutions-15.png)

步骤2：为上述 VM 创建非托管磁盘。
![创建非托管磁盘](./media/create-vm/vm-certification-issues-solutions-16.png)

请注意，可以采用默认值，也可以指定 NSG 和公共 IP 等字段的任何值。

步骤3：创建 VM 后，请单击左侧的 "磁盘"，如下所示 ![ 单击 "磁盘"](./media/create-vm/vm-certification-issues-solutions-17.png)

步骤4：请将 VHD 作为数据磁盘附加到上述 VM，以创建分区表，如下所示。
![附加 VHD](./media/create-vm/vm-certification-issues-solutions-18.png)

单击 "添加 DataDisk-> 现有 Blob-> 浏览 VHD 存储帐户-> 容器-> 选择 VHD-> 单击" 确定 "，如下所示 \
![选择 VHD](./media/create-vm/vm-certification-issues-solutions-19.png)

你的 VHD 将作为数据磁盘 LUN 0 添加，请在添加磁盘后重新启动 VM

步骤5：重新启动 VM 后，使用 Putty (或任何其他客户端) 登录到 VM，并运行 "sudo-i" 命令获取根访问权限。

![登录到 VM](./media/create-vm/vm-certification-issues-solutions-20.png)

步骤6：按照以下步骤在 VHD 上创建分区。

) 类型 fdisk/dev/sdb 命令

b) 从 VHD 查看现有分区列表，请键入 p

c) 键入 d 以删除 VHD 中的所有现有分区 (如果不需要此步骤) ![ 删除所有现有分区，则可以跳过此步骤](./media/create-vm/vm-certification-issues-solutions-21.png)

d) 键入 n 创建新分区，并为 (主分区) 选择 p。

e) 请输入2048作为 "第一个扇区" 值，可以保留 "last 扇区"，因为它将采用默认值。请注意，所有数据都将被清除到 2048 KB 之间。
           
>[!NOTE]
>* 请注意，通过创建分区时，所有现有数据都将被清除到 2048 KB，因此建议在执行上述命令之前对 VHD 进行备份。

请在下面的屏幕截图中查找参考。
![擦除的数据](./media/create-vm/vm-certification-issues-solutions-22.png)

f) 键入 w 以确认分区的创建。 

![创建分区](./media/create-vm/vm-certification-issues-solutions-23.png)

g) 你可以通过运行命令 "/dev/sdb" 和键入 p 来验证分区表，然后你可以看到如下所示的分区，其中包含 2048 offset 值。 

 ![2048偏移](./media/create-vm/vm-certification-issues-solutions-24.png)

步骤7：请从 VM 分离 VHD，并删除 VM。

         
## <a name="steps-for-creating-first-mb-2048-kb-partition-only-for-linux-by-moving-the-existing-data-on-vhd"></a>通过移动 VHD 上的现有数据，创建第一个 MB (2048 KB) 分区)  (的步骤

步骤1：创建任意类型的 VM (例如： Ubuntu、分币 OS 等) 。 填写必填字段，然后单击 "下一步：磁盘>" \
![单击 "下一步：磁盘>"](./media/create-vm/vm-certification-issues-solutions-15.png)

步骤2：为上述 VM 创建非托管磁盘。
![创建非托管磁盘](./media/create-vm/vm-certification-issues-solutions-16.png)

请注意，可以采用默认值，也可以指定 NSG 和公共 IP 等字段的任何值。

步骤3：创建 VM 后，请单击左侧的 "磁盘"，如下所示 ![ 单击 "磁盘"](./media/create-vm/vm-certification-issues-solutions-17.png)

步骤4：请将 VHD 作为数据磁盘附加到上述 VM，以创建分区表，如下所示。
![分区表](./media/create-vm/vm-certification-issues-solutions-18.png)

单击 "添加 DataDisk-> 现有 Blob-> 浏览 VHD 存储帐户-> 容器-> 选择 VHD-> 单击" 确定 "，如下所示 \
![选择 VHD](./media/create-vm/vm-certification-issues-solutions-19.png)

你的 VHD 将作为数据磁盘 LUN 0 添加，请在添加磁盘后重新启动 VM

步骤5：重新启动 VM 后，使用 Putty 登录到 VM，并运行 "sudo-i" 命令获取根访问权限。 \
![重新启动后登录](./media/create-vm/vm-certification-issues-solutions-20.png)

步骤6：请 excute 命令回显 "+ 1M" |sfdisk--/dev/sdc-N ![ Execute 命令](./media/create-vm/vm-certification-issues-solutions-25.png)

>[!NOTE]
>* 请注意，上面的命令可能需要更长时间才能完成，因为它取决于磁盘的大小

步骤7：请从 VM 分离 VHD，并删除 VM。


## <a name="default-credentials"></a>默认凭据

请始终确保不会随提交的 VHD 一起发送默认凭据。 添加默认凭据会使 VHD 更容易受到安全威胁。 请改为在提交 VHD 时创建自己的凭据。
  
## <a name="datadisk-mapped-incorrectly"></a>DataDisk 映射不正确

当使用多个数据磁盘提交请求，但未按顺序排列时，这会被视为映射问题。 例如，如果有三个数据磁盘，则编号顺序必须是 *0、1、2* 。 任何其他订单都被视为一个映射问题。

请重新提交请求，并对数据磁盘进行适当的排序。

## <a name="incorrect-os-mapping"></a>OS 映射不正确

创建映像时，它可能映射到或分配了错误的 OS 标签。 例如，当你在创建映像时选择 " **Windows** " 作为 OS 名称的一部分时，os 磁盘应仅与 windows 一起安装。 同一要求适用于 Linux。

## <a name="vm-not-generalized"></a>VM 未通用化

如果要重复使用从 Azure Marketplace 获取的所有映像，则必须通用化操作系统 VHD。

* 对于 **linux** ，以下过程通用化 linux vm，并将其重新部署为单独的 vm。

  在 SSH 窗口中，输入以下命令： `sudo waagent -deprovision+user`

* 对于 **windows** ，你使用来通用化 windows 映像 `sysreptool` 。

有关此工具的详细信息，请参阅 [系统准备 (Sysprep) 概述]( https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)。

## <a name="datadisk-errors"></a>DataDisk 错误

有关与数据磁盘相关的错误的解决方案，请使用下表：

|错误|Reason|解决方案|
|---|---|---|
|`DataDisk- InvalidUrl:`|此错误可能是由于在提交产品/服务时为逻辑单元号 (LUN) 指定的数字无效。|验证数据磁盘的 LUN 编号序列是否位于 "合作伙伴中心" 中。|
|`DataDisk- NotFound:`|出现此错误的原因可能是数据磁盘未位于指定的 SAS URL。|验证数据磁盘是否位于请求中指定的 SAS URL。|
|

## <a name="remote-access-issue"></a>远程访问问题

如果没有为 Windows 映像启用远程桌面协议 (RDP) 选项，将收到此错误。 

提交 Windows 映像之前对其启用 RDP 访问。

## <a name="bash-history-failed"></a>Bash 历史记录失败

如果提交图像的 bash 历史记录大小超过 1 kb (KB) ，你会看到此错误。 大小限制为 1 KB，以确保不会在 bash 历史记录文件中捕获任何潜在的敏感信息。

下面是删除 "Bash 历史记录" 的步骤。

步骤 1。 部署 VM，并单击 Azure 门户上的 "运行命令" 选项。
![Azure 门户上运行命令](./media/create-vm/vm-certification-issues-solutions-3.png)

步骤 2。 选择第一个选项 "RunShellScript"，并运行以下命令。

命令： "cat/dev/null > ~/.bash_history && history-c" ![ Bash history 命令（在 Azure 门户上）](./media/create-vm/vm-certification-issues-solutions-4.png)

步骤 3. 执行完命令后，重新启动 VM。

步骤 4. 通用化 VM，拍摄映像 VHD 并停止 VM。

步骤 5。 Re-Submit 一般化映像。

## <a name="requesting-exceptions-custom-templates-on-vm-images-for-selective-tests"></a>请求异常 (自定义模板) 在 VM 映像上进行选择性测试

发布者可以请求在 VM 认证过程中执行的几个测试的例外。 当发布服务器提供证据来支持请求时，在极少数情况下会提供异常。 认证团队保留拒绝或批准例外的权利。

在下面的部分中，我们将讨论请求异常的主要方案以及如何请求异常。

### <a name="scenarios-for-exception"></a>异常情况

通常情况下，发布者请求异常的情况通常为三种。

- **一个或多个测试用例例外** -发布方联系合作伙伴中心 [支持](https://aka.ms/marketplacepublishersupport) 以请求测试用例的异常。

- **锁定的 vm/无根访问** –少数发布服务器具有一些方案，在这些方案中，需要锁定 vm，因为 vm 上安装了防火墙等软件。 在这种情况下，发布者可以下载 [认证的测试工具](https://aka.ms/AzureCertificationTestTool) ，并在合作伙伴中心 [支持](https://aka.ms/marketplacepublishersupport)提交报告。

- **自定义模板** –某些发布者发布需要自定义 ARM 模板来部署 VM 的 VM 映像。 在这种情况下，发布者应该在合作伙伴中心的 [支持](https://aka.ms/marketplacepublishersupport) 下提交自定义模板，以便证书团队可以使用相同的验证。

### <a name="information-to-provide-for-exception-scenarios"></a>要提供给异常方案的信息

发布者应与合作伙伴中心 [支持](https://aka.ms/marketplacepublishersupport) 人员联系，以获得上述方案的请求异常，并提供以下信息：

   1. 发布者 ID –合作伙伴中心门户上的发布者 ID
   2. 产品/服务 ID/名称–请求例外的产品/服务 ID/名称
   3. SKU/计划 ID –请求了其例外的 VM 产品/服务的计划 ID/sku
   4. 版本–请求其异常的 VM 产品/服务的版本
   5. 异常类型–测试，锁定 VM，自定义模板
   6. 请求原因–导致此异常的原因，以及有关要免除的测试的信息
   7. 已请求此异常的时间线-日期
   8. 附件-附加任何重要性证据文档。 对于锁定的 Vm，附加测试报告和自定义模板，提供自定义 ARM 模板作为附件。 为自定义模板附加锁定 Vm 和自定义 ARM 模板的报告失败将导致拒绝请求

## <a name="address-a-vulnerability-or-exploit-in-a-vm-offer"></a>解决 VM 产品/服务中的漏洞或攻击

本部分介绍当使用其中一个 VM 映像发现漏洞或漏洞时，如何提供新的 VM 映像。 这仅适用于发布到 Azure Marketplace 的 Azure 虚拟机产品/服务。

> [!NOTE]
> 不能从计划中删除最后一个 VM 映像，也不能停止销售产品/服务的最后一个计划。

执行下列操作之一：

- 如果你有新的 VM 映像来替换有漏洞的 VM 映像，请参阅下面 [的提供固定 vm 映像](#provide-a-fixed-vm-image) 。
- 如果没有用于替换计划中唯一 VM 映像的新 VM 映像，或者在完成计划后 [停止销售计划](partner-center-portal/update-existing-offer.md#stop-selling-an-offer-or-plan)。
- 如果你不打算替换该产品/服务中的唯一 VM 映像，我们建议你 [停止销售该产品/服务](partner-center-portal/update-existing-offer.md#stop-selling-an-offer-or-plan)。

### <a name="provide-a-fixed-vm-image"></a>提供固定 VM 映像

若要提供固定 VM 映像来替换有漏洞或利用的 VM 映像，请执行以下操作：

1. 提供新的 VM 映像，以解决此安全漏洞。
2. 删除具有安全漏洞或利用的 VM 映像。
3. 重新发布产品/服务。

#### <a name="provide-a-new-vm-image-to-address-the-security-vulnerability-or-exploit"></a>提供新的 VM 映像来解决安全漏洞或攻击

若要完成这些步骤，需要为要添加的 VM 映像准备技术资产。 有关详细信息，请参阅 [使用已批准的基础创建虚拟机](azure-vm-create-using-approved-base.md) 或 [使用自己的映像创建虚拟机](azure-vm-create-using-own-image.md)，并 [为 VM 映像生成 SAS URI](azure-vm-get-sas-uri.md)。

1. 登录[合作伙伴中心](https://partner.microsoft.com/dashboard/home)。
2. 在左侧导航菜单中，选择 " **商业市场**  >  **概述** "。
3. 在 " **产品/服务别名** " 列中，选择产品/服务。
4. 在 " **计划概述** " 选项卡上的 " **名称** " 列中，选择要将 VM 添加到其中的计划。
5. 在 " **技术配置** " 选项卡上的 " **VM 映像** " 下，选择 " **+ 添加 VM 映像** "。

> [!NOTE]
> 一次只能向一个计划添加一个 VM 映像。 要添加多个 VM 映像，请先发布第一个 VM 映像，然后再添加下一个 VM 映像。

6. 在出现的框中，提供新的磁盘版本和虚拟机映像。
7. 选择“保存草稿”。

继续下一部分，以删除包含安全漏洞的 VM 映像。

#### <a name="remove-the-vm-image-with-the-security-vulnerability-or-exploit"></a>删除具有安全漏洞或攻击的 VM 映像

1. 登录[合作伙伴中心](https://partner.microsoft.com/dashboard/home)。
2. 在左侧导航菜单中，选择 " **商业市场**  >  **概述** "。
3. 在 " **产品/服务别名** " 列中，选择产品/服务。
4. 在 " **计划概述** " 选项卡上的 " **名称** " 列中，选择包含要删除的 VM 的计划。
5. 在 " **技术配置** " 选项卡上的 " **vm 映像** " 下，在要删除的 vm 映像旁边，选择 " **删除 vm 映像** "。
6. 在出现的对话框中，选择 " **继续** "。
7. 选择“保存草稿”。

请继续下一节来重新发布产品/服务。

#### <a name="republish-the-offer"></a>重新发布产品/服务

1. 选择 " **查看并发布** "。
2. 如果需要向证书团队提供任何信息，请将其添加到 " **证书的说明** " 框。
3. 选择“发布”  。

若要完成发布过程，请参阅 [查看和发布产品/服务](review-publish-offer.md)。

## <a name="next-steps"></a>后续步骤

- [配置 VM 产品/服务属性](azure-vm-create-properties.md)
- [活动 marketplace 奖励](partner-center-portal/marketplace-rewards.md)
- 如果你有疑问或反馈，请联系合作伙伴中心 [支持](https://aka.ms/marketplacepublishersupport)部门。
