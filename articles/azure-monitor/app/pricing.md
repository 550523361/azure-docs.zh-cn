---
title: 管理 Azure Application Insights 的使用情况和成本 | Microsoft docs
description: 在 Application Insights 中管理遥测量并监视成本。
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: Dale.Koetke
ms.date: 12/21/2018
ms.author: mbullwin
ms.openlocfilehash: f15a0670932a9017c079ff0cf1e7cb4ad598a9c4
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "54004329"
---
# <a name="manage-usage-and-costs-for-application-insights"></a>管理 Application Insights 的使用情况和成本

> [!NOTE]
> 本文介绍如何在 Application Insights 中分析数据使用情况。  相关信息请参阅以下文章。
> - [监视使用情况及预估成本](../../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md)介绍了如何针对不同的定价模型查看多个 Azure 监视功能的使用情况及预估成本。 它还介绍了如何更改定价模型。

如果对 Application Insights 定价工作原理存在疑问，欢迎在我们的[论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights)提出问题。

## <a name="pricing-model"></a>定价模型

[Azure Application Insights][start] 的定价基于所引入的数据量。 每个 Application Insights 资源作为独立服务计费，并在 Azure 订阅的帐单中产生相应费用。

### <a name="data-volume-details"></a>数据量详细信息

* 数据量是 Application Insights 收到的遥测数据的字节数。 数据量的衡量标准是 Application Insights 从用户应用程序收到的未压缩 JSON 数据包的大小。 对于[导入到 Analytics 中的表格数据](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import)，数据量的衡量标准是发送到 Application Insights 的文件的未压缩大小。
* 自 2018 年 4 月起，应用程序的数据量费用将在名为“数据引入”的新计费表中报告。 这种新型计量将在诸如 Applications Insights 和 Log Analytics 等监视技术之间进行共享，目前位于服务名称“Log Analytics”下。 
* 定价未考虑[实时指标流](../../azure-monitor/app/live-stream.md)数据。

有关在你所在区域中以适合你的货币计价的当前价格，请参阅 [Application Insights 定价][pricing]

### <a name="multi-step-web-tests"></a>多步骤 Web 测试

[多步骤 Web 测试](../../azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests)会额外收费。 多步骤 Web 测试是指执行一系列操作的 Web 测试。

单页“ping 测试”不单独计费。 进行 ping 测试和多步测试时发送的遥测数据与应用发送的其他遥测数据计费方式相同。

## <a name="review-usage-and-estimate-costs"></a>查看使用情况和估计成本

通过 Application Insights 可以根据最近的使用模式轻松估算成本。 若要开始使用，请在 Azure 门户中，转到 Application Insights 资源的“使用情况和估算成本”页：

![选择定价](./media/pricing/pricing-001.png)

A. 查看当月数据量。 这包括接收和保留的所有数据（在通过服务器和客户端应用以及可用性测试进行[采样](../../application-insights/app-insights-sampling.md)后）。  
B. 会单独对[多步骤 Web 测试](../../azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests)计费。 （这不包括简单可用性测试，它已包括在数据量费用中。）  
C. 查看上个月的数据量趋势。  
D. 启用数据引入[采样](../../application-insights/app-insights-sampling.md)。   
E. 设置每日数据量上限。  

若要更深入地调查 Application Insights 使用情况，请打开“指标”页，添加名为“数据点容量”的指标，然后选择“应用拆分”选项以按“遥测项类型”拆分数据。 

Application Insights 费用将添加到 Azure 帐单。 可以在 Azure 门户的“计费”部分或在 [Azure 计费门户](https://account.windowsazure.com/Subscriptions)中查看 Azure 账单的详细信息。 

![在左侧菜单中，选择“账单”](./media/pricing/02-billing.png)

## <a name="data-rate"></a>数据速率
通过三种方式限制发送数据量：

* **采样**：可以使用采样减少从服务器和客户端应用发送的遥测量，同时最大程度减小指标失真。 采样是你可用来调整发送数据量的主要工具。 了解有关[采样功能](../../application-insights/app-insights-sampling.md)的详细信息。 
* **每日上限**：在 Azure 门户中创建 Application Insights 资源时，每日上限设置为 100 GB/天。 在 Visual Studio 中创建 Application Insights 资源时，默认值很小（只有 32.3 MB/天）。 设置每日上限默认值，以便进行测试。 可预期用户在将应用部署到生产环境之前，会提高每日上限。 

    除非为高流量应用程序请求了更高的最大值，否则最大上限是 1000 GB/天。 

    设置每日上限时，务必小心。 你的目标应该是“永远不会达到每日上限”。 如果达到每日上限，会丢失当天剩余时间的数据，无法监视应用程序。 若要更改每日上限，请使用“每日容量上限”选项。 可在“使用情况和预估成本”窗格中访问此选项（本文后面会详述）。
    对于具有无法用于 Application Insights 的额度的某些订阅类型，我们已去除此限制。 以前，如果订阅有支出限制，“每日上限”对话框将提供如何去除此限制，并使每日上限提高到超过 32.3 MB/天的说明。
* **限制**：此项将数据速率限制为每秒 32,000 个事件，取每个检测密钥 1 分钟的平均值。

*如果应用超过限制速率，会发生什么情况？*

* 每分钟会评估一次应用发送的数据量。 如果超出一分钟内的平均每秒速率，服务器将拒绝某些请求。 SDK 将缓冲数据，然后尝试重新发送该数据。 它会在几分钟内出现传播高峰。 如果应用连续以超出限制的速率发送数据，一些数据会被丢弃。 （ASP.NET、Java 和 JavaScript SDK 会尝试以这种方式重新发送；其他 SDK 可能会只是丢弃超出限制的数据。）如果发生超出限制的情况，会显示一个通知，警告发生了这种情况。

*如何知道我的应用正在发送的数据量？*

可使用下列选项之一查看正在发送的数据量：

* 转到“使用情况和预估成本”页查看每日数据量图表。 
* 在指标资源管理器中，添加新图表。 对于图表指标，选择“数据点容量”。 启用“分组”，并按数据类型分组。

## <a name="reduce-your-data-rate"></a>降低数据速率
可通过以下操作降低数据量：

* 使用[采样](../../application-insights/app-insights-sampling.md)。 此项技术会降低数据速率，而无需倾斜指标。 你仍然可以在“搜索”中的相关项之间导航。 在服务器应用中，采样会自动运行。
* [限制可在每个页面视图中报告的 Ajax 调用数](../../azure-monitor/app/javascript.md#detailed-configuration)或关闭 Ajax 报告。
* [编辑 ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) 关闭不需要的集合模块。 例如，用户可能认为不再需要性能计数器或依赖项数据。
* 在单独的检测密钥之间拆分遥测。 
* 预先聚合指标。 如果将对 TrackMetric 的调用放在应用中，则可通过使用重载降低流量，这种重载接受对一批度量值的平均偏差和标准偏差的计算结果。 也可使用[预先聚合包](https://www.myget.org/gallery/applicationinsights-sdk-labs)。

## <a name="manage-the-maximum-daily-data-volume"></a>管理每日最大数据量

可以使用每日容量上限，限制所收集的数据。 但是，如果达到上限，会丢失当天剩余时间从应用程序发送的所有遥测数据。 不建议使应用程序达到每日上限。 达到每日上限后，无法跟踪应用程序的运行状况和性能。

不使用每日容量上限，而是使用[采样](../../application-insights/app-insights-sampling.md)，将数据量调整到所需级别。 然后，仅在应用程序意外开始发送远高于预期的遥测量时，才使用每日上限作为“最后的手段”。

若要更改每日上限，请在“使用情况和预估成本”窗格中 Application Insights 资源的“配置”部分，选择“每日上限”。

![调整每日遥测数据量上限](./media/pricing/pricing-003.png)

## <a name="sampling"></a>采样
[采样](../../application-insights/app-insights-sampling.md)是一种方法，可降低向应用发送遥测的速率，同时仍可在诊断搜索过程中查找相关事件。 此外，还可保留正确的事件计数。

采样是降低费用同时又不超出每月配额的有效方式。 采样算法会保留遥测的相关项，这样，当使用“搜索”时便可查找与特定异常相关的请求。 该算法还保留正确计数，使用户可在指标资源管理器中看到请求率、异常率和其他计数的正确值。

有数种形式的采样。

* [自适应采样](../../application-insights/app-insights-sampling.md)是用于 ASP.NET SDK 的默认设置。 自适应采样可自动调整为应用发送的遥测量。 它会在 Web 应用的 SDK 中自动运行，以便减少网络上的遥测流量。 
* *引入采样*是一种替代方法，会在应用的遥测进入 Application Insights 服务时运行。 引入采样不会影响从应用发送的遥测量，但会减少服务保留的量。 可以使用引入采样来降低来自浏览器和其他 SDK 的遥测所使用的配额。

若要设置引入采样，请转到“定价”窗格：

![选择“配额和定价”窗格中的“示例”磁贴，并选择采样分数](./media/pricing/pricing-004.png)

> [!WARNING]
> “数据采样”窗格仅控制引入采样的值。 它不反映 Application Insights SDK 在应用中应用的采样速率。 如果传入的遥测已在 SDK 处进行了采样，则引入采样不适用。
>

若要查找实际采样率（无论是否已应用），请使用[分析查询](analytics.md)。 该查询如下所示：

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

在每个保留的记录中，`itemCount` 指示它表示的原始记录数。 它等于 1 +（即以前已放弃的记录数）。 

## <a name="automation"></a>自动化

可使用 Azure 资源管理编写脚本进行价格计划设置。 [了解操作方法](powershell.md#price)。

## <a name="limits-summary"></a>限制摘要

[!INCLUDE [application-insights-limits](../../../includes/application-insights-limits.md)]

## <a name="disable-daily-cap-e-mails"></a>禁用每日上限电子邮件

若要禁用每日容量上限电子邮件，请在“使用情况和预估成本”窗格中 Application Insights 资源的“配置”部分下，选择“每日上限”。 有一些在达到上限时以及达到可调整的警告等级时发送电子邮件的设置。 如果要禁用与每日容量上限相关的所有电子邮件，请取消选中这两个框。

## <a name="legacy-enterprise-pricing-plan"></a>旧企业定价计划

对于 Azure Application Insights 的早期采用者来说，仍有两种可能存在的定价计划：基本计划和企业计划。 基本定价计划与上面所述相同，是默认计划。 它包括所有企业计划功能，不另外收费。 基本计划帐单主要针对引入的数据量。 

企业计划按节点收费，每个节点会收到每日数据限额。 在企业定价计划中，需要为引入数据中超出规定限额的数据支付费用。 如果使用 Operations Management Suite，应选择企业计划。 

有关在你所在区域中以适合你的货币计价的当前价格，请参阅 [Application Insights 定价](https://azure.microsoft.com/pricing/details/application-insights/)。

> [!NOTE]
> 在 2018 年 4 月，我们[引入了](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) Azure 监视的新定价模型。 此模型在监视服务的完整组合中采用简单的“即用即付”模型。 深入了解[新的定价模型](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs)、如何根据使用模式[评估移到此模型的影响](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#assessing-the-impact-of-the-new-pricing-model)以及[如何选择加入该新模型](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#moving-to-the-new-pricing-model)

### <a name="enterprise-plan-and-operations-management-suite-subscription-entitlements"></a>企业计划和 Operations Management Suite 订阅权利

按照[最近的公告](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/)，购买 Microsoft Operations Management Suite E1 和 E2 的客户能够以附加组件的形式获取 Application Insights Enterprise，而没有额外费用。 具体而言，Operations Management Suite E1 和 E2 的每个单位都包含对 Application Insights Enterprise 计划的 1 个节点的权利。 每个 Application Insights 节点包含每天最多 200 MB 的引入数据（独立于 Log Analytics 数据引入），并且数据会保留 90 天，无额外费用。 本文后面会详述该计划。 

由于此计划仅适用于使用 Operations Management Suite 订阅的客户，没有 Operations Management Suite 订阅的客户不会看到用于选择此计划的选项。

> [!NOTE]
> 若要确保获取此权利，必须在企业定价计划中具有 Application Insights 资源。 此权利仅以节点的形式应用。 基本计划中的 Application Insights 资源不会实现任何权益。 此权利不会出现在“使用情况和预估成本”窗格中显示的预估成本中。 此外，如果在 2018 年 4 月将订阅移到新的 Azure 监视定价模型，则只可使用基本计划。 如果具有 Operations Management Suite 订阅，不建议将订阅移到新的 Azure 监视定价模型。

### <a name="how-the-enterprise-plan-works"></a>企业计划的工作原理

* 将按在企业计划中为任何应用发送遥测数据的节点收费。
 * *节点*是托管应用的物理/虚拟服务器计算机或平台即服务角色实例。
 * 开发计算机、客户端浏览器和移动设备不计为节点。
 * 如果应用有多个组件（例如 Web 服务和后端辅助角色）发送遥测数据，则会对组件分开计数。
 * 定价未考虑[实时指标流](../../azure-monitor/app/live-stream.md)数据。 在订阅中，将按节点而非应用计费。 如果有 5 个节点在为 12 个应用发送遥测数据，则按 5 个节点计费。
* 虽然是按月计费，但对于从应用发送遥测数据的节点来说，实际上是按小时计费。 小时费率为月费除以 744（每月的小时数，每月按 31 天算）。
* 每天为每个检测到的节点分配的数据量为 200 MB（时间粒度为小时）。 分配的数据如果未使用，不会从当天转到第二天。
 * 如果选择企业定价计划，会根据该订阅中向 Application Insights 资源发送遥测数据的节点数为每个订阅提供每日数据限额。 因此，如果全天有 5 个节点在发送数据，则该订阅中的所有 Application Insights 资源可以共用 1 GB 的数据限额。 无所谓某些节点是否比其他节点发送更多数据，因为包含的数据在所有节点之间共享。 如果 Application Insights 资源在特定的某一天收到的数据量超出了为该订阅分配的每日数据限额，则会对超额数据按 GB 计费。 
 * 每日数据限额的计算方式为所有节点在一天中发送遥测数据的小时数（使用 UTC 计时方式）除以 24 再乘以 200 MB。 因此，如果有 4 个节点一天 24 小时中有 15 个小时在发送遥测，则当天包含的数据为 ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB。 超额数据的价格为 2.30 美元/GB，如果当天发送了 1 GB 的数据，则收费 1.15 美元。
 * 企业计划的每日限额不与已为其选择基本计划的应用程序共享。 未使用的限额不能从当天转到第二天。 

### <a name="examples-of-how-to-determine-distinct-node-count"></a>演示如何确定不同节点计数的示例

| 场景                               | 每日节点计数总数 |
|:---------------------------------------|:----------------:|
| 1 个应用程序使用 3 个 Azure 应用服务实例和 1 个虚拟服务器 | 4 |
| 3 个应用程序运行在 2 个 VM 上，这些应用程序的 Application Insights 资源属于同一订阅，所用计划为企业计划 | 2 | 
| 4 个应用程序的 Applications Insights 资源属于同一订阅；每个应用程序在 16 小时的非高峰时段内运行 2 个实例，在 8 小时的高峰时段内运行 4 个实例 | 13.33 | 
| 云服务有 1 个辅助角色和 1 个 Web 角色，每个角色运行 2 个实例 | 4 | 
| 5 节点 Azure Service Fabric 群集运行 50 个微服务；每个微服务运行 3 个实例 | 5|

* 若要进行精确的节点计数，必须了解应用程序在使用哪个 Application Insights SDK。 
  * 在 SDK 2.2 及更高版本中，Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) 或 [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 都会将每个应用程序主机作为节点进行报告。 例如，会报告物理服务器和 VM 主机的计算机名称，而对于云服务，则报告实例名称。  唯一的例外是应用程序仅使用 [.NET Core](https://dotnet.github.io/) 和 Application Insights Core SDK。 在这种情况下，所有主机只会报告一个节点，因为主机名不可用。 
  * 就早期版本的 SDK 来说，[Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 的行为与新版 SDK 并无二致，而 [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) 则只会报告一个节点，不管应用程序主机的数目是多少。 
  * 如果应用程序通过 SDK 将 **roleInstance** 设置为自定义值，则会默认使用该值确定节点计数。 
  * 如果对应用使用新版 SDK，而该应用在客户端计算机或移动设备中运行，则节点计数可能会返回很大的数目（因为在客户端计算机或移动设备数目很大）。 

## <a name="next-steps"></a>后续步骤

* [采样](../../application-insights/app-insights-sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: ../../application-insights/app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/