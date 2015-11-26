<properties
	pageTitle="Azure 资源管理器策略 | Microsoft Azure"
	description="介绍如何使用 Azure 资源管理器策略来防止订阅、资源组或单个资源等不同的范围发生冲突。"
	services="azure-resource-manager"
	documentationCenter="na"
	authors="ravbhatnagar"
	manager="ryjones"
	editor=""/>

<tags
	ms.service="azure-resource-manager"
	ms.date="10/06/2015"
	wacn.date="09/15/2015"/>

# 使用策略来管理资源和控制访问

Azure 资源管理器现在可让你通过自定义策略来控制访问。策略可在所需范围内防止的一项或多项违规。在此语境下，范围可以是订阅、资源组或单个资源。

策略是默认允许系统。策略将通过“策略定义”来定义，并通过“策略分配”来应用。策略分配可让你控制策略的应用范围。

在本文中，我们将说明可用于创建策略的策略定义语言的基本结构。然后说明如何在不同范围应用这些策略，最后演示有关如何通过 REST API 实现此目的的一些示例。不久之后，我们将增加 PowerShell 支持。

## 常见方案

一个常见方案是为了费用分摊而要求提供部门标记。组织可能只想在有关联的适当成本中心时允许操作，否则将拒绝请求。这将帮助它们向适当的成本中心对所执行的操作收费。

另一个常见方案是组织可能想要控制创建资源的位置。或者它们可能想要通过仅允许预配特定类型的资源，来控制对资源的访问。

同样地，组织可以控制服务类别或为资源强制运行所需的命名约定。

使用策略可轻松实现这些方案，如下所述。

## 策略定义结构

策略定义是使用 JSON 创建的。它包含定义操作和效果的一个或多个条件/逻辑运算符，告诉你满足条件时产生的效果。

简单而言，策略包含以下各项：

**条件/逻辑运算符：**包含可通过一组逻辑运算符操作的条件集。

**效果：**描述当满足条件时产生的效果 – 拒绝或审核。审核效果将发出警告事件服务日志。例如，管理员可以创建策略，如果有任何人创建大型 VM，则此策略会引发审核，然后管理员可以审查日志。

    {
      "if" : {
        <condition> | <logical operator>
      },
      "then" : {
        "effect" : "deny | audit"
      }
    }


## 逻辑运算符

下面列出了支持的逻辑运算符和语法：

| 运算符名称 | 语法 |
| :------------- | :------------- |
| Not | "not" : {&lt;condition&gt;} |
| And | "allOf" : [ {&lt;condition1&gt;},{&lt;condition2&gt;}] |
| 或 | "anyOf" : [ {&lt;condition1&gt;},{&lt;condition2&gt;}] |


## 条件

下面列出了支持的条件和语法：

| 条件名称 | 语法 |
| :------------- | :------------- |
| 等于 | "equals" : "&lt;value&gt;" |
| Like | "like" : "&lt;value&gt;" |
| Contains | "contains" : "&lt;value&gt;"|
| In | "in" : [ "&lt;value1&gt;","&lt;value2&gt;" ]|
| ContainsKey | "containsKey" : "&lt;keyName&gt;" |


## 字段和源

条件是使用字段和源构成的。支持以下字段和源：

字段：**name**、**kind**、**type**、**location**、**tags**、**tags.***。

源：**action**

## 策略定义示例

现在让我们看一下如何定义策略，以实现以上所列的方案。

### 费用分摊：要求提供部门标记

以下策略将拒绝不包含“costCenter”键标记的所有请求。

    {
      "if": {
        "not" : {
          "field" : "tags",
          "containsKey" : "costCenter"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }


### 地区法规遵循：确保资源位置

以下示例显示的策略将拒绝位置不是欧洲北部或欧洲西部的所有请求。

    {
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### 服务策展：选择服务目录

以下示例演示如何使用源。它显示只允许对 Microsoft.Resources/、Microsoft.Compute/*、Microsoft.Storage/*、Microsoft.Network/ 类型的服务执行的操作。其他任何请求将被拒绝。

    {
      "if" : {
        "not" : {
          "anyOf" : [
            {
              "source" : "action",
              "like" : "Microsoft.Resources/*"
            },
            {
              "source" : "action",
              "like" : "Microsoft.Compute/*"
            },
            {
              "source" : "action",
              "like" : "Microsoft.Storage/*"
            },
            {
              "source" : "action",
              "like" : "Microsoft.Network/*"
            }
          ]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### 命名约定

以下示例演示如何使用“like”条件支持的通配符。该条件指明，如果名称符合所述模式 (namePrefix*nameSuffix)，则拒绝请求。

    {
      "if" : {
        "not" : {
          "field" : "name",
          "like" : "namePrefix*nameSuffix"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

## 策略分配

策略可在不同的范围应用，如订阅、资源组和单个资源。策略由所有子资源继承。因此，如果将策略应用到资源组，则会将它应用到该资源组中的所有资源。

## 创建策略

本部分提供有关如何使用 REST API 创建策略的详细信息。

### 使用 REST API 创建策略定义

你可以使用[用于策略定义的 REST API](https://msdn.microsoft.com/library/azure/mt588471.aspx) 来创建策略。REST API 可让你创建和删除策略定义，以及获取现有定义的信息。

若要创建新策略，请运行：

    PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

使用类似于下面的请求正文：

    {
      "properties":{
        "policyType":"Custom",
        "description":"Test Policy",
        "policyRule":{
          "if" : {
            "not" : {
              "field" : "tags",
              "containsKey" : "costCenter"
            }
          },
          "then" : {
            "effect" : "deny"
          }
        }
      },
      "id":"/subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/testdefinition",
      "type":"Microsoft.Authorization/policyDefinitions",
      "name":"testdefinition"
    }


策略定义可以定义为如上所示的示例之一。对于 api-version，请使用 *2015-10-01-preview*。有关示例与其他详细信息，请参阅[用于策略定义的 REST API](https://msdn.microsoft.com/library/azure/mt588471.aspx)。

### 使用 PowerShell 创建策略定义

可以使用 New-AzureRmPolicyDefinition cmdlet 创建新的策略定义，如下所示。以下示例将创建一个策略，只允许欧洲北部和欧洲西部的资源。

    $policy = New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation onlyin certain regions" -Policy '{	"if" : {
    	    			    "not" : {
    	      			    	"field" : "location",
    	      			    		"in" : ["northeurope" , "westeurope"]
    	    			    	}
    	    		          },
    	      		    		"then" : {
    	    			    		"effect" : "deny"
    	      			    		}
    	    		    	}'    		

执行输出将存储在 $policy 对象中，以便稍后可在分配策略期间使用。对于策略参数，也可以提供包含策略的 .json 文件的路径，而不是指定内联策略，如下所示。

    New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain 	regions" -Policy "path-to-policy-json-on-disk"


## 应用策略

### 使用 REST API 分配策略

可以通过[用于策略分配的 REST API](https://msdn.microsoft.com/library/azure/mt588466.aspx)，在所需范围内应用策略定义。REST API 可让你创建和删除策略分配，以及获取现有分配的信息。

若要创建新的策略分配，请运行：

    PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}

{policy-assignment} 是策略分配的名称。对于 api-version，请使用 *2015-10-01-preview*。

使用类似于下面的请求正文：

    {
      "properties":{
        "displayName":"VM_Policy_Assignment",
        "policyDefinitionId":"/subscriptions/########/providers/Microsoft.Authorization/policyDefinitions/testdefinition",
        "scope":"/subscriptions/########-####-####-####-############"
      },
      "id":"/subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyAssignments/VMPolicyAssignment",
      "type":"Microsoft.Authorization/policyAssignments",
      "name":"VMPolicyAssignment"
    }

有关示例与其他详细信息，请参阅[用于策略分配的 REST API](https://msdn.microsoft.com/library/azure/mt588466.aspx)。

### 使用 PowerShell 分配策略

你可以使用 New-AzureRmPolicyAssignment cmdlet 将上面通过 PowerShell 创建的策略应用到所需的范围，如下所示：

    New-AzureRmPolicyAssignment -Name regionPolicyAssignment -PolicyDefinition $policy -Scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
此处的 $policy 是执行 New-AzureRmPolicyDefinition cmdlet 后返回的策略对象，如下所示。此处的范围是指定的资源组的名称。

如果你要删除上述策略分配，可执行以下操作：

    Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

可以分别通过 Get-AzureRmPolicyDefinition、Set-AzureRmPolicyDefinition 和 Remove-AzureRmPolicyDefinition cmdlet 来获取、更改或删除策略定义。

同样地，可以分别通过 Get-AzureRmPolicyAssignment、Set-AzureRmPolicyAssignment 和 Remove-AzureRmPolicyAssignment 来获取、更改或删除策略分配。

<!---HONumber=82-->