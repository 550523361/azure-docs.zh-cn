---
title: Azure 机器学习新增功能
description: 本文档详细说明了 Azure 机器学习的更新。
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
author: hning86
ms.author: haining
ms.reviewer: j-martens
ms.date: 10/24/2018
ms.openlocfilehash: 5ceb47f437f736829d0be10a856fe787fab944b0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261559"
---
# <a name="azure-machine-learning-service-release-notes"></a>Azure 机器学习服务发行说明

本文介绍 Azure 机器学习服务版本。 

## <a name="2018-11-05"></a>2018-11-05

### <a name="azure-portal"></a>Azure 门户 
Azure 机器学习服务的 Azure 门户具有以下更新：
  * 针对已发布的管道提供了一个新的“管道”选项卡。
  * 增加了以下支持：将现有 HDInsight 群集附加为计算目标。

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>适用于 Python 的 Azure 机器学习 SDK v0.1.74

+ **重大更改** 
  * Workspace.compute_targets、datastores、experiments、images、models 和 webservices 现在是属性而不是方法。 例如，将 Workspace.compute_targets() 替换为 Workspace.compute_targets。
  * *Run.get_context* 弃用了 *Run.get_submitted_run*。 后续版本中将删除后一种方法。
  * *PipelineData* 类现在需要使用数据存储对象作为参数而非需要 datastore_name。 类似地，*Pipeline* 接受 default_datastore 而非 default_datastore_name。

+ **新功能**
  * Azure 机器学习管道[示例 Notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) 现在使用 MPI 步骤。
  * Jupyter Notebook 的 RunDetails 小组件已更新，将显示管道的可视化效果。

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>Azure 机器学习数据准备 SDK v0.4.0 
 
+ **新功能**
  * 向数据配置文件中添加了类型计数 
  * 现在提供了值计数和直方图
  * 数据配置文件中提供了更多百分点值
  * 汇总中提供了中值
  * 现在支持 Python 3.7
  * 将包含数据存储的数据流保存到数据准备包时，数据存储信息将持久保存为数据准备包的一部分
  * 现在支持写入到数据存储 
        
+ **修复的 Bug**
  * Linux 上现在可以正确处理 64 位无符号整数溢出
  * 修复了 smart_read 中纯文本文件的不正确文本标签
  * 字符串列类型现在显示在指标视图中
  * 类型计数已修复，它现在显示映射到单个而非各个 FieldType 的 ValueKinds
  * 将路径提供为字符串时，Write_to_csv 不再失败
  * 使用“替换”时，将“查找”保留为空将不再失败 

## <a name="2018-10-12"></a>2018-10-12

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>适用于 Python 的 Azure 机器学习 SDK v0.1.68

+ **新功能**
  * 创建新工作区时的多租户支持。

+ **修复的 Bug**
  * 部署 Web 服务时，不再需要固定 pynacl 库版本。

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure 机器学习数据准备 SDK v0.3.0

+ **新功能**
  * 添加了方法 transform_partition_with_file(script_path)，允许用户传入 Python 文件的路径进行执行

## <a name="2018-10-01"></a>2018-10-01

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>适用于 Python 的 Azure 机器学习 SDK v0.1.65
[版本 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) 包括新增功能、更多文档、bug 修复和更多[示例笔记本](https://aka.ms/aml-notebooks)。

请参阅[已知问题列表](resource-known-issues.md)了解已知 bug 和解决方法。

+ **重大更改**
  * Workspace.experiments、Workspace.models、Workspace.compute_targets、Workspace.images，Workspace.web_services 将返回字典，而以前返回列表。 请参阅 [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) API 文档。

  * 自动化机器学习从主要指标中删除了规范化的均方误差。

+ **HyperDrive**
  * 针对 Bayesian 的多种 HyperDrive bug 修复；针对获取指标调用的性能改进。 
  * 从 1.9 升级的 TensorFlow 1.10 
  * 针对冷启动的 Docker 映像优化。 
  * 作业现在报告正确状态，即便退出时显示 0 之外的错误代码。 
  * SDK 中的 RunConfig 属性验证。 
  * HyperDrive 运行对象支持类似于“常规运行：不需要传递任何参数”的取消。 
  * 小组件改进，用于维护分布式运行和 HyperDrive 运行的下拉列表值的状态。 
  * TensorBoard 和针对参数服务器修复的其他日志文件支持。 
  * 服务端上的 Intel(R) MPI 支持。 
  * 对 BatchAI 中验证期间针对分布式运行修复的参数优化的 bug 修复。 
  * 上下文管理器现可标识主要实例。 

+ **Azure 门户体验**
  * 运行详细信息中支持 log_table() 和 log_row()。 
  * 为包含 1、2 或 3 数字列和可选分类列的表格和行自动创建关系图。

+ **自动化机器学习**
  * 改进了错误处理和文档 
  * 修复了运行属性检索性能问题。 
  * 修复了连续运行的问题。 
  * 修复了集成迭代问题。
  * 修复了 MAC 操作系统上的训练挂起 bug。
  * 减少对自定义验证场景中的宏平均 PR/ROC 曲线的采样。
  * 删除额外的索引逻辑。
  * 删除了 get_output API 中的筛选器。

+ **管道**
  * 添加了方法 Pipeline.publish()，用于直接发布管道，而无需首先执行运行。   
  * 添加了方法 PipelineRun.get_pipeline_runs()，用于获取从已发布管道生成的管道运行。

+ **Project Brainwave**
  * 更新了对 FPGA 上提供的新 AI 模型的支持。

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>Azure 机器学习数据准备 SDK v0.2.0
[版本 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) 包括以下功能和 bug 修复：

+ **新功能**
  * 独热编码支持
  * 分位转换支持
   
+ **Bug 修复：**
  * 适用于所有 Tornado 版本，无需降级 Tornado 版本
  * 值对所有值都有意义，而不仅仅是前三个值

## <a name="2018-09-public-preview-refresh"></a>2018-09（公共预览版刷新）

新的完全刷新的 Azure 机器学习版本：了解有关此版本的详细信息： https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/

## <a name="older-notes-sept-2017---jun-2018"></a>旧版说明：2017 年 9 月 - 2018 年 6 月
### <a name="2018-05-sprint-5"></a>2018-05（冲刺 (sprint) 5）

使用此版本的 Azure 机器学习，可以：
+ 使用量化版本 ResNet 50 将映像特征化，基于这些特征训练分类器，并[将该模型部署到 Azure 上的 FPGA](../service/how-to-deploy-fpga-web-service.md)，以便进行超低延迟推断。

+ 使用[自定义 Azure 机器学习包](../desktop-workbench/reference-python-package-overview.md)为以下域快速生成和部署高度准确的机器学习和深度学习模型：
  + [计算机视觉](../desktop-workbench/how-to-build-deploy-image-classification-models.md)
  + [文本分析](../desktop-workbench/how-to-build-deploy-text-classification-models.md)
  + [预测](../desktop-workbench/how-to-build-deploy-forecast-models.md)

### <a name="2018-03-sprint-4"></a>2018-03（冲刺 (sprint) 4）
**版本号**：0.1.1801.24353 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（[查找版本](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number)）


下面的许多更新就是针对你的反馈的直接结果。 请持续关注！

**值得注意的新功能和更改**

- 除了支持基于远程 docker 的执行外，还支持在你自己环境中的远程 Ubuntu VM 上本机运行脚本。
- Workbench 应用中的全新环境体验，除了我们的基于 CLI 的体验外，还允许创建计算目标和运行配置。
![“环境”选项卡](media/azure-machine-learning-release-notes/environment-page.png)
- 可自定义运行历史记录报告 ![新运行历史记录报告的图像](media/azure-machine-learning-release-notes/new-run-history-reports.png)

**详细的更新**

下面列出了此冲刺 (sprint) 中 Azure 机器学习的每个组件区域中的详细更新。

#### <a name="workbench-ui"></a>Workbench UI
- 可自定义运行历史记录报告
  - 运行历史记录报告的改进图表配置
    - 可以更改使用的入口点
    - 可以添加和修改顶级筛选器 ![添加筛选器](media/azure-machine-learning-release-notes/add-filters.jpg)
    - 可以添加或修改（以及拖放和重新排列）图表和统计信息。
    ![创建新图表](media/azure-machine-learning-release-notes/configure-charts.png)

  - 运行历史记录报告的 CRUD
  - 将所有现有运行历史记录列表视图配置移动到了服务器端报告，其行为与从所选入口点运行的管道类似。

- “环境”选项卡
  - 轻松将新计算目标和运行配置文件添加到项目 ![新建计算目标](media/azure-machine-learning-release-notes/add-new-environments.png)
  - 使用简单的基于窗体的 UX 管理和更新配置文件
  - 用于准备执行环境的新按钮

- 对侧栏中文件列表的性能改进

#### <a name="data-preparation"></a>数据准备工作 
- Azure Machine Learning Workbench 现在允许使用已知列的名称搜索列。


#### <a name="experimentation"></a>试验
- Azure Machine Learning Workbench 现在支持在你自己的 python 或 pyspark 环境中本机运行脚本。 为实现此功能，用户在远程 VM 上创建和管理自己的环境并使用 Azure Machine Learning Workbench 对该目标运行其脚本。 请参阅[配置 Azure 机器学习试验服务](../desktop-workbench/experimentation-service-configuration.md) 

#### <a name="model-management"></a>模型管理
- 支持自定义已部署的容器：通过允许使用 apt-get 等安装外部库来支持自定义容器映像。它不再限制为 pip 可安装库。 有关详细信息，请参阅[文档](../desktop-workbench/model-management-custom-container.md)。
  - 请将 `--docker-file myDockerStepsFilename` 标志和文件名用于清单、映像或服务创建命令。
  - 请注意，基础映像是 Ubuntu，不能修改。
  - 示例命令： 
  
      ```shell
      $ az ml image create -n myimage -m mymodel.pkl -f score.py --docker-file mydockerstepsfile
      ```



### <a name="2018-01-sprint-3"></a>2018-01（冲刺 (sprint) 3） 
版本号：0.1.1712.18263 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（[查找版本](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number)）

下面是此冲刺 (sprint) 中的更新和改进。 其中许多更新是用户反馈的直接结果。 


下面列出了此冲刺 (sprint) 中 Azure 机器学习的每个组件区域中的详细更新。

- 对身份验证堆栈的更新强制在启动时进行登录和选择帐户

#### <a name="workbench"></a>工作台
- 能够通过“添加/删除程序”安装/卸载应用
- 对身份验证堆栈的更新强制在启动时进行登录和选择帐户
- Windows 上改进的单一登录 (SSO) 体验
- 使用不同凭据属于多个租户的用户现在将能够登录到 Workbench

#### <a name="ui"></a>UI
- 常规改进和 bug 修复

#### <a name="notebooks"></a>笔记本
- 常规改进和 bug 修复

#### <a name="data-preparation"></a>数据准备工作 
- 改进了执行按示例转换时的自动建议
- 改进了模式频率检查器的算法
- 能够在执行按示例转换时发送示例数据和反馈 ![执行派生列转换时发送反馈链接的图像](media/azure-machine-learning-release-notes/SendFeedbackFromDeriveColumn.png)
- Spark 运行时改进
- Scala 已取代 Pyspark
- 修复了无法关闭不适用于时序检查器的数据 
- 修复了 HDI 数据准备执行的挂起时间

#### <a name="model-management-cli-updates"></a>模型管理 CLI 更新 
  - 预配资源不再需要订阅所有权。 对资源组的参与者访问权限将足以设置部署环境。
  - 为免费订阅启用了本地环境设置 

### <a name="2017-12-sprint-2-qfe"></a>2017-12（冲刺 (sprint) 2 QFE） 
版本号：0.1.1711.15323 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（[查找版本](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number)）

这是 QFE（快速修补工程）版本，为次版本。 它解决了几个遥测问题，可帮助产品团队更好地了解产品使用情况。 将来可利用这些信息改善产品体验。 

此外，还有两个重要更新：

- 修复了数据准备中的一个 bug，该 bug 导致时序检查器无法在数据准备包中显示。
- 在命令行工具中，再也不需要以 Azure 订阅所有者的身份来预配机器学习计算 ACS 群集。 

### <a name="2017-12-sprint-2"></a>2017-12（冲刺 (Sprint) 2）
版本号：0.1.1711.15263 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（[查找版本](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number)）

欢迎使用 Azure 机器学习的第三个更新。 此更新包括 Workbench 应用、命令行接口 (CLI) 以及后端服务中的改进。 非常感谢你向我们发送微笑和哭脸。 下面的许多更新就是针对你的反馈的直接结果。 

**值得注意的新功能**
- [支持 SQL Server 和 Azure SQL 数据库用作数据源](../desktop-workbench/data-prep-appendix2-supported-data-sources.md#types) 
- [使用 MMLSpark 深入学习具备 GPU 支持的 Spark](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [所有已部署的 AML 容器都与 Azure IoT Edge 设备兼容（无需额外步骤）](https://aka.ms/aml-iot-edge-blog)
- Azure 门户中提供了已注册的模型列表和详细信息视图
- 除了基于用户名/密码的身份验证，还使用基于 SSH 密钥的身份验证访问计算目标。 
- 引入新的模式频率检查器提供数据准备体验。 

**详细更新**下面列出了此冲刺 (sprint) 中 Azure 机器学习的每个组件区域中的详细更新。

#### <a name="installer"></a>安装程序
- 安装程序可以自动更新，用户无需重新安装就可以修复 Bug 并获取新功能。

#### <a name="workbench-authentication"></a>Workbench 身份验证
- 修复了身份验证系统的多个问题。 如果仍然遇到登录问题，请通知我们。
- 更改了 UI，可更轻松地找到代理管理器设置。

#### <a name="workbench"></a>工作台
- 只读文件视图现在具有浅蓝色背景
- 将“编辑”按钮移到了右侧，更易于发现。
- “dsource”、“dprep”和“ipynb”文件格式现在可以原始文本格式呈现
- Workbench 现在拥有新的编辑体验，可引导用户使用外部 IDE 来编辑脚本，并可直接使用 Workbench 来编辑具有丰富编辑体验的文件类型（如 Notebook、数据源、数据准备包）
- 现在可以更快地加载用户有权访问的工作区和项目列表

#### <a name="data-preparation"></a>数据准备工作 
- 可使用模式频率检查器在列中查看字符串模式。 还可以使用这些模式来筛选数据。 这显示了一个类似值计数检查器的视图。 不同之处在于，模式频率显示的是数据唯一模式的计数，而不是唯一数据的计数。 还可以筛选出或筛选掉符合某种模式的所有行。

![“产品编号”上模式频率检查器的图像](media/azure-machine-learning-release-notes/pattern-inspector-product-number.png)

- 改进了性能，同时建议在“按示例派生列”转换中查看边缘事例

- [支持 SQL Server 和 Azure SQL 数据库用作数据源](../desktop-workbench/data-prep-appendix2-supported-data-sources.md#types) 

![新建 SQL Server 数据源的图像](media/azure-machine-learning-release-notes/sql-server-data-source.png)

- 启用了行和列计数的“概览”视图

![概览模式下行列计数的图像](media/azure-machine-learning-release-notes/row-col-count.png)

- 在所有计算上下文中启用了数据准备
- 在所有计算上下文中启用了使用 SQL Server 数据库的数据源
- 可以按数据类型筛选数据准备网格列
- 修复了将多个列转换为日期时存在的问题
- 修复了在高级模式下更改输出列名称后用户可选择输出列作为“按示例派生列”中的源的问题。

#### <a name="job-execution"></a>作业执行
现在可以按照以下步骤使用基于 SSH 密钥的身份验证创建和访问 remotedocker 或群集类型的计算目标：
- 在 CLI 中使用以下命令附加计算目标

    ```azure-cli
    $ az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
    ```
>[!NOTE]
>在命令中指定 -k（或 --use-azureml-ssh-key）选项以生成并使用 SSH 密钥。

- Azure ML Workbench 将生成一个公钥，并将其输出至控制台。 使用相同的用户名登录到计算目标，然后将此公钥追加至 ~/.ssh/authorized_keys 文件。

- 可以准备此计算目标并将其用于执行，Azure ML Workbench 会使用此密钥来进行身份验证。  

有关创建计算目标的详细信息，请参阅 [配置 Azure 机器学习试验服务](../desktop-workbench/experimentation-service-configuration.md)

#### <a name="visual-studio-tools-for-ai"></a>用于 AI 的 Visual Studio Tools
- 增加了对 [Visual Studio Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vstoolsai-vs2017)的支持。 

#### <a name="command-line-interface-cli"></a>命令行接口 (CLI)
- 增加了 `az ml datasource create` 命令，以允许从命令行创建数据源文件

#### <a name="model-management-and-operationalization"></a>模型管理和操作化
- [所有已操作化的 AML 容器都与 Azure IoT Edge 设备兼容（无需额外步骤）](https://aka.ms/aml-iot-edge-blog) 
- 改进了 o16n CLI 中的错误消息
- 修复了模型管理门户 UX 中的 Bug  
- 详细信息页中模型管理属性的字母保持大小写一致
- 实时评分调用超时设置为 60 秒
- Azure 门户中提供了已注册的模型列表和详细信息视图

![门户中提供了模型详细信息](media/azure-machine-learning-release-notes/model-list.jpg)

![门户中提供了模型概述](media/azure-machine-learning-release-notes/model-overview-portal.jpg)

#### <a name="mmlspark"></a>MMLSpark
- 深入学习具备 [GPU 支持](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)的 Spark
- 支持资源管理器模板，可轻松部署资源
- 支持 SparklyR 生态系统
- [AZTK 集成](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

#### <a name="sample-projects"></a>示例项目
- 使用新的 Azure ML SDK 版本更新了 [Iris](https://github.com/Azure/MachineLearningSamples-Iris) 和 [MMLSpark](https://github.com/Azure/mmlspark) 示例

#### <a name="breaking-changes"></a>重大变化
- 将 `az ml computetarget attach` 中的 `--type` 开关提升到了子命令。 

    - `az ml computetarget attach --type remotedocker` 现为 `az ml computetarget attach remotedocker`
    - `az ml computetarget attach --type cluster` 现为 `az ml computetarget attach cluster`

### <a name="2017-11-sprint-1"></a>2017-11（冲刺 (sprint) 1） 
版本号：0.1.1710.31013 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（[查找版本](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number)）

在此版本中，我们改进了 Workbench 应用、CLI 及后端服务层中的安全性、稳定性及可维护性。 非常感谢向我们发送微笑和哭脸。 下面的许多更新就是你反馈的直接结果。 请持续关注！

#### <a name="notable-new-features"></a>值得注意的新功能
- 现在有两个新的 Azure 区域可提供 Azure ML：西欧和东南亚。 这两个区域加上以前的区域：“美国东部 2”、“美国中西部”和“澳大利亚东部”，使得部署区域的总数达到 5 个。
- 我们在 Workbench 应用中启用了 Python 代码语法突出显示功能，便于读取和编辑 Python 源代码。 
- 现在，你可以直接从文件中启动你喜欢的 IDE，而不是从整个项目中启动。  在 Workbench 中打开一个文件，然后单击“编辑”，即可启动当前文件和项目的 IDE（当前支持 VS Code 和 PyCharm）。  此外，也可以单击“编辑”按钮旁边的箭头，在 Workbench 文本编辑器中编辑文件。  文件在你单击“编辑”之前是只读的，用以防止意外更改。
- 常用的绘图库 `matplotlib` 版本 2.1.0 现在随 Workbench 应用一起提供。
- 我们已将适用于数据准备引擎的 .NET Core 版本升级到 2.0。 这不仅可以省去在 macOS 上安装应用期间使用 brew-install openssl 要求， 而且也为将在不久之后引入更多令人兴奋的数据准备功能铺平了道路。 
- 我们已启用特定于版本的应用主页，以便获取更具相关性的发行说明，以及基于当前应用版本的更新提示。
- 如果你的本地用户名包含空格，则应用程序现在可以成功安装。 

#### <a name="detailed-updates"></a>详细的更新
下面列出了此冲刺 (sprint) 中 Azure 机器学习的每个组件区域中的详细更新。

##### <a name="installer"></a>安装程序
- 应用安装程序现在将清理由旧版应用创建的安装目录。
- 修复了导致安装程序在 macOS High Sierra 上陷入 100% 停滞的 bug。
- 现在有了到安装程序目录的直接链接，用户可在安装失败的情况下查看安装程序日志。
- 安装现在适用于用户名中有空格的用户。

##### <a name="workbench-authentication"></a>Workbench 身份验证
- 支持在代理管理器中进行身份验证。
- 如果用户位于防火墙后面，现在则可成功登录。 
- 如果用户在多个 Azure 区域具有试验帐户并且有一个区域碰巧不可用，则应用将不再挂起。
- 未完成身份验证且仍然可以看到身份验证对话框时，应用将不再尝试从本地缓存中加载工作区。

##### <a name="workbench-app"></a>工作台应用
- 在文本编辑器中启用了 Python 代码语法突出显示。
- 通过文本编辑器中的“编辑”按钮，你可以在 IDE（支持 VS Code 和 PyCharm）或内置的文本编辑器中编辑该文件。
- 文本编辑器默认处于只读模式。 
- 在当前文件保存后，保存按钮可视状态现在更改到已禁用，因此不再包含脏数据。
- 启动运行时，Workbench 将保存“所有”未保存的文件。
- Workbench 会记住上次在本地计算机上使用的工作区以便它自动打开。
- 现在仅允许运行 Workbench 的单个实例。 以前可以启动多个实例，但在同一个项目中运行时这样做会导致问题。
- “重命名的文件”菜单“打开项目...”到“添加现有文件夹作为项目...” 
- 现在选项卡切换速度更快。
- 帮助链接已添加到“配置 IDE”对话框中。
- 反馈表单现在会记住你上次输入的电子邮件地址。
- 微笑和哭脸表单文本区域现在更大，因此，你可以向我们发送更多反馈！ 
- `az ml workspace create` 中的 `--owner` 切换可帮助文本得到更正。
- 我们添加了一个“关于”对话框，以帮助用户轻松查看和复制应用的版本号。
- “建议一项功能”菜单项被添加到“帮助”菜单。
- 实验帐户名称现在显示在应用标题栏中，前面是应用名称“Azure 机器学习 Workbench”。
- 特定于版本的应用主页现在基于检测到的应用版本显示。

##### <a name="data-preparation"></a>数据准备工作 
- 无法再从地图检查器加载外部网站，以防止潜在的安全问题。
- 直方图和值计数检查器现在提供了用对数刻度显示图形的选项。
- 当计算处于进行中时，数据质量栏现在可显示不同的颜色，以表示“正在计算”的状态。
- 列度量值现在显示分类值列的统计信息。
- 数据源名称中的最后一个字符不再被截断。
- 切换选项卡时数据准备包现在保持打开状态，从而显著提升性能。
- 在数据源中，在数据视图和指标视图之间切换时，列的顺序不再更改。
- 打开无效的 `.dprep` 或 `.dsource` 文件不再导致 Workbench 崩溃。
- 数据准备包现在可以为“写入到 CSV”转换中的输出使用相对路径。
- “保留列”转换现在允许用户在编辑时添加其他列。
- “替换此内容”菜单中现在实际上启动了“替换值”对话框。
- “替换值”转换现在按预期方式运行，而不再引发错误。
- 现在，数据准备包在引用项目文件夹之外的数据文件时使用绝对路径，这样可以在本地上下文中以数据文件的绝对路径运行准备包。
- 将 Azure blob 用作数据源时，现在支持将“完整文件”作为采样策略。
- （从数据准备包）生成的 Python 代码现在会带来 CR 和 LF，使其在 Windows 中变得友好。
- 切换到“数据”视图时，“选择度量值”下拉列表现在可隐藏属性。
- Workbench 现在可以处理 parquet 文件，即使它使用 Python 运行时也是如此。 以前处理 parquet 文件时仅可以使用 Spark。 
- 筛选列中数据类型为 date 的值时不再导致数据准备引擎崩溃。
- 度量值视图现在会遵守采样策略更新。
- 远程采样作业现在工作正常。

##### <a name="job-execution"></a>作业执行
- 运行历史记录中现在包括自变量。
- CLI 中启动的作业现在自动显示在“运行历史记录”作业面板中。
- 作业面板现在显示由来宾用户创建并添加到 Azure AD 租户的作业。
- 取消和删除作业面板操作会更为稳定。
- 单击“运行”按钮时，如果配置文件的格式错误，现在会触发错误消息。
- 终止应用程序不再干扰 CLI 中启动的作业。
- 即使在执行一小时之后，CLI 中启动的作业也将继续筛选标准。
- 数据准备包在 Python/PySpark 中运行失败时，则会显示更详细的错误消息。
- `az ml experiment clean` 现在也清理远程 VM 中的 Docker 映像。
- 现在，`az ml experiment clean` 在 macOS 上可正常用于本地目标。
- 定位本地或远程 Docker 运行时的错误消息已被清除，并且易于阅读。
- 在附加为一个执行目标时，如果 HDInsight 群集头节点名称格式不正确，则将显示更详细的错误消息。
- 在凭据服务中找不到密钥时，会显示更详细的错误消息。 
- MMLSpark 库已升级，现在可以支持 Apache Spark 2.2。
- MMLSpark 现在包含医疗文档的主题编码转换（网格编码）。
- `matplotlib` 版本 2.1.0 现已与 Workbench 一起提供。

##### <a name="jupyter-notebook"></a>Jupyter 笔记本
- Notebook 名称搜索现在可在 Notebook 视图中正常使用。
- 现在可以在 Notebook 视图中删除 Notebook。
- 已添加新的 Magic `%upload_artifact`，用于将在 Notebook 执行环境中生成的文件上传到运行历史记录数据存储中。
- 内核错误现在显示在 Notebook 作业状态中，更加易于调试。
- 用户退出应用时，Jupyter 服务器现在可正确关闭。

##### <a name="azure-portal"></a>Azure 门户
- 现在可以在两个新的 Azure 区域创建试验帐户和模型管理帐户：西欧和东南亚。
- 模型管理帐户 DevTest 计划现在只有在订阅中第一个创建时才可用。 
- Azure 门户中的帮助链接已更新以指向正确的文档页。
- 由于不适用，描述字段已从 Docker 映像详细信息页中删除。
- 包括 AppInsights 和自动缩放设置在内的详细信息已添加到 Web 服务详细信息页。
- 即使在浏览器中禁用了第三方 cookie，现在也可以呈现模型管理页面。 

##### <a name="operationalization"></a>实施
- 在其名称中有“score”的 Web 服务不再失败。
- 用户现在只需对 Azure 资源组或订阅具有参与者访问权限，即可创建部署环境。 不再需要拥有整个订阅的所有者访问权限。
- 操作化 CLI 现已可在 Linux 中使用选项卡自动完成功能。
- 映像构造服务现在支持为 Azure IoT 服务/设备构建映像。

##### <a name="sample-projects"></a>示例项目
- [_Classifying Iris_](../desktop-workbench/tutorial-classifying-iris-part-1.md) 示例项目：
    - `iris_pyspark.py` 已重命名为 `iris_spark.py`。
    - `iris_score.py` 已重命名为 `score_iris.py`。
    - `iris.dprep` 和 `iris.dsource` 已更新以反映最新的数据准备引擎更新。
    - `iris.ipynb` Notebook 已经过修改，可在 HDInsight 群集中工作。
    - 运行历史记录已在 `iris.ipynb` Notebook 单元格中打开。
- [_使用自行车共享数据的高级数据准备_](../desktop-workbench/tutorial-bikeshare-dataprep.md) 示例项目的 “处理错误值” 步骤已修复。
- [](https://github.com/Azure/MachineLearningSamples-mmlspark)“针对成人人口普查数据的 MMLSpark”示例项目 `docker.runconfig` 的格式已从 JSON 更新到 YAML。
- [](../desktop-workbench/scenario-distributed-tuning-of-hyperparameters.md)“分布式超参数优化”示例项目 `docker.runconfig` 的格式已从 JSON 更新到 YAML。
- 新建示例项目“使用 CNTK 的图像分类”[](../desktop-workbench/scenario-image-classification-using-cntk.md)。


### <a name="2017-10-sprint-0"></a>2017-10（冲刺 (sprint) 0） 
版本号：0.1.1710.31013 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（[查找版本](../desktop-workbench/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number)）

欢迎使用自 Microsoft Ignite 2017 大会发布 Azure Machine Learning Workbench 初始公共预览版以来的首个更新。 此发行版中的主要更新是可靠性和稳定性修复。  我们已解决的一些严重问题包括：

#### <a name="new-features"></a>新增功能
- 现在已支持 macOS High Sierra

#### <a name="bug-fixes"></a>Bug 修复
##### <a name="workbench-experience"></a>Workbench 体验
- 将文件拖放到 Workbench 中导致 Workbench 崩溃。
- 作为 Workbench 的 IDE 配置的 VS Code 中的终端窗口不能识别 _az ml_ 命令。

##### <a name="workbench-authentication"></a>Workbench 身份验证
我们已做了大量更新来改进所报告的各种登录和身份验证问题。
- 身份验证窗口一直保持弹出状态，尤其是当 Internet 连接不稳定时。
- 改进了与身份验证令牌过期相关的可靠性问题。
- 在某些情况下，身份验证窗口出现两次。
- 当身份验证过程已完成并且弹出对话框已关闭时，Workbench 主窗口仍然显示“正在进行身份验证”消息。
- 如果没有 Internet 连接，则身份验证对话框会弹出一个空白屏幕。

##### <a name="data-preparation"></a>数据准备工作 
- 筛选特定值时，错误和缺失值也将筛选出来。
- 更改采样策略会删除后续的现有联接操作。
- 替换缺失值转换不考虑 NaN。
- 遇到 null 值时，日期类型推理引发异常。

##### <a name="job-execution"></a>作业执行
- 当作业执行因为项目文件夹超出了大小限制而无法将其上传时，没有显示明确的错误消息。
- 如果用户的 Python 脚本更改了工作目录，则不会跟踪写入到输出文件夹中的文件。 
- 如果处于活动状态的 Azure 订阅不同于当前项目所属的订阅，则作业提交会导致 403 错误。
- 不存在 Docker 时，如果用户尝试使用 Docker 作为执行目标，则不会返回明确的错误消息。
- 当用户单击“运行”按钮时，.runconfig 文件不会自动保存。

##### <a name="jupyter-notebook"></a>Jupyter 笔记本
- 当用户使用某些登录类型时，Notebook 服务器无法启动。
- Notebook 服务器错误消息未出现在对用户可见的日志中。

##### <a name="azure-portal"></a>Azure 门户
- 选择 Azure 门户的深色主题会导致“模型管理”边栏选项卡显示为一个黑色框。

##### <a name="operationalization"></a>实施
- 重复使用某个清单更新 Web 服务会导致使用随机名称构建新的 Docker 映像。
- 无法从 Kubernetes 群集检索 Web 服务日志。
- 当用户尝试创建模型管理帐户或 ML 计算帐户并遇到权限问题时，会输出误导性错误消息。

## <a name="next-steps"></a>后续步骤

阅读 [Azure 机器学习](../service/overview-what-is-azure-ml.md)的概述。
