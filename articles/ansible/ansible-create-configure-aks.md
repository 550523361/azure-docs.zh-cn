---
title: 在 Azure 中使用 Ansible 创建和配置 Azure Kubernetes 服务群集
description: 了解如何在 Azure 中使用 Ansible 创建和管理 Azure Kubernetes 服务群集
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, aks, 容器, Kubernetes
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/21/2018
ms.openlocfilehash: de692b29902145e44a055680d662c16ed90c56c2
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "42617169"
---
# <a name="create-and-configure-azure-kubernetes-service-clusters-in-azure-using-ansible"></a>在 Azure 中使用 Ansible 创建和配置 Azure Kubernetes 服务群集
使用 Ansible 可以在环境中自动部署和配置资源。 可以使用 Ansible 管理 Azure Kubernetes 服务 (AKS)。 本文介绍如何使用 Ansible 创建和配置 Azure Kubernetes 服务群集。

## <a name="prerequisites"></a>先决条件
- **Azure 订阅** - 如果没有 Azure 订阅，请在开始前创建一个[免费帐户](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。
- **Azure 服务主体** - [创建服务主体](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal)时，请记下以下值：**appId**、**displayName**、**密码**和**租户**。

- **配置 Azure Cloud Shell** 或**在 Linux 虚拟机上安装和配置 Ansible**

  **配置 Azure Cloud Shell**

  1. **配置 Azure Cloud Shell** - 如果不熟悉 Azure Cloud Shell，[Bash in Azure Cloud Shell 快速入门](/azure/cloud-shell/quickstart)一文演示了如何启动和配置 Cloud Shell。 

  **--或者--**

  **在 Linux 虚拟机上安装和配置 Ansible**

  1. **安装 Ansible** - 在[支持的 Linux 平台](/azure/virtual-machines/linux/ansible-install-configure#install-ansible-on-an-azure-linux-virtual-machine)上安装 Ansible。

  1. **配置 Ansible** - [创建 Azure 凭据并配置 Ansible](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

> [!Note]
> 在本教程中运行以下示例 playbook 需要 Ansible 2.6。 

## <a name="create-a-managed-aks-cluster"></a>创建托管的 AKS 群集
以下示例 Ansible playbook 将创建一个资源组，以及一个驻留在资源组中的 AKS 群集：

  ```yaml
  - name: Create Azure Kubernetes Service
    hosts: localhost
    connection: local
    vars:
      resource_group: myResourceGroup
      location: eastus
      aks_name: myAKSCluster
      username: azureuser
      ssh_key: "your_ssh_key"
      client_id: "your_client_id"
      client_secret: "your_client_secret"
    tasks:
    - name: Create resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
    - name: Create a managed Azure Container Services (AKS) cluster
      azure_rm_aks:
        name: "{{ aks_name }}"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        dns_prefix: "{{ aks_name }}"
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 2
            vm_size: Standard_D2_v2
        tags:
          Environment: Production
  ```

以下项目符号有助于说明前面的 Ansible playbook 代码：
- **tasks** 中的第一部分在 **eastus** 位置定义了名为 **myResourceGroup** 的资源组。 
- **tasks** 中的第二部分在 **myResourceGroup** 资源组中定义了名为 **myAKSCluster** 的 AKS 群集。 

若要使用 Ansible 创建 AKS 群集，请将前面的示例 playbook 保存为 `azure_create_aks.yml`，然后使用以下命令运行该 playbook：

  ```bash
  ansible-playbook azure_create_aks.yml
  ```

**ansible-playbook* 命令的输出类似于以下内容，表明已成功创建 AKS 群集：

  ```bash
  PLAY [Create AKS] ****************************************************************************************

  TASK [Gathering Facts] ********************************************************************************************
  ok: [localhost]

  TASK [Create resource group] **************************************************************************************
  changed: [localhost]

  TASK [Create a Azure Container Services (AKS) cluster] ***************************************************
  changed: [localhost]

  PLAY RECAP *********************************************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0
  ```

## <a name="scale-aks-nodes"></a>缩放 AKS 节点

前一部分中的示例 playbook 定义了两个节点。 如果群集上需要更少或更多容器工作负荷，则可以轻松调整节点数。 本部分中的示例 playbook 将节点数从两个节点增加到三个节点。 通过更改 **agent_pool_profiles** 块中的 **count** 值来修改节点计数。 

在 **service_principal** 块中输入自己的 `ssh_key`、`client_id` 和 `client_secret`：

```yaml
- name: Scale AKS cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Scaling an existed AKS cluster
    azure_rm_aks:
        name: "{{ aks_name }}"    
        location: "{{ location }}"
        resource_group: "{{ resource_group }}" 
        dns_prefix: "{{ aks_name }}" 
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 3
            vm_size: Standard_D2_v2
```

若要使用 Ansible 缩放 Azure Kubernetes 服务群集，请将前面的 playbook 保存为 *azure_configure_aks.yml*，然后运行 playbook，如下所示：

  ```bash
  ansible-playbook azure_configure_aks.yml
  ```

以下输出显示已成功创建 AKS 群集：

  ```bash
  PLAY [Scale AKS cluster] ***************************************************************

  TASK [Gathering Facts] ******************************************************************
  ok: [localhost]

  TASK [Scaling an existed AKS cluster] **************************************************
  changed: [localhost]

  PLAY RECAP ******************************************************************************
  localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```
## <a name="delete-a-managed-aks-cluster"></a>删除托管的 AKS 群集

以下示例 Ansible playbook 部分演示了如何删除 AKS 群集：

  ```yaml
  - name: Delete a managed Azure Container Services (AKS) cluster
    hosts: localhost
    connection: local
    vars:
      resource_group: myResourceGroup
      aks_name: myAKSCluster
    tasks:
    - name: 
      azure_rm_aks:
        name: "{{ aks_name }}"
        resource_group: "{{ resource_group }}"
        state: absent
   ```

若要使用 Ansible 删除 Azure Kubernetes 服务群集，请将前面的 playbook 保存为 *azure_delete_aks.yml*，然后运行 playbook，如下所示：

  ```bash
  ansible-playbook azure_delete_aks.yml
  ```

以下输出显示已成功删除 AKS 群集：
  ```bash
PLAY [Delete a managed Azure Container Services (AKS) cluster] ****************************

TASK [Gathering Facts] ********************************************************************
ok: [localhost]

TASK [azure_rm_aks] *********************************************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```
  
## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"] 
> [教程：在 Azure Kubernetes 服务 (AKS) 中缩放应用程序](https://docs.microsoft.com/azure/aks/tutorial-kubernetes-scale)
