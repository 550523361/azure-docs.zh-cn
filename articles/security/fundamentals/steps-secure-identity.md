---
title: 保护 Azure AD 标识基础结构
titleSuffix: Azure Active Directory
description: 本文档概述了一系列重要操作，管理员应该执行它们以使用 Azure AD 功能保护其组织
author: martincoetzer
manager: manmeetb
tags: azuread
ms.service: security
ms.subservice: security-fundamentals
ms.topic: conceptual
ms.workload: identity
ms.date: 01/29/2020
ms.author: martinco
ms.openlocfilehash: e0db8edfdfa380697a1d8d7e262a7a84da2fb7d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "77565530"
---
# <a name="five-steps-to-securing-your-identity-infrastructure"></a>保护标识基础结构的五个步骤

阅读本文档意味着你认识到了安全的重要性。 你可能已在履行保护组织的责任。 如果需要说服其他人注重安全性，请建议他们阅读最新的 [Microsoft 安全情报](https://go.microsoft.com/fwlink/p/?linkid=2073747)。

本文档说明了如何使用包含五个步骤的查检表来防范组织受到网络攻击，帮助用户更安全地使用 Azure Active Directory 功能。

此查检表有助于快速部署关键的建议措施，立即为组织提供保护。具体措施包括：

* 增强凭据。
* 缩小受攻击面。
* 自动化威胁响应。
* 利用云智能。
* 启用最终用户自助服务。

请务必在阅读此清单时跟踪哪些功能和步骤已完成。

> [!NOTE]
> 本文档中的许多建议仅适用于配置为使用 Azure Active Directory 作为标识提供者的应用程序。 在应用中配置单一登录可确保将凭据策略、威胁检测、审核和日志记录的优势及其他功能添加到这些应用程序。 [通过 Azure Active Directory 实现的单一登录](../../active-directory/manage-apps/configure-single-sign-on-non-gallery-applications.md)是基础 - 本文中的所有建议都立足于此。

本文档中的建议符合[标识安全评分](../../active-directory/fundamentals/identity-secure-score.md)（Azure AD 租户的标识安全配置的自动评估）。 组织可以使用 Azure AD 门户中的"身份安全分数"页来查找其当前安全配置中的漏洞，以确保它们遵循 Microsoft 当前的安全[最佳实践](identity-management-best-practices.md)。 实施“安全评分”页中的每条建议可以提高评分和跟踪进度，并有助于将实施方案与其他类似规模的组织或行业进行比较。

![标识安全评分](./media/steps-secure-identity/azure-ad-sec-steps0.png)

> [!NOTE]
> 此处描述的许多功能都需要 Azure AD 高级订阅，而某些功能是免费的。 有关详细信息，请查看我们的[Azure 活动目录定价](https://azure.microsoft.com/pricing/details/active-directory/)和[Azure AD 部署清单](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-deployment-checklist-p2)。

## <a name="before-you-begin-protect-privileged-accounts-with-mfa"></a>开始之前：使用 MFA 保护特权帐户

在开始实施此查检表之前，请确保在阅读此查检表时不会受到攻击。 首先需要保护自己的特权帐户。

获得特权帐户控制权的攻击者可以执行巨大的破坏，因此先行保护这些帐户至关重要。 使用[Azure AD 安全默认值](../../active-directory/fundamentals/concept-fundamentals-security-defaults.md)或[条件访问](../../active-directory/conditional-access/plan-conditional-access.md)为组织中的所有管理员启用和要求[Azure 多重身份验证](../../active-directory/authentication/multi-factor-authentication.md)（MFA）。 如果尚未实施 MFA，请立即实施！ 因为 MFA 非常重要。

准备好了吗？ 让我们开始阅读查检表。

## <a name="step-1---strengthen-your-credentials"></a>步骤 1 - 增强凭据

大多数企业安全漏洞的起源在于某个帐户受到某种手段的攻击。这些手段多种多样，例如密码喷洒 (password spray)、破解重放或网络钓鱼。 请观看以下视频（45 分钟）来详细了解这些攻击：
> [!VIDEO https://www.youtube.com/embed/uy0j1_t5Hd4]

### <a name="make-sure-your-organization-uses-strong-authentication"></a>确保您的组织使用强身份验证

根据密码被猜出、钓鱼、被恶意软件盗窃或重复使用的频率，必须使用某种形式的强凭据来保护密码 – 详细了解 [Azure 多重身份验证](../../active-directory/authentication/multi-factor-authentication.md)。

要轻松启用基本级别的标识安全性，可以使用 Azure [AD 安全默认值](../../active-directory/fundamentals/concept-fundamentals-security-defaults.md)的一键启用。 安全默认值强制租户中的所有用户使用 Azure MFA，并阻止来自租户范围的旧协议登录。

### <a name="start-banning-commonly-attacked-passwords-and-turn-off-traditional-complexity-and-expiration-rules"></a>开始禁止使用经常受到攻击的密码，摒弃传统的复杂性规则和过期规则。

许多组织使用传统的复杂性规则（要求使用特殊字符、数字和大小写）和密码过期规则。 [Microsoft 的研究](https://aka.ms/passwordguidance)表明，这些策略会导致用户选择更容易猜出的密码。

Azure AD 的[动态禁止密码](../../active-directory/authentication/concept-password-ban-bad.md)功能使用当前的攻击者行为来防止用户设置容易猜出的密码。 在云中创建用户时，始终会启用此功能。混合型组织在部署[适用于 Windows Server Active Directory 的 Azure AD 密码保护](../../active-directory/authentication/concept-password-ban-bad-on-premises.md)时，也可以使用此功能。 Azure AD 密码保护会阻止用户选择这些常用密码，经扩展后可以阻止包含指定的自定义关键字的密码。 例如，可以阻止用户选择包含你公司的产品名称或当地球队的密码。

Microsoft 建议根据 [NIST 指导](https://pages.nist.gov/800-63-3/sp800-63b.html)采用以下新式密码策略：

1. 要求密码至少包含 8 个字符。 密码并非越长越好，因为这会导致用户选择容易预测的密码、将密码保存在文件中，或者写下密码。
2. 禁用过期规则，这驱使用户轻松猜到密码，如**Spring2019！**
3. 禁用字符组合要求，并防止用户选择经常受到攻击的密码，因为这些规则会导致用户在密码中选择容易预测的替代字符。

如果直接在 Azure AD 中创建标识，可以[使用 PowerShell 防止用户的密码过期](../../active-directory/authentication/concept-sspr-policy.md)。 混合型组织应该使用[域组策略设置](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994572(v%3dws.10))或 [Windows PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/set-addefaultdomainpasswordpolicy) 实施这些策略。

### <a name="protect-against-leaked-credentials-and-add-resilience-against-outages"></a>防止凭据泄漏，并针对服务中断添加复原功能

如果组织使用实施直通身份验证或联合身份验证的混合标识解决方案，应该启用密码哈希同步，原因包括以下两点：

* Azure AD 管理系统中的[凭据已泄漏的用户](../../active-directory/reports-monitoring/concept-risk-events.md)报告会警告用户名和密码对已在“黑暗网络”中透露。 日后遭到攻击的第三方站点上出现的网络钓鱼、恶意软件和密码重用，导致大量的密码泄漏。 Microsoft 会检测其中的许多泄漏凭据，如果它们与你组织中的凭据匹配，则会告诉你 – 但前提是已[启用密码哈希同步](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md)！
* 如果发生本地中断（例如，在勒索软件攻击中），您可以使用[密码哈希同步切换到使用云身份验证](choose-ad-authn.md)。此备份身份验证方法将允许您继续访问配置为使用 Azure 活动目录进行身份验证的应用，包括 Office 365。 在这种情况下，IT 人员无需求助于个人电子邮件帐户来共享数据，直到本地中断得到解决。

详细了解[密码哈希同步](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md)的工作原理。

> [!NOTE]
> 如果启用密码哈希同步并且使用 Azure AD 域服务，则 Kerberos (AES 256) 哈希和可选的 NTLM（RC4，不加盐）哈希也将加密并同步到 Azure AD。

### <a name="implement-ad-fs-extranet-smart-lockout"></a>实施 AD FS Extranet 智能锁定

将应用程序配置为直接向 Azure AD 进行身份验证的组织可以受益于 [Azure AD 智能锁定](../../active-directory/authentication/concept-sspr-howitworks.md)。 如果在 Windows Server 2012R2 中使用 AD FS，请实现 AD FS [Extranet 锁定保护](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection)。 如果在 Windows Server 2016 中使用 AD FS，请实现 [Extranet 智能锁定](https://support.microsoft.com/help/4096478/extranet-smart-lockout-feature-in-windows-server-2016)。 AD FS 智能 Extranet 锁定可以防范针对 AD FS 的暴力攻击，同时可以防止用户在 Active Directory 中遭到锁定。

### <a name="take-advantage-of-intrinsically-secure-easier-to-use-credentials"></a>利用本质安全的、易于使用的凭据

使用 [Windows Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification) 可在电脑和移动设备上将密码取代为强式双重身份验证。 这种身份验证包括一种与设备安全绑定的新型用户凭据，并使用生物识别技术或 PIN。

## <a name="step-2---reduce-your-attack-surface"></a>步骤 2 - 缩小受攻击面

密码攻击无处不在，尽量缩小组织中的受攻击面至关重要。 弃用不够安全的旧式协议、限制访问入口点、针对资源的管理访问实行更严格的控制有助于缩小受攻击面。

### <a name="block-legacy-authentication"></a>阻止传统身份验证

使用自身的传统方法在 Azure AD 中进行身份验证和访问公司数据的应用给组织带来了另一种风险。 使用传统身份验证的应用示例包括 POP3、IMAP4 或 SMTP 客户端。 传统身份验证应用会代表用户执行身份验证，并阻止 Azure AD 执行高级安全评估。 替代的现代身份验证将降低安全风险，因为它支持多重身份验证和条件访问。 我们建议采取以下三项措施：

1. [如果使用 AD FS，则阻止传统身份验证](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)。
2. [将 SharePoint Online 和 Exchange Online 设置为使用新式身份验证](../../active-directory/conditional-access/conditional-access-for-exo-and-spo.md)。
3. 如果您有 Azure AD 高级版，请使用[条件访问策略](../../active-directory/conditional-access/overview.md)来阻止旧版身份验证，否则请使用[Azure AD 安全默认值](../../active-directory/fundamentals/concept-fundamentals-security-defaults.md)。

### <a name="block-invalid-authentication-entry-points"></a>阻止无效的身份验证入口点

使用假设违规思路会减少用户凭据泄露造成的影响。 对于环境中的每个应用，请考虑有效的方案：要为哪些组、哪些网络、哪些设备和其他元素授权 – 然后阻止余下的部分。 使用[Azure AD 条件访问](../../active-directory/conditional-access/overview.md)，您可以根据定义的特定条件控制授权用户如何访问其应用和资源。

### <a name="restrict-user-consent-operations"></a>限制用户同意操作

了解各种[Azure AD 应用程序同意体验](https://docs.microsoft.com/azure/active-directory/develop/application-consent-experience)、[权限和同意的类型](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent)及其对组织安全状况的影响非常重要。 默认情况下，Azure AD 中的所有用户可以授予利用 Microsoft 标识平台访问组织数据的应用程序。 虽然允许用户自行同意，但允许用户轻松获取与 Microsoft 365、Azure 和其他服务集成的有用应用程序，但如果不仔细使用和监视，则可能会构成风险。

Microsoft 建议[禁用将来的用户同意操作](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-removing-user-access#i-want-to-disable-all-future-user-consent-operations-to-any-application)，以帮助减少您的表面积并减轻此风险。 如果禁用最终用户同意，以前的同意授予仍将得到遵守，但以后的所有同意操作必须由管理员执行。 用户可以通过集成[的管理员同意请求工作流](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-admin-consent-workflow)或通过您自己的支持流程请求管理员同意。 在禁用最终用户同意之前，请使用我们[的建议](https://docs.microsoft.com/azure/active-directory/manage-apps/manage-consent-requests)来规划您组织的此更改。 对于您希望允许所有用户访问的应用程序，请考虑[代表所有用户授予同意](https://docs.microsoft.com/azure/active-directory/develop/v2-admin-consent)，以确保尚未单独同意的用户能够访问该应用程序。 如果不希望这些应用程序在所有方案中都可供所有用户使用，请使用[应用程序分配](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups)和[条件访问](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)来限制用户对应用的访问。

确保用户可以请求管理批准新应用程序，以减少用户摩擦，最大限度地减少支持量，并防止用户使用非 Azure AD 凭据注册应用程序。 一旦您规范了同意操作，管理员应定期审核应用并授予权限。


### <a name="implement-azure-ad-privileged-identity-management"></a>实施 Azure AD Privileged Identity Management

“假设违规”的另一个影响是，需要尽量减少受到攻击的帐户以特权角色运行的可能性。

[Azure AD Privileged Identity Management (PIM)](../../active-directory/privileged-identity-management/pim-configure.md) 有助于尽量减少帐户特权，因为它可以帮助：

* 识别和管理已分配到管理角色的用户。
* 了解应删除的未使用或过多的特权角色。
* 建立规则来确保特权角色受到多重身份验证的保护。
* 建立规则来确保授予特权角色的时间长短刚好足够用于完成特权任务。

启用 Azure AD PIM，然后查看分配有管理角色的用户，并删除这些角色中不必要的帐户。 对于剩余的特权用户，请将其角色分配从“永久”更改为“符合条件”。 最后，建立适当的策略，确保当这些用户需要访问这些特权角色时，能够使用所需的变更控制安全地进行访问。

作为部署特权帐户过程的一部分，请遵循[最佳做法，至少创建两个紧急帐户](../../active-directory/users-groups-roles/directory-admin-roles-secure.md)，以确保在锁定自己时仍可以访问 Azure AD。

## <a name="step-3---automate-threat-response"></a>步骤 3 - 自动化威胁响应

Azure Active Directory 中的许多功能可以自动截获攻击，以消除检测与响应之间的延迟。 如果能够减少罪犯渗入你的环境所能利用的时间，则就可以降低成本和风险。 下面是可以采取的具体措施。

### <a name="implement-user-risk-security-policy-using-azure-ad-identity-protection"></a>使用“Azure AD 标识保护”实施用户风险安全策略

用户风险表示用户身份被泄露的可能性，并根据与用户标识关联的[用户风险检测](../../active-directory/identity-protection/overview.md)进行计算。 用户风险策略是一种条件访问策略，用于向特定用户或组评估风险级别。 根据“低”、“中”、“高”风险级别，可以配置一个策略来阻止访问，或者要求使用多重身份验证进行安全密码更改。 Microsoft 建议要求高风险用户进行安全密码更改。

![已标记为存在风险的用户](./media/steps-secure-identity/azure-ad-sec-steps1.png)

### <a name="implement-sign-in-risk-policy-using-azure-ad-identity-protection"></a>使用“Azure AD 标识保护”实施登录风险策略

登录风险是指除帐户所有者以外的其他某人尝试使用标识登录的可能性。 [登录风险策略](../../active-directory/identity-protection/overview.md)是条件访问策略，用于向特定用户或组评估风险级别。 根据风险级别（高/中/低），可以配置一个策略来阻止访问，或强制实施多重身份验证。 请确保针对“中”或更高风险的登录强制实施多重身份验证。

![从匿名 IP 登录](./media/steps-secure-identity/azure-ad-sec-steps2.png)

## <a name="step-4---utilize-cloud-intelligence"></a>第 4 步 - 利用云智能

审核和记录与安全相关的事件以及相关警报是有效保护策略的至关重要组成部分。 安全日志和报告提供可疑活动的电子记录，并帮助检测可能指示试图或已成功的外部网络渗透以及内部攻击的模式。 您可以使用审核来监视用户活动、记录法规遵从性、进行取证分析等。 警报提供安全事件的通知。

### <a name="monitor-azure-ad"></a>监视 Azure AD

Microsoft Azure 服务和功能提供可配置的安全审核和日志记录选项，帮助识别安全策略和机制的缺口，并解决这些缺口，以防违规。 可以使用 [Azure 日志记录和审核](log-audit.md)，以及 [Azure Active Directory 门户中的审核活动报告](../../active-directory/reports-monitoring/concept-audit-logs.md)。

### <a name="monitor-azure-ad-connect-health-in-hybrid-environments"></a>监视混合环境中的 Azure AD Connect Health

[使用 Azure AD Connect Health 监视 AD FS](../../active-directory/hybrid/how-to-connect-health-adfs.md) 能够更好地洞察潜在问题以及 AD FS 基础结构受到的攻击。 Azure AD Connect Health 提供附带详细信息的警报、解决方法步骤、相关文档的链接、与身份验证流量相关的多个指标的使用情况分析、性能监视和报告。

![Azure AD Connect Health](./media/steps-secure-identity/azure-ad-sec-steps4.png)

### <a name="monitor-azure-ad-identity-protection-events"></a>监视“Azure AD 标识保护”事件

[Azure AD 标识保护](../../active-directory/identity-protection/overview.md)是一个通知、监视和报告工具，可用于检测影响组织标识的潜在漏洞。 它检测风险检测，例如泄露的凭据、不可能的旅行以及来自受感染设备的登录、匿名 IP 地址、与可疑活动关联的 IP 地址以及未知位置。 启用通知警报可以接收有风险用户的电子邮件，和/或每周摘要电子邮件。

Azure AD 标识保护提供两份应该每日监视的重要报告：
1. 风险登录报告显示应该调查的用户登录活动，合法所有者不可以执行这种登录。
2. 风险用户报告显示可能已泄密的用户帐户，例如，检测到已泄漏的凭据，或者用户从不同的位置登录，导致不可能的行程事件。

![已标记为存在风险的用户](./media/steps-secure-identity/azure-ad-sec-steps3.png)

### <a name="audit-apps-and-consented-permissions"></a>审核应用和许可的权限

用户可以被骗导航到受攻击的网站或应用，这些网站或应用将访问其个人资料信息和用户数据（如电子邮件）。 恶意行动者可以使用获得的许可权限来加密用户的邮箱内容，并勒索邮箱数据的赎金。 [管理员应查看和审核](https://docs.microsoft.com/office365/securitycompliance/detect-and-remediate-illicit-consent-grants)用户授予的权限，或禁用用户默认同意的权限。

除了审核用户授予的权限外，您还可以在高级环境中[找到有风险或不需要的 OAuth 应用程序](https://docs.microsoft.com/cloud-app-security/investigate-risky-oauth)。

## <a name="step-5---enable-end-user-self-service"></a>第 5 步 - 启用最终用户自助服务

我们希望在安全性与工作效率之间尽量实现平衡。 在达成目标的同时兼顾长期安全基准的制定，可以消除组织中存在的冲突，让用户获得所需的能力，并让他们保持警惕。

### <a name="implement-self-service-password-reset"></a>实施自助密码重置

Azure AD 的[自助服务密码重置 （SSPR）](../../active-directory/authentication/quickstart-sspr.md)为 IT 管理员提供了一种简单的方法，允许用户重置或解锁其密码或帐户，而无需帮助台或管理员干预。 该系统包括详细报告，跟踪用户重置密码的时间，以及提醒您误用或滥用的通知。

### <a name="implement-self-service-group-and-application-access"></a>实现自助服务组和应用程序访问

Azure AD 为非管理员提供了使用安全组、Office 365 组、应用程序角色和访问包目录管理对资源的访问的能力。  [自助服务组管理](../../active-directory/users-groups-roles/groups-self-service-management.md)使组所有者能够管理自己的组，而无需分配管理角色。 用户还可以创建和管理 Office 365 组，而无需依赖管理员来处理其请求，未使用的组将自动过期。  [Azure AD 授权管理](../../active-directory/governance/entitlement-management-overview.md)进一步支持委派和可见性，具有全面的访问请求工作流和自动过期功能。  您可以将配置自己的访问包的能力委托给非管理员，以便为其拥有的组、团队、应用程序和 SharePoint Online 网站配置自己的访问包，并针对需要谁批准访问权限的自定义策略，包括配置员工的访问经理和业务合作伙伴发起人作为批准人。

### <a name="implement-azure-ad-access-reviews"></a>实施 Azure AD 访问评审

使用[Azure AD 访问审核](../../active-directory/governance/access-reviews-overview.md)，可以管理访问包和组成员身份、对企业应用程序的访问以及特权角色分配，以确保维护安全标准。  用户本身、资源所有者和其他审阅者定期进行监督，确保用户在不再需要访问权限时不会长时间保留访问权限。

## <a name="summary"></a>总结

保护安全标识基础结构涉及到很多方面，但此五步骤查检表可帮助你快速实现一个更可靠的安全标识基础结构：

* 增强凭据。
* 缩小受攻击面。
* 自动化威胁响应。
* 利用云智能。
* 通过自助措施提高可预测性，更全面地保护最终用户安全。

非常感谢你严肃看待标识安全性，希望本文档在帮助你提高组织的安全性上提供了有用的思路。

## <a name="next-steps"></a>后续步骤

如果在规划和部署这些建议方面需要帮助，请参阅 [Azure AD 项目部署计划](https://aka.ms/deploymentplans)。

如果您确信所有这些步骤都已完成，请使用 Microsoft 的[标识安全分数](../../active-directory/fundamentals/identity-secure-score.md)，这将让您了解[最新的最佳实践](identity-management-best-practices.md)和安全威胁。
