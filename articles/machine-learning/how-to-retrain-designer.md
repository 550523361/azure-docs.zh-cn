---
title: 使用 Azure 机器学习设计器重新训练模型（预览版）
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习设计器（预览版）中发布的管道重新训练模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: keli19
author: likebupt
ms.date: 02/24/2020
ms.openlocfilehash: 264b169eefde18880f50feae2554aa3ca7037b1f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2020
ms.locfileid: "79368156"
---
# <a name="retrain-models-with-azure-machine-learning-designer-preview"></a>使用 Azure 机器学习设计器重新训练模型（预览版）
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

在本操作操作者文章中，您将了解如何使用 Azure 机器学习设计器重新训练机器学习模型。 了解如何使用发布的管道自动执行机器学习工作流以进行重新训练。

在本文中，学习如何：

> [!div class="checklist"]
> * 训练机器学习模型。
> * 创建管道参数。
> * 发布训练管道。
> * 重新训练模型。

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果没有 Azure 订阅，请创建[一个免费帐户](https://aka.ms/AMLFree)。
* 具有企业版 SKU 的 Azure 机器学习工作区。

本文假定您具备在设计器中构建管道的基本知识。 有关设计器的引导式简介，请完成[教程](tutorial-designer-automobile-price-train-score.md)。 

### <a name="sample-pipeline"></a>示例管道

本文中使用的管道是[示例 3：收入预测](how-to-designer-sample-classification-predict-income.md)中找到的管道的更改版本。 它使用[导入数据](algorithm-module-reference/import-data.md)模块而不是示例数据集来演示如何使用自己的数据训练模型。

![屏幕截图，显示修改的示例管道，并带有突出显示导入数据模块的框](./media/how-to-retrain-designer/modified-sample-pipeline.png)

## <a name="train-a-machine-learning-model"></a>训练机器学习模型

若要重新训练模型，需要一个初始模型。 在本节中，您将了解如何使用设计器训练模型并访问保存的模型。

1. 选择“导入数据”**** 模块。
1. 在属性窗格中，指定数据源。

   ![显示导入数据模块的示例配置的屏幕截图](./media/how-to-retrain-designer/import-data-settings.png)

   在此示例中，数据存储在 [Azure 数据存储](how-to-access-data.md)中。 如果还没有数据存储，可以通过选择“新建数据存储”**** 来创建一个数据存储。

1. 指定数据路径。 您还可以选择 **"浏览路径"** 以浏览到数据存储。 
1. 选择"在画布顶部**运行**"。
    
   > [!NOTE]
   > 如果已为此管道草稿设置了默认计算，管道会自动运行。 否则，您可以按照设置窗格上的提示立即设置设置。

### <a name="find-your-trained-model"></a>查找您训练的模型

设计器会将所有管道输出（包括已训练的模型）保存到默认存储帐户中。 还可以直接在设计器中访问已训练的模型：

1. 等待管道完成运行。
1. 选择**训练模型**模块。
1. 在"设置"窗格中，选择 **"输出\日志**"。
1. 选择 **"查看输出**"图标，然后按照弹出窗口中的说明查找已训练的模型。

![显示如何下载训练的模型的屏幕截图](./media/how-to-retrain-designer/trained-model-view-output.png)

## <a name="create-a-pipeline-parameter"></a>创建管道参数

将管道参数添加到运行时动态设置变量。 对于此管道，为训练数据路径添加参数，以便您可以在新数据集上重新训练模型。

1. 选择“导入数据”**** 模块。
1. 在“设置”窗格中，选择“路径”**** 字段上方的省略号。
1. 选择“添加到管道参数”****。
1. 提供参数名称和默认值。

   > [!NOTE]
   > 您可以通过选择管道拔模标题旁边的 **"设置**齿轮"图标来检查和编辑管道参数。 

![演示如何创建管道参数的屏幕截图](media/how-to-retrain-designer/add-pipeline-parameter.png)

## <a name="publish-a-training-pipeline"></a>发布训练管道

发布管道后，系统会创建管道终结点。 通过管道终结点，可重复使用和管理管道，实现可重复性和自动化。 在此示例中，您已设置用于重新训练的管道。

1. 选择设计器画布上方的“发布”****。
1. 选择或创建管道终结点。

   > [!NOTE]
   > 可将多个管道发布到一个终结点。 终结点中的每个管道都会得到一个版本号，可以在调用管道终结点时指定该版本号。

1. 选择“发布”。

## <a name="retrain-your-model"></a>重新训练模型

现在，您已经发布了培训管道，您可以使用它使用新数据重新训练模型。 可以从 Azure 门户提交从管道终结点提交的运行，也可以以编程方式提交。

### <a name="submit-runs-by-using-the-designer"></a>使用设计器提交运行

通过以下步骤从设计器提交管道终结点运行：

1. 转到“终结点”**** 页。
1. 选择“管道终结点”**** 选项卡。
1. 选择管道终结点。
1. 选择“已发布的管道”**** 选项卡。
1. 选择要运行的管道。
1. 选择“提交”****。
1. 在设置对话框中，可以为输入数据路径值指定新值。 此值指向新数据集。

![演示如何设置在设计器中运行的参数化管道的屏幕截图](./media/how-to-retrain-designer/published-pipeline-run.png)

### <a name="submit-runs-by-using-code"></a>使用代码提交运行

您可以在概述面板中找到已发布管道的 REST 终结点。 通过调用终结点，可以重新训练已发布的管道。

要进行 REST 调用，您需要一个 OAuth 2.0 承载式身份验证标头。 有关将身份验证设置为工作区和进行参数化 REST 调用的信息，请参阅构建[Azure 机器学习管道以进行批处理评分](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint)。

## <a name="next-steps"></a>后续步骤

按照[设计器教程](tutorial-designer-automobile-price-train-score.md)来训练和部署回归模型。
