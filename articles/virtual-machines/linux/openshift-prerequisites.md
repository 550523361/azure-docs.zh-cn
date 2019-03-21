---
title: Azure 中的 OpenShift 的先决条件 | Microsoft Docs
description: 在 Azure 中部署 OpenShift 的先决条件。
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/02/2019
ms.author: haroldw
ms.openlocfilehash: f4fd33b250bf1f79610f4363e85b97be87892d78
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57449954"
---
# <a name="common-prerequisites-for-deploying-openshift-in-azure"></a>在 Azure 中部署 OpenShift 所要满足的一般先决条件

本文介绍了在 Azure 中部署 OpenShift 容器平台或 OKD 所要满足的常见先决条件。

OpenShift 的安装使用 Ansible 攻略。 Ansible 使用安全外壳 (SSH) 连接到所有群集主机来完成安装步骤。

当 ansible 发起到远程主机的 SSH 连接时，无法输入密码。 因此，私钥不能有与之关联的密码（通行短语），否则，部署将失败。

因为虚拟机 (VM) 是通过 Azure 资源管理器模板部署的，所以将使用同一公钥来访问所有 VM。 还需要将对应的私钥注入到执行所有攻略的 VM 中。 为安全地执行此操作，我们使用 Azure Key Vault 向 VM 传递私钥。

如果容器需要持久性存储，则需要永久卷。 OpenShift 支持使用 Azure 虚拟硬盘 (VHD) 实现此功能，但必须首先将 Azure 配置为云提供程序。

在此模型中，OpenShift 将会：

- 在 Azure 存储帐户或托管磁盘中创建一个 VHD 对象。
- 将 VHD 装载到 VM 中并格式化卷。
- 将卷装载到 Pod。

要使此配置可行，OpenShift 需要有权在 Azure 中执行这些任务。 若要实现此目的，需要一个服务主体。 服务主体是 Azure Active Directory 中的一个安全帐户，被授予了对资源的权限。

服务主体需要有权访问构成了群集的存储帐户和 VM。 如果所有 OpenShift 群集资源都部署到单个资源组中，则可以向服务主体授予对该资源组的权限。

本指南介绍了如何创建与先决条件关联的项目。

> [!div class="checklist"]
> * 创建一个 Key Vault 用于管理 OpenShift 群集的 SSH 密钥。
> * 创建一个供 Azure 云提供程序使用的服务主体。

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="sign-in-to-azure"></a>登录 Azure 
使用 [az login](/cli/azure/reference-index) 命令登录到 Azure 订阅，按屏幕说明操作，或者单击“试用”使用 Cloud Shell。

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>创建资源组

使用 [az group create](/cli/azure/group) 命令创建资源组。 Azure 资源组是在其中部署和管理 Azure 资源的逻辑容器。 建议使用专用资源组来托管密钥保管库。 此组与要将 OpenShift 群集资源部署到的资源组分开。

以下示例在 *eastus* 位置创建一个名为 *keyvaultrg* 的资源组：

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>创建 key vault
使用 [az keyvault create](/cli/azure/keyvault) 命令创建一个 Key Vault 用于管理群集的 SSH 密钥。 Key Vault 名称必须全局唯一。

以下示例在 *keyvaultrg* 资源组中创建一个名为 *keyvault* 的 Key Vault：

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>创建 SSH 密钥 
需要使用 SSH 密钥来保护对 OpenShift 群集的访问。 使用 `ssh-keygen` 命令创建 SSH 密钥对（在 Linux 或 macOS 上）：
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> SSH 密钥对不能包含密码/通行短语。

有关 Windows 上的 SSH 密钥的详细信息，请参阅[如何在 Windows 上创建 SSH 密钥](/azure/virtual-machines/linux/ssh-from-windows)。 请务必以 OpenSSH 格式导出私钥。

## <a name="store-the-ssh-private-key-in-azure-key-vault"></a>将 SSH 私钥存储在 Azure Key Vault 中
OpenShift 部署使用创建的 SSH 密钥来保护对 OpenShift 主设备的访问。 为使部署能够安全检索 SSH 密钥，请使用以下命令将密钥存储在 Key Vault 中：

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift_rsa
```

## <a name="create-a-service-principal"></a>创建服务主体 
OpenShift 使用用户名和密码或服务主体来与 Azure 通信。 Azure 服务主体是可用于应用、服务和 OpenShift 等自动化工具的安全标识。 控制和定义服务主体可在 Azure 中执行哪些操作的权限。 最好将服务主体的权限范围限定为特定资源组而不是整个订阅。

使用 [az ad sp create-for-rbac](/cli/azure/ad/sp) 创建服务主体并输出 OpenShift 需要的凭据。

以下示例创建一个服务主体并为其分配对名为 openshiftrg 的资源组的参与者权限。

首先，创建名为 openshiftrg 的资源组：

```azurecli
az group create -l eastus -n openshiftrg
```

创建服务主体：

```azurecli
scope=`az group show --name openshiftrg --query id`
az ad sp create-for-rbac --name openshiftsp \
      --role Contributor --password {Strong Password} \
      --scopes $scope
```
如果使用的是 Windows，则执行 ```az group show --name openshiftrg --query id``` 并使用输出代替 $scope。

记下该命令返回的 appId 属性：
```json
{
  "appId": "11111111-abcd-1234-efgh-111111111111",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {Strong Password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > 请务必创建安全密码。 遵循 [Azure AD 密码规则和限制](/azure/active-directory/active-directory-passwords-policy)指导原则。

有关服务主体的详细信息，请参阅[使用 Azure CLI 创建 Azure 服务主体](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)。

## <a name="next-steps"></a>后续步骤

本文涵盖了以下主题：
> [!div class="checklist"]
> * 创建一个 Key Vault 用于管理 OpenShift 群集的 SSH 密钥。
> * 创建一个供 Azure 云解决方案提供程序使用的服务主体。

接下来，可部署 OpenShift 群集：

- [部署 OpenShift Container Platform](./openshift-container-platform.md)
- [部署 OKD](./openshift-okd.md)
