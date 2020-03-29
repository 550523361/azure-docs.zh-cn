---
title: 在 Azure 缩放集模板中引用现有虚拟网络
description: 如何将虚拟网络添加到现有 Azure 虚拟机规模集模板
author: mayanknayar
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: manayar
ms.openlocfilehash: e725e75b8b19fd8b3295226639b5e5aeb3736e34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "76275536"
---
# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>在 Azure 规模集模板中添加对现有虚拟网络的引用

本文介绍了如何修改[基本规模集模板](virtual-machine-scale-sets-mvss-start.md)，以便将其部署到现有虚拟网络而非创建新的虚拟网络。

## <a name="change-the-template-definition"></a>更改模板定义

在[此前的文章](virtual-machine-scale-sets-mvss-start.md)中，我们创建了基本的规模集模板。 我们现在将使用这个此前的模板并对其进行修改，以便创建一个模板来将规模集部署到现有的虚拟网络中。 

首先，添加一个 `subnetId` 参数。 此字符串将传递到规模集配置，使得规模集能够识别要将虚拟机部署到的预先创建的子网。 此字符串必须采用以下格式：`/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`

例如，要将规模集部署到具有名称 `myvnet`、子网 `mysubnet`、资源组 `myrg` 和订阅 `00000000-0000-0000-0000-000000000000` 的现有虚拟网络，则 subnetId 将是：`/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`。

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

接下来，从 `resources` 阵列中删除虚拟网络资源，由于使用现有虚拟网络，因此不需要部署新的虚拟网络。

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2018-11-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

虚拟网络在部署模板前已存在，因此不需要指定从规模集到虚拟网络的 dependsOn 子句。 删除以下行：

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2019-03-01",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

最后，传入用户设置的 `subnetId` 参数（而非使用 `resourceId` 获取同一部署中某个 vnet 的 ID，这是基本可行规模集模板执行的操作）。

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>后续步骤

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
