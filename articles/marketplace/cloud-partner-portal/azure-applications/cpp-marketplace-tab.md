---
title: Azure 应用程序提供应用商店选项卡
description: 使用“市场”选项卡来标识适用于 Azure 应用程序套餐的营销资产。
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: dsindona
ms.openlocfilehash: 6113759cc68d07af4c22edf03b3274d92dcbe780
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416300"
---
# <a name="azure-application-marketplace-tab"></a>Azure 应用程序的“市场”选项卡

使用“市场”选项卡来介绍 Azure 应用程序并提供营销资产。 此选项卡包括以下窗体：概述、市场营销项目、潜在顾客管理和法律。

## <a name="overview-form"></a>“概览”窗体

“概览”窗体包含下一个屏幕截图中显示的必填字段和可选字段。 必填字段用星号 (*) 表示。

![“概览”窗体](./media/azureapp-marketplace-overview.png)

下表介绍的设置用于为套餐创建店面。   需要附加星号的字段。

|      字段         |    说明    |
|  ---------------   |  ---------------  |
| **标题\***        | 套餐的标题。 将在市场中突出显示。 最大长度为 50 个字符。 |
| **总结\***      | 套餐的简短摘要。 最大长度为 100 个字符。           |
| **长摘要\*** | 套餐的较长摘要（不过，其内容可与“摘要”相同）。 最大长度为 256 个字符。           |
| **说明\***  | 套餐的说明。 最大长度为 3000 个字符。 允许简单的 HTML 格式化，包括 &lt;p&gt;、&lt;em&gt;、&lt;ul&gt;、&lt;li&gt;、&lt;ol&gt; 和标头标记。  |
| **营销标识符\*** | 与此套餐关联的唯一 URL，通常包含组织名称和解决方案名称，最大长度为 50 个字符。 为服务选择一个短且易记的营销标识符。 此项将用在此套餐的市场 URL 中。 例如，如果您的发布者 ID 是"contoso"，而营销标识符为"示例App"，则 Azure 应用商店中产品/服务的 URL 将为`https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp`  
| **预览订阅代码\*** | 为预览器添加 1 到 100 个订阅标识符。 这些白名单的订阅将有权访问您的产品/服务，而优惠在发布后，在发布后，在发布之前，在预览版中可用。          |
| **有用链接**    | 或者，您可以为产品/服务的用户（如支持、文档、论坛等）提供指向各种资源的链接。 建议您至少向文档添加一个链接。            |
| **建议类别（最多 5 个）\*** | 选择一到五个类别。 所选类别用于将套餐映射到在 Azure 市场和 Azure 门户中提供的产品类别。 这些类别将显示在浏览页和产品详细信息页上。 |
|  |  |


## <a name="marketing-artifacts"></a>营销项目

“营销项目”窗体包含下一个屏幕截图中显示的必填字段和可选字段。 必填字段用星号 (*) 表示。

![“营销项目”窗体](./media/azureapp-marketplace-artifacts.png)

下表描述了营销项目。

|      字段         |    说明    |
|  ---------------   |  ---------------  |
| **小\***        | 小徽标：40x40 像素，采用 PNG 格式     |
| **中型\***       | 中等徽标：90x90 像素，采用 PNG 格式    |
| **大\***        | 大徽标：115x115 像素，采用 PNG 格式   |
| **宽\***         | 宽徽标：255x115 像素，采用 PNG 格式    |
| **主图**           | 可选的英雄徽标：815x290 像素，采用 PNG 格式。 **注：** 英雄图标在上载后无法删除。 |
| **屏幕截图（最多 5 个）** |        屏幕截图显示在产品详细信息页上。 它们能够十分直观地展示应用的作用及其工作原理。 例如，可以显示体系结构关系图或用例插图。 屏幕截图为可选，其限制为每个 SKU 5 个屏幕截图。 若要添加屏幕截图，请执行以下操作：<ul><li>选择“+ 添加屏幕截图”，打开“屏幕截图”窗口****</li><li>**名称** - 输入名称/标题（最大长度为 100 字符）。</li><li>**上传** - 上传图像。 图像必须为 PNG 格式，大小为 533 x 324 像素。</li></ul>           |
| **添加视频**      | 可选，视频将显示在您的产品详细信息页面上。 它们能够十分直观地展示应用程序的作用及其工作原理。 若要添加视频，请执行以下操作： <ul><li>选择“+ 添加视频”，打开“视频”窗口****</li><li>**名称** - 输入名称/标题（最大长度为 100 字符）。</li><li>**链接**- 输入托管视频的网站的网址（YouTube 或 Vimeo）</li><li>**缩略图**- 上传缩略图。 图像必须为 PNG 格式，大小为 533 x 324 像素。</li></ul>          |
|  |  |


### <a name="artifact-examples-in-azure-marketplace"></a>Azure 市场中的项目示例

接下来的屏幕截图显示市场搜索结果的示例。

![市场套餐搜索结果](./media/azureapp-marketplace-example-browse.png)

下图显示了客户在搜索结果中单击产品/服务磁贴后，产品/服务在应用商店中的显示方式。

![市场套餐搜索结果详细信息](./media/azureapp-marketplace-example-details.png)


### <a name="artifact-examples-in-azure-portal"></a>Azure 门户中的项目示例

以下屏幕捕获内容显示套餐在 Azure 门户中的显示情况。 若要查找此示例中的应用程序套餐，请浏览到“市场”>“所有内容”>“开发 + 测试”>“Jenkins”。**** Jenkins 套餐显示徽标、标题、发布者显示名称。

![在 Azure 门户中浏览套餐](./media/azureapp-portalbrowse-artifacts-jenkins.png)

下一个屏幕截图显示用户选择 Jenkins 时的应用程序详细信息。

![Azure 门户中的套餐详细信息](./media/azureapp-portal-artifacts-jenkins-details.png)


### <a name="logo-guidelines"></a>徽标准则

上传到云合作伙伴门户中的所有徽标都应遵循以下准则：

- Azure 设计具有简单的调色板。 保持徽标上的主要和辅助颜色数较低。
- Azure 门户的主题颜色为白色和黑色。 请避免将这两种颜色用作徽标的背景色。 使用可使徽标在 Azure 门户中突出显示的颜色。 建议使用简单的主颜色。 如果使用透明背景，请确保徽标/文本不使用白色、黑色或蓝色。
- 不要在徽标上使用渐变背景。
- 避免在徽标上放置文本，即使是公司或品牌名称也不可以。 徽标的外观应“平整”，并且应避免渐变。
- 不要拉伸徽标。


#### <a name="hero-logo"></a>特大徽标

特大徽标是可选的。

>[!IMPORTANT]
>上载英雄徽标后，无法将其删除。

主图徽标应遵循以下准则：

- 不允许使用黑色、白色和透明背景。
- 避免在徽标的背景中使用任何亮色。 发布者显示名称、计划标题和套餐详细摘要将以白色字体显示，因此必须与背景色之间有一定的反差。
- 设计徽标时请避免使用过多的文本。 列出套餐后，发布者名称、计划标题、套餐详细摘要和“创建”按钮以编程方式嵌入在徽标内。
- 在主图徽标的右侧留出一个不使用的矩形空间。 此空白区域为 415x100 像素，与左侧相距 370 像素。


## <a name="lead-management"></a>线索管理

“潜在顾客管理”窗体有一个可选字段，用于配置潜在顾客管理。 若要配置潜在顾客管理，请从下拉列表中选择“潜在顾客目标”。 接下来的屏幕截图显示可用的目标。

![选择潜在顾客管理目标](./media/azureapp-marketplace-leadmgmt.png)

>[!TIP]
>选择信息图标以查看此消息："选择将存储潜在顾客的系统。 在此处了解如何连接到 CRM[系统。](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)

有关详细信息，请参阅[配置潜在顾客](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)。


## <a name="legal"></a>合法

使用“法律”窗体提供每个套餐所需的法律文档。

提供以下信息：

- **隐私政策 URL\* ** - 输入指向应用隐私政策的链接。
- **使用\*条款**- 输入应用的使用条款。 客户必须接受这些条款才能试用应用。

![“法律”窗体](./media/azureapp-marketplace-legal.png)


## <a name="next-steps"></a>后续步骤

[“支持”选项卡](./cpp-support-tab.md)
