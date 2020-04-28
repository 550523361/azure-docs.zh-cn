---
title: 快速入门：使用适用于 Java 的自定义视觉 SDK 创建对象检测项目
titleSuffix: Azure Cognitive Services
description: 使用 Java SDK 创建项目、添加标记、上传图像、训练项目以及检测对象。
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: areddish
ms.openlocfilehash: 086e67d1058443a6f4afd615f2d2725aaf19a259
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2020
ms.locfileid: "81403825"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-sdk-for-java"></a>快速入门：使用适用于 Java 的自定义视觉 SDK 创建对象检测项目

本文介绍如何开始通过 Java 使用自定义视觉 SDK 来构建对象检测模型。 创建该项目后，可以添加标记的区域、上传图像、训练项目、获取项目的默认预测终结点 URL 并使用终结点以编程方式测试图像。 使用此示例作为构建自己的 Java 应用程序的模板。

## <a name="prerequisites"></a>先决条件

- 所选 Java IDE
- 已安装 [JDK 7 或 8](https://aka.ms/azure-jdks)。
- 已安装 [Maven](https://maven.apache.org/)
- [!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>获取自定义视觉 SDK 和示例代码

若要编写使用自定义视觉的 Java 应用，需要自定义视觉 maven 包。 这些包将包含在要下载的示例项目中，但你可以在此处逐个访问它们。

可在 maven 中央存储库中找到自定义视觉 SDK：
- [定型 SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [预测 SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

克隆或下载[认知服务 Java SDK 示例](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master)项目。 导航到 **Vision/CustomVision/** 文件夹。

此 Java 项目创建新的名为“示例 Java OD 项目”  的自定义视觉对象检测项目，该项目可以通过[自定义视觉网站](https://customvision.ai/)进行访问。 然后，它上传图像以定型和测试分类器。 在此项目中，分类器用于确定对象是**叉子**还是**剪刀**。

[!INCLUDE [get-keys](includes/get-keys.md)]

程序已经过配置，可以将密钥数据作为环境变量引用。 导航到 **Vision/CustomVision** 文件夹，然后输入以下 PowerShell 命令来设置环境变量。 

> [!NOTE]
> 如果使用的是非 Windows 操作系统，请参阅[配置环境变量](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows#configure-an-environment-variable-for-authentication)，了解相关说明。

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>了解代码

在 Java IDE 中加载 `Vision/CustomVision` 项目，打开 _CustomVisionSamples.java_ 文件。 找到 **runSample** 方法，注释掉 **ImageClassification_Sample** 方法调用&mdash;此方法会执行图像分类方案（本指南中未介绍）。 **ObjectDetection_Sample** 方法实现本快速入门的主要功能；请导航到其定义并检查代码。 

### <a name="create-a-new-custom-vision-service-project"></a>创建新的自定义视觉服务项目

转到创建训练客户端和对象检测项目的代码块。 创建的项目将显示在以前访问过的[自定义视觉网站](https://customvision.ai/)上。 请查看 [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_) 方法重载，以在创建项目时指定其他选项（在[生成检测器](get-started-build-detector.md) Web 门户指南中进行了说明）。

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_create_od)]

### <a name="add-tags-to-your-project"></a>将标记添加到你的项目

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_tags_od)]

### <a name="upload-and-tag-images"></a>上传和标记图像

在对象检测项目中标记图像时，需要使用标准化坐标指定每个标记对象的区域。 转到 `regionMap` 映射的定义。 此代码将每个示例图像与其标记的区域相关联。

> [!NOTE]
> 如果没有用于标记区域坐标的单击并拖动实用工具，则可以使用 [Customvision.ai](https://www.customvision.ai/) 的 Web UI。 在此示例中，已提供坐标。

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_od_mapping)]

然后，跳到可将图像添加到项目的代码块。 图像从项目的 **src/main/resources** 文件夹读取，然后与其相应的标记和区域坐标一起上传到服务。

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_upload_od)]

上一代码片段使用两个帮助程序函数，以资源流的形式检索图像并将其上传到服务（最多可以在单个批次中上传 64 个图像）。

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_helpers)]

### <a name="train-the-project-and-publish"></a>训练项目和发布

此代码创建预测模型的第一个迭代，然后将该迭代发布到预测终结点。 为发布的迭代起的名称可用于发送预测请求。 在发布迭代之前，迭代在预测终结点中不可用。

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_train_od)]

### <a name="use-the-prediction-endpoint"></a>使用预测终结点

此处的 `predictor` 对象表示的预测终结点是一个引用，用于将图像提交到当前模型并获取分类预测。 在此示例中，`predictor` 在别处使用预测密钥环境变量来定义。

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_prediction_od)]

## <a name="run-the-application"></a>运行应用程序

若要使用 maven 编译并运行解决方案，请在命令提示符下导航到项目目录 (**Vision/CustomVision**)，并执行运行命令：

```bash
mvn compile exec:java
```

查看控制台输出中的日志记录和预测结果。 然后，可以验证测试图像是否已正确标记，并验证检测区域是否正确。

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>后续步骤

现在你已了解如何在代码中完成对象检测过程的每一步。 此示例执行单次训练迭代，但通常需要多次训练和测试模型，以使其更准确。 以下训练指南涉及图像分类，但其原理与对象检测类似。

> [!div class="nextstepaction"]
> [测试和重新训练模型](test-your-model.md)
