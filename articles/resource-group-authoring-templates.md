<properties
   pageTitle="创作 Azure 资源管理器模板"
   description="使用声明性 JSON 语法创建 Azure 资源管理器模板，以将应用程序部署到 Azure。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.date="01/05/2016"
   wacn.date=""/>

# 创作 Azure 资源管理器模板

Azure 应用程序通常需要多种资源的组合（例如数据库服务器、数据库或网站）来达到所需的目标。你无需单独部署和管理每个资源，而可以创建一个 Azure 资源管理器模板，以单个协调操作为应用程序部署和设置所有资源。在模板中，定义应用程序所需的资源，并指定部署参数以针对不同的环境输入值。模板中包含可用于构造部署值的 JSON 和表达式。本主题介绍了该模板的部分。


您必须将您的模版大小限制为 1 MB 以内，每个参数文件大小限制为 64 KB 以内。已完成对迭代资源定义和变量值和参数值的扩展后，1 MB 的限制将适用于该模板的最终状态。

## 规划模板

开始使用模板之前，应该花些时间来确定你想要部署的资源以及使用模板的方式。具体而言，你应该考虑：

1. 需要部署哪些资源类型
2. 这些资源所在的位置
3. 部署资源时使用的资源提供程序 API 版本
4. 是否有任何资源必须部署在其他资源之后
5. 想要在部署期间传入的值，以及想要在模板中直接定义的值
6. 是否需要从部署返回值

为了帮助找出哪些资源类型可供部署、各类型支持的区域，以及每个类型可用的 API 版本，请参阅[资源管理器提供程序、区域、API 版本和架构](resource-manager-supported-services.md)。本主题提供的示例和链接可帮助你判断需要在模板中提供的值。

如果某个资源必须在另一个资源之后部署，你可以将它标记为依赖于其他资源。下面的 [Resources](#resources) 部分介绍了如何执行此操作。

可以在执行期间提供参数值，以改变模板部署的结果。下面的 [Parameters](#parameters) 部分介绍了如何执行此操作。

可以在 [Outputs](#outputs) 节中从部署返回值。

## 模板格式

使用最简单的结构时，模板包含以下元素。

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| 元素名称 | 必选 | 说明
| :------------: | :------: | :----------
| $schema | 是 | 描述模板语言版本的 JSON 架构文件所在的位置。应使用上面所示的 URL。
| contentVersion | 是 | 模板的版本（例如 1.0.0.0）。可为此元素提供任意值。使用模板部署资源时，此值可用于确保使用正确的模板。
| 参数 | 否 | 执行部署以自定义资源部署时提供的值。
| variables | 否 | 在模板中用作 JSON 片段以简化模板语言表达式的值。
| 资源 | 是 | 已在资源组中部署或更新的资源类型。
| outputs | 否 | 部署后返回的值。

本主题稍后将更详细地介绍模板的各个节。现在，我们将回顾构成模板的某些语法。

## 表达式和函数

模板的基本语法为 JSON；但是，表达式和函数扩展了模板中提供的 JSON，使你能够创建严格意义上不是文本值的值。表达式括在方括号（[ 和 ]）中，并在部署模板时进行求值。表达式可以出现在 JSON 字符串值中的任何位置，并始终返回另一个 JSON 值。如果需要使用以方括号 [ 开头的文本字符串，则必须使用两个方括号 [[。

通常，你会将表达式与函数一起使用，以执行用于配置部署的操作。如同在 JavaScript 中一样，函数调用的格式为 **functionName(arg1,arg2,arg3)**。使用点和 [index] 运算符引用属性。

以下示例演示如何在构造值时使用一些函数：
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

有关模板函数的完整列表，请参阅 [Azure 资源管理器模板函数](./resource-group-template-functions.md)。


## Parameters

在模板的 parameters 节中，你可以指定在部署资源时能够输入的值。提供针对特定环境（例如开发、测试和生产环境）定制的参数值可以自定义部署。无需在模板中提供参数，但如果没有参数，模板始终部署具有相同名称、位置和属性的相同资源。

你可以在整个模板中使用这些参数值，来为部署的资源设置值。在模板的其他节中，只能使用 parameters 节中声明的参数。

在 parameters 节中，不能使用一个参数值来构造另一个参数值。在 variables 节中构造新值。

使用以下结构定义参数：

    "parameters": {
       "<parameterName>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<optional-default-value-of-parameter>",
         "allowedValues": [ "<optional-array-of-allowed-values>" ],
         "minValue": <optional-minimum-value-for-int-parameters>,
         "maxValue": <optional-maximum-value-for-int-parameters>,
         "minLength": <optional-minimum-length-for-string-secureString-array-parameters>,
         "maxLength": <optional-maximum-length-for-string-secureString-array-parameters>,
         "metadata": {
             "description": "<optional-description-of-the parameter>" 
         }
       }
    }

| 元素名称 | 必选 | 说明
| :------------: | :------: | :----------
| parameterName | 是 | 参数的名称。必须是有效的 JavaScript 标识符。
| type | 是 | 参数值的类型。请参阅以下允许类型的列表。
| defaultValue | 否 | 参数的默认值，如果没有为参数提供任何值。
| allowedValues | 否 | 用来确保提供正确值的参数的允许值数组。
| minValue | 否 | int 类型参数的最小值，此值是包容性的。
| maxValue | 否 | int 类型参数的最大值，此值是包容性的。
| minLength | 否 | string、secureString 和 array 类型参数的最小长度，此值是包容性的。
| maxLength | 否 | string、secureString 和 array 类型参数的最大长度，此值是包容性的。
| description | 否 | 通过门户自定义模板界面向模板用户显示的参数描述。

允许的类型和值是：

- 字符串或 secureString - 任何有效的 JSON 字符串
- int - 任何有效的 JSON 整数
- 布尔值 - 任何有效的 JSON 布尔值
- 对象或 secureObject - 任何有效的 JSON 对象
- 数组 - 任何有效的 JSON 数组

若要将某个参数指定为可选，请将其 defaultValue 指定为空字符串。

>[AZURE.NOTE] 所有密码、密钥和其他机密信息应使用 **secureString** 类型。部署资源后，无法读取使用 secureString 类型的模板参数。

以下示例演示如何定义参数：

    "parameters": {
       "siteName": {
          "type": "string",
          "minLength": 2,
          "maxLength": 60
       },
       "siteLocation": {
          "type": "string",
          "minLength": 2
       },
       "hostingPlanName": {
          "type": "string"
       },  
       "hostingPlanSku": {
          "type": "string",
          "allowedValues": [
            "Free",
            "Shared",
            "Basic",
            "Standard",
            "Premium"
          ],
          "defaultValue": "Free"
       },
       "instancesCount": {
          "type": "int",
          "maxValue": 10
       },
       "numberOfWorkers": {
          "type": "int",
          "minValue": 1
       }
    }

## 变量

在 variables 节中构造可在整个模板中使用的值。通常，这些变量基于通过参数提供的值。不需要定义变量，但使用变量可以减少复杂的表达式，从而简化模板。

使用以下结构定义变量：

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

以下示例演示如何定义从两个参数值构造出的变量：

    "parameters": {
       "username": {
         "type": "string"
       },
       "password": {
         "type": "secureString"
       }
     },
     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

下一个示例演示一个属于复杂的 JSON 类型的变量，以及从其他变量构造出的变量：

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount"
    }

## 资源

在 resources 节，可以定义部署或更新的资源。模板中的此位置可能比较复杂，因为你必须了解要部署哪些类型才能提供正确的值。若要进一步了解资源提供程序，请参阅[资源管理器提供程序、区域、API 版本和架构](/documentation/articles/resource-manager-supported-services)。

使用以下结构定义资源：

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| 元素名称 | 必选 | 说明
| :----------------------: | :------: | :----------
| apiVersion | 是 | 用于创建资源的 REST API 版本。若要确定可用于特定资源类型的版本号，请参阅[支持的 API 版本](../resource-manager-supported-services/#supported-api-versions)。
| type | 是 | 资源的类型。此值是资源提供程序的命名空间以及资源提供程序支持的资源类型的组合。
| name | 是 | 资源的名称。该名称必须遵循 RFC3986 中定义的 URI 构成部分限制。
| location | 否 | 提供的资源支持的地理位置。若要确定可用的位置，请参阅[支持的区域](../resource-manager-supported-services/#supported-regions)。
| 标记 | 否 | 与资源关联的标记。
| 注释 | 否 | 用于描述模板中资源的注释
| dependsOn | 否 | 正在定义的资源所依赖的资源。将会评估资源之间的依赖关系，并按资源的依赖顺序来部署资源。如果资源不相互依赖，则会尝试并行部署资源。该值可以是资源名称或资源唯一标识符的逗号分隔列表。
| properties | 否 | 特定于资源的配置设置。properties 的值与你在创建资源时，在 REST API 操作（PUT 方法）的请求正文中提供的值完全相同。有关资源架构文档或 REST API 的链接，请参阅[资源管理器提供程序、区域、API 版本和架构](resource-manager-supported-services.md)。
| 资源 | 否 | 依赖于所定义的资源的子资源。只能提供父资源的架构允许的资源类型。子资源类型的完整名称包含父资源的名称，例如 **Microsoft.Web/sites/extensions**。对父资源的依赖性不是隐式的；你必须显式定义该依赖性。 


如果资源名称不是唯一的，你可以使用 **resourceId** 帮助器函数（下面将会介绍）获取任何资源的唯一标识符。

resources 节包含要部署的资源数组。在每个资源内，还可以定义该资源的子资源数组。因此，resources 节的结构可能类似于：

    "resources": [
       {
           "name": "resourceA",
           ...
       },
       {
           "name": "resourceB",
           ...
           "resources": [
               {
                   "name": "firstChildResourceB",
                   ...
               },
               {   
                   "name": "secondChildResourceB",
                   ...
               }
           ]
       },
       {
           "name": "resourceC",
           ...
       }
    ]



以下示例演示了 **Microsoft.Web/serverfarms** 资源，以及一个包含子 **Extensions** 资源的 **Microsoft.Web/sites** 资源。请注意，站点标记为依赖于服务器场，因为只有该服务器场存在，才能部署该站点。另请注意，**Extensions** 资源是站点的子级。

    "resources": [
        {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/serverfarms",
          "name": "[parameters('hostingPlanName')]",
          "location": "[resourceGroup().location]",
          "properties": {
              "name": "[parameters('hostingPlanName')]",
              "sku": "[parameters('hostingPlanSku')]",
              "workerSize": "0",
              "numberOfWorkers": 1
          }
        },
        {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          "tags": {
              "environment": "test",
              "team": "ARM"
          },
          "dependsOn": [
              "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
          ],
          "properties": {
              "name": "[parameters('siteName')]",
              "serverFarm": "[parameters('hostingPlanName')]"
          },
          "resources": [
              {
                  "apiVersion": "2014-06-01",
                  "type": "Extensions",
                  "name": "MSDeploy",
                  "dependsOn": [
                      "[resourceId('Microsoft.Web/sites', parameters('siteName'))]"
                  ],
                  "properties": {
                    "packageUri": "https://auxmktplceprod.blob.core.chinacloudapi.cn/packages/StarterSite-modified.zip",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                      "Application Path": "[parameters('siteName')]"
                    }
                  }
              }
          ]
        }
    ]


## Outputs

在 Outputs 节中，可以指定从部署返回的值。例如，可能会返回用于访问已部署资源的 URI。

以下示例演示了输出定义的结构：

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>",
       }
    }

| 元素名称 | 必选 | 说明
| :------------: | :------: | :----------
| outputName | 是 | 输出值的名称。必须是有效的 JavaScript 标识符。
| type | 是 | 输出值的类型。输出值支持的类型与模板输入参数相同。
| value | 是 | 要评估并作为输出值返回的模板语言表达式。


以下示例演示了 Outputs 节中返回的值。

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

## 更高级方案。
本主题提供了有关模板的简介。但是，你的方案可能需要更高级的任务。

你可能需要将两个模板合并在一起，或者在父模板中使用子模板。有关详细信息，请参阅[将链接的模板与 Azure 资源管理器配合使用](/documentation/articles/resource-group-linked-templates)。

若要在创建资源类型时迭代指定的次数，请参阅[在 Azure 资源管理器中创建多个资源实例](/documentation/articles/resource-group-create-multiple)。

你可能需要使用不同资源组中的资源。使用跨多个资源组共享的存储帐户或虚拟网络时，这很常见。有关详细信息，请参阅 [resourceId 函数](../resource-group-template-functions#resourceid)。

## 完整的模板
以下模板将部署一个 Web 应用程序，并使用 .zip 文件中的代码设置该应用程序。

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "siteName": {
           "type": "string"
         },
         "hostingPlanName": {
           "type": "string"
         },
         "hostingPlanSku": {
           "type": "string",
           "allowedValues": [
             "Free",
             "Shared",
             "Basic",
             "Standard",
             "Premium"
           ],
           "defaultValue": "Free"
         }
       },
       "resources": [
         {
           "apiVersion": "2014-06-01",
           "type": "Microsoft.Web/serverfarms",
           "name": "[parameters('hostingPlanName')]",
           "location": "[resourceGroup().location]",
           "properties": {
             "name": "[parameters('hostingPlanName')]",
             "sku": "[parameters('hostingPlanSku')]",
             "workerSize": "0",
             "numberOfWorkers": 1
           }
         },
         {
           "apiVersion": "2014-06-01",
           "type": "Microsoft.Web/sites",
           "name": "[parameters('siteName')]",
           "location": "[resourceGroup().location]",
           "tags": {
             "environment": "test",
             "team": "ARM"
           },
           "dependsOn": [
             "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
           ],
           "properties": {
             "name": "[parameters('siteName')]",
             "serverFarm": "[parameters('hostingPlanName')]"
           },
           "resources": [
             {
               "apiVersion": "2014-06-01",
               "type": "Extensions",
               "name": "MSDeploy",
               "dependsOn": [
                 "[resourceId('Microsoft.Web/sites', parameters('siteName'))]"
               ],
               "properties": {
                 "packageUri": "https://auxmktplceprod.blob.core.chinacloudapi.cn/packages/StarterSite-modified.zip",
                 "dbType": "None",
                 "connectionString": "",
                 "setParameters": {
                   "Application Path": "[parameters('siteName')]"
                 }
               }
             }
           ]
         }
       ],
       "outputs": {
         "siteUri": {
           "type": "string",
           "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
         }
       }
    }

## 后续步骤
- 有关你可以使用的来自模板中的函数的详细信息，请参阅 [Azure 资源管理器模板函数](/documentation/articles/resource-group-template-functions)
- 若要查看如何部署已创建的模板，请参阅[使用 Azure 资源管理器模板部署应用程序](/documentation/articles/resource-group-template-deploy)
- 有关部署应用程序的详细示例，请参阅[按可预见的方式在 Azure 中预配和部署微服务](app-service-web/app-service-deploy-complex-application-predictably.md)
- 若要查看可用架构，请参阅 [Azure 资源管理器架构](https://github.com/Azure/azure-resource-manager-schemas)

<!---HONumber=Mooncake_0215_2016-->