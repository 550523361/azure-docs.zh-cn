---
title: 在 Azure 自动化中管理 Python 2 包
description: 本文介绍如何在 Azure 自动化中管理 Python 2 包。
services: automation
ms.subservice: process-automation
ms.date: 02/25/2019
ms.topic: conceptual
ms.openlocfilehash: 9f52dfd92d430abffe5857d231898dd4b0e7745e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "81679921"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>在 Azure 自动化中管理 Python 2 包

通过 Azure 自动化，可以在 Azure 和 Linux 混合 Runbook 辅助角色上运行 Python 2 runbook。 为了帮助简化 runbook，可以使用 Python 包导入所需的模块。 本文介绍如何在 Azure 自动化中管理和使用 Python 包。

## <a name="import-packages"></a>导入包

在自动化帐户中，选择 "**共享资源**" 下的 " **Python 2 包**"。 单击“+ 添加 Python 2 包”  。

![添加 Python 包](media/python-packages/add-python-package.png)

在“添加 Python 包”页中，选择要上传的本地包。 包可以是**whl**或**gz**文件。 选择包后，单击 **"确定"** 将其上传。

![添加 Python 包](media/python-packages/upload-package.png)

导入包后，它将列在自动化帐户的 "Python 2 包" 页面上。 如果需要删除包，请选择该包，然后单击 "**删除**"。

![包列表](media/python-packages/package-list.png)

## <a name="import-packages-with-dependencies"></a>导入带依赖项的包

Azure 自动化不在导入过程中解析 Python 包的依赖项。 可以通过两种方式导入包及其所有依赖项。 只需使用以下步骤之一将包导入到自动化帐户中。

### <a name="manually-download"></a>手动下载

在安装了 [python2.7](https://www.python.org/downloads/release/latest/python2) 和 [pip](https://pip.pypa.io/en/stable/) 的 Windows 64 位计算机上运行以下命令，以便下载包及其所有依赖项：

```cmd
C:\Python27\Scripts\pip2.7.exe download -d <output dir> <package name>
```

等到这些包下载以后，即可将其导入自动化帐户中。

### <a name="runbook"></a>Runbook

导入 python runbook 将[python 2 包从 pypi](https://gallery.technet.microsoft.com/scriptcenter/Import-Python-2-packages-57f7d509)导入到自动化帐户中，将其从库导入到 Azure 自动化帐户。 请确保将运行设置设置为**Azure**并启动具有参数的 runbook。 Runbook 需要一个运行方式帐户，自动化帐户才能工作。 对于每个参数，请确保按照以下列表和图像中所示，通过开关启动该参数：

* -s \<subscriptionId\>
* -g \<资源组\>
* -a \<automationAccount\>
* -m \<modulePackage\>

![包列表](media/python-packages/import-python-runbook.png)

Runbook 允许你指定要下载的包。 例如，使用`Azure`参数会下载所有 Azure 模块和所有依赖项（大约105）。

完成 runbook 后，你可以在自动化帐户中检查 "**共享资源**" 下的 " **Python 2" 包**，验证包是否已正确导入。

## <a name="use-a-package-in-a-runbook"></a>在 runbook 中使用包

导入包后，可以在 runbook 中使用它。 以下示例使用[Azure 自动化实用工具包](https://github.com/azureautomation/azure_automation_utility)。 使用此包，可以轻松地通过 Azure 自动化使用 Python。 若要使用此包，请按照 GitHub 存储库中的说明进行操作，并将其添加到 runbook。 例如，你可以使用`from azure_automation_utility import get_automation_runas_credential`导入用于检索运行方式帐户的函数。

```python
import azure.mgmt.resource
import automationassets
from azure_automation_utility import get_automation_runas_credential

# Authenticate to Azure using the Azure Automation RunAs service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential()

# Intialize the resource management client with the RunAs credential and subscription
resource_client = azure.mgmt.resource.ResourceManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"]))

# Get list of resource groups and print them out
groups = resource_client.resource_groups.list()
for group in groups:
    print group.name
```

## <a name="develop-and-test-runbooks-offline"></a>脱机开发并测试 runbook

若要脱机开发并测试 Python 2 runbook，可使用 GitHub 上的[Azure Automation python emulated assets](https://github.com/azureautomation/python_emulated_assets)（Azure 自动化 python 模拟资产）模块。 此模块使你能够引用凭据、变量、连接和证书等共享资源。

## <a name="next-steps"></a>后续步骤

若要开始 Python 2 runbook，请参阅[我的第一个 python 2 runbook](automation-first-runbook-textual-python2.md)。
