---
title: 升级 Azure Kubernetes 服务 (AKS) 节点映像
description: 了解如何升级 AKS 群集节点和节点池上的映像。
ms.service: container-service
ms.topic: conceptual
ms.date: 08/17/2020
ms.openlocfilehash: b6abb0eb98e2548e53ff67a943970613e6981c2b
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92631115"
---
# <a name="azure-kubernetes-service-aks-node-image-upgrade"></a>Azure Kubernetes 服务 (AKS) 节点映像升级

AKS 支持升级节点上的映像，以便你能够获取最新的操作系统和运行时更新。 AKS 每周提供一个带有最新更新的新映像，因此，建议定期升级节点的映像以使用最新功能，包括 Linux 或 Windows 补丁。 本文介绍了在不升级 Kubernetes 版本的情况下如何升级 AKS 群集节点映像以及如何更新节点池映像。

如果需要了解 AKS 提供的最新映像，请参阅 [AKS 发行说明](https://github.com/Azure/AKS/releases)以获取更多详细信息。

有关如何升级群集的 Kubernetes 版本的信息，请参阅[升级 AKS 群集][upgrade-cluster]。

## <a name="limitations"></a>限制

* AKS 群集必须对节点使用虚拟机规模集。

## <a name="install-the-aks-cli-extension"></a>安装 AKS CLI 扩展

在下一个核心 CLI 版本发布之前，你需要具备 aks-preview CLI 扩展才能使用节点映像升级。 使用 [az extension add][az-extension-add] 命令，然后使用 [az extension update][az-extension-update] 命令来查找任何可用的更新：

```azurecli
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="upgrade-all-nodes-in-all-node-pools"></a>升级所有节点池中的所有节点

升级节点映像是使用 `az aks upgrade` 来完成的。 若要升级节点映像，请使用以下命令：

```azurecli
az aks upgrade \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-image-only
```

在升级过程中，请使用下面的 `kubectl` 命令来检查节点映像的状态，以获取标签并筛选出当前节点映像信息：

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

升级完成后，请使用 `az aks show` 来获取更新后的节点池详细信息。 当前节点映像在 `nodeImageVersion` 属性中显示。

```azurecli
az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster
```

## <a name="upgrade-a-specific-node-pool"></a>升级特定节点池

升级节点池上的映像类似于升级群集上的映像。

若要在不执行 Kubernetes 群集升级的情况下更新节点池的操作系统映像，请使用以下示例中的 `--node-image-only` 选项：

```azurecli
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-image-only
```

在升级过程中，请使用下面的 `kubectl` 命令来检查节点映像的状态，以获取标签并筛选出当前节点映像信息：

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

升级完成后，请使用 `az aks nodepool show` 来获取更新后的节点池详细信息。 当前节点映像在 `nodeImageVersion` 属性中显示。

```azurecli
az aks nodepool show \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool
```

## <a name="upgrade-node-images-with-node-surge"></a>通过节点冲击升级节点映像

若要加快节点映像升级过程，可以使用可自定义的节点浪涌值升级节点映像。 默认情况下，AKS 使用另一个节点来配置升级。

如果要提高升级速度，请使用 `--max-surge` 值来配置要用于升级的节点数，使其更快完成。 若要详细了解各种设置的权衡 `--max-surge` ，请参阅 [自定义节点浪涌升级][max-surge]。

以下命令设置用于执行节点映像升级的最大冲击值：

```azurecli
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --max-surge 33% \
    --node-image-only \
    --no-wait
```

在升级过程中，请使用下面的 `kubectl` 命令来检查节点映像的状态，以获取标签并筛选出当前节点映像信息：

```azurecli
kubectl get nodes -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.metadata.labels.kubernetes\.azure\.com\/node-image-version}{"\n"}{end}'
```

使用 `az aks nodepool show` 获取更新的节点池详细信息。 当前节点映像在 `nodeImageVersion` 属性中显示。

```azurecli
az aks nodepool show \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool
```

## <a name="next-steps"></a>后续步骤

- 参阅 [AKS 发行说明](https://github.com/Azure/AKS/releases)以了解有关最新节点映像的信息。
- 通过阅读[升级 AKS 群集][upgrade-cluster]一文来了解如何升级 Kubernetes 版本。
- [将安全更新和内核更新应用于 Azure Kubernetes 服务 (AKS) 中的 Linux 节点][security-update]
- 通过阅读[创建和管理多个节点池][use-multiple-node-pools]一文来详细了解多个节点池以及如何升级节点池。

<!-- LINKS - internal -->
[upgrade-cluster]: upgrade-cluster.md
[security-update]: node-updates-kured.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[max-surge]: upgrade-cluster.md#customize-node-surge-upgrade-preview
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
