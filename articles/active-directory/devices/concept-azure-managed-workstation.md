---
title: 了解 Azure 托管的安全工作站-Azure Active Directory
description: 了解 Azure 托管的安全工作站并了解其重要性。
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6837bbdb63caf0fb1ecb3f6e520d5f3623483b44
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93083229"
---
# <a name="understand-secure-azure-managed-workstations"></a>了解安全的 Azure 托管工作站

受保护的独立工作站对于机密角色（如管理员、开发人员和关键服务操作员）的安全性至关重要。 如果客户端工作站安全受到威胁，则很多安全控制和保证可能会失败或无效。

本文档介绍构建安全工作站所需的内容，通常称为特权访问工作站 (PAW) 。 本文还包含有关设置初始安全控制的详细说明。 本指南介绍了基于云的技术如何管理该服务。 它依赖于 Windows 10RS5 中引入的安全功能、Microsoft Defender 高级威胁防护 (ATP) 、Azure Active Directory 和 Microsoft Intune。

> [!NOTE]
> 本文介绍安全工作站的概念及其重要性。 如果已熟悉此概念，并且想要跳到部署，请访问 [部署安全工作站](howto-azure-managed-workstation.md)。

## <a name="why-secure-workstation-access-is-important"></a>为什么安全工作站访问非常重要

快速采用云服务以及从任何位置工作的功能已创建新的利用方法。 通过在管理员工作的设备上利用弱安全控制，攻击者可以获得对特权资源的访问权限。

特权滥用和供应链攻击是攻击者用来破坏组织的五大方法中的一种。 它们也是在2018中报告的事件的第二个最常检测到的策略，这些策略是根据 [Verizon 威胁报告](https://enterprise.verizon.com/resources/reports/dbir/)和 [安全智能报告](https://aka.ms/sir)在中报告的。

大多数攻击者执行以下步骤：

1. 用于在中寻找某种方式的侦测，通常特定于行业。
1. 用于收集信息和确定渗入工作站的最佳方法，以将其视为低价值。
1. 使用持久性查找移动 [横向](https://en.wikipedia.org/wiki/Network_Lateral_Movement)的方式。
1. 机密和敏感数据的渗透。

在侦测过程中，攻击者经常渗入的设备可能会面临低风险或受轻视。 他们使用这些易受攻击的设备为横向移动和查找管理用户和设备寻找机会。 攻击者获得特权用户角色的访问权限后，就可以识别高价值的数据，并成功地盗取这些数据。

![典型的折衷模式](./media/concept-azure-managed-workstation/typical-timeline.png)

本文档介绍可帮助保护计算设备免受此类横向攻击的解决方案。 此解决方案将管理和服务与不太重要的工作效率设备隔离开来，并在可以 infiltrated 访问敏感云资源的设备之前中断链。 此解决方案使用属于 Microsoft 365 企业版堆栈的本机 Azure 服务：

* 适用于设备管理的 Intune 和一个安全的应用程序和 Url 列表
* 用于设备设置、部署和刷新的 Autopilot
* Azure AD 用户管理、条件性访问和多重身份验证
* Windows 10 (当前版本的设备运行状况证明和用户体验) 
* 用于云托管的终结点保护、检测和响应的 Defender ATP
* 用于管理授权和实时 (JIT 的 Azure AD PIM) 对资源的特权访问
* 用于监视和警报的 Log Analytics 和 Sentinel

## <a name="who-benefits-from-a-secure-workstation"></a>谁受益于安全工作站？

所有用户和操作员使用安全工作站都受益。 损害电脑或设备的攻击者可以模拟所有缓存的帐户。 登录到设备时，它们也可能使用凭据和令牌。 此风险使确保用于特权角色（包括管理权限）的设备的安全非常重要。 具有特权帐户的设备是横向移动和特权提升攻击的目标。 这些帐户可用于各种资产，例如：

* 本地或基于云的系统的管理员
* 关键系统的开发人员工作站
* 具有高曝光度的社交媒体帐户管理员
* 高度敏感的工作站，如 SWIFT 支付终端
* 工作站处理商业机密

为了降低风险，应为使用这些帐户的特权工作站实现提升的安全控制。 有关详细信息，请参阅 [Azure Active Directory 功能部署指南](../fundamentals/active-directory-deployment-checklist-p2.md)、 [Microsoft 365 路线图](/microsoft-365/security/office-365-security/security-roadmap)以及 [保护特权访问路线图](/windows-server/identity/securing-privileged-access/securing-privileged-access)) 。

## <a name="why-use-dedicated-workstations"></a>为什么使用专用工作站？

虽然可以将安全性添加到现有设备，但最好从安全的基础入手。 若要使你的组织在最佳位置以保持高安全级别，请从你知道的设备是安全的，并实现一组已知的安全控制。

通过电子邮件和 web 浏览增加了数量不断增长的攻击媒介，这使得确保设备可信任成为一种非常困难的因素。 本指南假定专用工作站与标准工作效率、浏览和电子邮件隔离开来。 从设备中删除工作效率、web 浏览和电子邮件可能会对工作效率产生负面影响。 但是，这种安全措施通常适用于作业任务未明确要求的情况，并且安全事件的风险很高。

> [!NOTE]
> 此处的 Web 浏览指的是对可为高风险活动的任意网站的常规访问。 此类浏览与使用 web 浏览器来访问少量的已知管理网站（如 Azure、Microsoft 365、其他云提供程序和 SaaS 应用程序）有明显不同。

包含策略通过增加阻止攻击者获取敏感资产访问权限的控制数量和类型，提高安全性。 本文中所述的模型使用分层权限设计，并限制对特定设备的管理特权。

## <a name="supply-chain-management"></a>供应链管理

对安全工作站至关重要的是供应链解决方案，使用名为 "信任的根" 的可信工作站。 选择信任硬件的根时必须考虑的技术应包括现代便携式计算机中包含的以下技术： 

* [受信任的平台模块 (TPM) 2。0](/windows-hardware/design/device-experiences/oem-tpm)
* [BitLocker 驱动器加密](/windows-hardware/design/device-experiences/oem-bitlocker)
* [UEFI 安全启动](/windows-hardware/design/device-experiences/oem-secure-boot)
* [通过 Windows 更新分发的驱动程序和固件](/windows-hardware/drivers/dashboard/understanding-windows-update-automatic-and-optional-rules-for-driver-distribution)
* [已启用虚拟化和要求 HVCI](/windows-hardware/design/device-experiences/oem-vbs)
* [驱动程序和应用要求 HVCI](/windows-hardware/test/hlk/testref/driver-compatibility-with-device-guard)
* [Windows Hello](/windows-hardware/design/device-experiences/windows-hello-biometric-requirements)
* [DMA i/o 保护](/windows/security/information-protection/kernel-dma-protection-for-thunderbolt)
* [系统防护](/windows/security/threat-protection/windows-defender-system-guard/system-guard-how-hardware-based-root-of-trust-helps-protect-windows)
* [新式待机](/windows-hardware/design/device-experiences/modern-standby)

对于此解决方案，将使用 [Microsoft Autopilot](/windows/deployment/windows-autopilot/windows-autopilot) 技术部署信任的根，使硬件满足新式技术要求。 为了保护工作站，Autopilot 使你能够利用 Microsoft OEM 优化的 Windows 10 设备。 这些设备从制造商处进入已知的良好状态。 Autopilot 可以将 Windows 设备转换为 "企业就绪" 状态，而不是重置可能不安全的设备。 它应用设置和策略、安装应用，甚至更改 Windows 10 的版本。 例如，Autopilot 可能会将设备的 Windows 安装从 Windows 10 专业版更改为 Windows 10 企业版，以便能够使用高级功能。

:::image type="complex" source="./media/concept-azure-managed-workstation/supplychain.png" alt-text="显示安全工作站生命周期的关系图。" border="false":::
图顶部附近有一个设备供应商。 箭头指向购买工作站的客户，以及标记为 "完成" 和 "交付" 的卡车的客户。 在卡车上，箭头指向标记为 "部署" 的图像，该图像使用工作站。 标记为 "自助服务体验" 的箭头将从该用户延伸到标记为 "已准备好企业" 的屏幕。 在该屏幕下，将显示标记为受管理保护的图标。 标记为稳定状态的箭头，管理屏幕上的当前点，并将其保留在生命图标结尾和中断修复重置图标。 最终箭头将从中断修复图标循环回来到 Ready for business 屏幕。
:::image-end:::

## <a name="device-roles-and-profiles"></a>设备角色和配置文件

本指南引用了多个安全配置文件和角色，它们可帮助你为用户、开发人员和 IT 人员创建更安全的解决方案。 这些配置文件可为可从增强或安全工作站获益的常见用户平衡可用性和风险。 此处提供的设置配置基于行业接受的标准。 本指南演示如何强化 Windows 10 并降低与设备或用户泄露相关的风险。 为了利用新式硬件技术和信任设备的根，我们将使用从 **高安全性** 配置文件开始启用的 [设备运行状况证明](https://techcommunity.microsoft.com/t5/Intune-Customer-Success/Support-Tip-Using-Device-Health-Attestation-Settings-as-Part-of/ba-p/282643)。 提供此功能是为了确保攻击者在设备的早期启动过程中不能持久。 它通过使用策略和技术来帮助管理安全功能和风险来实现此目的。

:::image type="content" source="./media/concept-azure-managed-workstation/seccon-levels.png" alt-text="列出角色（如用户和开发人员）、配置文件（如 &quot;基本&quot; 和 &quot;增强&quot;）以及安全控制（如应用、操作和功能）的表。" border="false":::

* **基本安全性** –托管的标准工作站为大多数家庭和小型企业使用提供了很好的起点。 这些设备在 Azure AD 中注册，并通过 Intune 进行管理。 此配置文件允许用户运行任何应用程序并浏览任意网站。 应启用反恶意软件解决方案，如 [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security) 。

* **增强的安全性** –此项级别的受保护解决方案适用于家庭用户、小型企业用户和一般开发人员。

   增强型工作站是一种基于策略的方法，用于提高低安全性配置文件的安全性。 它提供一种安全的方法来处理客户数据，同时还使用电子邮件和 web 浏览等生产力工具。 你可以使用审核策略和 Intune 监视增强型工作站的用户行为和配置文件使用情况。 使用 Windows 10 (1809) 脚本部署增强的工作站配置文件，并利用高级 [威胁防护 (ATP) ](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)来利用高级恶意软件防护。

* **高安全性** –减小工作站的受攻击面最有效的方法是删除自行管理工作站的能力。 删除本地管理权限是提高安全性的步骤，但如果实施不正确，则可能会影响工作效率。 高安全性配置文件是在增强的安全配置文件上构建的，并有一项重大更改：删除本地管理员。此配置文件专为高配置文件用户设计：高级管理人员、工资和敏感数据用户、服务和流程的审批者。

   高安全性用户要求更受控制的环境，同时仍然能够在简单易用的体验中执行诸如电子邮件和 web 浏览之类的活动。 用户需要一些功能，如 cookie、收藏夹和其他快捷键。 但是，这些用户可能不需要修改或调试其设备的功能。 它们也不需要安装驱动程序。 高安全性配置文件是使用高安全性-Windows 10 (1809) 脚本部署的。

* **专门化** –攻击者面向开发人员和 IT 管理员，因为他们可以改变攻击者感兴趣的系统。 专用工作站通过管理本地应用程序和限制网站来扩展高安全性工作站的策略。 它还限制了高风险生产力功能，如 ActiveX、Java、browser 插件和其他 Windows 控件。 将此配置文件与 DeviceConfiguration_NCSC-Windows 10 (1803) SecurityBaseline 脚本一起部署。

* **受保护** –破坏管理帐户的攻击者可能会因数据被盗、数据更改或服务中断而导致严重的业务损害。 在此强制状态下，工作站启用所有安全控件和策略，以限制对本地应用程序管理的直接控制。 受保护的工作站没有生产力工具，因此设备更难泄露。 它阻止最常见的网页仿冒攻击矢量：电子邮件和社交媒体。 可以通过安全工作站部署安全工作站-Windows 10 (1809) SecurityBaseline 脚本。

   ![安全工作站](./media/concept-azure-managed-workstation/secure-workstation.png)

   安全工作站为管理员提供了一个强化的工作站，其中包含明确的应用程序控制和应用程序防护。 工作站使用 credential guard、device guard 和 exploit guard 来保护主机免遭恶意行为。 所有本地磁盘也通过 BitLocker 加密。

* **独立** –此自定义脱机方案表示广泛的极端。 没有为这种情况提供安装脚本。 你可能需要管理要求不支持或未修补的旧版操作系统的业务关键功能。 例如，高价值生产线或生命支持系统。 由于安全性是关键的，云服务不可用，因此你可以手动管理和更新这些计算机，也可以使用独立的 Active Directory 林体系结构，如 (ESAE) 的增强的安全管理环境。 在这些情况下，请考虑删除基本 Intune 和 ATP 运行状况检查之外的所有访问。

   * [Intune 网络通信要求](/intune/network-bandwidth-use)
   * [ATP 网络通信要求](/azure-advanced-threat-protection/configure-proxy)

## <a name="next-steps"></a>后续步骤

[部署安全的 Azure 托管工作站](howto-azure-managed-workstation.md)。