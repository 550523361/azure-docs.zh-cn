---
title: 理解查询语言
description: 介绍 Resource Graph 表和所提供的可以与 Azure Resource Graph 配合使用的 Kusto 数据类型、运算符和函数。
ms.date: 03/07/2020
ms.topic: conceptual
ms.openlocfilehash: 2f4be4d86a340867e1ad3015ff288f98fc54cecf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "78927500"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>了解 Azure Resource Graph 查询语言

Azure Resource Graph 查询语言支持多个运算符和函数。 每个运算符和函数的工作原理和操作方式基于 [Kusto 查询语言 (KQL)](/azure/kusto/query/index)。 若要了解 Resource Graph 使用的查询语言，请从 [KQL 教程](/azure/kusto/query/tutorial)着手。

本文介绍 Resource Graph 支持的语言组件：

- [Resource Graph 表](#resource-graph-tables)
- [支持的 KQL 语言元素](#supported-kql-language-elements)
- [转义字符](#escape-characters)

## <a name="resource-graph-tables"></a>Resource Graph 表

Resource Graph 为其存储的有关资源管理器资源类型及其属性的数据提供多个表。 这些表可以与 `join` 或 `union` 运算符配合使用，以便从相关资源类型获取属性。 下面是 Resource Graph 中提供的表的列表：

|Resource Graph 表 |描述 |
|---|---|
|资源 |默认表（如果查询中未定义任何表）。 大多数资源管理器资源类型和属性都在这里。 |
|ResourceContainers |包括订阅（预览中`Microsoft.Resources/subscriptions`） 和资源组`Microsoft.Resources/subscriptions/resourcegroups`（ ） 资源类型和数据。 |
|顾问资源 |包含与__`Microsoft.Advisor` 相关的资源。 |
|AlertsManagementResources |包含与__`Microsoft.AlertsManagement` 相关的资源。 |
|维护资源 |包含与__`Microsoft.Maintenance` 相关的资源。 |
|SecurityResources |包含与__`Microsoft.Security` 相关的资源。 |

有关包含资源类型的完整列表，请参阅[参考：支持的表和资源类型](../reference/supported-tables-resources.md)。

> [!NOTE]
> _Resources_ 是默认表。 查询 _Resources_ 表时，不需提供表名，除非使用了 `join` 或 `union`。 不过，建议的做法是始终在查询中包含初始表。

在门户中使用 Resource Graph 资源管理器，以便发现在每个表中提供的具体资源类型。 也可使用 `<tableName> | distinct type` 之类的查询来获取给定 Resource Graph 表支持的、存在于环境中的资源类型的列表。

以下查询显示了一个简单的 `join`。 查询结果将列混合在一起，联接的表（在此示例中为 _ResourceContainers_）中的任何重复列名都会追加 **1**。 由于 _ResourceContainers_ 表有用于订阅的类型和用于资源组的类型，因此可以使用任一类型来联接到 _resources_ 表中的资源。

```kusto
Resources
| join ResourceContainers on subscriptionId
| limit 1
```

以下查询显示了 `join` 的更复杂用法。 查询将联接表限制为订阅资源并具有 `project`，以仅包括原始字段 _SubscriptionId_ 和重命名为 _SubName_ 的 _name_ 字段。 字段重命名避免将其`join`添加为_name1，_ 因为该字段已存在于_参考资料_中。 原始表使用 `where` 进行筛选，以下 `project` 包括两个表中的列。 查询结果是单个密钥保管库，其中显示密钥保管库的类型、名称以及其所在的订阅的名称。

```kusto
Resources
| where type == 'microsoft.keyvault/vaults'
| join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId
| project type, name, SubName
| limit 1
```

> [!NOTE]
> 使用 `project` 来限制 `join` 结果时，`join` 所使用的将两个表关联在一起的属性（在上面的示例中为 _subscriptionId_）必须包含在 `project` 中。

## <a name="supported-kql-language-elements"></a>支持的 KQL 语言元素

Resource Graph 支持所有 KQL [数据类型](/azure/kusto/query/scalar-data-types/)、[标量函数](/azure/kusto/query/scalarfunctions)、[标量运算符](/azure/kusto/query/binoperators)和[聚合函数](/azure/kusto/query/any-aggfunction)。 特定的[表格运算符](/azure/kusto/query/queries)受 Resource Graph 的支持，其中的一部分有不同的行为。

### <a name="supported-tabulartop-level-operators"></a>支持的表格/顶级运算符

下面是一个列表，其中包含 Resource Graph 支持的 KQL 表格运算符和特定示例：

|KQL |Resource Graph 示例查询 |说明 |
|---|---|---|
|[count](/azure/kusto/query/countoperator) |[对密钥保管库计数](../samples/starter.md#count-keyvaults) | |
|[distinct](/azure/kusto/query/distinctoperator) |[显示特定别名的不同值](../samples/starter.md#distinct-alias-values) | |
|[扩展](/azure/kusto/query/extendoperator) |[按操作系统类型对虚拟机进行计数](../samples/starter.md#count-os) | |
|[加入](/azure/kusto/query/joinoperator) |[具有订阅名称的密钥保管库](../samples/advanced.md#join) |支持的联接风格：[innerunique](/azure/kusto/query/joinoperator#default-join-flavor)、[inner](/azure/kusto/query/joinoperator#inner-join)、[leftouter](/azure/kusto/query/joinoperator#left-outer-join)。 单个查询中存在 3 个 `join` 的限制。 不允许自定义联接策略，例如广播联接。 可以用在单个表中，也可以用在 _Resources_ 表和 _ResourceContainers_ 表之间。 |
|[限制](/azure/kusto/query/limitoperator) |[列出所有公共 IP 地址](../samples/starter.md#list-publicip) |`take` 的同义词 |
|[mv 展开](/azure/kusto/query/mvexpandoperator) | | 旧运算符，请改用 `mv-expand`。 _RowLimit_ 最大值：400。 默认值为 128。 |
|[mv 扩展](/azure/kusto/query/mvexpandoperator) |[列出具有特定写入位置的宇宙 DB](../samples/advanced.md#mvexpand-cosmosdb) |_RowLimit_ 最大值：400。 默认值为 128。 |
|[以](/azure/kusto/query/orderoperator) |[列出按名称排序的资源](../samples/starter.md#list-resources) |`sort` 的同义词 |
|[项目](/azure/kusto/query/projectoperator) |[列出按名称排序的资源](../samples/starter.md#list-resources) | |
|[project-away](/azure/kusto/query/projectawayoperator) |[从结果中删除列](../samples/advanced.md#remove-column) | |
|[排序](/azure/kusto/query/sortoperator) |[列出按名称排序的资源](../samples/starter.md#list-resources) |`order` 的同义词 |
|[总结](/azure/kusto/query/summarizeoperator) |[计算 Azure 资源](../samples/starter.md#count-resources) |仅限简化的第一页 |
|[采取](/azure/kusto/query/takeoperator) |[列出所有公共 IP 地址](../samples/starter.md#list-publicip) |`limit` 的同义词 |
|[返回页首](/azure/kusto/query/topoperator) |[按名称及其操作系统类型显示前五个虚拟机](../samples/starter.md#show-sorted) | |
|[联盟](/azure/kusto/query/unionoperator) |[将两个查询的结果合并为单个结果](../samples/advanced.md#unionresults) |允许的单一表 _：T_`| union`\[`kind=``inner`\|`outer`\]_Table_列名称表 。 _ColumnName_ \] \[ `withsource=` 单个查询中存在 3 个 `union` 支线的限制。 不允许对 `union` 支线表进行模糊解析。 可以用在单个表中，也可以用在 _Resources_ 表和 _ResourceContainers_ 表之间。 |
|[where](/azure/kusto/query/whereoperator) |[显示包含存储的资源](../samples/starter.md#show-storage) | |

## <a name="escape-characters"></a>转义字符

某些属性名称（例如那些包含 `.` 或 `$` 的属性名称）必须在查询中进行包装或转义，否则就会解释不正确，无法提供预期的结果。

- `.` - 将属性名称包装成这样：`['propertyname.withaperiod']`
  
  包装 _odata.type_ 属性的示例查询：

  ```kusto
  where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.['odata.type']
  ```

- `$` - 对属性名称中的字符进行转义。 所使用的转义字符取决于 Resource Graph 从哪个 shell 运行。

  - **Bash** - `\`

    在 bash 中转义属性_\$类型_的示例查询：

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.\$type
    ```

  - **cmd** - 不对 `$` 字符进行转义。

  - **电源外壳** - ``` ` ```

    转义 PowerShell 中的属性_\$类型_的示例查询：

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.`$type
    ```

## <a name="next-steps"></a>后续步骤

- 请参阅[初学者查询](../samples/starter.md)中正在使用的语言。
- 请参阅[高级查询](../samples/advanced.md)中的高级用途。
- 详细了解如何[浏览资源](explore-resources.md)。