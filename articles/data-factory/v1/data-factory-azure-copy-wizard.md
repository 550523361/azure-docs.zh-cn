---
title: 数据工厂 Azure 复制向导
description: 了解如何使用数据工厂 Azure 复制向导将数据从支持的数据源复制到接收器。
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: 0974eb40-db98-4149-a50d-48db46817076
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/26/2020
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d2d6ee8357698e4af59f44579ce7f2c3aa82991c
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2020
ms.locfileid: "96340316"
---
# <a name="azure-data-factory-copy-wizard"></a>Azure 数据工厂复制向导

> [!NOTE]
> 本文适用于数据工厂版本 1。 

Azure 数据工厂复制向导可简化数据引入过程，这通常是端到端数据集成方案中的第一步。 使用 Azure 数据工厂复制向导时，不需要了解有关链接服务、数据集和管道的任何 JSON 定义。 向导会自动创建管道，将数据从所选数据源复制到所选目标。 此外，复制向导可帮助用户验证创作时所引入的数据。 这可节省时间，特别适用于从数据源首次引入数据时。 若要启动复制向导，请单击数据工厂主页上的“复制数据”磁贴。

![复制向导](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="designed-for-big-data"></a>设计用于大数据
通过本向导，可在数分钟内轻松地将数据从各种源移动到目标。 完成向导步骤后，会自动创建带有复制活动的管道以及依赖数据工厂实体（链接的服务和数据集）。 创建管道不需要执行任何额外步骤。   

![选择数据源](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> 有关创建示例管道将数据从 Azure Blob 复制到 Azure SQL 数据库表的分步说明，请参阅[复制向导教程](data-factory-copy-data-wizard-tutorial.md)。

该向导从设计之初就考虑到了大数据，支持多种数据和对象类型。 可创建移动数百个文件夹、文件或表的数据工厂管道。 该向导支持自动数据预览、架构捕获和映射，以及筛选数据。

## <a name="automatic-data-preview"></a>自动数据预览
可从选定的数据源预览部分数据，验证数据是否是要复制的数据。 此外，如果源数据在文本文件中，复制向导会自动分析文本文件，了解行和列分隔符以及架构。

![文件格式设置](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>架构捕获和映射
某些情况下，输入数据的架构可能与输出数据的架构不匹配。 在这种情况下，需要将源架构中的列映射到目标架构中的列。

> [!TIP]
> 将数据从 SQL Server 或 Azure SQL 数据库复制到 Azure Synapse 分析 (以前的 SQL 数据仓库) ，如果目标存储中不存在该表，数据工厂将支持使用源架构自动创建表。 了解有关 [使用 Azure 数据工厂将数据移入和移出 Azure Synapse 分析](./data-factory-azure-sql-data-warehouse-connector.md)的详细信息。

使用下拉列表从源架构中选择列，映射到目标架构中的列。 复制向导尝试了解列映射的模式。 它将相同模式应用于其余列，从而不需要单独选择每个列以完成架构映射。 如需要，可通过使用下拉列表逐个映射列来替代这些映射。 映射的列越多，模式越准确。 复制向导不断更新模式，最终达到想要完成的列映射的正确模式。     

![架构映射](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>筛选数据
可筛选源数据，仅选择需要复制到接收器数据存储的数据。 筛选能够减少复制到接收器数据存储的数据量，从而增强复制操作的吞吐量。 它能够通过 SQL 查询语言灵活筛选关系数据库中的数据，或通过[数据工厂函数和变量](data-factory-functions-variables.md)灵活筛选 Azure Blob 文件夹中的文件。   

### <a name="filtering-of-data-in-a-database"></a>数据库中的数据筛选
以下屏幕截图显示了使用 `Text.Format` 函数和 `WindowStart` 变量的 SQL 查询。

![验证表达式](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Azure Blob 文件夹中的数据筛选
可以使用文件夹路径中的变量，从运行时基于[系统变量](data-factory-functions-variables.md#data-factory-system-variables)确定的文件夹中复制数据。 支持的变量：**{year}**、**{month}**、**{day}**、**{hour}**、**{minute}** 和 **{custom}**。 例如：inputfolder/{year}/{month}/{day}。

假设输入文件夹格式如下：

```text
2016/03/01/01
2016/03/01/02
2016/03/01/03
...
```

单击“文件或文件夹”的“浏览”按钮，找到其中一个文件夹（例如，2016-> 03-> 01-> 02），并单击“选择”。 应该会在文本框中看到 `2016/03/01/02`。 现在，请用 **{year}** 代替 **2016**、**{month}** 代替 **03**、**{day}** 代替 **01**、**{hour}** 代替 **02**，并按 **Tab** 键。 可以看到用于选择这四个变量格式的下拉列表：

![使用系统变量](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

如以下屏幕截图所示，还可以使用“自定义”变量和任何[支持格式的字符串](/dotnet/standard/base-types/custom-date-and-time-format-strings)。 若要选择具有该结构的文件夹，请首先使用“浏览”按钮。 然后将值替换为 **{custom}**，并按 **Tab** 键，查看可在其中键入格式字符串的文本框。     

![使用自定义变量](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="scheduling-options"></a>计划选项
可以一次性运行复制操作，也可按计划运行（每小时、每天等）。 这两种选项都可用于跨环境（包括本地、云和本地桌面复制）的各连接器。

一次性复制操作仅支持一次性将数据从源移动到目标。 它适用于任何大小的数据和任何受支持的格式。 通过计划的复制，可以按规定的重复周期复制数据。 可使用丰富的设置（例如，重试、超时和警报）来配置计划的复制。

![计划属性](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="troubleshooting"></a>疑难解答

本部分探讨 Azure 数据工厂中复制向导的常见故障排除方法。

> [!NOTE] 
> 这些故障排除提示适用于数据工厂版本1中的复制向导。 有关数据工厂 v2，请参阅 [Azure 数据工厂故障](../data-factory-ux-troubleshoot-guide.md)排除指南。

### <a name="error-code-unable-to-validate-in-copy-wizard"></a>错误代码：无法在复制向导中验证

- **症状**：在复制向导的第一步中，你遇到了 "无法验证" 的警告消息。
- 原因：禁用所有第三方 Cookie 时可能会发生这种情况。
- **解决方法**： 
    - 使用 Internet Explorer 或 Microsoft Edge 浏览器。
    - 如果使用的是 Chrome 浏览器，请按照以下说明为 microsoftonline.com 和 windows.net 添加 Cookie 例外 。
        1.  打开 Chrome 浏览器。
        2.  单击右侧的扳手或三条线（自定义并控制 Google Chrome）。
        3.  单击“设置”。
        4.  在“高级设置”下搜索 Cookie 或转到“隐私” 。
        5.  选择“内容设置”。    
        6.  Cookie 应设置为“允许设置本地数据(推荐)”。
        7.  单击“管理例外”。 在“主机名模式”下输入以下内容，并确保已设置为“允许” 。
            - login.microsoftonline.com
            - login.windows.net
        8.  关闭浏览器并重新启动。
    - 如果使用的是 Firefox 浏览器，请按照以下说明添加 Cookie 例外。
        1. 从 Firefox 菜单中转到“工具” > “选项” 。
        2. 在“隐私” > “历史记录”下，可能会看到当前设置为“对历史记录使用自定义设置”  。
        3. 在“接受第三方 Cookie”中，当前设置可能为“从不”，则应在右侧单击“例外”以添加以下站点  。
            - https://login.microsoftonline.com
            - https://login.windows.net
        4.  关闭浏览器并重新启动。 


### <a name="error-code-unable-to-open-login-page-and-enter-password"></a>错误代码：无法打开登录页并输入密码

- **症状**：复制向导将您重定向到登录页，但登录页面未成功显示。
- 原因：如果将网络环境从办公室网络更改为家庭网络，则可能会发生此问题。 浏览器中有一些缓存。 
- **解决方法**： 
    1.  请关闭浏览器并重试。 如果问题仍然存在，请继续执行下一步。   
    2.  如果使用的是 Internet Explorer 浏览器，请尝试以隐私模式打开它（按 Ctrl+Shift+P）。 如果使用的是 Chrome 浏览器，请尝试以无痕模式打开它（按 Ctrl+Shift+N）。 如果问题仍然存在，请继续执行下一步。 
    3.  尝试使用其他浏览器。 


## <a name="next-steps"></a>后续步骤
有关使用数据工厂复制向导创建具有复制活动的管道的快速演练，请参阅[教程：使用复制向导创建管道](data-factory-copy-data-wizard-tutorial.md)。