---
title: 在 Azure Kubernetes 服务 (AKS) 中使用门户创建虚拟节点
description: 了解如何通过 Azure 门户创建使用虚拟节点运行 Pod 的 Azure Kubernetes 服务 (AKS) 群集。
services: container-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 5f7bf75598c09c5c8c0654f7db863068f9e7be7d
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82128866"
---
# <a name="create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-in-the-azure-portal"></a>创建 Azure Kubernetes 服务 (AKS) 群集并将其配置为使用 Azure 门户中的虚拟节点

若要在 Azure Kubernetes 服务 (AKS) 群集中快速部署工作负荷，可以使用虚拟节点。 使用虚拟节点可快速预配 Pod，并且只需对其执行时间按秒付费。 在缩放方案中，无需等待 Kubernetes 群集自动缩放程序部署 VM 计算节点来运行其他 Pod。 只有 Linux pod 和节点支持虚拟节点。

本文介绍如何创建和配置虚拟网络资源以及启用了虚拟节点的 AKS 群集。

## <a name="before-you-begin"></a>在开始之前

虚拟节点在 Azure 容器实例（ACI）和 AKS 群集中运行的 pod 之间启用网络通信。 若要提供此通信，应创建虚拟网络子网并分配委派的权限。 虚拟节点仅适用于使用高级** 网络创建的 AKS 群集。 默认情况下，AKS 群集是使用基本** 网络创建的。 本文介绍如何创建虚拟网络和子网，然后部署使用高级网络的 AKS 群集。

如果以前没有使用过 ACI，请在订阅中注册服务提供程序。 可以使用 [az provider list][az-provider-list] 命令检查 ACI 提供程序注册的状态，如下面的示例所示：

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

Microsoft.ContainerInstance** 提供程序应报告为“已注册”**，如下面的示例输出所示：

```output
Namespace                    RegistrationState    RegistrationPolicy
---------------------------  -------------------  --------------------
Microsoft.ContainerInstance  Registered           RegistrationRequired
```

如果提供程序显示为“未注册”**，请使用 [az provider register][az-provider-register] 注册提供程序，如下面的示例所示：

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

## <a name="regional-availability"></a>区域可用性

虚拟节点部署支持以下区域：

* 澳大利亚东部（australiaeast）
* 美国中部（centralus）
* 美国东部 (eastus)
* 美国东部2（eastus2）
* 日本东部（japaneast）
* 北欧 (northeurope)
* 东南亚（southeastasia）
* 美国中部（westcentralus）
* 西欧 (westeurope)
* 美国西部 (westus)
* 美国西部 2 (westus2)

## <a name="known-limitations"></a>已知的限制
虚拟节点功能很大程度上依赖于 ACI 的功能集。 虚拟节点尚不支持以下方案

* 使用服务主体拉取 ACR 映像。 [解决方法](https://github.com/virtual-kubelet/azure-aci/blob/master/README.md#private-registry)是使用[Kubernetes 机密](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-by-providing-credentials-on-the-command-line)
* [虚拟网络限制](../container-instances/container-instances-vnet.md)，包括 VNet 对等互连、Kubernetes 网络策略和网络安全组发送到 internet 的出站流量。
* 初始化容器
* [主机别名](https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/)
* ACI 中 exec 的[参数](../container-instances/container-instances-exec.md#restrictions)
* [Daemonset](concepts-clusters-workloads.md#statefulsets-and-daemonsets)不会将 pod 部署到虚拟节点
* 虚拟节点支持 Linux pod 计划。 可以手动安装开源[Virtual KUBELET ACI](https://github.com/virtual-kubelet/azure-aci)提供程序，以便将 Windows Server 容器计划到 ACI。 

## <a name="sign-in-to-azure"></a>登录 Azure

通过 https://portal.azure.com 登录到 Azure 门户。

## <a name="create-an-aks-cluster"></a>创建 AKS 群集

在 Azure 门户的左上角，选择 "**创建资源** > **Kubernetes 服务**"。

在“基本信息”页面上，配置以下选项  ：

- *项目详细信息*：选择一个 azure 订阅，然后选择或创建一个 azure 资源组，如*myResourceGroup*。 输入 **Kubernetes 群集名称**，例如 *myAKSCluster*。
- *群集详细信息*：选择 AKS 群集的区域、Kubernetes 版本和 DNS 名称前缀。
- *主节点池*：为 AKS 节点选择 VM 大小。 一旦部署 AKS 群集，不能更改 VM 大小****。
     - 选择要部署到群集中的节点数。 在本文中，将“节点计数”设置为 *1*****。 部署群集后，可以调整节点计数  。

单击 "**下一步：缩放**"。

在 "**缩放**" 页上，选择 "**虚拟节点**" 下的 "*启用*"。

![创建 AKS 群集并启用虚拟节点](media/virtual-nodes-portal/enable-virtual-nodes.png)

默认情况下，将创建一个 Azure Active Directory 服务主体。 此服务主体用于群集通信以及与其他 Azure 服务集成。 或者，你可以将托管标识用于权限，而不是服务主体。 有关详细信息，请参阅[使用托管标识](use-managed-identity.md)。

群集还配置有高级网络。 虚拟节点配置为使用自己的 Azure 虚拟网络子网。 此子网具有委托的权限，可连接 AKS 群集之间的 Azure 资源。 如果还没有委托的子网，Azure 门户将创建并配置 Azure 虚拟网络和子网，以便与虚拟节点配合使用。

选择“查看 + 创建”  。 完成验证后，选择“创建”****。

创建 AKS 群集并让其可供使用需要几分钟的时间。

## <a name="connect-to-the-cluster"></a>连接到群集

Azure Cloud Shell 是免费的交互式 shell，可以使用它运行本文中的步骤。 它预安装有常用 Azure 工具并将其配置与帐户一起使用。 若要管理 Kubernetes 群集，请使用 Kubernetes 命令行客户端 [kubectl][kubectl]。 `kubectl` 客户端已预装在 Azure Cloud Shell 中。

若要打开 Cloud Shell，请从代码块的右上角选择“试一试”****。 也可以通过转到 [https://shell.azure.com/bash](https://shell.azure.com/bash) 在单独的浏览器标签页中启动 Cloud Shell。 选择“复制”以复制代码块，将其粘贴到 Cloud Shell 中，然后按 Enter 来运行它。 

使用 [az aks get-credentials][az-aks-get-credentials] 命令将 `kubectl` 配置为连接到 Kubernetes 群集。 以下示例获取名为 *myResourceGroup* 的资源组中群集名称 *myAKSCluster* 的凭据：

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

若要验证到群集的连接，请使用 [kubectl get][kubectl-get] 命令返回群集节点列表。

```console
kubectl get nodes
```

以下示例输出依次显示了所创建的单个 VM 节点以及用于 Linux 的虚拟节点 virtual-node-aci-linux**：

```output
NAME                           STATUS    ROLES     AGE       VERSION
virtual-node-aci-linux         Ready     agent     28m       v1.11.2
aks-agentpool-14693408-0       Ready     agent     32m       v1.11.2
```

## <a name="deploy-a-sample-app"></a>部署示例应用

在 Azure Cloud Shell 中，创建一个名为 `virtual-node.yaml` 的文件并复制到以下 YAML 中。 若要在节点上计划容器，需定义 [nodeSelector][node-selector] 和 [toleration][toleration]。 这些设置允许在虚拟节点上计划 Pod，并确认已成功启用该功能。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/role: agent
        beta.kubernetes.io/os: linux
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Exists
      - key: azure.com/aci
        effect: NoSchedule
```

使用 [kubectl apply][kubectl-apply] 命令运行该应用程序。

```azurecli-interactive
kubectl apply -f virtual-node.yaml
```

使用带有  参数的 [kubectl get pods`-o wide`][kubectl-get] 命令输出 Pod 和计划节点的列表。 请注意，已在 `virtual-node-linux` 节点上计划 `virtual-node-helloworld` pod。

```console
kubectl get pods -o wide
```

```output
NAME                                     READY     STATUS    RESTARTS   AGE       IP           NODE
virtual-node-helloworld-9b55975f-bnmfl   1/1       Running   0          4m        10.241.0.4   virtual-node-aci-linux
```

系统从被委派用于虚拟节点的 Azure 虚拟网络子网中为该 Pod 分配了一个内部 IP 地址。

> [!NOTE]
> 如果使用存储在 Azure 容器注册表中的映像，请[配置并使用 Kubernetes 机密][acr-aks-secrets]。 虚拟节点当前的限制是不能使用集成 Azure AD 服务主体身份验证。 如果不使用机密，则在虚拟节点上计划的 Pod 将无法启动并报告错误 `HTTP response status code 400 error code "InaccessibleImage"`。

## <a name="test-the-virtual-node-pod"></a>测试虚拟节点 Pod

若要测试虚拟节点上运行的 Pod，请使用 Web 客户端浏览到演示应用程序。 由于为该 Pod 分配了一个内部 IP 地址，因此，可以从 AKS 群集上的另一个 Pod 快速测试此连接。 创建一个测试 Pod，并在其上附加一个终端会话：

```console
kubectl run -it --rm virtual-node-test --image=debian
```

使用 `apt-get` 在 Pod 中安装 `curl`：

```console
apt-get update && apt-get install -y curl
```

现在，使用`curl`访问 pod 的地址，例如*http://10.241.0.4*。 提供上一个 `kubectl get pods` 命令中所示的你自己的内部 IP 地址：

```console
curl -L http://10.241.0.4
```

将显示演示应用程序，如以下精简版示例输出中所示：

```output
<html>
<head>
  <title>Welcome to Azure Container Instances!</title>
</head>
[...]
```

使用 `exit` 关闭与测试 Pod 之间的终端会话。 会话结束时便会删除该 Pod。

## <a name="next-steps"></a>后续步骤

在本文中，在虚拟节点上计划了一个 Pod，并为其分配了一个专用的内部 IP 地址。 也可以改为创建服务部署，并通过负载均衡器或入口控制器将流量路由到 Pod。 有关详细信息，请参阅[在 AKS 中创建基本入口控制器][aks-basic-ingress]。

虚拟节点是 AKS 中缩放解决方案的一个组件。 有关缩放解决方案的详细信息，请参阅以下文章：

- [使用 Kubernetes 水平 Pod 自动缩放程序][aks-hpa]
- [使用 Kubernetes 群集自动缩放程序][aks-cluster-autoscaler]
- [查看虚拟节点的自动缩放示例][virtual-node-autoscale]
- [阅读有关虚拟 Kubelet 开放源代码库的详细信息][virtual-kubelet-repo]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[aks-github]: https://github.com/azure/aks/issues]
[virtual-node-autoscale]: https://github.com/Azure-Samples/virtual-node-autoscale
[virtual-kubelet-repo]: https://github.com/virtual-kubelet/virtual-kubelet
[acr-aks-secrets]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- LINKS - internal -->
[aks-network]: ./networking-overview.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: cluster-autoscaler.md
[aks-basic-ingress]: ingress-basic.md
[az-provider-list]: /cli/azure/provider#az-provider-list
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register