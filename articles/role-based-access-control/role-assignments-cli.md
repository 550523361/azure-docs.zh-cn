---
title: 使用 RBAC 和 Azure CLI 管理对 Azure 资源的访问权限 | Microsoft Docs
description: 了解如何使用基于角色的访问控制 (RBAC) 和 Azure CLI 来管理用户、组和应用程序对 Azure 资源的访问权限。 这包括如何列出访问权限、授予访问权限以及删除访问权限。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 8e75a6344e517fb0343343f557cb7211f49cfed8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/18/2019
ms.locfileid: "57838302"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-cli"></a>使用 RBAC 和 Azure CLI 管理对 Azure 资源的访问权限

可以通过[基于角色的访问控制 (RBAC)](overview.md) 方式管理对 Azure 资源的访问权限。 本文介绍如何使用 RBAC 和 Azure CLI 来管理用户、组和应用程序的访问权限。

## <a name="prerequisites"></a>必备组件

若要管理访问权限，需要以下项之一：

* [Bash in Azure Cloud Shell](/azure/cloud-shell/overview)
* [Azure CLI](/cli/azure)

## <a name="list-roles"></a>列出角色

若要列出所有可用的角色定义，请使用 [az role definition list](/cli/azure/role/definition#az-role-definition-list)：

```azurecli
az role definition list
```

以下示例列出了所有可用的角色定义的名称和说明：

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.roleName, "description":.description}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs"
}

...
```

下面的示例列出了所有内置的角色定义：

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.roleName, "description":.description, "roleType":.roleType}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "roleType": "BuiltInRole"
}

...
```

## <a name="list-a-role-definition"></a>列出角色定义

若要列出角色定义，请使用 [az role definition list](/cli/azure/role/definition#az-role-definition-list)：

```azurecli
az role definition list --name <role_name>
```

下面的示例列出了“参与者”角色定义：

```azurecli
az role definition list --name "Contributor"
```

```Output
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

### <a name="list-actions-of-a-role"></a>列出角色的操作

以下示例仅列出了“参与者”角色的 actions 和 notActions：

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.permissions[0].actions, "notActions":.permissions[0].notActions}'
```

```Output
{
  "actions": [
    "*"
  ],
  "notActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ]
}
```

以下示例仅列出了“虚拟机参与者”角色的 actions：

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .permissions[0].actions'
```

```Output
[
  "Microsoft.Authorization/*/read",
  "Microsoft.Compute/availabilitySets/*",
  "Microsoft.Compute/locations/*",
  "Microsoft.Compute/virtualMachines/*",
  "Microsoft.Compute/virtualMachineScaleSets/*",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
  "Microsoft.Network/loadBalancers/backendAddressPools/join/action",

  ...

  "Microsoft.Storage/storageAccounts/listKeys/action",
  "Microsoft.Storage/storageAccounts/read"
]
```

## <a name="list-access"></a>列出访问权限

在 RBAC 中，若要列出访问权限，请列出角色分配。

### <a name="list-role-assignments-for-a-user"></a>为用户列出角色分配

若要列出特定用户的角色分配，请使用 [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list)：

```azurecli
az role assignment list --assignee <assignee>
```

默认情况下，只会列出局限于订阅的分配。 若要查看按资源或组来确定范围的分配，请使用 `--all`。

以下示例列出了直接分配给角色分配*patlong\@contoso.com*用户：

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
```

### <a name="list-role-assignments-for-a-resource-group"></a>列出资源组的角色分配

若要列出存在的针对资源组的角色分配，请使用 [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list)：

```azurecli
az role assignment list --resource-group <resource_group>
```

以下示例列出 *pharma-sales-projectforecast* 资源组的角色分配：

```azurecli
az role assignment list --resource-group pharma-sales-projectforecast --output json | jq '.[] | {"roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}

...
```

## <a name="grant-access"></a>授予访问权限

在 RBAC 中，若要授予访问权限，请创建角色分配。

### <a name="create-a-role-assignment-for-a-user"></a>为用户创建角色分配

若要为资源组范围的用户创建角色分配，请使用 [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create)：

```azurecli
az role assignment create --role <role> --assignee <assignee> --resource-group <resource_group>
```

以下示例将分配*虚拟机参与者*角色*patlong\@contoso.com*用户*医药销售 projectforecast*资源组范围内：

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales-projectforecast
```

### <a name="create-a-role-assignment-for-a-group"></a>为组创建角色分配

若要为组创建角色分配，请使用 [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create)：

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

以下示例将“读者”角色分配给订阅范围内 ID 为 22222222-2222-2222-2222-222222222222 的“Ann Mack 团队”组。 若要获取组的 ID，可以使用 [az ad group list](/cli/azure/ad/group#az-ad-group-list) 或 [az ad group show](/cli/azure/ad/group#az-ad-group-show)。

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

以下示例将“虚拟机参与者”角色分配给名为 *pharma-sales-project-network* 的虚拟网络的资源范围内 ID 为 22222222-2222-2222-2222-222222222222 的“Ann Mack 团队”组：

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111/resourcegroups/pharma-sales-projectforecast/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="create-a-role-assignment-for-an-application"></a>为应用程序创建角色分配

若要为应用程序创建角色，请使用 [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create)：

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

以下示例将“虚拟机参与者”角色分配给 *pharma-sales-projectforecast* 资源组范围内对象 ID 为 44444444-4444-4444-4444-444444444444 的应用程序。 若要获取应用程序的对象 ID，可以使用 [az ad app list](/cli/azure/ad/app#az-ad-app-list) 或 [az ad app show](/cli/azure/ad/app#az-ad-app-show)。

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales-projectforecast
```

## <a name="remove-access"></a>删除访问权限

在 RBAC 中，若要删除访问权限，请使用 [az role assignment delete](/cli/azure/role/assignment#az-role-assignment-delete) 删除角色分配：

```azurecli
az role assignment delete --assignee <assignee> --role <role> --resource-group <resource_group>
```

下面的示例移除*虚拟机参与者*角色分配从*patlong\@contoso.com*上的用户*医药销售 projectforecast*资源组：

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales-projectforecast
```

以下示例将“读者”角色从订阅范围内 ID 为 22222222-2222-2222-2222-222222222222 的“Ann Mack 团队”组删除。 若要获取组的 ID，可以使用 [az ad group list](/cli/azure/ad/group#az-ad-group-list) 或 [az ad group show](/cli/azure/ad/group#az-ad-group-show)。

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="next-steps"></a>后续步骤

- [教程：使用 Azure CLI 为 Azure 资源创建自定义角色](tutorial-custom-role-cli.md)
- [使用 Azure CLI 管理 Azure 资源和资源组](../azure-resource-manager/cli-azure-resource-manager.md)