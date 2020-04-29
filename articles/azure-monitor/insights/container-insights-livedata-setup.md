---
title: 设置容器实时数据 Azure Monitor （预览） |Microsoft Docs
description: 本文介绍如何设置容器日志（stdout/stderr）和事件的实时视图，而无需将 kubectl 与容器的 Azure Monitor 一起使用。
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: f19071ca642cd229cbd7d49b4eab90c970672eee
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "79275368"
---
# <a name="how-to-set-up-the-live-data-preview-feature"></a>如何设置实时数据（预览）功能

若要使用 Azure Kubernetes Service （AKS）群集中的容器 Azure Monitor 查看实时数据（预览版），需要配置身份验证，授予访问 Kubernetes 数据的权限。 此安全配置允许通过 Kubernetes API 在 Azure 门户中直接访问数据。

此功能支持以下方法来控制对日志、事件和指标的访问：

- 没有启用 Kubernetes RBAC 授权的 AKS
- 启用了 Kubernetes RBAC 授权的 AKS
    - AKS 配置了群集角色绑定** [clusterMonitoringUser](https://docs.microsoft.com/rest/api/aks/managedclusters/listclustermonitoringusercredentials?view=azurermps-5.2.0)**
- 启用了基于 SAML 的 Azure Active Directory (AD) 单一登录的 AKS

这些说明需要对 Kubernetes 群集的管理访问权限，并且如果将配置为使用 Azure Active Directory （AD）进行用户身份验证，则可以通过管理访问 Azure AD。  

本文介绍如何配置身份验证，以控制对群集中的实时数据（预览）功能的访问权限：

- 基于角色的访问控制（RBAC）已启用 AKS 群集
- Azure Active Directory 集成 AKS 群集。 

>[!NOTE]
>此功能不支持将 AKS 群集作为[专用群集](https://azure.microsoft.com/updates/aks-private-cluster/)启用。 此功能依赖于通过代理服务器直接从浏览器访问 Kubernetes API。 启用网络安全以阻止此代理中的 Kubernetes API 会阻止此流量。 

>[!NOTE]
>此功能在所有 Azure 区域均可用，包括Azure 中国。 它目前在 Azure 美国政府版中不可用。

## <a name="authentication-model"></a>身份验证模型

实时数据（预览）功能使用 Kubernetes API，与`kubectl`命令行工具相同。 Kubernetes API 终结点使用自签名证书，你的浏览器将无法对其进行验证。 此功能利用内部代理使用 AKS 服务验证证书，确保流量受信任。

Azure 门户提示你验证 Azure Active Directory 群集的登录凭据，并在群集创建过程中将你重定向到客户端注册设置（在本文中重新配置）。 此行为类似于所需的身份验证过程`kubectl`。 

>[!NOTE]
>对你的群集的授权由 Kubernetes 以及它所配置的安全模型进行管理。 访问此功能的用户需要下载 Kubernetes 配置（*kubeconfig*）的权限，这与运行`az aks get-credentials -n {your cluster name} -g {your resource group}`类似。 此配置文件包含**Azure Kubernetes Service 群集用户角色**的授权和身份验证令牌，适用于未启用 RBAC 授权的启用了 azure rbac 的和 AKS 群集的情况。 它包含有关启用 AKS 时的 Azure AD 和客户端注册详细信息，以及基于 Azure Active Directory （AD）基于 SAML 的单一登录的信息。

>[!IMPORTANT]
>此功能的用户需要[Azure Kubernetes 群集用户角色](../../azure/role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-user-role permissions)才能下载`kubeconfig`并使用此功能。 用户**不**需要对群集的参与者访问权限即可利用此功能。 

## <a name="using-clustermonitoringuser-with-rbac-enabled-clusters"></a>在启用 RBAC 的群集中使用 clusterMonitoringUser

若要在[启用 RBAC](#configure-kubernetes-rbac-authorization)授权后无需应用额外的配置更改，以允许 Kubernetes 用户角色绑定**ClusterUser**访问实时数据（预览版）功能，AKS 添加了名为**clusterMonitoringUser**的新 Kubernetes 群集角色绑定。 此群集角色绑定具有现成的所有必需权限，可用于访问 Kubernetes API 和用于利用实时数据（预览）功能的终结点。

若要利用此新用户使用实时数据（预览版）功能，你需要是 AKS 群集资源上 "[参与者](../../role-based-access-control/built-in-roles.md#contributor)" 角色的成员。 默认情况下，为容器 Azure Monitor 配置为使用此用户进行身份验证。 如果 clusterMonitoringUser 角色绑定不存在于群集上，则使用**clusterUser**进行身份验证。

AKS 在2020年1月发布此新的角色绑定，因此在2020年1月之前创建的群集不具备此项。 如果你有一个在2020年1月之前创建的群集，则可以通过在群集上执行 PUT 操作将新的**clusterMonitoringUser**添加到现有群集，或在群集上执行任何其他操作，以便在群集上执行 put 操作，例如更新群集版本。

## <a name="kubernetes-cluster-without-rbac-enabled"></a>未启用 RBAC 的 Kubernetes 群集

如果 Kubernetes 群集未配置 Kubernetes RBAC 授权或集成 Azure AD 单一登录，则不需执行这些步骤。 这是因为，默认情况下，在非 RBAC 配置中拥有管理权限。

## <a name="configure-kubernetes-rbac-authorization"></a>配置 Kubernetes RBAC 授权

启用 Kubernetes RBAC 授权时，将使用两个用户： **clusterUser**和**CLUSTERADMIN**访问 Kubernetes API。 这类似于不带`az aks get-credentials -n {cluster_name} -g {rg_name}`管理选项运行。 这意味着需要向**clusterUser**授予对 Kubernetes API 中终结点的访问权限。

以下示例步骤演示如何从此 yaml 配置模板配置群集角色绑定。

1. 复制并粘贴 yaml 文件，然后将其另存为 LogReaderRBAC.yaml。  

    ```
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRole 
    metadata: 
       name: containerHealth-log-reader 
    rules: 
        - apiGroups: ["", "metrics.k8s.io", "extensions", "apps"] 
          resources: 
             - "pods/log" 
             - "events" 
             - "nodes" 
             - "pods" 
             - "deployments" 
             - "replicasets" 
          verbs: ["get", "list"] 
    --- 
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRoleBinding 
    metadata: 
       name: containerHealth-read-logs-global 
    roleRef: 
       kind: ClusterRole 
       name: containerHealth-log-reader 
       apiGroup: rbac.authorization.k8s.io 
    subjects: 
    - kind: User 
      name: clusterUser 
      apiGroup: rbac.authorization.k8s.io 
    ```

2. 若要更新配置，请运行以下命令： `kubectl apply -f LogReaderRBAC.yaml`。

>[!NOTE] 
> 如果已将以前版本的`LogReaderRBAC.yaml`文件应用到群集，请通过复制并粘贴上面步骤1中所示的新代码进行更新，然后运行步骤2中显示的命令，将其应用到群集。

## <a name="configure-ad-integrated-authentication"></a>配置 AD 集成身份验证 

配置为使用 Azure Active Directory （AD）进行用户身份验证的 AKS 群集利用访问此功能的人员的登录凭据。 在此配置中，你可以使用自己的 Azure AD 身份验证令牌登录到 AKS 群集。

Azure AD 客户端注册必须重新配置为允许 Azure 门户将授权页面重定向为受信任的重定向 URL。 然后，将 Azure AD 中的用户通过**ClusterRoles**和**ClusterRoleBindings**向相同的 Kubernetes API 终结点授予访问权限。 

有关 Kubernetes 中的高级安全设置的详细信息，请参阅[Kubernetes 文档](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)。 

>[!NOTE]
>如果要创建新的启用 RBAC 的群集，请参阅将[Azure Active Directory 与 Azure Kubernetes 服务集成](../../aks/azure-ad-integration.md)，然后按照步骤配置 Azure AD 身份验证。 在创建客户端应用程序的步骤中，此部分中的说明将突出显示需要为与下面的步骤3中指定的容器 Azure Monitor 匹配的两个重定向 Url。

### <a name="client-registration-reconfiguration"></a>重新配置客户端注册

1. 在 Azure 门户中**Azure Active Directory > 应用注册**下的 Azure AD 中找到 Kubernetes 群集的客户端注册。

2. 从左侧窗格中选择 "**身份验证**"。 

3. 将两个重定向 Url 作为**Web**应用程序类型添加到此列表。 第一个基 URL 值应为 `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`，第二个基 URL 值应为 `https://monitoring.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`。

    >[!NOTE]
    >如果在 Azure 中国区使用此功能，则第一个 "基本 URL" 值`https://afd.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`应为，第二个 "基`https://monitoring.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`url" 值应为。 
    
4. 注册重定向 Url 之后，在 "**隐式授权**" 下选择 "**访问令牌**" 和 " **ID 令牌**" 选项，并保存所做的更改。

>[!NOTE]
>通过 Azure Active Directory 配置身份验证以便实现单一登录的操作只能在初次部署新 AKS 群集过程中完成。 不能为已部署的 AKS 群集配置单一登录。
  
>[!IMPORTANT]
>如果使用更新的 URI 重新配置了用于用户身份验证的 Azure AD，请清除浏览器的缓存，确保更新的身份验证令牌已下载并应用。

## <a name="grant-permission"></a>授予权限

必须向每个 Azure AD 帐户授予 Kubernetes 中相应 Api 的权限，以便访问实时数据（预览版）功能。 授予 Azure Active Directory 帐户的步骤类似于[KUBERNETES RBAC authentication](#configure-kubernetes-rbac-authorization)部分中所述的步骤。 将 yaml 配置模板应用到群集之前，请将**ClusterRoleBinding**下的**clusterUser**替换为所需的用户。 

>[!IMPORTANT]
>如果为其授予 RBAC 绑定的用户在同一个 Azure AD 租户中，请根据 userPrincipalName 分配权限。 如果用户位于不同的 Azure AD 租户中，则查询并使用 objectId 属性。

有关配置 AKS 群集**ClusterRoleBinding**的其他帮助，请参阅[创建 RBAC 绑定](../../aks/azure-ad-integration-cli.md#create-rbac-binding)。

## <a name="next-steps"></a>后续步骤

设置身份验证后，你可以从群集实时查看[指标](container-insights-livedata-metrics.md)、[部署](container-insights-livedata-deployments.md)和[事件和日志](container-insights-livedata-overview.md)。
