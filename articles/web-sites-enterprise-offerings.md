<properties urlDisplayName="Azure 网站企业产品" pageTitle="使用迁移助手将 IIS 网站迁移至 Azure 网站" metaKeywords="Azure Websites,solution" description="演示如何使用 Azure 网站为你的企业创建企业网站解决方案" metaCanonical="" services="web-sites" documentationCenter="" title="Azure Websites Offerings for Enterprise Whitepaper" authors="cephalin,Apurva.Joshi"  solutions="" writer="cephalin" manager="wpickett" editor=""  />

<tags ms.service="web-sites" ms.workload="web" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="10/20/2014" ms.author="cephalin" />

# Azure 网站企业产品白皮书 #

降低成本以及在快速发展的环境中更快地交付 IT 解决方案的要求为开发人员、IT 专业人员和管理人员带来了新的挑战。用户日益期望其业务线 (LOB) web 应用程序能够在任何设备上快速运行和响应。同时，企业正在尝试利用由于与云和移动服务集成所带来的生产力和效率提高，这简单可到使用 Active Directory 实现的跨设备的单一登录，复杂可到使用从内部 LOB 应用程序（反过来也可以从公司 Salesforce 实施中拉取数据）中提取的数据在 Office365 中进行协作。[Microsoft Azure 网站](/zh-cn/services/websites/) 是一种用于开发、测试和运行 web 及移动应用程序、Web API 和一般网站的企业级云服务。它可以用于在针对扩展和可用性优化的全球数据中心网络上运行企业网站、业务应用程序和数字营销活动，同时还支持持续集成和现代 DevOps 实践。 

本白皮书重点介绍专门侧重于运行 LOB Web 应用程序的功能，涵盖现有 web 应用程序的迁移和平台上全新 LOB web 应用程序的部署。 

## 目标受众 ##

希望迁移至当前在本地运行的云 web 工作负荷的 IT 专业人员、架构师和管理人员。Web 工作负荷能够跨越业务到员工，或者跨越业务到合作伙伴的 web 应用程序。

## 介绍 ##

Microsoft Azure 网站是一款理想的平台，可用于托管外部和内部 web 应用程序和服务，因为它提供了一款经济高效、高度可扩展、托管的解决方案，使你能够集中精力为用户创造商业价值，而不用耗费大量时间和资金维护和支持单独的环境。Microsoft Azure 网站提供了一款灵活的平台，可用于部署企业 web 应用程序，从而提供能够通过与 Microsoft Azure Active Directory 集成继续按照本地 Active Directory 进行身份验证的能力；支持利用内部连续集成和部署实践轻松、快速地进行部署，同时自动进行扩展来满足业务需求。所有内容均位于托管平台之上，使你能够专注于你的应用程序而不是基础架构。 

## 问题定义 ##

IT 环境正在快速变化，从在资本成本高、前置时间长的传统服务器上进行托管迁移到使用能够自动扩展以处理负载的按需式服务的托管。IT 部门不得不降低基础架构的成本和占地空间以及维护支出以减少资本支出，同时也在不断增加灵活性。较旧的基础架构平台（例如 Windows Server 2003）生命周期的即将结束正促使 IT 部门将云迁移作为一种避免新的长期资本成本的潜在方法。在过去，首席信息官为其他部门制定购买决策，但是慢慢地首席营销官和其他业务部门的负责人在如何花费其预算以及其投资有哪些回报方面发挥着更加积极的作用。企业日益需要其员工比以前任何时候都更具移动性，让员工能够远程工作，花费更多时间与客户在一起，满足客户轻松访问系统的需求。

企业需要每天、每周、每月都进行变革；企业正在借助由第三方或内部提供的充满新特性的定期更新服务寻求即时全球扩展。用户的期望更高，许多人在自己的私人生活中利用这些服务（例如 Office365），他们希望在其工作中有权访问类似、最新和功能丰富的服务。为了满足这种需求，IT 部门必须致力于通过选择和集成第三方服务，仔细选择可以适应业务需求的平台，同时可靠地降低总体拥有成本，帮助企业满足这种需求。

开发团队正在寻求提供直接业务利益，频繁地推出新功能；它们正在寻找一种集成其现有工具和实践（开发、测试、发布）的经济高效、可靠的平台；而且正在与 IT 部门合作实现部署、管理和告警的自动化，以期实现零停机的目标。

<a href="highlevel"></a>
## 高级别的解决方案 ##

Web 平台和框架越来越多地被用于开发、测试和托管业务线应用程序。就典型的业务线应用程序而言（例如内部员工支出系统），通常仅包含一个带有用于存储与应用程序相关数据的支持数据库的网站。

Microsoft Azure 网站是托管此类应用程序一个不错的选择，提供可扩展且可靠的基础架构，后者的管理和修补具有接近零的人工干预和停机时间。Microsoft Azure 平台提供了许多数据存储选项来支持在 Microsoft Azure 网站上托管的 web 应用程序，从托管的可扩展关系型数据库即服务 Microsoft Azure SQL Database，到我们的 ClearDB MySQL Database 和 MongoDB 等合作伙伴提供的热门服务。

另一种方法是在本地使用你的现有投资。在示例场景中（员工支出系统），你可能想要在自己的内部基础架构中维护数据存储。这可能是为了与内部系统 （报表、工资单、计费等）集成，或者为了满足 IT 监管要求。Microsoft Azure 网站提供两种方法支持你连接到本地的基础架构：

- [混合连接](http://aka.ms/hybridconnections)- 混合连接是 Microsoft Azure BizTalk 服务的一项功能，可支持 Azure 网站安全地连接到本地资源，例如 SQL Server、MySQL、Web API 和自定义 web 服务。 
- [虚拟网络集成](http://aka.ms/websitesvnet)- Azure 网站的虚拟网络集成支持你将网站连接到 Azure 虚拟网络，后者反过来又能够通过站点到站点 VPN 连接到你的本地基础架构。 

下图是一个具有适用于本地资源的连接选项的高级解决方案示例。

![](./media/web-sites-enterprise-offerings/on-premise-connectivity-solutions.png)

## 商业效益 ##

Microsoft Azure 网站提供大量商业效益，能够让你的功能更加经济高效，并灵活地进行交付来满足业务需求。 

### PaaS 模型 ###

Azure 网站构建于一种平台即服务的模型之上，可提供大量成本和效率节省。你不再需要花费数小时管理虚拟机以及修补操作系统和框架。Azure 网站是一个自动修补的环境，使你可以集中精力管理 web 应用程序而不用管理虚拟机，从而让团队有更多时间创造更多业务价值。

支撑 Microsoft Azure 网站的 PaaS 模型支持 DevOps 方法的执行者实现其目标。作为一项业务，这意味着在应用程序整个生命周期（包括开发、测试、发布、监控与管理和支持）中完整的管理和集成。 

对于开发团队而言，可以通过 Visual Studio Online、 GitHub、TeamCity、Hudson 或 BitBucket 配置连续的集成和开发工作流，从而自动地进行构建、测试和部署，加快发布周期，同时减少在现有基础架构中发布所涉及的摩擦。Azure 网站还支持为你的发布工作流创建多个测试和过渡环境，因此你不再需要为上述目的保留或分配硬件，你可以根据需要创建环境并定义自己发布工作流的升级。作为一项业务，你可以决定从源代码管理中发布到测试槽，执行一系列测试，成功完成后升级到过渡槽，最后在不停机的情况下切换到生产环境，其附加价值是：在 Azure 网站上托管的 web 应用程序为预加载，并且能够及时提供最佳客户体验。

借助内置的监控和警报功能，运营团队可以确信它们处于最佳位置，能够对在 Azure 网站上托管的 web 应用程序的任何问题作出快速反应。如果运营团队已投资来自 Microsoft Visual Studio Application Insights、New Relic 和 AppDynamics 的分析和监控解决方案，Azure 网站上也完全支持这些解决方案，从而实现了连续性，并支持通过熟悉的环境来监控 web 应用程序。

最后，Azure 网站提供自动将你的站点和托管数据库直接备份到 Azure Blob 存储容器的功能。为你提供了一种简单和经济高效的方法从灾难中恢复，从而降低了对复杂的本地硬件和软件的要求。

### 易迁移性 ###

随着硬件和操作系统的发布周期加速，硬件维护和循环成为企业的一个关键问题。你可能有大量即将在 2015 年支持服务到期的 Windows Server 2003 R2 服务器，但它们仍然托管着企业的关键 web 应用程序？Azure 网站是托管这些 web 应用程序和帮助你合理利用企业硬件资产的理想之选。Azure 网站允许你访问作为服务一部分管理和维护的各种硬件规格，从而无需考虑增加基础架构预算的替换和管理成本。迁移可以很简单，只需将来自你现有部署的操作复制和粘贴到 Azure 网站，或者更复杂一些的迁移，使用 Azure 网站迁移助手将增加价值。已迁移的 web 应用程序可享受全套 Azure 服务 - 将附加服务集成到 web 应用程序。例如，你可以考虑添加 Azure Active Directory，以基于用户与安全组的关联控制对应用程序的访问。另一个示例是可以添加缓存服务来提高性能和减少延迟，从而提供更佳的整体用户体验。 

### 企业类托管 ###

Azure 网站提供了一个稳定、可靠的平台，后者已被证实能够满足从小型内部开发和测试工作负荷到高度可扩展的高流量网站的各种业务需求。通过使用 Azure 网站，你可以使用与 Microsoft 公司用于处理高价值 web 工作负荷相同的企业类托管平台。Azure 网站以及 Azure 平台上的所有服务均符合安全要求和遵从法规要求，如 ISO (ISO/IEC 27001:2005）；SOC1 和 SOC2 SSAE 16/ISAE 3402 Attestations、HIPAA BAA、PCI 和 Fedramp，是每个元素和功能的核心，有关详细信息，请参阅[http://aka.ms/azurecompliance](http://aka.ms/azurecompliance)。 

Microsoft Azure 平台支持基于角色的身份验证控制，从而支持企业控制级别到 Azure 网站内的资源。RBAC 支持企业在 Azure 环境中为其所有资产实施自己的访问管理策略，将用户分配到组并反过来根据 Azure 网站等资产将所需的权限分配给这些组。关于 Azure 中 RBAC 的详细信息，请参阅[http://aka.ms/azurerbac](http://aka.ms/azurerbac)。通过利用 Azure 网站，你可以确保你的 web 应用程序部署在安全可靠的环境中，并且你对资产部署到哪些区域拥有完全控制权。 

此外，Azure 网站支持往回连接到你的内部资源（如数据仓库或 SharePoint 环境），从而还能够充分利用你在本地的投资。如[高级别的解决方案](#highlevel)中所述你可以使用混合连接和虚拟网络连接建立到本地基础架构和服务的连接。

### 全球扩展 ###

Azure 网站是一个全球和可扩展的平台，使你的 web 应用程序能够增长并快速适应不断成长的企业的需求，只需要最少的长期规划和最低的成本。在典型的本地基础架构场景中，本地和地理上需求的扩展和增长都需要大量的管理、规划和支出以供应和管理额外的基础架构。Azure 网站支持根据需求的变化扩展你的 web 应用程序。例如，下面我们使用开支应用程序作为例子，在一个月的大部分时间内，你的用户很少会使用该应用程序，但是随着每个月支出提交截止日期的邻近，应用程序的使用会增加，Azure 网站能够自动为你的应用程序供应更多基础架构，一旦应用程序的使用再次减少，它能够缩放回你所定义的基线基础架构。

Azure 网站现在在全球 17 个数据中心提供，且还在不断增长。有关最新的区域和位置列表，请参阅[http://aka.ms/azlocations](http://aka.ms/azlocations)。借助 Azure 网站网站，你的企业能够轻松实现全球覆盖和扩展。随着你的公司的发展到新区域，你在 Azure 网站上使用和托管的报表应用程序仪表板可以被轻松地将部署到其他数据中心，并通过 Azure 网站和 Azure Traffic Manager 组合更快地服务本地用户，其附加效益在于底层的可扩展基础架构能够随着区域办事处需求的变化而缩小和扩展。
 
## 解决方案详细信息 ##

让我们看一个应用程序迁移方案的示例。这概述了 Azure 网站功能如何共同来提供出色解决方案和业务价值的详细信息。
 
在整个示例中，我们要讨论的业务线应用程序是一个支出报表应用程序，它使员工能够提交费用进行报销。该应用程序托管于运行 IIS6 的 Windows Server 2003 R2 上，数据库是 SQL Server 2005 数据库。我们选择较旧服务器的原因在于 Windows Server 2003 R2 和 SQL Server 2005 的服务即将到期，并且我们拥有自动将工作负荷迁移到 Azure 的[工具](http://aka.ms/websitesmigration)和[指南](http://aka.ms/websitesmigrationresources)。基于这一点，此示例中所使用的模式将适用于各种迁移场景。 

### 迁移现有应用程序 ###

将业务线应用程序迁移到 Azure 网站的总体解决方案中的第一步是确定现有应用程序资产和体系结构。本白皮书中的示例是托管在单个 IIS 服务器上的 ASP.NET web 应用程序，以及托管在单独的 SQL Server 上的数据库（如图中所示）。员工使用用户名和密码组合登录到系统，输入支出的详细信息并将每项开支收据的扫描副本上传到数据库。 
 
![](./media/web-sites-enterprise-offerings/on-premise-app-example.png)

#### 需要注意的事项 ####

当迁移应用程序来自本地环境时，你可能需要记住 Azure 网站的几个限制。以下是将 web 应用程序迁移到 Azure 网站时需要注意的一些关键主题 ([http://aka.ms/websitesmigrationresources](http://aka.ms/websitesmigrationresources))：

-	端口绑定 - Azure 网站仅支持用于 HTTP 的端口 80 和用于 HTTPS 流量的端口 443，如果你的应用程序或站点使用任何其他端口，那么迁移完成后，应用程序或站点将使用用于 HTTP 的端口 80 和用于 HTTPS 流量的端口 443。这通常是一个无害的问题，因为在本地部署中使用不同的端口以克服域名的使用是很常见的情况，尤其是在开发和测试环境中
-	身份验证 - 默认情况下 Azure 网站支持匿名身份验证，而且还支持由应用程序鉴定的表单身份验证。当应用程序或站点仅集成 Azure Active Directory 和 ADFS 时，Azure 网站能够提供 Windows 身份验证，[此处](http://aka.ms/azurebizapp)将对该项功能进行详细介绍 
-	基于 GAC 的程序集 - Azure 网站不允许将程序集部署到全局程序集缓存 (GAC)，因此如果即将迁移的应用程序或网站在本地使用此功能，请考虑将这些程序集迁移到应用程序或站点的 bin 文件夹
-	IIS5 兼容模式 - Azure 网站不支持 IIS5 兼容模式，因此每个站点和父站点下的所有 web 应用程序均在单个应用程序池内相同的工作进程中运行
-	COM 库的使用 - Azure 网站不允许在平台上登记 COM 组件，因此如果站点或应用程序正在使用任何 COM 组件，这些组件需要重新写入到托管代码并与站点或应用程序一同部署
-	ISAPI 筛选器 - Azure 网站上可以支持 ISAPI 筛选器，它们需要作为站点的一部分部署并在站点或 web 应用程序的 web.config 文件中注册。有关详细信息，请参阅[http://aka.ms/azurewebsitesxdt](http://aka.ms/azurewebsitesxdt)。 

考虑完这些主题之后，你的 web 应用程序已准备好支持云。如果一些主题没有完全被满足也不用担心，迁移工具将为迁移提供最佳支持。 

迁移流程中的下一步骤是创建 Azure 网站和 Azure SQL Database。有多个具有不同 CPU 内核数量和 RAM 数量的各种规模的 Microsoft Azure 网站实例可供你根据 web 应用程序需求选择。有关详细信息和定价，请参阅[http://aka.ms/azurewebsitesskus](http://aka.ms/azurewebsitesskus)。同样，Microsoft Azure SQL Database 适用于所有业务需求，可提供各种服务层和性能级别来满足需求。更多信息可访问[http://aka.ms/azuresqldbskus](http://aka.ms/azuresqldbskus)。创建完成后，站点/应用程序被上传到 Azure 网站（通过 FTP 或 WebDeploy），然后再迁移到数据库。

在这种迁移中，该解决方案使用了 Azure SQL Database，但是这不是 Azure 支持的唯一的数据库。公司还可以通过外接程序（可在 [Azure 应用商店](http://aka.ms/azurestore)购买）使用 MySQL、MongoDB、Azure DocumentDB 等数据库。 

在创建 Azure SQL Database 时，可使用多种方法从本地服务器中导入现有数据库，从生成现有数据库的脚本到使用[数据层应用程序导出和导入](http://aka.ms/dacpac)。 

创建一个新的 Azure SQL Database，使用 SQL Server Management Studio 连接到数据库，然后运行脚本构建数据库架构并用来自本地数据库的数据对其进行填充，此时开支应用程序数据库创建完成。

迁移第一阶段中的最后一步需要将连接字符串更新到应用程序的数据库。这可以通过 Azure 管理门户来实现，每个 Azure 网站都具有其自己的仪表板以及配置屏幕，它允许修改应用程序特定的设置，包括正由应用程序用来连接到正在使用的任何数据库的任意连接字符串。

### 使用 Azure SQL Database 的替代方法 ###

Azure 平台提供多种将 Azure SQL Database 用作 web 应用程序主数据库的替代方法，这是为了支持不同的工作负荷（如使用 NoSQL 解决方案），或者为了使平台能够满足业务数据需求，例如，企业可以持有禁止脱离现场存储或在公共云环境中存储的数据，以此来维护其本地数据库的使用。

#### 到本地资源的连接 ####
Azure 网站提供两种连接到本地资源（如数据库）的方法，以支持重复使用现有的高价值基础架构。这两种方法分别是 Azure 网站虚拟网络集成和混合连接：

- Azure 网站虚拟网络集成支持 Azure 网站和 Azure 虚拟网络之间的集成，允许你访问在虚拟网络中运行的资源，这样如果借助站点到站点的 VPN 连接到你的本地网络，可直接连接到你的本地系统。
- 混合连接是 Azure BizTalk 服务的一项功能，提供了一种简单的方法来连接到单个本地资源，如 SQL Server、MySQL、HTTP Web API 和大多数自定义的 Web 服务。

#### 扩展性和弹性 ####

随着企业的成长，其员工数量也会增长（通过收购或自然的有机增长），因此也必须对 web 应用程序进行扩展以满足这些新需求。事实上，现在经常可以看到同地协同团队和远程办公员工的快速扩展，例如在美国、欧洲和亚洲设有办事处的公司，以及在很多地区组建移动销售团队的公司。Azure 网站能够方便、自动地规模化处理起伏变化。

Microsoft Azure 网站允许 web 应用程序配置为可以通过管理门户根据两个矢量 - 计划时间或按照 CPU 使用 - 自动扩展。Microsoft Azure 网站自动扩展提供了一种经济高效而极其灵活的方式来满足所有业务应用程序的更大变化，从像我们的支出报表系统这样的 web 应用程序到在短时间促销内经历访问量激增的营销网站。有关详细信息和使用 Microsoft Azure 网站扩展 web 应用程序的指南，请参阅[如何扩展网站](/zh-cn/documentation/articles/web-sites-scale/)。

除了 Microsoft Azure 网站的扩展灵活性，整体平台通过 web 应用程序及其资产跨多个数据中心和地理区域的分布，实现了业务连续性和弹性。

## 摘要 ##
Microsoft Azure 网站提供了一款灵活、经济高效、响应迅速的解决方案在快速发展的环境中满足企业不断变化的需求。Azure 网站企业利用托管平台以及现代化的 DevOps 功能和减少的人工管理提高了生产力和效率，同时提供企业扩展功能、弹性、安全性以及与本地资产的集成。

## 行动号召 ##
有关 Microsoft Azure 网站服务的详细信息，请访问[http://aka.ms/enterprisewebsites](http://aka.ms/enterprisewebsites)，在这里您可以查看更多信息；并登录[http://aka.ms/azuretrial](http://aka.ms/azuretrial)立刻注册试用版，评估该服务并发现为您企业带来的优势。
