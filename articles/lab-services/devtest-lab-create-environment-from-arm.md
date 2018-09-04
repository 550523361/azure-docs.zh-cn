---
title: 使用 Azure 资源管理器模板创建多 VM 环境和 PaaS 资源 | Microsoft Docs
description: 了解如何在 Azure 开发测试实验室中通过 Azure 资源管理器模板创建多 VM 环境和 PaaS 资源
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: spelluru
ms.openlocfilehash: 143d0d4b66fc8e6e62364090e3d3187c4aa7bb51
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42919000"
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>使用 Azure 资源管理器模板创建多 VM 环境和 PaaS 资源

使用 [Azure 门户](http://go.microsoft.com/fwlink/p/?LinkID=525040)可以轻松地[一次一个向实验室中添加 VM](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-add-vm)。 但是，如果环境包含多个 VM，则必须分别创建每个 VM。 对于多层 Web 应用或 SharePoint 场等情况，需要使用某种机制以单个步骤创建多个 VM。 现在，可以使用 Azure 资源管理器模板定义 Azure 解决方案的基础结构和配置，以一致的状态重复部署多个 VM。 此功能提供以下优势：

- Azure 资源管理器模板是从源代码管理存储库（GitHub 或 Team Services Git）直接加载的。
- 配置后，用户可以在 Azure 门户中通过选择 Azure 资源管理器模板来创建环境，就像对其他类型的 [VM 库](./devtest-lab-comparing-vm-base-image-types.md)所做的一样。
- 除了 IaaS VM 以外，还可以通过 Azure 资源管理器模板在环境中预配 Azure PaaS 资源。
- 除了其他类型的库创建的单个 VM 以外，还可以在实验室中跟踪环境的成本。
- PaaS 资源会被创建，并在成本跟踪中显示；但 VM 自动关闭不适用于 PaaS 资源。
- 对于环境，用户可以实施他们针对单实验室 VM 所实施的相同的 VM 策略控制。

详细了解[使用 Resource Manager 模板](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager)通过单个操作部署、更新或删除所有实验室资源的众多优点。

> [!NOTE]
> 基于 Resource Manager 模板创建更多实验室 VM 时，无论正创建多 VM 还是单 VM，都需注意某些差异。 [使用虚拟机的 Azure 资源管理器模板](devtest-lab-use-resource-manager-template.md)更加详细地阐述了这些差异。
>

## <a name="devtest-labs-public-environments"></a>开发测试实验室公共环境
Azure 开发测试实验室包含 [Azure 资源管理器模板的公共存储库](https://github.com/Azure/azure-devtestlab/tree/master/Environments)，可以使用此存储库来创建环境，而无需自行连接到外部 GitHub 源。 此存储库包含常用的模板，例如 Azure Web 应用、Service Fabric 群集和 SharePoint 场开发环境。 此功能类似于针对所创建的每个实验室包含的项目的公共存储库。 借助环境存储库，只需提供极少量的输入参数，即可快速开始使用预先编写的环境模板，在实验室中获得 PaaS 资源的顺畅入门体验。 有关详细信息，请参阅[在开发测试实验室中配置和使用公共环境](devtest-lab-configure-use-public-environments.md)。

## <a name="configure-your-own-template-repositories"></a>配置自己的模板存储库
在基础结构即代码和配置即代码方面，最佳做法之一是在源代码管理中管理环境模板。 Azure 开发测试实验室遵循这种做法，它直接从 GitHub 或 VSTS Git 存储库加载所有 Azure 资源管理器模板。 因此，从测试环境到生产环境，Resource Manager 模板可用于整个发布周期。

查看[公共 GitHub 存储库](https://github.com/Azure/azure-devtestlab/tree/master/Environments)中由开发测试实验室团队创建的模板。 在此公共存储库中，可以查看他人共享的模板，可以直接使用或自定义它们来满足你的需求。 创建模板后，将其存储在此存储库中以将其与他人共享。 还可以使用可以用来在云中设置环境的模板设置你自己的 Git 存储库。 

在存储库中整理 Azure 资源管理器模板时需遵循几条规则：

- 必须将主模板文件命名为 `azuredeploy.json`。 

    ![关键的 Azure 资源管理器模板文件](./media/devtest-lab-create-environment-from-arm/master-template.png)

- 如果想要使用参数文件中定义的参数值，必须将参数文件命名为 `azuredeploy.parameters.json`。
- 可以使用参数 `_artifactsLocation` 和 `_artifactsLocationSasToken` 来构造 parametersLink URI 的值，以允许开发测试实验室自动管理嵌套的模板。 有关详细信息，请参阅 [How Azure DevTest Labs makes nested Resource Manager template deployments easier for testing environments](https://blogs.msdn.microsoft.com/devtestlab/2017/05/23/how-azure-devtest-labs-makes-nested-arm-template-deployments-easier-for-testing-environments/)（Azure 开发测试实验室如何使在测试环境中部署嵌套的资源管理器模板部署更轻松）。
- 可以定义元数据来指定模板显示名称和说明。 此元数据必须在名为 `metadata.json` 的文件中。 以下示例元数据文件演示如何指定显示名称和说明： 

    ```json
    { 
        "itemDisplayName": "<your template name>", 
        "description": "<description of the template>" 
    }
    ```

以下步骤指导完成使用 Azure 门户将存储库添加到实验室的整个过程。 

1. 登录到 [Azure 门户](http://go.microsoft.com/fwlink/p/?LinkID=525040)。
1. 选择“所有服务”，并从列表中选择“开发测试实验室”。
1. 从实验室列表，选择所需的实验室。   
1. 在实验室的“概览”窗格中，选择“配置和策略”。

    ![配置和策略](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. 从“配置和策略”设置列表中选择“存储库”。 “存储库”窗格列出已添加到实验室的存储库。 名为 `Public Repo` 的存储库是系统自动为所有实验室生成的，它连接到包含你所用的多个 VM 项目的[开发测试实验室 GitHub 存储库](https://github.com/Azure/azure-devtestlab)。

    ![公共存储库](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. 选择“添加+”来添加 Azure 资源管理器模板存储库。
1. 当第二个“存储库”窗格打开时，请如下所示输入所需的信息：
    - **名称** - 输入实验室中使用的存储库名称。
    - **Git 克隆 URI** - 输入 GitHub 或 Visual Studio Team Services 中的 GIT HTTPS 克隆 URL。  
    - **分支** - 输入用于访问 Azure 资源管理器模板定义的分支名称。 
    - **个人访问令牌** - 个人访问令牌用于安全访问存储库。 要从 Visual Studio Team Services 获取令牌，请选择“&lt;姓名>”>“我的配置文件”>“安全”>“公共访问令牌”。 要从 GitHub 获取令牌，请选择头像，然后选择“设置”>“公共访问令牌”。 
    - **文件夹路径** - 使用两个输入字段中的一个，输入以正斜杠“/”开头的文件夹路径，该路径是项目定义（第一个输入字段）或 Azure 资源管理器模板定义的 Git 克隆 URI 的相对路径。   
    
        ![公共存储库](./media/devtest-lab-create-environment-from-arm/repo-values.png)


1. 输入所有必填字段并通过验证后，请选择“保存”。

下一部分将逐步讲解如何通过 Azure 资源管理器模板创建环境。

## <a name="create-an-environment-from-a-resource-manager-template-using-the-azure-portal"></a>使用 Azure 门户通过资源管理器模板创建环境

在实验室中配置 Azure 资源管理器模板存储库后，实验室用户可在 Azure 门户中使用以下步骤创建环境：

1. 登录到 [Azure 门户](http://go.microsoft.com/fwlink/p/?LinkID=525040)。
1. 选择“所有服务”，并从列表中选择“开发测试实验室”。
1. 从实验室列表，选择所需的实验室。   
1. 在实验室的窗格中，选择“添加+”。
1. “选择库”窗格显示了可与最前面列出的 Azure 资源管理器模板配合使用的基本映像。 选择所需的 Azure 资源管理器模板。

    ![选择一个库](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. 在“添加”窗格中，输入“环境名称”值。 环境名称是向实验室中的用户显示的名称。 剩余的输入字段已在 Azure 资源管理器模板中定义。 如果模板中定义了默认值或者存在 `azuredeploy.parameter.json` 文件，默认值会显示在这些输入字段中。 对于*安全字符串*类型的参数，可以使用 Azure 密钥保管库中存储的机密。 若要了解如何在密钥保管库中保存机密并在创建实验室资源时使用这些机密，请参阅[在 Azure 密钥保管库中存储机密](devtest-lab-store-secrets-in-key-vault.md)。  

    ![添加窗格](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > 有几个参数值会显示为空值，即使已指定这些值。 因此，如果用户将这些值分配到 Azure 资源管理器模板中的参数，则开发测试实验室不会显示这些值。 相反，将会显示空的输入字段，实验室用户在创建环境时必须在这些字段中输入值。
    > 
    > - GEN-UNIQUE
    > - GEN-UNIQUE-[N]
    > - GEN-SSH-PUB-KEY
    > - GEN-PASSWORD 
 
1. 选择“添加”创建环境。 环境随即会开始预配，其状态显示在“我的虚拟机”列表中。 实验室会自动创建一个新资源组，用于预配 Azure 资源管理器模板中定义的所有资源。
1. 创建环境后，请在“我的虚拟机”列表中选择该环境，打开资源组窗格并浏览该环境中预配的所有资源。
    
    ![“我的虚拟机”列表](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   还可以展开环境仅查看该环境中预配的 VM 列表。
    
    ![“我的虚拟机”列表](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. 单击任一环境可以查看可用的操作 - 例如，应用项目、附加数据磁盘、更改自动关闭时间，等等。

    ![环境操作](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="deploy-a-resource-manager-template-to-create-a-vm"></a>部署 Resource Manager 模板以创建 VM
保存 Resource Manager 模板并根据需要对其自定义后，可将该模板用于自动创建 VM。 
- 请参阅[使用 Resource Manager 模板和 Azure PowerShell 部署资源](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)了解如何使用 Azure PowerShell 和 Resource Manager 模板将资源部署到 Azure。 
- 请参阅[使用 Resource Manager 模板和 Azure CLI 部署资源](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli)了解如何使用 Azure CLI 和 Resource Manager 模板将资源部署到 Azure。

> [!NOTE]
> 只有具有实验室所有者权限的用户才能使用 Azure PowerShell 根据 Resource Manager 模板创建 VM。 如果想要使用 Resource Manager 模板自动创建 VM，并且只具有用户权限，则可以使用 [CLI 中的 az lab vm create 命令](https://docs.microsoft.com/cli/azure/lab/vm#az-lab-vm-create)。

### <a name="general-limitations"></a>一般限制 

在开发测试实验室中使用资源管理器模板时，请注意以下限制：

- 你创建的任何资源管理器模板都不能引用现有资源；它只能引用新资源。 此外，如果你具有通常在开发测试实验室外部部署的现有资源管理器模板并且它包含对现有资源的引用，则不能在实验室中使用它。

   唯一的例外是，**可以**引用现有的虚拟网络。 

- 无法从通过资源管理器模板创建的实验室 VM 创建公式。 

- 无法从通过资源管理器模板创建的实验室 VM 创建自定义图像。 

- 在部署资源管理器模板时，大多数策略不进行评估。

   例如，你可能具有指定了用户只能创建五个 VM 的实验室策略。 但是，用户可以部署创建数十个 VM 的资源管理器模板。 不评估的策略包括：

   - 每个用户的 VM 数
   - 每个实验室用户的高级 VM 数
   - 每个实验室用户的高级磁盘数


### <a name="configure-environment-resource-group-access-rights-for-lab-users"></a>为实验室用户配置环境资源组访问权限

实验室用户可以部署资源管理器模板。 但默认情况下，它们具有“读者”访问权限，这意味着他们不能更改环境资源组中的资源。 例如，他们无法停止或启动其资源。

请按照下列步骤向实验室用户授予“参与者”访问权限。 然后，当他们部署资源管理器模板时，他们将能够编辑该环境中的资源。 


1. 在实验室的“概览”窗格中，选择“配置和策略”。
1. 选择“实验室设置”。
1. 在“实验室设置”窗格中，选择“参与者”以向实验室用户授予写入权限。

    ![可配置的实验室用户访问权限](./media/devtest-lab-create-environment-from-arm/configure-access-rights.png)

1. 选择“保存”。

## <a name="next-steps"></a>后续步骤
* 创建 VM 后，可以通过在 VM 的管理窗格中选择“连接”来连接到该 VM。
* 在实验室的“我的虚拟机”列表中选择环境，查看和管理该环境中的资源。 
* 浏览 [Azure 快速入门模板库中的 Azure 资源管理器模板](https://github.com/Azure/azure-quickstart-templates)。
